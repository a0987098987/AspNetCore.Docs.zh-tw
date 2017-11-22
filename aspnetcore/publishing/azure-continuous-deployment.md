---
title: "使用 Visual Studio 與 Git 持續部署到 Azure"
author: rick-anderson
description: "了解如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過 Git 持續部署將它部署到 Azure App Service。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: f7ea2e76fdee19a3d964e42053f0060a0a505e5b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>使用 Visual Studio 與 Git 持續部署到適用於 ASP.NET Core 的 Azure

作者：[Erik Reitan](https://github.com/Erikre)

本教學課程說明如何使用 Visual Studio 建立 ASP.NET Core Web 應用程式，並透過連續部署將它從 Visual Studio 部署到 Azure App Service。 

另請參閱 [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic) (使用 VSTS 來建立 Azure Web 應用程式，並透過連續部署來發行)，其中說明如何使用 Visual Studio Team Services 來設定 [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) 的持續傳遞 (CD) 工作流程。 Team Services 中的 Azure 持續傳遞功能，可簡化將應用程式更新發行到 Azure App Service 的部署管道設定。 您可以透過 Azure 入口網站設定這個管道，以建置、執行測試、部署到預備位置，然後再部署到生產環境。

> [!NOTE]
> 若要完成本教學課程，您需要 Microsoft Azure 帳戶。 如果您沒有帳戶，可以[啟動 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)或[註冊免費試用帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>必要條件

本教學課程假設您已安裝下列項目：

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://www.microsoft.com/net/download/core) (執行階段和工具)

* [Git](https://git-scm.com/downloads) (適用於 Windows)

## <a name="create-an-aspnet-core-web-app"></a>建立 ASP.NET Core Web 應用程式

1. 啟動 Visual Studio。

2. 從 [檔案] 功能表選取 [新增] > [專案]。

3. 選取 [ASP.NET Core Web 應用程式] 專案範本。 它會出現在 [已安裝] > [範本] > [Visual C#] > [.NET Core] 下。 將專案命名為 `SampleWebAppDemo`。 選取 [建立新的 Git 存放庫] 選項，然後按一下 [確定]。

   ![[新增專案] 對話](azure-continuous-deployment/_static/01-new-project.png)

4. 在 [新增 ASP.NET Core 專案] 對話方塊中，選取 ASP.NET Core 的 [空白] 範本，然後按一下 [確定]。

   ![[新增 ASP.NET 專案] 對話方塊](azure-continuous-deployment/_static/02-web-site-template.png)

>[!NOTE]
    >.NET Core 的最新版本為 2.0

### <a name="running-the-web-app-locally"></a>在本機執行 Web 應用程式

1. 一旦 Visual Studio 完成建立應用程式之後，您即可選取 [偵錯] -> [開始偵錯] 以執行應用程式。 或者，也可以按 **F5**。

   系統可能需要一點時間來初始化 Visual Studio 和新的應用程式。 完成後，瀏覽器即會顯示執行中的應用程式。

   ![顯示執行中應用程式的瀏覽器視窗，該應用程式顯示 'Hello World!'](azure-continuous-deployment/_static/04-browser-runapp.png)

2. 在您檢閱執行中的 Web 應用程式之後，請關閉瀏覽器，然後按一下 Visual Studio 工具列中的「停止偵錯」圖示以停止應用程式。

## <a name="create-a-web-app-in-the-azure-portal"></a>在 Azure 入口網站中建立 Web 應用程式

下列步驟將引導您在 Azure 入口網站中建立 Web 應用程式。

1. 登入 [Azure 入口網站](https://portal.azure.com)
2. 點選入口網站左上角的 [新增]
3. 點選 [Web + 行動] > [Web 應用程式]

    ![Microsoft Azure 入口網站：新按鈕：Marketplace 下方的 [Web + 行動]：[精選 App] 下方的 [Web 應用程式] 按鈕](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  在 [Web 應用程式] 刀鋒視窗中，為 [App Service 名稱] 輸入唯一值。

    ![[Web 應用程式] 刀鋒視窗](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >[App Service 名稱] 必須是唯一的名稱。 當您嘗試輸入名稱時，入口網站會強制執行這項規則。 輸入不同的值之後，您必須將本教學課程中看到的每個 **SampleWebAppDemo** 取代為該值。

    &nbsp;
    
    此外，請在 [Web 應用程式] 刀鋒視窗中，選取現有的 [App Service 方案/位置] 或另外新建一個。 如果您建立新的方案，請選取定價層、位置和其他選項。 如需 App Service 方案的詳細資訊，請參閱 [Azure App Service 方案深入概觀](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/)。

5.  按一下 [建立] 。 Azure 即會佈建並啟動您的 Web 應用程式。

    ![Azure 入口網站：範例 Web 應用程式示範 01 [基本資訊] 刀鋒視窗](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>啟用新 Web 應用程式的 Git 發行功能

Git 是一種分散式版本控制系統，可用來部署您的 Azure App Service Web 應用程式。 您可將針對 Web 應用程式所撰寫的程式碼儲存在本機 Git 存放庫，並將這些程式碼推送至遠端存放庫以部署到 Azure。

1. 登入 [Azure 入口網站](https://portal.azure.com) (如果您尚未登入)。

2. 按一下 [應用程式服務]，即可檢視與 Azure 訂用帳戶相關聯的應用程式服務清單。

3. 選取您在本教學課程上一節所建立的 Web 應用程式。

4. 在 [開發] 刀鋒視窗中，選取 [部署選項] > [選擇來源] > [本機 Git 存放庫]。

   ![[設定] 刀鋒視窗：[部署來源] 刀鋒視窗：[選擇來源] 刀鋒視窗](azure-continuous-deployment/_static/deployment-options.png)

5. 按一下 [確定]。

6. 如果您先前尚未設定用來發行 Web 應用程式或其他 App Service 應用程式的部署認證，請立即設定：

   * 按一下 [設定] > [部署認證]。 [設定部署認證] 刀鋒視窗隨即顯示。

   * 建立使用者名稱和密碼。  稍後設定 Git 時需要此密碼。

   * 按一下 [儲存] 。

7. 在 [Web 應用程式] 刀鋒視窗中，按一下 [設定] > [屬性]。 [GIT URL] 下方顯示的遠端 Git 存放庫 URL，即為您的部署目的地。

8. 複製 **GIT URL** 值，以便於教學課程稍後使用。

   ![Azure 入口網站：應用程式的 [屬性] 刀鋒視窗](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>將 Web 應用程式發行至 Azure App Service

在本節中，您會使用 Visual Studio 建立本機 Git 存放庫，並將 Web 應用程式從該存放庫推送到 Azure 以進行部署。 其中包括下列步驟：

   * 使用 GIT URL 值來新增遠端存放庫設定，以將本機存放庫部署至 Azure。

   * 認可專案的變更。

   * 將專案變更從本機存放庫推送到 Azure 上的遠端存放庫。

&nbsp;
   
1.  在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。 **Team Explorer** 隨即顯示。

    ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/10-team-explorer.png)

2.  在 **Team Explorer** 中，選取 [首頁] (首頁圖示) > [設定] > [存放庫設定]。

3.  在 [存放庫設定] 的 [遠端] 區段中，選取 [新增]。 隨即顯示 [新增遠端] 對話方塊。

4.  將遠端的 [名稱] 設為 **Azure-SampleApp**。

5.  將 [擷取] 的值設為您在本教學課程稍早時從 Azure 複製的 **Git URL**。 請注意，這應該是結尾為 **.git** 的 URL。

    ![[編輯遠端] 對話方塊](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入命令，以透過**命令視窗**指定遠端存放庫。 例如：`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  選取 [首頁] (首頁圖示) > [設定] > [全域設定]。 確認您已設定好名稱和電子郵件地址。 您可能也需要選取 [更新]。

7.  選取 [首頁] > [變更]，即可返回 [變更] 檢視。

8.  輸入認可訊息，例如 **Initial Push #1**，然後按一下 [認可]。 此動作會在本機建立一項「認可」作業。 接下來，您必須與 Azure 進行同步。

    ![Team Explorer 的 [連線] 索引標籤](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入 git 命令，以透過**命令視窗**認可變更。 例如: 
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  選取 [首頁] > [同步] > [動作] > [開啟命令提示字元]。 命令提示字元即會開啟您的專案目錄。

10.  在命令視窗中輸入下列命令：

    `git push -u Azure-SampleApp master`

11.  輸入您稍早在 Azure 中建立的 Azure **部署認證**密碼。

    >[!NOTE]
    >輸入密碼時，系統不會將密碼顯示出來。

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
    > 如果您需要在專案上共同作業，應該考慮先將其推送到 [GitHub](https://github.com)，之後再推送至 Azure。
 
### <a name="verify-the-active-deployment"></a>驗證作用中的部署

您可以驗證是否已將 Web 應用程式從本機環境成功傳送至 Azure。 您可以查看提列的成功部署。

1. 在 [Azure 入口網站](https://portal.azure.com)中，選取您的 Web 應用程式。 然後，選取 [部署] > [部署選項]。

   ![Azure 入口網站：[設定] 刀鋒視窗：顯示成功部署的 [部署] 刀鋒視窗](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>在 Azure 中執行應用程式

現在，您已將 Web 應用程式部署至 Azure，即可執行應用程式。

有兩種方法可以達到這個目的：

* 在 Azure 入口網站中，找出您 Web 應用程式的 Web 應用程式刀鋒視窗，並按一下 [瀏覽]，即可在預設瀏覽器中檢視您的應用程式。

* 開啟瀏覽器，並輸入您的 Web 應用程式 URL。 例如: 

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>更新 Web 應用程式，並重新發行

您可以在變更本機程式碼之後，重新發行。

1.  在 Visual Studio 的方案總管中，開啟 *Startup.cs* 檔案。

2.  在 `Configure` 方法中，修改 `Response.WriteAsync` 方法，使其如下所示：

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  將變更儲存到 *Startup.cs*。

4.  在方案總管中，以滑鼠右鍵按一下 [方案 'SampleWebAppDemo']，然後選取 [認可]。 **Team Explorer** 隨即顯示。

5.  輸入認可訊息，例如：

    ```none
    Update #2
    ```

6.  按 [認可] 按鈕，認可專案中的變更。

7.  選取 [首頁] > [同步] > [動作] > [推送]。

>[!NOTE]
>或者，您可以開啟**命令視窗**、變更為您的專案目錄，並輸入 git 命令，以透過**命令視窗**推送變更。 例如: 
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>在 Azure 中檢視更新的 Web 應用程式

您可以從 Azure 入口網站的 Web 應用程式刀鋒視窗選取 [瀏覽]，或開啟瀏覽器，然後輸入 Web 應用程式的 URL，以檢視更新的 Web 應用程式。 例如: 

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>其他資源

* [發行和部署](index.md)

* [專案 Kudu](https://github.com/projectkudu/kudu/wiki)
