---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: "反覆項目 #7 – 新增 Ajax 功能 (C#) |Microsoft 文件"
author: microsoft
description: "在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: db313d12dfd6a146347f253dc3a1f4a889bee780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="iteration-7--add-ajax-functionality-c"></a>反覆項目 #7 – 新增 Ajax 功能 (C#)
====================
由[Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>建立連絡人管理 ASP.NET MVC 應用程式 (C#)

在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 與每個反覆項目，我們會逐漸改善應用程式。 這個多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-請看起來很棒的應用程式。 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。

- 反覆項目 #3-加入表單驗證。 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-請鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。

- 反覆項目 #6-使用測試為導向的開發。 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，我們會加入連絡人群組。

- 反覆項目 #7： 加入 Ajax 功能。 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。

## <a name="this-iteration"></a>這個反覆項目

在連絡人管理員應用程式的這個反覆項目，我們可以重構我們的應用程式來使用 Ajax。 利用 Ajax，就能讓我們的應用程式更能有效回應。 我們可以避免當我們需要更新只有特定區域在頁面中的時呈現整個頁面。

我們將會重構我們索引檢視，以便我們不 t 需要每當有人選取新的連絡人群組時，重新顯示整個頁面。 相反地，當使用者按一下連絡人群組，我們將只更新的連絡人清單，然後將其餘的頁面單獨。

我們也將會變更我們刪除連結運作的方式。 而不是顯示個別的確認頁面，我們將會顯示 JavaScript 確認對話方塊。 如果您確認您想要刪除的連絡人，針對要從資料庫刪除的連絡人記錄之伺服器執行 HTTP DELETE 作業。

此外，我們也會利用 jQuery 動畫效果加入我們的索引檢視。 正在從伺服器擷取新的連絡人清單時，我們將會顯示動畫。

最後，我們也會利用管理瀏覽器歷程記錄的 ASP.NET AJAX framework 支援。 我們執行更新連絡人清單的 Ajax 呼叫時，我們將建立記錄點。 這樣一來，瀏覽器向後和向前按鈕將會運作。

## <a name="why-use-ajax"></a>為何要使用 Ajax？

使用 Ajax 有許多好處。 首先，將 Ajax 功能加入至應用程式會導致較佳使用者體驗。 在一般 web 應用程式中，整個頁面必須回傳至伺服器，每次使用者執行的動作。 每當您執行的動作時，瀏覽器鎖定且使用者必須等到整個頁面提取，且會重新顯示。

這會在桌面應用程式的情況下，無法接受的體驗。 不過，傳統上，我們都和 web 應用程式在此不正確的使用者經驗因為我們不知道我們可以執行更好。 我們想的那麼 web 應用程式的限制時，實際上，它只我們 imaginations 的限制。

Ajax 應用程式中，您不會導致使用者體驗只是為了更新頁面中止 t 需要。 相反地，您可以更新頁面背景中執行的非同步要求。 您不 t 強制使用者稍候網頁的一部分取得更新。

利用 Ajax，您也可以改善應用程式的效能。 請考慮連絡人管理員應用程式的運作方式現在不使用 Ajax 功能。 當您按一下連絡人群組時，就必須重新顯示整個索引檢視。 必須從資料庫伺服器擷取的連絡人清單以及連絡人群組的清單。 所有資料必須傳遞透過網路從 web 伺服器到網頁瀏覽器。

我們將 Ajax 功能新增至我們的應用程式之後，不過，我們就可以避免重新顯示整個頁面中，當使用者按一下連絡人群組。 我們不需要再擷取從資料庫連絡人的群組。 我們也不 t 需要跨線路推送整個索引檢視。 利用 Ajax，我們必須先執行我們的資料庫伺服器的工作量，我們減小我們的應用程式所需的網路傳輸量。

## <a name="don-t-be-afraid-of-ajax"></a>不要被恐怕 Ajax

有些開發人員會避免使用 Ajax，因為它們擔心舊版瀏覽器。 他們想要確定其 web 應用程式將仍然運作時不支援 JavaScript 的瀏覽器來存取。 由於 Ajax 相依於 JavaScript 中，有些開發人員會避免使用 Ajax。

不過，如果您是您如何實作 Ajax 時請小心您可以建立使用新版和舊版瀏覽器應用程式。 我們的連絡人管理員應用程式將會使用支援 JavaScript 的瀏覽器和瀏覽器的不相符。

如果您使用連絡人管理員應用程式的瀏覽器支援 JavaScript，則您將有較佳使用者體驗。 例如，當您按一下連絡人群組，只顯示連絡人的網頁的區域將會更新。

如果相反地，您的瀏覽器不支援 JavaScript （或將停用 JavaScript） 使用連絡人管理員應用程式時，您會有稍微較不理想的使用者經驗。 例如，當您按一下連絡人群組，整個索引檢視必須回傳至瀏覽器以顯示符合的連絡人清單。

## <a name="adding-the-required-javascript-files"></a>新增必要的 JavaScript 檔案

我們需要使用三個的 JavaScript 檔案加入至我們的應用程式的 Ajax 功能。 這三個，這些檔案會包含在新的 ASP.NET MVC 應用程式的指令碼 資料夾。

如果您打算在應用程式中的多個網頁中使用 Ajax 然後合理您應用程式的檢視主版頁面中包含所需的 JavaScript 檔案。 這樣一來，JavaScript 檔案將會自動包含在所有應用程式中的頁面。

加入下列 JavaScript 包含內&lt;head&gt;檢視主版頁面的標記：

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>重構以使用 Ajax 索引檢視

可讓 s 一開始先修改我們索引檢視，以便按一下連絡人群組只會更新此檢視會顯示連絡人的區域。 圖 1 中的紅色方塊包含我們想要更新的區域。


[![更新連絡人](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**圖 01**： 更新的連絡人 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-cs/_static/image2.png))


第一個步驟是檢視的分隔的部分我們想要以非同步方式更新到不同的部分 （檢視使用者控制項）。 顯示連絡人的索引檢視的區段已移到清單 1 中部分。

**列出 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

請注意部分列表 1 中的有不同於索引檢視表的模型。 *Inherits*屬性&lt;%@ 頁面 %&gt;指示詞會指定部分繼承 ViewUserControl&lt;群組&gt;類別。

更新的索引檢視表包含在清單 2。

**列出 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

有兩個的項目，您應該注意到關於更新的檢視表中列出的 2。 首先，請注意，所有內容移動到部分會取代 Html.RenderPartial() 呼叫。 索引檢視第一次要求以顯示一組初始的連絡人時，會呼叫 Html.RenderPartial() 方法。

第二，請注意，用來顯示連絡人群組 Html.ActionLink() 已被取代 Ajax.ActionLink()。 Ajax.ActionLink() 呼叫具備下列參數：

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

第一個參數代表要顯示的連結文字、 第二個參數代表的路由值，而第三個參數代表 Ajax 選項。 在此情況下，我們使用 UpdateTargetId Ajax 選項以指向 HTML &lt;div&gt;我們想要更新 Ajax 要求完成之後的標記。 我們想要更新&lt;div&gt;標記與新的連絡人清單。

已更新 index （） 方法的連絡人控制器被包含在列出的 3。

**列出 3-Controllers\ContactController.cs （索引方法）**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

更新的 index 動作有條件地傳回其中一個兩件事。 如果 index 動作 Ajax 要求叫用控制器就會傳回部分。 否則，index （） 動作會傳回整個檢視。

請注意 index 動作不需要傳回 Ajax 要求叫用時，最多資料。 在一般的要求內容中，索引動作會傳回所有連絡人群組與所選的連絡人群組的清單。 在 Ajax 要求的內容中，index （） 動作會傳回選取的群組。 Ajax 中表示您的資料庫伺服器上的工作較少。

我們已修改的索引檢視的運作方式在新版和舊版瀏覽器的情況下。 如果您按一下連絡人群組，而且您的瀏覽器支援 JavaScript，則會更新檢視，其中包含連絡人清單的區域。 如果相反地，您的瀏覽器不支援 JavaScript，則會更新整個檢視。


更新的索引檢視表有一個問題。 當您按一下連絡人群組時，為非反白顯示所選的群組。 因為更新期間 Ajax 要求區域外顯示群組的清單，正確的群組不會無法取得反白顯示。 我們將在下一節中修正此問題。


## <a name="adding-jquery-animation-effects"></a>新增 jQuery 動畫效果

一般來說，當您按一下連結，以在網頁中，您可以使用瀏覽器進度列來偵測為主動擷取更新的內容瀏覽器中。 當執行 Ajax 要求時，相反地，在瀏覽器進度列不會顯示任何進度。 這可讓使用者外人。 如何知道是否有凍結的瀏覽器？

有幾種方式，您可以指出使用者執行 Ajax 要求時執行工作。 其中一個方法是顯示簡單的動畫。 例如，您可以淡出區域 Ajax 要求開始與要求完成時，區域淡出。

我們會使用隨附於 Microsoft ASP.NET MVC 架構中，若要建立的動畫效果的 jQuery 程式庫。 包含更新的索引檢視表中列出的 4。

**列出 4-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

請注意更新的索引檢視表包含三個新的 JavaScript 函式。 前兩個函式會使用 jQuery 淡出及淡入的連絡人清單，當您按一下 新連絡人的群組。 第三個函式會顯示錯誤訊息產生 Ajax 要求時，發生錯誤 （例如，網路逾時）。

第一個函式也會負責反白顯示選取的群組。 類別 = 選取的屬性加入至父項目 （LI 項目） 的已按下的項目。 同樣地，jQuery 容易，選取正確的項目並加入 CSS 類別。

這些指令碼會繫結至 Ajax.ActionLink() AjaxOptions 參數搭配群組連結。 更新的 Ajax.ActionLink() 方法呼叫看起來像這樣：

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>加入瀏覽器歷程記錄支援

一般來說，當您按一下 [更新] 頁面的連結，就會更新您的瀏覽器歷程記錄。 這樣一來，您可以按一下 [瀏覽器上一頁] 按鈕，以回到時間中先前的頁面狀態。 例如，如果您按一下好友連絡人群組，然後按一下 商務連絡人群組，您可以按一下 向後巡覽至之頁面的狀態時未選取的朋友連絡群組瀏覽器 上一頁 按鈕。

不幸的是，執行 Ajax 要求不會更新瀏覽器記錄自動。 如果您按一下連絡人群組，使用 Ajax 要求擷取的比對連絡人清單，然後瀏覽器歷程記錄不會更新。 若要導覽回到連絡人群組中，選取新的連絡人群組之後，您無法使用瀏覽器的 [上一頁] 按鈕。

如果您想要讓使用者能夠使用瀏覽器上一頁按鈕執行 Ajax 要求之後您就需要執行更多的工作。 您需要利用內建 ASP.NET AJAX Framework 的瀏覽器歷程記錄管理功能。

ASP.NET AJAX 瀏覽器歷程記錄，您需要做三件事：

1. 啟用瀏覽器歷程記錄 enableBrowserHistory 屬性設定為 true。
2. 藉由呼叫 addHistoryPoint() 方法的檢視狀態變更時，將儲存記錄點。
3. 巡覽事件引發時，重新建構檢視的狀態。

包含更新的索引檢視表中列出的 5。

**列出 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

在列出 5 中，瀏覽器歷程記錄已啟用 pageInit() 函式中。 PageInit() 函式也用於設定巡覽事件的事件處理常式。 當瀏覽器下一頁或上一頁 按鈕造成的頁面來變更狀態時，會引發導覽事件。

當您按一下連絡人群組，稱為 beginContactList() 方法。 這個方法會呼叫 addHistoryPoint() 方法來建立新的記錄點。 按一下連絡人群組的識別碼會新增至歷程記錄。

Expando 屬性連絡人群組連結會從擷取的群組識別碼。 連結會轉譯下列 Ajax.ActionLink() 呼叫。

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

最後一個參數傳遞至 Ajax.ActionLink() 加入名為 （小寫 XHTML 相容性） 連結的 groupid expando 屬性。

當使用者叫用的瀏覽器上一頁或下一頁 按鈕時，就會引發導覽事件和呼叫 navigate() 方法。 這個方法會更新以符合對應至瀏覽器歷程記錄點傳遞至瀏覽方法的頁狀態頁面中所顯示的連絡人。

## <a name="performing-ajax-deletes"></a>執行 Ajax 刪除

目前，若要刪除連絡人，您必須在 [刪除] 連結上按一下，然後按一下 [刪除確認] 頁面中顯示的 [刪除] 按鈕 （請參閱圖 2）。 這看起來很簡單，例如刪除資料庫記錄的要求頁面。


[![[刪除確認] 頁面](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**圖 02**： 刪除確認 頁面 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-cs/_static/image4.png))


很容易略過 [刪除確認] 頁面，並直接從索引檢視中刪除連絡人。 因為這種方式開啟您的應用程式的安全性漏洞，您應該避免此誘惑。 一般情況下，您不要想要執行 HTTP GET 作業，當叫用動作來修改 web 應用程式的狀態。 在執行 delete 時，您會想要執行 HTTP POST，或最好使用 HTTP DELETE 作業。

[刪除] 連結會包含在部分 ContactList。 部分 ContactList 的更新的版本都包含在程式碼範例 6。

**列出 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

[刪除] 連結會轉譯下列 Ajax.ImageActionLink() 方法呼叫：

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() 不是 ASP.NET MVC framework 的標準組件。 Ajax.ImageActionLink() 為連絡人管理員專案中包含自訂的 helper 方法。


AjaxOptions 參數有兩個屬性。 首先，確認屬性用來顯示快顯 JavaScript 確認對話方塊。 第二，HttpMethod 屬性用來執行 HTTP DELETE 作業。

列出 7 包含已新增至連絡人控制站的新 AjaxDelete() 動作。

**列出 7-Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

AjaxDelete() 動作是使用 AcceptVerbs 屬性加以裝飾。 這個屬性會阻止這個動作，除了 HTTP DELETE 作業以外的任何 HTTP 作業叫用。 特別是，您無法叫用此動作與 HTTP GET。

刪除資料庫記錄之後，您需要顯示更新的連絡人清單不包含已刪除的記錄。 AjaxDelete() 方法會傳回部分 ContactList 和更新的連絡人清單。

## <a name="summary"></a>總結

在這個反覆項目，我們加入 Ajax 功能連絡人管理員應用程式。 我們使用 Ajax 改善回應性和我們的應用程式的效能。

首先，我們重構索引檢視，以便按一下連絡人群組，不會更新整個檢視。 相反地，按一下連絡人群組只會更新連絡人的清單。

接下來，我們使用 jQuery 動畫效果淡出及淡入的連絡人清單。 將動畫加入至 Ajax 應用程式可以用來提供與瀏覽器進度列的對等應用程式的使用者。

我們也會加入至我們的 Ajax 應用程式的瀏覽器歷程記錄的支援。 我們已啟用使用者按一下瀏覽器上一頁及下一頁按鈕來變更索引檢視的狀態。

最後，我們會建立支援 HTTP DELETE 作業的刪除連結。 藉由執行 Ajax 刪除，我們會讓使用者刪除資料庫記錄而不需要使用者要求額外的刪除確認 頁面。

>[!div class="step-by-step"]
[上一頁](iteration-6-use-test-driven-development-cs.md)
[下一頁](iteration-1-create-the-application-vb.md)
