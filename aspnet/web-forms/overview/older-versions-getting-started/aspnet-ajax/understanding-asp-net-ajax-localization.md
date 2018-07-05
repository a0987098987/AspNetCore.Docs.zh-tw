---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: 了解 ASP.NET AJAX 當地語系化 |Microsoft Docs
author: scottcate
description: 當地語系化是設計及整合應用程式或應用程式元件的支援特定語言和文化特性的程序。 Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 7f089e147ae9c4c42da0ca798149488043480a79
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382504"
---
<a name="understanding-aspnet-ajax-localization"></a><span data-ttu-id="dd5a1-104">了解 ASP.NET AJAX 當地語系化</span><span class="sxs-lookup"><span data-stu-id="dd5a1-104">Understanding ASP.NET AJAX Localization</span></span>
====================
<span data-ttu-id="dd5a1-105">藉由[Scott Cate](https://github.com/scottcate)</span><span class="sxs-lookup"><span data-stu-id="dd5a1-105">by [Scott Cate](https://github.com/scottcate)</span></span>

[<span data-ttu-id="dd5a1-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="dd5a1-106">Download PDF</span></span>](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> <span data-ttu-id="dd5a1-107">當地語系化是設計及整合應用程式或應用程式元件的支援特定語言和文化特性的程序。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-107">Localization is the process of designing and integrating support for a specific language and culture into an application or an application component.</span></span> <span data-ttu-id="dd5a1-108">Microsoft ASP.NET 平台進行標準的 ASP.NET 應用程式的當地語系化可廣泛支援，藉由整合標準的.NET 當地語系化模型;Microsoft AJAX 架構利用整合式的模型，以支援各種可以在其中執行當地語系化的情況。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-108">The Microsoft ASP.NET platform provides extensive support for localization for standard ASP.NET applications by integrating the standard .NET localization model; the Microsoft AJAX Framework utilize the integrated model to support the diverse scenarios in which localization can be performed.</span></span>


## <a name="introduction"></a><span data-ttu-id="dd5a1-109">簡介</span><span class="sxs-lookup"><span data-stu-id="dd5a1-109">Introduction</span></span>

<span data-ttu-id="dd5a1-110">Microsoft ASP.NET 技術會以物件導向和事件導向的程式設計模型，以及結合編譯程式碼的優點。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-110">Microsoft's ASP.NET technology brings an object-oriented and event-driven programming model and unites it with the benefits of compiled code.</span></span> <span data-ttu-id="dd5a1-111">不過，其伺服器端處理模型有幾個缺點固有的技術，而其中有許多您可以解決包含封裝在.NET Framework 中的 Microsoft AJAX 服務 System.Web.Extensions 命名空間中的新功能3.5。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-111">However, its server-side processing model has several drawbacks inherent in the technology, many of which can be addressed by the new features included in the System.Web.Extensions namespace, which encapsulates the Microsoft AJAX Services in the .NET Framework 3.5.</span></span> <span data-ttu-id="dd5a1-112">這些擴充功能可讓許多豐富的用戶端可用的功能先前為 ASP.NET 2.0 AJAX Extensions 中的一部分，但現在的 Framework 基底類別庫的一部分。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-112">These extensions enable many rich client features, previously available as part of the ASP.NET 2.0 AJAX Extensions, but now part of the Framework Base Class Library.</span></span> <span data-ttu-id="dd5a1-113">控制項和功能，此命名空間中的包含部分呈現的頁面，而不需要重新整理整個頁面，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），存取 Web 服務的能力，而且廣泛的用戶端 API 設計以鏡像的許多在 ASP.NET 伺服器端控制項集合中看到控制項配置。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-113">Controls and features in this namespace include partial rendering of pages without requiring a full page refresh, the ability to access Web Services via client script (including the ASP.NET profiling API), and an extensive client-side API designed to mirror many of the control schemes seen in the ASP.NET server-side control set.</span></span>

<span data-ttu-id="dd5a1-114">本白皮書會檢查在 Microsoft AJAX Framework 和 Microsoft AJAX 指令碼程式庫，支援當地語系化和檢閱已經整合支援當地語系化 web 中的商務需求的內容中存在的當地語系化功能.NET Framework 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-114">This whitepaper examines the localization features present in the Microsoft AJAX Framework and Microsoft AJAX Script Library, in the context of business need for localization support and reviewing already-integrated support for localization in web applications provided by the .NET Framework.</span></span> <span data-ttu-id="dd5a1-115">Microsoft AJAX 指令碼程式庫會使用.resx 檔案格式已經由.NET 應用程式，可提供整合的 IDE 支援，以及可共用的資源類型。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-115">The Microsoft AJAX Script Library utilizes the .resx file format already used by .NET applications, which provides integrated IDE support and a shareable resource type.</span></span>

<span data-ttu-id="dd5a1-116">本白皮書為基礎的 Microsoft Visual Studio 2008 Beta 2 版本上。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-116">This whitepaper is based on the Beta 2 release of Microsoft Visual Studio 2008.</span></span> <span data-ttu-id="dd5a1-117">本白皮書也會假設您將使用 Visual Studio 2008，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-117">This whitepaper also assumes that you will be working with Visual Studio 2008, not Visual Web Developer Express, and will provide walkthroughs according to the user interface of Visual Studio.</span></span> <span data-ttu-id="dd5a1-118">某些程式碼範例會利用可能無法在 Visual Web Developer Express 的專案範本。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-118">Some code samples will utilize project templates that may be unavailable in Visual Web Developer Express.</span></span>

## <a name="the-need-for-localization"></a><span data-ttu-id="dd5a1-119">*需要當地語系化*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-119">*The Need for Localization*</span></span>

<span data-ttu-id="dd5a1-120">特別是針對企業應用程式開發人員和元件開發人員，能夠建立工具，可以很清楚的文化特性和語言之間的差異變得越來越必要。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-120">Particularly for enterprise application developers and component developers, the ability to create tools that can be aware of the differences between cultures and languages has become increasingly necessary.</span></span> <span data-ttu-id="dd5a1-121">設計元件能夠適應的用戶端的地區設定時，會增加開發人員的生產力，並減少元件調節全域函式所需的工作量。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-121">Designing components with the ability to adapt to the locale of the client increases developer productivity and reduces the amount of work required for the adaptation of a component to function globally.</span></span>

<span data-ttu-id="dd5a1-122">當地語系化是設計及整合應用程式或應用程式元件的支援特定語言和文化特性的程序。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-122">Localization is the process of designing and integrating support for a specific language and culture into an application or an application component.</span></span> <span data-ttu-id="dd5a1-123">Microsoft ASP.NET 平台進行標準的 ASP.NET 應用程式的當地語系化可廣泛支援，藉由整合標準的.NET 當地語系化模型;Microsoft AJAX 架構利用整合式的模型，以支援各種可以在其中執行當地語系化的情況。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-123">The Microsoft ASP.NET platform provides extensive support for localization for standard ASP.NET applications by integrating the standard .NET localization model; the Microsoft AJAX Framework utilize the integrated model to support the diverse scenarios in which localization can be performed.</span></span> <span data-ttu-id="dd5a1-124">Microsoft AJAX 架構中，所要部署到附屬組件，或藉由使用靜態檔案系統結構可能可以當地語系化指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-124">With the Microsoft AJAX Framework, scripts can either be localized by being deployed into satellite assemblies, or by utilizing a static file system structure.</span></span>

## <a name="embedding-scripts-with-satellite-assemblies"></a><span data-ttu-id="dd5a1-125">*使用附屬組件的內嵌指令碼*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-125">*Embedding Scripts with Satellite Assemblies*</span></span>

<span data-ttu-id="dd5a1-126">標準的.NET Framework 當地語系化策略與一致，資源可以包含在附屬組件。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-126">Consistent with the standard .NET Framework localization strategy, resources can be included in satellite assemblies.</span></span> <span data-ttu-id="dd5a1-127">附屬組件提供數個優點高於傳統的資源包含二進位檔-在任何指定的本地化可以更新而不需要更新較大的映像，可以部署額外的當地語系化資源，只要安裝附屬組件載入可以部署的專案資料夾和附屬組件，而不會造成主要專案組件的重新載入。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-127">Satellite assemblies provide several advantages over traditional resource inclusion in binaries - any given localization can be updated without updating the larger image, additional localizations can be deployed simply by installing satellite assemblies into the project folder, and satellite assemblies can be deployed without causing a reload of the main project assembly.</span></span> <span data-ttu-id="dd5a1-128">特別是在 ASP.NET 專案，這會有幫助，因為它可大幅減少累加式更新時，所使用的系統資源數量，並以最低限度中斷生產網站。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-128">Particularly in ASP.NET projects, this is beneficial because it can significantly reduce the amount of system resources used by incremental updates, and minimally disrupts production website usage.</span></span>

<span data-ttu-id="dd5a1-129">指令碼時，會內嵌至組件上，它們包括在管理.resx （或編譯.resources） 的組件在編譯時期包含的檔案。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-129">Scripts are embedded into assemblies by including them in managed .resx (or compiled .resources) files, which are included into the assembly at compile-time.</span></span> <span data-ttu-id="dd5a1-130">其資源會開放給指令碼應用程式，透過 AJAX 執行階段產生的程式碼，透過組件層級屬性</span><span class="sxs-lookup"><span data-stu-id="dd5a1-130">Their resources are then made available to the script application through AJAX runtime-generated code, via assembly-level attributes</span></span>

<span data-ttu-id="dd5a1-131">*內嵌指令碼檔案的命名慣例*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-131">*Naming Conventions for Embedded Script Files*</span></span>

<span data-ttu-id="dd5a1-132">Microsoft AJAX 架構指令碼管理支援各種不同的選項，以用於部署和測試指令碼，並提供指導方針來加速這些選項。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-132">The Microsoft AJAX Framework script management supports a variety of options for use in deployment and testing of scripts, and guidelines are provided to facilitate these options.</span></span>

<span data-ttu-id="dd5a1-133">*為了方便偵錯：*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-133">*To facilitate debugging:*</span></span>

<span data-ttu-id="dd5a1-134">發行 （生產） 指令碼不應包含`.debug`檔案名稱中的限定詞。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-134">Release (production) scripts should not include the `.debug` qualifier in the filename.</span></span> <span data-ttu-id="dd5a1-135">指令碼設計應包括偵錯`.debug`檔案名稱中。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-135">Scripts designed for debugging should include `.debug` in the filename.</span></span>

<span data-ttu-id="dd5a1-136">*若要協助當地語系化：*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-136">*To facilitate localization:*</span></span>

<span data-ttu-id="dd5a1-137">中性文化特性的指令碼不應包含任何檔案名稱的文化特性識別項。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-137">Neutral-culture scripts should not include any culture identifier in the name of the file.</span></span> <span data-ttu-id="dd5a1-138">針對包含當地語系化的資源的指令碼，應指定 ISO 語言程式碼中的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-138">For scripts that contain localized resources, the ISO language code should be specified in the file name.</span></span> <span data-ttu-id="dd5a1-139">比方說，`es-CO`代表西班牙文、 哥倫比亞省。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-139">For example, `es-CO` stands for Spanish, Columbia.</span></span>

<span data-ttu-id="dd5a1-140">下表摘要說明的檔案命名慣例範例：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-140">The following table summarizes the file naming conventions with examples:</span></span>

| <span data-ttu-id="dd5a1-141">Filename</span><span class="sxs-lookup"><span data-stu-id="dd5a1-141">Filename</span></span> | <span data-ttu-id="dd5a1-142">意義</span><span class="sxs-lookup"><span data-stu-id="dd5a1-142">Meaning</span></span> |
| --- | --- |
| <span data-ttu-id="dd5a1-143">Script.js</span><span class="sxs-lookup"><span data-stu-id="dd5a1-143">Script.js</span></span> | <span data-ttu-id="dd5a1-144">發行版本文化特性中性的指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-144">A release-version culture-neutral script.</span></span> |
| <span data-ttu-id="dd5a1-145">Script.debug.js</span><span class="sxs-lookup"><span data-stu-id="dd5a1-145">Script.debug.js</span></span> | <span data-ttu-id="dd5a1-146">偵錯版本的文化特性中性的指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-146">A debug-version culture-neutral script.</span></span> |
| <span data-ttu-id="dd5a1-147">Script.en-US.js</span><span class="sxs-lookup"><span data-stu-id="dd5a1-147">Script.en-US.js</span></span> | <span data-ttu-id="dd5a1-148">發行版英文，美國指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-148">A release version English, United States script.</span></span> |
| <span data-ttu-id="dd5a1-149">Script.debug.es-CO.js</span><span class="sxs-lookup"><span data-stu-id="dd5a1-149">Script.debug.es-CO.js</span></span> | <span data-ttu-id="dd5a1-150">偵錯版本西班牙文，哥倫比亞指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-150">A debug-version Spanish, Columbia script.</span></span> |

## <a name="walkthrough-create-an-localized-embedded-script"></a><span data-ttu-id="dd5a1-151">逐步解說： 建立當地語系化的內嵌指令碼</span><span class="sxs-lookup"><span data-stu-id="dd5a1-151">Walkthrough: Create an Localized, Embedded Script</span></span>

<span data-ttu-id="dd5a1-152">*請注意： 本逐步解說需要 Visual Studio 2008 使用，因為 Visual Web Developer Express 不包括類別庫專案的專案範本。*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-152">*Please note: this walkthrough requires the use of Visual Studio 2008 as Visual Web Developer Express does not include a project template for class library projects.*</span></span>

1. <span data-ttu-id="dd5a1-153">使用整合的 ASP.NET AJAX Extensions 中建立新的網站專案。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-153">Create a new Web Site project with ASP.NET AJAX Extensions integrated.</span></span> <span data-ttu-id="dd5a1-154">建立另一個專案，稱為 LocalizingResources 方案內的類別庫專案。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-154">Create another project, a Class Library project, within the solution called LocalizingResources.</span></span>
2. <span data-ttu-id="dd5a1-155">新增至 LocalizingResources 專案，稱為 VerifyDeletion.js Jscript 檔案，以及稱為 DeletionResources.resx 和 DeletionResources.es.resx.resx 資源檔。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-155">Add a Jscript file called VerifyDeletion.js to the LocalizingResources project, as well as .resx resources files called DeletionResources.resx and DeletionResources.es.resx.</span></span> <span data-ttu-id="dd5a1-156">前者會包含文化特性中性的資源;後者會包含西班牙文語言資源。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-156">The former will contain culture-neutral resources; the latter will contain Spanish-language resources.</span></span>
3. <span data-ttu-id="dd5a1-157">將下列程式碼新增至 VerifyDeletion.js 中：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-157">Add the following code to VerifyDeletion.js:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

<span data-ttu-id="dd5a1-158">對於熟悉 JavaScript 的 Regex 語法，在單一的正斜線的文字 （在上述範例中，/FILENAME/ 是舉例） 表示的 RegExp 物件。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-158">For those unfamiliar with JavaScript Regex syntax, text within single forward slashes (in the previous example, /FILENAME/ is an example) denotes a RegExp object.</span></span> <span data-ttu-id="dd5a1-159">MSDN Library 包含廣泛的 JavaScript 參考，線上找到 JavaScript 原生物件上的資源。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-159">The MSDN Library contains an extensive JavaScript reference, and resources on JavaScript native objects can be found online.</span></span>

1. <span data-ttu-id="dd5a1-160">將下列的資源字串加入至 DeletionResources.resx 中：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-160">Add the following resource strings to DeletionResources.resx:</span></span> 

    <span data-ttu-id="dd5a1-161">**VerifyDelete**： 確定您想要刪除的檔案名稱嗎？</span><span class="sxs-lookup"><span data-stu-id="dd5a1-161">**VerifyDelete**: Are you sure you want to delete FILENAME?</span></span>

    <span data-ttu-id="dd5a1-162">**刪除**： 已刪除的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-162">**Deleted**: FILENAME has been deleted.</span></span>

1. <span data-ttu-id="dd5a1-163">將下列的資源字串加入至 DeletionResources.es.resx 中：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-163">Add the following resource strings to DeletionResources.es.resx:</span></span> 

    <span data-ttu-id="dd5a1-164">**VerifyDelete**: Est seguro 查詢 desee quitar FILENAME 嗎？</span><span class="sxs-lookup"><span data-stu-id="dd5a1-164">**VerifyDelete**: Est seguro que desee quitar FILENAME?</span></span>

    <span data-ttu-id="dd5a1-165">**刪除**: FILENAME se ha quitado。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-165">**Deleted**: FILENAME se ha quitado.</span></span>
2. <span data-ttu-id="dd5a1-166">AssemblyInfo 檔案中加入下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-166">Add the following lines of code to the AssemblyInfo file:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. <span data-ttu-id="dd5a1-167">LocalizingResources 專案中加入 System.Web 和 System.Web.Extensions 的參考。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-167">Add references to System.Web and System.Web.Extensions to the LocalizingResources project.</span></span>
2. <span data-ttu-id="dd5a1-168">新增 LocalizingResources 專案中的網站專案的參考。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-168">Add a reference to the LocalizingResources project from the Web Site project.</span></span>
3. <span data-ttu-id="dd5a1-169">在 default.aspx 中，在網站專案中，請使用下列額外的標記更新 ScriptManager 控制項：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-169">In default.aspx, under the Web Site project, update the ScriptManager control with the following additional markup:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. <span data-ttu-id="dd5a1-170">在 default.aspx 中，在頁面上，任何一處包含此標記：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-170">In default.aspx, anywhere on the page, include this markup:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. <span data-ttu-id="dd5a1-171">按 F5。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-171">Press F5.</span></span> <span data-ttu-id="dd5a1-172">出現提示時，啟用 偵錯。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-172">If prompted, enable debugging.</span></span> <span data-ttu-id="dd5a1-173">載入頁面時，請按 [刪除] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-173">When the page is loaded, press the Delete button.</span></span> <span data-ttu-id="dd5a1-174">請注意，系統會提示您以英文 （除非您的電腦設定為慣用西班牙文語言資源上，依預設） 進行確認。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-174">Note that you are prompted in English (unless your computer is set to prefer Spanish-language resources by default) for confirmation.</span></span>
2. <span data-ttu-id="dd5a1-175">關閉瀏覽器視窗並返回 default.aspx。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-175">Close the browser window and return to default.aspx.</span></span> <span data-ttu-id="dd5a1-176">在 @Page標頭指示詞，取代附有的 Culture 與 UICulture ES-ES。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-176">In the @Page header directive, replace auto for Culture and UICulture with es-ES.</span></span> <span data-ttu-id="dd5a1-177">再次按 F5 以啟動 web 應用程式在瀏覽器中的一次。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-177">Press F5 again to launch the web application in the browser again.</span></span> <span data-ttu-id="dd5a1-178">這次請注意，系統會提示您刪除西班牙文中的檔案：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-178">This time, note that you are prompted to delete the file in Spanish:</span></span>


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

<span data-ttu-id="dd5a1-179">([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dd5a1-179">([Click to view full-size image](understanding-asp-net-ajax-localization/_static/image3.png))</span></span>


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

<span data-ttu-id="dd5a1-180">([按一下以檢視完整大小的影像](understanding-asp-net-ajax-localization/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="dd5a1-180">([Click to view full-size image](understanding-asp-net-ajax-localization/_static/image6.png))</span></span>


<span data-ttu-id="dd5a1-181">請注意，在此逐步解說的數個變化。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-181">Note that there are several variations for this walkthrough.</span></span> <span data-ttu-id="dd5a1-182">比方說，指令碼無法向 ScriptManager 控制項以程式設計方式在載入頁面時。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-182">For instance, scripts could be registered with the ScriptManager control programmatically during page load.</span></span>

## <a name="including-a-static-script-file-structure"></a><span data-ttu-id="dd5a1-183">*包含靜態指令碼檔案結構*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-183">*Including a Static Script File Structure*</span></span>

<span data-ttu-id="dd5a1-184">當部署使用靜態指令碼檔案，可能會遺失部分使用的固有的.NET 當地語系化配置的優點。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-184">When using static script files for deployment, you lose some of the benefits of using the inherent .NET localization scheme.</span></span> <span data-ttu-id="dd5a1-185">顯示主要是讓您將無法自動從包含指令碼資源檔; 產生的型別例如，在上述逐步解說中，資源已公開所自動產生的型別呼叫從 ScriptManager 控制項的訊息。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-185">Primarily visible is that you lose the automatic type generated from including script resource files; in the above walkthrough, for example, resources were exposed by an automatically-generated type called Message from the ScriptManager control.</span></span>

<span data-ttu-id="dd5a1-186">有，不過，若要使用靜態指令碼檔案結構的一些優點。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-186">There are, however, some benefits to using a static script file structure.</span></span> <span data-ttu-id="dd5a1-187">可以執行更新，不必重新編譯和重新部署附屬組件，並使用靜態檔案結構也可以覆寫內嵌指令碼，將次要的一種可能尚未運送的功能與元件。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-187">Updates can be performed without recompiling and redeploying satellite assemblies, and the use of a static file structure can also be done to override embedded script, to integrate a minor piece of functionality that may not have been shipped with a component.</span></span>

<span data-ttu-id="dd5a1-188">Microsoft 建議您避免版本控制問題，自動在專案編譯期間產生的指令碼資源。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-188">Microsoft recommends avoiding a version control issue by automatically generating your script resources during project compilation.</span></span> <span data-ttu-id="dd5a1-189">當維護廣泛的指令碼程式碼基底，它可能會變得越來越難確保程式碼變更會反映在每個當地語系化的指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-189">When maintaining an extensive script code base, it can become increasingly difficult to ensure that code changes are reflected in each localized script.</span></span> <span data-ttu-id="dd5a1-190">或者，您可以只維護一個邏輯的指令碼和多個當地語系化的指令碼，建置專案時，合併檔案。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-190">As an alternative, you can simply maintain one logic script and multiple localization scripts, merging the files while building the project.</span></span>

<span data-ttu-id="dd5a1-191">因為不是以宣告方式包含的資源，靜態指令碼檔案都應參考藉由新增`<asp:ScriptElement>`元素的子系`<Scripts>`標記的 ScriptManager 控制項，或以程式設計方式加入`ScriptReference`物件若要`Scripts`屬性`ScriptManager`在執行階段頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-191">Because there are not resources to declaratively include, static script files should be referenced either by adding `<asp:ScriptElement>` elements as a child of the `<Scripts>` tag of the ScriptManager control, or by programmatically adding `ScriptReference` objects to the `Scripts` property of the `ScriptManager` control on the page at runtime.</span></span>

## <a name="the-scriptmanager-and-its-role-in-localization"></a><span data-ttu-id="dd5a1-192">*ScriptManager 和它在當地語系化的角色*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-192">*The ScriptManager and its Role in Localization*</span></span>

<span data-ttu-id="dd5a1-193">ScriptManager 可讓多個當地語系化的應用程式的自動行為：</span><span class="sxs-lookup"><span data-stu-id="dd5a1-193">The ScriptManager enables several automatic behaviors for localized applications:</span></span>

- <span data-ttu-id="dd5a1-194">它會自動找出設定，以及命名慣例; 為基礎的指令碼檔案比方說，它會載入偵錯啟用在偵錯模式中的指令碼，並載入當地語系化根據瀏覽器的使用者介面選項的指令碼。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-194">It automatically locates script files based on settings and naming conventions; for instance, it loads debug-enabled scripts when in debugging mode, and loads localized scripts based on the browser's user interface selection.</span></span>
- <span data-ttu-id="dd5a1-195">它讓您定義的文化特性，包括自訂文化特性。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-195">It enables the definition of cultures, including custom cultures.</span></span>
- <span data-ttu-id="dd5a1-196">它會透過 HTTP 啟用指令碼檔案的壓縮。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-196">It enables the compression of script files over HTTP.</span></span>
- <span data-ttu-id="dd5a1-197">它會快取指令碼，以有效率地管理許多要求。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-197">It caches scripts to efficiently manage many requests.</span></span>
- <span data-ttu-id="dd5a1-198">它會加入一層間接取值的指令碼傳遞它們透過加密的 URL。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-198">It adds a layer of indirection to scripts by piping them through an encrypted URL.</span></span>

<span data-ttu-id="dd5a1-199">指令碼參考可以透過程式設計方式或宣告式標記加入 ScriptManager 控制項。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-199">Script references can be added to the ScriptManager control either programmatically or by declarative markup.</span></span> <span data-ttu-id="dd5a1-200">宣告式標記會特別有用的而非網站專案本身，內嵌在組件時使用指令碼時，指令碼的名稱可能不會變更為推入修訂。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-200">Declarative markup is particularly useful when working with scripts embedded in assemblies other than the web site project itself, as the name of the script will likely not change as revisions are pushed through.</span></span>

## <a name="summary"></a><span data-ttu-id="dd5a1-201">總結</span><span class="sxs-lookup"><span data-stu-id="dd5a1-201">Summary</span></span>

<span data-ttu-id="dd5a1-202">Web 應用程式成長到大量對象，必須能夠連線到更廣泛的文化特性和社群會變得核心商業模型;電子商務 web 應用程式必須能夠處理外部貨幣，內容管理系統需要其內容，但也他們瀏覽提示和其他語言，與公司中的表單欄位需要知道這項需求是能夠不只存在可存取。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-202">As web applications grow to reach a larger audience, the need to be able to reach broader cultures and communities becomes core to a business model; e-commerce web applications need to be able to deal with foreign currencies, content management systems need to be able to not only present their content but also their navigation hints and form fields in other languages, and companies need to know that this need is accessible.</span></span>

<span data-ttu-id="dd5a1-203">.NET Framework 本質上支援豐富的當地語系化的架構，利用附屬組件和 XML 資源 (.resx) 檔案來提供統一的方式，來查閱資源字串和映像。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-203">The .NET Framework intrinsically supports a rich localization framework, utilizing satellite assemblies and XML resource (.resx) files to present a uniform way to look up resource strings and images.</span></span> <span data-ttu-id="dd5a1-204">ASP.NET AJAX Extensions，包括 Microsoft AJAX 架構和 Microsoft AJAX 指令碼程式庫，提供支援對此程式設計模型為用戶端程式碼，讓您輕鬆的資源字串查閱。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-204">The ASP.NET AJAX Extensions, including the Microsoft AJAX Framework and Microsoft AJAX Script Library, provide support for this programming model into client-side code, enabling easy resource string lookups.</span></span> <span data-ttu-id="dd5a1-205">附屬組件會支援自動包含透過 ScriptResource.axd 的指令碼資源 （實際的.js 檔案），只要檔案名稱依照指定的命名配置。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-205">Satellite assemblies support the automatic inclusion of script resources (actual .js files) through ScriptResource.axd as long as the filenames follow a given naming scheme.</span></span> <span data-ttu-id="dd5a1-206">透過這項支援，ASP.NET AJAX 擴充功能會簡化的指令碼當地語系化和全球化應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-206">With this support, the ASP.NET AJAX Extensions simplify the localization of scripts and the globalization of applications.</span></span>

## <a name="bio"></a><span data-ttu-id="dd5a1-207">*簡歷*</span><span class="sxs-lookup"><span data-stu-id="dd5a1-207">*Bio*</span></span>

<span data-ttu-id="dd5a1-208">Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。</span><span class="sxs-lookup"><span data-stu-id="dd5a1-208">Scott Cate has been working with Microsoft Web technologies since 1997 and is the President of myKB.com ([www.myKB.com](http://www.myKB.com)) where he specializes in writing ASP.NET based applications focused on Knowledge Base Software solutions.</span></span> <span data-ttu-id="dd5a1-209">可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)</span><span class="sxs-lookup"><span data-stu-id="dd5a1-209">Scott can be contacted via email at [scott.cate@myKB.com](mailto:scott.cate@myKB.com) or his blog at [ScottCate.com](http://ScottCate.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dd5a1-210">[上一頁](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [下一頁](understanding-asp-net-ajax-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="dd5a1-210">[Previous](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
[Next](understanding-asp-net-ajax-web-services.md)</span></span>
