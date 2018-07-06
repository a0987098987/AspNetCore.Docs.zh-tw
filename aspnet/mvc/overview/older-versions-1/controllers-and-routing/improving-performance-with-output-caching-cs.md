---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: 改善效能，使用輸出快取 (C#) |Microsoft Docs
author: microsoft
description: 在本教學課程中，您將了解如何大幅改善您的 ASP.NET MVC web 應用程式的效能利用輸出快取。 您...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 81349f37d861e79ff4d95962c275a96576d0455b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372481"
---
<a name="improving-performance-with-output-caching-c"></a>改善效能，使用輸出快取 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 在本教學課程中，您將了解如何大幅改善您的 ASP.NET MVC web 應用程式的效能利用輸出快取。 您了解如何快取，因此不需要相同的內容，這是要建立新的使用者叫用動作的每個階段，從控制器動作傳回的結果。


本教學課程的目標是要說明如何大幅改善效能的 ASP.NET MVC 應用程式利用輸出快取。 輸出快取可讓您快取控制器動作傳回的內容。 如此一來，相同的內容不會不需要在每次叫用相同的控制器動作產生。

例如，試想一下，ASP.NET MVC 應用程式，會在名為索引檢視中顯示一份資料庫記錄。 一般來說，每一次使用者叫用傳回 [索引] 檢視中，控制器動作的一組資料庫記錄必須擷取從資料庫執行資料庫查詢。

如果相反地，您可以利用的輸出快取時您可以避免執行資料庫查詢，每次任何使用者叫用相同的控制器動作。 檢視可以擷取從快取，而不是從控制器動作重新產生。 您不必執行備援快取啟用在伺服器上運作。

## <a name="enabling-output-caching"></a>啟用輸出快取

您可以啟用輸出快取個別的控制器動作或整個控制器類別中加入 [OutputCache] 屬性。 例如，列表 1 中的控制站會公開名為 index （） 的動作。 Index （） 動作的輸出快取 10 秒的時間。

**列表 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

在 Beta 版的 ASP.NET MVC 中，輸出快取不適用於之類的 URL [ http://www.MySite.com/ ](http://www.mysite.com/)。 相反地，您必須輸入 URL，例如[ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index)。 

在列表 1 中，index （） 動作的輸出快取 10 秒的時間。 如果您想，您可以指定更長的快取持續時間。 比方說，如果您想要快取一天的控制器動作的輸出，然後您可以指定快取持續時間為 86400 秒 (60 秒\*60 分鐘\*24 小時)。

沒有內容不保證會進行快取，您指定的時間長度。 低記憶體資源時，快取會自動啟動收回的內容。

列表 1 中的主控制器會傳回列表 2 中的 [索引] 檢視。 沒有特別關於此檢視。 索引 檢視只會顯示目前的時間 （請參閱 圖 1）。

**列表 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**圖 1 – 快取 索引 檢視**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

如果您叫用 index （） 動作數次在您的瀏覽器的網址列中輸入 URL /Home/索引，並重複按瀏覽器中的 [重新整理/重新載入] 按鈕，然後 [索引] 檢視所顯示的時間不會變更 10 秒的時間。 因為檢視快取，則會顯示相同的時間。

請務必了解每一位使用者造訪您的應用程式相同的檢視，會快取。 Index （） 動作會叫用的人會收到相同的快取的版本的 [索引] 檢視。 這表示已大幅減少網頁伺服器必須執行來處理索引檢視的工作數量。

列表 2 中的檢視會做的動作很簡單的項目。 檢視只會顯示目前的時間。 不過，您可以如同輕鬆地快取會顯示一組資料庫記錄。 在此情況下的一組資料庫記錄就不需要從資料庫擷取每一次叫用傳回的檢視控制器動作。 快取可以減少您 web 伺服器和資料庫伺服器必須執行的工作數量。

不使用 頁面&lt;%@ OutputCache %&gt; MVC 檢視中的指示詞。 這個指示詞透過高度，從 Web Form 世界，不應用於 ASP.NET MVC 應用程式。

## <a name="where-content-is-cached"></a>會在快取內容

根據預設，當您使用 [OutputCache] 屬性中，內容會快取中三個位置： 網頁伺服器、 任何 proxy 伺服器和網頁瀏覽器。 您可以控制完全內容快取所在藉由修改 [OutputCache] 屬性的 Location 屬性。

您可以設定 [位置] 屬性以下列值之一：

> ·任何
> 
> ·用戶端
> 
> ·下游
> 
> ·伺服器
> 
> ·None
> 
> ·ServerAndClient


根據預設，[位置] 屬性有值 Any。 不過，有情況下您可能想要只在瀏覽器上，或只能在伺服器上的快取。 比方說，如果您要快取的每個使用者個人化資訊然後您應該快取伺服器上的資訊。 如果您要顯示不同的資訊給不同的使用者接著您應該快取的資訊只在用戶端。

例如，列表 3 中的控制站會公開名為 GetName() 傳回目前的使用者名稱的動作。 如果 Jack 登入網站，且叫用 GetName() 動作然後動作傳回的字串"Hi Jack"。 如果接下來，Jill 登入網站，並叫用 GetName() 動作然後她也會取得字串"Hi Jack"。 Jack 一開始會叫用的控制器動作之後，所有使用者的 web 伺服器上會快取字串。

**列表 3 – Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

最有可能列表 3 中的控制站無法您想要的方式。 您不想要顯示訊息"Hi Jack"給 Jill。

您應該永遠不會快取伺服器快取中的個人化的內容。 不過，您可能要快取中的瀏覽器快取以改善效能的個人化的內容。 如果您快取內容，在瀏覽器中，且使用者叫用相同的控制器動作多次，內容可以擷取從瀏覽器快取，而非伺服器。

在 列表 4 中的已修改的控制器會快取 GetName() 動作的輸出。 不過，只有瀏覽器和伺服器上不會快取內容。 這樣一來，當多位使用者叫用 GetName() 方法時，每位人員取得他們自己的使用者名稱和不可為另一個人的使用者名稱。

**列表 4 – Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

請注意 [OutputCache] 屬性，在 [列表 4 中的包含 [位置] 屬性設定為值 OutputCacheLocation.Client。 [OutputCache] 屬性也包含 NoStore 屬性。 NoStore 屬性用來通知 proxy 伺服器和瀏覽器，它們不應該儲存在快取內容的永久複本。

## <a name="varying-the-output-cache"></a>不同的輸出快取

在某些情況下，您可以使用不同的快取的版本的完全相同的內容。 例如，假設您要建立主要/詳細資料頁面。 主版頁面會顯示一份電影標題。 當您按一下標題時，您會取得所選的電影詳細資料。

如果您要快取的詳細資料頁面，則相同電影的詳細資料會顯示在您按一下不論哪一個電影。 未來的所有使用者，將會顯示第一個使用者所選取的第一部電影。

您可以利用 [OutputCache] 屬性的 VaryByParam 屬性來修正這個問題。 此屬性可讓您建立的相同內容時的表單參數的不同快取的版本，或查詢字串參數而有所不同。

例如，列表 5 中的控制站會公開名為 Master() 和 Details() 的兩個動作。 Master() 動作會傳回一份電影標題及 Details() 動作傳回選取的影片的詳細資料。

**列表 5 – Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Master() 動作包括 VaryByParam 屬性具有值"none"。 當叫用動作，Master() 相同的快取版本的主要檢視會傳回。 任何形式的參數或查詢字串參數被忽略 （請參閱 圖 2）。

**圖 2 – /Movies/Master 檢視**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**圖 3 – / Movies/詳細資料檢視**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Details() 動作包含值為"Id"VaryByParam 屬性。 當不同的 Id 參數的值會傳遞至控制器動作時，會產生不同的快取的版本的詳細資料檢視。

它是一定要了解使用 VaryByParam 屬性會導致多個快取並不是較少。 不同的快取的版本的詳細資料檢視會建立每個不同版本的 Id 參數。

您可以設定 VaryByParam 屬性為下列值：

> \* = 每當表單或查詢字串參數而異，請建立不同的快取的版本。
> 
> none = 永不建立不同的快取的版本
> 
> 以分號的參數清單 = 建立不同的快取的版本，每當任何表單或查詢字串中的參數清單不同


## <a name="creating-a-cache-profile"></a>建立快取設定檔

為藉由修改 [OutputCache] 屬性的屬性中設定輸出快取屬性的替代方案，您可以建立快取設定檔 web 組態 (web.config) 檔中。 在 web 組態檔中建立快取設定檔提供了幾項重要的優點。

首先，藉由設定輸出快取 web 組態檔中，您可以控制如何控制器動作快取內容，在單一中央位置。 您可以建立一個快取設定檔，並將設定檔套用至數個控制器或控制器動作。

第二，您可以修改 web 組態檔不需要重新編譯您的應用程式。 如果您需要停用快取已部署到生產環境的應用程式，您可以只修改 web 組態檔中定義的快取設定檔。 Web 組態檔的任何變更會自動偵測並套用。

例如，&lt;快取&gt;列表 6 中的 web 組態區段會定義名為 Cache1Hour 快取設定檔。 &lt;快取&gt;一節必須出現在&lt;system.web&gt; web 組態檔區段。

**列表 6 – web.config 的快取 > 章節**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

列表 7 中的控制站會說明如何將 Cache1Hour 設定檔套用至控制器動作以 [OutputCache] 屬性。

**列表 7 – Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

如果您叫用由 列表 7 中的控制器的 index （） 動作則會傳回相同的時間 （1 小時）。

## <a name="summary"></a>總結

輸出快取可以提供非常簡單的方法，大幅改善您的 ASP.NET MVC 應用程式的效能。 在本教學課程中，您已了解如何使用 [OutputCache] 屬性快取的控制器動作的輸出。 您也學到如何修改 [OutputCache] 屬性，例如修改內容取得快取持續時間和 VaryByParam 屬性的屬性。 最後，您已了解如何在 web 組態檔中定義快取設定檔。

> [!div class="step-by-step"]
> [上一頁](understanding-action-filters-cs.md)
> [下一頁](adding-dynamic-content-to-a-cached-page-cs.md)
