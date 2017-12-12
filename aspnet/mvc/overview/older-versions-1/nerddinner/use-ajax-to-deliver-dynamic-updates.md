---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "提供動態更新使用 AJAX |Microsoft 文件"
author: microsoft
description: "步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用 Ajax 為基礎的方式整合至 dinner 詳細資料..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>用於 AJAX 傳送動態更新
====================
由[Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 10 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用整合至 dinner 詳細資料頁面 Ajax 為基礎的方法。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner 步驟 10: AJAX 啟用與會者接受

我們現在實作支援 RSVP 興趣參加 dinner 登入的使用者。 我們將會啟用此使用 AJAX 為基礎的方式整合至 dinner 詳細資料頁面。

### <a name="indicating-whether-the-user-is-rsvpd"></a>指出是否使用者 rsvp 的活動

使用者可以瀏覽*/Dinners/詳細資料 / [識別碼*] 若要查看有關特定 dinner 詳細資料的 URL:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

動作方法實作 Details() 就像這樣：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

實作 RSVP 支援我們第一個步驟是加入 「 IsUserRegistered(username)"helper 方法我們 Dinner 物件 （在我們稍早建立的 Dinner.cs 部分類別）。 這個 helper 方法會傳回 true 或 false，根據是否使用者目前 rsvp 的活動的晚餐：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

我們 Details.aspx 檢視範本，以顯示適當的訊息，指出使用者是否已註冊或不適用於事件，我們就可以加入下列程式碼：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

然後，現在當使用者造訪的 Dinner，針對已註冊他們會看到此訊息：

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

和當他們在瀏覽的 Dinner，它們未登錄的就會看到下列訊息：

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>實作註冊動作方法

讓我們現在加入 RSVP 的晚餐來讓使用者從詳細資料頁面所需的功能。

若要實作這種情況，我們將建立新的 「 RSVPController 」 類別 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;控制器功能表命令。

我們會實作新 Dinner 做為引數所需的識別碼，擷取適當的 Dinner 物件，如果登入使用者目前已註冊，使用者清單中，如果檢查 RSVPController 類別內的 「 註冊 」 動作方法無法將 RSVP 物件加入它們：

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

請注意，上述我們如何傳回簡單的字串做為動作方法的輸出。 我們無法內嵌檢視範本 – 內此訊息，但因為它是很小，我們將只傳回字串訊息類似上述控制器的基底類別等使用 Content() helper 方法。

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>呼叫使用 AJAX RSVPForEvent 動作方法

我們將使用 AJAX 叫用註冊的動作方法，從我們的詳細資料檢視。 此實作是很簡單。 首先我們會將兩個指令碼程式庫參考：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

第一個程式庫參考核心 ASP.NET AJAX 用戶端指令碼程式庫。 這個檔案是大約 24 k （壓縮） 的大小，而且包含核心的用戶端 AJAX 功能。 第二個程式庫包含整合與 ASP.NET MVC 的內建 AJAX helper 方法 （我們將使用） 的公用程式函式。

我們可以更新檢視的範本程式碼我們稍早加入，好讓而不是 outputing 「 您未註冊此事件 」 的訊息，我們改為將連結呈現的推送時執行我們 RSVP 控制站上的我們 RSVPForEvent 動作方法會叫用的 AJAX 呼叫和 RSVPs 使用者：

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

上面使用 Ajax.ActionLink() helper 方法是內建 ASP.NET MVC，類似於 Html.ActionLink() helper 方法不同之處在於而不是執行標準巡覽 exports 動作方法的 AJAX 呼叫在按下連結時。 上方我們會呼叫 「 RSVP"控制站上的"Register"動作方法，並將 DinnerID 做為 「 識別碼 」 參數傳遞給它。 我們傳遞最後一個 AjaxOptions 參數表示我們想要採取的動作方法傳回的內容並更新 HTML &lt;div&gt;其識別碼是"rsvpmsg 」 頁面上的元素。

然後，現在當使用者瀏覽至 dinner 它們未登錄的但他們會看到它 RSVP 的連結：

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

如果按下 「 RSVP 這個事件 」 連結它們要進行註冊動作方法的 AJAX 呼叫 RSVP 站上，並在完成時，它們就會看到更新的訊息類似下面的：

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

網路頻寬和真的輕量型進行此 AJAX 呼叫時所涉及的流量。 當使用者按一下"RSVP 這個事件 」 連結時，對小型的 HTTP POST 網路要求*/Dinners/Register/1*看起來類似下面的在網路的 URL:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

而只是我們註冊動作方法的回應：

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

此輕量型的呼叫是快速，而且即使透過慢速網路運作。

### <a name="adding-a-jquery-animation"></a>新增 jQuery 動畫

我們實作 AJAX 功能的運作方式也和 fast。 有時還是有可能發生因此快速，不過，使用者可能不會注意到，已用新文字取代 RSVP 連結。 為了讓結果更明顯我們可以加入簡單的動畫，以強調更新訊息。

預設的 ASP.NET MVC 專案範本包括 jQuery – Microsoft 也支援絕佳 （和相當常用） 開放原始碼 JavaScript 程式庫。 jQuery 提供許多功能，包括 nice HTML DOM 選取範圍和影響文件庫。

若要使用 jQuery 我們會先將新增指令碼參考。 因為我們將在我們的網站中使用 jQuery 內的許多地方，我們會將新增指令碼參考內我們 Site.master 主版頁面檔案，讓所有頁面可以都使用它。

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*提示： 請確定您已安裝可提供 JavaScript 檔案 （包括 jQuery） 更豐富的 intellisense 支援的 VS 2008 sp1 的 JavaScript intellisense hotfix。您可以下載從： http://tinyurl.com/vs2008javascripthotfix*

通常使用 JQuery 撰寫的程式碼會使用全域"$ （）"JavaScript 方法，可擷取一或多個 HTML 項目，使用 CSS 選取器。 例如， *$("#rsvpmsg")*選取識別碼為 rsvpmsg，任何 HTML 項目時*$(".something")*會選取所有項目 「 結果 」 CSS 類別名稱。 您也可以撰寫更進階的查詢，例如 「 傳回所有選取的選項按鈕 」 使用 「 類似的選取器查詢： *$(「 輸入 [@type= 無線電] [@checked]")*。

一旦您已選取項目，您可以對它們採取的動作，例如隱藏它們呼叫方法： *$("#rsvpmsg").hide();*

我們 RSVP 的案例，我們會定義簡單的 JavaScript 函式，名為"AnimateRSVPMessage 「 選取 」 rsvpmsg" &lt;div&gt;和以動畫顯示其文字內容的大小。 下列程式碼啟動小型的文字，然後原因它增加 400 毫秒的時間範圍內：

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

我們可以再 wire 總我們 AJAX 呼叫成功完成將其名稱傳遞給我們 Ajax.ActionLink() helper 方法之後呼叫這個 JavaScript 函式 (透過 AjaxOptions"OnSuccess 」 事件屬性):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

和現在時按下 「 RSVP 這個事件 」 連結和我們的 AJAX 呼叫已順利完成，內容傳送的訊息後將會以動畫顯示成長大型：

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

除了提供 「 OnSuccess 」 事件，AjaxOptions 物件會公開 OnBegin、 OnFailure 和 OnComplete （以及各種不同的其他屬性和有用的選項），您可以處理的事件。

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>清除-重構出 RSVP 的部分檢視

我們的詳細資料檢視範本正在啟動以取得更長，哪些加班會更有點難了解。 為了協助改善程式碼的可讀性，讓我們先完成所建立的部分檢視 – RSVPStatus.ascx – 我們的詳細資料頁面的 RSVP 檢視程式碼的所有封裝。

我們可以這樣 \Views\Dinners 資料夾上按一下滑鼠右鍵，然後選擇 [新增]-&gt;檢視功能表命令。 我們必須才會建立其強型別 ViewModel Dinner 物件。 我們可以再複製/貼上 RSVP 內容從我們 Details.aspx 檢視它。

一旦我們所做的我們也建立另一個部分檢視 – EditAndDeleteLinks.ascx-封裝編輯和刪除連結檢視程式碼。 我們也會讓它接受以其強型別 」 ViewModel，Dinner 物件和複製/貼上的編輯和刪除的邏輯，從它我們 Details.aspx 檢視。

詳細資料檢視範本，然後只包含兩個在底部的 Html.RenderPartial() 方法呼叫：

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

這讓清除器來讀取與維護的程式碼。

### <a name="next-step"></a>下一個步驟

我們現在看我們可以更進一步使用 AJAX 和互動式對應支援將我們的應用程式的方法。

>[!div class="step-by-step"]
[上一頁](secure-applications-using-authentication-and-authorization.md)
[下一頁](use-ajax-to-implement-mapping-scenarios.md)
