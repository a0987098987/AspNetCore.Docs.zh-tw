---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 執行不同版本的 ASP.NET Web Pages (Razor) 並存 |Microsoft 文件
author: tfitzmac
description: 這篇文章說明如何在同一部電腦或伺服器上執行 ASP.NET Web Pages (Razor) 網站，網站設定為使用不同版本時...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898402"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>並存執行不同版本的 ASP.NET Web Pages (Razor)
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何在同一部電腦或伺服器上執行 ASP.NET Web Pages (Razor) 網站，網站設定為使用不同版本的 ASP.NET Web Pages 時。
> 
> 您將學習：
> 
> - 時，什麼的預設行為是在 ASP.NET 中您有建立以 ASP.NET Web Pages 站台。
> - 如何設定新的站台，以執行較舊版本的 ASP.NET Web Pages。
>   
> 
> 這是發行項中所引進的 ASP.NET 功能：
> 
> - `webPages:Version`組態設定。
>   
> 
> ## <a name="software-versions"></a>軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。


ASP.NET Web Pages 支援的功能執行並存的網站。 這可讓您可以繼續執行舊版 ASP.NET Web Pages 應用程式、 建立新的 ASP.NET Web Pages 應用程式，以及在相同電腦上執行所有程式。

以下是您使用 WebMatrix 安裝網頁時要注意一些事項：

- 根據預設，現有的 Web Pages 應用程式會在電腦上執行做為最新版本。 （組件安裝在全域組件快取 (GAC) 中的並且會自動使用）。
- 如果您想要執行不同版本的 ASP.NET Web Pages 站台，您可以設定站台執行此作業。 如果還沒有網站*web.config*檔根目錄中的站台、 建立一個新並將下列 XML 複製到其中，覆寫現有的內容。 如果網站已包含*web.config* file、 add`<appSettings>`元素類似下列的其中一個來`<configuration>`> 一節。

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-如果您未指定的版本中*web.config*檔案，網站會部署為最新版本。 (組件會複製到*bin*資料夾中已部署的站台。)
- 您使用 Web 矩陣中的網站範本所建立的新應用程式納入站台的網頁版組件*bin*資料夾。

一般情況下，您一律可以控制哪個版本的網頁使用與您的網站使用 NuGet 將適當的組件安裝到站台的*bin*資料夾。 若要尋找的封裝，請瀏覽[NuGet.org](http://NuGet.org)。

## <a name="additional-resources"></a>其他資源

[在 ASP.NET Web Pages 2 上的功能](top-features-in-web-pages-2.md)
