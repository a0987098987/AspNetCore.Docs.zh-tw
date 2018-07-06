---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: 成員資格 |Microsoft Docs
author: microsoft
description: ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來 incorp...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: f776ed628e206c06543589767ba364af3c76ae16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818213"
---
<a name="membership"></a><span data-ttu-id="76bae-104">成員資格</span><span class="sxs-lookup"><span data-stu-id="76bae-104">Membership</span></span>
====================
<span data-ttu-id="76bae-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="76bae-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="76bae-106">ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="76bae-106">ASP.NET Membership builds on the success of the Forms authentication model from ASP.NET 1.x.</span></span> <span data-ttu-id="76bae-107">ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。</span><span class="sxs-lookup"><span data-stu-id="76bae-107">ASP.NET Forms authentication provides a convenient way to incorporate a login form into your ASP.NET application and validate users against a database or other data store.</span></span>


<span data-ttu-id="76bae-108">ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="76bae-108">ASP.NET Membership builds on the success of the Forms authentication model from ASP.NET 1.x.</span></span> <span data-ttu-id="76bae-109">ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。</span><span class="sxs-lookup"><span data-stu-id="76bae-109">ASP.NET Forms authentication provides a convenient way to incorporate a login form into your ASP.NET application and validate users against a database or other data store.</span></span> <span data-ttu-id="76bae-110">FormsAuthentication 類別的成員可使您能夠處理驗證的 cookie、 檢查有有效的登入、 記錄將使用者登出等。不過，在 ASP.NET 1.x 應用程式中實作表單驗證，可能需要相當多的程式碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-110">The members of the FormsAuthentication class make it possible to handle cookies for authentication, check for a valid login, log a user out etc. However, implementing Forms authentication in an ASP.NET 1.x application can require a fair amount of code.</span></span>

<span data-ttu-id="76bae-111">在 ASP.NET 2.0 中的成員資格是實現了重大改進，透過使用單獨的表單驗證。</span><span class="sxs-lookup"><span data-stu-id="76bae-111">Membership in ASP.NET 2.0 is a major advancement over using Forms authentication alone.</span></span> <span data-ttu-id="76bae-112">（成員資格是與表單驗證，搭配最強大，但使用表單驗證，就不需要）。您很快就會看到，您可以使用 ASP.NET Membership 與 login 控制項在 ASP.NET 2.0 中實作功能強大的成員資格系統，而不需要完全撰寫太多程式碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-112">(Membership is most robust when coupled with Forms authentication, but using Forms authentication is not a requirement.) As you'll soon see, you can use ASP.NET Membership and the login controls in ASP.NET 2.0 to implement a powerful membership system without writing much code at all.</span></span>

## <a name="implementing-membership-in-aspnet-20"></a><span data-ttu-id="76bae-113">在 ASP.NET 2.0 中實作成員資格</span><span class="sxs-lookup"><span data-stu-id="76bae-113">Implementing Membership in ASP.NET 2.0</span></span>

<span data-ttu-id="76bae-114">成員資格是由下列四個步驟來實作。</span><span class="sxs-lookup"><span data-stu-id="76bae-114">Membership is implemented by following four steps.</span></span> <span data-ttu-id="76bae-115">請記住，有許多可以一併實作的選擇性組態以及所需的子步驟。</span><span class="sxs-lookup"><span data-stu-id="76bae-115">Keep in mind that there are many sub-steps that are involved as well as optional configuration that can be implemented as well.</span></span> <span data-ttu-id="76bae-116">這些步驟是為了說明設定成員資格的全貌。</span><span class="sxs-lookup"><span data-stu-id="76bae-116">These steps are meant to illustrate the big picture of configuring membership.</span></span>

1. <span data-ttu-id="76bae-117">建立成員資格資料庫 （如果 SQL Server 做為成員資格儲存區。）</span><span class="sxs-lookup"><span data-stu-id="76bae-117">Create your membership database (if SQL Server is used as the membership store.)</span></span>
2. <span data-ttu-id="76bae-118">在您的應用程式組態檔中指定的成員資格選項。</span><span class="sxs-lookup"><span data-stu-id="76bae-118">Specify the membership options in your applications configuration files.</span></span> <span data-ttu-id="76bae-119">（預設被啟用的成員資格）。</span><span class="sxs-lookup"><span data-stu-id="76bae-119">(Membership is enabled by default.)</span></span>
3. <span data-ttu-id="76bae-120">決定您想要使用的成員資格存放區的類型。</span><span class="sxs-lookup"><span data-stu-id="76bae-120">Determine the type of membership store you want to use.</span></span> <span data-ttu-id="76bae-121">選項包括：</span><span class="sxs-lookup"><span data-stu-id="76bae-121">Options are:</span></span> 

    - <span data-ttu-id="76bae-122">Microsoft SQL Server （版本 7.0 或更新版本）</span><span class="sxs-lookup"><span data-stu-id="76bae-122">Microsoft SQL Server (version 7.0 or later)</span></span>
    - <span data-ttu-id="76bae-123">Active Directory 存放區</span><span class="sxs-lookup"><span data-stu-id="76bae-123">Active Directory Store</span></span>
    - <span data-ttu-id="76bae-124">自訂成員資格提供者</span><span class="sxs-lookup"><span data-stu-id="76bae-124">Custom membership provider</span></span>
4. <span data-ttu-id="76bae-125">設定 ASP.NET 表單驗證的應用程式。</span><span class="sxs-lookup"><span data-stu-id="76bae-125">Configure the application for ASP.NET Forms authentication.</span></span> <span data-ttu-id="76bae-126">同樣地，成員資格可利用表單驗證，但使用表單驗證，就不需要。</span><span class="sxs-lookup"><span data-stu-id="76bae-126">Once again, Membership is designed to take advantage of Forms authentication, but using Forms authentication is not a requirement.</span></span>
5. <span data-ttu-id="76bae-127">定義成員資格的使用者帳戶及設定角色，如有需要。</span><span class="sxs-lookup"><span data-stu-id="76bae-127">Define user accounts for membership and configure roles if desired.</span></span>

## <a name="creating-the-membership-database"></a><span data-ttu-id="76bae-128">建立成員資格資料庫</span><span class="sxs-lookup"><span data-stu-id="76bae-128">Creating the Membership Database</span></span>

<span data-ttu-id="76bae-129">如果您正在使用 SQL Server 7.0 或更新版本為您的成員資格儲存區中，您可以使用 aspnet\_regsql 公用程式 （可輕易從 Visual Studio.NET 2005年命令提示字元） 來設定您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="76bae-129">If youre using SQL Server 7.0 or later as your membership store, you can use the aspnet\_regsql utility (available most easily from the Visual Studio .NET 2005 Command Prompt) to configure your database.</span></span> <span data-ttu-id="76bae-130">Aspnet\_regsql 公用程式可用的命令提示字元工具或透過經由 GUI 精靈。</span><span class="sxs-lookup"><span data-stu-id="76bae-130">The aspnet\_regsql utility can be used as a command prompt tool or via a GUI wizard.</span></span> <span data-ttu-id="76bae-131">精靈的方法是最簡單的方式來設定您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="76bae-131">The wizard method is the easiest way to configure your database.</span></span> <span data-ttu-id="76bae-132">若要存取精靈，只要執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="76bae-132">To access the wizard, simply run the following command:</span></span>

`aspnet_regsql W`

<span data-ttu-id="76bae-133">一旦您執行該命令時，您會看到使用 ASP.NET SQL Server 安裝精靈，如下所示。</span><span class="sxs-lookup"><span data-stu-id="76bae-133">Once you run that command, you will be presented with the ASP.NET SQL Server Setup Wizard as shown below.</span></span>


![](membership/_static/image1.jpg)

<span data-ttu-id="76bae-134">**圖 1**</span><span class="sxs-lookup"><span data-stu-id="76bae-134">**Figure 1**</span></span>


<span data-ttu-id="76bae-135">ASP.NET SQL Server 安裝精靈會建立網站，您在精靈中指定的執行個體中。</span><span class="sxs-lookup"><span data-stu-id="76bae-135">The ASP.NET SQL Server Setup Wizard creates the Web site in the instance you specify in the wizard.</span></span> <span data-ttu-id="76bae-136">不過，ASP.NET 會使用連接字串在 machine.config 檔案中連接到您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="76bae-136">However, ASP.NET will use the connection string in the machine.config file to connect to your database.</span></span> <span data-ttu-id="76bae-137">根據預設，此連接字串會指向的 SQL Server 2005 執行個體，因此如果您使用的 SQL Server 2000 或 SQL Server 7.0 執行個體，您必須修改 machine.config 檔案中的連接字串。</span><span class="sxs-lookup"><span data-stu-id="76bae-137">By default, this connection string will point to a SQL Server 2005 instance, so if you are using a SQL Server 2000 or SQL Server 7.0 instance, you will need to modify the connection string in the machine.config file.</span></span> <span data-ttu-id="76bae-138">該連接字串可以在以下：</span><span class="sxs-lookup"><span data-stu-id="76bae-138">That connection string can be located here:</span></span>

[!code-xml[Main](membership/samples/sample1.xml)]

<span data-ttu-id="76bae-139">不幸的是，如果您未修改連接字串，ASP.NET 將不會提供描述性的錯誤。</span><span class="sxs-lookup"><span data-stu-id="76bae-139">Unfortunately, if you dont modify the connection string, ASP.NET will not give you a descriptive error.</span></span> <span data-ttu-id="76bae-140">它只會繼續抱怨說，您還沒有建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="76bae-140">It will just continue to complain saying that you havent created the database.</span></span> <span data-ttu-id="76bae-141">在上述案例中，我已經修改連接字串以指向我的本機 SQL Server 2000 執行個體。</span><span class="sxs-lookup"><span data-stu-id="76bae-141">In the case above, I have modified the connection string to point to my local SQL Server 2000 instance.</span></span>

## <a name="specifying-configuration-and-adding-users-and-roles"></a><span data-ttu-id="76bae-142">指定的組態並新增使用者和角色</span><span class="sxs-lookup"><span data-stu-id="76bae-142">Specifying Configuration and Adding Users and Roles</span></span>

<span data-ttu-id="76bae-143">設定成員資格的下一個步驟是將所需的資訊新增至應用程式的 web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="76bae-143">The next step in configuring Membership is to add the necessary information to the web.config file of the application.</span></span> <span data-ttu-id="76bae-144">在 ASP.NET 1.x 中，修改 web.config 檔案難有時由於 lowerCamelCase 使用，其中沒有 Intellisense。</span><span class="sxs-lookup"><span data-stu-id="76bae-144">In ASP.NET 1.x, modifying the web.config file was sometimes difficult because of the use of lowerCamelCase and the lack of Intellisense.</span></span> <span data-ttu-id="76bae-145">Visual Studio.NET 2005年讓工作更容易使用 Intellisense 的組態檔，但 ASP.NET 2.0 則進一步編輯組態檔中提供的 Web 介面。</span><span class="sxs-lookup"><span data-stu-id="76bae-145">Visual Studio .NET 2005 makes the task much easier with Intellisense for configuration files, but ASP.NET 2.0 goes one step further by providing a Web interface for editing configuration files.</span></span>

<span data-ttu-id="76bae-146">您可以按一下 [方案總管] 工具列，如下所示的 [ASP.NET 組態] 按鈕來啟動 Web 介面。</span><span class="sxs-lookup"><span data-stu-id="76bae-146">You can launch the Web interface by clicking the ASP.NET Configuration button on the Solution Explorer toolbar as shown below.</span></span> <span data-ttu-id="76bae-147">您也可以啟動 Web 介面，透過插入登入控制項時顯示快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="76bae-147">You can also launch the Web interface via pop-ups that are displayed when Login controls are inserted.</span></span>


![](membership/_static/image2.jpg)

<span data-ttu-id="76bae-148">**圖 2**</span><span class="sxs-lookup"><span data-stu-id="76bae-148">**Figure 2**</span></span>


<span data-ttu-id="76bae-149">這會啟動 ASP.NET 網站管理工具如下所示。</span><span class="sxs-lookup"><span data-stu-id="76bae-149">This launches the ASP.NET Web Site Administration Tool shown below.</span></span> <span data-ttu-id="76bae-150">ASP.NET 網站管理是四個索引標籤介面，可讓您輕鬆地管理應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="76bae-150">The ASP.NET Web Site Administration is a four-tab interface that makes it easy to manage application settings.</span></span> <span data-ttu-id="76bae-151">可使用下列索引標籤：</span><span class="sxs-lookup"><span data-stu-id="76bae-151">The following tabs are available:</span></span>

- <span data-ttu-id="76bae-152">**Home**</span><span class="sxs-lookup"><span data-stu-id="76bae-152">**Home**</span></span>
- <span data-ttu-id="76bae-153">**安全性**設定使用者、 角色和存取。</span><span class="sxs-lookup"><span data-stu-id="76bae-153">**Security** Configure users, roles, and access.</span></span>
- <span data-ttu-id="76bae-154">**應用程式**設定應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="76bae-154">**Application** Configure application settings.</span></span>
- <span data-ttu-id="76bae-155">**提供者**設定及測試您的應用程式成員資格提供者。</span><span class="sxs-lookup"><span data-stu-id="76bae-155">**Provider** Configure and test your applications membership provider.</span></span>

<span data-ttu-id="76bae-156">Web Site Administration Tool 可讓您輕鬆地建立新的使用者，建立新的角色，以及管理使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-156">The Web Site Administration Tool allows you to easily create new users, create new roles, and to manage users and roles.</span></span> <span data-ttu-id="76bae-157">在 Windows 介面中，不提供此功能。</span><span class="sxs-lookup"><span data-stu-id="76bae-157">This ability is not available in the Windows interface.</span></span> <span data-ttu-id="76bae-158">Windows 介面可讓您輕鬆地定義授權設定，以及新增、 刪除和管理提供者，在 Web Site Administration Tool 所沒有的功能。</span><span class="sxs-lookup"><span data-stu-id="76bae-158">The Windows interface allows you to easily define authorization settings and to add, delete, and manage providers, capabilities that are not in the Web Site Administration Tool.</span></span>

<span data-ttu-id="76bae-159">若要啟動的 Windows 介面，開啟 Internet Information Services 嵌入式管理單元，以滑鼠右鍵按一下您的應用程式，並選擇屬性。</span><span class="sxs-lookup"><span data-stu-id="76bae-159">To launch the Windows interface, open the Internet Information Services snap-in, right-click on your application, and choose Properties.</span></span> <span data-ttu-id="76bae-160">按一下 [ASP.NET] 索引標籤，然後按一下 [編輯設定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="76bae-160">Click the ASP.NET tab and then click the Edit Configuration button.</span></span> <span data-ttu-id="76bae-161">（應用程式必須在啟用 [編輯設定] 按鈕的 ASP.NET 2.0 下執行。</span><span class="sxs-lookup"><span data-stu-id="76bae-161">(The application must be running under ASP.NET 2.0 for the Edit Configuration button to be enabled.</span></span> <span data-ttu-id="76bae-162">您可以設定的 ASP.NET 版本在 [ASP.NET] 對話方塊。）[ASP.NET 組態設定] 對話方塊隨即出現，如下所示。</span><span class="sxs-lookup"><span data-stu-id="76bae-162">You can configure the ASP.NET version in the ASP.NET dialog as well.) The ASP.NET Configuration Settings dialog is displayed as shown below.</span></span>


![](membership/_static/image3.jpg)

<span data-ttu-id="76bae-163">**圖 3**</span><span class="sxs-lookup"><span data-stu-id="76bae-163">**Figure 3**</span></span>


<span data-ttu-id="76bae-164">在 [一般] 索引標籤上會列出連接字串和應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="76bae-164">On the General tab, connection strings and application settings are listed.</span></span> <span data-ttu-id="76bae-165">斜體的任何設定父組態檔 （machine.config 或更高層級的 web.config） 中定義及設定不會以斜體顯示從應用程式組態檔。</span><span class="sxs-lookup"><span data-stu-id="76bae-165">Any settings in italics are defined in a parent configuration file (either the machine.config or a web.config at a higher level) and settings not in italics are from the applications configuration file.</span></span> <span data-ttu-id="76bae-166">如果設定已加入，已移除或編輯應用程式層級，ASP.NET 會新增、 移除或修改在應用程式層級 web.config，而不是從組態檔，從中繼承移除設定的設定。</span><span class="sxs-lookup"><span data-stu-id="76bae-166">If a setting is added, removed, or edited at the application level, ASP.NET will add, remove, or modify the setting at the application levels web.config instead of removing the setting from the configuration file from which it is inherited.</span></span>

<span data-ttu-id="76bae-167">[驗證] 索引標籤如下所示。</span><span class="sxs-lookup"><span data-stu-id="76bae-167">The Authentication tab is shown below.</span></span> <span data-ttu-id="76bae-168">這是您將在其中設定您的成員資格設定。</span><span class="sxs-lookup"><span data-stu-id="76bae-168">This is where you will configure your membership settings.</span></span> <span data-ttu-id="76bae-169">表單驗證設定，成員資格提供者，，和角色提供者可以在此設定。</span><span class="sxs-lookup"><span data-stu-id="76bae-169">Forms authentication settings, membership providers, and role providers can be configured here.</span></span>


![](membership/_static/image4.jpg)

<span data-ttu-id="76bae-170">**圖 4**</span><span class="sxs-lookup"><span data-stu-id="76bae-170">**Figure 4**</span></span>


## <a name="implementing-membership-in-your-application"></a><span data-ttu-id="76bae-171">在 您的應用程式中實作成員資格</span><span class="sxs-lookup"><span data-stu-id="76bae-171">Implementing Membership in Your Application</span></span>

<span data-ttu-id="76bae-172">實作 ASP.NET 2.0 成員資格應用程式中的最簡單方式是使用提供的登入控制項。</span><span class="sxs-lookup"><span data-stu-id="76bae-172">The easiest way to implement ASP.NET 2.0 membership in your application is to use the provided Logon controls.</span></span> <span data-ttu-id="76bae-173">這個方法可讓您不需要撰寫任何程式碼完全實作 ASP.NET 2.0 成員資格的基本概念。</span><span class="sxs-lookup"><span data-stu-id="76bae-173">This method allows you to implement the basics of ASP.NET 2.0 membership without writing any code at all.</span></span>

<span data-ttu-id="76bae-174">ASP.NET 2.0 中提供下列登入控制項︰</span><span class="sxs-lookup"><span data-stu-id="76bae-174">The following Logon controls are available in ASP.NET 2.0:</span></span>

## <a name="login-control"></a><span data-ttu-id="76bae-175">Login 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-175">Login Control</span></span>

<span data-ttu-id="76bae-176">Login 控制項提供有人登入您的成員資格系統的介面。</span><span class="sxs-lookup"><span data-stu-id="76bae-176">The Login control provides an interface for someone to log into your membership system.</span></span> <span data-ttu-id="76bae-177">它會提供您使用者名稱和密碼 textboxt] 和 [登入按鈕。</span><span class="sxs-lookup"><span data-stu-id="76bae-177">It provides you with a username and password textboxt and a login button.</span></span> <span data-ttu-id="76bae-178">許多其他常見功能，例如連結的人註冊尚未這樣做，核取方塊，可讓使用者自動登入在後續造訪時，密碼提示，等等的連結。Login 控制項的所有功能都都可透過控制項的屬性自訂項目。</span><span class="sxs-lookup"><span data-stu-id="76bae-178">Many other common features such as a link to register for people who have not yet done so, a checkbox that allows the user to automatically login on subsequent visits, a link for a password reminder, etc. All features of the Login control are customizable via the properties of the control.</span></span>

<span data-ttu-id="76bae-179">在 ASP.NET 1.x 中，開發人員必須撰寫一大堆程式碼來執行查詢，使用表單驗證時。</span><span class="sxs-lookup"><span data-stu-id="76bae-179">In ASP.NET 1.x, developers had to write a fair amount of code to do a lookup when using Forms authentication.</span></span> <span data-ttu-id="76bae-180">使用 ASP.NET 2.0 成員資格，也可以驗證使用者，而不需要撰寫任何程式碼完全。</span><span class="sxs-lookup"><span data-stu-id="76bae-180">With ASP.NET 2.0 membership, you can validate users without writing any code at all.</span></span> <span data-ttu-id="76bae-181">ASP.NET 會自動為您上進行使用者執行的查閱。</span><span class="sxs-lookup"><span data-stu-id="76bae-181">ASP.NET will automatically do the look-up of the user for you.</span></span> <span data-ttu-id="76bae-182">(如果您使用 Login 控制項，而不使用 ASP.NET 成員資格，您可以使用**OnAuthenticate**方法以驗證使用者。)</span><span class="sxs-lookup"><span data-stu-id="76bae-182">(If you are using the Login control without using ASP.NET membership, you can use the **OnAuthenticate** method to validate the user.)</span></span>

## <a name="loginview-control"></a><span data-ttu-id="76bae-183">LoginView 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-183">LoginView Control</span></span>

<span data-ttu-id="76bae-184">LoginView 控制項是預設的情況下，提供兩個範本的樣板化控制項AnonymousTemplate 和 LoggedInTemplate。</span><span class="sxs-lookup"><span data-stu-id="76bae-184">The LoginView control is a templated control that provides two templates by default; the AnonymousTemplate and the LoggedInTemplate.</span></span> <span data-ttu-id="76bae-185">顯示的範本取決於使用者已登入您的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="76bae-185">The template that is displayed is determined by whether or not the user is logged into your membership system.</span></span> <span data-ttu-id="76bae-186">這個控制項通常用來顯示登入控制項時，使用者有尚未登入和 LoginStatus 控制項和/或其他登入控制項，當使用者已登入。</span><span class="sxs-lookup"><span data-stu-id="76bae-186">This control is typically used to display a Login control when a user has not yet logged in and a LoginStatus control and/or other login controls when the user has logged in.</span></span> <span data-ttu-id="76bae-187">如果您使用角色管理 ASP.NET 應用程式中，LoginView 控制項可以顯示特定的範本，根據使用者角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-187">If you are using role management in your ASP.NET application, the LoginView control can display a specific template based upon the users role.</span></span> <span data-ttu-id="76bae-188">（更多有關 ASP.NET 角色管理後續將說明。）</span><span class="sxs-lookup"><span data-stu-id="76bae-188">(More on ASP.NET role management will be covered later.)</span></span>

## <a name="passwordrecovery-control"></a><span data-ttu-id="76bae-189">Provider 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-189">PasswordRecovery Control</span></span>

<span data-ttu-id="76bae-190">Provider 控制項可讓使用者會收到一封電子郵件與他或她的目前密碼或重設其密碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-190">The PasswordRecovery control allows users to receive an email with his or her current password or reset his or her password.</span></span> <span data-ttu-id="76bae-191">純文字和加密的密碼可復原，並在電子郵件傳送給使用者。</span><span class="sxs-lookup"><span data-stu-id="76bae-191">Clear text and encrypted passwords can be recovered and emailed to users.</span></span> <span data-ttu-id="76bae-192">如果密碼已雜湊，就無法復原。</span><span class="sxs-lookup"><span data-stu-id="76bae-192">If the password is hashed, it cannot be recovered.</span></span> <span data-ttu-id="76bae-193">而是使用者必須執行密碼重設。</span><span class="sxs-lookup"><span data-stu-id="76bae-193">Instead the user will be required to perform a password reset.</span></span>

## <a name="loginstatus-control"></a><span data-ttu-id="76bae-194">LoginStatus 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-194">LoginStatus Control</span></span>

<span data-ttu-id="76bae-195">LoginStatus 控制項用來顯示目前已登入的使用者未登入的使用者登入指標和登出指標。</span><span class="sxs-lookup"><span data-stu-id="76bae-195">The LoginStatus control is used to display a login indicator to users who are not logged in and a logout indicator to users who are currently logged in.</span></span> <span data-ttu-id="76bae-196">Request.IsAuthenticated 屬性用來判斷哪一個標記，來顯示。</span><span class="sxs-lookup"><span data-stu-id="76bae-196">The Request.IsAuthenticated property is used to determine which indicator to display.</span></span> <span data-ttu-id="76bae-197">LoginStatus 控制項所顯示的指標可以是文字 (透過實作**LoginText**並**LogoutText**屬性) 或映像 (可透過實作**LoginImageUrl**並**LogoutImageUrl**屬性。)</span><span class="sxs-lookup"><span data-stu-id="76bae-197">The indicator displayed by the LoginStatus control can be text (implemented via the **LoginText** and **LogoutText** properties) or images (implemented via the **LoginImageUrl** and **LogoutImageUrl** properties.)</span></span>

<span data-ttu-id="76bae-198">當使用者登出透過 LoginStatus 控制項時，他或她會重新導向至所指定的 URL **LogoutPageUrl**屬性。</span><span class="sxs-lookup"><span data-stu-id="76bae-198">When a user logs out via the LoginStatus control, he or she is redirected to the URL specified by the **LogoutPageUrl** property.</span></span> <span data-ttu-id="76bae-199">如果未設定該屬性，會重新整理目前的頁面。</span><span class="sxs-lookup"><span data-stu-id="76bae-199">If that property is not set, the current page is refreshed.</span></span> <span data-ttu-id="76bae-200">由於網站可能受到表單驗證，目前頁面的重新整理會將使用者重新導向網站的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="76bae-200">Since the site is likely protected by Forms authentication, the refresh of the current page will redirect the user to the login page for the site.</span></span>

## <a name="loginname-control"></a><span data-ttu-id="76bae-201">LoginName 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-201">LoginName Control</span></span>

<span data-ttu-id="76bae-202">LoginName 控制項顯示之使用者的目前登入網站的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="76bae-202">The LoginName control displays the username of the user currently logged into the site.</span></span>

## <a name="createuserwizard-control"></a><span data-ttu-id="76bae-203">CreateUserWizard 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-203">CreateUserWizard Control</span></span>

<span data-ttu-id="76bae-204">CreateUserWizard 控制項提供便利的方式來註冊您的成員資格系統使用者。</span><span class="sxs-lookup"><span data-stu-id="76bae-204">The CreateUserWizard control provides users with a convenient way to register for your membership system.</span></span> <span data-ttu-id="76bae-205">您可以透過如下所示的介面新增 （實作為 v kolekci WizardSteps 的集合） 的步驟。</span><span class="sxs-lookup"><span data-stu-id="76bae-205">You can add steps (implemented as a collection of WizardSteps) via the interface shown below.</span></span>


![](membership/_static/image5.jpg)

<span data-ttu-id="76bae-206">**圖 5**</span><span class="sxs-lookup"><span data-stu-id="76bae-206">**Figure 5**</span></span>


<span data-ttu-id="76bae-207">CreateUserWizard 是樣板化控制項衍生自精靈類別，並提供下列範本：</span><span class="sxs-lookup"><span data-stu-id="76bae-207">The CreateUserWizard is a templated control that derives from the Wizard class and provides the following templates:</span></span>

- <span data-ttu-id="76bae-208">**HeaderTemplate**此範本會控制在精靈的標頭的外觀。</span><span class="sxs-lookup"><span data-stu-id="76bae-208">**HeaderTemplate** This template controls the appearance of the header of the wizard.</span></span>
- <span data-ttu-id="76bae-209">**SidebarTemplate**此範本會控制在精靈的資訊看板 的外觀。</span><span class="sxs-lookup"><span data-stu-id="76bae-209">**SidebarTemplate** This template controls the appearance of the sidebar of the wizard.</span></span>
- <span data-ttu-id="76bae-210">**StartNavigationTemplate**導覽的外觀會在開始步驟在精靈的這個樣板控制項。</span><span class="sxs-lookup"><span data-stu-id="76bae-210">**StartNavigationTemplate** This template controls the appearance of the navigation are of the wizard at the start step.</span></span>
- <span data-ttu-id="76bae-211">**StepNavigationTemplate**此範本會控制瀏覽區，不在開始 或 完成 步驟時的外觀。</span><span class="sxs-lookup"><span data-stu-id="76bae-211">**StepNavigationTemplate** This template controls the appearance of the navigation area when not in the start or finish step.</span></span>
- <span data-ttu-id="76bae-212">**FinishNavigationTemplate**此範本會控制瀏覽區，在 [完成] 步驟上的外觀。</span><span class="sxs-lookup"><span data-stu-id="76bae-212">**FinishNavigationTemplate** This template controls the appearance of the navigation area when on the finish step.</span></span>

<span data-ttu-id="76bae-213">此外，您將加入至精靈每個步驟中，ASP.NET 會建立自訂範本，其中包含一個 ContentTemplate 和 CustomNavigationTemplate 該步驟。</span><span class="sxs-lookup"><span data-stu-id="76bae-213">Additionally, for each step that you add to the Wizard, ASP.NET will create a custom template that contains both a ContentTemplate and a CustomNavigationTemplate for that step.</span></span> <span data-ttu-id="76bae-214">如需自訂 CreateUserWizard 的完整詳細資訊，請參閱 VS.NET 2005 文件：</span><span class="sxs-lookup"><span data-stu-id="76bae-214">For full details on customizing the CreateUserWizard, see the VS.NET 2005 documentation:</span></span>

## <a name="changepassword-control"></a><span data-ttu-id="76bae-215">ChangePassword 控制項</span><span class="sxs-lookup"><span data-stu-id="76bae-215">ChangePassword Control</span></span>

<span data-ttu-id="76bae-216">ChangePassword 控制項允許使用者變更其密碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-216">The ChangePassword control allows users to change his or her password.</span></span> <span data-ttu-id="76bae-217">如果 DisplayUserName 屬性為 true （它是預設為 false），使用者可以在未登入時變更他或她的密碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-217">If the DisplayUserName property is true (it is false by default), the user can change his or her password when they are not logged in.</span></span> <span data-ttu-id="76bae-218">如果使用者*是*已經登入和 DisplayUserName 屬性為 true 時，使用者將能夠變更未登入時提供他們知道該使用者的使用者識別碼的另一位使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-218">If the user *is* already logged in and the DisplayUserName property is true, the user will be able to change the password of another user that is not logged in providing they know the user ID of that user.</span></span>

<span data-ttu-id="76bae-219">請記住，如果您想要能夠變更密碼，而不需要登入的使用者，您必須確保 ChangePassword 控制項顯示的頁面允許匿名存取。</span><span class="sxs-lookup"><span data-stu-id="76bae-219">Keep in mind that if you want users to be able to change passwords without having to log in, you will need to ensure that the page on which the ChangePassword control is displayed allows anonymous access.</span></span> <span data-ttu-id="76bae-220">很明顯地，使用者必須提供舊密碼，才能變更其密碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-220">Obviously, users will have to provide their old password in order to change their password.</span></span>

## <a name="role-management"></a><span data-ttu-id="76bae-221">角色管理</span><span class="sxs-lookup"><span data-stu-id="76bae-221">Role Management</span></span>

<span data-ttu-id="76bae-222">角色管理可讓您將使用者指派給特定的角色，並接著限制存取特定檔案或該角色為基礎的資料夾。</span><span class="sxs-lookup"><span data-stu-id="76bae-222">Role management allows you to assign users to a particular role and then restrict access to certain file or folders based on that role.</span></span> <span data-ttu-id="76bae-223">角色管理也會提供 API，讓您能夠以程式設計方式判斷地角色或判斷特定角色中的所有使用者並據以回應。</span><span class="sxs-lookup"><span data-stu-id="76bae-223">Role management also provides an API so that you can programmatically determine someones role or determine all users in a particular role and respond accordingly.</span></span>

<span data-ttu-id="76bae-224">角色管理，就不需要在 ASP.NET 成員資格，也不是成員資格，才能使用角色管理的需求。</span><span class="sxs-lookup"><span data-stu-id="76bae-224">Role management is not a requirement in ASP.NET membership, nor is membership a requirement in order to use role management.</span></span> <span data-ttu-id="76bae-225">不過，這兩個補充彼此得很好，而且很可能，開發人員會互相搭配使用。</span><span class="sxs-lookup"><span data-stu-id="76bae-225">However, the two supplement each other nicely and it is likely that developers will use them in conjunction with each other.</span></span>

<span data-ttu-id="76bae-226">若要啟用您的應用程式中的角色管理，請在 web.config 檔案進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="76bae-226">To enable role management in your application, make the following change in your web.config file:</span></span>

[!code-xml[Main](membership/samples/sample2.xml)]

<span data-ttu-id="76bae-227">當**cacheRolesInCookie**屬性設為 true，ASP.NET 會快取在 cookie 中的用戶端上的使用者角色成員資格。</span><span class="sxs-lookup"><span data-stu-id="76bae-227">When the **cacheRolesInCookie** attribute is set to true, ASP.NET caches a users role membership in a cookie on the client.</span></span> <span data-ttu-id="76bae-228">這可讓角色查閱 RoleProvider 呼叫的情況下發生。</span><span class="sxs-lookup"><span data-stu-id="76bae-228">This allows role lookups to occur without calls into the RoleProvider.</span></span> <span data-ttu-id="76bae-229">當使用這個屬性時，開發人員都以確保**cookieProtection**屬性會設定為 All。</span><span class="sxs-lookup"><span data-stu-id="76bae-229">When using this attribute, developers are encouraged to ensure that the **cookieProtection** attribute is set to All.</span></span> <span data-ttu-id="76bae-230">（這是預設值）。這可確保 cookie 資料會加密，並協助確保 cookie 內容未遭到變更。</span><span class="sxs-lookup"><span data-stu-id="76bae-230">(This is the default setting.) This ensures that the cookie data are encrypted and helps to ensure that the cookies contents havent been altered.</span></span> <span data-ttu-id="76bae-231">可以使用 Web Site Administration Tool 加入角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-231">Roles can be added using the Web Site Administration Tool.</span></span> <span data-ttu-id="76bae-232">它可讓您輕鬆地定義角色、 設定這些角色為基礎的站台的組件的存取，並將使用者指派給角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-232">It allows you to easily define roles, configure access to parts of the site based on those roles, and assign users to roles.</span></span>


![](membership/_static/image6.jpg)

<span data-ttu-id="76bae-233">**圖 6**</span><span class="sxs-lookup"><span data-stu-id="76bae-233">**Figure 6**</span></span>


<span data-ttu-id="76bae-234">如上所示，可以加入新的角色，只要輸入角色的名稱，然後按一下 新增角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-234">As shown above, new roles can be added by simply entering the name of the role and then clicking Add Role.</span></span> <span data-ttu-id="76bae-235">可以管理或在現有的角色清單中的適當連結，即可刪除現有的角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-235">Existing roles can be managed or deleted by clicking the appropriate link in the list of existing roles.</span></span>

<span data-ttu-id="76bae-236">當您管理角色時，您可以新增或移除使用者，如下所示。</span><span class="sxs-lookup"><span data-stu-id="76bae-236">When you manage a role, you can add or remove users as shown below.</span></span>


![](membership/_static/image7.jpg)

<span data-ttu-id="76bae-237">**圖 7**</span><span class="sxs-lookup"><span data-stu-id="76bae-237">**Figure 7**</span></span>


<span data-ttu-id="76bae-238">藉由檢查使用者在角色的核取方塊，您可以輕鬆地將使用者加入特定的角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-238">By checking the User Is In Role checkbox, you can easily add a user to a specific role.</span></span> <span data-ttu-id="76bae-239">ASP.NET 會自動使用適當的項目更新成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="76bae-239">ASP.NET will automatically update your membership database with the appropriate entries.</span></span> <span data-ttu-id="76bae-240">您也會想要設定您的應用程式的存取規則。</span><span class="sxs-lookup"><span data-stu-id="76bae-240">You will also want to configure access rules for your application.</span></span> <span data-ttu-id="76bae-241">透過這項操作的 ASP.NET 1.x 開發人員熟悉&lt;授權&gt;在 web.config 檔案中，以及該選項的項目是 ASP.NET 2.0 中仍然可用。</span><span class="sxs-lookup"><span data-stu-id="76bae-241">ASP.NET 1.x developers are familiar with doing this via the &lt;authorization&gt; element in the web.config file, and that option is still available in ASP.NET 2.0.</span></span> <span data-ttu-id="76bae-242">不過，其更容易管理的存取規則使用 Web Site Administration Tool 所示下方。</span><span class="sxs-lookup"><span data-stu-id="76bae-242">However, its easier to manage access rules using the Web Site Administration Tool as shown below.</span></span>


![](membership/_static/image8.jpg)

<span data-ttu-id="76bae-243">**圖 8**</span><span class="sxs-lookup"><span data-stu-id="76bae-243">**Figure 8**</span></span>


<span data-ttu-id="76bae-244">在此情況下，管理資料夾會反白顯示 （難以查看，因為此工具會反白顯示它以淺灰色） 和系統管理員角色授與存取權。</span><span class="sxs-lookup"><span data-stu-id="76bae-244">In this case, the Administration folder is highlighted (its difficult to see because the tool highlights it in light gray) and the Administrators role has been granted access.</span></span> <span data-ttu-id="76bae-245">會拒絕所有其他使用者。</span><span class="sxs-lookup"><span data-stu-id="76bae-245">All other users are denied.</span></span> <span data-ttu-id="76bae-246">您可以按一下標頭的圖示，以選取的規則，然後使用往上移和下移按鈕排列的規則。</span><span class="sxs-lookup"><span data-stu-id="76bae-246">You can click on the head icon to select a rule and then use the Move Up and Move Down buttons to arrange the rules.</span></span> <span data-ttu-id="76bae-247">使用 ASP.NET&lt;授權&gt;項目，以其出現的順序處理規則。</span><span class="sxs-lookup"><span data-stu-id="76bae-247">As with the ASP.NET &lt;authorization&gt; element, rules are processed in the order in which they appear.</span></span> <span data-ttu-id="76bae-248">換句話說，如果在上述螢幕擷取規則的順序反轉，沒有人必須存取管理資料夾因為 ASP.NET 會遇到的第一個規則的規則，拒絕每個人的資料夾。</span><span class="sxs-lookup"><span data-stu-id="76bae-248">In other words, if the order of rules in the shot above were reversed, no one would have access to the Administration folder because the first rule that ASP.NET would encounter would be the rule that denies everyone to the folder.</span></span>

<span data-ttu-id="76bae-249">ASP.NET 2.0 會將您指定的存取規則的資料夾中的 web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="76bae-249">ASP.NET 2.0 adds a web.config file to the folder for which you are specifying an access rule.</span></span> <span data-ttu-id="76bae-250">透過組態檔或透過 Web Site Administration Tool，就可以編輯的存取規則。</span><span class="sxs-lookup"><span data-stu-id="76bae-250">Access rules can be edited via the configuration file or via the Web Site Administration Tool.</span></span> <span data-ttu-id="76bae-251">換句話說，Web Site Administration Tool 是只可以在方便使用的環境中編輯組態檔透過此介面。</span><span class="sxs-lookup"><span data-stu-id="76bae-251">In other words, the Web Site Administration Tool is simply an interface through which the configuration file can be edited in a user-friendly environment.</span></span>

## <a name="using-roles-in-code"></a><span data-ttu-id="76bae-252">在程式碼中使用角色</span><span class="sxs-lookup"><span data-stu-id="76bae-252">Using Roles in Code</span></span>

<span data-ttu-id="76bae-253">角色管理的 API 版本之後並未變更 1.x。</span><span class="sxs-lookup"><span data-stu-id="76bae-253">The API for role management has not changed since version 1.x.</span></span> <span data-ttu-id="76bae-254">**IsInRole**方法用來判斷使用者是否具有特定角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-254">The **IsInRole** method is used to determine if a user is in a particular role.</span></span>

[!code-csharp[Main](membership/samples/sample3.cs)]

<span data-ttu-id="76bae-255">ASP.NET 也會建立目前內容的成員身分的 RolePrincipal 執行個體。</span><span class="sxs-lookup"><span data-stu-id="76bae-255">ASP.NET also creates a RolePrincipal instance as a member of the current context.</span></span> <span data-ttu-id="76bae-256">RolePrincipal 物件可用來取得所有角色給使用者所屬，如下所示：</span><span class="sxs-lookup"><span data-stu-id="76bae-256">The RolePrincipal object can be used to obtain all of the roles to which the user belongs as follows:</span></span>

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a><span data-ttu-id="76bae-257">LoginView 控制項搭配使用 kolekci RoleGroups</span><span class="sxs-lookup"><span data-stu-id="76bae-257">Using RoleGroups with the LoginView Control</span></span>

<span data-ttu-id="76bae-258">既然您已了解角色管理和成員資格，可讓簡短討論 LoginView 控制項如何在 ASP.NET 2.0 會充分利用這項功能。</span><span class="sxs-lookup"><span data-stu-id="76bae-258">Now that you have an understanding of role management and membership, lets discuss briefly how the LoginView control takes advantage of this capability in ASP.NET 2.0.</span></span> <span data-ttu-id="76bae-259">如先前所述，LoginView 控制項是包含兩個範本預設情況下，樣板化控制項AnonymousTemplate 和 LoggedInTemplate。</span><span class="sxs-lookup"><span data-stu-id="76bae-259">As previously discussed, the LoginView control is a templated control that contains two templates by default; the AnonymousTemplate and the LoggedInTemplate.</span></span> <span data-ttu-id="76bae-260">內 LoginView 工作對話方塊是連結 （如下所示），可讓您 upravit kolekci RoleGroups。</span><span class="sxs-lookup"><span data-stu-id="76bae-260">Within the LoginView Tasks dialog is a link (shown below) that allows you to edit RoleGroups.</span></span>


![](membership/_static/image9.jpg)

<span data-ttu-id="76bae-261">**圖 9**</span><span class="sxs-lookup"><span data-stu-id="76bae-261">**Figure 9**</span></span>


<span data-ttu-id="76bae-262">每個 RoleGroup 物件包含字串陣列，定義 RoleGroup 適用於哪些角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-262">Each RoleGroup object contains an array of strings that defines which roles that RoleGroup applies to.</span></span> <span data-ttu-id="76bae-263">若要加入新的 RoleGroup LoginView 控制項，按一下 編輯 kolekci RoleGroups 連結。</span><span class="sxs-lookup"><span data-stu-id="76bae-263">To add a new RoleGroup to the LoginView control, click the Edit RoleGroups link.</span></span> <span data-ttu-id="76bae-264">在上圖中，您可以看到我已新增新的 RoleGroup 系統管理員。</span><span class="sxs-lookup"><span data-stu-id="76bae-264">In the image above, you can see that I have added a new RoleGroup for Administrators.</span></span> <span data-ttu-id="76bae-265">選取該 RoleGroup （RoleGroup[0]) 檢視下拉式清單中的，我可以設定的範本，則只會顯示系統管理員角色的成員。</span><span class="sxs-lookup"><span data-stu-id="76bae-265">By selecting that RoleGroup (RoleGroup[0]) from the Views dropdown, I can configure a template that will only be displayed to members of the Administrators role.</span></span> <span data-ttu-id="76bae-266">在下圖中，我已新增新的 RoleGroup 適用於 「 銷售 」 角色和發佈角色的成員。</span><span class="sxs-lookup"><span data-stu-id="76bae-266">In the image below, I have added a new RoleGroup that applies to members of the Sales role and the Distribution role.</span></span> <span data-ttu-id="76bae-267">這會將第二個 RoleGroup 加入 LoginView 工作對話方塊中的 檢視 下拉式清單和任何項目加入至該範本會顯示 Sales 或 通訊群組中的任何使用者角色。</span><span class="sxs-lookup"><span data-stu-id="76bae-267">This adds a second RoleGroup to the Views dropdown in the LoginView Tasks dialog and anything added to that template will be visible by any user in either the Sales or Distribution role.</span></span>


![](membership/_static/image10.jpg)

<span data-ttu-id="76bae-268">**圖 10**</span><span class="sxs-lookup"><span data-stu-id="76bae-268">**Figure 10**</span></span>


## <a name="overriding-the-existing-membership-provider"></a><span data-ttu-id="76bae-269">覆寫現有的成員資格提供者</span><span class="sxs-lookup"><span data-stu-id="76bae-269">Overriding the Existing Membership Provider</span></span>

<span data-ttu-id="76bae-270">有幾種方式，您可以擴充功能的 ASP.NET 成員資格。</span><span class="sxs-lookup"><span data-stu-id="76bae-270">There are a couple of ways that you can extend the functionality of ASP.NET membership.</span></span> <span data-ttu-id="76bae-271">首先，您可以從它繼承並覆寫其方法很明顯地變更 SqlMembershipProvider 類別的現有功能。</span><span class="sxs-lookup"><span data-stu-id="76bae-271">First of all, you can obviously change the existing functionality of the SqlMembershipProvider class by inheriting from it and overriding its methods.</span></span> <span data-ttu-id="76bae-272">比方說，如果您想要實作您自己的功能，建立使用者時，您可以建立您自己的類別繼承從 SqlMembershipProvider，如下所示：</span><span class="sxs-lookup"><span data-stu-id="76bae-272">For example, if you want to implement your own functionality when users are created, you can create your own class that inherits from SqlMembershipProvider as follows:</span></span>

[!code-csharp[Main](membership/samples/sample5.cs)]

<span data-ttu-id="76bae-273">如果相反地，您會想要建立您自己的提供者 （例如將成員資格資訊儲存在 Access 資料庫），您可以建立自己的提供者。</span><span class="sxs-lookup"><span data-stu-id="76bae-273">If, on the other hand, you want to create your own provider (to store your membership information in an Access database, for example), you can create your own provider.</span></span>

## <a name="creating-your-own-membership-provider"></a><span data-ttu-id="76bae-274">建立您自己的成員資格提供者</span><span class="sxs-lookup"><span data-stu-id="76bae-274">Creating Your Own Membership Provider</span></span>

<span data-ttu-id="76bae-275">若要建立您自己的成員資格提供者，您必須建立繼承自 MembershipProvider 類別的類別。</span><span class="sxs-lookup"><span data-stu-id="76bae-275">To create your own membership provider, you will first need to create a class that inherits from the MembershipProvider class.</span></span> <span data-ttu-id="76bae-276">如果您使用的 VB.NET，Visual Studio 2005 會將所有您需要覆寫的方法虛設常式。</span><span class="sxs-lookup"><span data-stu-id="76bae-276">If you are using VB.NET, Visual Studio 2005 will add the stubs for all of the methods that you need to override.</span></span> <span data-ttu-id="76bae-277">如果您使用的 C# 中，您將新增虛設常式。</span><span class="sxs-lookup"><span data-stu-id="76bae-277">If you are using C#, its up to you to add the stubs.</span></span>

<span data-ttu-id="76bae-278">您必須覆寫下列：</span><span class="sxs-lookup"><span data-stu-id="76bae-278">You will need to override the following:</span></span>

- <span data-ttu-id="76bae-279">ApplicationName 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-279">ApplicationName property</span></span>
- <span data-ttu-id="76bae-280">ChangePassword 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-280">ChangePassword function</span></span>
- <span data-ttu-id="76bae-281">ChangePasswordQuestionAndAnswer 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-281">ChangePasswordQuestionAndAnswer function</span></span>
- <span data-ttu-id="76bae-282">CreateUser 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-282">CreateUser function</span></span>
- <span data-ttu-id="76bae-283">DeleteUser 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-283">DeleteUser function</span></span>
- <span data-ttu-id="76bae-284">EnablePasswordReset 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-284">EnablePasswordReset property</span></span>
- <span data-ttu-id="76bae-285">EnablePasswordRetrieval 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-285">EnablePasswordRetrieval property</span></span>
- <span data-ttu-id="76bae-286">FindUsersByEmail 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-286">FindUsersByEmail function</span></span>
- <span data-ttu-id="76bae-287">FindUsersByName 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-287">FindUsersByName function</span></span>
- <span data-ttu-id="76bae-288">GetAllUsers 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-288">GetAllUsers function</span></span>
- <span data-ttu-id="76bae-289">GetNumberOfUsersOnline 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-289">GetNumberOfUsersOnline function</span></span>
- <span data-ttu-id="76bae-290">GetPassword 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-290">GetPassword function</span></span>
- <span data-ttu-id="76bae-291">GetUser 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-291">GetUser function</span></span>
- <span data-ttu-id="76bae-292">GetUserNameByEmail 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-292">GetUserNameByEmail function</span></span>
- <span data-ttu-id="76bae-293">MaxInvalidPasswordAttempts 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-293">MaxInvalidPasswordAttempts property</span></span>
- <span data-ttu-id="76bae-294">MinRequiredNonAlphanumericCharacters 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-294">MinRequiredNonAlphanumericCharacters property</span></span>
- <span data-ttu-id="76bae-295">MinRequiredPasswordLength 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-295">MinRequiredPasswordLength property</span></span>
- <span data-ttu-id="76bae-296">PasswordAttemptWindow 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-296">PasswordAttemptWindow property</span></span>
- <span data-ttu-id="76bae-297">PasswordFormat 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-297">PasswordFormat property</span></span>
- <span data-ttu-id="76bae-298">PasswordStrengthRegularExpression 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-298">PasswordStrengthRegularExpression property</span></span>
- <span data-ttu-id="76bae-299">RequiresQuestionAndAnswer 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-299">RequiresQuestionAndAnswer property</span></span>
- <span data-ttu-id="76bae-300">RequiresUniqueEmail 屬性</span><span class="sxs-lookup"><span data-stu-id="76bae-300">RequiresUniqueEmail property</span></span>
- <span data-ttu-id="76bae-301">ResetPassword 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-301">ResetPassword function</span></span>
- <span data-ttu-id="76bae-302">解除鎖定使用者函式</span><span class="sxs-lookup"><span data-stu-id="76bae-302">Unlock user function</span></span>
- <span data-ttu-id="76bae-303">UpdateUser 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-303">UpdateUser function</span></span>
- <span data-ttu-id="76bae-304">ValidateUser 函式</span><span class="sxs-lookup"><span data-stu-id="76bae-304">ValidateUser function</span></span>

<span data-ttu-id="76bae-305">Thats 身為 C# 開發人員實作相當的清單。</span><span class="sxs-lookup"><span data-stu-id="76bae-305">Thats quite a list to implement as a C# developer.</span></span> <span data-ttu-id="76bae-306">您可能會發現它在 VB.NET 中建立類別，而不需要任何實作，然後使用.NET 反射程式或類似工具來轉換成 C# 的程式碼變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="76bae-306">You may find it easier to create the class in VB.NET without any implementation and then use .NET Reflector or a similar tool to convert the code to C#.</span></span>

<span data-ttu-id="76bae-307">連接字串和其他屬性應該設定為其 Initialize 方法中的預設值。</span><span class="sxs-lookup"><span data-stu-id="76bae-307">The connection string and other properties should be set to their defaults in the Initialize method.</span></span> <span data-ttu-id="76bae-308">（只有當提供者會在執行階段載入時會引發的 Initialize 方法）。Initialize 方法的第二個參數是型別 System.Collections.Specialized.NameValueCollection，且具備的參考&lt;新增&gt;與您的 web.config 檔案中的自訂提供者相關聯的項目。</span><span class="sxs-lookup"><span data-stu-id="76bae-308">(The Initialize method is fired when the provider is loaded at runtime.) The second parameter to the Initialize method is of type System.Collections.Specialized.NameValueCollection and is a reference to the &lt;add&gt; element that is associated with your custom provider in the web.config file.</span></span> <span data-ttu-id="76bae-309">該項目看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="76bae-309">That entry looks like the following:</span></span>

[!code-xml[Main](membership/samples/sample6.xml)]

<span data-ttu-id="76bae-310">Initialize 方法的範例如下。</span><span class="sxs-lookup"><span data-stu-id="76bae-310">Here is an example of the Initialize method.</span></span>

[!code-csharp[Main](membership/samples/sample7.cs)]

<span data-ttu-id="76bae-311">若要驗證使用者，當您登入表單送出時，您必須使用 ValidateUser 的方法。</span><span class="sxs-lookup"><span data-stu-id="76bae-311">In order to validate the user when they submit your login form, you will need to use the ValidateUser method.</span></span> <span data-ttu-id="76bae-312">當使用者按一下登入控制項中的 [登入] 按鈕，就會引發這個方法。</span><span class="sxs-lookup"><span data-stu-id="76bae-312">This method fires when the user clicks the login button in the Login control.</span></span> <span data-ttu-id="76bae-313">您會將您會執行使用者查閱，在此方法的程式碼。</span><span class="sxs-lookup"><span data-stu-id="76bae-313">You will place your code that does the user lookup in this method.</span></span>

<span data-ttu-id="76bae-314">如您所見，撰寫您自己的成員資格提供者並不困難，並可讓您擴充這個強大的功能，ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="76bae-314">As you can see, writing your own membership provider is not difficult and allows you to extend this powerful functionality of ASP.NET 2.0.</span></span>
