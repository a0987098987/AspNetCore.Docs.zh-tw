---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: 防止開啟重新導向攻擊 (C#) |Microsoft Docs
author: jongalloway
description: 本教學課程說明如何防止開啟重新導向攻擊，以及在您的 ASP.NET MVC 應用程式中。 本教學課程將告訴您所做的變更...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 33d2d050805d9b65741c121cdb2b65a59e1ea392
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813196"
---
<a name="preventing-open-redirection-attacks-c"></a>防止開啟重新導向攻擊 (C#)
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> 本教學課程說明如何防止開啟重新導向攻擊，以及在您的 ASP.NET MVC 應用程式中。 本教學課程會討論在 ASP.NET MVC 3 中 AccountController 所做的變更，並示範如何套用這些變更，以及在您現有的 ASP.NET MVC 1.0 和 2 個應用程式中。


## <a name="what-is-an-open-redirection-attack"></a>什麼是開啟重新導向攻擊？

任何重新導向至指定透過例如查詢字串或表單資料要求 URL 的 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。 這種竄改稱為開啟重新導向攻擊。

每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL 未遭竄改。 使用預設 AccountController ASP.NET MVC 1.0 及 ASP.NET MVC 2 的登入很容易就能開啟重新導向攻擊。 幸運的是，很容易更新您現有的應用程式使用 ASP.NET MVC 3 Preview 更正。

若要了解這項弱點，讓我們看看登入重新導向預設 ASP.NET MVC 2 Web 應用程式專案中的運作方式。 在此應用程式，嘗試瀏覽控制器動作，將 [Authorize] 屬性會未經授權的使用者重新導向 /Account/LogOn 檢視。 此重新導向至 /Account/LogOn 會包含 returnUrl 查詢字串參數，以便使用者可以傳回給原始要求的 URL 之後使用者已成功登入。

在以下的螢幕擷取畫面，我們可以看到，嘗試存取時不會記錄在 /Account/ChangePassword 檢視導致重新導向至 /Account/LogOn 嗎？ReturnUrl = %2faccount%2fChangePassword %2f。

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**圖 01**： 具有開啟重新導向的登入頁面

ReturnUrl 查詢字串參數無效的因為攻擊者可以修改它來進行開啟重新導向攻擊參數中插入任何 URL 位址。 為了示範這點，我們可以修改 ReturnUrl 參數[ http://bing.com ](http://bing.com)，因此會產生的登入 URL/帳戶/登入？ReturnUrl =<http://www.bing.com/>。 在成功登入網站，我們會重新導向至[ http://bing.com ](http://bing.com)。 因為不會驗證此重新導向，它可以改為指向惡意網站，並會嘗試誘騙使用者。

### <a name="a-more-complex-open-redirection-attack"></a>更複雜的開啟重新導向攻擊

開啟重新導向攻擊是特別危險，因為攻擊者可讓您知道我們正在嘗試登入特定網站，讓我們更容易遭受[網路釣魚攻擊](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)。 比方說，攻擊者無法傳送惡意電子郵件給網站的使用者在嘗試擷取其密碼。 讓我們看看它 NerdDinner 網站上的運作方式。 （請注意，即時 NerdDinner 網站已更新以防止開啟重新導向攻擊）。

首先，攻擊者會傳送我們連結 NerdDinner 包含重新導向至其偽造的頁面中的 [登入] 頁面：

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

請注意，傳回的 URL 指向 nerddiner.com，遺漏"n"從 word dinner。 在此範例中，這是攻擊者控制的網域。 當我們存取上面的連結時，我們要進入合法 NerdDinner.com 登入頁面。

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**圖 02**: NerdDinner 具有開啟重新導向的登入頁面

當我們正確登入時，ASP.NET MVC AccountController 登入動作會重新導向我們 returnUrl 查詢字串參數中指定的 URL。 在此情況下，它是攻擊者已輸入 URL，也就是[ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn)。 我們非常戒慎守望，特別是因為攻擊者已經過謹慎地確定就很可能是我們不會發現這個問題，除非其偽造的頁面看起來完全合法的登入頁面。 此登入頁面會包含錯誤訊息，要求，我們再次登入。 笨拙我們，我們必須有輸入錯誤，我們的密碼。

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**圖 03**： 偽造 NerdDinner 登入畫面

當我們重新輸入我們的使用者名稱和密碼時，將資訊儲存到偽造的登入頁面，並將我們傳送合法 NerdDinner.com 站台。 此時，NerdDinner.com 站台已經驗證，因此偽造的登入頁面可以重新導向至該頁面的直接。 最終結果是，攻擊者有沒有我們的使用者名稱和密碼，我們不知道，我們已提供給它們。

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>查看 AccountController 登入作用中的易受攻擊的程式碼

ASP.NET MVC 2 應用程式中的登入動作的程式碼如下所示。 請注意，成功的登入，控制器傳回重新導向實施 returnurl。 您可以看到針對 returnUrl 參數執行任何驗證。

**列表 1 – 在 ASP.NET MVC 2 登入動作 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

現在讓我們看看 ASP.NET MVC 3 登入動作所做的變更。 此程式碼已變更為呼叫中名為 System.Web.Mvc.Url helper 類別的新方法來驗證 returnUrl 參數`IsLocalUrl()`。

**列表 2 – 在 ASP.NET MVC 3 登入動作 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

這個部分已經變更來驗證在 System.Web.Mvc.Url 協助程式類別中，呼叫新方法的傳回 URL 參數`IsLocalUrl()`。

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>保護您的 ASP.NET MVC 1.0 和 MVC 2 應用程式

我們可以利用 ASP.NET MVC 3 變更我們現有的 ASP.NET MVC 1.0 和 2 個應用程式中藉由加入 IsLocalUrl() helper 方法更新登入動作，以驗證 returnUrl 參數。

UrlHelper IsLocalUrl() 方法實際上只呼叫 System.Web.WebPages，以作為此驗證中的方法也會使用 ASP.NET Web Pages 應用程式。

**列表 3 – 從 ASP.NET MVC 3 UrlHelper IsLocalUrl() 方法 `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost 方法包含實際的驗證邏輯，列表 4 中所示。

**列表 4 – IsUrlLocalToHost() System.Web.WebPages RequestExtensions 類別方法**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

在我們的 ASP.NET MVC 1.0 或 2 個應用程式中，我們會將 IsLocalUrl() 方法加入 AccountController，但是您可以建議盡可能將它新增至個別協助程式類別。 我們將會變更兩個小型 IsLocalUrl() 的 ASP.NET MVC 3 版本，讓它能夠 AccountController 內。 首先，我們會將它從公用方法的私用的方法，因為可以存取控制器中的公用方法，為控制器動作。 其次，我們要修改檢查針對應用程式主機 URL 主機的呼叫。 呼叫，會使用本機 RequestContext UrlHelper 類別中的欄位。 而不是使用這種情況。RequestContext.HttpContext.Request.Url.Host，我們將使用此。Request.Url.Host。 下列程式碼顯示修改的 IsLocalUrl() 方法的控制器類別搭配 ASP.NET MVC 1.0 和 2 個應用程式中。

**列表 5 – IsLocalUrl() 方法，這被修改搭配 MVC 控制器類別**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

既然 IsLocalUrl() 方法已就緒，我們可以從我們的登入動作，以驗證 returnUrl 參數中，呼叫它，如下列程式碼所示。

**列表 6-更新登入方法讓 returnUrl 參數進行驗證**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

現在我們可以測試開啟重新導向攻擊嘗試登入使用外部的傳回 URL。 讓我們使用 /Account/LogOn 嗎？ReturnUrl =<http://www.bing.com/>一次。

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**圖 04**： 測試更新的登入動作

已成功登入之後，我們將會重新導向至 Home/Index 控制器動作，而不是外部 URL。

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**圖 05**： 擊敗的開啟重新導向攻擊

## <a name="summary"></a>總結

開啟重新導向攻擊就會發生重新導向 Url 會當做應用程式的 URL 中的參數傳遞。 ASP.NET MVC 3 範本包含程式碼，以防止開啟重新導向攻擊。 您可以新增此程式碼進行一些修改 ASP.NET MVC 1.0 和 2 個應用程式。 若要防止開啟重新導向攻擊，登入 ASP.NET 1.0 和 2 個應用程式時，新增 IsLocalUrl() 方法並驗證登入作用中的 returnUrl 參數。
