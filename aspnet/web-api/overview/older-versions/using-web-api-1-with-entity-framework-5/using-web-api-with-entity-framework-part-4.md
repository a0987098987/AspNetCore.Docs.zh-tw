---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 第 4 部分： 加入系統管理員檢視 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879526"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="714e2-102">第 4 部分： 加入系統管理員檢視</span><span class="sxs-lookup"><span data-stu-id="714e2-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="714e2-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="714e2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="714e2-104">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="714e2-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="714e2-105">新增系統管理員檢視</span><span class="sxs-lookup"><span data-stu-id="714e2-105">Add an Admin View</span></span>

<span data-ttu-id="714e2-106">現在我們將開啟用戶端，並加入頁面可以取用從系統管理控制站的資料。</span><span class="sxs-lookup"><span data-stu-id="714e2-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="714e2-107">頁面可讓使用者建立、 編輯或刪除產品 AJAX 要求傳送到控制器。</span><span class="sxs-lookup"><span data-stu-id="714e2-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="714e2-108">在方案總管 中，展開 Controllers 資料夾，然後開啟名為 HomeController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="714e2-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="714e2-109">此檔案包含此 MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="714e2-109">This file contains an MVC controller.</span></span> <span data-ttu-id="714e2-110">新增名為`Admin`:</span><span class="sxs-lookup"><span data-stu-id="714e2-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="714e2-111">**HttpRouteUrl**方法會建立對 web API，URI 和我們存放此檢視的屬性包中的更新版本。</span><span class="sxs-lookup"><span data-stu-id="714e2-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="714e2-112">接下來，將文字資料指標內`Admin`動作方法，然後以滑鼠右鍵按一下，然後選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="714e2-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="714e2-113">這會顯示**加入檢視**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="714e2-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="714e2-114">在**加入檢視**對話方塊中，檢視 「 系統管理員 」。</span><span class="sxs-lookup"><span data-stu-id="714e2-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="714e2-115">選取標示的核取方塊**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="714e2-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="714e2-116">在下**模型類別**，選取 「 產品 (ProductStore.Models) 」。</span><span class="sxs-lookup"><span data-stu-id="714e2-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="714e2-117">將其他所有選項都保留為其預設值。</span><span class="sxs-lookup"><span data-stu-id="714e2-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="714e2-118">按一下**新增**加入名為 Admin.cshtml Views/Home 下的檔案。</span><span class="sxs-lookup"><span data-stu-id="714e2-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="714e2-119">開啟此檔案並加入以下的 HTML。</span><span class="sxs-lookup"><span data-stu-id="714e2-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="714e2-120">此 HTML 會定義的頁面結構，但尚未有線設定任何功能。</span><span class="sxs-lookup"><span data-stu-id="714e2-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="714e2-121">建立管理頁面的連結</span><span class="sxs-lookup"><span data-stu-id="714e2-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="714e2-122">在方案總管 中，展開 Views 資料夾，然後展開 Shared 資料夾。</span><span class="sxs-lookup"><span data-stu-id="714e2-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="714e2-123">開啟的檔案命名\_Layout.cshtml。</span><span class="sxs-lookup"><span data-stu-id="714e2-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="714e2-124">找出**ul**項目 id =""，並在 系統管理 檢視的動作連結：</span><span class="sxs-lookup"><span data-stu-id="714e2-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="714e2-125">在範例專案中，我變更幾個其他外觀，例如字串 「 您的標誌在此"來取代。</span><span class="sxs-lookup"><span data-stu-id="714e2-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="714e2-126">這些不會影響應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="714e2-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="714e2-127">您可以下載專案，並比較檔案。</span><span class="sxs-lookup"><span data-stu-id="714e2-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="714e2-128">執行應用程式，然後按一下 [首頁] 的上方都會出現的"Admin"連結。</span><span class="sxs-lookup"><span data-stu-id="714e2-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="714e2-129">[管理] 頁面看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="714e2-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="714e2-130">權限，頁面不會執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="714e2-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="714e2-131">在下一步 區段中，我們會使用解 Knockout.js 建立動態的 UI。</span><span class="sxs-lookup"><span data-stu-id="714e2-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="714e2-132">新增授權</span><span class="sxs-lookup"><span data-stu-id="714e2-132">Add Authorization</span></span>

<span data-ttu-id="714e2-133">目前存取給任何人瀏覽的網站管理頁面。</span><span class="sxs-lookup"><span data-stu-id="714e2-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="714e2-134">讓我們來變更這項限制系統管理員的權限。</span><span class="sxs-lookup"><span data-stu-id="714e2-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="714e2-135">開始加入 「 系統管理員 」 角色和系統管理員使用者。</span><span class="sxs-lookup"><span data-stu-id="714e2-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="714e2-136">在方案總管 中，展開篩選器 資料夾，然後開啟名為 InitializeSimpleMembershipAttribute.cs 的檔案。</span><span class="sxs-lookup"><span data-stu-id="714e2-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="714e2-137">找出`SimpleMembershipInitializer`建構函式。</span><span class="sxs-lookup"><span data-stu-id="714e2-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="714e2-138">若要在呼叫之後**WebSecurity.InitializeDatabaseConnection**，加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="714e2-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="714e2-139">這是應急的方法，加入 「 系統管理員 」 角色並建立使用者角色。</span><span class="sxs-lookup"><span data-stu-id="714e2-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="714e2-140">在方案總管 中，展開 Controllers 資料夾，然後開啟 HomeController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="714e2-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="714e2-141">新增**授權**屬性`Admin`方法。</span><span class="sxs-lookup"><span data-stu-id="714e2-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="714e2-142">開啟 AdminController.cs 檔案並加入**授權**屬性為整個`AdminController`類別。</span><span class="sxs-lookup"><span data-stu-id="714e2-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="714e2-143">MVC 和 Web API 兩者都定義**授權**不同命名空間中的屬性。</span><span class="sxs-lookup"><span data-stu-id="714e2-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="714e2-144">MVC 會使用**System.Web.Mvc.AuthorizeAttribute**，而 Web API 所使用**System.Web.Http.AuthorizeAttribute**。</span><span class="sxs-lookup"><span data-stu-id="714e2-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="714e2-145">現在只有系統管理員可以檢視管理頁面。</span><span class="sxs-lookup"><span data-stu-id="714e2-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="714e2-146">此外，如果您的系統管理控制站來傳送 HTTP 要求，要求必須包含驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="714e2-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="714e2-147">如果沒有，則伺服器會傳送 HTTP 401 （未經授權） 回應。</span><span class="sxs-lookup"><span data-stu-id="714e2-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="714e2-148">您可以在看到 Fiddler 的 GET 要求傳送到`http://localhost:*port*/api/admin`。</span><span class="sxs-lookup"><span data-stu-id="714e2-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="714e2-149">[上一頁](using-web-api-with-entity-framework-part-3.md)
> [下一頁](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="714e2-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
