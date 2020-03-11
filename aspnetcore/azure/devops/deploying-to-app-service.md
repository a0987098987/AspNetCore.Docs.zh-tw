---
title: 使用 ASP.NET Core 和 Azure 將應用程式部署到 App Service DevOps
author: CamSoper
description: 將 ASP.NET Core 應用程式部署至 Azure App Service，這是使用 ASP.NET Core 和 Azure 進行 DevOps 的第一個步驟。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657742"
---
# <a name="deploy-an-app-to-app-service"></a>將應用程式部署至 App Service

[Azure App Service](/azure/app-service/)是 Azure 的 web 裝載平臺。 將 web 應用程式部署至 Azure App Service 可以手動方式或透過自動化程式來完成。 本指南的這一節將討論可以手動觸發的部署方法，或是使用命令列以腳本手動觸發，或使用 Visual Studio 以手動方式觸發。

在本節中，您將完成下列工作：

* 下載並建立範例應用程式。
* 使用 Azure Cloud Shell 建立 Azure App Service Web 應用程式。
* 使用 Git 將範例應用程式部署至 Azure。
* 使用 Visual Studio 將變更部署至應用程式。
* 將預備位置新增至 web 應用程式。
* 將更新部署至預備位置。
* 交換預備與生產位置。

## <a name="download-and-test-the-app"></a>下載並測試應用程式

本指南中使用的應用程式是預先建立的 ASP.NET Core 應用程式、簡單的摘要[讀取器](https://github.com/Azure-Samples/simple-feed-reader/)。 這是一個 Razor Pages 應用程式，它會使用 `Microsoft.SyndicationFeed.ReaderWriter` API 來抓取 RSS/Atom 摘要，並將新聞專案顯示在清單中。

您可以自由地檢查程式碼，但請務必瞭解，這不是關於此應用程式的任何特殊資訊。 這只是一個簡單的 ASP.NET Core 應用程式，供說明之用。

從命令 shell 下載程式代碼、建立專案，然後如下所示執行。

> *注意： Linux/macOS 使用者應該對路徑進行適當的變更，例如，使用正斜線（`/`），而不是反斜線（`\`）。*

1. 將程式碼複製到本機電腦上的資料夾。

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. 將工作資料夾變更為已建立的「*簡易摘要讀取器*」資料夾。

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. 還原套件，並建立解決方案。

    ```dotnetcli
    dotnet build
    ```

4. 執行應用程式。

    ```dotnetcli
    dotnet run
    ```

    ![Dotnet 執行命令成功](./media/deploying-to-app-service/dotnet-run.png)

5. 開啟瀏覽器並瀏覽至 `http://localhost:5000` 。 應用程式可讓您輸入或貼上新聞訂閱摘要 URL，並查看新聞專案的清單。

     ![顯示 RSS 摘要內容的應用程式](./media/deploying-to-app-service/app-in-browser.png)

6. 當您滿意應用程式正常運作後，請在命令介面中按**Ctrl**+**C**將它關閉。

## <a name="create-the-azure-app-service-web-app"></a>建立 Azure App Service Web 應用程式

若要部署應用程式，您必須建立 App Service [Web 應用程式](/azure/app-service/app-service-web-overview)。 建立 Web 應用程式之後，您會使用 Git 從本機電腦進行部署。

1. 登入 [Azure Cloud Shell](https://shell.azure.com/bash)。 注意：當您第一次登入時，Cloud Shell 會提示您建立設定檔的儲存體帳戶。 接受預設值，或提供唯一的名稱。

2. 請使用 Cloud Shell 來執行下列步驟。

    a. 宣告變數以儲存 web 應用程式的名稱。 名稱必須是唯一的，才能在預設 URL 中使用。 使用 `$RANDOM` Bash 函式來建立名稱保證唯一性，並產生格式 `webappname99999`。

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. 建立資源群組。 資源群組提供了一種方法，可匯總要以群組方式管理的 Azure 資源。

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    `az` 命令會叫用[Azure CLI](/cli/azure/)。 CLI 可以在本機執行，但在 Cloud Shell 中使用會節省時間和設定。

    c. 在 S1 層中建立 App Service 計畫。 App Service 計畫是共用相同定價層的 web 應用程式群組。 S1 層不是免費的，但它是預備位置功能的必要項。

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. 在相同的資源群組中，使用 App Service 方案建立 web 應用程式資源。

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. 設定部署認證。 這些部署認證適用于您訂用帳戶中的所有 web 應用程式。 請勿在使用者名稱中使用特殊字元。

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. 將 web 應用程式設定為接受本機 Git 的部署，並顯示*Git 部署 URL*。 **請記下此 URL 以供稍後參考**。

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. 顯示*web 應用程式 URL*。 流覽至此 URL，以查看空白的 web 應用程式。 **請記下此 URL 以供稍後參考**。

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. 在您的本機電腦上使用命令 shell，流覽至 web 應用程式的專案資料夾（例如 `.\simple-feed-reader\SimpleFeedReader`）。 執行下列命令來設定 Git 以推送至部署 URL：

    a. 將遠端 URL 新增至本機存放庫。

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. 將本機*master*分支推送至*azure 生產*遠端的*主要*分支。

    ```console
    git push azure-prod master
    ```

    系統會提示您輸入稍早建立的部署認證。 觀察命令 shell 中的輸出。 Azure 會從遠端建立 ASP.NET Core 應用程式。

4. 在瀏覽器中，流覽至*Web 應用程式 URL* ，並記下已建立並部署應用程式。 您可以使用 `git commit`，將其他變更認可至本機 Git 存放庫。 這些變更會使用上述 `git push` 命令推送至 Azure。

## <a name="deployment-with-visual-studio"></a>使用 Visual Studio 部署

> *注意：本節僅適用于 Windows。Linux 和 macOS 使用者應該進行下列步驟2所述的變更。儲存檔案，並使用 `git commit`將變更認可至本機存放庫。最後，使用 `git push`推送變更，如第一節所示。*

已從命令 shell 部署應用程式。 讓我們使用 Visual Studio 的整合式工具，將更新部署到應用程式。 在幕後，Visual Studio 完成與命令列工具相同的工作，但在 Visual Studio 的熟悉 UI 中。

1. 在 Visual Studio 中開啟*SimpleFeedReader* 。
2. 在方案總管中，開啟*Pages\Index.cshtml*。 將 `<h2>Simple Feed Reader</h2>` 變更為 `<h2>Simple Feed Reader - V2</h2>`。
3. 按**Ctrl**+**Shift**+**B**來建立應用程式。
4. 在方案總管中，以滑鼠右鍵按一下專案，然後按一下 [**發佈**]。

    ![顯示以滑鼠右鍵按一下、發佈的螢幕擷取畫面](./media/deploying-to-app-service/publish.png)
5. Visual Studio 可以建立新的 App Service 資源，但此更新將會透過現有部署發行。 在 [**挑選發行目標**] 對話方塊中，從左側清單中選取 [ **App Service** ]，然後選取 [**選取現有**的]。 按一下 [發佈]。
6. 在 [ **App Service** ] 對話方塊中，確認用來建立 Azure 訂用帳戶的 Microsoft 或組織帳戶會顯示在右上方。 如果不是，請按一下下拉式並新增它。
7. 確認已選取正確的 Azure**訂**用帳戶。 針對 [ **View**]，選取 [**資源群組**]。 展開 [ **AzureTutorial** ] 資源群組，然後選取現有的 web 應用程式。 按一下 [確定]。

    ![顯示 [發行 App Service] 對話方塊的螢幕擷取畫面](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio 建立應用程式並將其部署至 Azure。 流覽至 web 應用程式 URL。 驗證 `<h2>` 元素的修改是否已上線。

![已變更標題的應用程式](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>部署位置

部署位置支援變更的暫存，而不會影響在生產環境中執行的應用程式。 一旦應用程式的階段式版本由品質保證小組驗證，就可以交換生產和預備位置。 預備環境中的應用程式會以這種方式升級到生產。 下列步驟會建立預備位置、對其部署一些變更，並在驗證後交換預備位置與生產位置。

1. 如果尚未登入，請登入[Azure Cloud Shell](https://shell.azure.com/bash)。
2. 建立預備位置。

    a. 建立名為*預備*的部署位置。

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. 將預備位置設定為使用本機 Git 的部署，並取得**預備**部署 URL。 **請記下此 URL 以供稍後參考**。

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. 顯示預備位置的 URL。 流覽至 URL，以查看空的預備位置。 **請記下此 URL 以供稍後參考**。

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. 在文字編輯器或 Visual Studio 中，再次修改*Pages/Index. cshtml* ，讓 `<h2>` 元素讀取 `<h2>Simple Feed Reader - V3</h2>` 並儲存檔案。

4. 使用 Visual Studio 的 [ *Team Explorer* ] 索引標籤中的 [**變更**] 頁面，或使用本機電腦的命令 shell 輸入下列內容，將檔案認可至本機 Git 存放庫：

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. 使用本機電腦的命令 shell，將預備部署 URL 新增為 Git 遠端，並推送認可的變更：

    a. 將預備環境的遠端 URL 新增至本機 Git 存放庫。

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. 將本機*master*分支推送至*azure 預備*遠端的*主要*分支。

    ```console
    git push azure-staging master
    ```

    等候 Azure 建立及部署應用程式。

6. 若要確認 V3 已部署至預備位置，請開啟兩個瀏覽器視窗。 在一個視窗中，流覽至原始的 web 應用程式 URL。 在另一個視窗中，流覽至預備 web 應用程式 URL。 生產 URL 會提供應用程式的第2版。 預備 URL 提供應用程式的 V3。

    ![比較瀏覽器視窗的螢幕擷取畫面](./media/deploying-to-app-service/ready-to-swap.png)

7. 在 Cloud Shell 中，將已驗證/準備就緒的預備位置交換到生產環境。

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. 重新整理兩個瀏覽器視窗，以確認交換是否已發生。

    ![在交換之後比較瀏覽器視窗](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>摘要

在本節中，已完成下列工作：

* 已下載並建立範例應用程式。
* 使用 Azure Cloud Shell 建立 Azure App Service Web 應用程式。
* 使用 Git 將範例應用程式部署至 Azure。
* 使用 Visual Studio 將變更部署至應用程式。
* 已將預備位置新增至 web 應用程式。
* 已將更新部署至預備位置。
* 已交換預備和生產位置。

在下一節中，您將瞭解如何使用 Azure Pipelines 建立 DevOps 管線。

## <a name="additional-reading"></a>其他閱讀資料

* [Web Apps 概觀](/azure/app-service/app-service-web-overview)
* [在 Azure App Service 中建立 .NET Core 和 SQL Database web 應用程式](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [設定 Azure App Service 的部署認證](/azure/app-service/app-service-deployment-credentials)
* [在 Azure App Service 中設定預備環境](/azure/app-service/web-sites-staged-publishing)
