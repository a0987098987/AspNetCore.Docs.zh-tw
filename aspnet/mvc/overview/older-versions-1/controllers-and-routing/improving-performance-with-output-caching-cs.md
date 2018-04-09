---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: 改善效能與輸出快取 (C#) |Microsoft 文件
author: microsoft
description: 在此教學課程中，您學會如何您可以大幅改善效能的 ASP.NET MVC web 應用程式藉由運用輸出快取。 您...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 8958caa5a0ccad669ca861bed261102625be5cb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="improving-performance-with-output-caching-c"></a>使用輸出快取 (C#) 改善效能
====================
by [Microsoft](https://github.com/microsoft)

> 在此教學課程中，您學會如何您可以大幅改善效能的 ASP.NET MVC web 應用程式藉由運用輸出快取。 您了解如何快取，因此相同的內容不需要建立新的使用者叫用的動作，每次從控制器動作傳回的結果。


本教學課程的目標是以說明您如何可以大幅改進 ASP.NET MVC 應用程式的效能利用輸出快取。 輸出快取可讓您快取由控制器動作傳回的內容。 這樣一來，相同的內容並不需要在每次叫用相同的控制器動作產生。

例如，假設 ASP.NET MVC 應用程式，會在名為索引檢視中顯示資料庫記錄的清單。 一般來說，每個使用者叫用傳回的索引檢視中，控制器動作的時間的資料庫記錄集必須擷取從資料庫執行資料庫查詢。

如果您充分利用輸出快取的相反地，您可以避免執行資料庫查詢，每次任何使用者叫用相同的控制器動作。 檢視可以擷取從快取，而不是從控制器動作正在重新產生。 您可以避免執行重複快取可讓在伺服器上運作。

## <a name="enabling-output-caching"></a>啟用輸出快取

您可以啟用輸出快取加入至個別的控制器動作或整個控制器類別 [OutputCache] 屬性。 例如，列表 1 中的控制站會公開名為 index （） 的動作。 Index 動作的輸出快取 10 秒。

**列出 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

ASP.NET MVC 的 Beta 版本，在輸出快取不適用於 URL，例如[ http://www.MySite.com/ ](http://www.mysite.com/)。 相反地，您必須輸入 URL，例如[ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index)。 

在清單 1 中，index （） 動作的輸出快取 10 秒。 如果您想要的話，您可以指定較長的快取持續時間。 例如，如果您想要快取輸出的控制器動作，一天然後您可以指定快取持續時間為 86400 秒 (60 秒\*60 分鐘\*24 小時)。

沒有內容也不保證會被快取，您指定的時間長度。 當記憶體資源不足時，快取會自動啟動收回的內容。

主控制器清單 1 中的傳回列表 2 中的索引檢視。 沒有特殊這份檢視相關的項目。 索引檢視只會顯示目前的時間 （請參閱圖 1）。

**列出 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**圖 1 – 快取索引檢視**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

如果您叫用的 index 動作多次在您的瀏覽器的網址列中輸入 URL /Home/索引和重複叫用您的瀏覽器中的 [重新整理/重新載入] 按鈕，然後索引檢視所顯示的時間不會變更為 10 秒。 因為此檢視會快取，會顯示相同的時間。

請務必了解每一位使用者造訪您的應用程式相同的檢視，會快取。 叫用 index （） 的動作的任何人會收到相同的快取的版本的索引檢視。 這表示 web 伺服器必須執行來處理索引檢視的工作數量就能大幅降低。

列出 2 中的檢視會做的動作非常簡單的項目。 檢視只會顯示目前的時間。 不過，您可以如同輕鬆地快取顯示一組資料庫記錄的檢視。 在此情況下，資料庫中的記錄組會不需要從資料庫擷取，每次叫用傳回檢視的控制器動作。 快取可以減少您的 web 伺服器和資料庫伺服器必須執行的工作數量。

不使用頁面&lt;%@ OutputCache %&gt; MVC 檢視中的指示詞。 這個指示詞透過從 Web Form 世界滴墨-並不能用於 ASP.NET MVC 應用程式。

## <a name="where-content-is-cached"></a>快取內容

根據預設，當您使用 [OutputCache] 屬性中，內容會快取中三個位置： 網頁伺服器、 任何 proxy 伺服器及網頁瀏覽器。 您可以控制完全內容快取所在藉由修改 [OutputCache] 屬性的 Location 屬性。

您可以將 [位置] 屬性為下列值之一：

> ·任何
> 
> ·用戶端
> 
> ·下游
> 
> ·伺服器
> 
> ·無
> 
> ·ServerAndClient


根據預設，[位置] 屬性任何具有值。 不過，有很多情況下您可以只在瀏覽器上，或只能在伺服器上的快取。 例如，如果您要快取的每位使用者個人化的資訊然後您應該快取伺服器上的資訊。 如果您要顯示不同的資訊給不同的使用者接著您應該快取只會在用戶端上的資訊。

例如，列表 3 中的控制站會公開名為 GetName() 傳回目前的使用者名稱的動作。 如果插頭登入網站，並叫用 GetName() 動作然後動作會傳回字串"Hi 端子"。 如果接下來，Jill 登入網站，並叫用 GetName() 動作然後她也會取得字串"Hi 端子"。 字串會針對所有使用者在網頁伺服器上快取的後端子一開始會叫用控制器的動作。

**列出 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

最有可能列表 3 中的控制站無法您想要的方式。 您不想顯示"Hi 端子 」 訊息給 Jill。

您應該永遠不會快取伺服器快取中的個人化的內容。 不過，您可能想要快取個人化的內容，在瀏覽器快取以改善效能。 如果您快取內容，在瀏覽器中，而且使用者叫用相同的控制器動作數次，內容可以擷取從瀏覽器快取，而非伺服器。

修改的控制器中列出的 4 會快取 GetName() 動作的輸出。 不過，只有瀏覽器和伺服器上不會快取內容。 這樣一來，當多位使用者叫用 GetName() 方法時，每位人員取得自己的使用者名稱和不可為另一個人的使用者名稱。

**列出 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

請注意 [OutputCache] 屬性中列出的 4 包含設定為值 OutputCacheLocation.Client Location 屬性。 [OutputCache] 屬性也包含 NoStore 屬性。 NoStore 屬性用來通知 proxy 伺服器與瀏覽器，它們不應該儲存快取內容的永久複本。

## <a name="varying-the-output-cache"></a>不同的輸出快取

在某些情況下，您可以使用不同的快取的版本的完全相同的內容。 例如，假設您要建立主要/詳細資料頁面。 主版頁面會顯示一份電影標題。 當您按一下標題時，您會取得詳細資訊，針對選取的影片。

如果您要快取的詳細資料頁面，然後相同的電影的詳細資料隨即出現的電影不論您按一下。 未來的所有使用者，將會顯示第一個使用者所選的第一部電影。

您可以利用 [OutputCache] 屬性的 VaryByParam 屬性來解決這個問題。 這個屬性可讓您建立不同的快取的版本的完全相同的內容時的表單參數或查詢字串參數而有所不同。

例如，列出 5 中的控制站會公開兩個名為 Master() 和 Details() 的動作。 Master() 動作將會傳回一份電影標題及 Details() 動作將會傳回選取的影片的詳細資料。

**列出 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Master() 動作包括 VaryByParam 屬性的值"none"。 當叫用動作時的 Master() 相同快取版本的主要檢視會傳回。 參數是任何形式的參數或查詢字串忽略 （請參閱圖 2）。

**圖 2 – /Movies/Master 檢視**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**圖 3 – / 電影/詳細資料檢視**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Details() 動作包括 VaryByParam 屬性與值 「 識別碼 」。 不同的 Id 參數的值傳遞至控制器動作，都會產生不同的快取的版本的詳細資料檢視。

它是一定要了解使用的 VaryByParam 屬性會導致多個快取，且不小於。 不同的快取的版本的詳細資料檢視會建立每個不同版本的識別碼參數。

您可以設定 VaryByParam 屬性為下列值：

> \* = 表單或查詢字串參數而有所不同時，請建立不同的快取的版本。
> 
> none = 永不建立不同的快取的版本
> 
> 以分號的參數清單 = 建立不同的快取的版本，只要在清單中的表單或查詢字串參數的任何變化


## <a name="creating-a-cache-profile"></a>建立快取設定檔

藉由修改 [OutputCache] 屬性的屬性來設定輸出快取屬性的替代方案，您可以建立 web 組態檔 (web.config) 中快取設定檔。 在 web 組態檔中建立快取設定檔提供了幾個重要的優點。

首先，藉由設定輸出快取 web 組態檔中，您可以控制如何控制器動作快取在單一集中位置的內容。 您可以建立一個快取設定檔，並將設定檔套用至數個控制站或控制器的動作。

第二，您可以修改 web 組態檔不需要重新編譯您的應用程式。 如果您需要停用快取的應用程式已經部署到生產環境，您可以只修改 web 組態檔中定義的快取設定檔。 Web 組態檔的任何變更將會自動偵測並套用。

例如，&lt;快取&gt;列出 6 中的 web 組態區段會定義名為 Cache1Hour 快取設定檔。 &lt;快取&gt;區段必須出現在&lt;system.web&gt; web 組態檔區段。

**列出 6 – web.config 的快取 > 章節**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

列出 7 中的控制站會說明如何將 Cache1Hour 設定檔套用至控制器動作以 [OutputCache] 屬性。

**Listing 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

如果您叫用列出 7 中的控制站所公開的 index 動作將會 （1 小時） 傳回相同的時間。

## <a name="summary"></a>總結

輸出快取可以提供大幅改善效能的 ASP.NET MVC 應用程式的簡單方法。 在本教學課程中，您將學會如何使用非同步控制器動作的輸出快取 [OutputCache] 屬性。 您也學會如何修改 [OutputCache] 屬性，例如持續時間和 VaryByParam 屬性來修改內容取得快取的內容。 最後，您學會如何在 web 組態檔中定義快取設定檔。

> [!div class="step-by-step"]
> [上一頁](understanding-action-filters-cs.md)
> [下一頁](adding-dynamic-content-to-a-cached-page-cs.md)
