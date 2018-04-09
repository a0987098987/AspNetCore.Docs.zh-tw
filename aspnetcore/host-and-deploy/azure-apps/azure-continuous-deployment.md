---
title: 使用 Visual Studio 在 Azure 和 ASP.NET Core 使用 Git 連續部署
author: rick-anderson
description: 了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a>使用 Visual Studio 在 Azure 和 ASP.NET Core 使用 Git 連續部署

作者：[Erik Reitan](https://github.com/Erikre)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

本教學課程會示範如何建立 ASP.NET Core web 應用程式使用 Visual Studio，並將它從 Visual Studio 部署至 Azure App Service 使用連續部署。

另請參閱 [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic) (使用 VSTS 來建立 Azure Web 應用程式，並透過連續部署來發行)，其中說明如何使用 Visual Studio Team Services 來設定 [Azure App Service](/azure/app-service/app-service-web-overview) 的持續傳遞 (CD) 工作流程。 在 Team Services 中的 azure 持續傳遞可簡化強固的部署管線設定可發行的 Azure App Service 中裝載的應用程式的更新。 從 Azure 入口網站中建置、 執行測試、 將部署到預備位置，然後再部署到生產環境，可以設定管線。

> [!NOTE]
> 若要完成本教學課程中，Microsoft Azure 帳戶。 若要取得帳戶，[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F)或[申請免費的試用版](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>必要條件

本教學課程會假設已安裝下列軟體：

* [Visual Studio](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://git-scm.com/downloads) (適用於 Windows)

## <a name="create-an-aspnet-core-web-app"></a>建立 ASP.NET Core Web 應用程式

1. 啟動 Visual Studio。

1. 從 [檔案] 功能表選取 [新增] > [專案]。

1. 選取 [ASP.NET Core Web 應用程式] 專案範本。 它會出現在 [已安裝] > [範本] > [Visual C#] > [.NET Core] 下。 將專案命名為 `SampleWebAppDemo`。 選取**建立新的 Git 儲存機制**選項，然後按一下**確定**。

   ![[新增專案] 對話](azure-continuous-deployment/_static/01-new-project.png)

1. 在 [新增 ASP.NET Core 專案] 對話方塊中，選取 ASP.NET Core 的 [空白] 範本，然後按一下 [確定]。

   ![[新增 ASP.NET 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> .NET Core 的最新版本為 2.0。

### <a name="running-the-web-app-locally"></a>在本機執行 Web 應用程式

1. 一旦 Visual Studio 完成建立應用程式之後，您即可選取 [偵錯] > [開始偵錯] 以執行應用程式。 或者，您可以按下**F5**。

   系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。 一旦完成後，瀏覽器會顯示執行中應用程式。

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

1. 檢閱後執行的 Web 應用程式，請關閉瀏覽器，並選取 [停止偵錯] 圖示以停止應用程式的 Visual Studio 的工具列中。

## <a name="create-a-web-app-in-the-azure-portal"></a>在 Azure 入口網站中建立 Web 應用程式

下列步驟會在 Azure 入口網站中建立 web 應用程式：

1. 登入[Azure 入口網站](https://portal.azure.com)。

1. 選取**新增**在入口網站介面的左上角。

1. 選取**Web + 行動** > **Web 應用程式**。

   ![Microsoft Azure 入口網站：新按鈕：Marketplace 下方的 [Web + 行動]：[精選 App] 下方的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. 在 [Web 應用程式] 刀鋒視窗中，為 [App Service 名稱] 輸入唯一值。

   ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > **應用程式服務名稱**必須是唯一的名稱。 提供名稱時，入口網站會強制執行這項規則。 如果提供不同的值，以取代該值，每次發生**SampleWebAppDemo**在本教學課程。

   此外，請在 [Web 應用程式] 刀鋒視窗中，選取現有的 [App Service 方案/位置] 或另外新建一個。 如果建立新的計畫，請選取定價層、 位置和其他選項。 如需有關應用程式服務方案的詳細資訊，請參閱[Azure App Service 方案深入概觀](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。

1. 選取 [建立]。 Azure 會佈建並啟動 web 應用程式。

   ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>啟用新 Web 應用程式的 Git 發行功能

Git 是分散式的版本控制系統可以用來部署 Azure App Service web 應用程式。 Web 應用程式程式碼會儲存在本機 Git 儲存機制並推送至遠端儲存機制的程式碼部署至 Azure。

1. 登入[Azure 入口網站](https://portal.azure.com)。

1. 選取**應用程式服務**，檢視 Azure 訂用帳戶相關聯的應用程式服務的清單。

1. 選取在本教學課程的上一節中建立的 web 應用程式。

1. 在 [開發] 刀鋒視窗中，選取 [部署選項] > [選擇來源] > [本機 Git 存放庫]。

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/deployment-options.png)

1. 選取 [確定]。

1. 如果發佈的 web 應用程式或其他 App Service 應用程式的部署認證您尚未先前已設定，立即設定：

   * 選取**設定** > **部署認證**。 **設定部署認證**刀鋒視窗會顯示。
   * 建立使用者名稱和密碼。 設定 Git 時，儲存供稍後使用的密碼。
   * 選取 [儲存]。

1. 在**Web 應用程式**刀鋒視窗中，選取**設定** > **屬性**。 將部署至遠端 Git 儲存機制的 URL 會顯示在下方**GIT URL**。

1. 複製 **GIT URL** 值，以便於教學課程稍後使用。

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>發行至 Azure App Service web 應用程式

在本節中，建立使用 Visual Studio 和推播從該儲存機制至 Azure 部署 web 應用程式的本機 Git 儲存機制。 其中包括下列步驟：

* 新增使用 GIT URL 值，因此本機儲存機制可部署至 Azure 的遠端儲存機制設定。
* 認可變更專案。
* 在 Azure 上至遠端儲存機制將專案變更從本機儲存機制推送。

1. 在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。 **Team Explorer**隨即出現。

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

1. 在 **Team Explorer** 中，選取 [首頁] (首頁圖示) > [設定] > [存放庫設定]。

1. 在**遙控器**區段**儲存機制設定**，選取**新增**。 **加入遠端**對話方塊隨即出現。

1. 將遠端的 [名稱] 設為 **Azure-SampleApp**。

1. 設定的值**提取**至**Git URL** ，從 Azure 複製稍早在本教學課程。 請注意，這應該是結尾為 **.git** 的 URL。

   ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > 或者，指定遠端儲存機制從**命令視窗**開啟**命令視窗**，變更至專案目錄中，輸入命令。 範例：
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. 選取 [首頁] (首頁圖示) > [設定] > [全域設定]。 確認已設定的名稱和電子郵件地址。 選取**更新**必要。

1. 選取 [首頁] > [變更]，即可返回 [變更] 檢視。

1. 輸入 「 認可 」 訊息，例如**初始推送 #1**選取**認可**。 這個動作會建立*認可*在本機。

   ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > 或者，變更從認可**命令視窗**開啟**命令視窗**、 專案目錄中，變更，以及輸入 git 命令。 範例：
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. 選取 [首頁] > [同步] > [動作] > [開啟命令提示字元]。 命令提示字元會開啟至專案目錄中。

1. 在命令視窗中輸入下列命令：

   `git push -u Azure-SampleApp master`

1. 輸入 Azure**部署認證**稍早在 Azure 中建立的密碼。

   此命令會啟動本機專案檔案推送至 Azure 的程序。 上述命令的輸出，結束部署成功的訊息。

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > 如果需要在專案上的共同作業，請考慮將推送到[GitHub](https://github.com)之前推入至 Azure。
 
### <a name="verify-the-active-deployment"></a>驗證作用中的部署

請確認從本機環境到 Azure 的 web 應用程式傳送成功。

在[Azure 入口網站](https://portal.azure.com)、 選取 web 應用程式。 選取**部署** > **部署選項**。

![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>在 Azure 中執行應用程式

現在 web 應用程式部署至 Azure 時，執行應用程式。

即可達成這兩種方式：

* 在 Azure 入口網站中，找出 web 應用程式 [web 應用程式] 刀鋒視窗。 選取**瀏覽**在預設瀏覽器中檢視應用程式。
* 開啟瀏覽器並輸入 web 應用程式的 URL。 範例：`http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>更新 web 應用程式，並重新發行

變更本機的程式碼後，重新發佈：

1. 在 Visual Studio 的方案總管中，開啟 *Startup.cs* 檔案。

1. 在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. 變更儲存到*Startup.cs*。

1. 在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。 **Team Explorer**隨即出現。

1. 輸入 「 認可 」 訊息，例如`Update #2`。

1. 按 [認可] 按鈕，認可專案中的變更。

1. 選取 [首頁] > [同步] > [動作] > [推送]。

> [!NOTE]
> 或者，從將變更內容推送**命令視窗**開啟**命令視窗**、 專案目錄中，變更，以及輸入 git 命令。 範例：
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>在 Azure 中檢視更新的 Web 應用程式

檢視更新的 web 應用程式選取**瀏覽**從 web 應用程式 刀鋒視窗在 Azure 入口網站或開啟瀏覽器，然後輸入 web 應用程式的 URL。 範例：`http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>其他資源

* [使用 VSTS 來建立及發行至 Azure Web 應用程式使用的連續部署](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [專案 Kudu](https://github.com/projectkudu/kudu/wiki)
