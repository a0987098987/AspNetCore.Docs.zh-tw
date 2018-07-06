---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 第 6 節： 使用資料註釋的模型驗證 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 6 部分將說明如何使用模型 V 的資料註解...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 10884f569f0f5d95517b73daab31fbd269a4726a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825949"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>第 6 節： 使用資料註釋的模型驗證
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 6 節涵蓋了使用資料註解的模型驗證。


我們透過我們建立和編輯表單的主要問題： 他們不用做任何驗證。 我們可以進行像是將必要的欄位空白或型別字母留在 [價格] 欄位中，我們會看到的第一個錯誤是來自資料庫，

我們可以將資料註解加入至我們的模型類別輕鬆地加入我們的應用程式的驗證。 資料註解，讓我們來描述套用至我們的模型屬性，我們想要的規則和 ASP.NET MVC 會負責強制執行的方式，並向使用者顯示適當的訊息。

## <a name="adding-validation-to-our-album-forms"></a>將驗證新增至我們的專輯表單

我們將使用下列資料註解屬性：

- **需要**– 指出屬性是否為必填的欄位
- **DisplayName** – 定義我們想要使用的表單欄位和驗證訊息的文字
- **StringLength** – 定義的字串欄位的最大長度
- **範圍**– 提供最大和最小值為數值欄位
- **繫結**-列出要繫結至模型屬性的參數或表單的值時，包含或排除的欄位
- **ScaffoldColumn** – 允許隱藏於編輯器表單的欄位

*注意： 如需有關使用資料註解屬性的模型驗證的詳細資訊，請參閱 MSDN 文件，*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

開啟專輯類別並新增下列*使用*陳述式。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

接下來，更新要加入顯示和驗證屬性，如下所示的屬性。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

雖然我們有，我們也已虛擬屬性來變更內容類型與演出者。 這可讓 Entity Framework 消極式載入它們為必要。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

在之後需要將這些屬性加入我們專輯的模型，我們建立與編輯畫面，立即開始驗證欄位，並使用顯示名稱我們選擇 (例如專輯封面 Url 而不是 AlbumArtUrl)。 執行應用程式，並瀏覽至 /StoreManager/Create。

![](mvc-music-store-part-6/_static/image1.png)

接下來，我們將會中斷某些驗證規則。 輸入價格為 0，並將標題保留空白。 當我們按一下 [建立] 按鈕時，我們會看到顯示有驗證錯誤訊息顯示哪些欄位不符合驗證規則，我們已定義的表單。

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>測試用戶端驗證

伺服器端驗證是應用程式的觀點而言，非常重要，因為使用者可以繞過用戶端驗證。 不過，只有實作伺服器端驗證網頁表單會表現出三個重要的問題。

1. 使用者必須等候表單張貼、 驗證的伺服器上，並為傳送至瀏覽器的回應。
2. 使用者不會立即回應，當它們會校正欄位，使它現在會將傳遞驗證規則。
3. 我們會浪費伺服器資源，才能執行驗證邏輯，而不是利用使用者的瀏覽器。

幸運的是，ASP.NET MVC 3 scaffold 樣板有用戶端驗證內建，需要對於任何其他工作。

輸入 [標題] 欄位中的單一字母，符合驗證需求，因此驗證訊息會立即移除。

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-5.md)
> [下一頁](mvc-music-store-part-7.md)
