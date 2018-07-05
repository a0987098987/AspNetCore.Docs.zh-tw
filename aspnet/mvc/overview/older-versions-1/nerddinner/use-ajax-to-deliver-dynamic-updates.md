---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: 使用 AJAX 傳送動態更新 |Microsoft Docs
author: microsoft
description: 步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用 Ajax 為基礎的方法整合在 dinner 詳細資料...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 789c9880dc43c5d15ee510d9856a4c4378019b71
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372326"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>使用 AJAX 傳送動態更新
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 10 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用整合在 dinner 詳細資料頁面的 Ajax 架構方法。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner 步驟 10： 啟用像是 AJAX 接受

讓我們現在實作支援 RSVP 興趣參加 dinner 登入的使用者。 我們將會啟用這個選項使用 AJAX 為基礎的方法整合在 dinner 詳細資料頁面。

### <a name="indicating-whether-the-user-is-rsvpd"></a>指出使用者是否為聚會

使用者可以瀏覽 */Dinners/詳細資料 / [id*] 若要查看特定 dinner 詳細的 URL:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

動作方法的實作 Details() 就像這樣：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

若要實作 RSVP 支援我們第一個步驟會將"IsUserRegistered(username) 「 協助程式方法新增至我們的 Dinner 物件 （在我們稍早建立的 Dinner.cs 部分類別）。 這個 helper 方法會傳回 true 或 false 取決於使用者目前是否聚會吃晚餐：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

我們 Details.aspx 檢視範本，以便顯示適當的訊息，指出使用者是否已註冊或不適用於事件，我們就可以加入下列程式碼：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

然後，現在當使用者瀏覽其所註冊的 Dinner 他們會看到此訊息：

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

以及當他們瀏覽的 Dinner，它們並不註冊就會看到下列訊息：

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>實作註冊動作方法

讓我們現在新增吃晚餐 RSVP 來啟用使用者，從詳細資料頁面所需的功能。

若要這樣做，我們將建立新的 「 RSVPController"類別的 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [加入]-&gt;控制器功能表命令。

我們將實作會採用吃晚餐做為引數的識別碼，擷取適當的 Dinner 物件，如果登入的使用者目前已註冊，使用者的清單中，如果檢查新 RSVPController 類別內的 [註冊] 動作方法不將它們的 RSVP 物件：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

請注意，上例我們要如何傳回簡單的字串做為動作方法的輸出。 我們無法內嵌檢視範本 – 在此訊息，但因為它是很小，我們將只傳回字串之類的訊息上方與控制器的基底類別上使用 Content() helper 方法。

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>使用 AJAX RSVPForEvent 動作方法的呼叫

我們將使用 AJAX 來叫用註冊動作方法，從我們的詳細資料檢視。 實作此並不難。 首先我們會將新增兩個指令碼程式庫參考：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

第一個程式庫參考的核心 ASP.NET AJAX 用戶端指令碼程式庫。 這個檔案是大約 24 k （壓縮） 的大小，且包含核心用戶端 AJAX 功能。 第二個程式庫包含整合與 ASP.NET MVC 的內建 AJAX helper 方法 （我們將使用） 的公用程式函式。

我們可以接著更新檢視範本程式碼，我們稍早新增以便而不是輸出的 「 您未註冊此事件 」 的訊息，我們改為呈現連結的推入執行叫用我們的 RSVP 控制站上我們 RSVPForEvent 動作方法的 AJAX 呼叫和 RSVPs 使用者：

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

上述範例中使用 Ajax.ActionLink() helper 方法是內建於 ASP.NET MVC，並為 Html.ActionLink() 協助程式方法類似，不同之處在於而不是執行標準的巡覽它進行 AJAX 呼叫動作方法時按下連結。 以上所述，我們會呼叫 「 RSVP 」 控制站上的 [註冊] 動作方法，並將 DinnerID 做為 「 識別碼 」 參數傳遞給它。 我們將傳遞的最終 AjaxOptions 參數可讓您表示我們想要採取的動作方法傳回的內容，並更新 HTML &lt;div&gt;其識別碼是"rsvpmsg 」 頁面上的項目。

然後，現在當使用者瀏覽至 dinner 它們未註冊，但他們會看到它的 RSVP 的連結：

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

如果使用者按一下 「 此事件的 RSVP"連結就會一目了然的註冊動作方法的 AJAX 呼叫 RSVP 站上，並完成時就會看到更新的訊息如下所示：

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

網路頻寬和流量時發出此 AJAX 呼叫是真正的輕量的相關。 當使用者按一下 」 的這個事件的 RSVP 」 連結時，對小型的 HTTP POST 網路要求 */Dinners/Register/1*在網路看起來如下所示的 URL:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

與我們註冊，動作方法的回應就是：

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

這個輕量型的呼叫很快速，而且即使是在較慢的網路運作。

### <a name="adding-a-jquery-animation"></a>新增 jQuery 動畫

我們所實作的 AJAX 功能的運作方式也和快速。 有時可能因此快速、 但，使用者可能不會注意到新的文字，已取代的 RSVP 連結。 若要讓結果更明顯，我們可以加入簡單的動畫，讓大家注意已經更新訊息。

預設的 ASP.NET MVC 專案範本包含 jQuery-Microsoft 也支援的絕佳的 （和非常受歡迎的） 的開放原始碼 JavaScript 程式庫。 jQuery 提供許多功能，包括美觀的 HTML DOM 選取項目和效果庫。

若要使用 jQuery 中，我們將第一次新增對它的指令碼參考。 因為我們要在我們的網站內使用 jQuery 中的許多地方，我們將新增的指令碼參考，我們 Site.master 主版頁面檔案內，讓所有頁面可以都使用它即可。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*秘訣︰ 請確定您已安裝 VS 2008 sp1 可讓 JavaScript 檔案 （包括 jQuery） 更豐富的 intellisense 支援的 JavaScript intellisense hotfix。您可以下載從： http://tinyurl.com/vs2008javascripthotfix*

通常使用 JQuery 撰寫的程式碼會使用全域"$ （）"JavaScript 方法，可擷取一或多個 HTML 項目，使用 CSS 選取器。 例如， <em>$("#rsvpmsg")</em>選取識別碼 rsvpmsg，為任何 HTML 項目時<em>$(".something")</em>會選取所有項目與 「 某事物 」 CSS 類別名稱。 您也可以撰寫更進階的查詢，像是 「 請傳回所有選取的選項按鈕的項目 」 使用選取器的查詢，例如： <em>$("輸入 [@type= technet 收音機] [@checked]")</em>。

一旦您已選取項目，您可以呼叫方法對其採取動作，例如隱藏它們： *$(「 #rsvpmsg").hide();*

我們的 RSVP 案例中，我們會定義名為"AnimateRSVPMessage 「 選取 」 rsvpmsg 」 簡單的 JavaScript 函式&lt;div&gt;並以動畫顯示其文字內容的大小。 下列程式碼會啟動小型的文字，然後會導致它在 400 毫秒的時間範圍內增加：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

我們可以再網路總 AJAX 呼叫成功完成將其名稱傳遞至我們 Ajax.ActionLink() 協助程式方法之後呼叫這個 JavaScript 函式 (透過 AjaxOptions"OnSuccess"事件屬性):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

而現在時按下 」 的這個事件的 RSVP 」 連結，和我們的 AJAX 呼叫已順利完成，內容的訊息傳送後會建立動畫並變大：

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

除了提供"OnSuccess"事件，AjaxOptions 物件會公開 OnBegin、 OnFailure 和 OnComplete （以及各種不同的其他屬性和有用的選項），您可以處理的事件。

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>清除-回覆的部分檢視的重構

詳細資料檢視範本開始有點長，哪些加班將它有點難理解。 若要改善程式碼的可讀性，讓我們完成藉由建立部分檢視 – RSVPStatus.ascx – 封裝所有我們詳細資料 頁面的 RSVP 檢視程式碼。

我們可以執行這項操作 \Views\Dinners 資料夾上按一下滑鼠右鍵，然後選擇 [加入]-&gt;檢視功能表命令。 我們必須採用 Dinner 物件作為其強型別 ViewModel 它。 我們可以接著複製/貼上的 RSVP 內容從我們的 Details.aspx 檢視到其中。

一旦我們所做的我們也建立另一個部分檢視 – EditAndDeleteLinks.ascx-封裝編輯和刪除連結檢視程式碼。 我們也會採用 Dinner 物件作為其強型別的 ViewModel，並複製/貼上的編輯和刪除的邏輯，從我們的 Details.aspx 檢視到其中。

我們的詳細資料檢視範本可以，則只包含兩個在底部的 Html.RenderPartial() 方法呼叫：

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

這會讓程式碼更容易閱讀及維護。

### <a name="next-step"></a>下一個步驟

現在來看看如何我們可以使用 AJAX，更進一步，並加入我們的應用程式中的互動式對應支援。

> [!div class="step-by-step"]
> [上一頁](secure-applications-using-authentication-and-authorization.md)
> [下一頁](use-ajax-to-implement-mapping-scenarios.md)
