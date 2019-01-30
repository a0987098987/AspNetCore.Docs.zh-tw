---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 教學課程：自訂檢視的 EF Database First 與 ASP.NET MVC 應用程式
description: 本教學課程著重於變更自動產生的檢視，以增強其呈現方式。
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236493"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="8bc25-103">教學課程：自訂檢視的 EF Database First 與 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="8bc25-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="8bc25-104">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8bc25-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8bc25-105">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="8bc25-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8bc25-106">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="8bc25-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="8bc25-107">本教學課程著重於變更自動產生的檢視，以增強其呈現方式。</span><span class="sxs-lookup"><span data-stu-id="8bc25-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="8bc25-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="8bc25-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bc25-109">將課程新增至學生詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="8bc25-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="8bc25-110">確認課程會新增至頁面</span><span class="sxs-lookup"><span data-stu-id="8bc25-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8bc25-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="8bc25-111">Prerequisites</span></span>

* [<span data-ttu-id="8bc25-112">變更資料庫</span><span class="sxs-lookup"><span data-stu-id="8bc25-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="8bc25-113">將課程新增至學生詳細資料</span><span class="sxs-lookup"><span data-stu-id="8bc25-113">Add courses to student detail</span></span>

<span data-ttu-id="8bc25-114">產生的程式碼應用程式提供很好的起點，但它不一定是提供的所有功能，您需要在您的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="8bc25-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="8bc25-115">您可以自訂程式碼，以符合您的應用程式的特定需求。</span><span class="sxs-lookup"><span data-stu-id="8bc25-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="8bc25-116">目前，您的應用程式不會顯示所選取學生的已註冊的課程。</span><span class="sxs-lookup"><span data-stu-id="8bc25-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="8bc25-117">在本節中，您將新增的已註冊的課程針對至每個學生**詳細資料**學生的檢視。</span><span class="sxs-lookup"><span data-stu-id="8bc25-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="8bc25-118">開啟**檢視** > **學生** > *Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8bc25-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="8bc25-119">最後一個以下&lt;/dl&gt;標記，但在關閉前&lt;/div&gt;標記中加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="8bc25-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="8bc25-120">此程式碼會建立 Enrollment 資料表中的每一筆記錄的資料列顯示所選取學生的資料表。</span><span class="sxs-lookup"><span data-stu-id="8bc25-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="8bc25-121">**顯示**方法將 HTML 呈現物件 (modelItem) 表示的運算式。</span><span class="sxs-lookup"><span data-stu-id="8bc25-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="8bc25-122">使用 Display 方法 （而非直接內嵌在程式碼中的屬性值） 以確定此值的格式正確根據其型別和該類型的範本。</span><span class="sxs-lookup"><span data-stu-id="8bc25-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="8bc25-123">在此範例中，每個運算式會從目前的記錄，在迴圈中，來傳回單一屬性且值為基本型別，其呈現為文字。</span><span class="sxs-lookup"><span data-stu-id="8bc25-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="8bc25-124">確認課程也會新增</span><span class="sxs-lookup"><span data-stu-id="8bc25-124">Confirm courses are added</span></span>

<span data-ttu-id="8bc25-125">執行方案。</span><span class="sxs-lookup"><span data-stu-id="8bc25-125">Run the solution.</span></span> <span data-ttu-id="8bc25-126">按一下 **學生的清單**，然後選取**詳細資料**學生的其中一個。</span><span class="sxs-lookup"><span data-stu-id="8bc25-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="8bc25-127">您會看到已註冊的課程已包含在檢視中。</span><span class="sxs-lookup"><span data-stu-id="8bc25-127">You will see the enrolled courses have been included in the view.</span></span>

![註冊的學生](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="8bc25-129">後續步驟</span><span class="sxs-lookup"><span data-stu-id="8bc25-129">Next steps</span></span>
<span data-ttu-id="8bc25-130">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="8bc25-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8bc25-131">已加入的課程，學生詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="8bc25-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="8bc25-132">確認課程會新增至頁面</span><span class="sxs-lookup"><span data-stu-id="8bc25-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="8bc25-133">請前進到下一個教學課程，以了解如何新增資料註解來指定驗證需求，並顯示格式。</span><span class="sxs-lookup"><span data-stu-id="8bc25-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8bc25-134">增強資料驗證</span><span class="sxs-lookup"><span data-stu-id="8bc25-134">Enhance data validation</span></span>](enhancing-data-validation.md)
