---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: '反覆項目 #2 – 讓應用程式看起來不錯 (VB) |Microsoft Docs'
author: microsoft
description: 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: a653d3f959756ab7cdc1e0a76e03cde3ec85a378
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396021"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a><span data-ttu-id="ba384-103">反覆項目 #2 – 讓應用程式看起來不錯 (VB)</span><span class="sxs-lookup"><span data-stu-id="ba384-103">Iteration #2 – Make the application look nice (VB)</span></span>
====================
<span data-ttu-id="ba384-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ba384-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ba384-105">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="ba384-105">Download Code</span></span>](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> <span data-ttu-id="ba384-106">這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="ba384-106">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="ba384-107">建立連絡人管理 ASP.NET MVC 應用程式 (VB)</span><span class="sxs-lookup"><span data-stu-id="ba384-107">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="ba384-108">在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="ba384-108">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="ba384-109">請連絡系統管理員應用程式可讓您儲存連絡資訊 – 名稱、 電話號碼和電子郵件地址 – 如需清單的人員。</span><span class="sxs-lookup"><span data-stu-id="ba384-109">The Contact Manager application enables you to store contact information – names, phone numbers and email addresses – for a list of people.</span></span>

<span data-ttu-id="ba384-110">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="ba384-110">We build the application over multiple iterations.</span></span> <span data-ttu-id="ba384-111">每次反覆運算時，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba384-111">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="ba384-112">此多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="ba384-112">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="ba384-113">反覆項目 #1 – 建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba384-113">Iteration #1 – Create the application.</span></span> <span data-ttu-id="ba384-114">在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="ba384-114">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="ba384-115">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="ba384-115">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="ba384-116">反覆項目 #2 – 讓應用程式看起來不錯。</span><span class="sxs-lookup"><span data-stu-id="ba384-116">Iteration #2 – Make the application look nice.</span></span> <span data-ttu-id="ba384-117">這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="ba384-117">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="ba384-118">反覆項目 #3 – 新增表單驗證。</span><span class="sxs-lookup"><span data-stu-id="ba384-118">Iteration #3 – Add form validation.</span></span> <span data-ttu-id="ba384-119">在第三個反覆項目，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="ba384-119">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="ba384-120">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="ba384-120">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="ba384-121">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="ba384-121">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="ba384-122">反覆項目 #4 – 讓應用程式鬆散耦合。</span><span class="sxs-lookup"><span data-stu-id="ba384-122">Iteration #4 – Make the application loosely coupled.</span></span> <span data-ttu-id="ba384-123">在這個第四個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba384-123">In this fourth iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="ba384-124">比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="ba384-124">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="ba384-125">反覆項目 #5 – 建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="ba384-125">Iteration #5 – Create unit tests.</span></span> <span data-ttu-id="ba384-126">在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。</span><span class="sxs-lookup"><span data-stu-id="ba384-126">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="ba384-127">我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。</span><span class="sxs-lookup"><span data-stu-id="ba384-127">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="ba384-128">反覆項目 #6 – 使用測試導向開發。</span><span class="sxs-lookup"><span data-stu-id="ba384-128">Iteration #6 – Use test-driven development.</span></span> <span data-ttu-id="ba384-129">在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba384-129">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="ba384-130">在這個反覆項目，我們會新增連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="ba384-130">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="ba384-131">反覆項目 #7 – 新增 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="ba384-131">Iteration #7 – Add Ajax functionality.</span></span> <span data-ttu-id="ba384-132">在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="ba384-132">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="ba384-133">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="ba384-133">This Iteration</span></span>

<span data-ttu-id="ba384-134">這個反覆項目的目的是要提升的連絡人管理員應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="ba384-134">The goal of this iteration is to improve the appearance of the Contact Manager application.</span></span> <span data-ttu-id="ba384-135">目前，連絡管理員會使用預設 ASP.NET MVC 檢視主版頁面和階層式樣式表的項目，（請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="ba384-135">Currently, the Contact Manager uses the default ASP.NET MVC view master page and cascading style sheet (see Figure 1).</span></span> <span data-ttu-id="ba384-136">這些不要看起來不正確，但我不想要連絡管理員，要看起來就像每個其他的 ASP.NET MVC 網站。</span><span class="sxs-lookup"><span data-stu-id="ba384-136">These don t look bad, but I don t want the Contact Manager to look just like every other ASP.NET MVC website.</span></span> <span data-ttu-id="ba384-137">我想要自訂的檔案中取代這些檔案。</span><span class="sxs-lookup"><span data-stu-id="ba384-137">I want to replace these files with custom files.</span></span>


<span data-ttu-id="ba384-138">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-138">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)</span></span>

<span data-ttu-id="ba384-139">**圖 01**： 在 ASP.NET MVC 應用程式的預設外觀 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-139">**Figure 01**: The default appearance of an ASP.NET MVC Application ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image2.png))</span></span>


<span data-ttu-id="ba384-140">在這個反覆項目，我會討論兩種方法來改善我們的應用程式的視覺化設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-140">In this iteration, I discuss two approaches to improving the visual design of our application.</span></span> <span data-ttu-id="ba384-141">首先，我示範如何利用 ASP.NET MVC 設計資源庫下載免費的 ASP.NET MVC 設計範本。</span><span class="sxs-lookup"><span data-stu-id="ba384-141">First, I show you how to take advantage of the ASP.NET MVC Design gallery to download a free ASP.NET MVC design template.</span></span> <span data-ttu-id="ba384-142">ASP.NET MVC 設計資源庫可讓您建立專業的 web 應用程式，而不執行任何工作。</span><span class="sxs-lookup"><span data-stu-id="ba384-142">The ASP.NET MVC Design gallery enables you to create a professional web application without doing any work.</span></span>

<span data-ttu-id="ba384-143">我決定不使用範本從 ASP.NET MVC 設計資源庫的連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba384-143">I decided to not use a template from the ASP.NET MVC Design gallery for the Contact Manager application.</span></span> <span data-ttu-id="ba384-144">相反地，我的專業設計公司所建立的自訂設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-144">Instead, I had a custom design created by a professional design firm.</span></span> <span data-ttu-id="ba384-145">在本教學課程的第二個部分中，我可以說明我曾經與專業設計公司建立的最終的 ASP.NET MVC 設計的方式。</span><span class="sxs-lookup"><span data-stu-id="ba384-145">In the second part of this tutorial, I explain how I worked with a professional design company to create the final ASP.NET MVC design.</span></span>

## <a name="the-aspnet-mvc-design-gallery"></a><span data-ttu-id="ba384-146">ASP.NET MVC 設計資源庫</span><span class="sxs-lookup"><span data-stu-id="ba384-146">The ASP.NET MVC Design Gallery</span></span>

<span data-ttu-id="ba384-147">ASP.NET MVC 設計資源庫是 Microsoft 所提供的免費資源。</span><span class="sxs-lookup"><span data-stu-id="ba384-147">The ASP.NET MVC Design Gallery is a free resource provided by Microsoft.</span></span> <span data-ttu-id="ba384-148">ASP.NET MVC 組件庫位於下列位址：</span><span class="sxs-lookup"><span data-stu-id="ba384-148">The ASP.NET MVC Gallery is located at the following address:</span></span>

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

<span data-ttu-id="ba384-149">ASP.NET MVC 的設計庫裝載建立專為在 ASP.NET MVC 專案中使用的免費網站設計的集合。</span><span class="sxs-lookup"><span data-stu-id="ba384-149">The ASP.NET MVC Design Gallery hosts a collection of free website designs that were created specifically for using in an ASP.NET MVC project.</span></span> <span data-ttu-id="ba384-150">設計是由社群成員的上傳。</span><span class="sxs-lookup"><span data-stu-id="ba384-150">Designs are uploaded by members of the community.</span></span> <span data-ttu-id="ba384-151">資源庫的訪客可以投票給他們最愛的設計 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="ba384-151">Visitors to the Gallery can vote for their favorite designs (see Figure 2).</span></span>


<span data-ttu-id="ba384-152">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-152">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)</span></span>

<span data-ttu-id="ba384-153">**圖 02**: 將 ASP.NET MVC 設計資源庫 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-153">**Figure 02**: The ASP.NET MVC Design Gallery ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image4.png))</span></span>


<span data-ttu-id="ba384-154">當我撰寫本教學課程中，資源庫中最受歡迎的設計是由 David Hauser 命名年 10 月的設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-154">As I write this tutorial, the most popular design in the gallery is a design named October by David Hauser.</span></span> <span data-ttu-id="ba384-155">您可以使用 ASP.NET MVC 專案的這項設計藉由完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="ba384-155">You can use this design for an ASP.NET MVC project by completing the following steps:</span></span>

1. <span data-ttu-id="ba384-156">按一下 [**下載**October.zip 檔案下載到您的電腦] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ba384-156">Click the **Download** button to download the October.zip file to your computer.</span></span>
2. <span data-ttu-id="ba384-157">以滑鼠右鍵按一下 下載的 October.zip 檔案，然後按一下 **解除封鎖**按鈕 （請參閱 圖 3）。</span><span class="sxs-lookup"><span data-stu-id="ba384-157">Right-click the downloaded October.zip file and click the **Unblock** button (see Figure 3).</span></span>
3. <span data-ttu-id="ba384-158">解壓縮檔案以名為年 10 月的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ba384-158">Unzip the file to a folder named October.</span></span>
4. <span data-ttu-id="ba384-159">從 10 月資料夾中包含 DesignTemplate 資料夾中選取的所有檔案，以滑鼠右鍵按一下檔案，並選取功能表選項**複製**。</span><span class="sxs-lookup"><span data-stu-id="ba384-159">Select all of the files from the DesignTemplate folder contained in the October folder, right-click the files, and select the menu option **Copy**.</span></span>
5. <span data-ttu-id="ba384-160">以滑鼠右鍵按一下 [ContactManager] 專案節點，在 Visual Studio 方案總管] 視窗中的，然後選取功能表選項**貼上**（請參閱 [圖 4）。</span><span class="sxs-lookup"><span data-stu-id="ba384-160">Right-click the ContactManager project node in the Visual Studio Solution Explorer window and select the menu option **Paste** (see Figure 4).</span></span>
6. <span data-ttu-id="ba384-161">選取 [Visual Studio] 功能表選項**編輯、 尋找和取代 快速取代**，並取代 *[MyProjectName]* 具有*ContactManager* （請參閱 [圖 5]）。</span><span class="sxs-lookup"><span data-stu-id="ba384-161">Select the Visual Studio menu option **Edit, Find and Replace, Quick Replace** and replace *[MyProjectName]* with *ContactManager* (see Figure 5).</span></span>


<span data-ttu-id="ba384-162">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-162">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)</span></span>

<span data-ttu-id="ba384-163">**圖 03**： 從 web 解除封鎖檔案下載 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-163">**Figure 03**: Unblocking a file downloaded from the web ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image6.png))</span></span>


<span data-ttu-id="ba384-164">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-164">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)</span></span>

<span data-ttu-id="ba384-165">**圖 04**： 覆寫在 [方案總管] 中的檔案 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-165">**Figure 04**: Overwriting files in the Solution Explorer ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image8.png))</span></span>


<span data-ttu-id="ba384-166">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-166">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)</span></span>

<span data-ttu-id="ba384-167">**圖 05**: [ProjectName] 取代為 ContactManager ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-167">**Figure 05**: Replacing [ProjectName] with ContactManager ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image10.png))</span></span>


<span data-ttu-id="ba384-168">完成這些步驟之後，您的 web 應用程式將使用新的設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-168">After you complete these steps, your web application will use the new design.</span></span> <span data-ttu-id="ba384-169">[圖 6] 頁面會說明與年 10 月設計的連絡人管理員應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="ba384-169">The page in Figure 6 illustrates the appearance of the Contact Manager application with the October design.</span></span>


<span data-ttu-id="ba384-170">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-170">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)</span></span>

<span data-ttu-id="ba384-171">**圖 06**： 使用年 10 月範本 ContactManager ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-171">**Figure 06**: ContactManager with the October template ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image12.png))</span></span>


## <a name="creating-a-custom-aspnet-mvc-design"></a><span data-ttu-id="ba384-172">建立自訂的 ASP.NET MVC 設計</span><span class="sxs-lookup"><span data-stu-id="ba384-172">Creating a Custom ASP.NET MVC Design</span></span>

<span data-ttu-id="ba384-173">ASP.NET MVC 設計資源庫上有不同的設計樣式的好選項。</span><span class="sxs-lookup"><span data-stu-id="ba384-173">The ASP.NET MVC Design Gallery has a good selection of different design styles.</span></span> <span data-ttu-id="ba384-174">資源庫為您提供較輕鬆的方法，以自訂您的 ASP.NET MVC 應用程式的外觀。</span><span class="sxs-lookup"><span data-stu-id="ba384-174">The Gallery provides you with a painless way to customize the appearance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="ba384-175">此外，當然，資源庫中具有最大的好處，就是完全免費。</span><span class="sxs-lookup"><span data-stu-id="ba384-175">And, of course, the Gallery has the big advantage of being completely free.</span></span>

<span data-ttu-id="ba384-176">不過，您可能需要建立完全專為您的網站設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-176">However, you might need to create a completely unique design for your website.</span></span> <span data-ttu-id="ba384-177">在此情況下，它可以合理地使用網站設計公司。</span><span class="sxs-lookup"><span data-stu-id="ba384-177">In that case, it makes sense to work with a website design company.</span></span> <span data-ttu-id="ba384-178">我決定採取這種方式，請連絡管理員應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-178">I decided to take this approach for the design for the Contact Manager application.</span></span>

<span data-ttu-id="ba384-179">我會壓縮設定連絡管理員反覆項目 # 1，並傳送設計公司的專案。</span><span class="sxs-lookup"><span data-stu-id="ba384-179">I zipped up the Contact Manager from Iteration #1 and sent the project to the design company.</span></span> <span data-ttu-id="ba384-180">它們沒有 Visual Studio （可惜在其上 ！），但該嘛 t 出現問題。</span><span class="sxs-lookup"><span data-stu-id="ba384-180">They did not own Visual Studio (shame on them!), but that didn t present a problem.</span></span> <span data-ttu-id="ba384-181">他們能夠從免費下載 Microsoft Visual Web Developer [ https://www.asp.net ](https://www.asp.net)網站並開啟在 Visual Web Developer 中的連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="ba384-181">They were able to download Microsoft Visual Web Developer for free from the [https://www.asp.net](https://www.asp.net) website and open the Contact Manager application in Visual Web Developer.</span></span> <span data-ttu-id="ba384-182">在幾天，它們必須產生圖 7 中的設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-182">In a couple of days, they had produced the design in Figure 7.</span></span>


<span data-ttu-id="ba384-183">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-183">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)</span></span>

<span data-ttu-id="ba384-184">**圖 07**: ASP.NET MVC 的連絡人管理員設計 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-184">**Figure 07**: The ASP.NET MVC Contact Manager Design ([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image14.png))</span></span>


<span data-ttu-id="ba384-185">新的設計是由兩個主要的檔案所組成： 新的階層式樣式表檔案和新的檢視主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="ba384-185">The new design consisted of two main files: a new cascading style sheet file and a new view master page file.</span></span> <span data-ttu-id="ba384-186">檢視主版頁面包含配置和 ASP.NET MVC 應用程式中檢視共用的內容。</span><span class="sxs-lookup"><span data-stu-id="ba384-186">A view master page contains the layout and shared content for views in an ASP.NET MVC application.</span></span> <span data-ttu-id="ba384-187">比方說，檢視主版頁面會包含標頭、 導覽索引標籤中和頁尾會出現 [圖 7] 中。</span><span class="sxs-lookup"><span data-stu-id="ba384-187">For example, the view master page includes the header, navigation tabs, and footer that appear in Figure 7.</span></span> <span data-ttu-id="ba384-188">我已在將現有 Site.Master 檢視主版頁面 Views\Shared 資料夾中的以新的 Site.Master 檔案，從設計公司，覆寫</span><span class="sxs-lookup"><span data-stu-id="ba384-188">I overwrote the existing Site.Master view master page in the Views\Shared folder with the new Site.Master file from the design company,</span></span>

<span data-ttu-id="ba384-189">設計公司也會建立新的階層式樣式表和影像集。</span><span class="sxs-lookup"><span data-stu-id="ba384-189">The design company also created a new cascading style sheet and set of images.</span></span> <span data-ttu-id="ba384-190">我的內容資料夾中放置這些新的檔案，並已覆寫現有的 Site.css 檔案。</span><span class="sxs-lookup"><span data-stu-id="ba384-190">I placed these new files in the Content folder and overwrote the existing Site.css file.</span></span> <span data-ttu-id="ba384-191">您應該將所有靜態內容的內容資料夾中。</span><span class="sxs-lookup"><span data-stu-id="ba384-191">You should place all static content in the Content folder.</span></span>

<span data-ttu-id="ba384-192">請注意，新的設計為連絡人管理員中，包含編輯和刪除連絡人的映像。</span><span class="sxs-lookup"><span data-stu-id="ba384-192">Notice that the new design for the Contact Manager includes images for editing and deleting contacts.</span></span> <span data-ttu-id="ba384-193">編輯和刪除映像旁邊連絡人的 HTML 資料表中的每位連絡人。</span><span class="sxs-lookup"><span data-stu-id="ba384-193">An Edit and Delete image appear next to each contact in the HTML table of contacts.</span></span>

<span data-ttu-id="ba384-194">一開始以 HTML 呈現這些連結。ActionLink() 協助程式，像這樣：</span><span class="sxs-lookup"><span data-stu-id="ba384-194">Originally, these links that were rendered with the HTML.ActionLink() helper like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

<span data-ttu-id="ba384-195">Html.ActionLink() 方法不支援映像 （方法 HTML 編碼的連結文字，基於安全性理由）。</span><span class="sxs-lookup"><span data-stu-id="ba384-195">The Html.ActionLink() method does not support images (the method HTML encodes the link text for security reasons).</span></span> <span data-ttu-id="ba384-196">因此，我會使用 Url.Action() 像這樣的呼叫取代 Html.ActionLink() 呼叫：</span><span class="sxs-lookup"><span data-stu-id="ba384-196">Therefore, I replaced the calls to Html.ActionLink() with calls to Url.Action() like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

<span data-ttu-id="ba384-197">Html.ActionLink() 方法會呈現整個的 HTML 超連結。</span><span class="sxs-lookup"><span data-stu-id="ba384-197">The Html.ActionLink() method renders an entire HTML hyperlink.</span></span> <span data-ttu-id="ba384-198">Url.Action() 方法時，相反地，呈現不含 URL &lt;&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="ba384-198">The Url.Action() method, on the other hand, renders just the URL without the &lt;a&gt; tag.</span></span>

<span data-ttu-id="ba384-199">此外，請注意，新的設計，包含選取或未選取索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ba384-199">Notice, furthermore, that the new design includes both selected and unselected tabs.</span></span> <span data-ttu-id="ba384-200">例如，在 圖 8**建立新的連絡人**已選取索引標籤和**我的連絡人**未選取索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ba384-200">For example, in Figure 8, the **Create New Contact** tab is selected and the **My Contacts** tab is not selected.</span></span>


<span data-ttu-id="ba384-201">[![[新增專案] 對話方塊](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="ba384-201">[![The New Project dialog box](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)</span></span>

<span data-ttu-id="ba384-202">**圖 08**： 選取和取消選取索引標籤 ([按一下以檢視完整大小的影像](iteration-2-make-the-application-look-nice-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="ba384-202">**Figure 08**: Selected and unselected tabs([Click to view full-size image](iteration-2-make-the-application-look-nice-vb/_static/image16.png))</span></span>


<span data-ttu-id="ba384-203">若要支援轉譯選取或未選取索引標籤，我建立了名為 MenuItemHelper 自訂 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="ba384-203">To support rendering both selected and unselected tabs, I created a custom HTML helper named the MenuItemHelper.</span></span> <span data-ttu-id="ba384-204">這個 helper 方法會呈現可能&lt;li&gt;標記或&lt;li 類別 = 選取&gt;取決於目前的控制器和動作是否對應至控制器和動作名稱傳遞給協助專家的標記。</span><span class="sxs-lookup"><span data-stu-id="ba384-204">This helper method renders either a &lt;li&gt; tag or a &lt;li class="selected"&gt; tag depending on whether the current controller and action corresponds to the controller and action name passed to the helper.</span></span> <span data-ttu-id="ba384-205">在 列表 1 中包含 MenuItemHelper 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba384-205">The code for the MenuItemHelper is contained in Listing 1.</span></span>

<span data-ttu-id="ba384-206">**列表 1 – Helpers\MenuItemHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="ba384-206">**Listing 1 – Helpers\MenuItemHelper.vb**</span></span>

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

<span data-ttu-id="ba384-207">MenuItemHelper TagBuilder 類別在內部用來建置&lt;li&gt; HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="ba384-207">The MenuItemHelper uses the TagBuilder class internally to build the &lt;li&gt; HTML tag.</span></span> <span data-ttu-id="ba384-208">TagBuilder 類別是每當您需要建立新的 HTML 標記，您可以使用一個非常實用的公用程式類別。</span><span class="sxs-lookup"><span data-stu-id="ba384-208">The TagBuilder class is a very useful utility class that you can use whenever you need to build up a new HTML tag.</span></span> <span data-ttu-id="ba384-209">它包含了新增屬性、 新增 CSS 類別，產生識別碼，以及修改標記的方法內部 HTML。</span><span class="sxs-lookup"><span data-stu-id="ba384-209">It includes methods for adding attributes, adding CSS classes, generating Ids, and modifying the tag s inner HTML.</span></span>

## <a name="summary"></a><span data-ttu-id="ba384-210">總結</span><span class="sxs-lookup"><span data-stu-id="ba384-210">Summary</span></span>

<span data-ttu-id="ba384-211">在此反覆項目，我們已改善我們的 ASP.NET MVC 應用程式的視覺化設計。</span><span class="sxs-lookup"><span data-stu-id="ba384-211">In this iteration, we improved the visual design of our ASP.NET MVC application.</span></span> <span data-ttu-id="ba384-212">首先，已向您介紹 ASP.NET MVC 設計資源庫。</span><span class="sxs-lookup"><span data-stu-id="ba384-212">First, you were introduced to the ASP.NET MVC Design Gallery.</span></span> <span data-ttu-id="ba384-213">您已了解如何從 ASP.NET MVC 應用程式中，您可以使用 ASP.NET MVC 設計資源庫下載免費的設計範本。</span><span class="sxs-lookup"><span data-stu-id="ba384-213">You learned how to download free design templates from the ASP.NET MVC Design Gallery that you can use in your ASP.NET MVC applications.</span></span>

<span data-ttu-id="ba384-214">接下來，我們會討論如何建立自訂的設計，以及在藉由修改預設階層式樣式表檔案和主版檢視頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="ba384-214">Next, we discussed how you can create a custom design by modifying the default cascading style sheet file and master view page file.</span></span> <span data-ttu-id="ba384-215">為了支援新的設計，我們必須對我們的連絡人管理員應用程式中進行一些微幅的變更。</span><span class="sxs-lookup"><span data-stu-id="ba384-215">In order to support the new design, we had to make some minor changes to our Contact Manager application.</span></span> <span data-ttu-id="ba384-216">比方說，我們已新增新的 HTML 協助程式，名為 MenuItemHelper 顯示所選和取消選取索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ba384-216">For example, we added a new HTML helper named the MenuItemHelper that displays selected and unselected tabs.</span></span>

<span data-ttu-id="ba384-217">中的下一個反覆項目中，我們會處理驗證的極為重要的主題。</span><span class="sxs-lookup"><span data-stu-id="ba384-217">In the next iteration, we tackle the very important subject of validation.</span></span> <span data-ttu-id="ba384-218">讓使用者無法建立新的連絡人，而不需要先提供必要的值，例如： 個人 s 和姓氏，我們可以加入至應用程式的驗證程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba384-218">We add validation code to our application so that a user cannot create a new contact without supplying required values such as a person s first and last name.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ba384-219">[上一頁](iteration-1-create-the-application-vb.md)
> [下一頁](iteration-3-add-form-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ba384-219">[Previous](iteration-1-create-the-application-vb.md)
[Next](iteration-3-add-form-validation-vb.md)</span></span>
