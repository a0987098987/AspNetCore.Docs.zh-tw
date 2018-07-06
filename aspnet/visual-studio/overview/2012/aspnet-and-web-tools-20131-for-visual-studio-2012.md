---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: 適用於 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 版本資訊 |Microsoft Docs
author: microsoft
description: 本文件說明版本的 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012。
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829097"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>適用於 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 版本資訊
====================
by [Microsoft](https://github.com/microsoft)

> 本文件說明版本的 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012。


## <a name="contents"></a>內容

- [安裝注意事項](#install)
- [軟體需求](#requirements)
- 在 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 中的新功能

    - [啟動程序](#bootstrap)
    - [範本](#templates)

        - [ASP.NET MVC 5 範本](#mvc5template)
        - [ASP.NET Web API 2 範本](#apitemplate)
        - [項目範本](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET Scaffolding](#scaffold)
    - [Razor 編輯器](#razor)
    - [NuGet 2.7](#nuget)
- 已知的問題和重大變更

    - [ASP.NET Scaffolding](#issuescaffolding)

        - [MVC 和 Web API Scaffolding-HTTP 404 找不到錯誤](#404issue)
        - [Visual Studio Express 2012 for Web 新增 scaffold 項目之後停止運作](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [檢視與瀏覽或 F5 的 cshtml 檔案會導致伺服器錯誤](#browseissue)
        - [Url 重寫並 Tilde(~)](#rewriteissue)
    - [範本](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>安裝注意事項

[安裝](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids)ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012。

<a id="requirements"></a>
## <a name="software-requirements"></a>軟體需求

您必須擁有 Visual Studio 2012 或 Visual Studio Express 2012 for Web。

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>在 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012 中的新功能

<a id="bootstrap"></a>
### <a name="bootstrap"></a>啟動程序

當您建立的 MVC 5 控制器和檢視結構時，會使用 檢視標記[Bootstrap](http://getbootstrap.com/)。

<a id="templates"></a>
### <a name="templates"></a>範本

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 範本

我們已新增新的 MVC 5 範本。 它會參考最新的 MVC 5 NuGet 套件，而且您可以使用 scaffolding 將控制器和檢視。

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2 範本

我們已新增新的 Web API 2 範本。 它會參考最新的 Web API 2 NuGet 套件，而且您可以使用 scaffolding 將控制器和檢視。

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>項目範本

我們已新增新的 MVC 5 檢視、 Web 頁面 (Razor 3)，以及 Web API 2 控制器的項目範本。 安裝至專案的相關的 NuGet 套件，同時加入新項目。

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

當您建立使用 Entity Framework 的 MVC 或 Web API 控制器的結構時，我們會使用 Framework 6。 如需有關 Entity Framework 的詳細資訊，請參閱[Entity Framework 版本歷程記錄](https://msdn.com/data/jj574253)。

您也可以下載並安裝適用於 Visual Studio 2012 的 Entity Framework 6 工具。 請參閱[取得 Entity Framework](https://msdn.com/data/ee712906#tooling)。

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET Scaffold 是 ASP.NET Web 應用程式的程式碼產生架構。 它可讓您輕鬆地將未定案程式碼新增至您的資料模型進行互動的專案。

在舊版的 Visual Studio 中，scaffolding 已限制為 ASP.NET MVC 專案。 透過這項更新中，您現在可以針對任何 ASP.NET 專案，包括 Web Form，以使用 scaffolding。 此更新不支援產生的頁面，針對 Web Form 專案，但您仍然可以使用與 Web Forms scaffolding，將 MVC 相依性新增至專案。 將在未來的更新中新增支援產生的 Web Form 的頁面。

當使用 scaffolding，我們會確保所有必要的相依性安裝到專案。 比方說，如果您開始使用 ASP.NET Web Form 專案，然後使用 scaffolding Web API 控制器，將必要的 NuGet 套件和參考會加入至您的專案會自動。

若要加入 Web Form 專案中的 MVC scaffolding，新增**新的 Scaffold 項目**，然後選取**MVC 5 相依性**對話視窗中。 有兩個選項，scaffolding MVC;最少且完整。 如果您選取最小，只有 NuGet 套件和 ASP.NET mvc 的參考會加入到專案中。 如果您選取 [完整] 選項中，加入基本的相依性，以及 MVC 專案所需的內容檔案。

Scaffolding 非同步控制器的支援會使用 Entity Framework 6 的新非同步功能。

如需詳細資訊和教學課程，請參閱 < [ASP.NET Scaffolding 概觀](../2013/aspnet-scaffolding-overview.md)。 這些教學課程會示範 Visual Studio 2013 中，scaffolding，但它們也是適用於 ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012。

<a id="razor"></a>
### <a name="razor-editor"></a>Razor 編輯器

透過這項更新，Visual Studio 2012 現在支援 Razor 3 工具/編輯。

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 包含一組豐富的新功能所說明的詳細討論[NuGet 2.7 版本資訊](http://docs.nuget.org/docs/release-notes/nuget-2.7)。

這個版本的 NuGet 中移除使用者明確地允許 NuGet 還原缺少的套件的需求。 安裝時 NuGet 2.7，使用者會隱含地同意自動還原缺少的套件。 使用者可以明確選擇退出 Visual Studio 中的 NuGet 設定透過套件還原。 這項變更可簡化在套件還原的運作方式。

## <a name="known-issues-and-breaking-changes"></a>已知的問題和重大變更

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC 和 Web API Scaffolding-HTTP 404 找不到錯誤

如果專案中加入 scaffold 項目時，您會遇到錯誤，，很可能您的專案將會處於不一致的狀態。 部分 scaffolding 所做的變更將會回復，但其他的變更，例如已安裝的 NuGet 套件，將不會回復。 如果路由的組態變更會回復，使用者會收到 HTTP 404 錯誤，當瀏覽至包含 scaffold 的項目。

若要修正這個錯誤，mvc，加入新的 scaffold 項目，然後選取 MVC 5 相依性 （基本或完整）。 此程序會加入所有必要的變更至您的專案。

若要修正此錯誤的 Web API:

1. 將下列 WebApiConfig 類別新增至您的專案。

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. 在 應用程式中設定 WebApiConfig.Register\_在 Global.asax 中啟動方法時，也將，如下所示：

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 for Web 新增 scaffold 項目之後停止運作

如果 Visual Studio Express 2012 for Web 之後加入 scaffold 項目與 Entity Framework （例如 Web API 2 控制器與動作，使用 Entity Framework） 停止運作，有可能，Visual Studio Express 無法載入組件的原生映像相依於 System.Web.Extensions。

若要修正此問題，請設定 System.Web.Extensions 的 MSIL 映像所使用的 Visual Studio Express:

1. 以系統管理員模式開啟命令提示字元。
2. 移至 %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE 或 %programfiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE （適用於 64 位元 Windows）。
3. 在文字編輯器中開啟 VWDExpress.exe.config。
4. 新增下的面這一行&lt;組態&gt;/&lt;runtime&gt;項目：  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. 重新啟動 Visual Studio Express 2012 for Web。

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>檢視 cshtml 檔案 withBrowse WithorF5causes 伺服器錯誤

當您在 Visual Studio 2012 （或在 Visual Studio 2013 中建立的 Visual Studio 2012 的 MVC 5 專案中開啟） 中建立的 MVC 5 專案，並嘗試使用瀏覽或 f5 鍵來檢視 cshtml 檔案時，您會收到錯誤訊息-**中發生伺服器錯誤'/' 應用程式**。 伺服器嘗試瀏覽至 `http://localhost:XXXX/Views/../XXXX.cshtml`

若要解決此問題，請變更**起始動作**在您的專案中設定**特定頁面**。 您不需要頁面提供的值。

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

完成此變更之後，選取 F5 瀏覽至您的應用程式的根目錄 (`http://localhost:XXXX`)。 這個行為並不在 Visual Studio 2013 中的 MVC 5 專案的行為相同地方**本頁**設定啟動開啟的頁面。

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Url 重寫並 Tilde(~)

升級至 ASP.NET Razor 3 或 ASP.NET MVC 5 之後，tilde(~) 標記法可能無法再正確運作如果您使用 URL 重寫。 URL 重寫會影響 tilde(~) 標記法中的 HTML 項目這類&lt;A /&gt;，&lt;指令碼 /&gt;，&lt;連結 /&gt;，並因此波狀符號不會再將對應至目錄的根目錄。

比方說，如果您重寫要求**asp.net/content**來**asp.net**中的 href 屬性&lt;A href ="~/content/"/&gt;會解析成 **/content/內容 /** 而非**/**。 若要隱藏這項變更，您可以設定**IIS\_WasUrlRewritten**為 false，每個網頁中或在內容**應用程式\_BeginRequest** Global.asax 中。

<a id="templateissue"></a>
### <a name="templates"></a>範本

當您建立 ASP.NET MVC 專案使用 Visual Studio 2012 在 Windows 8.1 或 Windows Server 2012 R2，Visual Studio 會顯示錯誤訊息，指出 「 設定 Web [url] 為 ASP.NET 4.5 失敗。 」

![組態錯誤](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

因為 Visual Studio 2012 時安裝這些版本的 Windows 上不會啟用 ASP.NET 4.5 功能，您會看到此錯誤。 若要啟用 ASP.NET 4.5，執行中所述的步驟[開啟或關閉開啟的 Windows 功能](https://windows.microsoft.com/windows-8/turn-windows-features-on-off)。

![開啟或關閉 Windows 功能](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

或者，您可以透過命令列來啟用 ASP.NET 4.5。

1. 以系統管理員模式開啟命令提示字元。
2. 執行下列命令，以啟用 ASP.NET 4.5。  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
