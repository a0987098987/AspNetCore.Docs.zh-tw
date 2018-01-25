---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "快取 ASP.NET Web 中的資料頁面 (Razor) 站台，以提升效能 |Microsoft 文件"
author: tfitzmac
description: "您可以加速您的網站將它儲存-也就是快取的資料，但通常需要相當長的時間來擷取，或處理結果..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>為了達到最佳效能的 ASP.NET Web Pages (Razor) 網站中的快取資料
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用更快的效能，在 ASP.NET Web Pages (Razor) 的網站中的協助程式快取資訊。 您可以透過將它存放區 &#8212; 加快您的網站也就是快取 &#8212;資料通常會花費相當長的時間來擷取，或處理和，不常變更的結果。
> 
> **您將學習：** 
> 
> - 如何使用快取來改善您的網站的回應能力。
> 
> 這些是發行項中所引進的 ASP.NET 功能：
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


每次有人從您的網站要求網頁，網頁伺服器必須執行一些工作，才能完成要求。 某些網頁伺服器可能必須執行需要 （相對） 長的時間，例如從資料庫擷取資料的工作。 即使這些工作才長時間在絕對詞彙中，如果您的網站體驗有大量的流量，整個系列的個別要求導致網頁伺服器來執行複雜或緩慢的工作很多的工作可以加總。 最後，這可能會影響網站效能。

快取資料是網站的一種方法改善您在像這樣的情況下的效能。 如果您的網站取得重複的要求相同的資訊，且不需要修改的每個人，資訊不是時間區分大小寫，而不是重新提取或重新計算，您可以一次擷取資料並儲存該結果。 下一次要求進入該資訊，您只出現它在快取。

一般情況下，您的快取不會經常變更的資訊。 當您將資訊放在快取中時，它會儲存在 web 伺服器上的記憶體。 您可以指定多久它應該快取，從數秒到天。 在快取期間過期時，從快取時，會自動移除資訊。

> [!NOTE]
> 快取中的項目可能會移除原因不是已過期。 例如，web 伺服器可能會暫時記憶體不足，而它可以回收記憶體的其中一個方法是藉由擲回在快取的項目。 您會看到，即使您已將資訊放入快取，您必須檢查以確定它仍在需要時。


假設您的網站有一個顯示目前的溫度和氣象預報預測的頁面。 若要取得這種類型的資訊，您可能會將要求傳送到外部服務。 因為這項資訊不會變更 （在兩小時期間內，例如） 的許多，因為外部呼叫需要時間和頻寬，它是絕佳候選快取。

## <a name="adding-caching-to-a-page"></a>加入快取的頁面

ASP.NET 包含`WebCache`helper，可讓您輕鬆地將快取加入至您的網站，並將資料加入至快取。 在此程序，您將建立快取的目前時間的頁面。 這不是真實世界範例中，因為目前的時間則是項目，並經常變更，而且這並不複雜計算。 不過，它是為了說明動作中的快取的好方法。

1. 加入新的頁面名稱為*WebCache.cshtml*網站。
2. 將下列程式碼和標記加入頁面：

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    當您快取資料時，請在放入快取使用的名稱這是唯一的網站。 在此情況下，您將使用名為快取項目`CachedTime`。 這是`cacheItemKey`程式碼範例所示。

    程式碼會先讀取`CachedTime`快取項目。 如果傳回值 （亦即，如果快取項目不是 null），程式碼只會將時間變數的值來快取資料。

    不過，如果快取項目不存在 （也就是說，它是 null），程式碼設定時間值、 將它加入至快取中，並設定到期值為一分鐘。 在一分鐘之後, 會捨棄快取項目。 （項目在快取中的預設到期值是 20 分鐘）。此命令`WebCache.Set(cacheItemKey, time, 1, false)`示範如何將目前的時間值加入至快取以及其期限設為 1 分鐘。 設定*slidingExpiration*參數`false`表示的到期時間未更新每次要求時。 到期完全原先加入至快取後 1 分鐘。 如果您將此值設定為`true`從快取要求的值每次重設的 1 分鐘到期時間。

    此程式碼說明時，您應該一律使用快取資料的模式。 您會得到在快取之前，一律先查看是否`WebCache.Get`方法已傳回 null。 請記住快取項目可能已過期或可能已移除基於其他原因，所以永遠不會保證任何指定的項目，快取中。
3. 執行*WebCache.cshtml*瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)第一次要求頁面的時間資料沒有快取中，而且的程式碼新增至快取的時間值。

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. 重新整理*WebCache.cshtml*瀏覽器中。 此時，[時間] 資料是快取中。 請注意，時間尚未變更檢視頁面最後一次。

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. 等待一分鐘先清空快取，然後重新整理頁面。 頁面會再次指出快取中，找不到時間資料，並新增到快取更新的時間。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [以圖表顯示資料](https://go.microsoft.com/fwlink/?LinkId=202895)
- [WebCache API 參考](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)
