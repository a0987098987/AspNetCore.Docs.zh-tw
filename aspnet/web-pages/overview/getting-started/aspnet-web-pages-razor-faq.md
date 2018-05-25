---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) 常見問題集 |Microsoft 文件
author: tfitzmac
description: 本文列出關於 ASP.NET Web Pages (Razor) 及 WebMatrix 的一些常見問題集的解答。 使用教學課程 ASP.NET Web Pages 中 (R..的軟體版本
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) 常見問題集
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix 是不建議使用做為整合式的開發環境的 ASP.NET Web Pages。 使用[Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio)或[Visual Studio 程式碼](https://code.visualstudio.com/)。
>
> 本文列出關於 ASP.NET Web Pages (Razor) 及 WebMatrix 的一些常見問題集的解答。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2、 WebMatrix 2 和 Visual Studio 2012。


- [ASP.NET Web 網頁、 ASP.NET Web Form 和 ASP.NET MVC 之間的差異為何？](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [需要為了使用網頁 WebMatrix 嗎？](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [可以使用 ASP.NET Web Form 網頁 」 頁面上的控制項嗎？](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [可以不使用 WebMatrix 部署 ASP.NET Web Pages 站台嗎？](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [是否有使用 WebSecurity helper 支援登入？](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web Pages 支援 HTML5？](#Does_ASP.NET_Web_Pages_support_HTML5)
- [我可以使用 JavaScript 和 jQuery 與網頁嗎？](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [其他資源](#AdditionalResources)

如錯誤和其他問題相關的問題，請參閱[ASP.NET Web Pages (Razor) 疑難排解指南](https://go.microsoft.com/fwlink/?LinkId=253001)。

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>ASP.NET Web 網頁、 ASP.NET Web Form 和 ASP.NET MVC 之間的差異為何？

所有的三種建立動態 web 應用程式的 ASP.NET 技術：

- 動態 （伺服器端） 程式碼與資料庫存取權加入 HTML 網頁和功能的簡單且輕量的語法著重於 ASP.NET Web 網頁。
- ASP.NET Web Form 為基礎的頁面物件模型和傳統的視窗型控制項 （按鈕、 清單等等）。 Web Form 會使用以事件為基礎的模型所熟悉的曾用戶端 (Windows form) 開發人員。
- ASP.NET MVC 會實作 ASP.NET 模型檢視控制器模式。 最重要的是 「 重要性分離 」 （處理、 資料和 UI 層）。

所有的三個架構完全支援，而且繼續 ASP.NET 小組所開發。 若要使用哪一個架構的選擇取決於您的背景的一般情況下，與 ASP.NET 體驗。

特別是 ASP.NET Web Pages 被設計可讓您輕鬆人員已經知道其頁面中加入伺服器處理的 HTML。 學生，愛好者、 一般人是誰程式設計的新手，它會是不錯的選擇。 它也可以是不錯的選擇的開發人員使用非 ASP.NET web 技術的經驗。

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>需要為了使用網頁 WebMatrix 嗎？

否。 WebMatrix 是不建議使用做為整合式的開發環境的 ASP.NET Web Pages。 使用[Visual Studio](program-asp-net-web-pages-in-visual-studio.md)或[Visual Studio 程式碼](https://code.visualstudio.com/)。

如果您不想要使用 Visual Studio 或 Visual Studio 程式碼，您可以安裝使用個別的元件產品[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。 您需要下列產品：

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 （這會安裝的 ASP.NET Web Pages framework）
- IIS Express （web 伺服器）
- Microsoft SQL Server Compact 4.0 （資料庫）

您可以使用文字編輯器來編輯 *.cshtml* (或 *.vbhtml*) 頁面。

管理 SQL Server Compact 資料庫 (*.sdf*檔案) 沒有一項工具會有點困難。 管理 visual Studio containds 工具 *.sdf*資料庫。 您也可以執行許多 SQL Server 管理工作的程式碼中執行 SQL 命令。

若要測試 *.cshtml*頁面而不使用整合式的開發環境 (IDE)，您可以將它們部署到即時伺服器。 (請參閱[可以部署 ASP.NET Web Pages 站台中未使用 WebMatrix？](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>執行 IIS Express 而不要使用的 IDE

如果您安裝 IIS Express 做為 web 伺服器電腦上，您可以使用，以測試網頁。 您可以從命令列執行 IIS Express，並將它與特定通訊埠編號產生關聯。 當您要求然後指定該通訊埠 *.cshtml*瀏覽器中的檔案。

在 Windows 中，以系統管理員權限開啟命令提示字元，並將變更為*C:\Program Files\IIS Express。* (若為 64 位元系統，使用資料夾*C:\Program Files (x86) \IIS Express。)* 然後輸入下列命令，您的網站使用的實際路徑：

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

您可以使用任何其他處理序不保留的連接埠號碼。 （大於 1024年的連接埠號碼是通常可用）。如`path`值，請使用的網站資料夾的路徑，其中 *.cshtml*檔案。

執行這個命令來設定 IIS Express，來服務您的網頁之後，您可以開啟瀏覽器並瀏覽至 *.cshtml*檔案。 使用的 URL 如下所示：

`http://localhost:35896/default.cshtml`

IIS Express 的命令列選項的說明，請輸入`iisexpress.exe /?`在命令列。

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>可以使用 ASP.NET Web Form 網頁 」 頁面上的控制項嗎？

否。 Web Form 控制項，例如[核取方塊](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox)控制項，[驗證控制項](https://msdn.microsoft.com/library/bwd43d0x)，而[GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview)控制只在 Web Form 網頁中的工作 (*.aspx*檔案)。 這些控制項需要 Web Form 網頁架構。

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>可以不使用 WebMatrix 部署 ASP.NET Web Pages 站台嗎？

可以。 您可以手動網站將檔案複製到伺服器 （通常是使用 FTP）。 如果您執行手動複製，您也必須將複製支援 SQL Server Compact （資料庫） 的檔案。 如需詳細資訊，請參閱部落格文章：[部署網頁應用程式沒有工具](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317)。

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>是否有使用 WebSecurity helper 支援登入？

否。 `SimpleMembership`屬於 ASP.NET 網頁的提供者是一個選項。 ASP.NET （您可能會使用 Web Form 中使用） 的一部分的安全性提供者也會提供。 例如，您可以使用表單驗證 ASP.NET Web Pages 中網頁表單中一樣。 其中一個範例說明如何使用表單驗證，請參閱 Microsoft 支援文章[How To Implement Forms-Based 驗證在您的 ASP.NET 應用程式使用 C#.net](https://support.microsoft.com/kb/301240)。 若要下載的簡單範例，請參閱[ASP.NET 版本的 「 登入&amp;密碼](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm)。

如需如何使用 Windows 驗證資訊，請參閱部落格文章[使用 Windows 驗證 ASP.NET Web Pages 中](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298)。

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web Pages 支援 HTML5？

可以。 建立以 ASP.NET Web Pages 頁面 (*.cshtml*或 *.vbhtml*頁面) 基本上是 HTML 頁也包含呈現頁面之前，在伺服器上，執行的程式碼。 只要使用者的瀏覽器支援 HTML5 時，您可以使用 HTML5 中的項目 *.cshtml*或 *.vbhtml*頁面。

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>我可以使用 JavaScript 和 jQuery 與網頁嗎？

當然可以。 建立以 ASP.NET Web Pages 頁面 (*.cshtml*或 *.vbhtml*頁面) 是只使用伺服端程式碼中的 HTML 網頁。 因此，您可以根據標準 HTML 網頁中使用 JavaScript 或 jQuery 的任何項目也您可以 *.cshtml*或 *.vbhtml*頁面。

**入門網站**在 WebMatrix 範本包含 jQuery 程式庫的數字。 如果您使用該範本，來建立站台*指令碼*資料夾包含 jQuery 核心程式庫 (*jquery 1.6.2.js)* 和 jQuery 驗證程式庫 (*jquery.validate.js*等。)。

以下是說明如何使用以 ASP.NET Web Pages jQuery 某些部落格文章：

- [加入至 ASP.NET Web Pages 使用 WebMatrix 的 jQuery 健全度](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/)Rachel Appel 由
- [5 分鐘： WebMatrix + jQuery UI + json + jQuery 範本](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/)Jonas Eriksson 由
- [WebMatrix 和 jQuery 表單](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms)的 Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>其他資源


[ASP.NET Web Pages (Razor) 疑難排解指南](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix 及 ASP.NET 網頁論壇](https://forums.asp.net/1224.aspx/1?WebMatrix)ASP.NET 網站上
