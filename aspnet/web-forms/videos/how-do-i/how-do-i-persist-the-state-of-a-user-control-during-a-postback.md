---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: "[如何]: 將使用者控制項的狀態保存在回傳期間 |Microsoft 文件"
author: rick-anderson
description: "在這個視訊 Chris Pels 示範如何保存使用者控制項中的一個或多個物件的狀態。 首先，會建立代表 abilit 使用者控制項..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="87e2a-104">[如何]: 將使用者控制項的狀態保存在回傳期間</span><span class="sxs-lookup"><span data-stu-id="87e2a-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="87e2a-105">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="87e2a-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="87e2a-106">在這個視訊 Chris Pels 示範如何保存使用者控制項中的一個或多個物件的狀態。</span><span class="sxs-lookup"><span data-stu-id="87e2a-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="87e2a-107">首先，會建立代表使用者可以指定篩選準則，將搜尋能力，使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="87e2a-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="87e2a-108">此外，附屬篩選類別是用來儲存的篩選資訊。</span><span class="sxs-lookup"><span data-stu-id="87e2a-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="87e2a-109">數個使用者介面項目會加入至篩選控制項，以及一些方法和屬性篩選條件的類別執行個體中儲存目前的篩選資訊。</span><span class="sxs-lookup"><span data-stu-id="87e2a-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="87e2a-110">接下來，為使用者控制項的持續性實作 RegisterRequiresControlState 方法和相關聯的儲存/還原方法。</span><span class="sxs-lookup"><span data-stu-id="87e2a-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="87e2a-111">這些方法會儲存頁面回傳期間篩選類別和其資料的執行個體。</span><span class="sxs-lookup"><span data-stu-id="87e2a-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="87e2a-112">最後，會討論如何將多個物件儲存在控制項狀態的實作。</span><span class="sxs-lookup"><span data-stu-id="87e2a-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="87e2a-113">&#9654;觀看影片 （23 分鐘）</span><span class="sxs-lookup"><span data-stu-id="87e2a-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
