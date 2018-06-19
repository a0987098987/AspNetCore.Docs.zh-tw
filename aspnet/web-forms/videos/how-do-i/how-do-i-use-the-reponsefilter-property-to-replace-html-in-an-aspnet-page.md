---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[如何:]使用 Reponse.Filter 屬性來取代 ASP.NET 網頁中的 HTML |Microsoft 文件'
author: rick-anderson
description: 在這個視訊 Chris Pels 示範如何使用 Reponse.Filter 屬性來攔截並變更傳送到網頁的 HTML。 首先，範例頁面會建立 w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525527"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[如何:]使用 Reponse.Filter 屬性來取代 ASP.NET 網頁中的 HTML
====================
由[Chris Pels](https://twitter.com/chrispels)

在這個視訊 Chris Pels 示範如何使用 Reponse.Filter 屬性來攔截並變更傳送到網頁的 HTML。 首先，範例頁面會建立一些簡單的文字。 然後，會建立自訂的資料流類別，做為取代資料流正在傳送給使用者的瀏覽器的目前資料流。 自訂資料流該類中頁面的內容會從改變，而回應資料流寫出資料流擷取的。 這個自訂的資料流類別中寫入方法已自訂為取代基底的回應資料流，藉此改變傳送到使用者的瀏覽器的 HTML。 新的資料流類別指派給 Response.Filter 內容頁面中的最後，\_載入事件，因此，提供改變頁面內容的機制。

[&#9654;觀看影片 （13 分鐘）](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
