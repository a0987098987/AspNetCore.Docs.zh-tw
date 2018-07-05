---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 第 7 節： 成員資格和授權 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 7 節涵蓋的成員資格和授權。
ms.author: aspnetcontent
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 1f2ad9de3a21366931efe6ca2466f4efc23a6192
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802535"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="3c83c-104">第 7 節： 成員資格和授權</span><span class="sxs-lookup"><span data-stu-id="3c83c-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="3c83c-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3c83c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3c83c-106">MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c83c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3c83c-107">MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。</span><span class="sxs-lookup"><span data-stu-id="3c83c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3c83c-108">本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="3c83c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3c83c-109">第 7 節涵蓋的成員資格和授權。</span><span class="sxs-lookup"><span data-stu-id="3c83c-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="3c83c-110">目前瀏覽我們的網站的任何人都能存取我們的存放區管理員控制器。</span><span class="sxs-lookup"><span data-stu-id="3c83c-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="3c83c-111">讓我們變更此限制的網站系統管理員的權限。</span><span class="sxs-lookup"><span data-stu-id="3c83c-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="3c83c-112">加入 AccountController 以及檢視</span><span class="sxs-lookup"><span data-stu-id="3c83c-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="3c83c-113">完整的 ASP.NET MVC 3 Web 應用程式範本和 ASP.NET MVC 3 空白 Web 應用程式範本的其中一個差異是空白的範本不包含帳戶控制器。</span><span class="sxs-lookup"><span data-stu-id="3c83c-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="3c83c-114">我們將新增此帳戶控制器會藉由複製幾個檔案從新的 ASP.NET MVC 應用程式，完整的 ASP.NET MVC 3 Web 應用程式範本所建立的。</span><span class="sxs-lookup"><span data-stu-id="3c83c-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="3c83c-115">建立新的 ASP.NET MVC 應用程式，使用完整的 ASP.NET MVC 3 Web 應用程式範本，並將下列檔案複製到相同的目錄，在我們的專案：</span><span class="sxs-lookup"><span data-stu-id="3c83c-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="3c83c-116">在 [Controllers] 目錄中的複製 AccountController.cs</span><span class="sxs-lookup"><span data-stu-id="3c83c-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="3c83c-117">在模型的目錄中的複製 AccountModels</span><span class="sxs-lookup"><span data-stu-id="3c83c-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="3c83c-118">建立檢視目錄內的帳戶目錄，並複製中的所有四個檢視</span><span class="sxs-lookup"><span data-stu-id="3c83c-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="3c83c-119">變更控制器和模型類別的命名空間，因此它們以 MvcMusicStore 開頭。</span><span class="sxs-lookup"><span data-stu-id="3c83c-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="3c83c-120">AccountController 類別應該使用 MvcMusicStore.Controllers 命名空間中，而 AccountModels 類別應該使用 MvcMusicStore.Models 命名空間。</span><span class="sxs-lookup"><span data-stu-id="3c83c-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="3c83c-121">*注意： 這些檔案也是從中複製網站設計檔案在本教學課程開頭 MvcMusicStore Assets.zip 下載中取得。成員資格檔案位於程式碼目錄中。*</span><span class="sxs-lookup"><span data-stu-id="3c83c-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="3c83c-122">更新的方案看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="3c83c-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="3c83c-123">新增與 ASP.NET 設定站台系統管理使用者</span><span class="sxs-lookup"><span data-stu-id="3c83c-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="3c83c-124">我們在我們的網站所需的授權之前，我們需要建立的使用者具有存取權。</span><span class="sxs-lookup"><span data-stu-id="3c83c-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="3c83c-125">建立使用者的最簡單方式是使用內建的 ASP.NET 組態網站。</span><span class="sxs-lookup"><span data-stu-id="3c83c-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="3c83c-126">按一下 依照 方案總管 中的圖示來啟動 ASP.NET 組態網站。</span><span class="sxs-lookup"><span data-stu-id="3c83c-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="3c83c-127">這會啟動組態網站。</span><span class="sxs-lookup"><span data-stu-id="3c83c-127">This launches a configuration website.</span></span> <span data-ttu-id="3c83c-128">在主畫面上，[安全性] 索引標籤上按一下，然後按一下 [啟用角色] 中的連結在螢幕的中央。</span><span class="sxs-lookup"><span data-stu-id="3c83c-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="3c83c-129">按一下 [建立] 或 [管理角色] 連結。</span><span class="sxs-lookup"><span data-stu-id="3c83c-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="3c83c-130">輸入 「 系統管理員 」 作為角色名稱，然後按 [新增角色] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3c83c-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="3c83c-131">按一下 [上一頁] 按鈕，然後按一下左側的 [建立使用者] 連結。</span><span class="sxs-lookup"><span data-stu-id="3c83c-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="3c83c-132">填妥使用者資訊欄位，在左側，使用下列資訊：</span><span class="sxs-lookup"><span data-stu-id="3c83c-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="3c83c-133">**欄位**</span><span class="sxs-lookup"><span data-stu-id="3c83c-133">**Field**</span></span> | <span data-ttu-id="3c83c-134">**值**</span><span class="sxs-lookup"><span data-stu-id="3c83c-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="3c83c-135">**使用者名稱**</span><span class="sxs-lookup"><span data-stu-id="3c83c-135">**User Name**</span></span> | <span data-ttu-id="3c83c-136">系統管理員</span><span class="sxs-lookup"><span data-stu-id="3c83c-136">Administrator</span></span> |
| <span data-ttu-id="3c83c-137">**密碼**</span><span class="sxs-lookup"><span data-stu-id="3c83c-137">**Password**</span></span> | <span data-ttu-id="3c83c-138">password123 ！</span><span class="sxs-lookup"><span data-stu-id="3c83c-138">password123!</span></span> |
| <span data-ttu-id="3c83c-139">**確認密碼**</span><span class="sxs-lookup"><span data-stu-id="3c83c-139">**Confirm Password**</span></span> | <span data-ttu-id="3c83c-140">password123 ！</span><span class="sxs-lookup"><span data-stu-id="3c83c-140">password123!</span></span> |
| <span data-ttu-id="3c83c-141">**電子郵件**</span><span class="sxs-lookup"><span data-stu-id="3c83c-141">**E-mail**</span></span> | <span data-ttu-id="3c83c-142">（可使用任何電子郵件地址）</span><span class="sxs-lookup"><span data-stu-id="3c83c-142">(any email address will work)</span></span> |
| <span data-ttu-id="3c83c-143">**安全性問題**</span><span class="sxs-lookup"><span data-stu-id="3c83c-143">**Security Question**</span></span> | <span data-ttu-id="3c83c-144">（任意）</span><span class="sxs-lookup"><span data-stu-id="3c83c-144">(whatever you like)</span></span> |
| <span data-ttu-id="3c83c-145">**安全性解答**</span><span class="sxs-lookup"><span data-stu-id="3c83c-145">**Security Answer**</span></span> | <span data-ttu-id="3c83c-146">（任意）</span><span class="sxs-lookup"><span data-stu-id="3c83c-146">(whatever you like)</span></span> |

<span data-ttu-id="3c83c-147">*注意： 您當然可以使用任何您想要的密碼。預設密碼安全性設定需要的 7 個字元且包含一個非英數字元密碼。*</span><span class="sxs-lookup"><span data-stu-id="3c83c-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="3c83c-148">選取此使用者，系統管理員角色，然後按一下 [建立使用者] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3c83c-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="3c83c-149">此時，您應該會看到一則訊息指出使用者已成功建立。</span><span class="sxs-lookup"><span data-stu-id="3c83c-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="3c83c-150">您現在可以關閉瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="3c83c-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="3c83c-151">以角色為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="3c83c-151">Role-based Authorization</span></span>

<span data-ttu-id="3c83c-152">現在我們可以使用 [Authorize] 屬性，指定使用者必須系統管理員角色，才能存取任何類別中的控制器動作 StoreManagerController 限制存取。</span><span class="sxs-lookup"><span data-stu-id="3c83c-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="3c83c-153">*附註： 可以放置 [Authorize] 屬性，以及在控制器類別層級上特定的動作方法。*</span><span class="sxs-lookup"><span data-stu-id="3c83c-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="3c83c-154">現在瀏覽至 /StoreManager 顯示的登入對話方塊：</span><span class="sxs-lookup"><span data-stu-id="3c83c-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="3c83c-155">使用我們新的系統管理員帳戶登入之後, 我們得以將前往專輯編輯畫面，為之前。</span><span class="sxs-lookup"><span data-stu-id="3c83c-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c83c-156">[上一頁](mvc-music-store-part-6.md)
> [下一頁](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="3c83c-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
