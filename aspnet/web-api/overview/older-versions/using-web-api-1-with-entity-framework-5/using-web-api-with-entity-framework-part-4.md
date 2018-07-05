---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 第 4 部分： 新增管理員檢視 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 599f684ba200821d7fcd33819937c5a5b5a29ce8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371047"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="f6a10-102">第 4 部分： 新增管理員檢視</span><span class="sxs-lookup"><span data-stu-id="f6a10-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="f6a10-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f6a10-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f6a10-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="f6a10-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="f6a10-105">新增系統管理員檢視</span><span class="sxs-lookup"><span data-stu-id="f6a10-105">Add an Admin View</span></span>

<span data-ttu-id="f6a10-106">現在我們會向用戶端，並加入可從系統管理控制站使用資料的頁面。</span><span class="sxs-lookup"><span data-stu-id="f6a10-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="f6a10-107">頁面可讓使用者建立、 編輯或刪除產品，將 AJAX 要求傳送到控制器。</span><span class="sxs-lookup"><span data-stu-id="f6a10-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="f6a10-108">在方案總管 中，展開 Controllers 資料夾，然後開啟名為 HomeController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6a10-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="f6a10-109">此檔案包含此 MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="f6a10-109">This file contains an MVC controller.</span></span> <span data-ttu-id="f6a10-110">新增名為`Admin`:</span><span class="sxs-lookup"><span data-stu-id="f6a10-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="f6a10-111">**HttpRouteUrl**方法會建立 web API 的 URI，以及我們將檢視包供稍後使用。</span><span class="sxs-lookup"><span data-stu-id="f6a10-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="f6a10-112">接下來，將文字游標放在`Admin`動作方法，然後以滑鼠右鍵按一下並選取**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="f6a10-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="f6a10-113">此時會出現**加入檢視**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f6a10-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="f6a10-114">在 **加入檢視**對話方塊中，命名為"Admin"的檢視。</span><span class="sxs-lookup"><span data-stu-id="f6a10-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="f6a10-115">選取核取方塊**建立強型別檢視**。</span><span class="sxs-lookup"><span data-stu-id="f6a10-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="f6a10-116">底下**模型類別**，選取 [產品 (ProductStore.Models)]。</span><span class="sxs-lookup"><span data-stu-id="f6a10-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="f6a10-117">所有其他選項保留為其預設值。</span><span class="sxs-lookup"><span data-stu-id="f6a10-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="f6a10-118">按一下 **新增**新增名為 Admin.cshtml Views/Home 下的檔案。</span><span class="sxs-lookup"><span data-stu-id="f6a10-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="f6a10-119">開啟這個檔案，並新增下列 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6a10-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="f6a10-120">此 HTML 會定義 頁面上的結構，但尚未有線設定任何功能。</span><span class="sxs-lookup"><span data-stu-id="f6a10-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="f6a10-121">建立系統管理員 頁面的連結</span><span class="sxs-lookup"><span data-stu-id="f6a10-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="f6a10-122">在 [方案總管] 中，展開 [Views] 資料夾，然後展開 Shared 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6a10-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="f6a10-123">開啟檔案，名為\_Layout.cshtml。</span><span class="sxs-lookup"><span data-stu-id="f6a10-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="f6a10-124">找出**ul**項目識別碼 = [功能表] 和 [系統管理] 檢視的動作連結：</span><span class="sxs-lookup"><span data-stu-id="f6a10-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="f6a10-125">在範例專案中，我可以做幾個其他外觀變更，例如取代 「 您標誌的位置 」 的字串。</span><span class="sxs-lookup"><span data-stu-id="f6a10-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="f6a10-126">這些不會影響應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="f6a10-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="f6a10-127">您可以下載專案，並比較檔案。</span><span class="sxs-lookup"><span data-stu-id="f6a10-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="f6a10-128">執行應用程式，然後按一下出現在 [首頁] 頁面頂端的 「 管理員 」 連結。</span><span class="sxs-lookup"><span data-stu-id="f6a10-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="f6a10-129">系統管理員 頁面看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="f6a10-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="f6a10-130">權限現在頁面不會執行任何項目。</span><span class="sxs-lookup"><span data-stu-id="f6a10-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="f6a10-131">在下一步 區段中，我們將使用 Knockout.js 建立動態 UI。</span><span class="sxs-lookup"><span data-stu-id="f6a10-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="f6a10-132">新增授權</span><span class="sxs-lookup"><span data-stu-id="f6a10-132">Add Authorization</span></span>

<span data-ttu-id="f6a10-133">目前瀏覽網站的任何人都能存取系統管理員 頁面。</span><span class="sxs-lookup"><span data-stu-id="f6a10-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="f6a10-134">讓我們變更此選項可限制系統管理員的權限。</span><span class="sxs-lookup"><span data-stu-id="f6a10-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="f6a10-135">啟動新增"Administrator"角色和系統管理員使用者。</span><span class="sxs-lookup"><span data-stu-id="f6a10-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="f6a10-136">在方案總管 中，展開 篩選器 資料夾，然後開啟名為 InitializeSimpleMembershipAttribute.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6a10-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="f6a10-137">找出`SimpleMembershipInitializer`建構函式。</span><span class="sxs-lookup"><span data-stu-id="f6a10-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="f6a10-138">若要在呼叫之後**WebSecurity.InitializeDatabaseConnection**，新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f6a10-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="f6a10-139">這是應急的方法來新增 「 系統管理員 」 角色，並建立使用者角色。</span><span class="sxs-lookup"><span data-stu-id="f6a10-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="f6a10-140">在方案總管 中，展開 Controllers 資料夾，然後開啟 HomeController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6a10-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="f6a10-141">新增**Authorize**屬性設定為`Admin`方法。</span><span class="sxs-lookup"><span data-stu-id="f6a10-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="f6a10-142">開啟 AdminController.cs 檔案並新增**Authorize**屬性設定為整個`AdminController`類別。</span><span class="sxs-lookup"><span data-stu-id="f6a10-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="f6a10-143">MVC 和 Web API，同時定義**授權**不同命名空間中的屬性。</span><span class="sxs-lookup"><span data-stu-id="f6a10-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="f6a10-144">MVC 會使用**System.Web.Mvc.AuthorizeAttribute**，而 Web API 會使用**System.Web.Http.AuthorizeAttribute**。</span><span class="sxs-lookup"><span data-stu-id="f6a10-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="f6a10-145">現在只有系統管理員可以檢視 [系統管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f6a10-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="f6a10-146">此外，如果您將 HTTP 要求傳送至系統管理控制站時，要求必須包含驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="f6a10-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="f6a10-147">如果沒有，則伺服器會傳送 HTTP 401 （未經授權） 回應。</span><span class="sxs-lookup"><span data-stu-id="f6a10-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="f6a10-148">您可以先將這在 Fiddler 中看到，傳送 GET 要求來`http://localhost:*port*/api/admin`。</span><span class="sxs-lookup"><span data-stu-id="f6a10-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6a10-149">[上一頁](using-web-api-with-entity-framework-part-3.md)
> [下一頁](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="f6a10-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
