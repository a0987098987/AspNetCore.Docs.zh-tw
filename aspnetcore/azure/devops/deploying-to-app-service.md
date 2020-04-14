---
title: 套用應用程式到應用程式服務 - 使用 ASP.NET 核心和 Azure 進行 DevOps
author: CamSoper
description: 將ASP.NET核心應用部署到Azure應用服務,這是使用ASP.NET核心和Azure的DevOps的第一步。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: d7ee3e42d320d35c2aaff6e097203c45289ec5b1
ms.sourcegitcommit: fbdb8b9ab5a52656384b117ff6e7c92ae070813c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81228123"
---
# <a name="deploy-an-app-to-app-service"></a>套用應用到應用服務

[Azure 應用服務](/azure/app-service/)是 Azure 的 Web 託管平臺。 將 Web 應用部署到 Azure 應用服務可以手動完成,也可以由自動化過程完成。 本指南的這一部分討論了可以使用命令行手動或腳本觸發的部署方法,或者使用 Visual Studio 手動觸發。

在本節中,您將完成以下任務:

* 下載並生成示例應用。
* 使用 Azure 雲外殼創建 Azure 應用服務 Web 應用。
* 使用 Git 將示例應用部署到 Azure。
* 使用 Visual Studio 將更改部署到應用。
* 向 Web 應用添加暫存槽。
* 將更新部署到暫存槽。
* 交換預備與生產位置。

## <a name="download-and-test-the-app"></a>下載並測試應用程式

這個指南中使用的應用程式是預先建構 ASP.NET 核心應用程式,[簡單的飼料閱讀器](https://github.com/Azure-Samples/simple-feed-reader/)。 它是 Razor Pages 應用`Microsoft.SyndicationFeed.ReaderWriter`, 它使用 API 檢索 RSS/Atom 源並在清單中顯示新聞專案。

隨意查看代碼,但請務必瞭解此應用程式沒有什麼特別之處。 它只是一個簡單的ASP.NET核心應用程式,用於說明目的。

從命令外殼下載代碼、生成專案並按如下方式運行它。

> *注意:Linux/macOS 用戶應該對路徑進行適當的更改,例如,使用正向斜杠`/`() 而不是`\`反斜杠 ()。*

1. 將代碼複製到本地電腦上的資料夾。

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. 將工作資料夾更改為已創建*的簡單源閱讀器*資料夾。

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. 還原包,並生成解決方案。

    ```dotnetcli
    dotnet build
    ```

4. 執行應用程式。

    ```dotnetcli
    dotnet run
    ```

    ![點網執行命令成功](./media/deploying-to-app-service/dotnet-run.png)

5. 開啟瀏覽器並瀏覽至 `http://localhost:5000` 。 該應用程式允許您鍵入或貼上聯合源網址並查看新聞專案清單。

     ![顯示 RSS 原始內容的應用程式](./media/deploying-to-app-service/app-in-browser.png)

6. 一旦您滿意應用程式工作正常,通過按命令 shell 中的**Ctrl**+**C**將其關閉。

## <a name="create-the-azure-app-service-web-app"></a>建立 Azure 應用服務 Web 應用

要部署應用程式,您需要建立套用服務[Web 應用](/azure/app-service/app-service-web-overview)。 建立 Web 應用程式後,您將使用 Git 從本地電腦部署到它。

1. 登入 [Azure Cloud Shell](https://shell.azure.com/bash)。 注意:首次登錄時,雲外殼會提示為配置檔創建存儲帳戶。 接受預設值或提供唯一名稱。

2. 在以下步驟中,請使用雲外殼。

    a. 聲明一個變數來存儲 Web 應用的名稱。 名稱必須是唯一才能在預設 URL 中使用。 使用`$RANDOM`Bash 函數建構名稱可確保唯一`webappname99999`性和格式 的結果。

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. 建立資源群組。 資源組提供了一種聚合要作為組管理的 Azure 資源的方法。

    ```azurecli
    az group create --location centralus --name AzureTutorial
    ```

    這個`az`指令呼叫[Azure CLI](/cli/azure/)。 CLI 可以在本地運行,但在雲外殼中使用它可以節省時間和配置。

    c. 在 S1 層中創建應用服務計畫。 應用服務計劃是共用相同定價層的 Web 應用的分組。 S1 層不是免費的,但它是暫存槽功能所必需的。

    ```azurecli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. 使用同一資源組中的應用服務計劃創建 Web 應用資源。

    ```azurecli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. 設置部署憑據。 這些部署憑據適用於訂閱中的所有 Web 應用。 不要在使用者名中使用特殊字元。

    ```azurecli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. 將 Web 應用設定為接受來自本地 Git 部署並顯示*Git 部署 URL。* **請注意此網址 回報**。

    ```azurecli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. 顯示*Web 應用程式網址*。 瀏覽到此 URL 以檢視空白 Web 應用。 **請注意此網址 回報**。

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. 在本地電腦上使用命令外殼,導航到 Web 應用的專案資料夾(`.\simple-feed-reader\SimpleFeedReader`例如。 執行以下指令以設定 Git 以推送到部署網址:

    a. 將遠端 URL 新增到本地儲存庫。

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. 將本地*主*分支推送到*azure prod* *遠端的主分支*。

    ```console
    git push azure-prod master
    ```

    系統會提示您獲得之前創建的部署憑據。 觀察命令 shell 中的輸出。 Azure 遠端生成ASP.NET核心應用。

4. 在瀏覽器中,導航到*Web 應用 URL*並注意已生成和部署應用。 可以使用將其他更改提交到本地 Git`git commit`儲存庫。 這些更改使用前面的`git push`命令推送到 Azure。

## <a name="deployment-with-visual-studio"></a>使用視覺化工作室進行部署

> *注意:此部分僅適用於 Windows。Linux 和macOS用戶應該進行下面的步驟2中描述的更改。保存檔,並將更改提交到本地存儲庫。 `git commit`最後,使用`git push`推送更改,如第一部分。*

應用已從命令外殼部署。 讓我們使用 Visual Studio 的整合工具將更新部署到應用。 在幕後,Visual Studio 完成與命令列工具相同的任務,但在 Visual Studio 熟悉的 UI 中完成。

1. 在視覺工作室打開*簡單閱讀器.sln。*
2. 在解決方案資源管理員中,開啟*頁面\Index.cshtml*。 將 `<h2>Simple Feed Reader</h2>` 變更為 `<h2>Simple Feed Reader - V2</h2>`。
3. 按**Ctrl**+**移位**+**B**可生成應用程式。
4. 在解決方案資源管理器中,右鍵單擊專案並按一下「**發布**」。

    ![顯示右鍵按一下、發佈功能的螢幕擷取](./media/deploying-to-app-service/publish.png)
5. Visual Studio 可以創建新的應用服務資源,但此更新將通過現有部署發佈。 在「**選擇發佈目標**」對話框中,從左側清單中選擇**應用服務**,然後選擇 **「選擇現有**」。 按一下 **[發行]**。
6. 在 **「應用服務」** 對話框中,確認用於創建 Azure 訂閱的 Microsoft 或組織帳戶顯示在右上角。 如果不是,請按下下拉清單並添加它。
7. 確認選擇了正確的 Azure**訂閱**。 對於**檢視**,請選擇 **「資源群組**」 。。 展開**Azure 教程**資源組,然後選擇現有的 Web 應用。 按一下 [確定]  。

    ![顯示發佈應用服務對話框的螢幕截圖](./media/deploying-to-app-service/publish-dialog.png)

可視化工作室生成應用並將其部署到 Azure。 瀏覽到 Web 應用 URL。 驗證`<h2>`元素修改是否為即時。

![具有已變更標題的應用程式](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>部署位置

部署槽支援暫存更改,而不會影響在生產中運行的應用。 一旦品質保證團隊驗證了應用的暫存版本,就可以交換生產和暫存槽。 以這種方式將暫存中的應用程式提升為生產。 以下步驟創建暫存槽,部署一些更改,並在驗證后將暫存槽與生產交換。

1. 登錄到 Azure[雲外殼](https://shell.azure.com/bash)(如果尚未登錄)。
2. 創建暫存槽。

    a. 創建名稱*暫存*的部署槽。

    ```azurecli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. 將暫存槽配置為使用本地 Git 的部署,並獲取**暫存**部署 URL。 **請注意此網址 回報**。

    ```azurecli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. 顯示暫存槽的 URL。 瀏覽到 URL 以查看空暫存槽。 **請注意此網址 回報**。

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. 在文字編輯器或 Visual Studio 中,再次修改*Pages/Index.cshtml,* 以便`<h2>`元素讀取`<h2>Simple Feed Reader - V3</h2>`並保存檔。

4. 使用 Visual Studio*的團隊資源管理員*選項卡中的 **「更改」** 頁,或使用本地電腦的命令外殼輸入以下內容,將檔案提交到本地 Git 儲存函式庫:

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. 使用本地電腦的命令外殼,將暫存部署網址 加入為 Git 遙控器,然後推送提交的變更:

    a. 將用於暫存的遠端 URL 添加到本地 Git 儲存庫。

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. 將本地*主*分支推送到*Azure 暫存**遠端的主分支*。

    ```console
    git push azure-staging master
    ```

    在 Azure 生成和部署應用時等待。

6. 要驗證 V3 已部署到暫存槽,請打開兩個瀏覽器視窗。 在一個視窗中,導航到原始 Web 應用 URL。 在其他視窗中,導航到暫存 Web 應用 URL。 生產 URL 為應用程式的 V2 提供服務。 暫存 URL 為應用程式的 V3 提供服務。

    ![比較瀏覽器視窗的螢幕擷取](./media/deploying-to-app-service/ready-to-swap.png)

7. 在雲外殼中,將已驗證/預熱的暫存槽交換到生產中。

    ```azurecli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. 通過刷新兩個瀏覽器窗口來驗證交換是否發生。

    ![比較交換瀏覽器視窗](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>總結

在本節中,已完成以下任務:

* 下載並構建了示例應用。
* 使用 Azure 雲外殼創建 Azure 應用服務 Web 應用。
* 使用 Git 將示例應用部署到 Azure。
* 使用 Visual Studio 對應用部署了更改。
* 向 Web 應用添加了暫存槽。
* 已部署到暫存槽的更新。
* 已交換暫存和生產插槽。

在下一節中,您將學習如何使用 Azure 管道構建 DevOps 管道。

## <a name="additional-reading"></a>其他閱讀資料

* [Web Apps 概觀](/azure/app-service/app-service-web-overview)
* [在 Azure App Service 中建置 .NET Core 和 SQL Database Web 應用程式](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [設定 Azure App Service 的部署認證](/azure/app-service/app-service-deployment-credentials)
* [在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)
