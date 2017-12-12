---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: "無法開啟重新導向攻擊 (C#) |Microsoft 文件"
author: jongalloway
description: "本教學課程說明如何避免開啟重新導向攻擊，以及在 ASP.NET MVC 應用程式中。 本教學課程將告訴您所做的變更..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 97e0aacbf21914bf95f01019cf4dcc9e7ca1c4be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="preventing-open-redirection-attacks-c"></a>無法開啟重新導向攻擊 (C#)
====================
由[Jon Galloway](https://github.com/jongalloway)

> 本教學課程說明如何避免開啟重新導向攻擊，以及在 ASP.NET MVC 應用程式中。 本教學課程會討論中 AccountController ASP.NET MVC 3 中所做的變更，並示範如何將這些變更套用在您現有的 ASP.NET MVC 1.0 和 2 的應用程式。


## <a name="what-is-an-open-redirection-attack"></a>什麼是開啟的重新導向攻擊？

指定透過例如查詢字串或表單資料要求的 URL 重新導向至任何 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。 這種竄改稱為開啟重新導向攻擊。

每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL，沒有遭到竄改。 使用預設 AccountController 在 ASP.NET MVC 1.0 和 ASP.NET MVC 2 的登入很容易就能開啟重新導向攻擊。 幸運的是，所以可以輕鬆地更新您現有的應用程式使用 ASP.NET MVC 3 Preview 更正。

若要了解這項弱點，讓我們看看登入重新導向預設的 ASP.NET MVC 2 Web 應用程式專案中的運作方式。 在此應用程式，嘗試瀏覽具有 [Authorize] 屬性的控制器動作會重新導向未經授權的使用者 /Account/LogOn 檢視。 此重新導向至 /Account/LogOn 會包含傳回的 Url 查詢字串參數，以便使用者可以傳回至原本要求的 URL 之後使用者成功登入。

在以下的螢幕擷取畫面，我們可以看到，嘗試存取 /Account/ChangePassword 檢視時不會記錄在會導致重新導向至 /Account/LogOn 嗎？ReturnUrl = %2faccount%2fChangePassword %2f。

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**圖 01**： 與開啟的重新導向的登入頁面

ReturnUrl querystring 參數未經過驗證，因為攻擊者可以修改它的任何 URL 位址中插入參數來進行開啟重新導向攻擊。 若要示範這點，我們可以修改的 ReturnUrl 參數[http://bing.com](http://bing.com)，因此會產生登入 URL/帳戶/登入？ ReturnUrl = http://www.bing.com/。 在成功登入至站台，我們會重新導向至[http://bing.com](http://bing.com)。此重新導向未經過驗證，因為它無法改為指向嘗試欺騙使用者惡意網站。

### <a name="a-more-complex-open-redirection-attack"></a>更複雜的開啟重新導向攻擊

開啟的重新導向攻擊會特別危險，因為攻擊者可讓您知道我們正在嘗試登入特定網站，讓我們更容易遭受[網路釣魚攻擊](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)。 比方說，攻擊者無法傳送惡意電子郵件給網站使用者嘗試擷取其密碼。 讓我們看看 NerdDinner 站台上運作方式。 （請注意，已更新即時 NerdDinner 網站以防止開啟重新導向攻擊）。

首先，攻擊者會傳送給我們連結登入頁面上，其中包含重新導向至其偽造的頁面 NerdDinner:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

請注意，傳回的 URL 指向 nerddiner.com，遺漏"n"從 word dinner。 在此範例中，這是攻擊者可以控制的網域。 當我們存取上面的連結時，我們正在採取合法 NerdDinner.com 登入頁面。

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**圖 02**: NerdDinner 與開啟的重新導向的登入頁面

當我們正確登入時，ASP.NET MVC AccountController 登入動作重新導向至我們 returnUrl querystring 參數中指定的 URL。 在此情況下，它是攻擊者有輸入 URL，也就是[http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn)。 我們非常戒慎守望，尤其是因為攻擊者已經確定，請小心就很可能是我們不會注意到這一點，除非其偽造的頁面看起來完全合法的登入頁面。 這個登入頁面將包含錯誤訊息，要求，我們登入一次。 同時，我們必須輸入錯誤密碼。

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**圖 03**： 偽造 NerdDinner 登入畫面

當我們重新輸入我們的使用者名稱和密碼時，偽造的登入頁面將儲存的資訊，並將我們傳送至合法 NerdDinner.com 站台。 此時，NerdDinner.com 站台已經驗證，因此偽造的登入頁面可以重新導向至該頁面的直接。 最終結果是，攻擊者有我們的使用者名稱和密碼，我們不知道，我們已將它提供給它們。

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>查看 AccountController 登入動作中的弱點程式碼

ASP.NET MVC 2 應用程式中的登入動作的程式碼如下所示。 請注意，成功的登入，控制器會傳回重新導向根據 returnurl 進行。 您可以查看針對 returnUrl 參數執行任何驗證。

**列出 1 – 在 ASP.NET MVC 2 登入動作`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

現在讓我們看看 ASP.NET MVC 3 登入動作所做的變更。 此程式碼已變更為呼叫中名為的 System.Web.Mvc.Url 協助程式類別的新方法來驗證 returnUrl 參數`IsLocalUrl()`。

**列出 2 – 在 ASP.NET MVC 3 登入動作`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

這個部分已經變更驗證 System.Web.Mvc.Url 協助程式類別中呼叫新方法的傳回 URL 參數`IsLocalUrl()`。

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>保護您的 ASP.NET MVC 1.0 和 MVC 2 應用程式

我們可以利用 ASP.NET MVC 3 變更我們現有的 ASP.NET MVC 1.0 和 2 的應用程式中藉由加入 IsLocalUrl() helper 方法更新登入動作，以驗證 returnUrl 參數。

UrlHelper IsLocalUrl() 方法實際上只呼叫 System.Web.WebPages，做為此驗證中的方法也會使用 ASP.NET Web Pages 應用程式。

**列出 3 – 從 ASP.NET MVC 3 UrlHelper IsLocalUrl() 方法`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost 方法包含的實際驗證邏輯，在列出的 4 所示。

**列出 4 – 自 System.Web.WebPages RequestExtensions 類別 IsUrlLocalToHost() 方法**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

在我們的 ASP.NET MVC 1.0 或 2 的應用程式中，我們會將 IsLocalUrl() 方法新增至 AccountController，但是您可以建議盡可能將它加入至個別的 helper 類別。 我們會讓兩個小變更 IsLocalUrl() 的 ASP.NET MVC 3 版本，以便將 AccountController 內運作。 首先，我們會將它變更從公用方法的私用的方法，因為控制器中的公用方法可以存取為控制器的動作。 其次，我們將修改檢查 URL 主機應用程式主機的呼叫。 呼叫會用到的本機 RequestContext UrlHelper 類別中的欄位。 而不是使用這種情況。RequestContext.HttpContext.Request.Url.Host，我們將使用此。Request.Url.Host。 下列程式碼會顯示已修改的 IsLocalUrl() 方法控制器類別搭配使用，在 ASP.NET MVC 1.0 和 2 的應用程式。

**列出 5 – IsLocalUrl() 方法，這會修改用於 MVC 控制器類別**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

既然 IsLocalUrl() 方法是在位置中，我們可以從驗證 returnUrl 參數中，我們登入動作呼叫它，如下列程式碼所示。

**列出 6 – 已更新登入方法，這些方法會驗證 returnUrl 參數**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

現在我們可以嘗試使用外部的傳回 URL 登入來測試開啟重新導向攻擊。 讓我們使用/帳戶/登入？ ReturnUrl = http://www.bing.com/ 一次。

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**圖 04**： 測試更新的登入動作

已成功登入之後，我們會重新導向至 Home/Index 控制器動作，而不是外部 URL。

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**圖 05**： 擊敗開啟重新導向攻擊

## <a name="summary"></a>總結

重新導向 Url 會當做應用程式的 URL 中的參數傳遞，就會發生開啟重新導向攻擊。 範本包含程式碼，以避免 ASP.NET MVC 3 開啟重新導向攻擊。 您可以加入這個程式碼並進行一些修改 ASP.NET MVC 1.0 和 2 的應用程式。 若要防止開啟重新導向攻擊，登入 ASP.NET 1.0 和 2 的應用程式時，加入 IsLocalUrl() 方法並驗證登入作用中的 returnUrl 參數。
