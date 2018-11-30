---
title: 使用 ASP.NET Core 和 Azure 的 DevOps |持續整合和部署
author: CamSoper
description: 本指南為如何為 Azure 上裝載的 ASP.NET Core 應用程式，建置 DevOps 管線的完整指導。
ms.author: scaddie
ms.date: 10/24/2018
uid: azure/devops/cicd
ms.openlocfilehash: edaf2c2e1428e5e82104786d94584a4ef08f9ee3
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570083"
---
# <a name="continuous-integration-and-deployment"></a>持續整合和部署

在上一章中，您會建立簡單的摘要讀取程式應用程式的本機 Git 存放庫。 在本章中，您會將該程式碼發行至 GitHub 存放庫，並建構 Azure DevOps 服務管線，使用 Azure 的管線。 持續建置與部署應用程式，可讓管線。 建置和部署至 Azure Web 應用程式的預備位置，就會觸發任何認可的 GitHub 存放庫。

在本節中，您將完成下列工作：

* 將應用程式的程式碼發行至 GitHub
* 中斷連線的本機 Git 部署
* 建立 Azure DevOps 的組織
* Azure DevOps 服務中建立 team 專案
* 建立組建定義
* 建立發行管線
* GitHub 中認可變更，並自動部署至 Azure
* 檢查 Azure 管線的管線

## <a name="publish-the-apps-code-to-github"></a>將應用程式的程式碼發行至 GitHub

1. 開啟瀏覽器視窗，並瀏覽至`https://github.com`。
1. 按一下  **+** 下拉式清單中的標頭，然後選取**新的存放庫**:

    ![GitHub 新的存放庫選項](media/cicd/github-new-repo.png)

1. 選取您的帳戶**擁有者**下拉式清單中，然後輸入*簡單-摘要讀取器*中**存放庫名稱**文字方塊中。
1. 按一下 [**建立存放庫**] 按鈕。
1. 開啟 [本機電腦的命令殼層]。 瀏覽至在其中的目錄*簡單-摘要讀取器*儲存 Git 儲存機制。
1. 重新命名現有*原點*遠端*上游*。 執行下列命令：
    ```console
    git remote rename origin upstream
    ```
1. 加入新*原點*遠端指向您的 GitHub 上的存放庫複本。 執行下列命令：
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. 將本機 Git 儲存機制發行至新建立的 GitHub 存放庫。 執行下列命令：
    ```console
    git push -u origin master
    ```
1. 開啟瀏覽器視窗，並瀏覽至`https://github.com/<GitHub_username>/simple-feed-reader/`。 驗證您的程式碼，會出現在 GitHub 存放庫。

## <a name="disconnect-local-git-deployment"></a>中斷連線的本機 Git 部署

移除本機 Git 部署進行下列步驟。 Azure 的管線 （Azure DevOps 服務） 會取代並增強了該功能。

1. 開啟[Azure 入口網站](https://portal.azure.com/)，然後瀏覽至*接移 (mywebapp\<unique_number\>/預備)* Web 應用程式。 Web 應用程式可以藉由輸入快速找到*預備*入口網站的 [搜尋] 方塊中：

    ![預備 Web 應用程式搜尋字詞](media/cicd/portal-search-box.png)

1. 按一下 **部署選項**。 此時會出現一個新的面板。 按一下 **中斷連線**移除在前一章中已加入本機 Git 原始檔控制組態。 按一下 [確認移除操作**是**] 按鈕。
1. 瀏覽至*mywebapp < unique_number >* App Service。 提醒您，入口網站的 [搜尋] 方塊可用來快速找出應用程式服務。
1. 按一下 **部署選項**。 此時會出現一個新的面板。 按一下 **中斷連線**移除在前一章中已加入本機 Git 原始檔控制組態。 按一下 [確認移除操作**是**] 按鈕。

## <a name="create-an-azure-devops-organization"></a>建立 Azure DevOps 的組織

1. 開啟瀏覽器，並瀏覽至[Azure DevOps 的組織建立頁面](https://go.microsoft.com/fwlink/?LinkId=307137)。
1. 輸入唯一名稱**挑選一個易記的名稱**文字方塊來形成來存取組織的 Azure DevOps 的 URL。
1. 選取  **Git**選項按鈕，因為程式碼裝載於 GitHub 存放庫。
1. 按一下 [繼續] 按鈕。 在之後短暫的等候、 帳戶以及 team 專案，名為*MyFirstProject*，所建立。

    ![Azure DevOps 的組織建立頁面](media/cicd/vsts-account-creation.png)

1. 開啟 確認電子郵件，指出 Azure DevOps 的組織和專案可供使用。 按一下 **啟動您的專案**按鈕：

    ![啟動您的專案 按鈕](media/cicd/vsts-start-project.png)

1. 瀏覽器中開啟 *\<account_name\>。 visualstudio.com*。 按一下  *MyFirstProject*連結，即可開始設定專案的 DevOps 管線。

## <a name="configure-the-azure-pipelines-pipeline"></a>設定 Azure 管線的管線

有三個不同的步驟，才能完成。 完成操作的 DevOps 管線中的下列三個區段結果中的步驟。

### <a name="grant-azure-devops-access-to-the-github-repository"></a>授與 Azure DevOps 的 GitHub 存放庫的存取權

1. 依序展開**建置程式碼，從外部存放庫或**accordion。 按一下 **安裝程式建置**按鈕：

    ![設定組建按鈕](media/cicd/vsts-setup-build.png)

1. 選取  **GitHub**選項**選取來源**區段：

    ![選取來源-GitHub](media/cicd/vsts-select-source.png)

1. Azure DevOps 才能存取您的 GitHub 存放庫需要授權。 請輸入 *< GitHub_username > GitHub 連線*中**連線名稱**文字方塊中。 例如: 

    ![GitHub 連接名稱](media/cicd/vsts-repo-authz.png)

1. 如果您的 GitHub 帳戶上啟用雙因素驗證，個人存取權杖是必要的。 在此情況下，按一下**使用 GitHub 個人存取權杖的授權**連結。 請參閱[官方 GitHub 個人存取權杖建立指示](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)取得協助。 只有*存放庫*需要的權限的範圍。 否則，請按一下**使用 OAuth 授權** 按鈕。
1. 出現提示時，登入您的 GitHub 帳戶。 然後選取 [授權] 可授與您 Azure DevOps 的組織的存取權。 如果成功，則會建立新的服務端點。
1. 按一下省略符號按鈕旁**存放庫** 按鈕。 選取  *< GitHub_username > 簡單-摘要讀取器 /* 從清單中的存放庫。 按一下 [**選取**] 按鈕。
1. 選取 *主要*從分支**手動和排程組建的預設分支**下拉式清單。 按一下 [繼續] 按鈕。 範本的 [選取] 頁面隨即出現。

### <a name="create-the-build-definition"></a>建立組建定義

1. 從 [範本選擇] 頁面中，輸入*ASP.NET Core*在搜尋方塊中：

    ![在 [範本] 頁面上的 ASP.NET Core 搜尋](media/cicd/vsts-template-selection.png)

1. 範本搜尋結果會出現。 將滑鼠停留**ASP.NET Core**範本，然後按**套用** 按鈕。
1. **任務**的組建定義的索引標籤會出現。 按一下 [觸發程序]  索引標籤。
1. 請檢查**啟用持續整合** 方塊中。 底下**分支篩選器**區段中，確認**型別**下拉式清單設定為*Include*。 設定**分支規格**下拉式清單可*主要*。

    ![啟用持續整合設定](media/cicd/vsts-enable-ci.png)

    這些設定會使要若要推送的任何變更時觸發的組建*主要*GitHub 儲存機制分支。 持續整合測試中[認可變更到 GitHub，並自動部署至 Azure](#commit-changes-to-github-and-automatically-deploy-to-azure)一節。

1. 按一下 **儲存與佇列**按鈕，然後選取**儲存**選項：

    ![[儲存] 按鈕](media/cicd/vsts-save-build.png)

1. 下列的強制回應對話方塊隨即出現：

    ![儲存組建定義為強制回應對話方塊](media/cicd/vsts-save-modal.png)

    使用的預設資料夾 *\\* ，然後按一下**儲存** 按鈕。

### <a name="create-the-release-pipeline"></a>建立發行管線

1. 按一下 [**版本**] 索引標籤，您的 team 專案。 按一下 [**新的管線**] 按鈕。

    ![Releases 索引標籤-新增定義 按鈕](media/cicd/vsts-new-release-definition.png)

    範本的 [選取] 窗格隨即出現。

1. 從 [範本選擇] 頁面中，輸入*App Service*在搜尋方塊中：

    ![發行管線範本搜尋 方塊](media/cicd/vsts-release-template-search.png)

1. 範本搜尋結果會出現。 將滑鼠停留**Azure App Service 部署位置**範本，然後按**套用** 按鈕。 **管線**發行管線的索引標籤會出現。

    ![發行管線管線 索引標籤](media/cicd/vsts-release-definition-pipeline.png)

1. 按一下 [**新增**按鈕**成品**] 方塊中。 **新增構件**面板出現時：

    ![發行管線-加入成品面板](media/cicd/vsts-release-add-artifact.png)

1. 選取 **建置**圖格**來源類型**一節。 這個型別允許在發行管線對組建定義的連結。
1. 選取  *MyFirstProject*從**專案**下拉式清單。
1. 選取組建定義名稱*MyFirstProject ASP.NET Core CI*，從**來源 （組建定義）** 下拉式清單。
1. 選取 *最新*從**預設版本**下拉式清單。 此選項會建立組建定義的最新的執行所產生的成品。
1. 中的文字替換**來源別名**具有 textbox*卸除*。
1. 按一下 [新增] 按鈕。 **成品**區段以顯示變更的更新。
1. 按一下 啟用持續部署的閃電圖示：

    ![發行管線成品-閃電圖示](media/cicd/vsts-artifacts-lightning-bolt.png)

    啟用此選項，部署就會發生每個新的組建時的時間。
1. A**持續部署觸發程序**面板出現在右邊。 按一下切換按鈕，即可啟用此功能。 不需要啟用**提取要求觸發程序**。
1. 按一下 **新增**下拉式清單中**組建分支篩選**一節。 選擇**組建定義預設分支**選項。 此篩選條件會導致只會針對從 GitHub 存放庫的組建觸發發行*主要*分支。
1. 按一下 [儲存] 按鈕。 按一下  **確定**按鈕，在產生**儲存**強制回應對話方塊。
1. 按一下 [**環境 1** ] 方塊中。 **環境**面板出現在右邊。 變更*環境 1*中的文字**環境名稱**文字方塊*生產*。

   ![發行管線-環境名稱 文字方塊中](media/cicd/vsts-environment-name-textbox.png)

1. 按一下 **階段 1、 2 個工作**連結**生產**方塊：

    ![發行管線-生產環境 link.png](media/cicd/vsts-production-link.png)

    **任務**環境索引標籤會出現。
1. 按一下 **部署 Azure App Service，插槽以**工作。 它的設定會出現在右邊面板。
1. 選取 Azure 訂用帳戶，並從應用程式服務相關聯**Azure 訂用帳戶**下拉式清單。 選取之後，按一下**授權** 按鈕。
1. 選取  *Web 應用程式*從**應用程式類型**下拉式清單。
1. 選取  *mywebapp / < unique_number / >* 從**App service 名稱**下拉式清單。
1. 選取  *AzureTutorial*從**資源群組**下拉式清單。
1. 選取 *預備*從**插槽**下拉式清單。
1. 按一下 [儲存] 按鈕。
1. 暫留在預設的發行管線名稱。 按一下鉛筆圖示以編輯它。 使用*MyFirstProject ASP.NET Core CD*做為名稱。

    ![發行管線名稱](media/cicd/vsts-release-definition-name.png)

1. 按一下 [儲存] 按鈕。

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>GitHub 中認可變更，並自動部署至 Azure

1. 開啟*SimpleFeedReader.sln* Visual Studio 中。
1. 在 [方案總管] 中，開啟*Pages\Index.cshtml*。 變更`<h2>Simple Feed Reader - V3</h2>`至`<h2>Simple Feed Reader - V4</h2>`。
1. 按下**Ctrl**+**Shift**+**B**建置應用程式。
1. 檔案認可到 GitHub 存放庫。 使用任何一種**變更**Visual Studio 中的頁面*Team Explorer*索引標籤，或執行下列命令，使用本機電腦的命令殼層：

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. 推送變更*主要*分支*原點*遠端的 GitHub 存放庫：

    ```console
    git push origin master
    ```

    認可會出現在 GitHub 儲存機制*主要*分支：

    ![主要分支中的 GitHub 認可](media/cicd/github-commit.png)

    觸發組建時，由於在組建定義中啟用持續整合**觸發程序** 索引標籤：

    ![啟用持續整合](media/cicd/enable-ci.png)

1. 瀏覽至**已排入佇列**索引標籤**Azure 管線** > **建置**Azure DevOps 服務中的頁面。 已排入佇列的組建會顯示新分支和認可觸發組建：

    ![已排入佇列的組建](media/cicd/build-queued.png)

1. 一旦建置成功時，它會部署至 Azure，就會發生。 瀏覽至瀏覽器中的應用程式。 請注意，"V4"的文字會出現在標題中：

    ![更新應用程式](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>檢查 Azure 管線的管線

### <a name="build-definition"></a>組建定義

建立組建定義名稱*MyFirstProject ASP.NET Core CI*。 完成時，組建會產生 *.zip*包含要發佈的資產檔案。 發行管線部署到 Azure 的資產。

組建定義**任務**索引標籤會列出所使用的個別步驟。 有五個建置工作。

![建置定義工作](media/cicd/build-definition-tasks.png)

1. **還原**&mdash;執行`dotnet restore`還原應用程式的 NuGet 套件 命令。 預設封裝使用的摘要是 nuget.org。
1. **建置**&mdash;執行`dotnet build --configuration release`命令以編譯應用程式的程式碼。 這`--configuration`選項用來產生程式碼，適合用來部署至生產環境的最佳化的版本。 修改*BuildConfiguration*在組建定義的變數**變數**索引標籤上，如果您需要偵錯組態，例如。
1. **測試**&mdash;執行`dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`命令來執行應用程式的單元測試。 執行單元測試內任何 C# 專案符合`**/*Tests/*.csproj`glob 模式。 測試結果會儲存在 *.trx*所指定的位置在檔案`--results-directory`選項。 如果任何測試失敗，組建會失敗，並不會部署。

    > [!NOTE]
    > 若要確認單元測試工作，修改*SimpleFeedReader.Tests\Services\NewsServiceTests.cs*特意不中斷其中一個測試。 例如，變更`Assert.True(result.Count > 0);`要`Assert.False(result.Count > 0);`在`Returns_News_Stories_Given_Valid_Uri`方法。 認可並推送變更至 GitHub。 組建會觸發，並會失敗。 建置管線狀態會變成**失敗**。 一次還原的變更、 認可並推送。 建置成功。

1. **發佈**&mdash;執行`dotnet publish --configuration release --output <local_path_on_build_agent>`命令，以產生 *.zip*與要部署之成品的檔案。 `--output`選項指定的發行位置 *.zip*檔案。 位置由傳遞[預先定義的變數](/azure/devops/pipelines/build/variables)名為`$(build.artifactstagingdirectory)`。 該變數會展開為本機路徑，例如*c:\agent\_work\1\a*，組建代理程式上。
1. **發行成品** &mdash; Publishes *.zip*所產生檔案**發行**工作。 此工作會接受 *.zip*檔案位置做為參數，也就是預先定義的變數`$(build.artifactstagingdirectory)`。 *.Zip*檔案會發行名為的資料夾*卸除*。

按一下 組建定義**摘要**連結以檢視組建定義的歷程記錄：

![建置定義歷程記錄](media/cicd/build-definition-summary.png)

在產生的頁面上，按一下對應至唯一的組建編號的連結：

![組建定義摘要 頁面](media/cicd/build-definition-completed.png)

這個特定組建的摘要會顯示。 按一下 **成品**索引標籤，並注意*卸除*組建所產生的資料夾會列出：

![組建定義成品-置放資料夾](media/cicd/build-definition-artifacts.png)

使用**下載**並**瀏覽**檢查已發行的成品的連結。

### <a name="release-pipeline"></a>發行管線

發行管線建立同名*MyFirstProject ASP.NET Core CD*:

![發行管線的概觀](media/cicd/release-definition-overview.png)

發行管線的兩個主要元件如下**成品**並**環境**。 按一下  方塊中的**成品**區段會顯示下列窗格：

![發行管線成品](media/cicd/release-definition-artifacts.png)

**來源 （組建定義）** 值代表此發行管線所連結的組建定義。 *.Zip*成功執行的組建定義時所產生的檔案提供給*生產*環境以部署至 Azure。 按一下 *階段 1、 2 個工作*連結*生產*環境方塊，以檢視發行管線工作：

![發行管線工作](media/cicd/release-definition-tasks.png)

發行管線是由兩個工作所組成：*位置中部署 Azure App Service*並*管理 Azure App Service-位置交換*。 按一下第一項工作會顯示下列工作組態：

![發行管線部署工作](media/cicd/release-definition-task1.png)

在 [部署] 工作中定義的 Azure 訂用帳戶、 服務類型、 web 應用程式名稱、 資源群組和部署位置。 **封裝或資料夾**文字方塊中會保留 *.zip*擷取並部署到的檔案路徑*預備*插槽*mywebapp\<唯一（_n)\>*  web 應用程式。

按一下位置交換工作會顯示下列工作組態：

![發行管線位置交換工作](media/cicd/release-definition-task2.png)

提供訂用帳戶、 資源群組、 服務類型、 web 應用程式名稱，以及部署位置的詳細資料。 **與生產環境交換** 核取方塊。 因此，將位元部署到*預備*位置交換到生產環境。

## <a name="additional-reading"></a>其他閱讀資料

* [使用 Azure Pipelines 建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)
* [組建和.NET Core 專案](/azure/devops/pipelines/languages/dotnet-core)
* [部署 web 應用程式與 Azure 的管線](/azure/devops/pipelines/targets/webapp)
