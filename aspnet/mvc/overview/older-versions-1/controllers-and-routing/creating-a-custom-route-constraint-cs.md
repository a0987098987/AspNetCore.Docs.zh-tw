---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: "建立自訂路由條件約束 (C#) |Microsoft 文件"
author: StephenWalther
description: "作者： Stephen Walther 示範如何建立自訂的路由條件約束。 我們會實作簡單的自訂條件約束可以防止路由相符 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: c31ba3382b9dbe22a6826b9f858944c223efdd9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-c"></a>建立自訂路由條件約束 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 作者： Stephen Walther 示範如何建立自訂的路由條件約束。 我們會實作簡單的自訂條件約束，避免從遠端電腦進行瀏覽器要求的比對路由。


本教學課程的目標是為了示範如何建立自訂的路由條件約束。 自訂路由條件約束，可讓您防止路由所比對，除非某些自訂條件為 matched。

在本教學課程中，我們會建立 Localhost 的路由條件約束。 Localhost 路由條件約束只符合從本機電腦所提出的要求。 從遠端在網際網路上的要求不符。

您藉由實作 IRouteConstraint 介面實作的自訂路由條件約束。 這是非常簡單的介面描述單一方法：

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

方法會傳回布林值。 如果傳回 false，則路由相關聯的條件約束都不會符合瀏覽器要求。

Localhost 條件約束被包含在程式碼範例 1。

**列出 1-LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

程式碼範例 1 中的條件約束會利用 HttpRequest 類別所公開的 IsLocal 屬性。 當要求的 IP 位址是任一個 127.0.0.1 或要求的 IP 時伺服器的 IP 位址相同，則這個屬性會傳回 true。

您使用自訂的條件約束內 Global.asax 檔中定義的路由。 Global.asax 檔案中列出的 2 會使用 Localhost 條件約束，以防止任何人要求管理頁面，除非它們從本機伺服器提出要求。 例如，從遠端伺服器時，將會失敗 /Admin/DeleteAll 的要求。

**列出 2-Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

管理路由的定義所用的 Localhost 條件約束。 遠端瀏覽器要求，將不會符合此路由。 不過，請注意，在 Global.asax 中定義其他路由可能會符合相同的要求。 請務必了解條件約束可防止特定路由比對要求而不是所有的路由 Global.asax 檔案中定義。

請注意，預設路由已從 Global.asax 檔案中列出 2 略過。 如果您包含預設路由，則預設路由會符合管理控制器的要求。 在此情況下，遠端使用者可能仍叫用的管理控制器的動作即使其要求不符合系統管理員路由。

>[!div class="step-by-step"]
[上一頁](creating-a-route-constraint-cs.md)
[下一頁](asp-net-mvc-controller-overview-vb.md)
