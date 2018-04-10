---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: 了解 ASP.NET AJAX 當地語系化 |Microsoft 文件
author: scottcate
description: 當地語系化是設計並將特定語言和文化特性的支援整合到應用程式或應用程式元件的程序。 Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="understanding-aspnet-ajax-localization"></a><span data-ttu-id="e9979-104">了解 ASP.NET AJAX 當地語系化</span><span class="sxs-lookup"><span data-stu-id="e9979-104">Understanding ASP.NET AJAX Localization</span></span>
====================
<span data-ttu-id="e9979-105">由[Scott 類別](https://github.com/scottcate)</span><span class="sxs-lookup"><span data-stu-id="e9979-105">by [Scott Cate](https://github.com/scottcate)</span></span>

[<span data-ttu-id="e9979-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="e9979-106">Download PDF</span></span>](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> <span data-ttu-id="e9979-107">當地語系化是設計並將特定語言和文化特性的支援整合到應用程式或應用程式元件的程序。</span><span class="sxs-lookup"><span data-stu-id="e9979-107">Localization is the process of designing and integrating support for a specific language and culture into an application or an application component.</span></span> <span data-ttu-id="e9979-108">Microsoft ASP.NET 平台提供適用於標準的 ASP.NET 應用程式的當地語系化藉由整合標準的.NET 當地語系化模型; 廣泛的支援Microsoft AJAX Framework 利用整合式的模型，以支援各種不同的案例可以在其中執行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e9979-108">The Microsoft ASP.NET platform provides extensive support for localization for standard ASP.NET applications by integrating the standard .NET localization model; the Microsoft AJAX Framework utilize the integrated model to support the diverse scenarios in which localization can be performed.</span></span>


## <a name="introduction"></a><span data-ttu-id="e9979-109">簡介</span><span class="sxs-lookup"><span data-stu-id="e9979-109">Introduction</span></span>

<span data-ttu-id="e9979-110">Microsoft ASP.NET 技術，帶來物件導向和事件驅動的程式設計模型，與聯集它已編譯程式碼的優點。</span><span class="sxs-lookup"><span data-stu-id="e9979-110">Microsoft's ASP.NET technology brings an object-oriented and event-driven programming model and unites it with the benefits of compiled code.</span></span> <span data-ttu-id="e9979-111">不過，其伺服器端處理模型有幾個缺點固有的技術，其中有許多可以定址 System.Web.Extensions 命名空間，封裝在.NET Framework 中的 Microsoft AJAX 服務中包含的新功能3.5。</span><span class="sxs-lookup"><span data-stu-id="e9979-111">However, its server-side processing model has several drawbacks inherent in the technology, many of which can be addressed by the new features included in the System.Web.Extensions namespace, which encapsulates the Microsoft AJAX Services in the .NET Framework 3.5.</span></span> <span data-ttu-id="e9979-112">這些延伸可讓許多豐富型用戶端可用的功能先前為 ASP.NET 2.0 AJAX Extensions 中的一部分，但現在的架構基底類別程式庫的一部分。</span><span class="sxs-lookup"><span data-stu-id="e9979-112">These extensions enable many rich client features, previously available as part of the ASP.NET 2.0 AJAX Extensions, but now part of the Framework Base Class Library.</span></span> <span data-ttu-id="e9979-113">控制項和功能，此命名空間中的包含局部網頁呈現，而不需要完整頁面重新整理，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），存取 Web 服務的能力，並且鏡像的許多可廣泛的用戶端 API控制配置，ASP.NET 伺服器端控制項集所示。</span><span class="sxs-lookup"><span data-stu-id="e9979-113">Controls and features in this namespace include partial rendering of pages without requiring a full page refresh, the ability to access Web Services via client script (including the ASP.NET profiling API), and an extensive client-side API designed to mirror many of the control schemes seen in the ASP.NET server-side control set.</span></span>

<span data-ttu-id="e9979-114">本白皮書會檢查存在於 Microsoft AJAX Framework 和 Microsoft AJAX 指令碼程式庫，支援當地語系化和檢閱已經整合支援當地語系化 web 中的商務需求的內容中的當地語系化功能.NET Framework 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e9979-114">This whitepaper examines the localization features present in the Microsoft AJAX Framework and Microsoft AJAX Script Library, in the context of business need for localization support and reviewing already-integrated support for localization in web applications provided by the .NET Framework.</span></span> <span data-ttu-id="e9979-115">Microsoft AJAX 指令碼程式庫會利用.resx 檔案格式已經由.NET 應用程式，它提供整合的 IDE 支援，可共用的資源類型。</span><span class="sxs-lookup"><span data-stu-id="e9979-115">The Microsoft AJAX Script Library utilizes the .resx file format already used by .NET applications, which provides integrated IDE support and a shareable resource type.</span></span>

<span data-ttu-id="e9979-116">本白皮書以 Microsoft Visual Studio 2008 Beta 2 版本上。</span><span class="sxs-lookup"><span data-stu-id="e9979-116">This whitepaper is based on the Beta 2 release of Microsoft Visual Studio 2008.</span></span> <span data-ttu-id="e9979-117">本白皮書也會假設您將使用 Visual Studio 2008 中，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="e9979-117">This whitepaper also assumes that you will be working with Visual Studio 2008, not Visual Web Developer Express, and will provide walkthroughs according to the user interface of Visual Studio.</span></span> <span data-ttu-id="e9979-118">某些程式碼範例會利用可能無法在 Visual Web Developer Express 中的專案範本。</span><span class="sxs-lookup"><span data-stu-id="e9979-118">Some code samples will utilize project templates that may be unavailable in Visual Web Developer Express.</span></span>

## <a name="the-need-for-localization"></a><span data-ttu-id="e9979-119">*需要當地語系化*</span><span class="sxs-lookup"><span data-stu-id="e9979-119">*The Need for Localization*</span></span>

<span data-ttu-id="e9979-120">特別是針對企業應用程式開發人員，元件開發人員能夠建立工具可感知文化特性和語言之間的差異變得越來越必要。</span><span class="sxs-lookup"><span data-stu-id="e9979-120">Particularly for enterprise application developers and component developers, the ability to create tools that can be aware of the differences between cultures and languages has become increasingly necessary.</span></span> <span data-ttu-id="e9979-121">設計元件，以套用到用戶端的地區設定的能力會增加開發人員生產力，並減少調元件的全域函式所需的工作量。</span><span class="sxs-lookup"><span data-stu-id="e9979-121">Designing components with the ability to adapt to the locale of the client increases developer productivity and reduces the amount of work required for the adaptation of a component to function globally.</span></span>

<span data-ttu-id="e9979-122">當地語系化是設計並將特定語言和文化特性的支援整合到應用程式或應用程式元件的程序。</span><span class="sxs-lookup"><span data-stu-id="e9979-122">Localization is the process of designing and integrating support for a specific language and culture into an application or an application component.</span></span> <span data-ttu-id="e9979-123">Microsoft ASP.NET 平台提供適用於標準的 ASP.NET 應用程式的當地語系化藉由整合標準的.NET 當地語系化模型; 廣泛的支援Microsoft AJAX Framework 利用整合式的模型，以支援各種不同的案例可以在其中執行當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e9979-123">The Microsoft ASP.NET platform provides extensive support for localization for standard ASP.NET applications by integrating the standard .NET localization model; the Microsoft AJAX Framework utilize the integrated model to support the diverse scenarios in which localization can be performed.</span></span> <span data-ttu-id="e9979-124">Microsoft AJAX 架構中，所要部署至附屬組件，或藉由使用靜態檔案系統結構可能可以當地語系化指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-124">With the Microsoft AJAX Framework, scripts can either be localized by being deployed into satellite assemblies, or by utilizing a static file system structure.</span></span>

## <a name="embedding-scripts-with-satellite-assemblies"></a><span data-ttu-id="e9979-125">*具有附屬組件的內嵌指令碼*</span><span class="sxs-lookup"><span data-stu-id="e9979-125">*Embedding Scripts with Satellite Assemblies*</span></span>

<span data-ttu-id="e9979-126">與標準.NET Framework 當地語系化策略一致，資源可以包含在附屬組件。</span><span class="sxs-lookup"><span data-stu-id="e9979-126">Consistent with the standard .NET Framework localization strategy, resources can be included in satellite assemblies.</span></span> <span data-ttu-id="e9979-127">附屬組件提供數個優點高於傳統資源包含二進位檔案位在任何給定的當地語系化可以更新而無須更新更大的影像，您可以部署額外的當地語系化資源，只要安裝至附屬組件可以部署的專案資料夾和附屬組件，而不會造成主要專案組件的重新載入。</span><span class="sxs-lookup"><span data-stu-id="e9979-127">Satellite assemblies provide several advantages over traditional resource inclusion in binaries - any given localization can be updated without updating the larger image, additional localizations can be deployed simply by installing satellite assemblies into the project folder, and satellite assemblies can be deployed without causing a reload of the main project assembly.</span></span> <span data-ttu-id="e9979-128">特別是在 ASP.NET 專案，這會是有幫助，因為它可以大幅降低累加式更新時，所使用的系統資源數量，並最少中斷生產網站的使用方式。</span><span class="sxs-lookup"><span data-stu-id="e9979-128">Particularly in ASP.NET projects, this is beneficial because it can significantly reduce the amount of system resources used by incremental updates, and minimally disrupts production website usage.</span></span>

<span data-ttu-id="e9979-129">它們包括指令碼內嵌至組件中受管理的.resx （或編譯.resources） 檔案，會包含在編譯時期組件。</span><span class="sxs-lookup"><span data-stu-id="e9979-129">Scripts are embedded into assemblies by including them in managed .resx (or compiled .resources) files, which are included into the assembly at compile-time.</span></span> <span data-ttu-id="e9979-130">其資源則可透過 AJAX 執行階段產生程式碼，透過組件層級屬性的指令碼應用程式</span><span class="sxs-lookup"><span data-stu-id="e9979-130">Their resources are then made available to the script application through AJAX runtime-generated code, via assembly-level attributes</span></span>

<span data-ttu-id="e9979-131">*內嵌指令碼檔案的命名慣例*</span><span class="sxs-lookup"><span data-stu-id="e9979-131">*Naming Conventions for Embedded Script Files*</span></span>

<span data-ttu-id="e9979-132">Microsoft AJAX Framework 指令碼管理以用於部署和測試指令碼支援各種不同的選項，這些選項，以便提供指導方針。</span><span class="sxs-lookup"><span data-stu-id="e9979-132">The Microsoft AJAX Framework script management supports a variety of options for use in deployment and testing of scripts, and guidelines are provided to facilitate these options.</span></span>

<span data-ttu-id="e9979-133">*為了方便偵錯：*</span><span class="sxs-lookup"><span data-stu-id="e9979-133">*To facilitate debugging:*</span></span>

<span data-ttu-id="e9979-134">發行 （生產環境） 指令碼不應包含`.debug`檔案名稱中的限定詞。</span><span class="sxs-lookup"><span data-stu-id="e9979-134">Release (production) scripts should not include the `.debug` qualifier in the filename.</span></span> <span data-ttu-id="e9979-135">指令碼偵錯應該包含`.debug`檔案名稱中。</span><span class="sxs-lookup"><span data-stu-id="e9979-135">Scripts designed for debugging should include `.debug` in the filename.</span></span>

<span data-ttu-id="e9979-136">*若要協助當地語系化：*</span><span class="sxs-lookup"><span data-stu-id="e9979-136">*To facilitate localization:*</span></span>

<span data-ttu-id="e9979-137">中性文化特性的指令碼不應該包含檔案的名稱的任何文化特性識別項。</span><span class="sxs-lookup"><span data-stu-id="e9979-137">Neutral-culture scripts should not include any culture identifier in the name of the file.</span></span> <span data-ttu-id="e9979-138">包含當地語系化的資源指令碼，應指定 ISO 語言程式碼中的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="e9979-138">For scripts that contain localized resources, the ISO language code should be specified in the file name.</span></span> <span data-ttu-id="e9979-139">例如，`es-CO`代表西班牙文、 哥倫比亞省。</span><span class="sxs-lookup"><span data-stu-id="e9979-139">For example, `es-CO` stands for Spanish, Columbia.</span></span>

<span data-ttu-id="e9979-140">下表摘要說明的檔案命名慣例範例：</span><span class="sxs-lookup"><span data-stu-id="e9979-140">The following table summarizes the file naming conventions with examples:</span></span>

| <span data-ttu-id="e9979-141">Filename</span><span class="sxs-lookup"><span data-stu-id="e9979-141">Filename</span></span> | <span data-ttu-id="e9979-142">意義</span><span class="sxs-lookup"><span data-stu-id="e9979-142">Meaning</span></span> |
| --- | --- |
| <span data-ttu-id="e9979-143">Script.js</span><span class="sxs-lookup"><span data-stu-id="e9979-143">Script.js</span></span> | <span data-ttu-id="e9979-144">發行版本文化特性中性的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-144">A release-version culture-neutral script.</span></span> |
| <span data-ttu-id="e9979-145">Script.debug.js</span><span class="sxs-lookup"><span data-stu-id="e9979-145">Script.debug.js</span></span> | <span data-ttu-id="e9979-146">偵錯版本文化特性中性的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-146">A debug-version culture-neutral script.</span></span> |
| <span data-ttu-id="e9979-147">Script.en-US.js</span><span class="sxs-lookup"><span data-stu-id="e9979-147">Script.en-US.js</span></span> | <span data-ttu-id="e9979-148">發行版英文，美國指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-148">A release version English, United States script.</span></span> |
| <span data-ttu-id="e9979-149">Script.debug.es-CO.js</span><span class="sxs-lookup"><span data-stu-id="e9979-149">Script.debug.es-CO.js</span></span> | <span data-ttu-id="e9979-150">偵錯版本西班牙文，哥倫比亞省的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-150">A debug-version Spanish, Columbia script.</span></span> |

## <a name="walkthrough-create-an-localized-embedded-script"></a><span data-ttu-id="e9979-151">逐步解說： 建立當地語系化的內嵌指令碼</span><span class="sxs-lookup"><span data-stu-id="e9979-151">Walkthrough: Create an Localized, Embedded Script</span></span>

<span data-ttu-id="e9979-152">*請注意： 本逐步解說需要使用 Visual Studio 2008，因為 Visual Web Developer Express 不包括類別庫專案的專案範本。*</span><span class="sxs-lookup"><span data-stu-id="e9979-152">*Please note: this walkthrough requires the use of Visual Studio 2008 as Visual Web Developer Express does not include a project template for class library projects.*</span></span>

1. <span data-ttu-id="e9979-153">建立新的網站專案與 ASP.NET AJAX 擴充功能整合。</span><span class="sxs-lookup"><span data-stu-id="e9979-153">Create a new Web Site project with ASP.NET AJAX Extensions integrated.</span></span> <span data-ttu-id="e9979-154">建立另一個專案，類別庫專案，稱為 LocalizingResources 方案中。</span><span class="sxs-lookup"><span data-stu-id="e9979-154">Create another project, a Class Library project, within the solution called LocalizingResources.</span></span>
2. <span data-ttu-id="e9979-155">加入至 LocalizingResources 專案，稱為 VerifyDeletion.js Jscript 檔案，以及稱為 DeletionResources.resx 和 DeletionResources.es.resx.resx 資源檔。</span><span class="sxs-lookup"><span data-stu-id="e9979-155">Add a Jscript file called VerifyDeletion.js to the LocalizingResources project, as well as .resx resources files called DeletionResources.resx and DeletionResources.es.resx.</span></span> <span data-ttu-id="e9979-156">前者會包含文化特性中性的資源;後者會包含西班牙文語言資源。</span><span class="sxs-lookup"><span data-stu-id="e9979-156">The former will contain culture-neutral resources; the latter will contain Spanish-language resources.</span></span>
3. <span data-ttu-id="e9979-157">將下列程式碼加入至 VerifyDeletion.js:</span><span class="sxs-lookup"><span data-stu-id="e9979-157">Add the following code to VerifyDeletion.js:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

<span data-ttu-id="e9979-158">熟悉 JavaScript Regex 語法中，單一正斜線內文字的 （在上述範例中，/FILENAME/ 是範例） 代表 RegExp 物件。</span><span class="sxs-lookup"><span data-stu-id="e9979-158">For those unfamiliar with JavaScript Regex syntax, text within single forward slashes (in the previous example, /FILENAME/ is an example) denotes a RegExp object.</span></span> <span data-ttu-id="e9979-159">MSDN Library 包含廣泛的 JavaScript 參考，並可以線上找到 JavaScript 原生物件上的資源。</span><span class="sxs-lookup"><span data-stu-id="e9979-159">The MSDN Library contains an extensive JavaScript reference, and resources on JavaScript native objects can be found online.</span></span>

1. <span data-ttu-id="e9979-160">將下列的資源字串加入至 DeletionResources.resx:</span><span class="sxs-lookup"><span data-stu-id="e9979-160">Add the following resource strings to DeletionResources.resx:</span></span> 

    <span data-ttu-id="e9979-161">**VerifyDelete**： 確定您想要刪除的檔案名稱嗎？</span><span class="sxs-lookup"><span data-stu-id="e9979-161">**VerifyDelete**: Are you sure you want to delete FILENAME?</span></span>

    <span data-ttu-id="e9979-162">**刪除**： 已刪除的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="e9979-162">**Deleted**: FILENAME has been deleted.</span></span>

1. <span data-ttu-id="e9979-163">將下列的資源字串加入至 DeletionResources.es.resx:</span><span class="sxs-lookup"><span data-stu-id="e9979-163">Add the following resource strings to DeletionResources.es.resx:</span></span> 

    <span data-ttu-id="e9979-164">**VerifyDelete**: Est seguro 佇列 desee quitar FILENAME 嗎？</span><span class="sxs-lookup"><span data-stu-id="e9979-164">**VerifyDelete**: Est seguro que desee quitar FILENAME?</span></span>

    <span data-ttu-id="e9979-165">**刪除**: FILENAME se ha quitado。</span><span class="sxs-lookup"><span data-stu-id="e9979-165">**Deleted**: FILENAME se ha quitado.</span></span>
2. <span data-ttu-id="e9979-166">將下列程式碼加入至 AssemblyInfo 檔案：</span><span class="sxs-lookup"><span data-stu-id="e9979-166">Add the following lines of code to the AssemblyInfo file:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. <span data-ttu-id="e9979-167">LocalizingResources 專案中加入 System.Web 和 System.Web.Extensions 的參考。</span><span class="sxs-lookup"><span data-stu-id="e9979-167">Add references to System.Web and System.Web.Extensions to the LocalizingResources project.</span></span>
2. <span data-ttu-id="e9979-168">新增 LocalizingResources 專案中的網站專案的參考。</span><span class="sxs-lookup"><span data-stu-id="e9979-168">Add a reference to the LocalizingResources project from the Web Site project.</span></span>
3. <span data-ttu-id="e9979-169">在 default.aspx 中，在網站專案，請使用下列額外的標記更新 ScriptManager 控制項：</span><span class="sxs-lookup"><span data-stu-id="e9979-169">In default.aspx, under the Web Site project, update the ScriptManager control with the following additional markup:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. <span data-ttu-id="e9979-170">在 default.aspx 中，任何位置在頁面上，包含這個標記：</span><span class="sxs-lookup"><span data-stu-id="e9979-170">In default.aspx, anywhere on the page, include this markup:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. <span data-ttu-id="e9979-171">按 F5。</span><span class="sxs-lookup"><span data-stu-id="e9979-171">Press F5.</span></span> <span data-ttu-id="e9979-172">如果出現提示，請啟用偵錯。</span><span class="sxs-lookup"><span data-stu-id="e9979-172">If prompted, enable debugging.</span></span> <span data-ttu-id="e9979-173">當網頁載入時，請按 [刪除] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e9979-173">When the page is loaded, press the Delete button.</span></span> <span data-ttu-id="e9979-174">請注意，系統會提示您以英文 （除非您的電腦設定為慣用西班牙文語言資源上，依預設） 確認。</span><span class="sxs-lookup"><span data-stu-id="e9979-174">Note that you are prompted in English (unless your computer is set to prefer Spanish-language resources by default) for confirmation.</span></span>
2. <span data-ttu-id="e9979-175">關閉瀏覽器視窗並返回 default.aspx。</span><span class="sxs-lookup"><span data-stu-id="e9979-175">Close the browser window and return to default.aspx.</span></span> <span data-ttu-id="e9979-176">在@Page標頭指示詞，取代附有 Culture 和 UICulture ES-ES。</span><span class="sxs-lookup"><span data-stu-id="e9979-176">In the @Page header directive, replace auto for Culture and UICulture with es-ES.</span></span> <span data-ttu-id="e9979-177">按 F5 再次啟動 web 應用程式瀏覽器中的一次。</span><span class="sxs-lookup"><span data-stu-id="e9979-177">Press F5 again to launch the web application in the browser again.</span></span> <span data-ttu-id="e9979-178">這次請注意，系統會提示您刪除西班牙文中的檔案：</span><span class="sxs-lookup"><span data-stu-id="e9979-178">This time, note that you are prompted to delete the file in Spanish:</span></span>


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

<span data-ttu-id="e9979-179">([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e9979-179">([Click to view full-size image](understanding-asp-net-ajax-localization/_static/image3.png))</span></span>


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

<span data-ttu-id="e9979-180">([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e9979-180">([Click to view full-size image](understanding-asp-net-ajax-localization/_static/image6.png))</span></span>


<span data-ttu-id="e9979-181">請注意，此逐步解說中的數個變化。</span><span class="sxs-lookup"><span data-stu-id="e9979-181">Note that there are several variations for this walkthrough.</span></span> <span data-ttu-id="e9979-182">比方說，指令碼無法向 ScriptManager 控制項以程式設計方式在載入頁面時。</span><span class="sxs-lookup"><span data-stu-id="e9979-182">For instance, scripts could be registered with the ScriptManager control programmatically during page load.</span></span>

## <a name="including-a-static-script-file-structure"></a><span data-ttu-id="e9979-183">*包括靜態指令碼檔案結構*</span><span class="sxs-lookup"><span data-stu-id="e9979-183">*Including a Static Script File Structure*</span></span>

<span data-ttu-id="e9979-184">使用靜態指令碼檔案進行部署，便會失去某些使用固有的.NET 當地語系化配置的優點。</span><span class="sxs-lookup"><span data-stu-id="e9979-184">When using static script files for deployment, you lose some of the benefits of using the inherent .NET localization scheme.</span></span> <span data-ttu-id="e9979-185">主要可見時，就會失去自動包括指令碼資源檔; 所產生的型別比方說，在上述的逐步解說中，資源已公開呼叫訊息從 ScriptManager 控制項自動產生型別。</span><span class="sxs-lookup"><span data-stu-id="e9979-185">Primarily visible is that you lose the automatic type generated from including script resource files; in the above walkthrough, for example, resources were exposed by an automatically-generated type called Message from the ScriptManager control.</span></span>

<span data-ttu-id="e9979-186">有，不過，若要使用的靜態指令碼檔案結構的一些優點。</span><span class="sxs-lookup"><span data-stu-id="e9979-186">There are, however, some benefits to using a static script file structure.</span></span> <span data-ttu-id="e9979-187">可以執行更新，而不需要重新編譯和重新部署附屬組件，並使用靜態檔案結構也可以透過覆寫內嵌指令碼，來整合一次要項可能不已寄出的功能與元件。</span><span class="sxs-lookup"><span data-stu-id="e9979-187">Updates can be performed without recompiling and redeploying satellite assemblies, and the use of a static file structure can also be done to override embedded script, to integrate a minor piece of functionality that may not have been shipped with a component.</span></span>

<span data-ttu-id="e9979-188">Microsoft 建議您避免版本控制問題自動在專案的編譯期間產生的指令碼資源。</span><span class="sxs-lookup"><span data-stu-id="e9979-188">Microsoft recommends avoiding a version control issue by automatically generating your script resources during project compilation.</span></span> <span data-ttu-id="e9979-189">當維護廣泛的指令碼程式碼基底會變得愈來愈困難，以確保程式碼變更會反映在每個當地語系化的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-189">When maintaining an extensive script code base, it can become increasingly difficult to ensure that code changes are reflected in each localized script.</span></span> <span data-ttu-id="e9979-190">或者，您可以只維護一個邏輯指令碼和多個當地語系化的指令碼，合併的檔案建立專案時。</span><span class="sxs-lookup"><span data-stu-id="e9979-190">As an alternative, you can simply maintain one logic script and multiple localization scripts, merging the files while building the project.</span></span>

<span data-ttu-id="e9979-191">因為不是以宣告方式包含的資源，靜態指令碼檔案應該參考藉由加入`<asp:ScriptElement>`做為子系的項目`<Scripts>`標記 ScriptManager 控制項，或以程式設計方式加入`ScriptReference`物件若要`Scripts`屬性`ScriptManager`在執行階段頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="e9979-191">Because there are not resources to declaratively include, static script files should be referenced either by adding `<asp:ScriptElement>` elements as a child of the `<Scripts>` tag of the ScriptManager control, or by programmatically adding `ScriptReference` objects to the `Scripts` property of the `ScriptManager` control on the page at runtime.</span></span>

## <a name="the-scriptmanager-and-its-role-in-localization"></a><span data-ttu-id="e9979-192">*ScriptManager 及當地語系化其角色*</span><span class="sxs-lookup"><span data-stu-id="e9979-192">*The ScriptManager and its Role in Localization*</span></span>

<span data-ttu-id="e9979-193">ScriptManager 可讓多個當地語系化的應用程式的自動行為：</span><span class="sxs-lookup"><span data-stu-id="e9979-193">The ScriptManager enables several automatic behaviors for localized applications:</span></span>

- <span data-ttu-id="e9979-194">自動找出指令碼檔案，根據設定和命名慣例。比方說，它會載入偵錯功能的指令碼在偵錯模式中，而且載入當地語系化根據瀏覽器的使用者介面選項的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-194">It automatically locates script files based on settings and naming conventions; for instance, it loads debug-enabled scripts when in debugging mode, and loads localized scripts based on the browser's user interface selection.</span></span>
- <span data-ttu-id="e9979-195">它讓您定義的文化特性，包括自訂文化特性。</span><span class="sxs-lookup"><span data-stu-id="e9979-195">It enables the definition of cultures, including custom cultures.</span></span>
- <span data-ttu-id="e9979-196">它可以透過 HTTP 的指令碼檔案的壓縮。</span><span class="sxs-lookup"><span data-stu-id="e9979-196">It enables the compression of script files over HTTP.</span></span>
- <span data-ttu-id="e9979-197">它會快取，有效地管理多要求的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e9979-197">It caches scripts to efficiently manage many requests.</span></span>
- <span data-ttu-id="e9979-198">將會加入一層間接取值指令碼經由管道它們透過加密的 URL。</span><span class="sxs-lookup"><span data-stu-id="e9979-198">It adds a layer of indirection to scripts by piping them through an encrypted URL.</span></span>

<span data-ttu-id="e9979-199">指令碼參考可以加入 ScriptManager 控制項，以程式設計方式或宣告式標記。</span><span class="sxs-lookup"><span data-stu-id="e9979-199">Script references can be added to the ScriptManager control either programmatically or by declarative markup.</span></span> <span data-ttu-id="e9979-200">宣告式標記為特別有用內嵌在組件的網站專案本身以外使用指令碼時的指令碼名稱可能不會變更為推入修訂。</span><span class="sxs-lookup"><span data-stu-id="e9979-200">Declarative markup is particularly useful when working with scripts embedded in assemblies other than the web site project itself, as the name of the script will likely not change as revisions are pushed through.</span></span>

## <a name="summary"></a><span data-ttu-id="e9979-201">總結</span><span class="sxs-lookup"><span data-stu-id="e9979-201">Summary</span></span>

<span data-ttu-id="e9979-202">當 web 應用程式成長以達到更大的對象時，必須能夠連線到更廣泛的文化特性和 「 社群 」 會變成核心商務模型;電子商務 web 應用程式需要能夠處理外幣，內容管理系統需要以其內容，但也其導覽提示和其他語言和公司中的表單欄位必須知道這項需求是能夠不只存在可存取。</span><span class="sxs-lookup"><span data-stu-id="e9979-202">As web applications grow to reach a larger audience, the need to be able to reach broader cultures and communities becomes core to a business model; e-commerce web applications need to be able to deal with foreign currencies, content management systems need to be able to not only present their content but also their navigation hints and form fields in other languages, and companies need to know that this need is accessible.</span></span>

<span data-ttu-id="e9979-203">.NET Framework 在本質上支援豐富的當地語系化架構，利用附屬組件和 XML 資源 (.resx) 檔案提供統一的方式來尋找資源字串和影像。</span><span class="sxs-lookup"><span data-stu-id="e9979-203">The .NET Framework intrinsically supports a rich localization framework, utilizing satellite assemblies and XML resource (.resx) files to present a uniform way to look up resource strings and images.</span></span> <span data-ttu-id="e9979-204">ASP.NET AJAX 擴充功能，包括 Microsoft AJAX Framework 和 Microsoft AJAX 指令碼程式庫提供支援對此程式設計模型到用戶端程式碼中，啟用簡單的資源字串查閱。</span><span class="sxs-lookup"><span data-stu-id="e9979-204">The ASP.NET AJAX Extensions, including the Microsoft AJAX Framework and Microsoft AJAX Script Library, provide support for this programming model into client-side code, enabling easy resource string lookups.</span></span> <span data-ttu-id="e9979-205">附屬組件支援在自動包含透過 ScriptResource.axd 指令碼資源 （實際的.js 檔案），只要檔案名稱依照指定的命名配置。</span><span class="sxs-lookup"><span data-stu-id="e9979-205">Satellite assemblies support the automatic inclusion of script resources (actual .js files) through ScriptResource.axd as long as the filenames follow a given naming scheme.</span></span> <span data-ttu-id="e9979-206">這項支援，ASP.NET AJAX 擴充功能簡化將指令碼的當地語系化和全球化應用程式。</span><span class="sxs-lookup"><span data-stu-id="e9979-206">With this support, the ASP.NET AJAX Extensions simplify the localization of scripts and the globalization of applications.</span></span>

## <a name="bio"></a><span data-ttu-id="e9979-207">*Bio*</span><span class="sxs-lookup"><span data-stu-id="e9979-207">*Bio*</span></span>

<span data-ttu-id="e9979-208">Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e9979-208">Scott Cate has been working with Microsoft Web technologies since 1997 and is the President of myKB.com ([www.myKB.com](http://www.myKB.com)) where he specializes in writing ASP.NET based applications focused on Knowledge Base Software solutions.</span></span> <span data-ttu-id="e9979-209">透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)</span><span class="sxs-lookup"><span data-stu-id="e9979-209">Scott can be contacted via email at [scott.cate@myKB.com](mailto:scott.cate@myKB.com) or his blog at [ScottCate.com](http://ScottCate.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e9979-210">[上一頁](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [下一頁](understanding-asp-net-ajax-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="e9979-210">[Previous](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
[Next](understanding-asp-net-ajax-web-services.md)</span></span>
