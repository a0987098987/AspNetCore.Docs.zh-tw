---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: 保護應用程式使用驗證和授權 |Microsoft Docs
author: microsoft
description: 步驟 9 示範如何加入驗證和授權來保護我們的 NerdDinner 應用程式，讓使用者需要註冊並登入若要建立站台...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d5f1b26312f11fd6d4ab500c7f24a4d89d428e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825370"
---
<a name="secure-applications-using-authentication-and-authorization"></a><span data-ttu-id="0e55e-103">保護使用驗證和授權的應用程式</span><span class="sxs-lookup"><span data-stu-id="0e55e-103">Secure Applications Using Authentication and Authorization</span></span>
====================
<span data-ttu-id="0e55e-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0e55e-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0e55e-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="0e55e-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="0e55e-106">這是一套免費的步驟 9 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e55e-106">This is step 9 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="0e55e-107">步驟 9 示範如何加入驗證和授權來保護我們的 NerdDinner 應用程式，讓使用者需要註冊和登入站台建立新的 dinners，以及只有裝載 dinner 的使用者可以稍後再進行編輯。</span><span class="sxs-lookup"><span data-stu-id="0e55e-107">Step 9 shows how to add authentication and authorization to secure our NerdDinner application, so that users need to register and login to the site to create new dinners, and only the user who is hosting a dinner can edit it later.</span></span>
> 
> <span data-ttu-id="0e55e-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="0e55e-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-9-authentication-and-authorization"></a><span data-ttu-id="0e55e-109">NerdDinner 步驟 9： 驗證和授權</span><span class="sxs-lookup"><span data-stu-id="0e55e-109">NerdDinner Step 9: Authentication and Authorization</span></span>

<span data-ttu-id="0e55e-110">現在我們 NerdDinner 應用程式授與任何人瀏覽的網站能夠建立和編輯任何 dinner 的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0e55e-110">Right now our NerdDinner application grants anyone visiting the site the ability to create and edit the details of any dinner.</span></span> <span data-ttu-id="0e55e-111">讓我們將此變更，讓使用者需要註冊並登入網站來建立新的 dinners，並新增一項限制，以便只有裝載 dinner 的使用者可以稍後再進行編輯。</span><span class="sxs-lookup"><span data-stu-id="0e55e-111">Let's change this so that users need to register and login to the site to create new dinners, and add a restriction so that only the user who is hosting a dinner can edit it later.</span></span>

<span data-ttu-id="0e55e-112">若要啟用此我們將使用的驗證和授權來保護我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e55e-112">To enable this we'll use authentication and authorization to secure our application.</span></span>

### <a name="understanding-authentication-and-authorization"></a><span data-ttu-id="0e55e-113">了解驗證和授權</span><span class="sxs-lookup"><span data-stu-id="0e55e-113">Understanding Authentication and Authorization</span></span>

<span data-ttu-id="0e55e-114">*驗證*是用來識別和驗證存取的應用程式的用戶端的身分識別的程序。</span><span class="sxs-lookup"><span data-stu-id="0e55e-114">*Authentication* is the process of identifying and validating the identity of a client accessing an application.</span></span> <span data-ttu-id="0e55e-115">來說更簡單的做法，這是關於 「 誰 」，他們瀏覽網站時，終端使用者識別。</span><span class="sxs-lookup"><span data-stu-id="0e55e-115">Put more simply, it is about identifying "who" the end-user is when they visit a website.</span></span> <span data-ttu-id="0e55e-116">ASP.NET 支援多種方式來驗證瀏覽器的使用者。</span><span class="sxs-lookup"><span data-stu-id="0e55e-116">ASP.NET supports multiple ways to authenticate browser users.</span></span> <span data-ttu-id="0e55e-117">對於網際網路的 web 應用程式，最常見的驗證方法，使用稱為 「 表單驗證 」。</span><span class="sxs-lookup"><span data-stu-id="0e55e-117">For Internet web applications, the most common authentication approach used is called "Forms Authentication".</span></span> <span data-ttu-id="0e55e-118">表單驗證可讓開發人員撰寫 HTML 登入表單在其應用程式，並驗證使用者提交針對資料庫或其他密碼認證存放區的使用者名稱/密碼。</span><span class="sxs-lookup"><span data-stu-id="0e55e-118">Forms Authentication enables a developer to author an HTML login form within their application, and then validate the username/password an end-user submits against a database or other password credential store.</span></span> <span data-ttu-id="0e55e-119">如果使用者名稱/密碼組合不正確，開發人員則可以要求發出加密的 HTTP cookie 來識別使用者在未來的要求間的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="0e55e-119">If the username/password combination is correct, the developer can then ask ASP.NET to issue an encrypted HTTP cookie to identify the user across future requests.</span></span> <span data-ttu-id="0e55e-120">我們將使用表單驗證的 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e55e-120">We'll by using forms authentication with our NerdDinner application.</span></span>

<span data-ttu-id="0e55e-121">*授權*程序會判斷已驗證的使用者是否有權存取特定 URL/資源，或執行某些動作。</span><span class="sxs-lookup"><span data-stu-id="0e55e-121">*Authorization* is the process of determining whether an authenticated user has permission to access a particular URL/resource or to perform some action.</span></span> <span data-ttu-id="0e55e-122">比方說，我們 NerdDinner 應用程式中我們會想要授權僅登入的使用者可以存取*Dinners/建立*URL，並建立新的 Dinners。</span><span class="sxs-lookup"><span data-stu-id="0e55e-122">For example, within our NerdDinner application we'll want to authorize that only users who are logged in can access the */Dinners/Create* URL and create new Dinners.</span></span> <span data-ttu-id="0e55e-123">我們也要新增授權邏輯，讓只有裝載 dinner 的使用者可以編輯它 – 並拒絕所有其他使用者的編輯權限。</span><span class="sxs-lookup"><span data-stu-id="0e55e-123">We'll also want to add authorization logic so that only the user who is hosting a dinner can edit it – and deny edit access to all other users.</span></span>

### <a name="forms-authentication-and-the-accountcontroller"></a><span data-ttu-id="0e55e-124">表單驗證和 AccountController</span><span class="sxs-lookup"><span data-stu-id="0e55e-124">Forms Authentication and the AccountController</span></span>

<span data-ttu-id="0e55e-125">ASP.NET MVC 的預設 Visual Studio 專案範本會建立新的 ASP.NET MVC 應用程式時，會自動啟用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="0e55e-125">The default Visual Studio project template for ASP.NET MVC automatically enables forms authentication when new ASP.NET MVC applications are created.</span></span> <span data-ttu-id="0e55e-126">它也會自動將專案 – 可讓您輕鬆將站台內的安全性整合預先建立的帳戶登入頁面實作。</span><span class="sxs-lookup"><span data-stu-id="0e55e-126">It also automatically adds a pre-built account login page implementation to the project – which makes it really easy to integrate security within a site.</span></span>

<span data-ttu-id="0e55e-127">存取它的使用者未經驗證時，預設 Site.master 主版頁面會顯示 「 登入 」 連結在右上方的 站台：</span><span class="sxs-lookup"><span data-stu-id="0e55e-127">The default Site.master master page displays a "Log On" link at the top-right of the site when the user accessing it is not authenticated:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

<span data-ttu-id="0e55e-128">按一下 「 登入 」 連結會帶使用者 */帳戶/登入*URL:</span><span class="sxs-lookup"><span data-stu-id="0e55e-128">Clicking the "Log On" link takes a user to the */Account/LogOn* URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

<span data-ttu-id="0e55e-129">訪客尚未註冊做法是按一下 [註冊] 連結 – 這會需要他們 */帳戶/註冊*URL，並允許他們輸入帳戶的詳細資料：</span><span class="sxs-lookup"><span data-stu-id="0e55e-129">Visitors who haven't registered can do so by clicking the "Register" link – which will take them to the */Account/Register* URL and allow them to enter account details:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

<span data-ttu-id="0e55e-130">按一下 [註冊] 按鈕建立新的使用者，在 ASP.NET 成員資格系統，並驗證連線至站台，使用表單驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="0e55e-130">Clicking the "Register" button will create a new user within the ASP.NET Membership system, and authenticate the user onto the site using forms authentication.</span></span>

<span data-ttu-id="0e55e-131">Site.master 登入的使用者時，變更輸出 「 歡迎 [username] ！ 頁面右上方</span><span class="sxs-lookup"><span data-stu-id="0e55e-131">When a user is logged-in, the Site.master changes the top-right of the page to output a "Welcome [username]!"</span></span> <span data-ttu-id="0e55e-132">訊息並呈現 「 登出 」 連結而不是 「 登入 」 其中一個。</span><span class="sxs-lookup"><span data-stu-id="0e55e-132">message and renders a "Log Off" link instead of a "Log On" one.</span></span> <span data-ttu-id="0e55e-133">按一下 「 登出 」 連結，將使用者記錄：</span><span class="sxs-lookup"><span data-stu-id="0e55e-133">Clicking the "Log Off" link logs out the user:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

<span data-ttu-id="0e55e-134">已新增至我們的專案的 Visual Studio 建立專案時 AccountController 類別中實作上述的登入、 登出和註冊功能。</span><span class="sxs-lookup"><span data-stu-id="0e55e-134">The above login, logout, and registration functionality is implemented within the AccountController class that was added to our project by Visual Studio when it created the project.</span></span> <span data-ttu-id="0e55e-135">AccountController 的使用者介面的實作使用 \Views\Account 目錄內的檢視範本：</span><span class="sxs-lookup"><span data-stu-id="0e55e-135">The UI for the AccountController is implemented using view templates within the \Views\Account directory:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

<span data-ttu-id="0e55e-136">AccountController 類別會使用 ASP.NET 表單驗證系統發出加密的驗證 cookie 和 ASP.NET 成員資格 API 來儲存和驗證使用者名稱/密碼。</span><span class="sxs-lookup"><span data-stu-id="0e55e-136">The AccountController class uses the ASP.NET Forms Authentication system to issue encrypted authentication cookies, and the ASP.NET Membership API to store and validate usernames/passwords.</span></span> <span data-ttu-id="0e55e-137">ASP.NET Membership API 加以擴充，而且可讓任何密碼的認證存放區可用。</span><span class="sxs-lookup"><span data-stu-id="0e55e-137">The ASP.NET Membership API is extensible and enables any password credential store to be used.</span></span> <span data-ttu-id="0e55e-138">ASP.NET 隨附儲存使用者名稱/密碼，SQL database，或 Active Directory 中的內建的成員資格提供者實作。</span><span class="sxs-lookup"><span data-stu-id="0e55e-138">ASP.NET ships with built-in membership provider implementations that store username/passwords within a SQL database, or within Active Directory.</span></span>

<span data-ttu-id="0e55e-139">我們可以設定我們 NerdDinner 應用程式應該使用開啟的專案根目錄"web.config"檔案，並尋找哪一個成員資格提供者&lt;成員資格&gt;其內的區段。</span><span class="sxs-lookup"><span data-stu-id="0e55e-139">We can configure which membership provider our NerdDinner application should use by opening the "web.config" file at the root of the project and looking for the &lt;membership&gt; section within it.</span></span> <span data-ttu-id="0e55e-140">當建立專案時加入預設 web.config 註冊 SQL 成員資格提供者，並將它設定為使用名為"ApplicationServices"的連接字串來指定資料庫位置。</span><span class="sxs-lookup"><span data-stu-id="0e55e-140">The default web.config added when the project was created registers the SQL membership provider, and configures it to use a connection-string named "ApplicationServices" to specify the database location.</span></span>

<span data-ttu-id="0e55e-141">預設的 「 ApplicationServices"連接字串 (指定內&lt;connectionStrings&gt; web.config 檔案區段) 已設定為使用 SQL Express。</span><span class="sxs-lookup"><span data-stu-id="0e55e-141">The default "ApplicationServices" connection string (which is specified within the &lt;connectionStrings&gt; section of the web.config file) is configured to use SQL Express.</span></span> <span data-ttu-id="0e55e-142">它會指向名為"ASPNETDB SQL Express 資料庫。MDF 」 的應用程式 」 應用程式\_Data"目錄。</span><span class="sxs-lookup"><span data-stu-id="0e55e-142">It points to a SQL Express database named "ASPNETDB.MDF" under the application's "App\_Data" directory.</span></span> <span data-ttu-id="0e55e-143">如果此資料庫不存在的成員資格 API 可在應用程式中的第一次，ASP.NET 會自動建立資料庫，並佈建適當的成員資格資料庫結構描述，其中：</span><span class="sxs-lookup"><span data-stu-id="0e55e-143">If this database doesn't exist the first time the Membership API is used within the application, ASP.NET will automatically create the database and provision the appropriate membership database schema within it:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

<span data-ttu-id="0e55e-144">如果不使用 SQL Express 我們想要使用完整的 SQL Server 執行個體 （或連接到遠端資料庫），我們會需要待辦事項是更新 web.config 檔案中的 「 ApplicationServices"連接字串，並確定適當的成員資格結構描述已新增至它所指向的資料庫。</span><span class="sxs-lookup"><span data-stu-id="0e55e-144">If instead of using SQL Express we wanted to use a full SQL Server instance (or connect to a remote database), all we'd need to-do is to update the "ApplicationServices" connection string within the web.config file and make sure that the appropriate membership schema has been added to the database it points at.</span></span> <span data-ttu-id="0e55e-145">您可以執行"aspnet\_regsql.exe"\Windows\Microsoft.NET\Framework\v2.0.50727\ 目錄，將成員資格和其他 ASP.NET 應用程式服務的適當結構描述新增至資料庫內的公用程式。</span><span class="sxs-lookup"><span data-stu-id="0e55e-145">You can run the "aspnet\_regsql.exe" utility within the \Windows\Microsoft.NET\Framework\v2.0.50727\ directory to add the appropriate schema for membership and the other ASP.NET application services to a database.</span></span>

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a><span data-ttu-id="0e55e-146">授權 [Authorize] 篩選 Dinners/建立 URL</span><span class="sxs-lookup"><span data-stu-id="0e55e-146">Authorizing the /Dinners/Create URL using the [Authorize] filter</span></span>

<span data-ttu-id="0e55e-147">我們不必撰寫任何程式碼，以啟用安全的驗證和帳戶管理實作 NerdDinner 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0e55e-147">We didn't have to write any code to enable a secure authentication and account management implementation for the NerdDinner application.</span></span> <span data-ttu-id="0e55e-148">使用者可以向我們的應用程式和登入/登出的站台的新帳戶。</span><span class="sxs-lookup"><span data-stu-id="0e55e-148">Users can register new accounts with our application, and login/logout of the site.</span></span>

<span data-ttu-id="0e55e-149">現在我們可以新增授權邏輯應用程式，並使用的訪客的使用者名稱與驗證狀態來控制什麼他們能夠與無法執行站台內。</span><span class="sxs-lookup"><span data-stu-id="0e55e-149">Now we can add authorization logic to the application, and use the authentication status and username of visitors to control what they can and can't do within the site.</span></span> <span data-ttu-id="0e55e-150">讓我們先授權邏輯加入我們的 DinnersController 類別的 [建立] 動作方法。</span><span class="sxs-lookup"><span data-stu-id="0e55e-150">Let's begin by adding authorization logic to the "Create" action methods of our DinnersController class.</span></span> <span data-ttu-id="0e55e-151">具體來說，我們會要求使用者存取*Dinners/建立*URL 必須登入。</span><span class="sxs-lookup"><span data-stu-id="0e55e-151">Specifically, we will require that users accessing the */Dinners/Create* URL must be logged in.</span></span> <span data-ttu-id="0e55e-152">如果使用者未登入我們將會重新導向至登入頁面，讓他們可以登入。</span><span class="sxs-lookup"><span data-stu-id="0e55e-152">If they aren't logged in we'll redirect them to the login page so that they can sign-in.</span></span>

<span data-ttu-id="0e55e-153">實作此邏輯並不難。</span><span class="sxs-lookup"><span data-stu-id="0e55e-153">Implementing this logic is pretty easy.</span></span> <span data-ttu-id="0e55e-154">我們只需要待辦事項是將 [Authorize] 篩選條件屬性加入至我們建立的動作方法就像這樣：</span><span class="sxs-lookup"><span data-stu-id="0e55e-154">All we need to-do is to add an [Authorize] filter attribute to our Create action methods like so:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

<span data-ttu-id="0e55e-155">ASP.NET MVC 支援能夠建立 「 動作篩選條件 」，可用來實作可重複使用透過宣告方式套用至動作方法的邏輯。</span><span class="sxs-lookup"><span data-stu-id="0e55e-155">ASP.NET MVC supports the ability to create "action filters" that can be used to implement re-usable logic that can be declaratively applied to action methods.</span></span> <span data-ttu-id="0e55e-156">[Authorize] 篩選條件是其中一個 ASP.NET MVC 中，所提供的內建的動作篩選條件，它可讓開發人員以宣告方式將授權規則套用至動作方法和控制器類別。</span><span class="sxs-lookup"><span data-stu-id="0e55e-156">The [Authorize] filter is one of the built-in action filters provided by ASP.NET MVC, and it enables a developer to declaratively apply authorization rules to action methods and controller classes.</span></span>

<span data-ttu-id="0e55e-157">套用不含任何參數 （如上述） 時要求動作方法的使用者必須登入 –，它會自動重新導向瀏覽器登入 url 如果它們不會強制執行 [Authorize] 篩選器。</span><span class="sxs-lookup"><span data-stu-id="0e55e-157">When applied without any parameters (like above) the [Authorize] filter enforces that the user making the action method request must be logged in – and it will automatically redirect the browser to the login URL if they aren't.</span></span> <span data-ttu-id="0e55e-158">執行此重新導向原本要求的 URL 做為查詢字串引數傳遞時 (例如: / 帳戶/登入？ReturnUrl = %2fDinners %2fcreate)。</span><span class="sxs-lookup"><span data-stu-id="0e55e-158">When doing this redirect the originally requested URL is passed as a querystring argument (for example: /Account/LogOn?ReturnUrl=%2fDinners%2fCreate).</span></span> <span data-ttu-id="0e55e-159">AccountController 會再將使用者重新導向回到原來要求的 URL 之後登入。</span><span class="sxs-lookup"><span data-stu-id="0e55e-159">The AccountController will then redirect the user back to the originally requested URL once they login.</span></span>

<span data-ttu-id="0e55e-160">[Authorize] 篩選器選擇性地支援能夠指定可用來要求的使用者同時登中和允許的使用者或允許的安全性角色的成員的清單中的 「 使用者 」 或 「 角色 」 屬性。</span><span class="sxs-lookup"><span data-stu-id="0e55e-160">The [Authorize] filter optionally supports the ability to specify a "Users" or "Roles" property that can be used to require that the user is both logged in and within a list of allowed users or a member of an allowed security role.</span></span> <span data-ttu-id="0e55e-161">例如，下列程式碼只允許兩個特定的使用者、 「 scottgu"和"billg 」，以存取 Dinners/建立的 URL:</span><span class="sxs-lookup"><span data-stu-id="0e55e-161">For example, the code below only allows two specific users, "scottgu" and "billg", to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

<span data-ttu-id="0e55e-162">內嵌程式碼中的特定使用者名稱通常不過是很不容易維護。</span><span class="sxs-lookup"><span data-stu-id="0e55e-162">Embedding specific user names within code tends to be pretty un-maintainable though.</span></span> <span data-ttu-id="0e55e-163">更好的方法是定義較高層級的 「 角色 」 的程式碼會檢查，然後將使用者對應到角色使用的資料庫 」 或 「 active directory 系統 （啟用實際的使用者對應清單，從程式碼外部儲存）。</span><span class="sxs-lookup"><span data-stu-id="0e55e-163">A better approach is to define higher-level "roles" that the code checks against, and then to map users into the role using either a database or active directory system (enabling the actual user mapping list to be stored externally from the code).</span></span> <span data-ttu-id="0e55e-164">ASP.NET 包含內建的角色管理 API，以及一組內建的角色提供者 （包括 SQL 和 Active Directory），可協助您執行此使用者/角色對應。</span><span class="sxs-lookup"><span data-stu-id="0e55e-164">ASP.NET includes a built-in role management API as well as a built-in set of role providers (including ones for SQL and Active Directory) that can help perform this user/role mapping.</span></span> <span data-ttu-id="0e55e-165">然後，我們可以更新為只允許特定的 「 管理員 」 角色中的使用者存取 Dinners/建立 URL 的程式碼：</span><span class="sxs-lookup"><span data-stu-id="0e55e-165">We could then update the code to only allow users within a specific "admin" role to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a><span data-ttu-id="0e55e-166">建立時，使用 User.Identity.Name 屬性 Dinners</span><span class="sxs-lookup"><span data-stu-id="0e55e-166">Using the User.Identity.Name property when Creating Dinners</span></span>

<span data-ttu-id="0e55e-167">我們可以擷取之使用者的要求，使用控制器的基底類別上公開的 User.Identity.Name 屬性的目前登入的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="0e55e-167">We can retrieve the username of the currently logged-in user of a request using the User.Identity.Name property exposed on the Controller base class.</span></span>

<span data-ttu-id="0e55e-168">先前我們實作我們 create （） 動作方法的 HTTP POST 新版時我們有硬式編碼為靜態字串 Dinner"HostedBy"屬性。</span><span class="sxs-lookup"><span data-stu-id="0e55e-168">Earlier when we implemented the HTTP-POST version of our Create() action method we had hardcoded the "HostedBy" property of the Dinner to a static string.</span></span> <span data-ttu-id="0e55e-169">我們可以立即更新此程式碼以改用 User.Identity.Name 屬性，以及自動建立 Dinner 的主機新增 RSVP:</span><span class="sxs-lookup"><span data-stu-id="0e55e-169">We can now update this code to instead use the User.Identity.Name property, as well as automatically add an RSVP for the host creating the Dinner:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

<span data-ttu-id="0e55e-170">我們在 create （） 方法中加入 [Authorize] 屬性，因為 ASP.NET MVC 可確保在動作方法只執行如果網站上造訪 Dinners/建立 URL 的使用者登入。</span><span class="sxs-lookup"><span data-stu-id="0e55e-170">Because we have added an [Authorize] attribute to the Create() method, ASP.NET MVC ensures that the action method only executes if the user visiting the /Dinners/Create URL is logged in on the site.</span></span> <span data-ttu-id="0e55e-171">因此，User.Identity.Name 屬性值一律會包含有效的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="0e55e-171">As such, the User.Identity.Name property value will always contain a valid username.</span></span>

### <a name="using-the-useridentityname-property-when-editing-dinners"></a><span data-ttu-id="0e55e-172">編輯時，使用 User.Identity.Name 屬性 Dinners</span><span class="sxs-lookup"><span data-stu-id="0e55e-172">Using the User.Identity.Name property when Editing Dinners</span></span>

<span data-ttu-id="0e55e-173">讓我們現在新增授權邏輯，以限制使用者，讓他們只能編輯的 dinners 裝載他們自己的屬性。</span><span class="sxs-lookup"><span data-stu-id="0e55e-173">Let's now add some authorization logic that restricts users so that they can only edit the properties of dinners they themselves are hosting.</span></span>

<span data-ttu-id="0e55e-174">若要協助，我們會先加入"IsHostedBy(username)"helper 方法我們 Dinner 物件 （在我們稍早建立的 Dinner.cs 部分類別）。</span><span class="sxs-lookup"><span data-stu-id="0e55e-174">To help with this, we'll first add an "IsHostedBy(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="0e55e-175">這個 helper 方法會傳回 true 或 false 取決於提供的使用者名稱是否符合 Dinner HostedBy 屬性，並封裝執行不區分大小寫的字串比較，其中所需的邏輯：</span><span class="sxs-lookup"><span data-stu-id="0e55e-175">This helper method returns true or false depending on whether a supplied username matches the Dinner HostedBy property, and encapsulates the logic necessary to perform a case-insensitive string comparison of them:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

<span data-ttu-id="0e55e-176">然後，我們將新增至 edit （） 動作方法 [Authorize] 屬性中我們 DinnersController 類別。</span><span class="sxs-lookup"><span data-stu-id="0e55e-176">We'll then add an [Authorize] attribute to the Edit() action methods within our DinnersController class.</span></span> <span data-ttu-id="0e55e-177">這可確保使用者必須登入，要求 */Dinners/編輯 / [id]* URL。</span><span class="sxs-lookup"><span data-stu-id="0e55e-177">This will ensure that users must be logged in to request a */Dinners/Edit/[id]* URL.</span></span>

<span data-ttu-id="0e55e-178">我們就可以加入程式碼，我們將使用 Dinner.IsHostedBy(username) 協助程式方法來驗證登入的使用者符合 Dinner 主應用程式的編輯方法。</span><span class="sxs-lookup"><span data-stu-id="0e55e-178">We can then add code to our Edit methods that uses the Dinner.IsHostedBy(username) helper method to verify that the logged-in user matches the Dinner host.</span></span> <span data-ttu-id="0e55e-179">如果使用者不是主應用程式，我們將會顯示 「 InvalidOwner 」 檢視，並終止要求。</span><span class="sxs-lookup"><span data-stu-id="0e55e-179">If the user is not the host, we'll display an "InvalidOwner" view and terminate the request.</span></span> <span data-ttu-id="0e55e-180">若要這樣做的程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="0e55e-180">The code to do this looks like below:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

<span data-ttu-id="0e55e-181">我們可以 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [加入]-&gt;檢視功能表命令來建立新的 「 InvalidOwner 」 檢視。</span><span class="sxs-lookup"><span data-stu-id="0e55e-181">We can then right-click on the \Views\Dinners directory and choose the Add-&gt;View menu command to create a new "InvalidOwner" view.</span></span> <span data-ttu-id="0e55e-182">我們將會填入它與下面的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0e55e-182">We'll populate it with the below error message:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

<span data-ttu-id="0e55e-183">然後，現在當使用者嘗試編輯它們並不擁有 dinner，他們會收到錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="0e55e-183">And now when a user attempts to edit a dinner they don't own, they'll get an error message:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

<span data-ttu-id="0e55e-184">我們可以 delete （） 動作方法的重複相同步驟，在我們的控制器，都要鎖定，刪除 Dinners，並確定只有 Dinner 主機可以將它刪除的權限。</span><span class="sxs-lookup"><span data-stu-id="0e55e-184">We can repeat the same steps for the Delete() action methods within our controller to lock down permission to delete Dinners as well, and ensure that only the host of a Dinner can delete it.</span></span>

### <a name="showinghiding-edit-and-delete-links"></a><span data-ttu-id="0e55e-185">顯示/隱藏編輯和刪除連結</span><span class="sxs-lookup"><span data-stu-id="0e55e-185">Showing/Hiding Edit and Delete Links</span></span>

<span data-ttu-id="0e55e-186">我們要連結到我們 DinnersController 類別的編輯和刪除動作方法，從我們的詳細資料 URL:</span><span class="sxs-lookup"><span data-stu-id="0e55e-186">We are linking to the Edit and Delete action method of our DinnersController class from our Details URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

<span data-ttu-id="0e55e-187">我們會在目前顯示的編輯和刪除動作連結，不論訪客能夠詳細資料 URL 是 dinner 的主機。</span><span class="sxs-lookup"><span data-stu-id="0e55e-187">Currently we are showing the Edit and Delete action links regardless of whether the visitor to the details URL is the host of the dinner.</span></span> <span data-ttu-id="0e55e-188">讓我們來變更這使造訪的使用者是否 dinner 的擁有者，只會顯示連結。</span><span class="sxs-lookup"><span data-stu-id="0e55e-188">Let's change this so that the links are only displayed if the visiting user is the owner of the dinner.</span></span>

<span data-ttu-id="0e55e-189">在我們 DinnersController Details() 動作方法會擷取 Dinner 物件，並再將它當做模型物件，傳遞至我們的檢視範本：</span><span class="sxs-lookup"><span data-stu-id="0e55e-189">The Details() action method within our DinnersController retrieves a Dinner object and then passes it as the model object to our view template:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

<span data-ttu-id="0e55e-190">我們可以更新檢視範本有條件地顯示/隱藏的編輯和刪除連結使用 Dinner.IsHostedBy() helper 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0e55e-190">We can update our view template to conditionally show/hide the Edit and Delete links by using the Dinner.IsHostedBy() helper method like below:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a><span data-ttu-id="0e55e-191">後續步驟</span><span class="sxs-lookup"><span data-stu-id="0e55e-191">Next Steps</span></span>

<span data-ttu-id="0e55e-192">現在來看看我們如何啟用 dinners 使用 AJAX 的 RSVP 經過驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="0e55e-192">Let's now look at how we can enable authenticated users to RSVP for dinners using AJAX.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e55e-193">[上一頁](implement-efficient-data-paging.md)
> [下一頁](use-ajax-to-deliver-dynamic-updates.md)</span><span class="sxs-lookup"><span data-stu-id="0e55e-193">[Previous](implement-efficient-data-paging.md)
[Next](use-ajax-to-deliver-dynamic-updates.md)</span></span>
