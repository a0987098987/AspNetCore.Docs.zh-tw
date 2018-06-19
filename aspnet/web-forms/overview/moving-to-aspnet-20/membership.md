---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: 成員資格 |Microsoft 文件
author: microsoft
description: ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。 ASP.NET 表單驗證會提供便利的方式來 incorp...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885560"
---
<a name="membership"></a><span data-ttu-id="dd03b-104">成員資格</span><span class="sxs-lookup"><span data-stu-id="dd03b-104">Membership</span></span>
====================
<span data-ttu-id="dd03b-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="dd03b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="dd03b-106">ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="dd03b-106">ASP.NET Membership builds on the success of the Forms authentication model from ASP.NET 1.x.</span></span> <span data-ttu-id="dd03b-107">ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。</span><span class="sxs-lookup"><span data-stu-id="dd03b-107">ASP.NET Forms authentication provides a convenient way to incorporate a login form into your ASP.NET application and validate users against a database or other data store.</span></span>


<span data-ttu-id="dd03b-108">ASP.NET 成員資格的表單驗證模型的成功組建從 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="dd03b-108">ASP.NET Membership builds on the success of the Forms authentication model from ASP.NET 1.x.</span></span> <span data-ttu-id="dd03b-109">ASP.NET 表單驗證會提供便利的方式來併入您的 ASP.NET 應用程式的登入表單，並驗證使用者針對資料庫或其他資料存放區。</span><span class="sxs-lookup"><span data-stu-id="dd03b-109">ASP.NET Forms authentication provides a convenient way to incorporate a login form into your ASP.NET application and validate users against a database or other data store.</span></span> <span data-ttu-id="dd03b-110">FormsAuthentication 類別的成員可使其可處理驗證的 cookie、 檢查有有效的登入、 記錄等出使用者。但是，在 ASP.NET 1.x 應用程式中實作表單驗證可能需要大量的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-110">The members of the FormsAuthentication class make it possible to handle cookies for authentication, check for a valid login, log a user out etc. However, implementing Forms authentication in an ASP.NET 1.x application can require a fair amount of code.</span></span>

<span data-ttu-id="dd03b-111">ASP.NET 2.0 中的成員資格是主要的進階透過使用單獨的表單驗證。</span><span class="sxs-lookup"><span data-stu-id="dd03b-111">Membership in ASP.NET 2.0 is a major advancement over using Forms authentication alone.</span></span> <span data-ttu-id="dd03b-112">（成員資格是最健全的表單驗證，結合時，但使用表單驗證，就不需要）。您很快就會發現，您可以使用 ASP.NET 成員資格和登入控制項在 ASP.NET 2.0 中實作功能強大的成員資格系統，而不需要完全撰寫太多程式碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-112">(Membership is most robust when coupled with Forms authentication, but using Forms authentication is not a requirement.) As you'll soon see, you can use ASP.NET Membership and the login controls in ASP.NET 2.0 to implement a powerful membership system without writing much code at all.</span></span>

## <a name="implementing-membership-in-aspnet-20"></a><span data-ttu-id="dd03b-113">在 ASP.NET 2.0 中實作的成員資格</span><span class="sxs-lookup"><span data-stu-id="dd03b-113">Implementing Membership in ASP.NET 2.0</span></span>

<span data-ttu-id="dd03b-114">成員資格是由下列四個步驟來實作。</span><span class="sxs-lookup"><span data-stu-id="dd03b-114">Membership is implemented by following four steps.</span></span> <span data-ttu-id="dd03b-115">請注意，有許多可一併實作的選擇性組態以及所涉及的子步驟。</span><span class="sxs-lookup"><span data-stu-id="dd03b-115">Keep in mind that there are many sub-steps that are involved as well as optional configuration that can be implemented as well.</span></span> <span data-ttu-id="dd03b-116">這些步驟是為了說明設定成員資格的大圖片。</span><span class="sxs-lookup"><span data-stu-id="dd03b-116">These steps are meant to illustrate the big picture of configuring membership.</span></span>

1. <span data-ttu-id="dd03b-117">建立成員資格資料庫 （如果 SQL Server 會使用做為成員資格存放區）。</span><span class="sxs-lookup"><span data-stu-id="dd03b-117">Create your membership database (if SQL Server is used as the membership store.)</span></span>
2. <span data-ttu-id="dd03b-118">在您的應用程式組態檔中指定的成員資格選項。</span><span class="sxs-lookup"><span data-stu-id="dd03b-118">Specify the membership options in your applications configuration files.</span></span> <span data-ttu-id="dd03b-119">（依預設已啟用的成員資格）。</span><span class="sxs-lookup"><span data-stu-id="dd03b-119">(Membership is enabled by default.)</span></span>
3. <span data-ttu-id="dd03b-120">決定您想要使用的成員資格儲存區的類型。</span><span class="sxs-lookup"><span data-stu-id="dd03b-120">Determine the type of membership store you want to use.</span></span> <span data-ttu-id="dd03b-121">選項如下：</span><span class="sxs-lookup"><span data-stu-id="dd03b-121">Options are:</span></span> 

    - <span data-ttu-id="dd03b-122">Microsoft SQL Server （版本 7.0 或更新版本）</span><span class="sxs-lookup"><span data-stu-id="dd03b-122">Microsoft SQL Server (version 7.0 or later)</span></span>
    - <span data-ttu-id="dd03b-123">Active Directory Store</span><span class="sxs-lookup"><span data-stu-id="dd03b-123">Active Directory Store</span></span>
    - <span data-ttu-id="dd03b-124">自訂成員資格提供者</span><span class="sxs-lookup"><span data-stu-id="dd03b-124">Custom membership provider</span></span>
4. <span data-ttu-id="dd03b-125">設定 ASP.NET 表單驗證的應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd03b-125">Configure the application for ASP.NET Forms authentication.</span></span> <span data-ttu-id="dd03b-126">同樣地，成員資格設計成利用表單驗證，但使用表單驗證，就不需要。</span><span class="sxs-lookup"><span data-stu-id="dd03b-126">Once again, Membership is designed to take advantage of Forms authentication, but using Forms authentication is not a requirement.</span></span>
5. <span data-ttu-id="dd03b-127">定義成員資格的使用者帳戶，並視需要設定角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-127">Define user accounts for membership and configure roles if desired.</span></span>

## <a name="creating-the-membership-database"></a><span data-ttu-id="dd03b-128">建立成員資格資料庫</span><span class="sxs-lookup"><span data-stu-id="dd03b-128">Creating the Membership Database</span></span>

<span data-ttu-id="dd03b-129">如果 youre 使用 SQL Server 7.0 或更新版本作為您的成員資格儲存區中，您可以使用 aspnet\_regsql 公用程式 （可取得最容易從 Visual Studio.NET 2005年命令提示字元） 來設定您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="dd03b-129">If youre using SQL Server 7.0 or later as your membership store, you can use the aspnet\_regsql utility (available most easily from the Visual Studio .NET 2005 Command Prompt) to configure your database.</span></span> <span data-ttu-id="dd03b-130">Aspnet\_regsql 公用程式可用的命令提示字元工具或是透過 GUI 精靈。</span><span class="sxs-lookup"><span data-stu-id="dd03b-130">The aspnet\_regsql utility can be used as a command prompt tool or via a GUI wizard.</span></span> <span data-ttu-id="dd03b-131">精靈方法是設定您的資料庫最簡單的方式。</span><span class="sxs-lookup"><span data-stu-id="dd03b-131">The wizard method is the easiest way to configure your database.</span></span> <span data-ttu-id="dd03b-132">若要存取此精靈，只要執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="dd03b-132">To access the wizard, simply run the following command:</span></span>

`aspnet_regsql W`

<span data-ttu-id="dd03b-133">一旦您執行該命令，您會使用 ASP.NET SQL Server 安裝精靈如下所示。</span><span class="sxs-lookup"><span data-stu-id="dd03b-133">Once you run that command, you will be presented with the ASP.NET SQL Server Setup Wizard as shown below.</span></span>


![](membership/_static/image1.jpg)

<span data-ttu-id="dd03b-134">**圖 1**</span><span class="sxs-lookup"><span data-stu-id="dd03b-134">**Figure 1**</span></span>


<span data-ttu-id="dd03b-135">ASP.NET SQL Server 安裝精靈會在您在精靈中指定的執行個體中建立的網站。</span><span class="sxs-lookup"><span data-stu-id="dd03b-135">The ASP.NET SQL Server Setup Wizard creates the Web site in the instance you specify in the wizard.</span></span> <span data-ttu-id="dd03b-136">不過，ASP.NET 會使用連接字串，machine.config 檔案中連接到您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="dd03b-136">However, ASP.NET will use the connection string in the machine.config file to connect to your database.</span></span> <span data-ttu-id="dd03b-137">根據預設，此連接字串將會為非 SQL Server 2005 執行個體，因此如果您使用的 SQL Server 2000 或 SQL Server 7.0 執行個體，您必須修改 machine.config 檔案中的連接字串。</span><span class="sxs-lookup"><span data-stu-id="dd03b-137">By default, this connection string will point to a SQL Server 2005 instance, so if you are using a SQL Server 2000 or SQL Server 7.0 instance, you will need to modify the connection string in the machine.config file.</span></span> <span data-ttu-id="dd03b-138">此處的連接字串可以是位於：</span><span class="sxs-lookup"><span data-stu-id="dd03b-138">That connection string can be located here:</span></span>

[!code-xml[Main](membership/samples/sample1.xml)]

<span data-ttu-id="dd03b-139">不幸的是，如果您不需要修改連接字串，ASP.NET 將不會提供描述性的錯誤。</span><span class="sxs-lookup"><span data-stu-id="dd03b-139">Unfortunately, if you dont modify the connection string, ASP.NET will not give you a descriptive error.</span></span> <span data-ttu-id="dd03b-140">它只會繼續抱怨指出您尚未建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="dd03b-140">It will just continue to complain saying that you havent created the database.</span></span> <span data-ttu-id="dd03b-141">在上述情況中，我已經修改為指向 我的本機 SQL Server 2000 執行個體的連接字串。</span><span class="sxs-lookup"><span data-stu-id="dd03b-141">In the case above, I have modified the connection string to point to my local SQL Server 2000 instance.</span></span>

## <a name="specifying-configuration-and-adding-users-and-roles"></a><span data-ttu-id="dd03b-142">指定的組態和新增使用者和角色</span><span class="sxs-lookup"><span data-stu-id="dd03b-142">Specifying Configuration and Adding Users and Roles</span></span>

<span data-ttu-id="dd03b-143">設定成員資格的下一個步驟是將所需的資訊加入至應用程式的 web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="dd03b-143">The next step in configuring Membership is to add the necessary information to the web.config file of the application.</span></span> <span data-ttu-id="dd03b-144">在 ASP.NET 中 1.x，修改 web.config 檔案發生有時困難 lowerCamelCase 使用，因為缺少的 Intellisense。</span><span class="sxs-lookup"><span data-stu-id="dd03b-144">In ASP.NET 1.x, modifying the web.config file was sometimes difficult because of the use of lowerCamelCase and the lack of Intellisense.</span></span> <span data-ttu-id="dd03b-145">Visual Studio.NET 2005年，使得工作更為容易，因為 Intellisense 組態檔，但 ASP.NET 2.0 更進一步編輯組態檔中提供 Web 介面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-145">Visual Studio .NET 2005 makes the task much easier with Intellisense for configuration files, but ASP.NET 2.0 goes one step further by providing a Web interface for editing configuration files.</span></span>

<span data-ttu-id="dd03b-146">您可以依序按一下 [ASP.NET 組態] 按鈕，如下所示的 [方案總管] 工具列上，啟動 Web 介面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-146">You can launch the Web interface by clicking the ASP.NET Configuration button on the Solution Explorer toolbar as shown below.</span></span> <span data-ttu-id="dd03b-147">您也可以啟動 Web 介面，透過快顯視窗會顯示插入登入控制項時。</span><span class="sxs-lookup"><span data-stu-id="dd03b-147">You can also launch the Web interface via pop-ups that are displayed when Login controls are inserted.</span></span>


![](membership/_static/image2.jpg)

<span data-ttu-id="dd03b-148">**圖 2**</span><span class="sxs-lookup"><span data-stu-id="dd03b-148">**Figure 2**</span></span>


<span data-ttu-id="dd03b-149">這會啟動 ASP.NET 網站管理工具如下所示。</span><span class="sxs-lookup"><span data-stu-id="dd03b-149">This launches the ASP.NET Web Site Administration Tool shown below.</span></span> <span data-ttu-id="dd03b-150">ASP.NET 網站管理是四個索引標籤介面，以簡化管理應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="dd03b-150">The ASP.NET Web Site Administration is a four-tab interface that makes it easy to manage application settings.</span></span> <span data-ttu-id="dd03b-151">可用的下列索引標籤如下：</span><span class="sxs-lookup"><span data-stu-id="dd03b-151">The following tabs are available:</span></span>

- <span data-ttu-id="dd03b-152">**首頁**</span><span class="sxs-lookup"><span data-stu-id="dd03b-152">**Home**</span></span>
- <span data-ttu-id="dd03b-153">**安全性**設定使用者、 角色和存取。</span><span class="sxs-lookup"><span data-stu-id="dd03b-153">**Security** Configure users, roles, and access.</span></span>
- <span data-ttu-id="dd03b-154">**應用程式**設定應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="dd03b-154">**Application** Configure application settings.</span></span>
- <span data-ttu-id="dd03b-155">**提供者**設定和測試您的應用程式成員資格提供者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-155">**Provider** Configure and test your applications membership provider.</span></span>

<span data-ttu-id="dd03b-156">網站管理工具可讓您輕鬆地建立新的使用者，建立新的角色，以及管理使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-156">The Web Site Administration Tool allows you to easily create new users, create new roles, and to manage users and roles.</span></span> <span data-ttu-id="dd03b-157">Windows 介面中，不提供此功能。</span><span class="sxs-lookup"><span data-stu-id="dd03b-157">This ability is not available in the Windows interface.</span></span> <span data-ttu-id="dd03b-158">Windows 介面可讓您輕鬆地定義授權設定，以及加入、 刪除和管理提供者，不在網站管理工具的功能。</span><span class="sxs-lookup"><span data-stu-id="dd03b-158">The Windows interface allows you to easily define authorization settings and to add, delete, and manage providers, capabilities that are not in the Web Site Administration Tool.</span></span>

<span data-ttu-id="dd03b-159">若要啟動 Windows 介面，開啟 [網際網路資訊服務] 嵌入式管理單元，以滑鼠右鍵按一下您的應用程式，然後選擇屬性。</span><span class="sxs-lookup"><span data-stu-id="dd03b-159">To launch the Windows interface, open the Internet Information Services snap-in, right-click on your application, and choose Properties.</span></span> <span data-ttu-id="dd03b-160">按一下 [ASP.NET] 索引標籤，然後按一下 [編輯設定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="dd03b-160">Click the ASP.NET tab and then click the Edit Configuration button.</span></span> <span data-ttu-id="dd03b-161">（應用程式必須啟用 編輯組態按鈕 ASP.NET 2.0 底下執行。</span><span class="sxs-lookup"><span data-stu-id="dd03b-161">(The application must be running under ASP.NET 2.0 for the Edit Configuration button to be enabled.</span></span> <span data-ttu-id="dd03b-162">您可以設定 ASP.NET 版本在 [ASP.NET] 對話方塊。）[ASP.NET 組態設定] 對話方塊隨即出現如下所示。</span><span class="sxs-lookup"><span data-stu-id="dd03b-162">You can configure the ASP.NET version in the ASP.NET dialog as well.) The ASP.NET Configuration Settings dialog is displayed as shown below.</span></span>


![](membership/_static/image3.jpg)

<span data-ttu-id="dd03b-163">**圖 3**</span><span class="sxs-lookup"><span data-stu-id="dd03b-163">**Figure 3**</span></span>


<span data-ttu-id="dd03b-164">在 [一般] 索引標籤會列出連接字串和應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="dd03b-164">On the General tab, connection strings and application settings are listed.</span></span> <span data-ttu-id="dd03b-165">任何以斜體設定父組態檔 （machine.config 或更高層級的 web.config） 中定義及設定不會以斜體從應用程式組態檔。</span><span class="sxs-lookup"><span data-stu-id="dd03b-165">Any settings in italics are defined in a parent configuration file (either the machine.config or a web.config at a higher level) and settings not in italics are from the applications configuration file.</span></span> <span data-ttu-id="dd03b-166">新增設定，如果已移除或編輯應用程式層級，ASP.NET 會新增、 移除或修改應用程式層級 web.config，而不是從它繼承的組態檔中移除此設定的設定。</span><span class="sxs-lookup"><span data-stu-id="dd03b-166">If a setting is added, removed, or edited at the application level, ASP.NET will add, remove, or modify the setting at the application levels web.config instead of removing the setting from the configuration file from which it is inherited.</span></span>

<span data-ttu-id="dd03b-167">[驗證] 索引標籤如下所示。</span><span class="sxs-lookup"><span data-stu-id="dd03b-167">The Authentication tab is shown below.</span></span> <span data-ttu-id="dd03b-168">這是您將在其中設定成員資格設定。</span><span class="sxs-lookup"><span data-stu-id="dd03b-168">This is where you will configure your membership settings.</span></span> <span data-ttu-id="dd03b-169">表單驗證設定，成員資格提供者，並在此處設定角色提供者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-169">Forms authentication settings, membership providers, and role providers can be configured here.</span></span>


![](membership/_static/image4.jpg)

<span data-ttu-id="dd03b-170">**圖 4**</span><span class="sxs-lookup"><span data-stu-id="dd03b-170">**Figure 4**</span></span>


## <a name="implementing-membership-in-your-application"></a><span data-ttu-id="dd03b-171">在您的應用程式中實作的成員資格</span><span class="sxs-lookup"><span data-stu-id="dd03b-171">Implementing Membership in Your Application</span></span>

<span data-ttu-id="dd03b-172">在您的應用程式中實作 ASP.NET 2.0 的成員資格的最簡單方式是使用提供的登入控制項。</span><span class="sxs-lookup"><span data-stu-id="dd03b-172">The easiest way to implement ASP.NET 2.0 membership in your application is to use the provided Logon controls.</span></span> <span data-ttu-id="dd03b-173">這個方法可讓您不需要撰寫任何程式碼完全實作 ASP.NET 2.0 的成員資格的基本概念。</span><span class="sxs-lookup"><span data-stu-id="dd03b-173">This method allows you to implement the basics of ASP.NET 2.0 membership without writing any code at all.</span></span>

<span data-ttu-id="dd03b-174">下列的登入控制項可在 ASP.NET 2.0:</span><span class="sxs-lookup"><span data-stu-id="dd03b-174">The following Logon controls are available in ASP.NET 2.0:</span></span>

## <a name="login-control"></a><span data-ttu-id="dd03b-175">登入控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-175">Login Control</span></span>

<span data-ttu-id="dd03b-176">登入控制項提供身分登入的成員資格系統的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-176">The Login control provides an interface for someone to log into your membership system.</span></span> <span data-ttu-id="dd03b-177">它提供使用者名稱和密碼 textboxt] 和 [登入按鈕。</span><span class="sxs-lookup"><span data-stu-id="dd03b-177">It provides you with a username and password textboxt and a login button.</span></span> <span data-ttu-id="dd03b-178">例如若要註冊的使用者連結的其他許多常用功能尚未這樣做，核取方塊，可讓使用者自動登入在後續造訪時，密碼提示等等的連結。登入控制項的所有功能都皆透過控制項的屬性可自訂的。</span><span class="sxs-lookup"><span data-stu-id="dd03b-178">Many other common features such as a link to register for people who have not yet done so, a checkbox that allows the user to automatically login on subsequent visits, a link for a password reminder, etc. All features of the Login control are customizable via the properties of the control.</span></span>

<span data-ttu-id="dd03b-179">在 ASP.NET 中 1.x，開發人員必須撰寫牽涉到大量的程式碼來執行查詢，使用表單驗證時。</span><span class="sxs-lookup"><span data-stu-id="dd03b-179">In ASP.NET 1.x, developers had to write a fair amount of code to do a lookup when using Forms authentication.</span></span> <span data-ttu-id="dd03b-180">使用 ASP.NET 2.0 的成員資格，也可以驗證使用者，而不需要撰寫任何程式碼完全。</span><span class="sxs-lookup"><span data-stu-id="dd03b-180">With ASP.NET 2.0 membership, you can validate users without writing any code at all.</span></span> <span data-ttu-id="dd03b-181">ASP.NET 自動會為您執行查詢的使用者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-181">ASP.NET will automatically do the look-up of the user for you.</span></span> <span data-ttu-id="dd03b-182">(如果您在使用登入控制項，而不需要使用 ASP.NET 成員資格，您可以使用**OnAuthenticate**方法以驗證使用者。)</span><span class="sxs-lookup"><span data-stu-id="dd03b-182">(If you are using the Login control without using ASP.NET membership, you can use the **OnAuthenticate** method to validate the user.)</span></span>

## <a name="loginview-control"></a><span data-ttu-id="dd03b-183">LoginView 控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-183">LoginView Control</span></span>

<span data-ttu-id="dd03b-184">LoginView 控制項是根據預設，會提供兩個範本樣板化控制項AnonymousTemplate 和 LoggedInTemplate。</span><span class="sxs-lookup"><span data-stu-id="dd03b-184">The LoginView control is a templated control that provides two templates by default; the AnonymousTemplate and the LoggedInTemplate.</span></span> <span data-ttu-id="dd03b-185">會顯示範本取決於使用者登入您的成員資格系統。</span><span class="sxs-lookup"><span data-stu-id="dd03b-185">The template that is displayed is determined by whether or not the user is logged into your membership system.</span></span> <span data-ttu-id="dd03b-186">這個控制項通常用於顯示當使用者未擁有登入的登入控制項和 LoginStatus 控制項和/或其他登入控制項，當使用者登入。</span><span class="sxs-lookup"><span data-stu-id="dd03b-186">This control is typically used to display a Login control when a user has not yet logged in and a LoginStatus control and/or other login controls when the user has logged in.</span></span> <span data-ttu-id="dd03b-187">如果您使用角色管理 ASP.NET 應用程式中，LoginView 控制項可以顯示特定使用者角色為基礎的範本。</span><span class="sxs-lookup"><span data-stu-id="dd03b-187">If you are using role management in your ASP.NET application, the LoginView control can display a specific template based upon the users role.</span></span> <span data-ttu-id="dd03b-188">（更 asp.net 角色管理會涵蓋更新版本。）</span><span class="sxs-lookup"><span data-stu-id="dd03b-188">(More on ASP.NET role management will be covered later.)</span></span>

## <a name="passwordrecovery-control"></a><span data-ttu-id="dd03b-189">Provider 控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-189">PasswordRecovery Control</span></span>

<span data-ttu-id="dd03b-190">Provider 控制項可讓使用者收到電子郵件，與目前密碼或重設其密碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-190">The PasswordRecovery control allows users to receive an email with his or her current password or reset his or her password.</span></span> <span data-ttu-id="dd03b-191">純文字和加密的密碼可復原，並在電子郵件傳送給使用者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-191">Clear text and encrypted passwords can be recovered and emailed to users.</span></span> <span data-ttu-id="dd03b-192">如果密碼已雜湊，就無法復原。</span><span class="sxs-lookup"><span data-stu-id="dd03b-192">If the password is hashed, it cannot be recovered.</span></span> <span data-ttu-id="dd03b-193">改為使用者就必須執行密碼重設。</span><span class="sxs-lookup"><span data-stu-id="dd03b-193">Instead the user will be required to perform a password reset.</span></span>

## <a name="loginstatus-control"></a><span data-ttu-id="dd03b-194">LoginStatus 控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-194">LoginStatus Control</span></span>

<span data-ttu-id="dd03b-195">LoginStatus 控制項用來顯示目前登入的使用者的登入指標使用者未登入和登出指標。</span><span class="sxs-lookup"><span data-stu-id="dd03b-195">The LoginStatus control is used to display a login indicator to users who are not logged in and a logout indicator to users who are currently logged in.</span></span> <span data-ttu-id="dd03b-196">Request.IsAuthenticated 屬性用來判斷哪些標記，來顯示。</span><span class="sxs-lookup"><span data-stu-id="dd03b-196">The Request.IsAuthenticated property is used to determine which indicator to display.</span></span> <span data-ttu-id="dd03b-197">LoginStatus 控制項所顯示的指標可以是文字 (透過實作**LoginText**和**LogoutText**屬性) 或映像 (透過實作**LoginImageUrl**和**LogoutImageUrl**屬性。)</span><span class="sxs-lookup"><span data-stu-id="dd03b-197">The indicator displayed by the LoginStatus control can be text (implemented via the **LoginText** and **LogoutText** properties) or images (implemented via the **LoginImageUrl** and **LogoutImageUrl** properties.)</span></span>

<span data-ttu-id="dd03b-198">當使用者登出透過 LoginStatus 控制項時，他或她會重新導向至所指定的 URL **LogoutPageUrl**屬性。</span><span class="sxs-lookup"><span data-stu-id="dd03b-198">When a user logs out via the LoginStatus control, he or she is redirected to the URL specified by the **LogoutPageUrl** property.</span></span> <span data-ttu-id="dd03b-199">如果未設定該屬性，會重新整理目前頁面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-199">If that property is not set, the current page is refreshed.</span></span> <span data-ttu-id="dd03b-200">站台可能受到表單驗證，因為目前頁面的重新整理將使用者重新導向至網站的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-200">Since the site is likely protected by Forms authentication, the refresh of the current page will redirect the user to the login page for the site.</span></span>

## <a name="loginname-control"></a><span data-ttu-id="dd03b-201">LoginName 控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-201">LoginName Control</span></span>

<span data-ttu-id="dd03b-202">LoginName 控制項顯示之使用者的目前登入網站的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="dd03b-202">The LoginName control displays the username of the user currently logged into the site.</span></span>

## <a name="createuserwizard-control"></a><span data-ttu-id="dd03b-203">適用於 CreateUserWizard 控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-203">CreateUserWizard Control</span></span>

<span data-ttu-id="dd03b-204">適用於 CreateUserWizard 控制項提供便利的方式來註冊您的成員資格系統使用者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-204">The CreateUserWizard control provides users with a convenient way to register for your membership system.</span></span> <span data-ttu-id="dd03b-205">您可以新增 （實作為的 WizardSteps 集合） 的步驟，透過如下所示的介面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-205">You can add steps (implemented as a collection of WizardSteps) via the interface shown below.</span></span>


![](membership/_static/image5.jpg)

<span data-ttu-id="dd03b-206">**圖 5**</span><span class="sxs-lookup"><span data-stu-id="dd03b-206">**Figure 5**</span></span>


<span data-ttu-id="dd03b-207">適用於 CreateUserWizard 是樣板化控制項衍生自精靈類別，並提供下列範本：</span><span class="sxs-lookup"><span data-stu-id="dd03b-207">The CreateUserWizard is a templated control that derives from the Wizard class and provides the following templates:</span></span>

- <span data-ttu-id="dd03b-208">**HeaderTemplate**這個範本就會控制在精靈的標頭的外觀。</span><span class="sxs-lookup"><span data-stu-id="dd03b-208">**HeaderTemplate** This template controls the appearance of the header of the wizard.</span></span>
- <span data-ttu-id="dd03b-209">**SidebarTemplate**這個範本就會控制在精靈的 [資訊看板] 的外觀。</span><span class="sxs-lookup"><span data-stu-id="dd03b-209">**SidebarTemplate** This template controls the appearance of the sidebar of the wizard.</span></span>
- <span data-ttu-id="dd03b-210">**StartNavigationTemplate**所巡覽的外觀會開始步驟在精靈的這個範本控制項。</span><span class="sxs-lookup"><span data-stu-id="dd03b-210">**StartNavigationTemplate** This template controls the appearance of the navigation are of the wizard at the start step.</span></span>
- <span data-ttu-id="dd03b-211">**StepNavigationTemplate**此範本控制導覽區域中，不在開始或完成步驟時的外觀。</span><span class="sxs-lookup"><span data-stu-id="dd03b-211">**StepNavigationTemplate** This template controls the appearance of the navigation area when not in the start or finish step.</span></span>
- <span data-ttu-id="dd03b-212">**FinishNavigationTemplate**此範本控制導覽區域中，在 [完成] 步驟上的外觀。</span><span class="sxs-lookup"><span data-stu-id="dd03b-212">**FinishNavigationTemplate** This template controls the appearance of the navigation area when on the finish step.</span></span>

<span data-ttu-id="dd03b-213">此外，您將加入至精靈的每個步驟，ASP.NET 將建立自訂範本，其中包含 ContentTemplate 和 CustomNavigationTemplate 該步驟。</span><span class="sxs-lookup"><span data-stu-id="dd03b-213">Additionally, for each step that you add to the Wizard, ASP.NET will create a custom template that contains both a ContentTemplate and a CustomNavigationTemplate for that step.</span></span> <span data-ttu-id="dd03b-214">如需自訂適用於 CreateUserWizard 完整的詳細資訊，請參閱 VS.NET 2005 文件：</span><span class="sxs-lookup"><span data-stu-id="dd03b-214">For full details on customizing the CreateUserWizard, see the VS.NET 2005 documentation:</span></span>

## <a name="changepassword-control"></a><span data-ttu-id="dd03b-215">ChangePassword 控制項</span><span class="sxs-lookup"><span data-stu-id="dd03b-215">ChangePassword Control</span></span>

<span data-ttu-id="dd03b-216">ChangePassword 控制項可讓使用者變更其密碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-216">The ChangePassword control allows users to change his or her password.</span></span> <span data-ttu-id="dd03b-217">如果 DisplayUserName 屬性為 true （它是預設為 false），使用者可以在未登入時變更密碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-217">If the DisplayUserName property is true (it is false by default), the user can change his or her password when they are not logged in.</span></span> <span data-ttu-id="dd03b-218">如果使用者*是*已經登入和 DisplayUserName 屬性為 true 時，使用者將無法變更不會記錄在提供他們知道該使用者的使用者識別碼的另一位使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-218">If the user *is* already logged in and the DisplayUserName property is true, the user will be able to change the password of another user that is not logged in providing they know the user ID of that user.</span></span>

<span data-ttu-id="dd03b-219">請記住，如果您想要能夠變更密碼，而不需要登入的使用者，您必須確定 ChangePassword 控制項顯示的頁面允許匿名存取。</span><span class="sxs-lookup"><span data-stu-id="dd03b-219">Keep in mind that if you want users to be able to change passwords without having to log in, you will need to ensure that the page on which the ChangePassword control is displayed allows anonymous access.</span></span> <span data-ttu-id="dd03b-220">很明顯地，使用者必須提供舊密碼，才能變更其密碼。</span><span class="sxs-lookup"><span data-stu-id="dd03b-220">Obviously, users will have to provide their old password in order to change their password.</span></span>

## <a name="role-management"></a><span data-ttu-id="dd03b-221">角色管理</span><span class="sxs-lookup"><span data-stu-id="dd03b-221">Role Management</span></span>

<span data-ttu-id="dd03b-222">角色管理可讓您將使用者指派至特定角色，然後限制存取特定檔案或資料夾，根據該角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-222">Role management allows you to assign users to a particular role and then restrict access to certain file or folders based on that role.</span></span> <span data-ttu-id="dd03b-223">角色管理也會提供應用程式開發介面，讓您能夠以程式設計方式判斷 abv 角色或判斷特定角色中的所有使用者並據以回應。</span><span class="sxs-lookup"><span data-stu-id="dd03b-223">Role management also provides an API so that you can programmatically determine someones role or determine all users in a particular role and respond accordingly.</span></span>

<span data-ttu-id="dd03b-224">角色管理，就不需要 ASP.NET 成員資格，也不是成員資格才能使用角色管理的需求。</span><span class="sxs-lookup"><span data-stu-id="dd03b-224">Role management is not a requirement in ASP.NET membership, nor is membership a requirement in order to use role management.</span></span> <span data-ttu-id="dd03b-225">不過，這兩個精美補充彼此，而且很可能的開發人員相互搭配使用。</span><span class="sxs-lookup"><span data-stu-id="dd03b-225">However, the two supplement each other nicely and it is likely that developers will use them in conjunction with each other.</span></span>

<span data-ttu-id="dd03b-226">若要啟用應用程式中的角色管理，請在 web.config 檔案中進行以下變更：</span><span class="sxs-lookup"><span data-stu-id="dd03b-226">To enable role management in your application, make the following change in your web.config file:</span></span>

[!code-xml[Main](membership/samples/sample2.xml)]

<span data-ttu-id="dd03b-227">當**cacheRolesInCookie**屬性設為 true，ASP.NET 會快取在 cookie 中用戶端上的使用者角色成員資格。</span><span class="sxs-lookup"><span data-stu-id="dd03b-227">When the **cacheRolesInCookie** attribute is set to true, ASP.NET caches a users role membership in a cookie on the client.</span></span> <span data-ttu-id="dd03b-228">這可讓角色查閱 RoleProvider 呼叫的情況下發生。</span><span class="sxs-lookup"><span data-stu-id="dd03b-228">This allows role lookups to occur without calls into the RoleProvider.</span></span> <span data-ttu-id="dd03b-229">當使用這個屬性時，開發人員都以確保**cookieProtection**屬性將設為全部。</span><span class="sxs-lookup"><span data-stu-id="dd03b-229">When using this attribute, developers are encouraged to ensure that the **cookieProtection** attribute is set to All.</span></span> <span data-ttu-id="dd03b-230">（這是預設值）。這可確保 cookie 資料加密，並可協助確保 cookie 內容未遭到變更。</span><span class="sxs-lookup"><span data-stu-id="dd03b-230">(This is the default setting.) This ensures that the cookie data are encrypted and helps to ensure that the cookies contents havent been altered.</span></span> <span data-ttu-id="dd03b-231">您可以使用網站管理工具新增角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-231">Roles can be added using the Web Site Administration Tool.</span></span> <span data-ttu-id="dd03b-232">它可讓您輕鬆地定義角色、 設定這些角色為基礎的站台的組件的存取，並將使用者指派至角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-232">It allows you to easily define roles, configure access to parts of the site based on those roles, and assign users to roles.</span></span>


![](membership/_static/image6.jpg)

<span data-ttu-id="dd03b-233">**圖 6**</span><span class="sxs-lookup"><span data-stu-id="dd03b-233">**Figure 6**</span></span>


<span data-ttu-id="dd03b-234">如上所示，可以加入新的角色只需輸入角色的名稱，然後按一下 加入角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-234">As shown above, new roles can be added by simply entering the name of the role and then clicking Add Role.</span></span> <span data-ttu-id="dd03b-235">可以管理或現有的角色清單中的適當連結，即可刪除現有的角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-235">Existing roles can be managed or deleted by clicking the appropriate link in the list of existing roles.</span></span>

<span data-ttu-id="dd03b-236">當您管理角色時，您可以新增或移除使用者，如下所示。</span><span class="sxs-lookup"><span data-stu-id="dd03b-236">When you manage a role, you can add or remove users as shown below.</span></span>


![](membership/_static/image7.jpg)

<span data-ttu-id="dd03b-237">**圖 7**</span><span class="sxs-lookup"><span data-stu-id="dd03b-237">**Figure 7**</span></span>


<span data-ttu-id="dd03b-238">藉由檢查使用者在角色中的核取方塊，您可以輕鬆加入使用者至特定角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-238">By checking the User Is In Role checkbox, you can easily add a user to a specific role.</span></span> <span data-ttu-id="dd03b-239">ASP.NET 會自動使用適當的項目更新成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="dd03b-239">ASP.NET will automatically update your membership database with the appropriate entries.</span></span> <span data-ttu-id="dd03b-240">您也會想要設定您的應用程式的存取規則。</span><span class="sxs-lookup"><span data-stu-id="dd03b-240">You will also want to configure access rules for your application.</span></span> <span data-ttu-id="dd03b-241">透過這項操作的 ASP.NET 1.x 開發人員熟悉&lt;授權&gt;web.config 檔案中和該選項中的項目是 ASP.NET 2.0 中仍然可用。</span><span class="sxs-lookup"><span data-stu-id="dd03b-241">ASP.NET 1.x developers are familiar with doing this via the &lt;authorization&gt; element in the web.config file, and that option is still available in ASP.NET 2.0.</span></span> <span data-ttu-id="dd03b-242">不過，其更輕鬆地管理存取規則使用網站管理工具所示下方。</span><span class="sxs-lookup"><span data-stu-id="dd03b-242">However, its easier to manage access rules using the Web Site Administration Tool as shown below.</span></span>


![](membership/_static/image8.jpg)

<span data-ttu-id="dd03b-243">**圖 8**</span><span class="sxs-lookup"><span data-stu-id="dd03b-243">**Figure 8**</span></span>


<span data-ttu-id="dd03b-244">在此情況下，管理資料夾會反白顯示 （其難以看見，因為此工具會反白顯示它以淺灰色） 和系統管理員角色授與存取權。</span><span class="sxs-lookup"><span data-stu-id="dd03b-244">In this case, the Administration folder is highlighted (its difficult to see because the tool highlights it in light gray) and the Administrators role has been granted access.</span></span> <span data-ttu-id="dd03b-245">會拒絕所有其他使用者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-245">All other users are denied.</span></span> <span data-ttu-id="dd03b-246">您可以按一下前端圖示以選取的規則，然後使用往上移和下移按鈕排列的規則。</span><span class="sxs-lookup"><span data-stu-id="dd03b-246">You can click on the head icon to select a rule and then use the Move Up and Move Down buttons to arrange the rules.</span></span> <span data-ttu-id="dd03b-247">搭配 ASP.NET 使用&lt;授權&gt;項目，以它們出現的順序處理規則。</span><span class="sxs-lookup"><span data-stu-id="dd03b-247">As with the ASP.NET &lt;authorization&gt; element, rules are processed in the order in which they appear.</span></span> <span data-ttu-id="dd03b-248">換句話說，如果上述擷取畫面中的規則的順序已顛倒，沒有人必須存取管理資料夾因為 ASP.NET 會遇到的第一個規則的規則，拒絕每個人的資料夾。</span><span class="sxs-lookup"><span data-stu-id="dd03b-248">In other words, if the order of rules in the shot above were reversed, no one would have access to the Administration folder because the first rule that ASP.NET would encounter would be the rule that denies everyone to the folder.</span></span>

<span data-ttu-id="dd03b-249">ASP.NET 2.0 會將 web.config 檔案加入至您指定的存取規則的資料夾。</span><span class="sxs-lookup"><span data-stu-id="dd03b-249">ASP.NET 2.0 adds a web.config file to the folder for which you are specifying an access rule.</span></span> <span data-ttu-id="dd03b-250">透過組態檔或透過網站管理工具，可以編輯存取規則。</span><span class="sxs-lookup"><span data-stu-id="dd03b-250">Access rules can be edited via the configuration file or via the Web Site Administration Tool.</span></span> <span data-ttu-id="dd03b-251">換句話說，網站管理工具是只要透過此組態檔可以編輯好記的環境中的介面。</span><span class="sxs-lookup"><span data-stu-id="dd03b-251">In other words, the Web Site Administration Tool is simply an interface through which the configuration file can be edited in a user-friendly environment.</span></span>

## <a name="using-roles-in-code"></a><span data-ttu-id="dd03b-252">在程式碼中使用角色</span><span class="sxs-lookup"><span data-stu-id="dd03b-252">Using Roles in Code</span></span>

<span data-ttu-id="dd03b-253">角色管理的 API 版本後尚未變更 1.x。</span><span class="sxs-lookup"><span data-stu-id="dd03b-253">The API for role management has not changed since version 1.x.</span></span> <span data-ttu-id="dd03b-254">**IsInRole**方法用來判斷使用者是否為特定角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-254">The **IsInRole** method is used to determine if a user is in a particular role.</span></span>

[!code-csharp[Main](membership/samples/sample3.cs)]

<span data-ttu-id="dd03b-255">ASP.NET 也會建立目前內容的成員身分的 RolePrincipal 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dd03b-255">ASP.NET also creates a RolePrincipal instance as a member of the current context.</span></span> <span data-ttu-id="dd03b-256">RolePrincipal 物件可以用來取得所有的使用者所屬的角色，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dd03b-256">The RolePrincipal object can be used to obtain all of the roles to which the user belongs as follows:</span></span>

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a><span data-ttu-id="dd03b-257">LoginView 控制項搭配使用 RoleGroups</span><span class="sxs-lookup"><span data-stu-id="dd03b-257">Using RoleGroups with the LoginView Control</span></span>

<span data-ttu-id="dd03b-258">既然您已了解角色管理和成員資格時，可讓簡要討論 LoginView 控制項會利用這項功能在 ASP.NET 2.0 的方式。</span><span class="sxs-lookup"><span data-stu-id="dd03b-258">Now that you have an understanding of role management and membership, lets discuss briefly how the LoginView control takes advantage of this capability in ASP.NET 2.0.</span></span> <span data-ttu-id="dd03b-259">如先前所討論，LoginView 控制項是樣板化控制項，其中包含兩個範本的預設值;AnonymousTemplate 和 LoggedInTemplate。</span><span class="sxs-lookup"><span data-stu-id="dd03b-259">As previously discussed, the LoginView control is a templated control that contains two templates by default; the AnonymousTemplate and the LoggedInTemplate.</span></span> <span data-ttu-id="dd03b-260">內 LoginView 工作對話方塊的連結 （如下所示），可讓您編輯 RoleGroups。</span><span class="sxs-lookup"><span data-stu-id="dd03b-260">Within the LoginView Tasks dialog is a link (shown below) that allows you to edit RoleGroups.</span></span>


![](membership/_static/image9.jpg)

<span data-ttu-id="dd03b-261">**圖 9**</span><span class="sxs-lookup"><span data-stu-id="dd03b-261">**Figure 9**</span></span>


<span data-ttu-id="dd03b-262">每個 RoleGroup 物件包含字串的陣列，定義 RoleGroup 適用於哪些角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-262">Each RoleGroup object contains an array of strings that defines which roles that RoleGroup applies to.</span></span> <span data-ttu-id="dd03b-263">若要加入新 RoleGroup LoginView 控制項中，按一下 編輯 RoleGroups 連結。</span><span class="sxs-lookup"><span data-stu-id="dd03b-263">To add a new RoleGroup to the LoginView control, click the Edit RoleGroups link.</span></span> <span data-ttu-id="dd03b-264">在上圖中，您可以查看我已新增為系統管理員的新 RoleGroup。</span><span class="sxs-lookup"><span data-stu-id="dd03b-264">In the image above, you can see that I have added a new RoleGroup for Administrators.</span></span> <span data-ttu-id="dd03b-265">選取該 RoleGroup （RoleGroup[0]) 檢視下拉式清單中的，我可以設定的範本，則只會顯示系統管理員角色的成員。</span><span class="sxs-lookup"><span data-stu-id="dd03b-265">By selecting that RoleGroup (RoleGroup[0]) from the Views dropdown, I can configure a template that will only be displayed to members of the Administrators role.</span></span> <span data-ttu-id="dd03b-266">在下面的影像中，我已新增適用於 「 銷售 」 角色和角色的成員發佈新 RoleGroup。</span><span class="sxs-lookup"><span data-stu-id="dd03b-266">In the image below, I have added a new RoleGroup that applies to members of the Sales role and the Distribution role.</span></span> <span data-ttu-id="dd03b-267">這會將第二個 RoleGroup 加入 LoginView 工作對話方塊中檢視下拉式清單中，任何項目加入至該範本就可以看到在 Sales 或發佈中的任何使用者角色。</span><span class="sxs-lookup"><span data-stu-id="dd03b-267">This adds a second RoleGroup to the Views dropdown in the LoginView Tasks dialog and anything added to that template will be visible by any user in either the Sales or Distribution role.</span></span>


![](membership/_static/image10.jpg)

<span data-ttu-id="dd03b-268">**圖 10**</span><span class="sxs-lookup"><span data-stu-id="dd03b-268">**Figure 10**</span></span>


## <a name="overriding-the-existing-membership-provider"></a><span data-ttu-id="dd03b-269">覆寫現有的成員資格提供者</span><span class="sxs-lookup"><span data-stu-id="dd03b-269">Overriding the Existing Membership Provider</span></span>

<span data-ttu-id="dd03b-270">有幾種方式，您可以擴充功能的 ASP.NET 成員資格。</span><span class="sxs-lookup"><span data-stu-id="dd03b-270">There are a couple of ways that you can extend the functionality of ASP.NET membership.</span></span> <span data-ttu-id="dd03b-271">首先，您可以從它繼承和覆寫其方法很明顯地變更 SqlMembershipProvider 類別的既有的功能。</span><span class="sxs-lookup"><span data-stu-id="dd03b-271">First of all, you can obviously change the existing functionality of the SqlMembershipProvider class by inheriting from it and overriding its methods.</span></span> <span data-ttu-id="dd03b-272">例如，如果您想要實作您自己的功能，建立使用者時，您可以建立您自己的類別繼承從 SqlMembershipProvider，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dd03b-272">For example, if you want to implement your own functionality when users are created, you can create your own class that inherits from SqlMembershipProvider as follows:</span></span>

[!code-csharp[Main](membership/samples/sample5.cs)]

<span data-ttu-id="dd03b-273">如果相反地，您會想要建立您自己的提供者 （例如在存取資料庫中，儲存成員資格資訊），您可以建立自己的提供者。</span><span class="sxs-lookup"><span data-stu-id="dd03b-273">If, on the other hand, you want to create your own provider (to store your membership information in an Access database, for example), you can create your own provider.</span></span>

## <a name="creating-your-own-membership-provider"></a><span data-ttu-id="dd03b-274">建立您自己的成員資格提供者</span><span class="sxs-lookup"><span data-stu-id="dd03b-274">Creating Your Own Membership Provider</span></span>

<span data-ttu-id="dd03b-275">若要建立您自己的成員資格提供者，您必須建立繼承自 MembershipProvider 類別的類別。</span><span class="sxs-lookup"><span data-stu-id="dd03b-275">To create your own membership provider, you will first need to create a class that inherits from the MembershipProvider class.</span></span> <span data-ttu-id="dd03b-276">如果您使用 VB.NET，Visual Studio 2005 會將所有您需要覆寫的方法虛設常式。</span><span class="sxs-lookup"><span data-stu-id="dd03b-276">If you are using VB.NET, Visual Studio 2005 will add the stubs for all of the methods that you need to override.</span></span> <span data-ttu-id="dd03b-277">如果您使用的 C# 中，您將新增虛設常式其最多。</span><span class="sxs-lookup"><span data-stu-id="dd03b-277">If you are using C#, its up to you to add the stubs.</span></span>

<span data-ttu-id="dd03b-278">您必須覆寫下列：</span><span class="sxs-lookup"><span data-stu-id="dd03b-278">You will need to override the following:</span></span>

- <span data-ttu-id="dd03b-279">ApplicationName 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-279">ApplicationName property</span></span>
- <span data-ttu-id="dd03b-280">ChangePassword 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-280">ChangePassword function</span></span>
- <span data-ttu-id="dd03b-281">ChangePasswordQuestionAndAnswer 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-281">ChangePasswordQuestionAndAnswer function</span></span>
- <span data-ttu-id="dd03b-282">CreateUser 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-282">CreateUser function</span></span>
- <span data-ttu-id="dd03b-283">DeleteUser 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-283">DeleteUser function</span></span>
- <span data-ttu-id="dd03b-284">EnablePasswordReset 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-284">EnablePasswordReset property</span></span>
- <span data-ttu-id="dd03b-285">EnablePasswordRetrieval 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-285">EnablePasswordRetrieval property</span></span>
- <span data-ttu-id="dd03b-286">FindUsersByEmail 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-286">FindUsersByEmail function</span></span>
- <span data-ttu-id="dd03b-287">FindUsersByName 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-287">FindUsersByName function</span></span>
- <span data-ttu-id="dd03b-288">GetAllUsers 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-288">GetAllUsers function</span></span>
- <span data-ttu-id="dd03b-289">GetNumberOfUsersOnline 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-289">GetNumberOfUsersOnline function</span></span>
- <span data-ttu-id="dd03b-290">GetPassword 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-290">GetPassword function</span></span>
- <span data-ttu-id="dd03b-291">GetUser 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-291">GetUser function</span></span>
- <span data-ttu-id="dd03b-292">GetUserNameByEmail 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-292">GetUserNameByEmail function</span></span>
- <span data-ttu-id="dd03b-293">MaxInvalidPasswordAttempts 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-293">MaxInvalidPasswordAttempts property</span></span>
- <span data-ttu-id="dd03b-294">MinRequiredNonAlphanumericCharacters 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-294">MinRequiredNonAlphanumericCharacters property</span></span>
- <span data-ttu-id="dd03b-295">MinRequiredPasswordLength 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-295">MinRequiredPasswordLength property</span></span>
- <span data-ttu-id="dd03b-296">PasswordAttemptWindow 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-296">PasswordAttemptWindow property</span></span>
- <span data-ttu-id="dd03b-297">PasswordFormat 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-297">PasswordFormat property</span></span>
- <span data-ttu-id="dd03b-298">PasswordStrengthRegularExpression 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-298">PasswordStrengthRegularExpression property</span></span>
- <span data-ttu-id="dd03b-299">RequiresQuestionAndAnswer 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-299">RequiresQuestionAndAnswer property</span></span>
- <span data-ttu-id="dd03b-300">RequiresUniqueEmail 屬性</span><span class="sxs-lookup"><span data-stu-id="dd03b-300">RequiresUniqueEmail property</span></span>
- <span data-ttu-id="dd03b-301">ResetPassword 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-301">ResetPassword function</span></span>
- <span data-ttu-id="dd03b-302">解除鎖定使用者函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-302">Unlock user function</span></span>
- <span data-ttu-id="dd03b-303">UpdateUser 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-303">UpdateUser function</span></span>
- <span data-ttu-id="dd03b-304">ValidateUser 函式</span><span class="sxs-lookup"><span data-stu-id="dd03b-304">ValidateUser function</span></span>

<span data-ttu-id="dd03b-305">Thats 實作為 C# 開發人員相當多的清單。</span><span class="sxs-lookup"><span data-stu-id="dd03b-305">Thats quite a list to implement as a C# developer.</span></span> <span data-ttu-id="dd03b-306">您可能會發現 VB.NET 中建立類別，任何實作情況下，然後使用.NET 反映或類似的工具來轉換成 C# 程式碼變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="dd03b-306">You may find it easier to create the class in VB.NET without any implementation and then use .NET Reflector or a similar tool to convert the code to C#.</span></span>

<span data-ttu-id="dd03b-307">連接字串和其他屬性應該設定為其預設值的 Initialize 方法中。</span><span class="sxs-lookup"><span data-stu-id="dd03b-307">The connection string and other properties should be set to their defaults in the Initialize method.</span></span> <span data-ttu-id="dd03b-308">（只有當提供者是在執行階段載入時會引發的初始化方法）。Initialize 方法的第二個參數的型別 System.Collections.Specialized.NameValueCollection 而且參考&lt;新增&gt;與您在 web.config 檔案中的自訂提供者相關聯的項目。</span><span class="sxs-lookup"><span data-stu-id="dd03b-308">(The Initialize method is fired when the provider is loaded at runtime.) The second parameter to the Initialize method is of type System.Collections.Specialized.NameValueCollection and is a reference to the &lt;add&gt; element that is associated with your custom provider in the web.config file.</span></span> <span data-ttu-id="dd03b-309">該項目看起來如下：</span><span class="sxs-lookup"><span data-stu-id="dd03b-309">That entry looks like the following:</span></span>

[!code-xml[Main](membership/samples/sample6.xml)]

<span data-ttu-id="dd03b-310">以下是 Initialize 方法的範例。</span><span class="sxs-lookup"><span data-stu-id="dd03b-310">Here is an example of the Initialize method.</span></span>

[!code-csharp[Main](membership/samples/sample7.cs)]

<span data-ttu-id="dd03b-311">若要驗證使用者，當您登入表單送出時，您必須使用 ValidateUser 方法。</span><span class="sxs-lookup"><span data-stu-id="dd03b-311">In order to validate the user when they submit your login form, you will need to use the ValidateUser method.</span></span> <span data-ttu-id="dd03b-312">當使用者按一下登入控制項中的 [登入] 按鈕，就會引發這個方法。</span><span class="sxs-lookup"><span data-stu-id="dd03b-312">This method fires when the user clicks the login button in the Login control.</span></span> <span data-ttu-id="dd03b-313">您會將您的程式碼會執行使用者查閱，在此方法。</span><span class="sxs-lookup"><span data-stu-id="dd03b-313">You will place your code that does the user lookup in this method.</span></span>

<span data-ttu-id="dd03b-314">如您所見，撰寫您自己的成員資格提供者並不困難，並可讓您擴充這項功能強大的 ASP.NET 2.0 功能。</span><span class="sxs-lookup"><span data-stu-id="dd03b-314">As you can see, writing your own membership provider is not difficult and allows you to extend this powerful functionality of ASP.NET 2.0.</span></span>
