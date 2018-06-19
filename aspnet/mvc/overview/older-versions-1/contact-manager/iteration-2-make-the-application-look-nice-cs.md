---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: '反覆項目 #2 – 讓應用程式看起來不錯 (C#) |Microsoft 文件'
author: microsoft
description: 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: cad28fb6ff02625674e59674d1ec08d52373c269
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871840"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>反覆項目 #2 – 讓應用程式看起來不錯 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。


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

這個反覆項目的是改善連絡人管理員應用程式的外觀。 目前，請連絡管理員會使用預設 ASP.NET MVC 檢視主版頁面和階層式樣式表 （請參閱圖 1）。 這些不要看起來不正確，但我不想要看起來就像每個 ASP.NET MVC 網站連絡管理員。 我想要自訂的檔案中取代這些檔案。


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**圖 01**: ASP.NET MVC 應用程式的預設外觀 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


在這個反覆項目，我將討論兩種方法來改善我們的應用程式的視覺化設計。 首先，我示範您如何利用 ASP.NET MVC 設計資源庫下載免費的 ASP.NET MVC 設計範本。 ASP.NET MVC 設計組件庫可讓您建立專業的 web 應用程式，而不需要執行任何工作。

我決定不使用樣板庫中的 ASP.NET MVC 設計連絡人管理員應用程式。 相反地，我必須專業人員設計公司所建立的自訂設計。 在本教學課程的第二個部分中，我將說明如何我曾經與工作來建立最終的 ASP.NET MVC 設計專業人員設計公司。

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC 設計組件庫

ASP.NET MVC 設計庫是由 Microsoft 提供的免費資源。 ASP.NET MVC 組件庫位於下列位址：

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC 設計庫主控免費網站設計特別針對使用 ASP.NET MVC 專案中所建立的集合。 設計都會上傳由社群的成員。 組件庫的訪客可以在此投票他們最愛的設計 （請參閱圖 2）。


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**圖 02**: ASP.NET MVC 設計組件庫 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


當我撰寫此教學課程中，在圖庫中最常用的設計是 David Hauser 命名年 10 月的設計。 您可以使用 ASP.NET MVC 專案的這項設計藉由完成下列步驟：

1. 按一下**下載**October.zip 檔案下載到您的電腦 按鈕。
2. 以滑鼠右鍵按一下 下載的 October.zip 檔案，然後按一下**解除封鎖**按鈕 （請參閱圖 3）。
3. 解壓縮檔案到名為年 10 月的資料夾。
4. 選取的所有檔案包含在年 10 月資料夾 DesignTemplate 資料夾中，以滑鼠右鍵按一下檔案，並選取功能表選項**複製**。
5. 以滑鼠右鍵按一下 ContactManager 專案節點，在 Visual Studio 方案總管 視窗中的，然後選取功能表選項**貼上**（請參閱圖 4）。
6. 選取 [Visual Studio] 功能表選項**編輯、 尋找和取代 快速取代**和取代 *[MyProjectName]* 與*ContactManager* （請參閱圖 5）。


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**圖 03**： 解除封鎖檔案從 web 下載 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**圖 04**： 覆寫在 [方案總管] 中的檔案 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**圖 05**: ContactManager 以取代 [ProjectName] ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


完成這些步驟之後，web 應用程式會使用新的設計。 圖 6 中的頁面說明與年 10 月設計連絡人管理員應用程式的外觀。


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**圖 06**: ContactManager 年 10 月範本 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>建立自訂 ASP.NET MVC 設計

ASP.NET MVC 設計庫有良好的不同的設計樣式選取範圍。 組件庫可提供您較輕鬆的方法，以自訂 ASP.NET MVC 應用程式的外觀。 而且，當然，組件庫的大優點是完全免費。

不過，您可能需要建立完全獨特的設計為您的網站。 在此情況下，因此才會使用網站設計公司。 我決定採取這種方式為連絡人管理員應用程式的設計。

我壓縮反覆項目 # 1 向上連絡管理員，並傳送到設計公司的專案。 他們並未擁有 Visual Studio （遺憾上 ！），但該 professionals t 出現問題。 它們是能從免費下載 Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net)網站並開啟連絡人管理員應用程式在 Visual Web Developer。 在幾天，它們必須產生的圖 7 中的設計。


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**圖 07**: 在 ASP.NET MVC，請連絡管理員設計 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


新的設計是由兩個主要的檔案所組成： 新的階層式樣式表檔案和新的檢視主版頁面檔案。 檢視主版頁面包含的配置和 ASP.NET MVC 應用程式中檢視共用的內容。 例如，檢視主版頁面濆迶 7 包含標頭、 導覽索引標籤上和會出現的頁尾。 我使用設計公司中，新 Site.Master 檔案覆寫現有 Site.Master 檢視主版頁面在 Views\Shared 資料夾

設計公司也會建立新的階層式樣式表和一組影像。 我的內容資料夾中放置這些新的檔案，並覆寫現有的 Site.css 檔案。 您應該將所有的靜態內容的內容資料夾中。

請注意，新的設計為連絡人管理員包含映像，以編輯和刪除連絡人。 編輯和刪除映像出現在旁邊連絡人的 HTML 資料表中每位連絡人。

一開始由與 HTML 轉譯這些連結。ActionLink() helper，像這樣：

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Html.ActionLink() 方法不支援映像 （方法 HTML 編碼的連結文字，基於安全性理由）。 因此，我取代 Html.ActionLink() 呼叫呼叫 Url.Action() 像這樣：

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Html.ActionLink() 方法呈現整個 HTML 超連結。 Url.Action() 方法，相反地，呈現不含 URL &lt;&gt;標記。

此外，請注意，新的設計包含選取和取消選取索引標籤。 例如，在圖 8**建立新的連絡人**選取索引標籤和**我的連絡人**不選取索引標籤。


[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**圖 08**： 選取和取消選取索引標籤 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


若要支援呈現已選取和取消選取索引標籤，建立名為 MenuItemHelper 的自訂 HTML helper。 這個 helper 方法轉譯是&lt;li&gt;標記或&lt;li 類別 ="選取"&gt;根據目前的控制器和動作是否對應至控制器和動作名稱傳遞給協助專家的標記。 MenuItemHelper 的程式碼會包含在程式碼範例 1。

**Listing 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper TagBuilder 類別在內部用來建置&lt;li&gt; HTML 標記。 TagBuilder 類別是非常有用的公用程式類別，可讓您視需要來建立新的 HTML 標記。 它包含了加入屬性、 加入 CSS 類別、 產生識別碼，以及修改標記的方法內部 HTML。

## <a name="summary"></a>總結

在這個反覆項目，我們可以改進我們的 ASP.NET MVC 應用程式的視覺化設計。 首先，我們將會介紹至 ASP.NET MVC 設計組件庫。 您已學習如何從您可以在 ASP.NET MVC 應用程式中使用 ASP.NET MVC 設計庫下載免費的設計範本。

接下來，我們將討論如何建立自訂的設計，藉由修改預設階層式樣式表檔案和主版檢視頁面檔案。 為了支援新的設計，我們必須對我們的連絡人管理員應用程式進行一些微幅變更。 比方說，我們加入了新的 HTML helper，名為 MenuItemHelper 顯示選取和取消選取索引標籤。

中的下一個反覆項目，我們可以處理非常重要的主體的驗證。 使使用者無法建立新的連絡人，而不需要先提供必要的值，例如個人 s 和姓氏，我們可以加入至我們的應用程式的驗證程式碼。

> [!div class="step-by-step"]
> [上一頁](iteration-1-create-the-application-cs.md)
> [下一頁](iteration-3-add-form-validation-cs.md)
