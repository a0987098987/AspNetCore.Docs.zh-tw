---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: EF Database First 與 ASP.NET MVC： 自訂檢視 |Microsoft Docs
author: Rick-Anderson
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: f66e097d53514ab3842e04cd545ca626c652478a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021206"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="56189-104">EF Database First 與 ASP.NET MVC： 自訂檢視</span><span class="sxs-lookup"><span data-stu-id="56189-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="56189-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="56189-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="56189-106">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="56189-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="56189-107">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="56189-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="56189-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="56189-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="56189-109">系列的這個部分會著重於變更自動產生的檢視，以增強其呈現方式。</span><span class="sxs-lookup"><span data-stu-id="56189-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="56189-110">將已註冊的課程新增至學生詳細資料</span><span class="sxs-lookup"><span data-stu-id="56189-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="56189-111">產生的程式碼應用程式提供很好的起點，但它不一定是提供的所有功能，您需要在您的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="56189-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="56189-112">您可以自訂程式碼，以符合您的應用程式的特定需求。</span><span class="sxs-lookup"><span data-stu-id="56189-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="56189-113">目前，您的應用程式不會顯示所選取學生的已註冊的課程。</span><span class="sxs-lookup"><span data-stu-id="56189-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="56189-114">在本節中，您將新增的已註冊的課程針對至每個學生**詳細資料**學生的檢視。</span><span class="sxs-lookup"><span data-stu-id="56189-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="56189-115">開啟**Students/Details.cshtml**，和最後一個以下&lt;/dl&gt;索引標籤上，但在關閉前&lt;/div&gt;標記中加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="56189-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="56189-116">此程式碼會建立 Enrollment 資料表中的每一筆記錄的資料列顯示所選取學生的資料表。</span><span class="sxs-lookup"><span data-stu-id="56189-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="56189-117">**顯示**方法將 HTML 呈現物件 (modelItem) 表示的運算式。</span><span class="sxs-lookup"><span data-stu-id="56189-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="56189-118">使用 Display 方法 （而非直接內嵌在程式碼中的屬性值） 以確定此值的格式正確根據其型別和該類型的範本。</span><span class="sxs-lookup"><span data-stu-id="56189-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="56189-119">在此範例中，每個運算式會從目前的記錄，在迴圈中，來傳回單一屬性且值為基本型別，其呈現為文字。</span><span class="sxs-lookup"><span data-stu-id="56189-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="56189-120">再次瀏覽至 / Students 索引 檢視，然後選取**詳細資料**學生的其中一個。</span><span class="sxs-lookup"><span data-stu-id="56189-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="56189-121">您會看到已註冊的課程已包含在檢視中。</span><span class="sxs-lookup"><span data-stu-id="56189-121">You will see the enrolled courses have been included in the view.</span></span>

![註冊的學生](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="56189-123">[上一頁](changing-the-database.md)
> [下一頁](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="56189-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
