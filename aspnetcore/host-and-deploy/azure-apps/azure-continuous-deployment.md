---
title: 搭配 ASP.NET Core 使用 Visual Studio 與 Git 持續部署至 Azure
author: rick-anderson
description: 了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 97da88b6fb79944d99b69c92eb611dd0e4e39454
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85400163"
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>搭配 ASP.NET Core 使用 Visual Studio 與 Git 持續部署至 Azure

作者：[Erik Reitan](https://github.com/Erikre)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

本教學課程說明如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過持續部署將它從 Visual Studio 部署到 Azure App Service。

另請參閱[使用 Azure Pipelines 建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)，顯示如何使用 Azure DevOps Services，為 [Azure App Service](/azure/app-service/app-service-web-overview) 設定持續傳遞 (CD) 工作流程。 Azure Pipelines (一種 Azure DevOps Services 服務) 可輕鬆設定強大的部署管線，為託管於 Azure App Service 的應用程式發佈更新。 您可以透過 Azure 入口網站設定這個管道，以建置，執行測試，部署到預備位置，然後再部署到實際執行環境。

> [!NOTE]
> 若要完成本教學課程，您需要 Microsoft Azure 帳戶。 若要取得帳戶，請[啟動 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)或[註冊免費試用](https://azure.microsoft.com/free/dotnet/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>必要條件

本教學課程假設您已安裝下列軟體：

* [Visual Studio](https://visualstudio.microsoft.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* 適用于 Windows 的[Git](https://git-scm.com/downloads)

## <a name="create-an-aspnet-core-web-app"></a>建立 ASP.NET Core Web 應用程式

1. 啟動 Visual Studio。

1. 從 [檔案]**** 功能表選取 [新增]**** > [專案]****。

1. 選取 [ASP.NET Core Web 應用程式]**** 專案範本。 它會出現在 [**已安裝**的  >  **範本**]  >  **Visual c #**  >  **.net Core**底下。 將專案命名為 `SampleWebAppDemo`。 選取 [建立新的 Git 存放庫]**** 選項，然後按一下 [確定]****。

   ![[新增專案] 對話方塊](azure-continuous-deployment/_static/01-new-project.png)

1. 在 [新增 ASP.NET Core 專案]**** 對話方塊中，選取 ASP.NET Core 的 [空白]**** 範本，然後按一下 [確定]****。

   ![[新增 ASP.NET Core 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> .NET Core 的最新版本為 2.0。

### <a name="running-the-web-app-locally"></a>在本機執行 Web 應用程式

1. 一旦 Visual Studio 完成建立應用程式，請選取 [**調試**程式] [  >  **開始調試**] 來執行應用程式。 或者，也可以按 **F5**。

   系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。 完成後，瀏覽器會顯示執行中的應用程式。

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

1. 在檢閱執行中的 Web 應用程式之後，請關閉瀏覽器，然後選取 Visual Studio 工具列中的「停止偵錯」圖示以停止應用程式。

## <a name="create-a-web-app-in-the-azure-portal"></a>在 Azure 入口網站中建立 Web 應用程式

下列步驟會在 Azure 入口網站中建立 Web 應用程式：

1. 登入[Azure 入口網站](https://portal.azure.com)。

1. 選取入口網站介面左上角的 [新增]****。

1. 選取 [ **web +** 行動] [  >  **web 應用程式**]。

   ![Microsoft Azure 入口網站：新按鈕：Marketplace 下方的 [Web + 行動]：[精選 App] 下方的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. 在 [Web 應用程式]**** 刀鋒視窗中，為 [App Service 名稱]**** 輸入唯一值。

   ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > [App Service 名稱]**** 必須是唯一的名稱。 提供名稱時，入口網站會強制執行這項規則。 如果輸入不同的值，請將本教學課程中的每個 **SampleWebAppDemo** 取代為該值。

   此外，請在 [Web 應用程式]**** 刀鋒視窗中，選取現有的 [App Service 方案/位置]**** 或另外新建一個。 如果要建立新的方案，請選取定價層、位置和其他選項。 如需 App Service 方案的詳細資訊，請參閱 [Azure App Service 方案深入概觀](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。

1. 選取 [建立]****。 Azure 將會開始佈建並啟動 Web 應用程式。

   ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>啟用新 Web 應用程式的 Git 發佈

Git 是一種分散式版本控制系統，可用來部署 Azure App Service Web 應用程式。 Web 應用程式程式碼會儲存在本機 Git 存放庫中，而系統會透過將該程式碼推送至遠端存放庫來將它部署至 Azure。

1. 登入[Azure 入口網站](https://portal.azure.com)。

1. 選取 [應用程式服務]**** 以檢視與 Azure 訂用帳戶相關聯的應用程式服務清單。

1. 選取本教學課程上一節所建立的 Web 應用程式。

1. 在 [開發]**** 刀鋒視窗中，選取 [部署選項]**** > [選擇來源]**** > [本機 Git 存放庫]****。

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/deployment-options.png)

1. 選取 [確定]****。

1. 如果尚未設定發行 Web 應用程式或其他 App Service 應用程式所需的部署認證，請立即設定：

   * 選取 [**設定**] [  >  **部署認證**]。 [設定部署認證]**** 刀鋒視窗隨即顯示。
   * 建立使用者名稱和密碼。 儲存該密碼，以於日後設定 Git 時使用。
   * 選取 [儲存]****。

1. 在 [Web 應用程式]**** 刀鋒視窗中，選取 [設定]**** > [屬性]****。 [GIT URL]**** 下方會顯示作為部署目的地遠端 Git 存放庫的 URL。

1. 複製 [GIT URL] **** 值以供教學課程稍後使用。

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>將 Web 應用程式發佈至 Azure App Service

在本節中，使用 Visual Studio 建立本機 Git 存放庫，並將 Web 應用程式從該存放庫推送到 Azure 以進行部署。 其中包括下列步驟：

* 使用 [GIT URL] 值來新增遠端存放庫設定，以便本機存放庫可以被部署至 Azure。
* 認可專案的變更。
* 將專案變更從本機存放庫推送到 Azure 上的遠端存放庫。

1. 在方案總管**** 中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']****，然後選取 [認可]****。 **Team Explorer** 隨即顯示。

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

1. 在 **Team Explorer** 中，選取 [首頁]**** (首頁圖示) > [設定]**** > [存放庫設定]****。

1. 在 [存放庫設定]**** 的 [遠端]**** 區段中，選取 [新增]****。 [新增遠端]**** 對話方塊隨即顯示。

1. 將遠端的 [名稱]**** 設為 **Azure-SampleApp**。

1. 將 [擷取]**** 的值設為本教學課程稍早從 Azure 所複製的 [Git URL]****。 請注意，這應該是結尾為 **.git** 的 URL。

   ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > 或者，透過開啟 [命令視窗]****，變更為專案目錄，然後輸入命令，來從 [命令視窗]**** 指定遠端存放庫。 範例：
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. 選取 [首頁]**** (首頁圖示) > [設定]**** > [全域設定]****。 確認名稱和電子郵件地址皆已設定。 必要時，請選取 [更新]****。

1. 選取 [**首頁**  >  **變更**] 以返回 [**變更**] 視圖。

1. 輸入認可訊息，例如 **Initial Push #1**，然後選取 [認可]****。 此動作會在本機建立一項*認可*作業。

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > 或者，透過開啟 [命令視窗]****，變更為專案目錄，然後輸入 git 命令，來從 [命令視窗]**** 認可變更。 範例：
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. 選取 [**首頁**  >  **同步**  >  **動作**] [  >  **開啟命令提示**字元]。 命令提示字元會開啟並切換至專案資料夾。

1. 在命令視窗中輸入下列命令：

   `git push -u Azure-SampleApp master`

1. 輸入稍早在 Azure 中建立的 Azure [部署認證]**** 密碼。

   此命令會開始進行將本機專案檔推送到 Azure 的程序。 上述命令的輸出結尾會顯示部署成功的訊息。

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > 如果需在專案上進行共同作業，請考慮在推送到 Azure 之前，先推送到 [GitHub](https://github.com) \(英文\)。
 
### <a name="verify-the-active-deployment"></a>驗證作用中的部署

確認 Web 應用程式已成功從本機環境傳送至 Azure。

在 [Azure 入口網站](https://portal.azure.com)中，選取該 Web 應用程式。 選取 [**部署**] [部署  >  **選項**]。

![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>在 Azure 中執行應用程式

在 Web 應用程式已部署至 Azure 之後，請執行該應用程式。

這可由下列兩種方法來完成：

* 在 Azure 入口網站中，找出該 Web 應用程式的 Web 應用程式刀鋒視窗。 選取 [瀏覽]**** 以在預設瀏覽器中檢視該應用程式。
* 開啟瀏覽器，並輸入 Web 應用程式的 URL。 範例： `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>更新 Web 應用程式，並重新發行

在針對本機程式碼做出變更後，請重新發行：

1. 在 Visual Studio 的方案總管**** 中，開啟 *Startup.cs* 檔案。

1. 在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. 儲存對 *Startup.cs* 所做的變更。

1. 在**方案總管**中，以滑鼠右鍵按一下 [**方案 ' SampleWebAppDemo '** ]，然後選取 [**認可**]。 **Team Explorer** 隨即顯示。

1. 輸入認可訊息，例如 `Update #2`。

1. 按 [認可]**** 按鈕，認可專案中的變更。

1. 選取 [**首頁**  >  **同步**  >  **動作**  >  **推**播]。

> [!NOTE]
> 或者，透過開啟 [命令視窗]****，變更為專案目錄，然後輸入 git 命令，來從 [命令視窗]**** 推送變更。 範例：
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>在 Azure 中檢視更新的 Web 應用程式

從 Azure 入口網站的 Web 應用程式刀鋒視窗選取 [瀏覽]****，或是開啟瀏覽器並輸入 Web 應用程式的 URL，來檢視更新之後的 Web 應用程式。 範例： `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>其他資源

* [使用 Azure Pipelines 建立您的第一個管線](/azure/devops/pipelines/get-started-yaml)
* [專案 Kudu](https://github.com/projectkudu/kudu/wiki)
* <xref:host-and-deploy/visual-studio-publish-profiles>
