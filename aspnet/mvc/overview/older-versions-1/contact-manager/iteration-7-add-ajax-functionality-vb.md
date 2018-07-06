---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: '反覆項目 #7 – 新增 Ajax 功能 (VB) |Microsoft Docs'
author: microsoft
description: 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: f5ea425c69dec803bfbdcb9f319a1d24e816bcf7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840128"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>反覆項目 #7 – 新增 Ajax 功能 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>建立連絡人管理 ASP.NET MVC 應用程式 (VB)
  

在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 每次反覆運算時，我們會逐漸改善應用程式。 此多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-讓應用程式看起來不錯。 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。

- 反覆項目 #3-新增表單驗證。 在第三個反覆項目，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-進行鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。 比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。 我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。

- 反覆項目 #6-使用測試導向開發。 在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。 在這個反覆項目，我們會新增連絡人群組。

- 反覆項目 #7-新增 Ajax 功能。 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。

## <a name="this-iteration"></a>這個反覆項目

在 Contact Manager 應用程式的這個反覆項目，我們可以重構我們的應用程式，才能使用 Ajax。 利用 Ajax，我們讓我們的應用程式回應速度更快。 我們可以避免當我們需要更新只有特定區域網頁中的呈現整個頁面。

我們將重構我們 [索引] 檢視，讓我們不 t 不必重新顯示整個頁面，只要有人選取新的連絡人群組。 相反地，當有人按一下連絡人群組，我們將只更新的連絡人清單，然後將其餘頁面的單獨。

我們也會變更我們刪除連結運作的方式。 不會顯示個別的確認頁面，我們會顯示 JavaScript 確認對話方塊。 如果您確認您想要刪除的連絡人，針對要從資料庫刪除的連絡人記錄之伺服器執行 HTTP DELETE 作業。

此外，我們也會利用 jQuery 來加入我們的 [索引] 檢視中的動畫效果。 正在從伺服器擷取新的連絡人清單時，我們會顯示動畫。

最後，我們也會利用 ASP.NET AJAX 架構支援管理瀏覽器歷程記錄。 每當我們執行更新的連絡人清單的 Ajax 呼叫時，我們將建立記錄點。 如此一來，瀏覽器向後和向前按鈕將會運作。

## <a name="why-use-ajax"></a>為何要使用 Ajax？

使用 Ajax，有許多優點。 首先，將 Ajax 功能新增到應用程式會導致較佳使用者體驗。 在一般的 web 應用程式中，整個頁面必須回傳至伺服器，每次使用者執行的動作。 每當您執行的動作時，瀏覽器鎖定和的使用者必須等到擷取和重新顯示整個頁面。

這是在桌面應用程式的情況下，無法接受的體驗。 不過，傳統上，我們有實質這個不正確的使用者體驗，在 web 應用程式因為我們不知道我們可以執行任何高愈好。 我們認為它是 web 應用程式的限制時，實際上，它只是我們 imaginations 的限制。

Ajax 應用程式中，您不會導致中止只為了更新頁面的使用者體驗的 t 需要。 相反地，您也可以更新該頁面背景中執行的非同步要求。 您不 t 強制使用者等候取得更新的網頁部分時。

利用 Ajax，您也可以改善您的應用程式的效能。 請考慮連絡管理員應用程式的運作方式現在不使用 Ajax 功能。 當您按一下連絡人群組時，就必須重新顯示整個索引檢視。 必須從資料庫伺服器擷取的連絡人清單和連絡人群組的清單。 所有這些資料必須傳遞透過網路從 web 伺服器到網頁瀏覽器。

我們將 Ajax 功能新增至我們的應用程式之後，不過，我們就可以避免當使用者按一下連絡人群組，請選擇整個頁面。 我們不需要再擷取從資料庫連絡人的群組。 我們也不 t 需要透過網路將整個 [索引] 檢視。 利用 Ajax，我們我們的資料庫伺服器必須執行的工作數量，我們減小我們的應用程式所需的網路傳輸量。

## <a name="don-t-be-afraid-of-ajax"></a>不要是怕的 Ajax

有些開發人員避免使用 Ajax，因為它們會擔心舊版瀏覽器。 他們想要確定其 web 應用程式將仍會運作時不支援 JavaScript 的瀏覽器來存取。 由於 Ajax 相依於 JavaScript，有些開發人員會避免使用 Ajax。

不過，如果您是實作 Ajax 的方式請小心您可以建置新版和舊版的瀏覽器所使用的應用程式。 我們的連絡人管理員應用程式以支援 JavaScript 的瀏覽器和瀏覽器不會使用。

如果您使用支援 JavaScript 的瀏覽器的連絡人管理員應用程式，您會有較佳使用者體驗。 例如，當您按一下連絡人群組，將會更新只會顯示 [連絡人] 頁面的區域。

如果相反地，您的瀏覽器不支援 JavaScript （或已停用 JavaScript），會在使用 Contact Manager 應用程式時，您會有較理想的使用者體驗。 例如，當您按一下連絡人群組，整個索引檢視必須公佈回瀏覽器以顯示符合的連絡人清單。

## <a name="adding-the-required-javascript-files"></a>新增必要的 JavaScript 檔案

我們必須將 Ajax 功能新增至我們的應用程式使用三個 JavaScript 檔案。 這三個檔案會包含在新的 ASP.NET MVC 應用程式的指令碼資料夾中。

如果您打算使用 Ajax 應用程式中的多個網頁中再合理納入您的應用程式檢視主版頁面中的所需的 JavaScript 檔案。 如此一來，JavaScript 檔案將會包含在所有應用程式中的頁面自動。

新增下列 JavaScript 包含了內&lt;head&gt;檢視主版頁面的標記：

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>重構來使用 Ajax 的索引檢視

可讓 s 開始先修改索引檢視，以便按一下連絡人群組只會更新此檢視會顯示連絡人的區域。 圖 1 中的紅色方塊包含我們想要更新的區域。


[![更新連絡人](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**圖 01**： 更新的連絡人 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-vb/_static/image2.png))


第一個步驟是檢視的個別，我們想要以非同步方式更新至不同的部分 （檢視使用者控制項） 的一部分。 顯示連絡人的 索引 檢視的區段已移至 清單 1 部分。

**列表 1-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

請注意，在 列表 1 中的部分有索引檢視與不同的模型。 *Inherits*屬性中&lt;%@ %頁&gt;指示詞會指定部分繼承 ViewUserControl&lt;群組&gt;類別。

已更新的 索引 檢視會包含在 列表 2。

**列表 2-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

有兩件事，您應該注意到關於列表 2 中更新的檢視。 首先，請注意，所有的內容移入部分取代 Html.RenderPartial() 呼叫。 第一次要求 [索引] 檢視以顯示一組初始的連絡人時，會呼叫 Html.RenderPartial() 方法。

其次，要請您注意的是用來顯示連絡人群組 Html.ActionLink() 已被取代 Ajax.ActionLink()。 Ajax.ActionLink() 呼叫具備下列參數：

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

第一個參數表示要顯示連結的文字，第二個參數代表路由值，而第三個參數代表的 Ajax 選項。 在此情況下，我們使用 UpdateTargetId Ajax 選項指向 HTML &lt;div&gt;我們想要在 Ajax 要求完成之後更新的標記。 我們想要更新&lt;div&gt;標記，並且具有新的連絡人清單。

已更新的 index （） 方法，請連絡控制站都包含在 列表 3。

**列表 3-Controllers\ContactController.vb （Index 方法）**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

更新的 index （） 動作有條件地傳回下列其中一種。 如果 index （） 動作會叫用 Ajax 要求的控制器就會傳回部分。 否則，index （） 動作會傳回整個檢視。

請注意，index （） 動作不需要傳回的資料量時叫用 Ajax 要求。 在一般的要求內容中，索引動作會傳回所有連絡人的群組和選取的連絡人群組的清單。 在 Ajax 要求的內容中，index （） 動作會傳回選取的群組。 Ajax 表示較少的工作，在您的資料庫伺服器上。

我們已修改的索引檢視的運作方式在新版和舊版的瀏覽器的情況下。 如果您按一下連絡人群組，而且您的瀏覽器支援 JavaScript，只有包含連絡人清單的檢視區域會更新。 如果相反地，您的瀏覽器不支援 JavaScript，則會更新整個檢視。


我們已更新的 [索引] 檢視有一個問題。 當您按一下連絡人群組時，是不反白顯示選取的群組。 因為群組的清單會顯示為 Ajax 要求期間，會更新區域之外，不會不反白顯示正確的群組。 我們將修正此問題下, 一節。


## <a name="adding-jquery-animation-effects"></a>加入 jQuery 動畫效果

一般來說，當您按一下連結，以在網頁中的，您可以使用瀏覽器進度列來偵測在瀏覽器正在主動擷取更新的內容。 執行時的 Ajax 要求，相反地，在瀏覽器進度列不會顯示任何進度。 這可讓使用者感到不安。 如何知道是否有凍結的瀏覽器？

有數種方式，您可以指出要執行的 Ajax 要求時執行工作。 其中一個方法是顯示簡單的動畫。 例如，您可以淡出區域時的 Ajax 要求開始，並要求完成時，在區域淡出。

我們將使用隨附於 Microsoft ASP.NET MVC 架構，來建立動畫效果的 jQuery 程式庫。 已更新的 索引 檢視包含在 列表 4 中。

**列表 4-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

請注意更新的 [索引] 檢視包含三個新的 JavaScript 函式。 前兩個函式會使用 jQuery 淡出及淡入的連絡人清單，當您按一下 新連絡人群組。 第三個函式會顯示錯誤訊息時的 Ajax 要求結果，造成錯誤 （例如網路逾時）。

第一個函式也會負責反白顯示選取的群組。 類別 = 選取的屬性加入至按下之項目的父項目 （LI 項目）。 同樣地，jQuery 輕鬆選取正確的項目並加入的 CSS 類別。

這些指令碼會繫結至 Ajax.ActionLink() AjaxOptions 參數的協助的群組連結。 已更新的 Ajax.ActionLink() 方法呼叫看起來像這樣：

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>加入瀏覽器歷程記錄支援

一般來說，當您按一下 [更新] 頁面的連結時，會更新您的瀏覽器歷程記錄。 如此一來，您可以按一下 [瀏覽器上一頁] 按鈕，將先前的狀態之頁面的時間移回。 例如，如果您按一下朋友連絡人群組，然後按一下 商務連絡人群組，您可以按一下 向後巡覽至頁面的狀態時的朋友連絡群組已選取瀏覽器 上一頁 按鈕。

不幸的是，執行 Ajax 要求不會更新瀏覽器記錄自動。 如果您按一下連絡人群組，和 Ajax 要求擷取相符的連絡人的清單，則不會更新瀏覽器歷程記錄。 若要瀏覽回到連絡人群組，選取新的連絡人群組之後，您無法使用瀏覽器的 [上一頁] 按鈕。

如果您要讓使用者能夠使用瀏覽器上一頁按鈕執行 Ajax 要求之後您就需要執行多一點的工作。 您需要利用內建的 ASP.NET AJAX 架構的瀏覽器歷程記錄管理功能。

ASP.NET AJAX 瀏覽器歷程記錄，您需要做三件事：

1. EnableBrowserHistory 屬性設定為 true，以啟用瀏覽器歷程記錄。
2. 藉由呼叫 addHistoryPoint() 方法的檢視狀態變更時，請將儲存記錄點。
3. Navigate 事件引發時，請重建檢視狀態。

已更新的 索引 檢視會包含在 列表 5。

**列表 5-Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

表 5 中，已啟用 pageInit() 函式中的瀏覽器歷程記錄。 PageInit() 函式也會設為 navigate 事件的事件處理常式。 當瀏覽器下一頁或上一頁 按鈕會造成的頁面來變更狀態時，會引發 navigate 事件。

當您按一下 連絡人群組，稱為 beginContactList() 方法。 這個方法會藉由呼叫 addHistoryPoint() 方法建立新的記錄點。 按一下連絡人群組的識別碼會新增至歷程記錄。

群組識別碼被擷取自 expando 屬性 [連絡人群組] 連結。 使用下列呼叫來 Ajax.ActionLink() 呈現連結。

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

最後一個參數傳遞至 Ajax.ActionLink() 新增名為 groupid （小寫的 XHTML 相容性） 連結的 expando 屬性。

當使用者叫用的瀏覽器上一頁或下一頁按鈕時，瀏覽事件引發時，並呼叫 navigate() 方法。 這個方法會更新以符合對應到傳遞至 navigate 方法瀏覽器歷程記錄點之頁面的狀態 頁面中顯示的連絡人。

## <a name="performing-ajax-deletes"></a>執行 Ajax 刪除

目前，若要刪除的連絡人，您必須按一下 刪除 連結，然後按一下 刪除 確認頁面中顯示的 刪除 按鈕 （請參閱 圖 2）。 這似乎是相當繁重的頁面要求執行一些簡單，例如刪除資料庫記錄。


[![刪除確認頁面](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**圖 02**： 刪除確認頁面 ([按一下以檢視完整大小的影像](iteration-7-add-ajax-functionality-vb/_static/image4.png))


相當吸引人，請略過刪除確認頁面，並直接從 [索引] 檢視中刪除連絡人。 您應該避免起舞，因為這種方式開啟您的應用程式的安全性漏洞。 一般情況下，您不要想要叫用動作來修改您的 web 應用程式的狀態時，執行 HTTP GET 作業。 在執行刪除時，您會想要執行 HTTP POST，或最好使用 HTTP DELETE 作業。

[刪除] 連結會包含在部分 ContactList。 部分 ContactList 的更新的版本都包含在 列表 6。

**列表 6-Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

刪除連結會轉譯下列 Ajax.ImageActionLink() 方法呼叫：

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() 不是 ASP.NET MVC framework 的標準部分。 Ajax.ImageActionLink() 是 Contactmanager 專案中包含自訂的 helper 方法。


AjaxOptions 參數有兩個屬性。 首先，確認屬性用來顯示快顯 JavaScript 確認對話方塊。 第二，HttpMethod 屬性用來執行 HTTP DELETE 作業。

列表 7 包含新的 AjaxDelete() 動作已新增至連絡人控制器。

**列表 7-Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

AcceptVerbs 屬性裝飾 AjaxDelete() 動作。 這個屬性會防止以外的 HTTP DELETE 作業以外的任何 HTTP 作業所叫用動作。 特別是，您無法叫用此動作，使用 HTTP GET。

刪除資料庫記錄之後，您需要顯示更新的連絡人清單不包含已刪除的資料錄。 AjaxDelete() 方法會傳回部分 ContactList 和更新的連絡人清單。

## <a name="summary"></a>總結

這個反覆項目，在中，我們會新增 Ajax 功能到我們的連絡人管理員應用程式。 我們使用 Ajax，以改善回應性和我們的應用程式的效能。

首先，我們重構 [索引] 檢視，以便按一下連絡人群組，不會更新整個檢視。 相反地，按一下連絡人群組只會更新連絡人清單。

接下來，我們使用 jQuery 動畫效果淡出及淡入的連絡人清單。 將動畫新增至 Ajax 應用程式可以用來提供應用程式的瀏覽器進度列的對等的使用者。

我們也會加入至我們的 Ajax 應用程式的瀏覽器歷程記錄 」 支援。 我們已啟用使用者按一下 [瀏覽器上一頁及下一頁按鈕來變更索引] 檢視的狀態。

最後，我們會建立支援 HTTP DELETE 作業的刪除連結。 藉由執行 Ajax 刪除，我們可以讓使用者能夠刪除資料庫中的記錄，而不需要使用者要求額外的刪除確認頁面。

> [!div class="step-by-step"]
> [上一步](iteration-6-use-test-driven-development-vb.md)
