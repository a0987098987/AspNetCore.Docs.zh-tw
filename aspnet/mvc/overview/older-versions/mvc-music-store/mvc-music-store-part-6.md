---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 第 6 單元： 使用資料註解的模型驗證 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 6 單元涵蓋 V 模型使用資料註解...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872412"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>第 6 單元： 使用資料註解的模型驗證
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 6 單元涵蓋的模型驗證中使用資料註解。


我們對我們的建立與編輯表單的主要問題： 他們不進行任何驗證。 我們可以執行一些作業，例如將必要的欄位空白或型別字母留在 [價格] 欄位中，我們會看到的第一個錯誤是來自資料庫。

我們可以將資料註解加入至我們的模型類別輕鬆加入我們的應用程式的驗證。 資料附註可讓我們來描述套用至我們的模型屬性，我們想要的規則和 ASP.NET MVC 會負責強制執行的方式，並向使用者顯示適當的訊息。

## <a name="adding-validation-to-our-album-forms"></a>將驗證加入至我們的專輯表單

我們將使用下列資料註解屬性：

- **需要**– 表示此屬性是必要的欄位
- **DisplayName** – 定義我們想要使用的表單欄位和驗證訊息的文字
- **StringLength** – 定義字串欄位的最大長度
- **範圍**– 提供數值欄位的最大和最小值
- **繫結**-列出要繫結模型屬性的參數或表單的值時，包含或排除的欄位
- **ScaffoldColumn** – 允許隱藏編輯器表單中的欄位

*注意： 如需有關使用資料註解屬性的模型驗證的詳細資訊，請參閱 MSDN 文件，*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

開啟專輯類別並加入下列*使用*上方的陳述式。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

接下來，更新屬性，將顯示和驗證屬性，如下所示。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

雖然我們有，我們也已經虛擬屬性來變更音樂類型與演出者。 這可讓 Entity Framework 了延遲載入它們為必要。

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

之後需要這些屬性加入我們的專輯模型中，我們建立與編輯螢幕立即開始驗證欄位，並使用顯示名稱我們選擇 (例如專輯封面 Url 中而非 AlbumArtUrl)。 執行應用程式，並瀏覽至 /StoreManager/Create。

![](mvc-music-store-part-6/_static/image1.png)

接下來，我們將會中斷某些驗證規則。 輸入價格為 0，並將標題保留空白。 當我們按一下 [建立] 按鈕時，我們會看到顯示驗證錯誤訊息顯示哪些欄位不符合驗證規則，我們已定義的表單。

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>測試用戶端驗證

伺服器端驗證是從應用程式的觀點而言，非常重要，因為使用者可以規避用戶端驗證。 但是，只實作伺服器端驗證的網頁表單會呈現三個重要的問題。

1. 使用者必須等候表單張貼、 驗證的伺服器上，且傳送至瀏覽器的回應。
2. 使用者不會收到立即回應，當它們更正欄位，使它現在通過驗證規則。
3. 我們會浪費伺服器資源用來執行驗證邏輯，而不是利用使用者的瀏覽器。

幸運的是，ASP.NET MVC 3 scaffold 範本具有用戶端驗證內建的需要採取任何其他恕不另行通知。

在 [標題] 欄位中輸入單一字母滿足驗證需求，因此驗證訊息會立即移除。

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-5.md)
> [下一頁](mvc-music-store-part-7.md)
