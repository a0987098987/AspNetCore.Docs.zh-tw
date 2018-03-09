---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "ASP.NET Web Pages 2 開發人員預覽讀我檔案 |Microsoft 文件"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 開發人員預覽讀我檔案
====================
由[Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 開發人員預覽讀我檔案

14 年 9 月 2011

### <a name="contents"></a>內容

#### <a id="_Toc303701284"></a>安裝注意事項

若要安裝 Web Pages 2 Developer Preview，您有下列選項：

- 安裝 WebMatrix 2 Beta 使用[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883)。 WebMatrix 是一組可用的 web 開發工具，其中包含 ASP.NET Web Pages。 如需詳細資訊，請參閱 < 安裝 > 一節中的[ASP.NET Web Pages 2 開發人員指南預覽中的 Top 功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

- 安裝 Web Pages 2 開發人員預覽使用直接[下載連結](https://go.microsoft.com/fwlink/?LinkID=226335)。 如果您想要建立網頁應用程式使用文字編輯器例如記事本，請使用此方法。 若要執行 Web Pages 2 應用程式，您必須使用 IIS Express 7.5。 （這是自動隨附 WebMatrix）。如秘訣如何測試網頁 」 頁面使用 IIS Express，請參閱資訊看板 」 建立和測試 ASP.NET 頁面使用您自己文字編輯器 」 中[WebMatrix 及 ASP.NET Web Pages 入門](https://go.microsoft.com/fwlink/?LinkId=202889)。

ASP.NET Web Pages 2 Developer Preview 可以安裝，並且可以執行的並行與 ASP.NET Web Pages 1。 <a id="a"></a>如需詳細資訊，請參閱 < > 一節 「 執行網頁應用程式的並存 」 中[Web Pages 2 開發人員指南預覽中的 Top 功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

#### <a id="_Toc303701285"></a>文件

教學課程和其他資訊有關 ASP.NET Web 網頁上可用的 ASP.NET 網站的 [網頁] 頁面 ([https://www.asp.net/web-pages/](../../index.md))。 如需新功能和 Web Pages 2 中的增強功能的資訊，請參閱[Web Pages 2 開發人員指南預覽中的 Top 功能](https://go.microsoft.com/fwlink/?LinkID=227824)。

#### <a id="_Toc303701286"></a>支援

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>這是預覽版本，而且未正式支援。 如果您有關於使用此版本的問題，張貼至 ASP.NET Web Pages 論壇 ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) )，其中的 ASP.NET 社群成員可經常提供非正式的支援。

#### <a id="_Toc303701287"></a>軟體需求

ASP.NET Web Pages 2 需要.NET Framework 4。 它也可搭配.NET Framework 4.5 Developer Preview 版本。

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>修正程式、 已知的問題和重大變更

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **是\*方法 (例如，IsDateTime) 現在傳回正確值的所有文化特性。** 等某些方法*IsDateTime*先前傳回*false*時應該有傳回這些*true*因為其先前執行特定文化特性的檢查。 這些方法已獲得修正，現在將文化特性列入考量。 這是一項重大變更。如果您的應用程式依賴舊版的行為，它會中斷。
- **Href 方法的行為已變更。** 先前，呼叫 Href("~/SomeFile") 會傳回目前執行之檔案的相對 URL。 現在 Href("~/SomeFile") 一律會傳回絕對路徑根目錄中的應用程式。 大部分情況下，這種行為將不會讓傳回值的差異。 這項變更已修正某些 Ajax 案例。 例如，請考慮下列範例程式碼： 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    此程式碼之前會解析成 Images/Logo.jpg，就可能發生不正確的 Ajax 要求至該頁面。 它現在會解析成的根目錄 (/ MySite/Images/Logo.jpg)。
- **HttpContext.RedirectLocal 方法已變更**。 此方法現在接受相對於目前的應用程式的 Url。 完整的 Url 會遭到拒絕。
- **ModelState.IsValid 方法現在必須要先呼叫 Validate**。 如果您想要轉換您的應用程式使用新的輸入的驗證方法，將會呼叫*ModelState.IsValid*方法，您必須立即呼叫*Validation.Validate*事先。 例如，您現在必須遵循此模式： 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 不過，我們建議，如果您使用新的輸入的驗證方法，請勿使用*ModelState.IsValid*。 相反地，建構您的程式碼，就像這樣： 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **在 Internet Explorer 7 和 Internet Explorer 8 中，用戶端驗證無法運作**。 用戶端驗證無法運作由於不相容的 jQuery 1.6.2，而隨附的預設專案範本。 （伺服器端驗證運作。）。

#### <a id="_Toc303701289"></a>免責聲明

© 2011 Microsoft Corporation。 著作權所有，並保留一切權利。 本文件係"做為-是。 」 資訊及檢視在此文件，包括 URL 及其他網際網路網站參考資料，可能會變更恕不另行通知。 貴用戶須自行承擔使用風險。
