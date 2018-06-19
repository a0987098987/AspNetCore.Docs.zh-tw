---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: '反覆項目 #2 – 讓應用程式看起來不錯 (C#) |Microsoft 文件'
author: microsoft
description: 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: cad28fb6ff02625674e59674d1ec08d52373c269
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871840"
---
<a name="iteration-2--make-the-application-look-nice-c"></a><span data-ttu-id="8a74d-103">反覆項目 #2 – 讓應用程式看起來不錯 (C#)</span><span class="sxs-lookup"><span data-stu-id="8a74d-103">Iteration #2 – Make the application look nice (C#)</span></span>
====================
<span data-ttu-id="8a74d-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8a74d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8a74d-105">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="8a74d-105">Download Code</span></span>](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> <span data-ttu-id="8a74d-106">在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="8a74d-106">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="8a74d-107">建立連絡人管理 ASP.NET MVC 應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="8a74d-107">Building a Contact Management ASP.NET MVC Application (C#)</span></span>
  

<span data-ttu-id="8a74d-108">在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="8a74d-108">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="8a74d-109">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="8a74d-109">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="8a74d-110">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="8a74d-110">We build the application over multiple iterations.</span></span> <span data-ttu-id="8a74d-111">與每個反覆項目，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-111">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="8a74d-112">這個多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="8a74d-112">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="8a74d-113">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-113">Iteration #1 - Create the application.</span></span> <span data-ttu-id="8a74d-114">在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="8a74d-114">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="8a74d-115">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="8a74d-115">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="8a74d-116">反覆項目 #2-請看起來很棒的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-116">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="8a74d-117">在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="8a74d-117">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="8a74d-118">反覆項目 #3-加入表單驗證。</span><span class="sxs-lookup"><span data-stu-id="8a74d-118">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="8a74d-119">第三個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="8a74d-119">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="8a74d-120">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="8a74d-120">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="8a74d-121">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="8a74d-121">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="8a74d-122">反覆項目 #4-請鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-122">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="8a74d-123">在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-123">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="8a74d-124">例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-124">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="8a74d-125">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="8a74d-125">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="8a74d-126">第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="8a74d-126">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="8a74d-127">我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="8a74d-127">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="8a74d-128">反覆項目 #6-使用測試為導向的開發。</span><span class="sxs-lookup"><span data-stu-id="8a74d-128">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="8a74d-129">這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8a74d-129">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="8a74d-130">在這個反覆項目，我們會加入連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="8a74d-130">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="8a74d-131">反覆項目 #7： 加入 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="8a74d-131">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="8a74d-132">在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="8a74d-132">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="8a74d-133">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="8a74d-133">This Iteration</span></span>

<span data-ttu-id="8a74d-134">這個反覆項目的是改善連絡人管理員應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="8a74d-134">The goal of this iteration is to improve the appearance of the Contact Manager application.</span></span> <span data-ttu-id="8a74d-135">目前，請連絡管理員會使用預設 ASP.NET MVC 檢視主版頁面和階層式樣式表 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="8a74d-135">Currently, the Contact Manager uses the default ASP.NET MVC view master page and cascading style sheet (see Figure 1).</span></span> <span data-ttu-id="8a74d-136">這些不要看起來不正確，但我不想要看起來就像每個 ASP.NET MVC 網站連絡管理員。</span><span class="sxs-lookup"><span data-stu-id="8a74d-136">These don t look bad, but I don t want the Contact Manager to look just like every other ASP.NET MVC website.</span></span> <span data-ttu-id="8a74d-137">我想要自訂的檔案中取代這些檔案。</span><span class="sxs-lookup"><span data-stu-id="8a74d-137">I want to replace these files with custom files.</span></span>


<span data-ttu-id="8a74d-138">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-138">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)</span></span>

<span data-ttu-id="8a74d-139">**圖 01**: ASP.NET MVC 應用程式的預設外觀 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-139">**Figure 01**: The default appearance of an ASP.NET MVC Application ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image2.png))</span></span>


<span data-ttu-id="8a74d-140">在這個反覆項目，我將討論兩種方法來改善我們的應用程式的視覺化設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-140">In this iteration, I discuss two approaches to improving the visual design of our application.</span></span> <span data-ttu-id="8a74d-141">首先，我示範您如何利用 ASP.NET MVC 設計資源庫下載免費的 ASP.NET MVC 設計範本。</span><span class="sxs-lookup"><span data-stu-id="8a74d-141">First, I show you how to take advantage of the ASP.NET MVC Design gallery to download a free ASP.NET MVC design template.</span></span> <span data-ttu-id="8a74d-142">ASP.NET MVC 設計組件庫可讓您建立專業的 web 應用程式，而不需要執行任何工作。</span><span class="sxs-lookup"><span data-stu-id="8a74d-142">The ASP.NET MVC Design gallery enables you to create a professional web application without doing any work.</span></span>

<span data-ttu-id="8a74d-143">我決定不使用樣板庫中的 ASP.NET MVC 設計連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a74d-143">I decided to not use a template from the ASP.NET MVC Design gallery for the Contact Manager application.</span></span> <span data-ttu-id="8a74d-144">相反地，我必須專業人員設計公司所建立的自訂設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-144">Instead, I had a custom design created by a professional design firm.</span></span> <span data-ttu-id="8a74d-145">在本教學課程的第二個部分中，我將說明如何我曾經與工作來建立最終的 ASP.NET MVC 設計專業人員設計公司。</span><span class="sxs-lookup"><span data-stu-id="8a74d-145">In the second part of this tutorial, I explain how I worked with a professional design company to create the final ASP.NET MVC design.</span></span>

## <a name="the-aspnet-mvc-design-gallery"></a><span data-ttu-id="8a74d-146">ASP.NET MVC 設計組件庫</span><span class="sxs-lookup"><span data-stu-id="8a74d-146">The ASP.NET MVC Design Gallery</span></span>

<span data-ttu-id="8a74d-147">ASP.NET MVC 設計庫是由 Microsoft 提供的免費資源。</span><span class="sxs-lookup"><span data-stu-id="8a74d-147">The ASP.NET MVC Design Gallery is a free resource provided by Microsoft.</span></span> <span data-ttu-id="8a74d-148">ASP.NET MVC 組件庫位於下列位址：</span><span class="sxs-lookup"><span data-stu-id="8a74d-148">The ASP.NET MVC Gallery is located at the following address:</span></span>

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

<span data-ttu-id="8a74d-149">ASP.NET MVC 設計庫主控免費網站設計特別針對使用 ASP.NET MVC 專案中所建立的集合。</span><span class="sxs-lookup"><span data-stu-id="8a74d-149">The ASP.NET MVC Design Gallery hosts a collection of free website designs that were created specifically for using in an ASP.NET MVC project.</span></span> <span data-ttu-id="8a74d-150">設計都會上傳由社群的成員。</span><span class="sxs-lookup"><span data-stu-id="8a74d-150">Designs are uploaded by members of the community.</span></span> <span data-ttu-id="8a74d-151">組件庫的訪客可以在此投票他們最愛的設計 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="8a74d-151">Visitors to the Gallery can vote for their favorite designs (see Figure 2).</span></span>


<span data-ttu-id="8a74d-152">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-152">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)</span></span>

<span data-ttu-id="8a74d-153">**圖 02**: ASP.NET MVC 設計組件庫 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-153">**Figure 02**: The ASP.NET MVC Design Gallery ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image4.png))</span></span>


<span data-ttu-id="8a74d-154">當我撰寫此教學課程中，在圖庫中最常用的設計是 David Hauser 命名年 10 月的設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-154">As I write this tutorial, the most popular design in the gallery is a design named October by David Hauser.</span></span> <span data-ttu-id="8a74d-155">您可以使用 ASP.NET MVC 專案的這項設計藉由完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="8a74d-155">You can use this design for an ASP.NET MVC project by completing the following steps:</span></span>

1. <span data-ttu-id="8a74d-156">按一下**下載**October.zip 檔案下載到您的電腦 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8a74d-156">Click the **Download** button to download the October.zip file to your computer.</span></span>
2. <span data-ttu-id="8a74d-157">以滑鼠右鍵按一下 下載的 October.zip 檔案，然後按一下**解除封鎖**按鈕 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="8a74d-157">Right-click the downloaded October.zip file and click the **Unblock** button (see Figure 3).</span></span>
3. <span data-ttu-id="8a74d-158">解壓縮檔案到名為年 10 月的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a74d-158">Unzip the file to a folder named October.</span></span>
4. <span data-ttu-id="8a74d-159">選取的所有檔案包含在年 10 月資料夾 DesignTemplate 資料夾中，以滑鼠右鍵按一下檔案，並選取功能表選項**複製**。</span><span class="sxs-lookup"><span data-stu-id="8a74d-159">Select all of the files from the DesignTemplate folder contained in the October folder, right-click the files, and select the menu option **Copy**.</span></span>
5. <span data-ttu-id="8a74d-160">以滑鼠右鍵按一下 ContactManager 專案節點，在 Visual Studio 方案總管 視窗中的，然後選取功能表選項**貼上**（請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="8a74d-160">Right-click the ContactManager project node in the Visual Studio Solution Explorer window and select the menu option **Paste** (see Figure 4).</span></span>
6. <span data-ttu-id="8a74d-161">選取 [Visual Studio] 功能表選項**編輯、 尋找和取代 快速取代**和取代 *[MyProjectName]* 與*ContactManager* （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="8a74d-161">Select the Visual Studio menu option **Edit, Find and Replace, Quick Replace** and replace *[MyProjectName]* with *ContactManager* (see Figure 5).</span></span>


<span data-ttu-id="8a74d-162">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-162">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)</span></span>

<span data-ttu-id="8a74d-163">**圖 03**： 解除封鎖檔案從 web 下載 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-163">**Figure 03**: Unblocking a file downloaded from the web ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image6.png))</span></span>


<span data-ttu-id="8a74d-164">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-164">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)</span></span>

<span data-ttu-id="8a74d-165">**圖 04**： 覆寫在 [方案總管] 中的檔案 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-165">**Figure 04**: Overwriting files in the Solution Explorer ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image8.png))</span></span>


<span data-ttu-id="8a74d-166">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-166">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)</span></span>

<span data-ttu-id="8a74d-167">**圖 05**: ContactManager 以取代 [ProjectName] ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-167">**Figure 05**: Replacing [ProjectName] with ContactManager ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image10.png))</span></span>


<span data-ttu-id="8a74d-168">完成這些步驟之後，web 應用程式會使用新的設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-168">After you complete these steps, your web application will use the new design.</span></span> <span data-ttu-id="8a74d-169">圖 6 中的頁面說明與年 10 月設計連絡人管理員應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="8a74d-169">The page in Figure 6 illustrates the appearance of the Contact Manager application with the October design.</span></span>


<span data-ttu-id="8a74d-170">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-170">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)</span></span>

<span data-ttu-id="8a74d-171">**圖 06**: ContactManager 年 10 月範本 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-171">**Figure 06**: ContactManager with the October template ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image12.png))</span></span>


## <a name="creating-a-custom-aspnet-mvc-design"></a><span data-ttu-id="8a74d-172">建立自訂 ASP.NET MVC 設計</span><span class="sxs-lookup"><span data-stu-id="8a74d-172">Creating a Custom ASP.NET MVC Design</span></span>

<span data-ttu-id="8a74d-173">ASP.NET MVC 設計庫有良好的不同的設計樣式選取範圍。</span><span class="sxs-lookup"><span data-stu-id="8a74d-173">The ASP.NET MVC Design Gallery has a good selection of different design styles.</span></span> <span data-ttu-id="8a74d-174">組件庫可提供您較輕鬆的方法，以自訂 ASP.NET MVC 應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="8a74d-174">The Gallery provides you with a painless way to customize the appearance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="8a74d-175">而且，當然，組件庫的大優點是完全免費。</span><span class="sxs-lookup"><span data-stu-id="8a74d-175">And, of course, the Gallery has the big advantage of being completely free.</span></span>

<span data-ttu-id="8a74d-176">不過，您可能需要建立完全獨特的設計為您的網站。</span><span class="sxs-lookup"><span data-stu-id="8a74d-176">However, you might need to create a completely unique design for your website.</span></span> <span data-ttu-id="8a74d-177">在此情況下，因此才會使用網站設計公司。</span><span class="sxs-lookup"><span data-stu-id="8a74d-177">In that case, it makes sense to work with a website design company.</span></span> <span data-ttu-id="8a74d-178">我決定採取這種方式為連絡人管理員應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-178">I decided to take this approach for the design for the Contact Manager application.</span></span>

<span data-ttu-id="8a74d-179">我壓縮反覆項目 # 1 向上連絡管理員，並傳送到設計公司的專案。</span><span class="sxs-lookup"><span data-stu-id="8a74d-179">I zipped up the Contact Manager from Iteration #1 and sent the project to the design company.</span></span> <span data-ttu-id="8a74d-180">他們並未擁有 Visual Studio （遺憾上 ！），但該 professionals t 出現問題。</span><span class="sxs-lookup"><span data-stu-id="8a74d-180">They did not own Visual Studio (shame on them!), but that didn t present a problem.</span></span> <span data-ttu-id="8a74d-181">它們是能從免費下載 Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net)網站並開啟連絡人管理員應用程式在 Visual Web Developer。</span><span class="sxs-lookup"><span data-stu-id="8a74d-181">They were able to download Microsoft Visual Web Developer for free from the [https://www.asp.net](https://www.asp.net) website and open the Contact Manager application in Visual Web Developer.</span></span> <span data-ttu-id="8a74d-182">在幾天，它們必須產生的圖 7 中的設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-182">In a couple of days, they had produced the design in Figure 7.</span></span>


<span data-ttu-id="8a74d-183">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-183">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)</span></span>

<span data-ttu-id="8a74d-184">**圖 07**: 在 ASP.NET MVC，請連絡管理員設計 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-184">**Figure 07**: The ASP.NET MVC Contact Manager Design ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image14.png))</span></span>


<span data-ttu-id="8a74d-185">新的設計是由兩個主要的檔案所組成： 新的階層式樣式表檔案和新的檢視主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="8a74d-185">The new design consisted of two main files: a new cascading style sheet file and a new view master page file.</span></span> <span data-ttu-id="8a74d-186">檢視主版頁面包含的配置和 ASP.NET MVC 應用程式中檢視共用的內容。</span><span class="sxs-lookup"><span data-stu-id="8a74d-186">A view master page contains the layout and shared content for views in an ASP.NET MVC application.</span></span> <span data-ttu-id="8a74d-187">例如，檢視主版頁面濆迶 7 包含標頭、 導覽索引標籤上和會出現的頁尾。</span><span class="sxs-lookup"><span data-stu-id="8a74d-187">For example, the view master page includes the header, navigation tabs, and footer that appear in Figure 7.</span></span> <span data-ttu-id="8a74d-188">我使用設計公司中，新 Site.Master 檔案覆寫現有 Site.Master 檢視主版頁面在 Views\Shared 資料夾</span><span class="sxs-lookup"><span data-stu-id="8a74d-188">I overwrote the existing Site.Master view master page in the Views\Shared folder with the new Site.Master file from the design company,</span></span>

<span data-ttu-id="8a74d-189">設計公司也會建立新的階層式樣式表和一組影像。</span><span class="sxs-lookup"><span data-stu-id="8a74d-189">The design company also created a new cascading style sheet and set of images.</span></span> <span data-ttu-id="8a74d-190">我的內容資料夾中放置這些新的檔案，並覆寫現有的 Site.css 檔案。</span><span class="sxs-lookup"><span data-stu-id="8a74d-190">I placed these new files in the Content folder and overwrote the existing Site.css file.</span></span> <span data-ttu-id="8a74d-191">您應該將所有的靜態內容的內容資料夾中。</span><span class="sxs-lookup"><span data-stu-id="8a74d-191">You should place all static content in the Content folder.</span></span>

<span data-ttu-id="8a74d-192">請注意，新的設計為連絡人管理員包含映像，以編輯和刪除連絡人。</span><span class="sxs-lookup"><span data-stu-id="8a74d-192">Notice that the new design for the Contact Manager includes images for editing and deleting contacts.</span></span> <span data-ttu-id="8a74d-193">編輯和刪除映像出現在旁邊連絡人的 HTML 資料表中每位連絡人。</span><span class="sxs-lookup"><span data-stu-id="8a74d-193">An Edit and Delete image appear next to each contact in the HTML table of contacts.</span></span>

<span data-ttu-id="8a74d-194">一開始由與 HTML 轉譯這些連結。ActionLink() helper，像這樣：</span><span class="sxs-lookup"><span data-stu-id="8a74d-194">Originally, these links that were rendered with the HTML.ActionLink() helper like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

<span data-ttu-id="8a74d-195">Html.ActionLink() 方法不支援映像 （方法 HTML 編碼的連結文字，基於安全性理由）。</span><span class="sxs-lookup"><span data-stu-id="8a74d-195">The Html.ActionLink() method does not support images (the method HTML encodes the link text for security reasons).</span></span> <span data-ttu-id="8a74d-196">因此，我取代 Html.ActionLink() 呼叫呼叫 Url.Action() 像這樣：</span><span class="sxs-lookup"><span data-stu-id="8a74d-196">Therefore, I replaced the calls to Html.ActionLink() with calls to Url.Action() like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

<span data-ttu-id="8a74d-197">Html.ActionLink() 方法呈現整個 HTML 超連結。</span><span class="sxs-lookup"><span data-stu-id="8a74d-197">The Html.ActionLink() method renders an entire HTML hyperlink.</span></span> <span data-ttu-id="8a74d-198">Url.Action() 方法，相反地，呈現不含 URL &lt;&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="8a74d-198">The Url.Action() method, on the other hand, renders just the URL without the &lt;a&gt; tag.</span></span>

<span data-ttu-id="8a74d-199">此外，請注意，新的設計包含選取和取消選取索引標籤。</span><span class="sxs-lookup"><span data-stu-id="8a74d-199">Notice, furthermore, that the new design includes both selected and unselected tabs.</span></span> <span data-ttu-id="8a74d-200">例如，在圖 8**建立新的連絡人**選取索引標籤和**我的連絡人**不選取索引標籤。</span><span class="sxs-lookup"><span data-stu-id="8a74d-200">For example, in Figure 8, the **Create New Contact** tab is selected and the **My Contacts** tab is not selected.</span></span>


<span data-ttu-id="8a74d-201">[![新增專案 對話方塊](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="8a74d-201">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)</span></span>

<span data-ttu-id="8a74d-202">**圖 08**： 選取和取消選取索引標籤 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-cs/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="8a74d-202">**Figure 08**: Selected and unselected tabs([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image16.png))</span></span>


<span data-ttu-id="8a74d-203">若要支援呈現已選取和取消選取索引標籤，建立名為 MenuItemHelper 的自訂 HTML helper。</span><span class="sxs-lookup"><span data-stu-id="8a74d-203">To support rendering both selected and unselected tabs, I created a custom HTML helper named the MenuItemHelper.</span></span> <span data-ttu-id="8a74d-204">這個 helper 方法轉譯是&lt;li&gt;標記或&lt;li 類別 ="選取"&gt;根據目前的控制器和動作是否對應至控制器和動作名稱傳遞給協助專家的標記。</span><span class="sxs-lookup"><span data-stu-id="8a74d-204">This helper method renders either a &lt;li&gt; tag or a &lt;li class="selected"&gt; tag depending on whether the current controller and action corresponds to the controller and action name passed to the helper.</span></span> <span data-ttu-id="8a74d-205">MenuItemHelper 的程式碼會包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="8a74d-205">The code for the MenuItemHelper is contained in Listing 1.</span></span>

<span data-ttu-id="8a74d-206">**Listing 1 - Helpers\MenuItemHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="8a74d-206">**Listing 1 - Helpers\MenuItemHelper.cs**</span></span>

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

<span data-ttu-id="8a74d-207">MenuItemHelper TagBuilder 類別在內部用來建置&lt;li&gt; HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="8a74d-207">The MenuItemHelper uses the TagBuilder class internally to build the &lt;li&gt; HTML tag.</span></span> <span data-ttu-id="8a74d-208">TagBuilder 類別是非常有用的公用程式類別，可讓您視需要來建立新的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="8a74d-208">The TagBuilder class is a very useful utility class that you can use whenever you need to build up a new HTML tag.</span></span> <span data-ttu-id="8a74d-209">它包含了加入屬性、 加入 CSS 類別、 產生識別碼，以及修改標記的方法內部 HTML。</span><span class="sxs-lookup"><span data-stu-id="8a74d-209">It includes methods for adding attributes, adding CSS classes, generating Ids, and modifying the tag s inner HTML.</span></span>

## <a name="summary"></a><span data-ttu-id="8a74d-210">總結</span><span class="sxs-lookup"><span data-stu-id="8a74d-210">Summary</span></span>

<span data-ttu-id="8a74d-211">在這個反覆項目，我們可以改進我們的 ASP.NET MVC 應用程式的視覺化設計。</span><span class="sxs-lookup"><span data-stu-id="8a74d-211">In this iteration, we improved the visual design of our ASP.NET MVC application.</span></span> <span data-ttu-id="8a74d-212">首先，我們將會介紹至 ASP.NET MVC 設計組件庫。</span><span class="sxs-lookup"><span data-stu-id="8a74d-212">First, you were introduced to the ASP.NET MVC Design Gallery.</span></span> <span data-ttu-id="8a74d-213">您已學習如何從您可以在 ASP.NET MVC 應用程式中使用 ASP.NET MVC 設計庫下載免費的設計範本。</span><span class="sxs-lookup"><span data-stu-id="8a74d-213">You learned how to download free design templates from the ASP.NET MVC Design Gallery that you can use in your ASP.NET MVC applications.</span></span>

<span data-ttu-id="8a74d-214">接下來，我們將討論如何建立自訂的設計，藉由修改預設階層式樣式表檔案和主版檢視頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="8a74d-214">Next, we discussed how you can create a custom design by modifying the default cascading style sheet file and master view page file.</span></span> <span data-ttu-id="8a74d-215">為了支援新的設計，我們必須對我們的連絡人管理員應用程式進行一些微幅變更。</span><span class="sxs-lookup"><span data-stu-id="8a74d-215">In order to support the new design, we had to make some minor changes to our Contact Manager application.</span></span> <span data-ttu-id="8a74d-216">比方說，我們加入了新的 HTML helper，名為 MenuItemHelper 顯示選取和取消選取索引標籤。</span><span class="sxs-lookup"><span data-stu-id="8a74d-216">For example, we added a new HTML helper named the MenuItemHelper that displays selected and unselected tabs.</span></span>

<span data-ttu-id="8a74d-217">中的下一個反覆項目，我們可以處理非常重要的主體的驗證。</span><span class="sxs-lookup"><span data-stu-id="8a74d-217">In the next iteration, we tackle the very important subject of validation.</span></span> <span data-ttu-id="8a74d-218">使使用者無法建立新的連絡人，而不需要先提供必要的值，例如個人 s 和姓氏，我們可以加入至我們的應用程式的驗證程式碼。</span><span class="sxs-lookup"><span data-stu-id="8a74d-218">We add validation code to our application so that a user cannot create a new contact without supplying required values such as a person s first and last name.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8a74d-219">[上一頁](iteration-1-create-the-application-cs.md)
> [下一頁](iteration-3-add-form-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8a74d-219">[Previous](iteration-1-create-the-application-cs.md)
[Next](iteration-3-add-form-validation-cs.md)</span></span>
