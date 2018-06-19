---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 頁面模型 |Microsoft 文件
author: microsoft
description: 在 ASP.NET 中 1.x，開發人員就必須是內嵌程式碼模型與程式碼後置程式碼模型之間的選擇。 使用任一個 Src attr 實作程式碼後置...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891291"
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="75a5f-104">ASP.NET 2.0 頁面模型</span><span class="sxs-lookup"><span data-stu-id="75a5f-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="75a5f-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="75a5f-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="75a5f-106">在 ASP.NET 中 1.x，開發人員就必須是內嵌程式碼模型與程式碼後置程式碼模型之間的選擇。</span><span class="sxs-lookup"><span data-stu-id="75a5f-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="75a5f-107">無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。</span><span class="sxs-lookup"><span data-stu-id="75a5f-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="75a5f-108">在 ASP.NET 2.0 中，開發人員仍然可以內嵌程式碼和程式碼後置之間選擇，但是已有程式碼後置模型的重大增強功能。</span><span class="sxs-lookup"><span data-stu-id="75a5f-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="75a5f-109">在 ASP.NET 中 1.x，開發人員就必須是內嵌程式碼模型與程式碼後置程式碼模型之間的選擇。</span><span class="sxs-lookup"><span data-stu-id="75a5f-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="75a5f-110">無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。</span><span class="sxs-lookup"><span data-stu-id="75a5f-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="75a5f-111">在 ASP.NET 2.0 中，開發人員仍然可以內嵌程式碼和程式碼後置之間選擇，但是已有程式碼後置模型的重大增強功能。</span><span class="sxs-lookup"><span data-stu-id="75a5f-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="75a5f-112">程式碼後置模型中的增強功能</span><span class="sxs-lookup"><span data-stu-id="75a5f-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="75a5f-113">若要完全了解程式碼後置模型，在 ASP.NET 2.0 中的變更，可快速檢閱模型的原貌最好存在於 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="75a5f-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="75a5f-114">在 ASP.NET 中的程式碼後置模型 1.x</span><span class="sxs-lookup"><span data-stu-id="75a5f-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="75a5f-115">在 ASP.NET 中 1.x，程式碼後置模型組成 ASPX 檔案 (Webform) 包含在程式碼的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="75a5f-116">兩個檔案連接使用@PageASPX 檔指示詞。</span><span class="sxs-lookup"><span data-stu-id="75a5f-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="75a5f-117">每個 ASPX 頁面上的控制項的程式碼後置檔案中具有對應的宣告，做為執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="75a5f-118">程式碼後置檔案也包含事件繫結的程式碼，而且產生的程式碼所需的 Visual Studio 設計工具。</span><span class="sxs-lookup"><span data-stu-id="75a5f-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="75a5f-119">此模型中的工作很不錯，但因為每個 ASPX 頁面中的 ASP.NET 項目所需的程式碼後置檔案中對應的程式碼，沒有，則為 true 的程式碼和分隔內容。</span><span class="sxs-lookup"><span data-stu-id="75a5f-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="75a5f-120">例如，如果設計工具會將新的伺服器控制項加入 Visual Studio IDE 外部 ASPX 檔，應用程式會破壞因為沒有宣告該控制項的程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="75a5f-121">程式碼後置模型，在 ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="75a5f-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="75a5f-122">在此模型時可大幅提升 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="75a5f-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="75a5f-123">在 ASP.NET 2.0 中，程式碼後置會實作使用新*部分類別*ASP.NET 2.0 中提供。</span><span class="sxs-lookup"><span data-stu-id="75a5f-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="75a5f-124">在 ASP.NET 2.0 中的程式碼後置類別是定義為部分類別，表示它包含的類別定義的組件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="75a5f-125">類別定義的其餘部分是動態產生的 ASP.NET 2.0 使用 ASPX 頁面，在執行階段，或在先行編譯網站。</span><span class="sxs-lookup"><span data-stu-id="75a5f-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="75a5f-126">程式碼後置檔案與 ASPX 頁面之間的連結仍是使用 @ Page 指示詞已建立。</span><span class="sxs-lookup"><span data-stu-id="75a5f-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="75a5f-127">不過，而不是程式碼後置或 Src 屬性，ASP.NET 2.0 現在會使用 CodeFile 屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="75a5f-128">Inherits 屬性也用於指定網頁的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="75a5f-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="75a5f-129">典型的 @ Page 指示詞可能看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="75a5f-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="75a5f-130">ASP.NET 2.0 程式碼後置檔案中的一般類別定義看起來可能像這樣：</span><span class="sxs-lookup"><span data-stu-id="75a5f-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="75a5f-131">C# 和 Visual Basic 是只有目前支援的部分類別的 managed 的語言。</span><span class="sxs-lookup"><span data-stu-id="75a5f-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="75a5f-132">因此，使用 J# 的開發人員將無法使用 ASP.NET 2.0 中的程式碼後置模型。</span><span class="sxs-lookup"><span data-stu-id="75a5f-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="75a5f-133">新的模型增強程式碼後置模型，因為開發人員現在會有包含他們所建立的程式碼的程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="75a5f-134">它也會提供 true 隔離的程式碼和內容，因為沒有任何執行個體的變數宣告中的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="75a5f-135">ASPX 頁面的部分類別是事件繫結發生的位置，因為 Visual Basic 開發人員可以利用控點關鍵字的程式碼後置中，繫結事件實現輕微的效能增加。</span><span class="sxs-lookup"><span data-stu-id="75a5f-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="75a5f-136">C# 擁有沒有對等的關鍵字。</span><span class="sxs-lookup"><span data-stu-id="75a5f-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="75a5f-137">新的 @ Page 指示詞屬性</span><span class="sxs-lookup"><span data-stu-id="75a5f-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="75a5f-138">ASP.NET 2.0 到 @ Page 指示詞加入許多新的屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="75a5f-139">下列屬性是在 ASP.NET 2.0 的新功能。</span><span class="sxs-lookup"><span data-stu-id="75a5f-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="75a5f-140">Async</span><span class="sxs-lookup"><span data-stu-id="75a5f-140">Async</span></span>

<span data-ttu-id="75a5f-141">Async 屬性可讓您設定要以非同步方式執行的頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="75a5f-142">也涵蓋稍後在此模組中的非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="75a5f-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="75a5f-143">AsyncTimeout</span></span>

<span data-ttu-id="75a5f-144">指定非同步頁面的逾時。</span><span class="sxs-lookup"><span data-stu-id="75a5f-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="75a5f-145">預設值為 45 秒。</span><span class="sxs-lookup"><span data-stu-id="75a5f-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="75a5f-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="75a5f-146">CodeFile</span></span>

<span data-ttu-id="75a5f-147">CodeFile 屬性會取代程式碼後置屬性在 Visual Studio 2002/2003年。</span><span class="sxs-lookup"><span data-stu-id="75a5f-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="75a5f-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="75a5f-148">CodeFileBaseClass</span></span>

<span data-ttu-id="75a5f-149">在您想要從單一基底類別衍生的多個頁面的情況下使用 CodeFileBaseClass 屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="75a5f-150">由於在 ASP.NET 中，若沒有這個屬性的部分類別的實作會使用共用通用的欄位來參考 ASPX 頁面中宣告控制項的基底類別會無法正確運作，因為 ASP。通道編譯引擎會自動建立新成員 頁面中的控制項為基礎。</span><span class="sxs-lookup"><span data-stu-id="75a5f-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="75a5f-151">因此，如果您想要在 ASP.NET 中的兩個或多個頁面通用基底類別，您必須定義 CodeFileBaseClass 屬性中指定基底類別，然後再衍生自該基底類別的每個頁面的類別。</span><span class="sxs-lookup"><span data-stu-id="75a5f-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="75a5f-152">這個屬性使用時，也需要 CodeFile 屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="75a5f-153">CompilationMode</span><span class="sxs-lookup"><span data-stu-id="75a5f-153">CompilationMode</span></span>

<span data-ttu-id="75a5f-154">這個屬性可讓您設定 ASPX 頁面的 CompilationMode 屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="75a5f-155">CompilationMode 屬性是包含值的列舉**永遠**，**自動**，和**永不**。</span><span class="sxs-lookup"><span data-stu-id="75a5f-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="75a5f-156">預設值是**永遠**。</span><span class="sxs-lookup"><span data-stu-id="75a5f-156">The default is **Always**.</span></span> <span data-ttu-id="75a5f-157">**自動**設定會防止 ASP.NET 動態盡可能編譯網頁。</span><span class="sxs-lookup"><span data-stu-id="75a5f-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="75a5f-158">排除動態編譯的頁面，可增加效能。</span><span class="sxs-lookup"><span data-stu-id="75a5f-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="75a5f-159">不過，如果已排除的頁面包含必須編譯該程式碼，將會擲回錯誤瀏覽頁面時。</span><span class="sxs-lookup"><span data-stu-id="75a5f-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="75a5f-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="75a5f-160">EnableEventValidation</span></span>

<span data-ttu-id="75a5f-161">這個屬性會指定驗證回傳和回呼事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="75a5f-162">這啟用時，引數回傳或回呼事件會檢查以確保它們來自原本呈現它們的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="75a5f-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="75a5f-163">EnableTheming</span></span>

<span data-ttu-id="75a5f-164">這個屬性會指定在頁面上使用 ASP.NET 佈景主題。</span><span class="sxs-lookup"><span data-stu-id="75a5f-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="75a5f-165">預設值為 **false**。</span><span class="sxs-lookup"><span data-stu-id="75a5f-165">The default is **false**.</span></span> <span data-ttu-id="75a5f-166">ASP.NET 主題涵蓋[單元 10](profiles-themes-and-web-parts.md)。</span><span class="sxs-lookup"><span data-stu-id="75a5f-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="75a5f-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="75a5f-167">LinePragmas</span></span>

<span data-ttu-id="75a5f-168">這個屬性會指定是否應在編譯期間新增列 pragma。</span><span class="sxs-lookup"><span data-stu-id="75a5f-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="75a5f-169">行是由偵錯工具用來標記程式碼的特定區段的選項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="75a5f-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="75a5f-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="75a5f-171">這個屬性會指定 JavaScript 插入頁面以維護回傳之間的捲動位置。</span><span class="sxs-lookup"><span data-stu-id="75a5f-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="75a5f-172">這個屬性是**false**預設。</span><span class="sxs-lookup"><span data-stu-id="75a5f-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="75a5f-173">當這個屬性是**true**，ASP.NET 會加入&lt;指令碼&gt;區塊在回傳時，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="75a5f-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="75a5f-174">請注意，此指令碼區塊的 src WebResource.axd。</span><span class="sxs-lookup"><span data-stu-id="75a5f-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="75a5f-175">此資源不是實體路徑。</span><span class="sxs-lookup"><span data-stu-id="75a5f-175">This resource is not a physical path.</span></span> <span data-ttu-id="75a5f-176">此指令碼要求時，ASP.NET 會動態建立指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="75a5f-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="75a5f-177">MasterPageFile</span></span>

<span data-ttu-id="75a5f-178">這個屬性會指定目前網頁的主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="75a5f-179">路徑可以是相對或絕對。</span><span class="sxs-lookup"><span data-stu-id="75a5f-179">The path can be relative or absolute.</span></span> <span data-ttu-id="75a5f-180">主版頁面都涵蓋[模組 4](master-pages.md)。</span><span class="sxs-lookup"><span data-stu-id="75a5f-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="75a5f-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="75a5f-181">StyleSheetTheme</span></span>

<span data-ttu-id="75a5f-182">這個屬性可讓您覆寫 ASP.NET 2.0 佈景主題所定義的使用者介面外觀屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="75a5f-183">佈景主題會說明[單元 10](profiles-themes-and-web-parts.md)。</span><span class="sxs-lookup"><span data-stu-id="75a5f-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="75a5f-184">主題</span><span class="sxs-lookup"><span data-stu-id="75a5f-184">Theme</span></span>

<span data-ttu-id="75a5f-185">指定網頁佈景主題。</span><span class="sxs-lookup"><span data-stu-id="75a5f-185">Specifies the theme for the page.</span></span> <span data-ttu-id="75a5f-186">如果未指定 StyleSheetTheme 屬性的值，Theme 屬性會覆寫所有樣式套用至頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="75a5f-187">標題</span><span class="sxs-lookup"><span data-stu-id="75a5f-187">Title</span></span>

<span data-ttu-id="75a5f-188">設定頁面的標題。</span><span class="sxs-lookup"><span data-stu-id="75a5f-188">Sets the title for the page.</span></span> <span data-ttu-id="75a5f-189">此處指定的值將會出現在&lt;標題&gt;所呈現頁面的項目。</span><span class="sxs-lookup"><span data-stu-id="75a5f-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="75a5f-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="75a5f-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="75a5f-191">設定 ViewStateEncryptionMode 列舉的值。</span><span class="sxs-lookup"><span data-stu-id="75a5f-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="75a5f-192">可用的值為**永遠**，**自動**，和**永不**。</span><span class="sxs-lookup"><span data-stu-id="75a5f-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="75a5f-193">預設值是**自動**。當這個屬性設定的值為**自動**，加密 viewstate 是控制項要求藉由呼叫**RegisterRequiresViewStateEncryption**方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="75a5f-194">設定公用屬性值透過 @ Page 指示詞</span><span class="sxs-lookup"><span data-stu-id="75a5f-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="75a5f-195">@ Page 指示詞，在 ASP.NET 2.0 中的另一個新功能是能夠設定基底類別的公用屬性的初始值。</span><span class="sxs-lookup"><span data-stu-id="75a5f-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="75a5f-196">假設您擁有的公用屬性，例如呼叫**SomeText**在基底類別和 d 與它類似您初始化為**Hello**網頁載入時。</span><span class="sxs-lookup"><span data-stu-id="75a5f-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="75a5f-197">您可以完成這項作業只需要設定 @ Page 指示詞中的值如下所示：</span><span class="sxs-lookup"><span data-stu-id="75a5f-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="75a5f-198">**SomeText** @ Page 指示詞屬性的基底類別中設定 SomeText 屬性的初始值*Hello ！*。</span><span class="sxs-lookup"><span data-stu-id="75a5f-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="75a5f-199">以下影片是在基底類別使用 @ Page 指示詞中設定的公用屬性的初始值的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="75a5f-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="75a5f-200">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="75a5f-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="75a5f-201">頁面類別的新公用屬性</span><span class="sxs-lookup"><span data-stu-id="75a5f-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="75a5f-202">下列公用屬性是在 ASP.NET 2.0 的新功能。</span><span class="sxs-lookup"><span data-stu-id="75a5f-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="75a5f-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="75a5f-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="75a5f-204">回到網頁或控制項的應用程式的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="75a5f-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="75a5f-205">例如，針對位於頁面http://app/folder/page.aspx，屬性會傳回 ~ / 資料夾 /。</span><span class="sxs-lookup"><span data-stu-id="75a5f-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="75a5f-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="75a5f-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="75a5f-207">回到網頁或控制項的相對虛擬目錄路徑。</span><span class="sxs-lookup"><span data-stu-id="75a5f-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="75a5f-208">例如對於頁面位於http://app/folder/page.aspx，屬性會傳回 ~ / folder/page.aspx。</span><span class="sxs-lookup"><span data-stu-id="75a5f-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="75a5f-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="75a5f-209">AsyncTimeout</span></span>

<span data-ttu-id="75a5f-210">取得或設定用來非同步頁面處理的逾時。</span><span class="sxs-lookup"><span data-stu-id="75a5f-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="75a5f-211">（本單元稍後將討論非同步頁面）。</span><span class="sxs-lookup"><span data-stu-id="75a5f-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="75a5f-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="75a5f-212">ClientQueryString</span></span>

<span data-ttu-id="75a5f-213">唯讀屬性，傳回要求的 URL 查詢字串部分。</span><span class="sxs-lookup"><span data-stu-id="75a5f-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="75a5f-214">這個值會是 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-214">This value is URL encoded.</span></span> <span data-ttu-id="75a5f-215">您可以使用 UrlDecode HttpServerUtility 類別方法，以將它解碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="75a5f-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="75a5f-216">ClientScript</span></span>

<span data-ttu-id="75a5f-217">這個屬性會傳回可以用來管理的用戶端指令碼的 ASP.NETs 發出 ClientScriptManager 物件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="75a5f-218">（ClientScriptManager 類別涵蓋此模組中的更新版本）。</span><span class="sxs-lookup"><span data-stu-id="75a5f-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="75a5f-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="75a5f-219">EnableEventValidation</span></span>

<span data-ttu-id="75a5f-220">這個屬性控制啟用事件驗證回傳和回呼事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="75a5f-221">啟用時，會驗證引數回傳或回呼事件以確保它們來自原本呈現它們的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="75a5f-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="75a5f-222">EnableTheming</span></span>

<span data-ttu-id="75a5f-223">這個屬性會取得或設定布林值，指定 ASP.NET 2.0 佈景主題套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="75a5f-224">表單</span><span class="sxs-lookup"><span data-stu-id="75a5f-224">Form</span></span>

<span data-ttu-id="75a5f-225">這個屬性會傳回為 HtmlForm 物件 ASPX 頁面上的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="75a5f-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="75a5f-226">頁首</span><span class="sxs-lookup"><span data-stu-id="75a5f-226">Header</span></span>

<span data-ttu-id="75a5f-227">這個屬性會傳回包含頁首的 HtmlHead 物件的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="75a5f-228">取得或設定樣式表、 Meta 標記和其他內容，您可以使用傳回的 HtmlHead 物件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="75a5f-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="75a5f-229">IdSeparator</span></span>

<span data-ttu-id="75a5f-230">這個唯讀屬性會取得用來分隔控制識別項，當 ASP.NET 建置網頁上控制項的唯一識別碼的字元。</span><span class="sxs-lookup"><span data-stu-id="75a5f-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="75a5f-231">但並不是針對直接從程式碼使用而設計。</span><span class="sxs-lookup"><span data-stu-id="75a5f-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="75a5f-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="75a5f-232">IsAsync</span></span>

<span data-ttu-id="75a5f-233">這個屬性允許非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="75a5f-234">此模組稍後會討論非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="75a5f-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="75a5f-235">IsCallback</span></span>

<span data-ttu-id="75a5f-236">這個唯讀屬性會傳回**true**頁面是否為回呼的結果。</span><span class="sxs-lookup"><span data-stu-id="75a5f-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="75a5f-237">此模組稍後會討論程式回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="75a5f-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="75a5f-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="75a5f-239">這個唯讀屬性會傳回**true**頁面是否跨網頁回傳的一部分。</span><span class="sxs-lookup"><span data-stu-id="75a5f-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="75a5f-240">此模組稍後會說明跨網頁回傳。</span><span class="sxs-lookup"><span data-stu-id="75a5f-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="75a5f-241">項目</span><span class="sxs-lookup"><span data-stu-id="75a5f-241">Items</span></span>

<span data-ttu-id="75a5f-242">傳回包含頁面內容中所儲存的所有物件的 IDictionary 執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="75a5f-243">您可以將項目加入這個 IDictionary 物件，並將它們提供給您的整個內容的存留期。</span><span class="sxs-lookup"><span data-stu-id="75a5f-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="75a5f-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="75a5f-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="75a5f-245">這個屬性會控制 ASP.NET 發出維護頁面捲動瀏覽器中的位置，就會發生回傳之後的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="75a5f-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="75a5f-246">（稍早在此模組中所討論的這個屬性的詳細資料）。</span><span class="sxs-lookup"><span data-stu-id="75a5f-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="75a5f-247">主要</span><span class="sxs-lookup"><span data-stu-id="75a5f-247">Master</span></span>

<span data-ttu-id="75a5f-248">這個唯讀屬性傳回已套用主版頁面的頁面的 MasterPage 執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="75a5f-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="75a5f-249">MasterPageFile</span></span>

<span data-ttu-id="75a5f-250">取得或設定頁面的主版頁面檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="75a5f-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="75a5f-251">這個屬性只可以設定 PreInit 方法中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="75a5f-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="75a5f-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="75a5f-253">這個屬性會取得或設定頁面狀態的最大長度 （位元組）。</span><span class="sxs-lookup"><span data-stu-id="75a5f-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="75a5f-254">如果屬性設定為正數，頁面檢視狀態會分成多個隱藏的欄位，讓它不會超過指定的位元組數目。</span><span class="sxs-lookup"><span data-stu-id="75a5f-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="75a5f-255">如果屬性是負數，檢視狀態將不會中斷為區塊。</span><span class="sxs-lookup"><span data-stu-id="75a5f-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="75a5f-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="75a5f-256">PageAdapter</span></span>

<span data-ttu-id="75a5f-257">傳回修改的頁面要求瀏覽器 PageAdapter 物件的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="75a5f-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="75a5f-258">PreviousPage</span></span>

<span data-ttu-id="75a5f-259">在 Server.Transfer 或跨網頁回傳的情況下會傳回前一個頁面的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="75a5f-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="75a5f-260">SkinID</span></span>

<span data-ttu-id="75a5f-261">指定要套用到頁面的 ASP.NET 2.0 面板。</span><span class="sxs-lookup"><span data-stu-id="75a5f-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="75a5f-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="75a5f-262">StyleSheetTheme</span></span>

<span data-ttu-id="75a5f-263">這個屬性會取得或設定套用至頁面的樣式表。</span><span class="sxs-lookup"><span data-stu-id="75a5f-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="75a5f-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="75a5f-264">TemplateControl</span></span>

<span data-ttu-id="75a5f-265">傳回包含頁面控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="75a5f-266">主題</span><span class="sxs-lookup"><span data-stu-id="75a5f-266">Theme</span></span>

<span data-ttu-id="75a5f-267">取得或設定套用至網頁之 ASP.NET 2.0 佈景主題的名稱。</span><span class="sxs-lookup"><span data-stu-id="75a5f-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="75a5f-268">PreInit 方法之前，必須設定此值。</span><span class="sxs-lookup"><span data-stu-id="75a5f-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="75a5f-269">標題</span><span class="sxs-lookup"><span data-stu-id="75a5f-269">Title</span></span>

<span data-ttu-id="75a5f-270">這個屬性會取得或設定頁面的標題，取得頁面標頭。</span><span class="sxs-lookup"><span data-stu-id="75a5f-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="75a5f-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="75a5f-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="75a5f-272">取得或設定頁面的 ViewStateEncryptionMode。</span><span class="sxs-lookup"><span data-stu-id="75a5f-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="75a5f-273">請參閱稍早在此模組中的這個屬性的詳細的討論。</span><span class="sxs-lookup"><span data-stu-id="75a5f-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="75a5f-274">網頁類別的新受保護的內容</span><span class="sxs-lookup"><span data-stu-id="75a5f-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="75a5f-275">以下是在 ASP.NET 2.0 頁面類別的新受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="75a5f-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="75a5f-276">配接器</span><span class="sxs-lookup"><span data-stu-id="75a5f-276">Adapter</span></span>

<span data-ttu-id="75a5f-277">傳回參考轉譯的裝置上的頁面 ControlAdapter 要求它。</span><span class="sxs-lookup"><span data-stu-id="75a5f-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="75a5f-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="75a5f-278">AsyncMode</span></span>

<span data-ttu-id="75a5f-279">這個屬性會指出要以非同步方式處理的頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="75a5f-280">它適用於執行階段，而不是直接在程式碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="75a5f-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="75a5f-281">ClientIDSeparator</span></span>

<span data-ttu-id="75a5f-282">這個屬性會傳回為控制項建立唯一的用戶端識別碼時，做為分隔符號的字元。</span><span class="sxs-lookup"><span data-stu-id="75a5f-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="75a5f-283">它適用於執行階段，而不是直接在程式碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="75a5f-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="75a5f-284">PageStatePersister</span></span>

<span data-ttu-id="75a5f-285">這個屬性會傳回頁面 PageStatePersister 物件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="75a5f-286">這個屬性主要是由 ASP.NET 控制項開發人員使用。</span><span class="sxs-lookup"><span data-stu-id="75a5f-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="75a5f-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="75a5f-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="75a5f-288">這個屬性會傳回唯一的 suffic 附加至瀏覽器快取的檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="75a5f-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="75a5f-289">預設值是\_ \_ufps = 和 6 位數的數字。</span><span class="sxs-lookup"><span data-stu-id="75a5f-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="75a5f-290">新的頁面類別的公用方法</span><span class="sxs-lookup"><span data-stu-id="75a5f-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="75a5f-291">下列的公用方法不熟悉 ASP.NET 2.0 中的頁面類別。</span><span class="sxs-lookup"><span data-stu-id="75a5f-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="75a5f-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="75a5f-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="75a5f-293">這個方法會註冊事件處理常式委派的非同步頁面執行。</span><span class="sxs-lookup"><span data-stu-id="75a5f-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="75a5f-294">此模組稍後會討論非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="75a5f-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="75a5f-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="75a5f-296">頁面樣式表中的屬性套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="75a5f-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="75a5f-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="75a5f-298">此方法操作的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="75a5f-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="75a5f-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="75a5f-299">GetValidators</span></span>

<span data-ttu-id="75a5f-300">如果未指定，則傳回驗證程式指定的驗證群組或預設的驗證群組的集合。</span><span class="sxs-lookup"><span data-stu-id="75a5f-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="75a5f-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="75a5f-301">RegisterAsyncTask</span></span>

<span data-ttu-id="75a5f-302">這個方法會註冊新的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="75a5f-302">This method registers a new async task.</span></span> <span data-ttu-id="75a5f-303">此模組稍後會說明非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="75a5f-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="75a5f-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="75a5f-305">這個方法會告知 ASP.NET 網頁控制項狀態必須 persisted。</span><span class="sxs-lookup"><span data-stu-id="75a5f-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="75a5f-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="75a5f-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="75a5f-307">這個方法會告知 ASP.NET 網頁 viewstate 需要加密。</span><span class="sxs-lookup"><span data-stu-id="75a5f-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="75a5f-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="75a5f-308">ResolveClientUrl</span></span>

<span data-ttu-id="75a5f-309">傳回可用於等映像的用戶端要求的相對 URL。</span><span class="sxs-lookup"><span data-stu-id="75a5f-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="75a5f-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="75a5f-310">SetFocus</span></span>

<span data-ttu-id="75a5f-311">這個方法會將焦點設定一開始載入的頁面時指定的控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="75a5f-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="75a5f-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="75a5f-313">這個方法會將取消登錄不再需要控制狀態持續性傳遞給它的控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="75a5f-314">變更頁面生命週期</span><span class="sxs-lookup"><span data-stu-id="75a5f-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="75a5f-315">網頁生命週期，在 ASP.NET 2.0 中未變更急遽增加，但您應該注意的一些新方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="75a5f-316">ASP.NET 2.0 頁面生命週期如下所述。</span><span class="sxs-lookup"><span data-stu-id="75a5f-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="75a5f-317">PreInit （新的 ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="75a5f-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="75a5f-318">PreInit 事件是開發人員可以存取的生命週期中的最早階段。</span><span class="sxs-lookup"><span data-stu-id="75a5f-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="75a5f-319">此事件加入可讓您能夠以程式設計方式變更主版頁面、 ASP.NET 2.0 佈景主題，請存取 ASP.NET 2.0 設定檔等屬性。如果您是在回傳狀態，其一定要了解，Viewstate 具有尚未套用至這個時候生命週期中的控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="75a5f-320">因此，如果開發人員變更控制項的屬性在這個階段，它將可能覆寫網頁生命週期的更新版本中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="75a5f-321">Init</span><span class="sxs-lookup"><span data-stu-id="75a5f-321">Init</span></span>

<span data-ttu-id="75a5f-322">在 Init 事件未變更 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="75a5f-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="75a5f-323">這是您想要讀取或初始化您的頁面上的控制項屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="75a5f-324">在此階段、 主版頁面、 佈景主題等已套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="75a5f-325">InitComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="75a5f-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="75a5f-326">在頁面初始化階段結尾稱為 InitComplete 事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="75a5f-327">此時生命週期中，您可以存取控制項在頁面上，但尚未擴展其狀態。</span><span class="sxs-lookup"><span data-stu-id="75a5f-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="75a5f-328">預先載入 （2.0 的新功能）</span><span class="sxs-lookup"><span data-stu-id="75a5f-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="75a5f-329">此事件會呼叫在套用所有的回傳資料之後，和之前頁面\_負載。</span><span class="sxs-lookup"><span data-stu-id="75a5f-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="75a5f-330">Load</span><span class="sxs-lookup"><span data-stu-id="75a5f-330">Load</span></span>

<span data-ttu-id="75a5f-331">載入事件未變更 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="75a5f-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="75a5f-332">LoadComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="75a5f-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="75a5f-333">LoadComplete 事件中的頁面載入階段是最後一個事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="75a5f-334">在這個階段，所有回傳和 viewstate 資料已都套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="75a5f-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="75a5f-335">PreRender</span></span>

<span data-ttu-id="75a5f-336">如果您想要的正確維護的控制項，以動態方式加入至頁面的 viewstate，PreRender 事件就會是將它們加入的最後機會。</span><span class="sxs-lookup"><span data-stu-id="75a5f-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="75a5f-337">PreRenderComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="75a5f-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="75a5f-338">在 PreRenderComplete 階段，所有控制項都已都加入至頁面，頁面已準備好呈現。</span><span class="sxs-lookup"><span data-stu-id="75a5f-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="75a5f-339">PreRenderComplete 事件是在儲存頁面 viewstate 之前引發的最後一個事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="75a5f-340">SaveStateComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="75a5f-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="75a5f-341">已儲存的所有頁面的 viewstate 和控制狀態之後，立即呼叫 SaveStateComplete 事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="75a5f-342">這是前頁面實際上會呈現在瀏覽器的最後一個事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="75a5f-343">轉譯</span><span class="sxs-lookup"><span data-stu-id="75a5f-343">Render</span></span>

<span data-ttu-id="75a5f-344">ASP.NET 之後並未變更轉譯方法 1.x。</span><span class="sxs-lookup"><span data-stu-id="75a5f-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="75a5f-345">這是初始化 HtmlTextWriter 和瀏覽器中呈現的網頁位置。</span><span class="sxs-lookup"><span data-stu-id="75a5f-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="75a5f-346">在 ASP.NET 2.0 中的跨網頁回傳</span><span class="sxs-lookup"><span data-stu-id="75a5f-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="75a5f-347">在 ASP.NET 中 1.x 回傳都需要公佈到相同的頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="75a5f-348">不允許跨網頁回傳。</span><span class="sxs-lookup"><span data-stu-id="75a5f-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="75a5f-349">ASP.NET 2.0 加入回傳至不同的頁面，透過 IButtonControl 介面的功能。</span><span class="sxs-lookup"><span data-stu-id="75a5f-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="75a5f-350">實作新的 IButtonControl 介面 （按鈕、 LinkButton 和 ImageButton 除了協力廠商的自訂控制項） 的任何控制項可以利用這個新的功能，透過使用 PostBackUrl 屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="75a5f-351">下列程式碼會示範回傳至第二個頁面上的按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="75a5f-352">網頁回傳，啟始回傳的頁面時，可透過第二頁上的 [上一頁] 屬性存取。</span><span class="sxs-lookup"><span data-stu-id="75a5f-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="75a5f-353">這項功能透過新 WebForm 實作\_DoPostBackWithOptions 用戶端函式，會呈現 ASP.NET 2.0 頁面，但控制項回傳至不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="75a5f-354">這個 JavaScript 函式會提供新的 WebResource.axd 處理常式，這時用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="75a5f-355">以下影片是跨網頁回傳的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="75a5f-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="75a5f-356">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="75a5f-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="75a5f-357">在跨網頁回傳的更多詳細資料</span><span class="sxs-lookup"><span data-stu-id="75a5f-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="75a5f-358">Viewstate</span><span class="sxs-lookup"><span data-stu-id="75a5f-358">Viewstate</span></span>

<span data-ttu-id="75a5f-359">您可能會要求您自己已經有關會發生什麼事 viewstate 從跨網頁回傳的案例中的第一頁。</span><span class="sxs-lookup"><span data-stu-id="75a5f-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="75a5f-360">所有之後，任何未實作 IPostBackDataHandler 的控制項將會因此保存 viewstate，透過其狀態，具有存取權的跨網頁回傳的第二頁上的控制項屬性，您必須擁有存取頁面的 viewstate。</span><span class="sxs-lookup"><span data-stu-id="75a5f-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="75a5f-361">ASP.NET 2.0 會處理此案例中使用新的隱藏的欄位，在第二個頁面呼叫\_\_上一頁。</span><span class="sxs-lookup"><span data-stu-id="75a5f-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="75a5f-362">\_\_上一頁表單欄位會包含第一個頁面的 viewstate，讓您在第二個頁面都可以存取所有控制項的屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="75a5f-363">規避 FindControl</span><span class="sxs-lookup"><span data-stu-id="75a5f-363">Circumventing FindControl</span></span>

<span data-ttu-id="75a5f-364">在跨網頁回傳的視訊逐步解說中，我可以使用 FindControl 方法來取得第一頁上的文字方塊控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="75a5f-365">該方法適用於該目的，但 FindControl 成本很高，而且它需要撰寫額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="75a5f-366">幸運的是，ASP.NET 2.0 提供此用途，在許多案例中運作 FindControl 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="75a5f-367">PreviousPageType 指示詞可讓您有使用 TypeName 或 VirtualPath 屬性前一頁的強型別參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="75a5f-368">TypeName 屬性可讓您指定先前的頁面類型而 VirtualPath 屬性可讓您參考至上一頁使用的虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="75a5f-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="75a5f-369">設定 PreviousPageType 指示詞之後，您必須再公開控制項，您要允許存取使用的公用屬性的等等。</span><span class="sxs-lookup"><span data-stu-id="75a5f-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="75a5f-370">實驗室 1 跨網頁回傳</span><span class="sxs-lookup"><span data-stu-id="75a5f-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="75a5f-371">在此實驗室中，您將建立使用 ASP.NET 2.0 的新的跨網頁回傳功能的應用程式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="75a5f-372">開啟 Visual Studio 2005，並建立新的 ASP.NET 網站。</span><span class="sxs-lookup"><span data-stu-id="75a5f-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="75a5f-373">加入新的 Webform 呼叫 page2.aspx。</span><span class="sxs-lookup"><span data-stu-id="75a5f-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="75a5f-374">在 [設計] 檢視中開啟 Default.aspx，然後加入按鈕控制項和文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="75a5f-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="75a5f-375">按鈕控制項提供給識別碼為**交付按鈕**和文字方塊控制項的 ID **UserName**。</span><span class="sxs-lookup"><span data-stu-id="75a5f-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="75a5f-376">設 page2.aspx PostBackUrl 按鈕的屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="75a5f-377">開啟 page2.aspx 來源檢視中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="75a5f-378">加入 @ PreviousPageType 指示詞，如下所示：</span><span class="sxs-lookup"><span data-stu-id="75a5f-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="75a5f-379">下列程式碼加入至頁面\_負載 page2.aspx 的程式碼後置：</span><span class="sxs-lookup"><span data-stu-id="75a5f-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="75a5f-380">按一下 [建置] 功能表建置建置專案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="75a5f-381">將下列程式碼加入至程式碼後置 Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="75a5f-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="75a5f-382">變更頁面\_負載下 page2.aspx 中：</span><span class="sxs-lookup"><span data-stu-id="75a5f-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="75a5f-383">建置專案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-383">Build the project.</span></span>
11. <span data-ttu-id="75a5f-384">執行專案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-384">Run the project.</span></span>
12. <span data-ttu-id="75a5f-385">在文字方塊中輸入您的名稱，然後按一下按鈕。</span><span class="sxs-lookup"><span data-stu-id="75a5f-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="75a5f-386">什麼是結果？</span><span class="sxs-lookup"><span data-stu-id="75a5f-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="75a5f-387">在 ASP.NET 2.0 中的非同步頁面</span><span class="sxs-lookup"><span data-stu-id="75a5f-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="75a5f-388">在 ASP.NET 中的許多競爭問題 （例如 Web 服務或資料庫呼叫） 的外部呼叫、 檔案 IO 延遲等等的延遲所造成。對 ASP.NET 應用程式進行要求時，ASP.NET 會使用其中一個背景工作執行緒服務該要求。</span><span class="sxs-lookup"><span data-stu-id="75a5f-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="75a5f-389">該要求擁有該執行緒，直到完成要求且在傳送回應。</span><span class="sxs-lookup"><span data-stu-id="75a5f-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="75a5f-390">ASP.NET 2.0 會設法加入以非同步方式執行 「 網頁 」 功能來解決這類問題的延遲問題。</span><span class="sxs-lookup"><span data-stu-id="75a5f-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="75a5f-391">這表示工作者執行緒可以啟動要求，並再交給其他執行的其他的執行緒，進而快速地返回可用的執行緒集區。</span><span class="sxs-lookup"><span data-stu-id="75a5f-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="75a5f-392">檔案 IO、 資料庫呼叫等完成之後，新的執行緒被取自完成要求的執行緒集區中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="75a5f-393">讓頁面以非同步方式執行的第一個步驟是設定**非同步**頁面指示詞屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="75a5f-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="75a5f-394">這個屬性會告知 ASP.NET 來實作頁面 IHttpAsyncHandler。</span><span class="sxs-lookup"><span data-stu-id="75a5f-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="75a5f-395">下一個步驟是呼叫 AddOnPreRenderCompleteAsync 方法在 PreRender 之前頁面生命週期中的點。</span><span class="sxs-lookup"><span data-stu-id="75a5f-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="75a5f-396">(在頁面中通常會呼叫這個方法\_負載。)AddOnPreRenderCompleteAsync 方法接受兩個參數。BeginEventHandler 和 EndEventHandler。</span><span class="sxs-lookup"><span data-stu-id="75a5f-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="75a5f-397">BeginEventHandler 傳回 IAsyncResult 再傳遞給 EndEventHandler 做為參數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="75a5f-398">下列視訊是非同步頁面要求的逐步解說。</span><span class="sxs-lookup"><span data-stu-id="75a5f-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="75a5f-399">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="75a5f-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="75a5f-400">非同步頁面不會轉譯至瀏覽器 EndEventHandler 完成為止。</span><span class="sxs-lookup"><span data-stu-id="75a5f-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="75a5f-401">任何不確定，但有些開發人員會將視為類似非同步回呼的非同步要求。</span><span class="sxs-lookup"><span data-stu-id="75a5f-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="75a5f-402">請務必了解它們不是。</span><span class="sxs-lookup"><span data-stu-id="75a5f-402">It's important to realize that they are not.</span></span> <span data-ttu-id="75a5f-403">非同步要求的好處是可傳回第一個背景工作執行緒的執行緒集區，才能服務新要求，藉此減少由於繫結的 IO 的爭用等等。</span><span class="sxs-lookup"><span data-stu-id="75a5f-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="75a5f-404">ASP.NET 2.0 中的指令碼回呼</span><span class="sxs-lookup"><span data-stu-id="75a5f-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="75a5f-405">Web 開發人員一律尋找的方法可防止閃爍的關聯回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="75a5f-406">在 ASP.NET 中 1.x SmartNavigation 避免閃爍，最常見的方法卻 SmartNavigation 某些開發人員造成問題，因為其在用戶端實作的複雜性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="75a5f-407">ASP.NET 2.0 解決這個問題的指令碼的回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="75a5f-408">回呼指令碼會利用 XMLHttp 針對透過 JavaScript 的 Web 伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="75a5f-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="75a5f-409">XMLHttp 要求會傳回 XML 資料，然後可操作透過瀏覽器的 dom。</span><span class="sxs-lookup"><span data-stu-id="75a5f-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="75a5f-410">XMLHttp 程式碼會對使用者隱藏的新 WebResource.axd 處理常式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="75a5f-411">有幾個步驟所需，若要設定 ASP.NET 2.0 中的指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="75a5f-412">步驟 1： 實作 ICallbackEventHandler 介面</span><span class="sxs-lookup"><span data-stu-id="75a5f-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="75a5f-413">為了讓 ASP.NET 才能辨識您的網頁做為參與指令碼回呼，您必須實作 ICallbackEventHandler 介面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="75a5f-414">您可以在程式碼後置檔案進行就像這樣：</span><span class="sxs-lookup"><span data-stu-id="75a5f-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="75a5f-415">您同樣可以這樣因此使用 @ 實作指示詞類似：</span><span class="sxs-lookup"><span data-stu-id="75a5f-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="75a5f-416">使用內嵌的 ASP.NET 程式碼時，您通常會使用 @ 實作指示詞。</span><span class="sxs-lookup"><span data-stu-id="75a5f-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="75a5f-417">步驟 2： 呼叫 GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="75a5f-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="75a5f-418">如先前所述，XMLHttp 呼叫會封裝在 WebResource.axd 處理常式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="75a5f-419">ASP.NET 頁面轉譯時，會將呼叫加入 WebForm\_DoCallback，係由 WebResource.axd 用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="75a5f-420">WebForm\_DoCallback 函式取代\_ \_doPostBack 函式的回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="75a5f-421">請記住， \_ \_doPostBack 以程式設計的方式送出頁面上的表單。</span><span class="sxs-lookup"><span data-stu-id="75a5f-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="75a5f-422">在回呼案例中，您想要防止回傳，所以\_ \_doPostBack 會不敷使用。</span><span class="sxs-lookup"><span data-stu-id="75a5f-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="75a5f-423">\_\_doPostBack 仍然會轉譯到用戶端指令碼回呼案例中的頁面。</span><span class="sxs-lookup"><span data-stu-id="75a5f-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="75a5f-424">不過，它不用於回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="75a5f-425">引數 WebForm\_DoCallback 用戶端函式可以透過伺服器端函式通常會在頁面中稱為 GetCallbackEventReference\_負載。</span><span class="sxs-lookup"><span data-stu-id="75a5f-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="75a5f-426">一般 GetCallbackEventReference 呼叫可能會看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="75a5f-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="75a5f-427">在此情況下，cm 是 ClientScriptManager 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="75a5f-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="75a5f-428">稍後在此模組將涵蓋 ClientScriptManager 類別。</span><span class="sxs-lookup"><span data-stu-id="75a5f-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="75a5f-429">有數個多載的 GetCallbackEventReference 版本。</span><span class="sxs-lookup"><span data-stu-id="75a5f-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="75a5f-430">在此情況下，引數是的如下所示：</span><span class="sxs-lookup"><span data-stu-id="75a5f-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="75a5f-431">呼叫 GetCallbackEventReference 控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="75a5f-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="75a5f-432">在此情況下，它是網頁本身。</span><span class="sxs-lookup"><span data-stu-id="75a5f-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="75a5f-433">從用戶端程式碼時傳遞給伺服器端事件字串引數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="75a5f-434">在此情況下，傳遞的值的下拉式清單中的 Im 呼叫 ddlCompany。</span><span class="sxs-lookup"><span data-stu-id="75a5f-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="75a5f-435">用戶端函式接受的傳回值 （如字串） 從伺服器端的回呼事件的名稱。</span><span class="sxs-lookup"><span data-stu-id="75a5f-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="75a5f-436">伺服器端回呼成功時，只會呼叫此函式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="75a5f-437">因此，為了提高健全度，通常建議使用 GetCallbackEventReference 其他字串引數指定的名稱發生錯誤時所執行的用戶端函式多載的版本。</span><span class="sxs-lookup"><span data-stu-id="75a5f-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="75a5f-438">字串，表示系統已起始伺服器在回呼之前的用戶端函式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="75a5f-439">在此情況下，沒有任何這類指令碼，因此引數為 null。</span><span class="sxs-lookup"><span data-stu-id="75a5f-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="75a5f-440">布林值，指定要以非同步方式進行回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="75a5f-441">呼叫 WebForm\_DoCallback 用戶端上的會將這些引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="75a5f-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="75a5f-442">因此，當用戶端上呈現此頁面時，該程式碼看起來就像這樣：</span><span class="sxs-lookup"><span data-stu-id="75a5f-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="75a5f-443">請注意，用戶端上的函式的簽章有些許不同。</span><span class="sxs-lookup"><span data-stu-id="75a5f-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="75a5f-444">用戶端函式會在 5 個字串、 布林值傳遞。</span><span class="sxs-lookup"><span data-stu-id="75a5f-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="75a5f-445">（這是 null，在上述範例中） 的其他字串包含用戶端函式會處理任何錯誤，伺服器端的回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="75a5f-446">步驟 3： 攔截 (Hook) 用戶端控制項事件</span><span class="sxs-lookup"><span data-stu-id="75a5f-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="75a5f-447">請注意，上述 GetCallbackEventReference 的傳回值已指派給字串變數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="75a5f-448">該字串用來連結控制項初始化回呼用戶端事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="75a5f-449">在此範例中，回呼由起始下拉式清單中的頁面上，所以我想要攔截 (hook) *OnChange*事件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="75a5f-450">若要攔截 (hook) 用戶端事件，只要加入處理常式至用戶端標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="75a5f-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="75a5f-451">請記得， *cbRef*為 GetCallbackEventReference，呼叫的傳回值。</span><span class="sxs-lookup"><span data-stu-id="75a5f-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="75a5f-452">包含呼叫 WebForm\_已如上所示的 DoCallback。</span><span class="sxs-lookup"><span data-stu-id="75a5f-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="75a5f-453">步驟 4： 註冊用戶端指令碼</span><span class="sxs-lookup"><span data-stu-id="75a5f-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="75a5f-454">提醒您，GetCallbackEventReference 呼叫指定的用戶端指令碼呼叫**ShowCompanyName**伺服器端回呼成功時，就會執行。</span><span class="sxs-lookup"><span data-stu-id="75a5f-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="75a5f-455">該指令碼必須加入至頁面使用 ClientScriptManager 執行個體。</span><span class="sxs-lookup"><span data-stu-id="75a5f-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="75a5f-456">（ClientScriptManager 類別將稍後在本單元這。）因此，您執行該類似：</span><span class="sxs-lookup"><span data-stu-id="75a5f-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="75a5f-457">步驟 5： 呼叫 ICallbackEventHandler 介面的方法</span><span class="sxs-lookup"><span data-stu-id="75a5f-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="75a5f-458">ICallbackEventHandler 包含兩個方法，您需要在程式碼中實作。</span><span class="sxs-lookup"><span data-stu-id="75a5f-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="75a5f-459">它們是**RaiseCallbackEvent**和**GetCallbackEvent**。</span><span class="sxs-lookup"><span data-stu-id="75a5f-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="75a5f-460">**RaiseCallbackEvent**接受字串做為引數，並傳回任何項目。</span><span class="sxs-lookup"><span data-stu-id="75a5f-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="75a5f-461">字串引數會傳遞 WebForm，用戶端呼叫\_DoCallback。</span><span class="sxs-lookup"><span data-stu-id="75a5f-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="75a5f-462">在此情況下，該值是*值*呼叫 ddlCompany 下拉式清單中的屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="75a5f-463">您的伺服器端程式碼應置於 RaiseCallbackEvent 方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="75a5f-464">例如，如果您的回呼正在進行 WebRequest 對外部資源時，該程式碼應放在 RaiseCallbackEvent。</span><span class="sxs-lookup"><span data-stu-id="75a5f-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="75a5f-465">**GetCallbackEvent**負責處理回傳給用戶端的回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="75a5f-466">它會採用任何引數，並傳回字串。</span><span class="sxs-lookup"><span data-stu-id="75a5f-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="75a5f-467">它會傳回的字串將傳遞做為引數至用戶端函式，在此情況下*ShowCompanyName*。</span><span class="sxs-lookup"><span data-stu-id="75a5f-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="75a5f-468">完成上述步驟之後，即準備好在 ASP.NET 2.0 中執行的指令碼的回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="75a5f-469">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="75a5f-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="75a5f-470">支援在 ASP.NET 中的指令碼回呼支援讓 XMLHttp 呼叫任何瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="75a5f-471">包含所有現代化瀏覽器中使用今天。</span><span class="sxs-lookup"><span data-stu-id="75a5f-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="75a5f-472">其他 （包含即將發行的 IE 7） 的新式瀏覽器使用的內建 XMLHttp 物件時，Internet Explorer 使用 XMLHttp ActiveX 物件。</span><span class="sxs-lookup"><span data-stu-id="75a5f-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="75a5f-473">若要以程式設計方式判斷是否瀏覽器支援回呼，您可以使用**Request.Browser.SupportCallback**屬性。</span><span class="sxs-lookup"><span data-stu-id="75a5f-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="75a5f-474">這個屬性會傳回**true**如果要求的用戶端支援回呼指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="75a5f-475">使用 ASP.NET 2.0 中的用戶端指令碼</span><span class="sxs-lookup"><span data-stu-id="75a5f-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="75a5f-476">透過使用 ClientScriptManager 類別管理 ASP.NET 2.0 中的用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="75a5f-477">ClientScriptManager 類別會追蹤的類型和名稱使用的用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="75a5f-478">這可防止相同的指令碼要以程式設計方式插入頁面上一次以上。</span><span class="sxs-lookup"><span data-stu-id="75a5f-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="75a5f-479">指令碼成功註冊頁面上之後，任何後續嘗試註冊相同的指令碼會直接導致未註冊第二次指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="75a5f-480">加入任何重複的指令碼，並沒有發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="75a5f-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="75a5f-481">若要避免不必要的計算，有可用來判斷 使未嘗試註冊一次以上，是否已註冊的指令碼的方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="75a5f-482">ClientScriptManager 方法應該熟悉所有目前的 ASP.NET 開發人員：</span><span class="sxs-lookup"><span data-stu-id="75a5f-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="75a5f-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="75a5f-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="75a5f-484">這個方法所呈現頁面的頂端加入指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="75a5f-485">這是用於將加入將用戶端明確呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="75a5f-486">有兩個多載的版本，這個方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="75a5f-487">四個引數中的三種通之間。</span><span class="sxs-lookup"><span data-stu-id="75a5f-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="75a5f-488">包括：</span><span class="sxs-lookup"><span data-stu-id="75a5f-488">They are:</span></span>

`type (string)`

<span data-ttu-id="75a5f-489">***類型***引數會識別指令碼的類型。</span><span class="sxs-lookup"><span data-stu-id="75a5f-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="75a5f-490">通常會建議来使用的頁面類型 （此項。GetType()) 類型。</span><span class="sxs-lookup"><span data-stu-id="75a5f-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="75a5f-491">***金鑰***引數是使用者定義索引鍵的指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="75a5f-492">這應該是唯一的每個指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-492">This should be unique for each script.</span></span> <span data-ttu-id="75a5f-493">如果您嘗試加入具有相同索引鍵和類型已經加入的指令碼的指令碼，它就不會加入。</span><span class="sxs-lookup"><span data-stu-id="75a5f-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="75a5f-494">***指令碼***引數是字串，包含要加入的實際指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="75a5f-495">建議您在建立指令碼，然後在指派的 StringBuilder 上使用 tostring （） 方法使用 StringBuilder***指令碼***引數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="75a5f-496">如果您使用多載的 RegisterClientScriptBlock 只採用三個引數時，您必須包含指令碼項目 (&lt;指令碼&gt;和&lt;/指令碼&gt;) 指令碼中。</span><span class="sxs-lookup"><span data-stu-id="75a5f-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="75a5f-497">您可以選擇使用 RegisterClientScriptBlock 接受第四個引數的多載。</span><span class="sxs-lookup"><span data-stu-id="75a5f-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="75a5f-498">第四個引數是布林值，指定 ASP.NET 應該為您加入指令碼項目。</span><span class="sxs-lookup"><span data-stu-id="75a5f-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="75a5f-499">如果這個引數是**true**，您的指令碼不應包含的指令碼項目明確。</span><span class="sxs-lookup"><span data-stu-id="75a5f-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="75a5f-500">您可以使用 IsClientScriptBlockRegistered 方法來判斷是否已登錄指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="75a5f-501">這可讓您避免嘗試重新註冊已註冊的指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="75a5f-502">RegisterClientScriptInclude （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="75a5f-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="75a5f-503">RegisterClientScriptInclude 標記建立指令碼區塊連結至外部指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="75a5f-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="75a5f-504">它有兩個多載。</span><span class="sxs-lookup"><span data-stu-id="75a5f-504">It has two overloads.</span></span> <span data-ttu-id="75a5f-505">一個採用索引鍵和 URL。</span><span class="sxs-lookup"><span data-stu-id="75a5f-505">One takes a key and a URL.</span></span> <span data-ttu-id="75a5f-506">第二個新增的指定類型的第三個引數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="75a5f-507">例如，下列程式碼會產生指令碼區塊連結至 jsfunctions.js 根目錄中的應用程式的指令碼資料夾：</span><span class="sxs-lookup"><span data-stu-id="75a5f-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="75a5f-508">此程式碼會產生下列程式碼，在呈現的頁面：</span><span class="sxs-lookup"><span data-stu-id="75a5f-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="75a5f-509">在頁面底部轉譯的指令碼區塊。</span><span class="sxs-lookup"><span data-stu-id="75a5f-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="75a5f-510">您可以使用 IsClientScriptIncludeRegistered 方法來判斷是否已登錄指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="75a5f-511">這可讓您避免嘗試重新註冊指令碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="75a5f-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="75a5f-512">RegisterStartupScript</span></span>

<span data-ttu-id="75a5f-513">RegisterStartupScript 方法會採用 RegisterClientScriptBlock 方法相同的引數。</span><span class="sxs-lookup"><span data-stu-id="75a5f-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="75a5f-514">在頁面載入之後，但在這段用戶端之前，向 RegisterStartupScript 指令碼會執行。</span><span class="sxs-lookup"><span data-stu-id="75a5f-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="75a5f-515">在 1.X RegisterStartupScript 與註冊的指令碼已放置的結束&lt;/形成&gt;標記時向 RegisterClientScriptBlock 指令碼放置在開啟之後立即&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="75a5f-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="75a5f-516">在 ASP.NET 2.0 中，同時位於關閉之前立即&lt;/形成&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="75a5f-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="75a5f-517">如果您使用 RegisterStartupScript 註冊函式，直到您明確呼叫它在用戶端程式碼中，將不會執行該函式。</span><span class="sxs-lookup"><span data-stu-id="75a5f-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="75a5f-518">若要判斷是否已登錄指令碼，並避免重新註冊的指令碼嘗試使用 IsStartupScriptRegistered 方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="75a5f-519">其他 ClientScriptManager 方法</span><span class="sxs-lookup"><span data-stu-id="75a5f-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="75a5f-520">以下是一些 ClientScriptManager 類別的其他有用的方法。</span><span class="sxs-lookup"><span data-stu-id="75a5f-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="75a5f-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="75a5f-522">請參閱稍早在此模組中的指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="75a5f-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="75a5f-524">取得 JavaScript 參考 (javascript:&lt;呼叫&gt;)，可以用來從用戶端事件公佈。</span><span class="sxs-lookup"><span data-stu-id="75a5f-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="75a5f-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="75a5f-526">取得可用來從用戶端的 post 的字串。</span><span class="sxs-lookup"><span data-stu-id="75a5f-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="75a5f-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="75a5f-528">內嵌組件中的資源傳回的 URL。</span><span class="sxs-lookup"><span data-stu-id="75a5f-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="75a5f-529">必須用於搭配<strong>RegisterClientScriptResource</strong>。</span><span class="sxs-lookup"><span data-stu-id="75a5f-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="75a5f-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="75a5f-531">註冊 Web 資源的網頁。</span><span class="sxs-lookup"><span data-stu-id="75a5f-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="75a5f-532">這些是內嵌在組件，而且新 WebResource.axd 處理常式所處理的資源。</span><span class="sxs-lookup"><span data-stu-id="75a5f-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="75a5f-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="75a5f-534">與網頁註冊隱藏的欄位。</span><span class="sxs-lookup"><span data-stu-id="75a5f-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="75a5f-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="75a5f-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="75a5f-536">註冊用戶端 HTML 表單送出時執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="75a5f-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

