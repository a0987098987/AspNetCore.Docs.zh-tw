---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) 常見問題集 |Microsoft Docs
author: tfitzmac
description: 本文列出一些常見問題解 ASP.NET Web Pages (Razor) 和 WebMatrix。 使用教學課程 ASP.NET Web Pages 中 (R...的軟體版本
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 1f97056c952757ea2d009eaca57d904791e80ca3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814232"
---
<a name="aspnet-web-pages-razor-faq"></a><span data-ttu-id="e9c4c-104">ASP.NET Web Pages (Razor) 常見問題集</span><span class="sxs-lookup"><span data-stu-id="e9c4c-104">ASP.NET Web Pages (Razor) FAQ</span></span>
====================
<span data-ttu-id="e9c4c-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e9c4c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> > [!NOTE] 
> > <span data-ttu-id="e9c4c-106">WebMatrix 不再建議使用整合式的開發環境適用於 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-106">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="e9c4c-107">使用[Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio)或是[Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-107">Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
>
> <span data-ttu-id="e9c4c-108">本文列出一些常見問題解 ASP.NET Web Pages (Razor) 和 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-108">This article lists some frequently asked questions about ASP.NET Web Pages (Razor) and WebMatrix.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e9c4c-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="e9c4c-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e9c4c-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="e9c4c-110">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="e9c4c-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e9c4c-111">Visual Studio 2013</span></span>
> - <span data-ttu-id="e9c4c-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="e9c4c-112">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="e9c4c-113">本教學課程也適用於 ASP.NET Web Pages 2，WebMatrix 2 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-113">This tutorial also works with ASP.NET Web Pages 2, WebMatrix 2, and Visual Studio 2012.</span></span>


- [<span data-ttu-id="e9c4c-114">ASP.NET 網頁、 ASP.NET Web Form 和 ASP.NET MVC 之間的差異為何？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-114">What's the difference between ASP.NET Web Pages, ASP.NET Web Forms, and ASP.NET MVC?</span></span>](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [<span data-ttu-id="e9c4c-115">需要才能與 Web Pages WebMatrix 嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-115">Do I need WebMatrix in order to work with Web Pages?</span></span>](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [<span data-ttu-id="e9c4c-116">我可以使用網頁 」 頁面上的 ASP.NET Web Form 控制項嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-116">Can I use ASP.NET Web Forms controls on a Web Pages page?</span></span>](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [<span data-ttu-id="e9c4c-117">可以不使用 WebMatrix 部署 ASP.NET Web Pages 網站嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-117">Can I deploy an ASP.NET Web Pages site without using WebMatrix?</span></span>](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [<span data-ttu-id="e9c4c-118">我有使用 WebSecurity helper 支援登入嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-118">Do I have to use the WebSecurity helper to support logins?</span></span>](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [<span data-ttu-id="e9c4c-119">ASP.NET Web Pages 支援 HTML5？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-119">Does ASP.NET Web Pages support HTML5?</span></span>](#Does_ASP.NET_Web_Pages_support_HTML5)
- [<span data-ttu-id="e9c4c-120">可以使用 JavaScript 和 jQuery 網頁嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-120">Can I use JavaScript and jQuery with Web Pages?</span></span>](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [<span data-ttu-id="e9c4c-121">其他資源</span><span class="sxs-lookup"><span data-stu-id="e9c4c-121">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="e9c4c-122">錯誤和其他問題相關的問題，請參閱[ASP.NET Web Pages (Razor) 疑難排解指南](https://go.microsoft.com/fwlink/?LinkId=253001)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-122">For questions about errors and other issues, see the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a><span data-ttu-id="e9c4c-123">ASP.NET 網頁、 ASP.NET Web Form 和 ASP.NET MVC 之間的差異為何？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-123">What's the difference between ASP.NET Web Pages, ASP.NET Web Forms, and ASP.NET MVC?</span></span>

<span data-ttu-id="e9c4c-124">所有的三種建立動態 web 應用程式的 ASP.NET 技術：</span><span class="sxs-lookup"><span data-stu-id="e9c4c-124">All three are ASP.NET technologies for creating dynamic web applications:</span></span>

- <span data-ttu-id="e9c4c-125">ASP.NET Web Pages 著重於 HTML 網頁和功能的簡單且輕量語法中加入動態的 （伺服器端） 程式碼和資料庫存取權。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-125">ASP.NET Web Pages focuses on adding dynamic (server-side) code and database access to HTML pages, and features simple and lightweight syntax.</span></span>
- <span data-ttu-id="e9c4c-126">ASP.NET Web Form 為基礎的網頁物件模型和傳統的視窗類型的控制項 （按鈕、 清單等）。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-126">ASP.NET Web Forms is based on a page object model and traditional window-type controls (buttons, lists, etc.).</span></span> <span data-ttu-id="e9c4c-127">Web Form 會使用以事件為基礎的模型以進行用戶端型 (Windows form) 開發用過的人熟悉。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-127">Web Forms uses an event-based model that's familiar to those who've worked with client-based (Windows forms) development.</span></span>
- <span data-ttu-id="e9c4c-128">ASP.NET MVC 會實作 ASP.NET 「 模型-檢視-控制器模式。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-128">ASP.NET MVC implements the model-view-controller pattern for ASP.NET.</span></span> <span data-ttu-id="e9c4c-129">重點在於 「 關注點分離 」 （處理、 資料和 UI 層）。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-129">The emphasis is on "separation of concerns" (processing, data, and UI layers).</span></span>

<span data-ttu-id="e9c4c-130">所有的三個架構完全支援，而且將繼續 ASP.NET 小組所開發。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-130">All three frameworks are fully supported and continue to be developed by the ASP.NET team.</span></span> <span data-ttu-id="e9c4c-131">若要使用哪一個架構的選擇取決於您的背景的一般情況下，與體驗的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-131">In general, the choice of which framework to use depends on your background and experience with ASP.NET.</span></span>

<span data-ttu-id="e9c4c-132">ASP.NET Web Pages 特別設計是讓您輕鬆人士已經知道將伺服器處理新增至其頁面的 HTML。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-132">ASP.NET Web Pages in particular was designed to make it easy for people who already know HTML to add server processing to their pages.</span></span> <span data-ttu-id="e9c4c-133">針對學生、 業餘愛好者一般人是誰程式設計的新手，它就會是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-133">It's a good choice for students, hobbyists, people in general who are new to programming.</span></span> <span data-ttu-id="e9c4c-134">它也可以是不錯的選擇的開發人員使用非 ASP.NET 的 web 技術的經驗。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-134">It can also be a good choice for developers who have experience with non-ASP.NET web technologies.</span></span>

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a><span data-ttu-id="e9c4c-135">需要才能與 Web Pages WebMatrix 嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-135">Do I need WebMatrix in order to work with Web Pages?</span></span>

<span data-ttu-id="e9c4c-136">否。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-136">No.</span></span> <span data-ttu-id="e9c4c-137">WebMatrix 不再建議使用整合式的開發環境適用於 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-137">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="e9c4c-138">使用[Visual Studio](program-asp-net-web-pages-in-visual-studio.md)或是[Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-138">Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>

<span data-ttu-id="e9c4c-139">如果您不想要使用 Visual Studio 或 Visual Studio Code 中，您可以安裝使用個別的元件產品[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-139">If you don't want to use either Visual Studio or Visual Studio Code, you can install the component products individually using [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span> <span data-ttu-id="e9c4c-140">您需要下列產品：</span><span class="sxs-lookup"><span data-stu-id="e9c4c-140">You need the following products:</span></span>

- <span data-ttu-id="e9c4c-141">Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="e9c4c-141">Microsoft .NET Framework 4.5</span></span>
- <span data-ttu-id="e9c4c-142">ASP.NET MVC 5 （可安裝的 ASP.NET Web Pages framework）</span><span class="sxs-lookup"><span data-stu-id="e9c4c-142">ASP.NET MVC 5 (which installs the ASP.NET Web Pages framework as well)</span></span>
- <span data-ttu-id="e9c4c-143">IIS Express （web 伺服器）</span><span class="sxs-lookup"><span data-stu-id="e9c4c-143">IIS Express (the web server)</span></span>
- <span data-ttu-id="e9c4c-144">Microsoft SQL Server Compact 4.0 （資料庫）</span><span class="sxs-lookup"><span data-stu-id="e9c4c-144">Microsoft SQL Server Compact 4.0 (the database)</span></span>

<span data-ttu-id="e9c4c-145">您可以使用文字編輯器來編輯 *.cshtml* (或 *.vbhtml*) 頁面。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-145">You can use a text editor to edit *.cshtml* (or *.vbhtml*) pages.</span></span>

<span data-ttu-id="e9c4c-146">管理 SQL Server Compact 資料庫 (*.sdf*檔案) 沒有一項工具是有點困難了。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-146">Managing SQL Server Compact databases (*.sdf* files) without a tool is a bit harder.</span></span> <span data-ttu-id="e9c4c-147">管理 visual Studio containds 工具 *.sdf*資料庫。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-147">Visual Studio containds tools for managing *.sdf* databases.</span></span> <span data-ttu-id="e9c4c-148">您也可以執行許多 SQL Server 管理工作的程式碼中執行 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-148">You can also run SQL commands in code to perform many SQL Server management tasks.</span></span>

<span data-ttu-id="e9c4c-149">若要測試 *.cshtml*頁面而不需使用整合式的開發環境 (IDE)，您可以將它們部署到即時伺服器。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-149">To test *.cshtml* pages without using an integrated development environment (IDE), you can deploy them to a live server.</span></span> <span data-ttu-id="e9c4c-150">(請參閱[可以部署 ASP.NET Web Pages 網站而不需使用 WebMatrix？](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))</span><span class="sxs-lookup"><span data-stu-id="e9c4c-150">(See [Can I deploy an ASP.NET Web Pages site without using WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))</span></span>

### <a name="running-iis-express-without-using-an-ide"></a><span data-ttu-id="e9c4c-151">執行而不會透過 IDE 的 IIS Express</span><span class="sxs-lookup"><span data-stu-id="e9c4c-151">Running IIS Express without using an IDE</span></span>

<span data-ttu-id="e9c4c-152">如果您安裝 IIS Express 當做 web 伺服器電腦上，您可以使用，若要測試的頁面。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-152">If you install IIS Express on your computer as a web server, you can use that to test the pages.</span></span> <span data-ttu-id="e9c4c-153">您可以執行從命令列的 IIS Express，並將它產生關聯的特定連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-153">You can run IIS Express from the command line and associate it with a specific port number.</span></span> <span data-ttu-id="e9c4c-154">當您要求然後指定該連接埠 *.cshtml*瀏覽器中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-154">You then specify that port when you request *.cshtml* files in your browser.</span></span>

<span data-ttu-id="e9c4c-155">在 Windows 中，以系統管理員權限開啟命令提示字元，並將*C:\Program Files\IIS Express。*</span><span class="sxs-lookup"><span data-stu-id="e9c4c-155">In Windows, open a command prompt with administrator privileges and change to *C:\Program Files\IIS Express.*</span></span> <span data-ttu-id="e9c4c-156">(針對 64 位元系統，使用資料夾*C:\Program Files (x86) \IIS Express。)* 然後輸入下列命令，您的網站使用的實際路徑：</span><span class="sxs-lookup"><span data-stu-id="e9c4c-156">(For 64-bit systems, use the folder *C:\Program Files (x86)\IIS Express.)* Then enter the following command, using the actual path to your site:</span></span>

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

<span data-ttu-id="e9c4c-157">您可以使用任何其他處理序不已保留的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-157">You can use any port number that isn't already reserved by some other process.</span></span> <span data-ttu-id="e9c4c-158">（大於 1024年的連接埠號碼是通常是免費的）。針對`path`值，請使用 [網站] 資料夾的路徑，其中 *.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-158">(Port numbers above 1024 are typically free.) For the `path` value, use the path of the website folder where the *.cshtml* files are.</span></span>

<span data-ttu-id="e9c4c-159">執行此命令來設定 IIS Express，以提供您的網頁之後，您可以開啟瀏覽器並瀏覽至 *.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-159">After you run this command to set up IIS Express to serve your pages, you can open a browser and browse to a *.cshtml* file.</span></span> <span data-ttu-id="e9c4c-160">使用的 URL，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e9c4c-160">Use a URL like the following:</span></span>

`http://localhost:35896/default.cshtml`

<span data-ttu-id="e9c4c-161">使用 IIS Express 的命令列選項的說明，請輸入`iisexpress.exe /?`在命令列。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-161">For help with IIS Express command line options, enter `iisexpress.exe /?` at the command line.</span></span>

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a><span data-ttu-id="e9c4c-162">我可以使用網頁 」 頁面上的 ASP.NET Web Form 控制項嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-162">Can I use ASP.NET Web Forms controls on a Web Pages page?</span></span>

<span data-ttu-id="e9c4c-163">否。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-163">No.</span></span> <span data-ttu-id="e9c4c-164">Web Form 控制項，例如[核取方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox)控制[驗證控制項](https://msdn.microsoft.com/library/bwd43d0x)，而[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview)控制 Web Form 網頁中的工作 (*.aspx*檔案)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-164">Web Forms controls like the [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) control, the [validation controls](https://msdn.microsoft.com/library/bwd43d0x), and the [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) control only work in Web Forms pages (*.aspx* files).</span></span> <span data-ttu-id="e9c4c-165">這些控制項需要 Web Form 網頁架構。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-165">These controls require the Web Forms page framework.</span></span>

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a><span data-ttu-id="e9c4c-166">可以不使用 WebMatrix 部署 ASP.NET Web Pages 網站嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-166">Can I deploy an ASP.NET Web Pages site without using WebMatrix?</span></span>

<span data-ttu-id="e9c4c-167">可以。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-167">Yes.</span></span> <span data-ttu-id="e9c4c-168">您可以手動網站將檔案複製到伺服器 （通常是透過使用 FTP）。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-168">You can manually copy website files to a server (typically by using FTP).</span></span> <span data-ttu-id="e9c4c-169">如果您執行手動複製時，您也必須支援 SQL Server Compact （資料庫） 將檔案複製。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-169">If you perform a manual copy, you also have to copy the files that support SQL Server Compact (the database).</span></span> <span data-ttu-id="e9c4c-170">如需詳細資訊，請參閱部落格文章[部署的 Web Pages 應用程式，而不需要工具](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-170">For details, see the blog entry [Deploying Web Pages applications without a tool](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).</span></span>

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a><span data-ttu-id="e9c4c-171">我有使用 WebSecurity helper 支援登入嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-171">Do I have to use the WebSecurity helper to support logins?</span></span>

<span data-ttu-id="e9c4c-172">否。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-172">No.</span></span> <span data-ttu-id="e9c4c-173">`SimpleMembership`屬於 ASP.NET 網頁的提供者是其中一個選項。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-173">The `SimpleMembership` provider that is part of ASP.NET Web Pages is one option.</span></span> <span data-ttu-id="e9c4c-174">屬於 ASP.NET （，您可能會用來在 Web Form 中使用） 的安全性提供者也會提供。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-174">The security providers that are part of ASP.NET (that you might be used to working with in Web Forms) are also available.</span></span> <span data-ttu-id="e9c4c-175">例如，您可以使用表單驗證 ASP.NET Web Pages 中就如同在 Web Form 中。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-175">For example, you can use forms authentication in ASP.NET Web Pages just as you would in Web Forms.</span></span> <span data-ttu-id="e9c4c-176">如要使用的其中一個範例表單驗證，請參閱 Microsoft 支援服務文章[How To Implement Forms-Based 驗證您的 ASP.NET 應用程式所使用的 C#.NET 中](https://support.microsoft.com/kb/301240)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-176">For one example of how to use forms authentication, see the Microsoft Support article [How To Implement Forms-Based Authentication in Your ASP.NET Application by Using C#.NET](https://support.microsoft.com/kb/301240).</span></span> <span data-ttu-id="e9c4c-177">若要下載的簡單範例，請參閱[ASP.NET 版本的 「 登入&amp;密碼](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-177">To download a simple example, see [ASP.NET version of "Login &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).</span></span>

<span data-ttu-id="e9c4c-178">如需如何使用 Windows 驗證的詳細資訊，請參閱部落格文章[使用 Windows 驗證 ASP.NET Web Pages 中](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-178">For information about how to use Windows authentication, see the blog post [Using Windows authentication in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).</span></span>

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a><span data-ttu-id="e9c4c-179">ASP.NET Web Pages 支援 HTML5？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-179">Does ASP.NET Web Pages support HTML5?</span></span>

<span data-ttu-id="e9c4c-180">可以。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-180">Yes.</span></span> <span data-ttu-id="e9c4c-181">建立以 ASP.NET Web Pages 頁面 (*.cshtml*或 *.vbhtml*頁面) 是本質上也包含之前呈現網頁時，在伺服器上，執行的程式碼的 HTML 頁面。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-181">The pages you create with ASP.NET Web Pages (*.cshtml* or *.vbhtml* pages) are essentially HTML pages that also contain code that runs on the server, before the page is rendered.</span></span> <span data-ttu-id="e9c4c-182">只要使用者的瀏覽器支援 HTML5，您可以使用 HTML5 中的項目 *.cshtml*或是 *.vbhtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-182">As long as the user's browser supports HTML5, you can use HTML5 elements in a *.cshtml* or *.vbhtml* page.</span></span>

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a><span data-ttu-id="e9c4c-183">可以使用 JavaScript 和 jQuery 網頁嗎？</span><span class="sxs-lookup"><span data-stu-id="e9c4c-183">Can I use JavaScript and jQuery with Web Pages?</span></span>

<span data-ttu-id="e9c4c-184">當然可以。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-184">Absolutely.</span></span> <span data-ttu-id="e9c4c-185">建立以 ASP.NET Web Pages 頁面 (*.cshtml*或是 *.vbhtml*頁面) 是只與伺服器程式碼中的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-185">The pages you create with ASP.NET Web Pages (*.cshtml* or *.vbhtml* pages) are just HTML pages with server code in them.</span></span> <span data-ttu-id="e9c4c-186">因此，您可以在正常的 HTML 頁面，藉由使用 JavaScript 或 jQuery 的任何項目可以也在執行 *.cshtml*或是 *.vbhtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-186">Therefore, anything you can do in a normal HTML page by using JavaScript or jQuery you can also do in a *.cshtml* or *.vbhtml* page.</span></span>

<span data-ttu-id="e9c4c-187">**入門網站**在 WebMatrix 中的範本包含 jQuery 程式庫的數目。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-187">The **Starter Site** template in WebMatrix contains a number of jQuery libraries.</span></span> <span data-ttu-id="e9c4c-188">如果您使用該範本，建立站台*指令碼*資料夾包含 jQuery core 程式庫 (*jquery 1.6.2.js)* 和 jQuery 驗證的程式庫 (*於 jquery.validate.js*等等。)。</span><span class="sxs-lookup"><span data-stu-id="e9c4c-188">If you create a site by using that template, the *Scripts* folder contains a jQuery core library (*jquery-1.6.2.js)* and libraries for jQuery validation (*jquery.validate.js*, etc.).</span></span>

<span data-ttu-id="e9c4c-189">以下是一些說明如何使用 jQuery 與 ASP.NET Web Pages 的部落格文章：</span><span class="sxs-lookup"><span data-stu-id="e9c4c-189">Here are some blog posts that illustrate ways to use jQuery with ASP.NET Web Pages:</span></span>

- <span data-ttu-id="e9c4c-190">[新增至 ASP.NET Web Pages 使用 WebMatrix 的 jQuery 吻合度](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/)由 Rachel Appel</span><span class="sxs-lookup"><span data-stu-id="e9c4c-190">[Adding jQuery Goodness to ASP.NET Web Pages using WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) by Rachel Appel</span></span>
- <span data-ttu-id="e9c4c-191">[5 分鐘： WebMatrix + 的 jQuery UI + json + jQuery 範本](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/)由 Jonas Eriksson</span><span class="sxs-lookup"><span data-stu-id="e9c4c-191">[5 min: WebMatrix + jQuery UI + json + jQuery templates](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) by Jonas Eriksson</span></span>
- <span data-ttu-id="e9c4c-192">[WebMatrix 和 jQuery 表單](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms)由 Mike Brind</span><span class="sxs-lookup"><span data-stu-id="e9c4c-192">[WebMatrix And jQuery Forms](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) by Mike Brind</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="e9c4c-193">其他資源</span><span class="sxs-lookup"><span data-stu-id="e9c4c-193">Additional Resources</span></span>


[<span data-ttu-id="e9c4c-194">ASP.NET Web Pages (Razor) 疑難排解指南</span><span class="sxs-lookup"><span data-stu-id="e9c4c-194">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>](https://go.microsoft.com/fwlink/?LinkId=253001)

<span data-ttu-id="e9c4c-195">[WebMatrix 及 ASP.NET 網頁論壇](https://forums.asp.net/1224.aspx/1?WebMatrix)ASP.NET 網站上</span><span class="sxs-lookup"><span data-stu-id="e9c4c-195">[WebMatrix and ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix) on the ASP.NET website</span></span>
