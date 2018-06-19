---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 第 7 部分： 成員資格和授權 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 7 部分涵蓋的成員資格和授權。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879500"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="dcf3a-104">第 7 部分： 成員資格和授權</span><span class="sxs-lookup"><span data-stu-id="dcf3a-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="dcf3a-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="dcf3a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="dcf3a-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="dcf3a-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="dcf3a-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="dcf3a-109">7 部分涵蓋的成員資格和授權。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="dcf3a-110">目前瀏覽我們的網站的任何人存取我們的存放區管理員控制器。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="dcf3a-111">讓我們來變更這項限制站台系統管理員的權限。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="dcf3a-112">加入 AccountController 以及檢視</span><span class="sxs-lookup"><span data-stu-id="dcf3a-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="dcf3a-113">完整的 ASP.NET MVC 3 Web 應用程式範本與 ASP.NET MVC 3 空 Web 應用程式範本之間的差異是空白的範本不包含帳戶控制器。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="dcf3a-114">我們會將帳戶控制器新增完整的 ASP.NET MVC 3 Web 應用程式範本所建立的新 ASP.NET MVC 應用程式從複製一些檔案。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="dcf3a-115">建立新的 ASP.NET MVC 應用程式，使用完整的 ASP.NET MVC 3 Web 應用程式範本，並將下列檔案複製到我們的受測專案的相同目錄：</span><span class="sxs-lookup"><span data-stu-id="dcf3a-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="dcf3a-116">控制站目錄複製 AccountController.cs</span><span class="sxs-lookup"><span data-stu-id="dcf3a-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="dcf3a-117">模型目錄複製 AccountModels</span><span class="sxs-lookup"><span data-stu-id="dcf3a-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="dcf3a-118">建立帳戶目錄檢視目錄內，並複製所有的四個檢視中</span><span class="sxs-lookup"><span data-stu-id="dcf3a-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="dcf3a-119">變更控制器和模型類別的命名空間，讓它們以 MvcMusicStore 開頭。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="dcf3a-120">AccountController 類別應使用 MvcMusicStore.Controllers 命名空間，而且 AccountModels 類別應該使用 MvcMusicStore.Models 命名空間。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="dcf3a-121">*注意： 這些檔案也會提供在從中複製我們站台設計檔案在本教學課程開頭 MvcMusicStore Assets.zip 下載。成員資格檔案位於程式碼目錄中。*</span><span class="sxs-lookup"><span data-stu-id="dcf3a-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="dcf3a-122">更新的方案看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="dcf3a-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="dcf3a-123">新增與 ASP.NET 設定站台系統管理使用者</span><span class="sxs-lookup"><span data-stu-id="dcf3a-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="dcf3a-124">我們在我們的網站所需的授權之前，我們需要建立的使用者具有存取權。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="dcf3a-125">建立使用者的最簡單方式是使用內建 ASP.NET 組態網站。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="dcf3a-126">遵循 [方案總管] 中的圖示，即可啟動 ASP.NET 組態網站。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="dcf3a-127">這會啟動組態網站。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-127">This launches a configuration website.</span></span> <span data-ttu-id="dcf3a-128">按一下 [首頁] 螢幕上的 [安全性] 索引標籤，然後按一下 [啟用角色] 中的連結，在螢幕的中央。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="dcf3a-129">按一下 [建立或管理角色] 連結。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="dcf3a-130">輸入 「 系統管理員 」 做為角色名稱，然後按 [新增角色] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="dcf3a-131">按一下 [上一頁] 按鈕，然後按一下左邊的 [建立使用者] 連結。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="dcf3a-132">填入使用者資訊欄位左邊使用下列資訊：</span><span class="sxs-lookup"><span data-stu-id="dcf3a-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="dcf3a-133">**欄位**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-133">**Field**</span></span> | <span data-ttu-id="dcf3a-134">**值**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="dcf3a-135">**使用者名稱**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-135">**User Name**</span></span> | <span data-ttu-id="dcf3a-136">系統管理員</span><span class="sxs-lookup"><span data-stu-id="dcf3a-136">Administrator</span></span> |
| <span data-ttu-id="dcf3a-137">**密碼**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-137">**Password**</span></span> | <span data-ttu-id="dcf3a-138">password123 ！</span><span class="sxs-lookup"><span data-stu-id="dcf3a-138">password123!</span></span> |
| <span data-ttu-id="dcf3a-139">**確認密碼**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-139">**Confirm Password**</span></span> | <span data-ttu-id="dcf3a-140">password123 ！</span><span class="sxs-lookup"><span data-stu-id="dcf3a-140">password123!</span></span> |
| <span data-ttu-id="dcf3a-141">**電子郵件**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-141">**E-mail**</span></span> | <span data-ttu-id="dcf3a-142">（任何電子郵件地址將作用中）</span><span class="sxs-lookup"><span data-stu-id="dcf3a-142">(any email address will work)</span></span> |
| <span data-ttu-id="dcf3a-143">**安全性問題**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-143">**Security Question**</span></span> | <span data-ttu-id="dcf3a-144">（任意）</span><span class="sxs-lookup"><span data-stu-id="dcf3a-144">(whatever you like)</span></span> |
| <span data-ttu-id="dcf3a-145">**安全性解答**</span><span class="sxs-lookup"><span data-stu-id="dcf3a-145">**Security Answer**</span></span> | <span data-ttu-id="dcf3a-146">（任意）</span><span class="sxs-lookup"><span data-stu-id="dcf3a-146">(whatever you like)</span></span> |

<span data-ttu-id="dcf3a-147">*注意： 您當然可以使用任何您想要的密碼。預設的密碼安全性設定需要的 7 個字元且包含一個非英數字元密碼。*</span><span class="sxs-lookup"><span data-stu-id="dcf3a-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="dcf3a-148">選取此使用者，系統管理員角色，然後按一下 [建立使用者] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="dcf3a-149">此時，您應該會看到訊息，指出使用者已成功建立。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="dcf3a-150">您現在可以關閉瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="dcf3a-151">以角色為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="dcf3a-151">Role-based Authorization</span></span>

<span data-ttu-id="dcf3a-152">現在我們可以使用 [Authorize] 屬性，指定使用者必須系統管理員角色，才能存取任何類別中的控制器動作 StoreManagerController 限制存取。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="dcf3a-153">*附註： 可以放置 [Authorize] 屬性，以及控制器類別層級上特定動作方法。*</span><span class="sxs-lookup"><span data-stu-id="dcf3a-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="dcf3a-154">現在瀏覽至 /StoreManager 登入 對話方塊隨即開啟：</span><span class="sxs-lookup"><span data-stu-id="dcf3a-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="dcf3a-155">我們新的系統管理員帳戶登入之後, 您可以前往專輯編輯畫面上做為前。</span><span class="sxs-lookup"><span data-stu-id="dcf3a-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dcf3a-156">[上一頁](mvc-music-store-part-6.md)
> [下一頁](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="dcf3a-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
