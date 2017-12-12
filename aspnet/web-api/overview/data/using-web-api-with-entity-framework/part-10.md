---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "將應用程式發行至 Azure 的 Azure App Service |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="e565b-102">將應用程式發行至 Azure 的 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e565b-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="e565b-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e565b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e565b-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="e565b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e565b-105">最後一個步驟中，您將會發行至 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e565b-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="e565b-106">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="e565b-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="e565b-107">按一下**發行**叫用**發行 Web**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="e565b-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="e565b-108">如果您核取**雲端中的主機**當您第一次建立專案，然後連線並已設定。</span><span class="sxs-lookup"><span data-stu-id="e565b-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="e565b-109">在此情況下，只要按一下**設定**索引標籤上，並檢查&quot;執行 Code First 移轉&quot;。</span><span class="sxs-lookup"><span data-stu-id="e565b-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="e565b-110">(如果您沒有核取**雲端中的主機**開頭，然後依照中的步驟[下一節](#new-website)。)</span><span class="sxs-lookup"><span data-stu-id="e565b-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="e565b-111">若要部署應用程式，按一下 **發行**。</span><span class="sxs-lookup"><span data-stu-id="e565b-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="e565b-112">您可以檢視中的發行進度**Web 發行活動**視窗。</span><span class="sxs-lookup"><span data-stu-id="e565b-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="e565b-113">(從**檢視**功能表上，選取**其他視窗**，然後選取**Web 發行活動**。)</span><span class="sxs-lookup"><span data-stu-id="e565b-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="e565b-114">當 Visual Studio 完成部署應用程式時，預設瀏覽器會自動開啟至已部署網站的 URL，並且您建立的應用程式現在正在執行雲端中。</span><span class="sxs-lookup"><span data-stu-id="e565b-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="e565b-115">在瀏覽器網址列中的 URL 會顯示從網際網路載入站台。</span><span class="sxs-lookup"><span data-stu-id="e565b-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="e565b-116">部署到新的網站</span><span class="sxs-lookup"><span data-stu-id="e565b-116">Deploying to a New Website</span></span>

<span data-ttu-id="e565b-117">如果您沒有勾選**雲端中的主機**當您第一次建立專案時，您可以現在設定新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e565b-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="e565b-118">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="e565b-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="e565b-119">選取**設定檔**索引標籤上，按一下  **Microsoft Azure 網站**。</span><span class="sxs-lookup"><span data-stu-id="e565b-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="e565b-120">如果您不目前登入至 Azure，系統會提示您登入。</span><span class="sxs-lookup"><span data-stu-id="e565b-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="e565b-121">在**現有網站**] 對話方塊中，按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="e565b-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="e565b-122">輸入網站名稱。</span><span class="sxs-lookup"><span data-stu-id="e565b-122">Enter a site name.</span></span> <span data-ttu-id="e565b-123">選取您的 Azure 訂閱和區域。</span><span class="sxs-lookup"><span data-stu-id="e565b-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="e565b-124">在下**資料庫伺服器**，選取**建立新的伺服器**，或選取現有的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e565b-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="e565b-125">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="e565b-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="e565b-126">按一下**設定**索引標籤上，並檢查&quot;執行 Code First 移轉&quot;。</span><span class="sxs-lookup"><span data-stu-id="e565b-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="e565b-127">然後按一下 **發行**。</span><span class="sxs-lookup"><span data-stu-id="e565b-127">Then click **Publish**.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e565b-128">上一步</span><span class="sxs-lookup"><span data-stu-id="e565b-128">Previous</span></span>](part-9.md)
