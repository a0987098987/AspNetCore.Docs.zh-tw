---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: 快取資料在 ASP.NET Web Pages (Razor) 網站以提升效能 |Microsoft Docs
author: tfitzmac
description: 您可以加速您的網站透過儲存-也就是讓快取-通常會花相當長的時間，以擷取或處理資料的結果...
ms.author: aspnetcontent
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 28be9194bbd95e896311700ddcf89379a82ee636
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805193"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>快取 ASP.NET Web Pages (Razor) 網站中的資料，以提升效能
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何使用更快的效能，在 ASP.NET Web Pages (Razor) 網站中的協助程式快取資訊。 您可以透過讓儲存加快您的網站&#8212;也就是快取&#8212;的結果通常會花費相當長的時間，以擷取或處理和可不常變更的資料。
> 
> **您將學到什麼：** 
> 
> - 如何使用快取來改善您的網站的回應能力。
> 
> 以下是所引進的發行項 ASP.NET 功能：
> 
> - `WebCache`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


每次有人從您的網站要求網頁，網頁伺服器必須執行一些工作，以履行要求。 針對某些頁面，伺服器可能必須執行需要 （相較之下） 長的時間，例如從資料庫擷取資料的工作。 即使這些工作不需要時間以絕對的條款中,，如果您的網站遇到大量流量，會導致 web 伺服器來執行複雜或速度很慢的工作的個別要求的整個系列最多可新增很多工作。 最後，這可能會影響網站的效能。

快取資料是網站的一種方法改善效能的情況下，像這樣在您。 如果您的網站取得重複的要求相同的資訊，資訊就不需要修改每位人員，且不是時間區分大小寫，而不是重新擷取或重新計算，您可以一次提取資料並儲存結果。 下次要求該傳入的詳細資訊，您只取得從快取。

一般情況下，您快取不會經常變更的資訊。 當您將資訊放在快取時，它會儲存在 web 伺服器上的記憶體。 您可以指定多久它應該快取，從 天前的秒數。 當快取期間到期時，從快取時，會自動移除資訊。

> [!NOTE]
> 快取中的項目可能已過期而不移除原因。 例如，網頁伺服器可能會暫時記憶體不足，而可以回收記憶體的其中一個方法是藉由擲回從快取的項目。 如您所見，即使您已經將資訊放入快取，您必須檢查以確定它仍然會有在需要時。


假設您的網站具有會顯示目前的溫度和氣象預報的頁面。 若要取得這種類型的資訊，您可能會將要求傳送至外部服務。 因為這項資訊不會變更大部分 （在兩小時期間內，例如），因為外部呼叫都需要時間和頻寬，它是適合快取。

## <a name="adding-caching-to-a-page"></a>加入至頁面快取

ASP.NET 包含`WebCache`協助程式，可讓您輕鬆地新增至您網站的 快取，並將資料新增至快取。 在此程序中，您將建立快取的目前時間的頁面。 這不是真實世界範例中，因為目前的時間，並經常變更且此外沒有複雜計算的項目。 不過，很適合用來說明快取的運作。

1. 新增名為頁面*WebCache.cshtml*網站。
2. 您可以將下列程式碼和標記加入頁面：

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    當您要快取資料時，您將它放入快取使用的名稱這是唯一的網站。 在此情況下，您將使用名為快取項目`CachedTime`。 這是`cacheItemKey`程式碼範例所示。

    程式碼會先讀取`CachedTime`快取項目。 （亦即，如果快取項目不是 null），則會傳回值，如果程式碼只快取資料設定時間變數的值。

    不過，如果快取項目不存在 （也就是它是 null），程式碼，可設定的時間值、 將它新增至快取中，和在逾期值設定為一分鐘。 一分鐘後，會捨棄快取項目。 （項目在快取的預設到期值是 20 分鐘）。此命令`WebCache.Set(cacheItemKey, time, 1, false)`示範如何將目前的時間值新增至快取，並將其到期設定為 1 分鐘。 設定*slidingExpiration*參數來`false`表示到期時間未更新每次要求時。 到期完全原先加入至快取之後 1 分鐘。 如果您將此值設定為`true`從快取所要求的值每次重設的 1 分鐘到期時間。

    此程式碼說明快取資料時，應該一律會使用您的模式。 您的項目從快取之前，請一律先檢查是否`WebCache.Get`方法傳回 null。 請記住，快取項目可能已過期或可能已移除某些其他原因，因此永遠不會保證任何指定的項目，快取。
3. 執行*WebCache.cshtml*瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)第一次要求頁面上，[時間] 資料快取中，但程式碼會將時間值新增至快取。

    ![快取-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. 重新整理*WebCache.cshtml*瀏覽器中。 此時，[時間] 資料位於快取中。 請注意，時間沒有變更上次檢視頁面。

    ![快取-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. 等候一分鐘，讓快取會清空，然後重新整理頁面。 頁面會再次指出快取中，找不到時間資料，並快取中加入更新的時間。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [以圖表顯示資料](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API 參考](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)
