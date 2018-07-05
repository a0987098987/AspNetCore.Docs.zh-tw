---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: 將應用程式發佈至 Azure 的 Azure App Service |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 0290b392c1b292d0f3cc080dbfa25ec6103b2751
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400800"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="e34be-102">將應用程式發佈至 Azure 的 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e34be-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="e34be-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e34be-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e34be-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="e34be-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e34be-105">最後一個步驟中，您將應用程式發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="e34be-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="e34be-106">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="e34be-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="e34be-107">按一下 **發佈**叫用**發佈 Web**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="e34be-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="e34be-108">如果您勾選**雲端中的主機**當您第一次建立專案，然後連線並已設定。</span><span class="sxs-lookup"><span data-stu-id="e34be-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="e34be-109">在此情況下，只要按一下**設定**索引標籤，然後檢查&quot;Execute Code First Migrations&quot;。</span><span class="sxs-lookup"><span data-stu-id="e34be-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="e34be-110">(如果未核取**雲端中的主機**開頭，然後依照[下一節](#new-website)。)</span><span class="sxs-lookup"><span data-stu-id="e34be-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="e34be-111">若要部署應用程式，請按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="e34be-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="e34be-112">您可以檢視中的發佈進度**Web 發行活動**視窗。</span><span class="sxs-lookup"><span data-stu-id="e34be-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="e34be-113">(從**檢視**功能表上，選取**其他 Windows**，然後選取**Web 發行活動**。)</span><span class="sxs-lookup"><span data-stu-id="e34be-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="e34be-114">完成 Visual Studio 部署應用程式，預設瀏覽器會自動開啟至已部署的網站，URL 和您所建立的應用程式現在正在雲端中。</span><span class="sxs-lookup"><span data-stu-id="e34be-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="e34be-115">在瀏覽器網址列中的 URL 會顯示從網際網路載入站台。</span><span class="sxs-lookup"><span data-stu-id="e34be-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="e34be-116">部署至新的網站</span><span class="sxs-lookup"><span data-stu-id="e34be-116">Deploying to a New Website</span></span>

<span data-ttu-id="e34be-117">如果您沒有勾選**雲端中的主機**當您第一次建立專案時，您可以現在設定新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e34be-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="e34be-118">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="e34be-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="e34be-119">選取 **設定檔**索引標籤，然後按一下**Microsoft Azure 網站**。</span><span class="sxs-lookup"><span data-stu-id="e34be-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="e34be-120">如果您不目前登入至 Azure，系統會提示您登入。</span><span class="sxs-lookup"><span data-stu-id="e34be-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="e34be-121">在 [**現有的網站**] 對話方塊中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="e34be-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="e34be-122">輸入網站名稱。</span><span class="sxs-lookup"><span data-stu-id="e34be-122">Enter a site name.</span></span> <span data-ttu-id="e34be-123">選取您的 Azure 訂用帳戶和區域。</span><span class="sxs-lookup"><span data-stu-id="e34be-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="e34be-124">底下**資料庫伺服器**，選取**建立新的伺服器**，或選取現有的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e34be-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="e34be-125">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="e34be-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="e34be-126">按一下 **設定**索引標籤，然後檢查&quot;Execute Code First Migrations&quot;。</span><span class="sxs-lookup"><span data-stu-id="e34be-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="e34be-127">然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="e34be-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e34be-128">上一步</span><span class="sxs-lookup"><span data-stu-id="e34be-128">Previous</span></span>](part-9.md)
