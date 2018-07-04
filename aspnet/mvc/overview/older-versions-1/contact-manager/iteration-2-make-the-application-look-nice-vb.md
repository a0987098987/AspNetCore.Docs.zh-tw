---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: '反覆項目 #2 – 讓應用程式看起來不錯 (VB) |Microsoft Docs'
author: microsoft
description: 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: c1209a925a43bd7846a9dc07ce557c55bb1827ae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381101"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>反覆項目 #2 – 讓應用程式看起來不錯 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>建立連絡人管理 ASP.NET MVC 應用程式 (VB)
  

在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您儲存連絡資訊 – 名稱、 電話號碼和電子郵件地址 – 如需清單的人員。

我們會建置應用程式透過多個反覆項目。 每次反覆運算時，我們會逐漸改善應用程式。 此多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1 – 建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2 – 讓應用程式看起來不錯。 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。

- 反覆項目 #3 – 新增表單驗證。 在第三個反覆項目，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4 – 讓應用程式鬆散耦合。 在此第三個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。 比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。

- 反覆項目 #5 – 建立單元測試。 在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。 我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。

- 反覆項目 #6 – 使用測試導向開發。 在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。 在這個反覆項目，我們會新增連絡人群組。

- 反覆項目 #7 – 新增 Ajax 功能。 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。

## <a name="this-iteration"></a>這個反覆項目

這個反覆項目的目的是要提升的連絡人管理員應用程式的外觀。 目前，連絡管理員會使用預設 ASP.NET MVC 檢視主版頁面和階層式樣式表的項目，（請參閱 圖 1）。 這些不要看起來不正確，但我不想要連絡管理員，要看起來就像每個其他的 ASP.NET MVC 網站。 我想要自訂的檔案中取代這些檔案。


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**圖 01**： 在 ASP.NET MVC 應用程式的預設外觀 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


在這個反覆項目，我會討論兩種方法來改善我們的應用程式的視覺化設計。 首先，我示範如何利用 ASP.NET MVC 設計資源庫下載免費的 ASP.NET MVC 設計範本。 ASP.NET MVC 設計資源庫可讓您建立專業的 web 應用程式，而不執行任何工作。

我決定不使用範本從 ASP.NET MVC 設計資源庫的連絡人管理員應用程式。 相反地，我的專業設計公司所建立的自訂設計。 在本教學課程的第二個部分中，我可以說明我曾經與專業設計公司建立的最終的 ASP.NET MVC 設計的方式。

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC 設計資源庫

ASP.NET MVC 設計資源庫是 Microsoft 所提供的免費資源。 ASP.NET MVC 組件庫位於下列位址：

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC 的設計庫裝載建立專為在 ASP.NET MVC 專案中使用的免費網站設計的集合。 設計是由社群成員的上傳。 資源庫的訪客可以投票給他們最愛的設計 （請參閱 圖 2）。


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**圖 02**: 將 ASP.NET MVC 設計資源庫 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


當我撰寫本教學課程中，資源庫中最受歡迎的設計是由 David Hauser 命名年 10 月的設計。 您可以使用 ASP.NET MVC 專案的這項設計藉由完成下列步驟：

1. 按一下 [**下載**October.zip 檔案下載到您的電腦] 按鈕。
2. 以滑鼠右鍵按一下 下載的 October.zip 檔案，然後按一下 **解除封鎖**按鈕 （請參閱 圖 3）。
3. 解壓縮檔案以名為年 10 月的資料夾。
4. 從 10 月資料夾中包含 DesignTemplate 資料夾中選取的所有檔案，以滑鼠右鍵按一下檔案，並選取功能表選項**複製**。
5. 以滑鼠右鍵按一下 [ContactManager] 專案節點，在 Visual Studio 方案總管] 視窗中的，然後選取功能表選項**貼上**（請參閱 [圖 4）。
6. 選取 [Visual Studio] 功能表選項**編輯、 尋找和取代 快速取代**，並取代 *[MyProjectName]* 具有*ContactManager* （請參閱 [圖 5]）。


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**圖 03**： 從 web 解除封鎖檔案下載 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**圖 04**： 覆寫在 [方案總管] 中的檔案 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**圖 05**: [ProjectName] 取代為 ContactManager ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


完成這些步驟之後，您的 web 應用程式將使用新的設計。 [圖 6] 頁面會說明與年 10 月設計的連絡人管理員應用程式的外觀。


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**圖 06**： 使用年 10 月範本 ContactManager ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>建立自訂的 ASP.NET MVC 設計

ASP.NET MVC 設計資源庫上有不同的設計樣式的好選項。 資源庫為您提供較輕鬆的方法，以自訂您的 ASP.NET MVC 應用程式的外觀。 此外，當然，資源庫中具有最大的好處，就是完全免費。

不過，您可能需要建立完全專為您的網站設計。 在此情況下，它可以合理地使用網站設計公司。 我決定採取這種方式，請連絡管理員應用程式的設計。

我會壓縮設定連絡管理員反覆項目 # 1，並傳送設計公司的專案。 它們沒有 Visual Studio （可惜在其上 ！），但該嘛 t 出現問題。 他們能夠從免費下載 Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net)網站並開啟在 Visual Web Developer 中的連絡人管理員應用程式。 在幾天，它們必須產生圖 7 中的設計。


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**圖 07**: ASP.NET MVC 的連絡人管理員設計 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


新的設計是由兩個主要的檔案所組成： 新的階層式樣式表檔案和新的檢視主版頁面檔案。 檢視主版頁面包含配置和 ASP.NET MVC 應用程式中檢視共用的內容。 比方說，檢視主版頁面會包含標頭、 導覽索引標籤中和頁尾會出現 [圖 7] 中。 我已在將現有 Site.Master 檢視主版頁面 Views\Shared 資料夾中的以新的 Site.Master 檔案，從設計公司，覆寫

設計公司也會建立新的階層式樣式表和影像集。 我的內容資料夾中放置這些新的檔案，並已覆寫現有的 Site.css 檔案。 您應該將所有靜態內容的內容資料夾中。

請注意，新的設計為連絡人管理員中，包含編輯和刪除連絡人的映像。 編輯和刪除映像旁邊連絡人的 HTML 資料表中的每位連絡人。

一開始以 HTML 呈現這些連結。ActionLink() 協助程式，像這樣：

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Html.ActionLink() 方法不支援映像 （方法 HTML 編碼的連結文字，基於安全性理由）。 因此，我會使用 Url.Action() 像這樣的呼叫取代 Html.ActionLink() 呼叫：

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Html.ActionLink() 方法會呈現整個的 HTML 超連結。 Url.Action() 方法時，相反地，呈現不含 URL &lt;&gt;標記。

此外，請注意，新的設計，包含選取或未選取索引標籤。 例如，在 圖 8**建立新的連絡人**已選取索引標籤和**我的連絡人**未選取索引標籤。


[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**圖 08**： 選取和取消選取索引標籤 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


若要支援轉譯選取或未選取索引標籤，我建立了名為 MenuItemHelper 自訂 HTML 協助程式。 這個 helper 方法會呈現可能&lt;li&gt;標記或&lt;li 類別 = 選取&gt;取決於目前的控制器和動作是否對應至控制器和動作名稱傳遞給協助專家的標記。 在 列表 1 中包含 MenuItemHelper 的程式碼。

**列表 1 – Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

MenuItemHelper TagBuilder 類別在內部用來建置&lt;li&gt; HTML 標記。 TagBuilder 類別是每當您需要建立新的 HTML 標記，您可以使用一個非常實用的公用程式類別。 它包含了新增屬性、 新增 CSS 類別，產生識別碼，以及修改標記的方法內部 HTML。

## <a name="summary"></a>總結

在此反覆項目，我們已改善我們的 ASP.NET MVC 應用程式的視覺化設計。 首先，已向您介紹 ASP.NET MVC 設計資源庫。 您已了解如何從 ASP.NET MVC 應用程式中，您可以使用 ASP.NET MVC 設計資源庫下載免費的設計範本。

接下來，我們會討論如何建立自訂的設計，以及在藉由修改預設階層式樣式表檔案和主版檢視頁面檔案。 為了支援新的設計，我們必須對我們的連絡人管理員應用程式中進行一些微幅的變更。 比方說，我們已新增新的 HTML 協助程式，名為 MenuItemHelper 顯示所選和取消選取索引標籤。

中的下一個反覆項目中，我們會處理驗證的極為重要的主題。 讓使用者無法建立新的連絡人，而不需要先提供必要的值，例如： 個人 s 和姓氏，我們可以加入至應用程式的驗證程式碼。

> [!div class="step-by-step"]
> [上一頁](iteration-1-create-the-application-vb.md)
> [下一頁](iteration-3-add-form-validation-vb.md)
