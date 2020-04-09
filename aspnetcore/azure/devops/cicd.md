---
title: 持續整合與部署 - 使用 ASP.NET 核心與 Azure 的 DevOps
author: CamSoper
description: 使用ASP.NET核心和 Azure 在 DevOps 中持續整合和部署
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: mvc, seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 5fdf52235b49119503885f92c370dc588e809ffe
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78655831"
---
# <a name="continuous-integration-and-deployment"></a>持續整合及部署

在上一章中,您為簡單源閱讀器應用創建了本地 Git 儲存庫。 在本章中,您將將該代碼發佈到 GitHub 儲存庫,並使用 Azure 管道建構 Azure DevOps 服務管道。 管道支援應用的連續生成和部署。 任何提交到 GitHub 儲存庫都觸發生成和部署到 Azure Web 應用的暫存槽。

在本節中,您將完成以下任務:

* 將應用程式的代碼發布到 GitHub
* 斷線開發的 Git 部署
* 建立 Azure DevOps 組織
* 在 Azure DevOps 服務中建立團隊專案
* 建立組建定義
* 建立發行管線
* 將變更認可至 GitHub 並自動部署至 Azure
* 檢查 Azure 管道導管

## <a name="publish-the-apps-code-to-github"></a>將應用程式的代碼發布到 GitHub

1. 開啟瀏覽器視窗,然後瀏覽`https://github.com`到 。
1. 按下**+** 標題中的下拉清單,然後選擇 **"新建儲存庫**"

    ![GitHub 新儲存函式庫選項](media/cicd/github-new-repo.png)

1. 在 **「擁有者」** 下拉清單中選擇您的帳戶,並在**儲存庫名稱**文字框中輸入*簡單來源閱讀器*。
1. 按下「**創建儲存庫**」 按鈕。
1. 打開本地電腦的命令外殼。 導航到儲存*簡單源讀取器*Git 儲存庫的目錄。
1. 將現有*來源*重新命名到遠端到*上游*。 執行下列命令：

    ```console
    git remote rename origin upstream
    ```

1. 在 GitHub 上添加指向儲存庫副本的新*源*遙控器。 執行下列命令：

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. 將本地 Git 儲存庫發表到新創建的 GitHub 儲存庫。 執行下列命令：

    ```console
    git push -u origin master
    ```

1. 開啟瀏覽器視窗,然後瀏覽`https://github.com/<GitHub_username>/simple-feed-reader/`到 。 驗證您的代碼是否顯示在 GitHub 儲存庫中。

## <a name="disconnect-local-git-deployment"></a>斷線開發的 Git 部署

使用以下步驟刪除本地 Git 部署。 Azure 管道(Azure DevOps 服務)既替換並增強了該功能。

1. 打開[Azure 門戶](https://portal.azure.com/),然後導航到*暫存\<(mywebapp unique_number\>/暫存)Web*應用。 透過在門戶搜尋框中輸入*暫存,* 可以快速定位 Web 應用:

    ![暫存 Web 應用搜尋詞](media/cicd/portal-search-box.png)

1. 點選**部署中心**。 將顯示一個新面板。 按下 **「斷開連線**」可刪除上一章中添加的本地 Git 原始程式碼管理設定。 按下「**是**」按鈕確認刪除操作。
1. 導航到*mywebapp<unique_number>* 應用服務。 作為提醒,門戶的搜索框可用於快速查找應用服務。
1. 點選**部署中心**。 將顯示一個新面板。 按下 **「斷開連線**」可刪除上一章中添加的本地 Git 原始程式碼管理設定。 按下「**是**」按鈕確認刪除操作。

## <a name="create-an-azure-devops-organization"></a>建立 Azure DevOps 組織

1. 開啟瀏覽器,然後瀏覽到[Azure DevOps 組織建立頁](https://go.microsoft.com/fwlink/?LinkId=307137)。
1. 在 **「選取令人難忘的名稱**文字框」中鍵入唯一名稱,以形成用於訪問 Azure DevOps 組織的 URL。
1. 選擇**Git**單選按鈕,因為代碼託管在 GitHub 儲存庫中。
1. 按一下 [繼續]**** 按鈕。 經過短暫的等待,將創建名為*MyFirstProject*的帳戶和團隊專案。

    ![Azure DevOps 組織建立頁](media/cicd/vsts-account-creation.png)

1. 打開確認電子郵件,指示 Azure DevOps 組織和專案已準備就緒, 按下「**啟動項目」** 按鈕:

    ![啟動項目按鈕](media/cicd/vsts-start-project.png)

1. 瀏覽器將開啟*\<到\>account_name .visualstudio.com*。 按下*MyFirstProject*連結開始配置專案的 DevOps 管道。

## <a name="configure-the-azure-pipelines-pipeline"></a>設定 Azure 導管導管

有三個不同的步驟要完成。 完成以下三節中的步驟后,將生成可操作的 DevOps 管道。

### <a name="grant-azure-devops-access-to-the-github-repository"></a>授予 Azure 開發人員對 GitHub 儲存庫的權限

1. 從外部儲存函式庫手風琴延伸**或產生代碼**。 點選 **「設定產生」** 按鈕:

    ![設定產生按鈕](media/cicd/vsts-setup-build.png)

1. 從選擇**來源**「部份中選擇**GitHub**選項:

    ![選擇來源 - GitHub](media/cicd/vsts-select-source.png)

1. 在 Azure DevOps 可以訪問 GitHub 儲存庫之前,需要授權。 在**連接名稱**文字框中*輸入> GitHub 連接<GitHub_username。* 例如：

    ![GitHub 連線名稱](media/cicd/vsts-repo-authz.png)

1. 如果在 GitHub 帳戶上啟用了雙重身份驗證,則需要個人訪問令牌。 在這種情況下,按下**使用 GitHub 個人存取權杖連結的授權**。 有關說明,請參閱[官方 GitHub 個人存取權杖建立說明](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)。 只需要許可權的*回購*範圍。 否則,按下 **「使用 OAuth 授權**」按鈕。
1. 出現提示后,請登錄到 GitHub 帳戶。 然後選擇"授權"以授予對 Azure DevOps 組織的訪問許可權。 如果成功,將創建新的服務終結點。
1. 按下 **「存儲庫」** 按鈕旁邊的省略號按鈕。 從清單中選擇*GitHub_username>/簡單饋送讀取體儲存庫<。* 按一下 [選取]**** 按鈕。
1. 從 **「預設分支」中選擇主分支,用於手動和計畫生成**下拉*master*清單。 按一下 [繼續]**** 按鈕。 將顯示範本選擇頁。

### <a name="create-the-build-definition"></a>建立組建定義

1. 在樣本選擇頁中,在搜尋框中輸入*ASP.NET核心*:

    ![ASP.NET範本頁面上的核心搜尋](media/cicd/vsts-template-selection.png)

1. 將顯示範本搜尋結果。 將滑鼠懸停在**ASP.NET核心**範本上,然後按下「**應用」** 按鈕。
1. 將顯示生成定義的 **「任務**」選項卡。 按下「**觸發」** 選項卡。
1. 選中"**啟用連續集成**"框。 在 **「分支篩選器」** 部分下,確認 **「類型**下拉清單」設置為 *「包括*」。。 將**分支規範**下拉清單設定為*主*。

    ![開啟連續整合設定](media/cicd/vsts-enable-ci.png)

    當將任何更改推送到 GitHub 儲存庫*的主*分支時,這些設置會導致生成觸發。 在[GitHub 的「提交」更改中測試持續整合,自動部署到 Azure](#commit-changes-to-github-and-automatically-deploy-to-azure)部分。

1. 按下「**儲存& 佇列**」按鈕,然後選擇「**儲存**」選項:

    ![[儲存] 按鈕](media/cicd/vsts-save-build.png)

1. 顯示以下模式對話框:

    ![儲存產生定義 - 模式對話框](media/cicd/vsts-save-modal.png)

    使用 的預設*\\*資料夾 ,然後按下 **「儲存**」按鈕。

### <a name="create-the-release-pipeline"></a>建立發行管線

1. 單擊團隊專案的 **「發佈」** 選項卡。 按下 **「新建管道**」 按鈕。

    ![釋放選項卡 - 新訂按鈕](media/cicd/vsts-new-release-definition.png)

    將顯示範本選擇窗格。

1. 在樣本選擇頁中,在搜尋框中輸入*應用服務*:

    ![發布導管樣本搜尋框](media/cicd/vsts-release-template-search.png)

1. 將顯示範本搜尋結果。 使用插槽範本將滑鼠懸停在**Azure 應用服務部署**上,然後單擊「**應用」** 按鈕。 將顯示發佈**管道的管道**選項卡。

    ![釋放導管導管選項卡](media/cicd/vsts-release-definition-pipeline.png)

1. 按下 **「項目**」 的 **「新增**」 按鈕。 將顯示 **'新增專案'** 面板:

    ![發布導管 - 新增工件面板](media/cicd/vsts-release-add-artifact.png)

1. 從 **「源類型」** 部分選擇 **「生成**」磁貼。 此類型允許將發佈管道連結到生成定義。
1. 從**專案**下拉清單中選擇*MyFirst 專案*。
1. 從 **「源(生成定義)** 下拉清單中選擇生成定義名稱*MyFirstProject-ASP.NET Core-CI。*
1. 從**預設版本**下拉清單中選擇 *「最新*」。 此選項生成生成定義的最新運行生成的工件。
1. 將**源別名**文字框中的文字取代為 *"刪除*"。
1. 按一下 [新增]**** 按鈕。 **項目「** 部分將更新以顯示更改」。
1. 點選閃電圖示以開啟連續部署:

    ![釋放導管防火 - 閃電圖示](media/cicd/vsts-artifacts-lightning-bolt.png)

    啟用此選項后,每次新生成可用時都會進行部署。
1. **"連續部署觸發器**"面板顯示在右側。 按下切換按鈕以啟用該功能。 不要開啟 **「拉取」要求觸發器**。
1. 按下 **「生成分支篩選器**」部分中的「**添加**下拉清單」。 選擇**定義的預設分支**選項。 此篩選器使版本僅觸發來自 GitHub 儲存庫*的主*分支的生成。
1. 按一下 [儲存]**** 按鈕。 按下產生的 **「儲存**模式」對話框中的 **「確定**」按鈕。
1. 按下 **「環境 1」** 框。 **右側將顯示"環境**"面板。 將**環境名稱**文字框中*的環境 1*文字更改為 *「生產*」。

   ![執行導管 - 環境名稱文字框](media/cicd/vsts-environment-name-textbox.png)

1. 點選 **「生產**」 框中**1 階段、2 個工作**連結:

    ![啟動導管 - 生產環境連結.png](media/cicd/vsts-production-link.png)

    將顯示環境的 **「任務**」選項卡。
1. 按下「**將 Azure 應用服務部署到槽**」任務。 其設置顯示在右側的面板中。
1. 從**Azure 訂閱**下拉清單中選擇與應用服務關聯的 Azure 訂閱。 選中後,按下 **「授權」** 按鈕。
1. 從**套用型態**下拉清單中選擇*Web 應用*。
1. 從**應用服務名稱**下拉清單中選擇*mywebapp/<unique_number/>。*
1. 從**資源群組**下拉清單中選擇*Azure 教程*。
1. 從 **「插槽**」 下拉清單中選擇*暫存*。
1. 按一下 [儲存]**** 按鈕。
1. 將滑鼠懸停在預設發佈管道名稱上。 按一下鉛筆圖示進行編輯。 使用*MyFirstProject-ASP.NET核心 CD*作為名稱。

    ![釋放導管名稱](media/cicd/vsts-release-definition-name.png)

1. 按一下 [儲存]**** 按鈕。

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>將變更認可至 GitHub 並自動部署至 Azure

1. 在視覺工作室打開*簡單閱讀器.sln。*
1. 在解決方案資源管理員中,開啟*頁面\Index.cshtml*。 將 `<h2>Simple Feed Reader - V3</h2>` 變更為 `<h2>Simple Feed Reader - V4</h2>`。
1. 按**Ctrl**+**移位**+**B**可生成應用程式。
1. 將檔提交到 GitHub 儲存庫。 使用 Visual Studio*的團隊資源管理員*選項卡中的 **「更改」** 頁,或使用本地電腦的命令外殼執行以下內容:

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. 將*主*分支的更改推送到 GitHub 儲存函式庫的原*點*遠端:

    ```console
    git push origin master
    ```

    提交將顯示在 GitHub*儲存函式庫的主分支*中:

    ![GitHub 提交主分支](media/cicd/github-commit.png)

    產生將觸發,因為在生成定義的**觸發器**選項卡中啟用了連續整合:

    ![實現持續整合](media/cicd/enable-ci.png)

1. 導覽 Azure DevOps 服務中的**Azure 管道** > **產生**頁的 **「已排隊」** 選項卡。 重新產生的佇列的顯示觸發成的分支與提交:

    ![佇列產生](media/cicd/build-queued.png)

1. 生成成功後,將部署到 Azure。 在瀏覽器中導航到應用。 請注意,「V4」文字顯示在標題中:

    ![更新的應用程式](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>檢查 Azure 管道導管

### <a name="build-definition"></a>組建定義

使用*核心 CI MyFirstProject-ASP.NET*創建了生成定義。 完成後,生成將生成一個 *.zip*檔,包括要發佈的資產。 發佈管道將這些資產部署到 Azure。

生成定義**任務選項**卡列出了正在使用的各個步驟。 有五個生成任務。

![組建定義工作](media/cicd/build-definition-tasks.png)

1. **Restore**還原`dotnet restore`執行命令以還原應用的 NuGet&mdash;包。 使用的預設包饋送為nuget.org。
1. **生成**&mdash;執行`dotnet build --configuration release`命令以編譯應用的代碼。 此選項`--configuration`用於生成代碼的優化版本,該版本適用於部署到生產環境。 如果需要調試*配置*,則修改生成定義"**變數"** 選項卡上的 Build 配置變數。
1. **測試**&mdash;執行`dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`命令 以運行應用的單元測試。 單元測試在匹配`**/*Tests/*.csproj`球形模式的任何 C# 專案中執行。 測試結果儲存在`--results-directory`選項指定的位置的 *.trx*檔案中。 如果任何測試失敗,則生成將失敗,並且未部署。

    > [!NOTE]
    > 要驗證單元測試的工作,請修改*SimpleFeedReader.Test_Services_NewsServiceTest.cs*以故意中斷其中一個測試。 例如,在`Assert.True(result.Count > 0);`方法`Assert.False(result.Count > 0);`中更改為`Returns_News_Stories_Given_Valid_Uri`。 提交更改並將更改推送到 GitHub。 生成將觸發並失敗。 產生導管狀態變更為**失敗**。 還原更改、提交並再次推送。 生成成功。

1. **執行**&mdash;執行指令`dotnet publish --configuration release --output <local_path_on_build_agent>`以產生包含要部署的專案的 *.zip*檔。 這個`--output`選項指定 *.zip*檔的發佈位置。 該位置是通過傳遞名為[的預定義變數](/azure/devops/pipelines/build/variables)來`$(build.artifactstagingdirectory)`指定的。 該變數延伸到生成代理上的本地路徑,例如*c:\代理\_工作\1\a。*
1. **發佈項目**&mdash;發佈**由發佈**任務生成的 *.zip*檔。 工作接受 *.zip*檔案位置作為參數,這是預先定義`$(build.artifactstagingdirectory)`的變數 。 *.zip*檔案作為名為*drop*的資料夾發佈。

點選產生定義的 **「摘要」** 連結,檢視具有定義的產生歷史紀錄:

![顯示產生定義歷史記錄的螢幕擷取](media/cicd/build-definition-summary.png)

在產生的頁面上,按下與唯一內部編號對應的連結:

![顯示產生定義摘要頁的螢幕擷取](media/cicd/build-definition-completed.png)

將顯示此特定生成摘要。 按下「**項目」** 選項卡,並注意到列出生成產生的*放置*資料夾:

![顯示產生定義的螢幕截圖 - 放置資料夾](media/cicd/build-definition-artifacts.png)

使用 **「下載**和**瀏覽」** 連結檢查已發布的專案。

### <a name="release-pipeline"></a>發行管線

建立一個發佈導管,名稱*MyFirstProject-ASP.NET核心 CD*:

![顯示宣告導管概述的螢幕擷圖](media/cicd/release-definition-overview.png)

執行導管的兩個主要元件是**工件****與環境**。 按下 **「工件」** 部分中的框將顯示以下面板:

![顯示釋放導管的螢幕擷圖](media/cicd/release-definition-artifacts.png)

**源(生成定義)** 值表示此發佈管道連結到的生成定義。 成功執行生成定義生成的 *.zip*檔案將提供給*生產*環境以部署到 Azure。 按下 *「生產*環境」 方塊中 *, 2 個工作*連結以檢視發佈導管工作:

![顯示宣告導管工作的螢幕擷取](media/cicd/release-definition-tasks.png)

執行導管由兩個工作組成:*將 Azure 應用服務部署到插槽**和管理 Azure 應用服務 - 插槽交換*。 按一個工作會顯示以下任務設定:

![顯示宣告導管部署工作的螢幕擷取](media/cicd/release-definition-task1.png)

Azure 訂閱、服務類型、Web 應用名稱、資源組和部署槽在部署任務中定義。 **包或資料夾**文字框保存要提取和部署到*\<\> mywebapp unique_number Web*應用*的暫存*槽的 *.zip*檔路徑。

點選插槽交換工作會顯示以下任務設定:

![顯示宣告導管插槽交換工作的螢幕截圖](media/cicd/release-definition-task2.png)

提供了訂閱、資源組、服務類型、Web 應用名稱和部署槽詳細資訊。 選中"**與生產交換**「複選框。 因此,部署到*暫存*槽的位將交換到生產環境中。

## <a name="additional-reading"></a>其他閱讀資料

* [使用 Azure Pipelines 建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)
* [產生與 .NET 核心項目](/azure/devops/pipelines/languages/dotnet-core)
* [使用 Azure 管道部署 Web 應用](/azure/devops/pipelines/targets/webapp)
