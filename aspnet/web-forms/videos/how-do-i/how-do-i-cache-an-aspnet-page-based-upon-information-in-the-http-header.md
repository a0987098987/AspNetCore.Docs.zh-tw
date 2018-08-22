---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[How Do i:] 快取 ASP.NET 網頁會根據 HTTP 標頭中的資訊 |Microsoft Docs'
author: rick-anderson
description: 在此影片的 Chris Pels 示範如何根據頁面的 HTTP 標頭中的資訊在 ASP.NET 輸出快取中保留的頁面。 可能的 HTTP 頁首的第一個...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: c90a3db1357df062909ad0e3b73fdeeb3dc16329
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834233"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[How Do i:] 快取 ASP.NET 網頁會根據 HTTP 標頭中的資訊
====================
藉由[Chris Pels](https://twitter.com/chrispels)

在此影片的 Chris Pels 示範如何根據頁面的 HTTP 標頭中的資訊在 ASP.NET 輸出快取中保留的頁面。 首先，檢閱可能的 HTTP 標頭值。 然後，建立範例頁面，然後 OutputCache 指示詞搭配 VaryByHeader 屬性包含值 「 接受語言 」，HTTP 標頭，來控制快取根據使用者的瀏覽器的語言。 在 IE 中是設定為英文，然後它會設定為使用法文的 FireFox 中檢視範例網頁。 最後，將會討論將快取的定義移至 CacheProfile web.config 檔案中的選項。

[&#9654;觀看影片 （12 分鐘）](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
