---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 執行不同版本的 ASP.NET Web Pages (Razor) 並排顯示 |Microsoft Docs
author: tfitzmac
description: 這篇文章說明如何在相同電腦或伺服器上執行 ASP.NET Web Pages (Razor) 網站，當網站已設定為使用不同版本...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831140"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>並存執行不同版本的 ASP.NET Web Pages (Razor)
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何執行相同的電腦或伺服器上的 ASP.NET Web Pages (Razor) 網站，當網站已設定為使用不同版本的 ASP.NET Web Pages。
> 
> 您將學到什麼：
> 
> - 功能的預設行為是在 ASP.NET 中當您有站台，建置以 ASP.NET Web Pages。
> - 如何設定新站台執行較舊版本的 ASP.NET Web Pages。
>   
> 
> 這是發行項中導入的 ASP.NET 功能：
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


ASP.NET Web Pages 支援執行網站並排顯示的能力。 這可讓您繼續執行舊版的 ASP.NET Web Pages 應用程式，建置新的 ASP.NET Web Pages 應用程式，並執行所有人都在相同電腦上。

以下是要記住，當您使用 WebMatrix 安裝網頁時的一些事項：

- 根據預設，現有的 Web Pages 應用程式會在電腦上執行的最新版本。 （組件安裝在全域組件快取 (GAC) 中的而且系統會自動使用）。
- 如果您想要執行使用不同版本的 ASP.NET Web Pages 站台，您可以設定站台，若要這麼做。 如果您的網站尚未*web.config*檔案根目錄中的站台，建立一個新，並將下列 XML 複製到其中，覆寫現有的內容。 如果網站已包含*web.config*檔案中，新增`<appSettings>`至如下所示的項目`<configuration>`一節。

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-如果您未指定的版本*web.config*檔案，網站會部署為最新版本。 (組件複製到*bin*資料夾中已部署的站台。)
- 您在 Webmatrix 中使用的網站範本所建立的新應用程式納入站台的網頁版的組件*bin*資料夾。

一般情況下，您一律可以控制的網頁，以搭配您的網站，使用 NuGet 將適當的組件安裝到站台的版本*bin*資料夾。 若要尋找套件，請造訪[NuGet.org](http://NuGet.org)。

## <a name="additional-resources"></a>其他資源

[ASP.NET Web pages 2 最上層的功能](top-features-in-web-pages-2.md)
