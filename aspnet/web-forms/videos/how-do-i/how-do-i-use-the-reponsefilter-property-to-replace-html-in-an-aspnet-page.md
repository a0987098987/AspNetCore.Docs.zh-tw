---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[How Do i:]使用 Reponse.Filter 屬性取代 ASP.NET 網頁中的 HTML |Microsoft Docs'
author: rick-anderson
description: 在此影片的 Chris Pels 示範如何使用 Reponse.Filter 屬性來攔截及變更傳送至網頁的 HTML。 首先，範例頁面建立 w...
ms.author: aspnetcontent
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: bf1936e5710b67185977f71bea03240e6c6abf41
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808099"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[How Do i:]使用 Reponse.Filter 屬性取代 ASP.NET 網頁中的 HTML
====================
藉由[Chris Pels](https://twitter.com/chrispels)

在此影片的 Chris Pels 示範如何使用 Reponse.Filter 屬性來攔截及變更傳送至網頁的 HTML。 首先，範例頁面會建立一些簡單的文字。 然後，會建立自訂的 Stream 類別，做為傳送至使用者的瀏覽器的目前資料流的替代資料流。 在自訂資料流類別，該頁面的內容會擷取從資料流，改變，而再寫出至回應資料流。 在此自訂的 Stream 類別寫入方法被自訂取代基底的回應資料流，藉此改變 什麼傳送到使用者的瀏覽器的 HTML。 新的資料流類別指派給 Response.Filter 內容頁面中的最後，\_載入事件，進而提供變更網頁內容的機制。

[&#9654;觀看影片 （13 分鐘）](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
