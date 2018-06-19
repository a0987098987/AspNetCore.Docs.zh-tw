---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: 安全使用驗證和授權的應用程式 |Microsoft 文件
author: microsoft
description: 步驟 9 示範如何加入驗證和授權來保護 NerdDinner 應用程式中，以便使用者註冊需要和站台才能建立登入...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4a9b1e6d7d453bd8dc5a61b1f1cec4617af7d693
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874378"
---
<a name="secure-applications-using-authentication-and-authorization"></a><span data-ttu-id="3c481-103">安全使用驗證和授權的應用程式</span><span class="sxs-lookup"><span data-stu-id="3c481-103">Secure Applications Using Authentication and Authorization</span></span>
====================
<span data-ttu-id="3c481-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3c481-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="3c481-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="3c481-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="3c481-106">這是一套免費的步驟 9 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="3c481-106">This is step 9 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="3c481-107">步驟 9 示範如何加入驗證和授權來保護 NerdDinner 應用程式中，以便使用者需要註冊和登入站台建立新的 dinners，以及只裝載 dinner 的使用者可以稍後再進行編輯。</span><span class="sxs-lookup"><span data-stu-id="3c481-107">Step 9 shows how to add authentication and authorization to secure our NerdDinner application, so that users need to register and login to the site to create new dinners, and only the user who is hosting a dinner can edit it later.</span></span>
> 
> <span data-ttu-id="3c481-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="3c481-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-9-authentication-and-authorization"></a><span data-ttu-id="3c481-109">NerdDinner 步驟 9： 驗證和授權</span><span class="sxs-lookup"><span data-stu-id="3c481-109">NerdDinner Step 9: Authentication and Authorization</span></span>

<span data-ttu-id="3c481-110">現在我們 NerdDinner 應用程式會授與任何人瀏覽網站的 建立和編輯任何 dinner 的詳細資料的能力。</span><span class="sxs-lookup"><span data-stu-id="3c481-110">Right now our NerdDinner application grants anyone visiting the site the ability to create and edit the details of any dinner.</span></span> <span data-ttu-id="3c481-111">讓我們來變更，這讓使用者需要註冊和登入的站台建立新的 dinners 加入限制，以便只有裝載 dinner 的使用者可以稍後再進行編輯。</span><span class="sxs-lookup"><span data-stu-id="3c481-111">Let's change this so that users need to register and login to the site to create new dinners, and add a restriction so that only the user who is hosting a dinner can edit it later.</span></span>

<span data-ttu-id="3c481-112">若要啟用此我們會使用驗證和授權來保護應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c481-112">To enable this we'll use authentication and authorization to secure our application.</span></span>

### <a name="understanding-authentication-and-authorization"></a><span data-ttu-id="3c481-113">了解驗證和授權</span><span class="sxs-lookup"><span data-stu-id="3c481-113">Understanding Authentication and Authorization</span></span>

<span data-ttu-id="3c481-114">*驗證*是識別及驗證應用程式存取的用戶端的身分識別的程序。</span><span class="sxs-lookup"><span data-stu-id="3c481-114">*Authentication* is the process of identifying and validating the identity of a client accessing an application.</span></span> <span data-ttu-id="3c481-115">它更簡單來說，是關於 「 誰 」，在瀏覽的網站時，使用者識別。</span><span class="sxs-lookup"><span data-stu-id="3c481-115">Put more simply, it is about identifying "who" the end-user is when they visit a website.</span></span> <span data-ttu-id="3c481-116">ASP.NET 支援多種方式來驗證使用者的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="3c481-116">ASP.NET supports multiple ways to authenticate browser users.</span></span> <span data-ttu-id="3c481-117">網際網路的 web 應用程式最常見的驗證方法使用稱為 「 表單驗證 」。</span><span class="sxs-lookup"><span data-stu-id="3c481-117">For Internet web applications, the most common authentication approach used is called "Forms Authentication".</span></span> <span data-ttu-id="3c481-118">表單驗證可讓開發人員撰寫 HTML 登入表單在其應用程式，並驗證使用者名稱/密碼使用者送出針對資料庫或其他密碼認證存放區。</span><span class="sxs-lookup"><span data-stu-id="3c481-118">Forms Authentication enables a developer to author an HTML login form within their application, and then validate the username/password an end-user submits against a database or other password credential store.</span></span> <span data-ttu-id="3c481-119">如果使用者名稱/密碼組合不正確，開發人員可以再詢問 ASP.NET 發行加密的 HTTP cookie 來識別使用者在未來的要求。</span><span class="sxs-lookup"><span data-stu-id="3c481-119">If the username/password combination is correct, the developer can then ask ASP.NET to issue an encrypted HTTP cookie to identify the user across future requests.</span></span> <span data-ttu-id="3c481-120">我們會使用表單驗證與我們 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c481-120">We'll by using forms authentication with our NerdDinner application.</span></span>

<span data-ttu-id="3c481-121">*授權*是判斷是否已驗證的使用者有權限來存取特定 URL/資源或執行某些動作的程序。</span><span class="sxs-lookup"><span data-stu-id="3c481-121">*Authorization* is the process of determining whether an authenticated user has permission to access a particular URL/resource or to perform some action.</span></span> <span data-ttu-id="3c481-122">比方說，我們 NerdDinner 應用程式中我們會想要授權登入使用者可以存取 */Dinners/建立*URL，並建立新的 Dinners。</span><span class="sxs-lookup"><span data-stu-id="3c481-122">For example, within our NerdDinner application we'll want to authorize that only users who are logged in can access the */Dinners/Create* URL and create new Dinners.</span></span> <span data-ttu-id="3c481-123">我們也會想要新增授權邏輯，讓只有裝載 dinner 的使用者可以編輯它 – 並拒絕所有其他使用者的編輯。</span><span class="sxs-lookup"><span data-stu-id="3c481-123">We'll also want to add authorization logic so that only the user who is hosting a dinner can edit it – and deny edit access to all other users.</span></span>

### <a name="forms-authentication-and-the-accountcontroller"></a><span data-ttu-id="3c481-124">表單驗證和 AccountController</span><span class="sxs-lookup"><span data-stu-id="3c481-124">Forms Authentication and the AccountController</span></span>

<span data-ttu-id="3c481-125">ASP.NET MVC 的預設 Visual Studio 專案範本建立新的 ASP.NET MVC 應用程式時，會自動啟用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="3c481-125">The default Visual Studio project template for ASP.NET MVC automatically enables forms authentication when new ASP.NET MVC applications are created.</span></span> <span data-ttu-id="3c481-126">也會自動將預先建立的帳戶登入頁面實作加入至專案 – 可輕鬆整合在站台內的安全性。</span><span class="sxs-lookup"><span data-stu-id="3c481-126">It also automatically adds a pre-built account login page implementation to the project – which makes it really easy to integrate security within a site.</span></span>

<span data-ttu-id="3c481-127">預設 Site.master 主版頁面會顯示 「 登入 」 連結在右上方的 站台，當不進行驗證時存取它的使用者：</span><span class="sxs-lookup"><span data-stu-id="3c481-127">The default Site.master master page displays a "Log On" link at the top-right of the site when the user accessing it is not authenticated:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

<span data-ttu-id="3c481-128">按一下 「 登入 」 的連結會引導使用者 */帳戶/登入*URL:</span><span class="sxs-lookup"><span data-stu-id="3c481-128">Clicking the "Log On" link takes a user to the */Account/LogOn* URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

<span data-ttu-id="3c481-129">訪客您尚未註冊可以這樣做，請按一下 「 註冊 」 連結 – 這會移至 */帳戶/暫存器*URL，讓他們輸入帳戶的詳細資料：</span><span class="sxs-lookup"><span data-stu-id="3c481-129">Visitors who haven't registered can do so by clicking the "Register" link – which will take them to the */Account/Register* URL and allow them to enter account details:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

<span data-ttu-id="3c481-130">按一下 「 註冊 」 按鈕，將建立新的使用者，ASP.NET 成員資格系統，內，並驗證至站台使用表單驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="3c481-130">Clicking the "Register" button will create a new user within the ASP.NET Membership system, and authenticate the user onto the site using forms authentication.</span></span>

<span data-ttu-id="3c481-131">Site.master 登入的使用者時，變更輸出 「 歡迎使用 [username] ！ 」 頁面右上方</span><span class="sxs-lookup"><span data-stu-id="3c481-131">When a user is logged-in, the Site.master changes the top-right of the page to output a "Welcome [username]!"</span></span> <span data-ttu-id="3c481-132">訊息和呈現 「 登出 」 連結而不是 「 登入 」 其中一個。</span><span class="sxs-lookup"><span data-stu-id="3c481-132">message and renders a "Log Off" link instead of a "Log On" one.</span></span> <span data-ttu-id="3c481-133">按一下 「 登出 」 連結登出使用者：</span><span class="sxs-lookup"><span data-stu-id="3c481-133">Clicking the "Log Off" link logs out the user:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

<span data-ttu-id="3c481-134">上述的登入、 登出和註冊功能是已加入至受測專案由 Visual Studio 建立專案時 AccountController 類別內實作。</span><span class="sxs-lookup"><span data-stu-id="3c481-134">The above login, logout, and registration functionality is implemented within the AccountController class that was added to our project by Visual Studio when it created the project.</span></span> <span data-ttu-id="3c481-135">AccountController 的使用者介面會實作使用 \Views\Account 目錄內的檢視範本：</span><span class="sxs-lookup"><span data-stu-id="3c481-135">The UI for the AccountController is implemented using view templates within the \Views\Account directory:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

<span data-ttu-id="3c481-136">AccountController 類別來發出加密的驗證 cookie 與 ASP.NET 成員資格 API，來儲存及驗證使用者名稱/密碼使用 ASP.NET 表單驗證系統。</span><span class="sxs-lookup"><span data-stu-id="3c481-136">The AccountController class uses the ASP.NET Forms Authentication system to issue encrypted authentication cookies, and the ASP.NET Membership API to store and validate usernames/passwords.</span></span> <span data-ttu-id="3c481-137">ASP.NET 成員資格 API 可延伸，並提供要使用任何密碼認證存放區。</span><span class="sxs-lookup"><span data-stu-id="3c481-137">The ASP.NET Membership API is extensible and enables any password credential store to be used.</span></span> <span data-ttu-id="3c481-138">ASP.NET 提供了與儲存使用者名稱/密碼的 SQL 資料庫，或 Active Directory 中的內建成員資格提供者實作。</span><span class="sxs-lookup"><span data-stu-id="3c481-138">ASP.NET ships with built-in membership provider implementations that store username/passwords within a SQL database, or within Active Directory.</span></span>

<span data-ttu-id="3c481-139">我們可以設定我們 NerdDinner 應用程式應該使用開啟專案的根目錄中的"web.config"檔案，並尋找哪一個成員資格提供者&lt;成員資格&gt;區段內。</span><span class="sxs-lookup"><span data-stu-id="3c481-139">We can configure which membership provider our NerdDinner application should use by opening the "web.config" file at the root of the project and looking for the &lt;membership&gt; section within it.</span></span> <span data-ttu-id="3c481-140">建立專案時加入預設 web.config 註冊 SQL 成員資格提供者，並將它設定為使用名為"ApplicationServices"的連接字串來指定資料庫位置。</span><span class="sxs-lookup"><span data-stu-id="3c481-140">The default web.config added when the project was created registers the SQL membership provider, and configures it to use a connection-string named "ApplicationServices" to specify the database location.</span></span>

<span data-ttu-id="3c481-141">預設值"ApplicationServices"連接字串 (內指定&lt;connectionStrings&gt; web.config 檔案區段) 設定為使用 SQL Express。</span><span class="sxs-lookup"><span data-stu-id="3c481-141">The default "ApplicationServices" connection string (which is specified within the &lt;connectionStrings&gt; section of the web.config file) is configured to use SQL Express.</span></span> <span data-ttu-id="3c481-142">它所指向的 SQL Express 資料庫名為"ASPNETDB。MDF 」 在應用程式的 「 應用程式\_資料 」 目錄。</span><span class="sxs-lookup"><span data-stu-id="3c481-142">It points to a SQL Express database named "ASPNETDB.MDF" under the application's "App\_Data" directory.</span></span> <span data-ttu-id="3c481-143">如果此資料庫不存在的成員資格 API 可以用在應用程式中的第一次，ASP.NET 會自動建立資料庫，並提供適當的成員資格資料庫結構描述，其中：</span><span class="sxs-lookup"><span data-stu-id="3c481-143">If this database doesn't exist the first time the Membership API is used within the application, ASP.NET will automatically create the database and provision the appropriate membership database schema within it:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

<span data-ttu-id="3c481-144">如果不使用 SQL Express 我們想要使用完整的 SQL Server 執行個體 （或連接到遠端資料庫），我們需要待辦事項是更新在 web.config 檔案中的"ApplicationServices"連接字串，並請確定適當的成員資格結構描述已加入到它所指向的資料庫。</span><span class="sxs-lookup"><span data-stu-id="3c481-144">If instead of using SQL Express we wanted to use a full SQL Server instance (or connect to a remote database), all we'd need to-do is to update the "ApplicationServices" connection string within the web.config file and make sure that the appropriate membership schema has been added to the database it points at.</span></span> <span data-ttu-id="3c481-145">您可以執行"aspnet\_regsql.exe 」 公用程式內加入資料庫中的成員資格和其他 ASP.NET 應用程式服務的適當結構描述的 \Windows\Microsoft.NET\Framework\v2.0.50727\ 目錄。</span><span class="sxs-lookup"><span data-stu-id="3c481-145">You can run the "aspnet\_regsql.exe" utility within the \Windows\Microsoft.NET\Framework\v2.0.50727\ directory to add the appropriate schema for membership and the other ASP.NET application services to a database.</span></span>

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a><span data-ttu-id="3c481-146">授權 Dinners/建立 URL，使用 [Authorize] 篩選器</span><span class="sxs-lookup"><span data-stu-id="3c481-146">Authorizing the /Dinners/Create URL using the [Authorize] filter</span></span>

<span data-ttu-id="3c481-147">我們不需要撰寫任何程式碼，以啟用安全的驗證和帳戶管理實作 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3c481-147">We didn't have to write any code to enable a secure authentication and account management implementation for the NerdDinner application.</span></span> <span data-ttu-id="3c481-148">使用者可以註冊新帳戶，與我們的應用程式中，登入/登出的站台。</span><span class="sxs-lookup"><span data-stu-id="3c481-148">Users can register new accounts with our application, and login/logout of the site.</span></span>

<span data-ttu-id="3c481-149">現在我們可以將授權邏輯加入至應用程式，並控制哪些它們可以和無法進行站台內使用的驗證狀態 」 和 「 訪客的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="3c481-149">Now we can add authorization logic to the application, and use the authentication status and username of visitors to control what they can and can't do within the site.</span></span> <span data-ttu-id="3c481-150">讓我們先將授權邏輯加入至我們 DinnersController 類別的 [建立] 動作方法。</span><span class="sxs-lookup"><span data-stu-id="3c481-150">Let's begin by adding authorization logic to the "Create" action methods of our DinnersController class.</span></span> <span data-ttu-id="3c481-151">具體而言，我們將要求的使用者存取 */Dinners/建立*URL 必須以登入。</span><span class="sxs-lookup"><span data-stu-id="3c481-151">Specifically, we will require that users accessing the */Dinners/Create* URL must be logged in.</span></span> <span data-ttu-id="3c481-152">如果使用者未登入我們會重新導向至登入頁面，讓他們可以登入。</span><span class="sxs-lookup"><span data-stu-id="3c481-152">If they aren't logged in we'll redirect them to the login page so that they can sign-in.</span></span>

<span data-ttu-id="3c481-153">實作這個邏輯是很簡單。</span><span class="sxs-lookup"><span data-stu-id="3c481-153">Implementing this logic is pretty easy.</span></span> <span data-ttu-id="3c481-154">我們只需要待辦事項是將 [Authorize] 篩選條件屬性加入至我們建立動作方法就像這樣：</span><span class="sxs-lookup"><span data-stu-id="3c481-154">All we need to-do is to add an [Authorize] filter attribute to our Create action methods like so:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

<span data-ttu-id="3c481-155">ASP.NET MVC 支援建立 「 動作篩選 」 可以用來實作可重複使用的邏輯是以宣告方式套用至動作方法的能力。</span><span class="sxs-lookup"><span data-stu-id="3c481-155">ASP.NET MVC supports the ability to create "action filters" that can be used to implement re-usable logic that can be declaratively applied to action methods.</span></span> <span data-ttu-id="3c481-156">[Authorize] 篩選器是一個由 ASP.NET MVC 中，提供內建動作篩選條件，它可讓開發人員以宣告方式將授權規則套用至動作方法和控制器類別。</span><span class="sxs-lookup"><span data-stu-id="3c481-156">The [Authorize] filter is one of the built-in action filters provided by ASP.NET MVC, and it enables a developer to declaratively apply authorization rules to action methods and controller classes.</span></span>

<span data-ttu-id="3c481-157">套用不含任何參數 （類似上述） 時提出動作方法要求的使用者必須登入 –，它會自動重新導向瀏覽器的登入 URL 如果它們不會強制執行 [Authorize] 篩選器。</span><span class="sxs-lookup"><span data-stu-id="3c481-157">When applied without any parameters (like above) the [Authorize] filter enforces that the user making the action method request must be logged in – and it will automatically redirect the browser to the login URL if they aren't.</span></span> <span data-ttu-id="3c481-158">執行這個重新導向原本要求的 URL 會傳遞做為查詢字串引數時 (例如: / 帳戶/登入？ReturnUrl = %2fDinners %2fcreate)。</span><span class="sxs-lookup"><span data-stu-id="3c481-158">When doing this redirect the originally requested URL is passed as a querystring argument (for example: /Account/LogOn?ReturnUrl=%2fDinners%2fCreate).</span></span> <span data-ttu-id="3c481-159">AccountController 會再將使用者重新導向至原本要求的 URL 一旦他們登入。</span><span class="sxs-lookup"><span data-stu-id="3c481-159">The AccountController will then redirect the user back to the originally requested URL once they login.</span></span>

<span data-ttu-id="3c481-160">[Authorize] 篩選選擇性地支援能夠指定可用來要求，使用者會同時記錄中另一項權限的使用者或允許的安全性角色的成員的清單中的 「 使用者 」 或 「 角色 」 屬性。</span><span class="sxs-lookup"><span data-stu-id="3c481-160">The [Authorize] filter optionally supports the ability to specify a "Users" or "Roles" property that can be used to require that the user is both logged in and within a list of allowed users or a member of an allowed security role.</span></span> <span data-ttu-id="3c481-161">例如，下列程式碼只允許兩個特定的使用者、 「 scottgu"和"billg 」，以存取 Dinners/建立的 URL:</span><span class="sxs-lookup"><span data-stu-id="3c481-161">For example, the code below only allows two specific users, "scottgu" and "billg", to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

<span data-ttu-id="3c481-162">內嵌程式碼中的特定使用者名稱通常會以雖然是很不容易維護。</span><span class="sxs-lookup"><span data-stu-id="3c481-162">Embedding specific user names within code tends to be pretty un-maintainable though.</span></span> <span data-ttu-id="3c481-163">更好的方法是定義較高層級的 「 角色 」 的程式碼會檢查，然後將使用者對應到使用資料庫或 active directory 系統 （啟用 外部程式碼與儲存的實際的使用者對應清單） 的角色。</span><span class="sxs-lookup"><span data-stu-id="3c481-163">A better approach is to define higher-level "roles" that the code checks against, and then to map users into the role using either a database or active directory system (enabling the actual user mapping list to be stored externally from the code).</span></span> <span data-ttu-id="3c481-164">ASP.NET 包含內建的角色管理 API，以及一組內建的角色提供者 （包括 SQL 和 Active Directory），可協助您執行此使用者/角色對應。</span><span class="sxs-lookup"><span data-stu-id="3c481-164">ASP.NET includes a built-in role management API as well as a built-in set of role providers (including ones for SQL and Active Directory) that can help perform this user/role mapping.</span></span> <span data-ttu-id="3c481-165">我們無法再更新，只允許特定"admin"角色中的使用者存取 Dinners/建立 URL 的程式碼：</span><span class="sxs-lookup"><span data-stu-id="3c481-165">We could then update the code to only allow users within a specific "admin" role to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a><span data-ttu-id="3c481-166">建立時，使用 User.Identity.Name 屬性 Dinners</span><span class="sxs-lookup"><span data-stu-id="3c481-166">Using the User.Identity.Name property when Creating Dinners</span></span>

<span data-ttu-id="3c481-167">我們無法擷取使用者的要求使用控制器的基底類別上公開 User.Identity.Name 屬性的目前登入的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="3c481-167">We can retrieve the username of the currently logged-in user of a request using the User.Identity.Name property exposed on the Controller base class.</span></span>

<span data-ttu-id="3c481-168">舊版我們實作我們 create （） 的動作方法的 HTTP POST 版本時，我們必須硬式編碼成靜態字串 Dinner"HostedBy"屬性。</span><span class="sxs-lookup"><span data-stu-id="3c481-168">Earlier when we implemented the HTTP-POST version of our Create() action method we had hardcoded the "HostedBy" property of the Dinner to a static string.</span></span> <span data-ttu-id="3c481-169">我們可以立即更新這個程式碼以改用 User.Identity.Name 屬性，以及建立 Dinner 主機自動新增 RSVP:</span><span class="sxs-lookup"><span data-stu-id="3c481-169">We can now update this code to instead use the User.Identity.Name property, as well as automatically add an RSVP for the host creating the Dinner:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

<span data-ttu-id="3c481-170">我們在 create （） 方法中加入 [Authorize] 屬性，因為 ASP.NET MVC 可確保在動作方法只執行如果造訪 Dinners/建立 URL 的使用者登入站台上。</span><span class="sxs-lookup"><span data-stu-id="3c481-170">Because we have added an [Authorize] attribute to the Create() method, ASP.NET MVC ensures that the action method only executes if the user visiting the /Dinners/Create URL is logged in on the site.</span></span> <span data-ttu-id="3c481-171">因此，User.Identity.Name 屬性值將永遠包含有效的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="3c481-171">As such, the User.Identity.Name property value will always contain a valid username.</span></span>

### <a name="using-the-useridentityname-property-when-editing-dinners"></a><span data-ttu-id="3c481-172">編輯時，使用 User.Identity.Name 屬性 Dinners</span><span class="sxs-lookup"><span data-stu-id="3c481-172">Using the User.Identity.Name property when Editing Dinners</span></span>

<span data-ttu-id="3c481-173">讓我們現在加入授權邏輯，會限制使用者，讓他們只能編輯的 dinners 他們自行裝載的屬性。</span><span class="sxs-lookup"><span data-stu-id="3c481-173">Let's now add some authorization logic that restricts users so that they can only edit the properties of dinners they themselves are hosting.</span></span>

<span data-ttu-id="3c481-174">為協助解決此，我們會先加入"IsHostedBy(username)"helper 方法 （在我們稍早建立的 Dinner.cs 部分類別） 我們 Dinner 物件。</span><span class="sxs-lookup"><span data-stu-id="3c481-174">To help with this, we'll first add an "IsHostedBy(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="3c481-175">這個 helper 方法會傳回 true 或 false，根據提供的使用者名稱是否符合 Dinner HostedBy 屬性，以及封裝執行不區分大小寫字串比較它們所需的邏輯：</span><span class="sxs-lookup"><span data-stu-id="3c481-175">This helper method returns true or false depending on whether a supplied username matches the Dinner HostedBy property, and encapsulates the logic necessary to perform a case-insensitive string comparison of them:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

<span data-ttu-id="3c481-176">然後，我們會新增至 Edit() 動作方法 [Authorize] 屬性內 DinnersController 類別。</span><span class="sxs-lookup"><span data-stu-id="3c481-176">We'll then add an [Authorize] attribute to the Edit() action methods within our DinnersController class.</span></span> <span data-ttu-id="3c481-177">這可確保使用者必須登入，要求 */Dinners/編輯 / [id]* URL。</span><span class="sxs-lookup"><span data-stu-id="3c481-177">This will ensure that users must be logged in to request a */Dinners/Edit/[id]* URL.</span></span>

<span data-ttu-id="3c481-178">然後我們可以將程式碼加入使用 Dinner.IsHostedBy(username) helper 方法來驗證登入使用者符合 Dinner 主機我們編輯方法。</span><span class="sxs-lookup"><span data-stu-id="3c481-178">We can then add code to our Edit methods that uses the Dinner.IsHostedBy(username) helper method to verify that the logged-in user matches the Dinner host.</span></span> <span data-ttu-id="3c481-179">如果使用者不是主機，我們將會顯示 「 InvalidOwner 」 檢視，並終止要求。</span><span class="sxs-lookup"><span data-stu-id="3c481-179">If the user is not the host, we'll display an "InvalidOwner" view and terminate the request.</span></span> <span data-ttu-id="3c481-180">若要這樣做的程式碼看起來如下：</span><span class="sxs-lookup"><span data-stu-id="3c481-180">The code to do this looks like below:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

<span data-ttu-id="3c481-181">我們可以 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;檢視功能表命令，以建立新的 「 InvalidOwner"檢視。</span><span class="sxs-lookup"><span data-stu-id="3c481-181">We can then right-click on the \Views\Dinners directory and choose the Add-&gt;View menu command to create a new "InvalidOwner" view.</span></span> <span data-ttu-id="3c481-182">我們將會填入它具有下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="3c481-182">We'll populate it with the below error message:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

<span data-ttu-id="3c481-183">然後，現在當使用者嘗試編輯它們並不擁有 dinner，它們將會得到錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="3c481-183">And now when a user attempts to edit a dinner they don't own, they'll get an error message:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

<span data-ttu-id="3c481-184">我們可以鎖定，刪除 Dinners，並確保只有 Dinner 主機才能刪除它的權限我們控制器內重複 delete 動作方法相同的步驟。</span><span class="sxs-lookup"><span data-stu-id="3c481-184">We can repeat the same steps for the Delete() action methods within our controller to lock down permission to delete Dinners as well, and ensure that only the host of a Dinner can delete it.</span></span>

### <a name="showinghiding-edit-and-delete-links"></a><span data-ttu-id="3c481-185">顯示/隱藏編輯和刪除連結</span><span class="sxs-lookup"><span data-stu-id="3c481-185">Showing/Hiding Edit and Delete Links</span></span>

<span data-ttu-id="3c481-186">我們要從我們詳細資料的 URL 連結我們 DinnersController 類別的編輯和刪除動作方法：</span><span class="sxs-lookup"><span data-stu-id="3c481-186">We are linking to the Edit and Delete action method of our DinnersController class from our Details URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

<span data-ttu-id="3c481-187">目前，我們會說明不論訪客的詳細資料的 URL 是 dinner 主機的編輯和刪除動作連結。</span><span class="sxs-lookup"><span data-stu-id="3c481-187">Currently we are showing the Edit and Delete action links regardless of whether the visitor to the details URL is the host of the dinner.</span></span> <span data-ttu-id="3c481-188">讓我們來變更這使如果正在瀏覽使用者 dinner 的擁有者，只會顯示連結。</span><span class="sxs-lookup"><span data-stu-id="3c481-188">Let's change this so that the links are only displayed if the visiting user is the owner of the dinner.</span></span>

<span data-ttu-id="3c481-189">我們 DinnersController 內 Details() 動作方法會擷取 Dinner 物件，並再將它當做模型物件，傳遞至我們檢視範本：</span><span class="sxs-lookup"><span data-stu-id="3c481-189">The Details() action method within our DinnersController retrieves a Dinner object and then passes it as the model object to our view template:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

<span data-ttu-id="3c481-190">我們可以更新有條件地顯示/隱藏的編輯和刪除連結使用 helper 方法類似下面的 Dinner.IsHostedBy() 我們檢視的範本：</span><span class="sxs-lookup"><span data-stu-id="3c481-190">We can update our view template to conditionally show/hide the Edit and Delete links by using the Dinner.IsHostedBy() helper method like below:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a><span data-ttu-id="3c481-191">後續步驟</span><span class="sxs-lookup"><span data-stu-id="3c481-191">Next Steps</span></span>

<span data-ttu-id="3c481-192">我們現在看我們如何啟用使用 AJAX dinners 的 RSVP 經過驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="3c481-192">Let's now look at how we can enable authenticated users to RSVP for dinners using AJAX.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c481-193">[上一頁](implement-efficient-data-paging.md)
> [下一頁](use-ajax-to-deliver-dynamic-updates.md)</span><span class="sxs-lookup"><span data-stu-id="3c481-193">[Previous](implement-efficient-data-paging.md)
[Next](use-ajax-to-deliver-dynamic-updates.md)</span></span>
