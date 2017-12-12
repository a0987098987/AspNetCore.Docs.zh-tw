---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: "第 6 單元： 使用資料註解的模型驗證 |Microsoft 文件"
author: jongalloway
description: "此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 6 單元涵蓋 V 模型使用資料註解..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="336e3-104">第 6 單元： 使用資料註解的模型驗證</span><span class="sxs-lookup"><span data-stu-id="336e3-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="336e3-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="336e3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="336e3-106">MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。</span><span class="sxs-lookup"><span data-stu-id="336e3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="336e3-107">MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。</span><span class="sxs-lookup"><span data-stu-id="336e3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="336e3-108">此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="336e3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="336e3-109">第 6 單元涵蓋的模型驗證中使用資料註解。</span><span class="sxs-lookup"><span data-stu-id="336e3-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="336e3-110">我們對我們的建立與編輯表單的主要問題： 他們不進行任何驗證。</span><span class="sxs-lookup"><span data-stu-id="336e3-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="336e3-111">我們可以執行一些作業，例如將必要的欄位空白或型別字母留在 [價格] 欄位中，我們會看到的第一個錯誤是來自資料庫。</span><span class="sxs-lookup"><span data-stu-id="336e3-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="336e3-112">我們可以將資料註解加入至我們的模型類別輕鬆加入我們的應用程式的驗證。</span><span class="sxs-lookup"><span data-stu-id="336e3-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="336e3-113">資料附註可讓我們來描述套用至我們的模型屬性，我們想要的規則和 ASP.NET MVC 會負責強制執行的方式，並向使用者顯示適當的訊息。</span><span class="sxs-lookup"><span data-stu-id="336e3-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="336e3-114">將驗證加入至我們的專輯表單</span><span class="sxs-lookup"><span data-stu-id="336e3-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="336e3-115">我們將使用下列資料註解屬性：</span><span class="sxs-lookup"><span data-stu-id="336e3-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="336e3-116">**需要**– 表示此屬性是必要的欄位</span><span class="sxs-lookup"><span data-stu-id="336e3-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="336e3-117">**DisplayName** – 定義我們想要使用的表單欄位和驗證訊息的文字</span><span class="sxs-lookup"><span data-stu-id="336e3-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="336e3-118">**StringLength** – 定義字串欄位的最大長度</span><span class="sxs-lookup"><span data-stu-id="336e3-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="336e3-119">**範圍**– 提供數值欄位的最大和最小值</span><span class="sxs-lookup"><span data-stu-id="336e3-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="336e3-120">**繫結**-列出要繫結模型屬性的參數或表單的值時，包含或排除的欄位</span><span class="sxs-lookup"><span data-stu-id="336e3-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="336e3-121">**ScaffoldColumn** – 允許隱藏編輯器表單中的欄位</span><span class="sxs-lookup"><span data-stu-id="336e3-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="336e3-122">*注意： 如需有關使用資料註解屬性的模型驗證的詳細資訊，請參閱 MSDN 文件，*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="336e3-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="336e3-123">開啟專輯類別並加入下列*使用*上方的陳述式。</span><span class="sxs-lookup"><span data-stu-id="336e3-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="336e3-124">接下來，更新屬性，將顯示和驗證屬性，如下所示。</span><span class="sxs-lookup"><span data-stu-id="336e3-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="336e3-125">雖然我們有，我們也已經虛擬屬性來變更音樂類型與演出者。</span><span class="sxs-lookup"><span data-stu-id="336e3-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="336e3-126">這可讓 Entity Framework 了延遲載入它們為必要。</span><span class="sxs-lookup"><span data-stu-id="336e3-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="336e3-127">之後需要這些屬性加入我們的專輯模型中，我們建立與編輯螢幕立即開始驗證欄位，並使用顯示名稱我們選擇 (例如專輯封面 Url 中而非 AlbumArtUrl)。</span><span class="sxs-lookup"><span data-stu-id="336e3-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="336e3-128">執行應用程式，並瀏覽至 /StoreManager/Create。</span><span class="sxs-lookup"><span data-stu-id="336e3-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="336e3-129">接下來，我們將會中斷某些驗證規則。</span><span class="sxs-lookup"><span data-stu-id="336e3-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="336e3-130">輸入價格為 0，並將標題保留空白。</span><span class="sxs-lookup"><span data-stu-id="336e3-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="336e3-131">當我們按一下 [建立] 按鈕時，我們會看到顯示驗證錯誤訊息顯示哪些欄位不符合驗證規則，我們已定義的表單。</span><span class="sxs-lookup"><span data-stu-id="336e3-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="336e3-132">測試用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="336e3-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="336e3-133">伺服器端驗證是從應用程式的觀點而言，非常重要，因為使用者可以規避用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="336e3-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="336e3-134">但是，只實作伺服器端驗證的網頁表單會呈現三個重要的問題。</span><span class="sxs-lookup"><span data-stu-id="336e3-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="336e3-135">使用者必須等候表單張貼、 驗證的伺服器上，且傳送至瀏覽器的回應。</span><span class="sxs-lookup"><span data-stu-id="336e3-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="336e3-136">使用者不會收到立即回應，當它們更正欄位，使它現在通過驗證規則。</span><span class="sxs-lookup"><span data-stu-id="336e3-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="336e3-137">我們會浪費伺服器資源用來執行驗證邏輯，而不是利用使用者的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="336e3-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="336e3-138">幸運的是，ASP.NET MVC 3 scaffold 範本具有用戶端驗證內建的需要採取任何其他恕不另行通知。</span><span class="sxs-lookup"><span data-stu-id="336e3-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="336e3-139">在 [標題] 欄位中輸入單一字母滿足驗證需求，因此驗證訊息會立即移除。</span><span class="sxs-lookup"><span data-stu-id="336e3-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
<span data-ttu-id="336e3-140">[上一頁](mvc-music-store-part-5.md)
[下一頁](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="336e3-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
