---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: 在此影片的 Chris Pels 示範如何保存使用者控制項中的一個或多個物件的狀態。 首先，會建立代表 abilit 使用者控制項...
ms.author: aspnetcontent
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 7862d83d3df3ca5407b7d8fd465cf42da8e7228a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805174"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[How Do I]: 在回傳期間保存使用者控制項的狀態
====================
<span data-ttu-id="be5c6-104">藉由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="be5c6-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="be5c6-105">在此影片的 Chris Pels 示範如何保存使用者控制項中的一個或多個物件的狀態。</span><span class="sxs-lookup"><span data-stu-id="be5c6-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="be5c6-106">首先，使用者控制項被建立表示要指定搜尋的篩選準則的使用者的能力。</span><span class="sxs-lookup"><span data-stu-id="be5c6-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="be5c6-107">此外，附帶篩選類別會建立儲存的篩選資訊。</span><span class="sxs-lookup"><span data-stu-id="be5c6-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="be5c6-108">數個使用者介面項目會加入至篩選控制項，以及一些方法和屬性來篩選類別執行個體中儲存目前的篩選資訊。</span><span class="sxs-lookup"><span data-stu-id="be5c6-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="be5c6-109">接下來，使用者控制項持續性實作使用 RegisterRequiresControlState 方法和相關聯的儲存/還原方法。</span><span class="sxs-lookup"><span data-stu-id="be5c6-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="be5c6-110">這些方法會儲存頁面回傳期間篩選類別和其資料執行個體。</span><span class="sxs-lookup"><span data-stu-id="be5c6-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="be5c6-111">最後，還有如何儲存狀態的控制項實作中的多個物件的討論。</span><span class="sxs-lookup"><span data-stu-id="be5c6-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="be5c6-112">&#9654;觀看影片 （23 分鐘）</span><span class="sxs-lookup"><span data-stu-id="be5c6-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
