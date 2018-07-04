---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: ASP.NET 2.0 頁面模型 |Microsoft Docs
author: microsoft
description: 在 ASP.NET 1.x 中，開發人員必須內嵌程式碼模型和程式碼後置程式碼模型之間做選擇。 無法使用其中一個的 Src 屬性來實作程式碼後置...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379883"
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="86e28-104">ASP.NET 2.0 頁面模型</span><span class="sxs-lookup"><span data-stu-id="86e28-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="86e28-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="86e28-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="86e28-106">在 ASP.NET 1.x 中，開發人員必須內嵌程式碼模型和程式碼後置程式碼模型之間做選擇。</span><span class="sxs-lookup"><span data-stu-id="86e28-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="86e28-107">無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。</span><span class="sxs-lookup"><span data-stu-id="86e28-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="86e28-108">在 ASP.NET 2.0 中，開發人員仍然必須選擇內嵌程式碼和程式碼後置，但已有顯著的增強功能的程式碼後置模型。</span><span class="sxs-lookup"><span data-stu-id="86e28-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="86e28-109">在 ASP.NET 1.x 中，開發人員必須內嵌程式碼模型和程式碼後置程式碼模型之間做選擇。</span><span class="sxs-lookup"><span data-stu-id="86e28-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="86e28-110">無法使用的 Src 屬性或程式碼後置屬性的實作程式碼後置@Page指示詞。</span><span class="sxs-lookup"><span data-stu-id="86e28-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="86e28-111">在 ASP.NET 2.0 中，開發人員仍然必須選擇內嵌程式碼和程式碼後置，但已有顯著的增強功能的程式碼後置模型。</span><span class="sxs-lookup"><span data-stu-id="86e28-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="86e28-112">在程式碼後置模型中的增強功能</span><span class="sxs-lookup"><span data-stu-id="86e28-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="86e28-113">若要完全了解 ASP.NET 2.0 中的程式碼後置模型中的變更，最好先快速的模型，因為它存在於 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="86e28-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="86e28-114">程式碼後置模型，在 ASP.NET 1.x</span><span class="sxs-lookup"><span data-stu-id="86e28-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="86e28-115">在 ASP.NET 1.x 中，程式碼後置模型所組成的 ASPX 檔案 (Webform) 和包含在程式碼的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="86e28-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="86e28-116">兩個檔案連接使用@PageASPX 檔案中的指示詞。</span><span class="sxs-lookup"><span data-stu-id="86e28-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="86e28-117">每個 ASPX 頁面上的控制項程式碼後置檔案中有對應的宣告，做為執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="86e28-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="86e28-118">程式碼後置檔案也包含事件繫結的程式碼，而且產生的程式碼所需的 Visual Studio 設計工具。</span><span class="sxs-lookup"><span data-stu-id="86e28-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="86e28-119">此模型運作很不錯，但每個 ASPX 頁面中的 ASP.NET 項目所需的程式碼後置檔案中的對應程式碼，因為沒有，則為 true 的區隔的程式碼和內容。</span><span class="sxs-lookup"><span data-stu-id="86e28-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="86e28-120">例如，如果設計工具會加入 Visual Studio IDE 外部 ASPX 檔案中的新的伺服器控制項，應用程式會破壞由於沒有宣告該控制項的程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="86e28-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="86e28-121">ASP.NET 2.0 中的程式碼後置模型</span><span class="sxs-lookup"><span data-stu-id="86e28-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="86e28-122">在此模型時可大幅提升 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="86e28-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="86e28-123">在 ASP.NET 2.0 中，程式碼後置會實作使用新*部分類別*ASP.NET 2.0 中提供。</span><span class="sxs-lookup"><span data-stu-id="86e28-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="86e28-124">在 ASP.NET 2.0 中的程式碼後置類別是定義為部分類別，表示它包含的類別定義的組件。</span><span class="sxs-lookup"><span data-stu-id="86e28-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="86e28-125">使用 ASPX 頁面，在執行階段，或當網站已先行編譯的 ASP.NET 2.0 來動態產生的類別定義的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="86e28-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="86e28-126">仍然使用 @ Page 指示詞來建立程式碼後置檔案與 ASPX 頁面之間的連結。</span><span class="sxs-lookup"><span data-stu-id="86e28-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="86e28-127">不過，而不是程式碼後置或 Src 屬性，ASP.NET 2.0 現在會使用 CodeFile 屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="86e28-128">Inherits 屬性也用於指定頁面的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="86e28-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="86e28-129">典型的 @ Page 指示詞可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="86e28-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="86e28-130">ASP.NET 2.0 程式碼後置檔案中的一般類別定義可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="86e28-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="86e28-131">C# 和 Visual Basic 是唯一的 managed 的語言目前支援部分類別。</span><span class="sxs-lookup"><span data-stu-id="86e28-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="86e28-132">因此，使用 J# 開發人員將無法使用 ASP.NET 2.0 中的程式碼後置模型。</span><span class="sxs-lookup"><span data-stu-id="86e28-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="86e28-133">新的模型會增強程式碼後置模型，因為開發人員現在會有包含他們所建立的程式碼的程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="86e28-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="86e28-134">它也會提供 true 隔離的程式碼和內容，因為沒有任何執行個體的變數宣告中的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="86e28-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="86e28-135">ASPX 頁面的部分類別是事件繫結發生的地方，因為 Visual Basic 開發人員可以利用控點關鍵字在程式碼後置中，若要將事件繫結實現輕微的效能增加。</span><span class="sxs-lookup"><span data-stu-id="86e28-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="86e28-136">C# 有沒有對等的關鍵字。</span><span class="sxs-lookup"><span data-stu-id="86e28-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="86e28-137">新的 @ Page 指示詞屬性</span><span class="sxs-lookup"><span data-stu-id="86e28-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="86e28-138">ASP.NET 2.0 會將 @ Page 指示詞中的許多新的屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="86e28-139">下列屬性是在 ASP.NET 2.0 新功能。</span><span class="sxs-lookup"><span data-stu-id="86e28-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="86e28-140">Async</span><span class="sxs-lookup"><span data-stu-id="86e28-140">Async</span></span>

<span data-ttu-id="86e28-141">Atribut Async 可讓您設定頁面，以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="86e28-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="86e28-142">我們將介紹稍後在本單元中的非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="86e28-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="86e28-143">AsyncTimeout</span></span>

<span data-ttu-id="86e28-144">指定的非同步頁面的逾時。</span><span class="sxs-lookup"><span data-stu-id="86e28-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="86e28-145">預設值為 45 秒。</span><span class="sxs-lookup"><span data-stu-id="86e28-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="86e28-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="86e28-146">CodeFile</span></span>

<span data-ttu-id="86e28-147">CodeFile 屬性會在 Visual Studio 2002/2003年 CodeBehind 屬性取代。</span><span class="sxs-lookup"><span data-stu-id="86e28-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="86e28-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="86e28-148">CodeFileBaseClass</span></span>

<span data-ttu-id="86e28-149">在您想要衍生自單一基底類別的多個頁面的情況下使用 CodeFileBaseClass 屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="86e28-150">由於在 ASP.NET 中，而這個屬性，不需要的部分類別的實作會使用共用通用的欄位來參考在 ASPX 頁面中宣告的控制項的基底類別將無法正常運作因為 ASP。網路編譯引擎會自動建立 頁面中的控制項為基礎的新成員。</span><span class="sxs-lookup"><span data-stu-id="86e28-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="86e28-151">因此，如果您想要在 ASP.NET 中的兩個或多個頁面通用的基底類別，您必須定義 CodeFileBaseClass 屬性中指定基底類別，然後再衍生自該基底類別的每個頁面的類別。</span><span class="sxs-lookup"><span data-stu-id="86e28-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="86e28-152">使用此屬性時，也是必要 CodeFile 屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="86e28-153">CompilationMode</span><span class="sxs-lookup"><span data-stu-id="86e28-153">CompilationMode</span></span>

<span data-ttu-id="86e28-154">這個屬性可讓您設定在 ASPX 頁面的 CompilationMode 屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="86e28-155">CompilationMode 屬性是包含值的列舉型別**永遠**，**自動**，並**永不**。</span><span class="sxs-lookup"><span data-stu-id="86e28-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="86e28-156">預設值是**永遠**。</span><span class="sxs-lookup"><span data-stu-id="86e28-156">The default is **Always**.</span></span> <span data-ttu-id="86e28-157">**自動**設定將使 ASP.NET 從動態編譯的頁面的話。</span><span class="sxs-lookup"><span data-stu-id="86e28-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="86e28-158">排除動態編譯的頁面，可以提升效能。</span><span class="sxs-lookup"><span data-stu-id="86e28-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="86e28-159">不過，如果已排除的頁面包含必須編譯該程式碼，將會擲回錯誤時瀏覽網頁時。</span><span class="sxs-lookup"><span data-stu-id="86e28-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="86e28-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="86e28-160">EnableEventValidation</span></span>

<span data-ttu-id="86e28-161">這個屬性會指定驗證回傳和回呼事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="86e28-162">當啟用此選項時，回傳的引數或回撥的事件會檢查以確定其來自原本呈現它們的伺服器控制項中。</span><span class="sxs-lookup"><span data-stu-id="86e28-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="86e28-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="86e28-163">EnableTheming</span></span>

<span data-ttu-id="86e28-164">這個屬性會指定在頁面上使用 ASP.NET 佈景主題。</span><span class="sxs-lookup"><span data-stu-id="86e28-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="86e28-165">預設值為 **false**。</span><span class="sxs-lookup"><span data-stu-id="86e28-165">The default is **false**.</span></span> <span data-ttu-id="86e28-166">ASP.NET 佈景主題所述[模組 10](profiles-themes-and-web-parts.md)。</span><span class="sxs-lookup"><span data-stu-id="86e28-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="86e28-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="86e28-167">LinePragmas</span></span>

<span data-ttu-id="86e28-168">這個屬性會指定是否應在編譯期間新增列 pragma。</span><span class="sxs-lookup"><span data-stu-id="86e28-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="86e28-169">行是由偵錯工具，用於標示的程式碼的特定區段的選項。</span><span class="sxs-lookup"><span data-stu-id="86e28-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="86e28-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="86e28-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="86e28-171">這個屬性會指定 JavaScript 會插入頁面維持回傳之間的捲軸位置。</span><span class="sxs-lookup"><span data-stu-id="86e28-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="86e28-172">這個屬性是**false**預設。</span><span class="sxs-lookup"><span data-stu-id="86e28-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="86e28-173">當這個屬性是 **，則為 true**，將 ASP.NET&lt;指令碼&gt;區塊在回傳時，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="86e28-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="86e28-174">請注意，此指令碼區塊的 src WebResource.axd。</span><span class="sxs-lookup"><span data-stu-id="86e28-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="86e28-175">此資源不是實體路徑。</span><span class="sxs-lookup"><span data-stu-id="86e28-175">This resource is not a physical path.</span></span> <span data-ttu-id="86e28-176">此指令碼要求時，ASP.NET 會動態建立指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="86e28-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="86e28-177">MasterPageFile</span></span>

<span data-ttu-id="86e28-178">這個屬性會指定目前頁面的主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="86e28-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="86e28-179">路徑可以是相對或絕對。</span><span class="sxs-lookup"><span data-stu-id="86e28-179">The path can be relative or absolute.</span></span> <span data-ttu-id="86e28-180">主版頁面所述[模組 4](master-pages.md)。</span><span class="sxs-lookup"><span data-stu-id="86e28-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="86e28-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="86e28-181">StyleSheetTheme</span></span>

<span data-ttu-id="86e28-182">這個屬性可讓您覆寫 ASP.NET 2.0 主題所定義的使用者介面外觀屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="86e28-183">佈景主題所述[模組 10](profiles-themes-and-web-parts.md)。</span><span class="sxs-lookup"><span data-stu-id="86e28-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="86e28-184">主題</span><span class="sxs-lookup"><span data-stu-id="86e28-184">Theme</span></span>

<span data-ttu-id="86e28-185">指定網頁的佈景主題。</span><span class="sxs-lookup"><span data-stu-id="86e28-185">Specifies the theme for the page.</span></span> <span data-ttu-id="86e28-186">如果值不指定 StyleSheetTheme 屬性，佈景主題屬性會覆寫套用至頁面上控制項的所有樣式。</span><span class="sxs-lookup"><span data-stu-id="86e28-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="86e28-187">標題</span><span class="sxs-lookup"><span data-stu-id="86e28-187">Title</span></span>

<span data-ttu-id="86e28-188">設定頁面的標題。</span><span class="sxs-lookup"><span data-stu-id="86e28-188">Sets the title for the page.</span></span> <span data-ttu-id="86e28-189">在這裡指定的值會出現在&lt;title&gt;呈現之網頁的項目。</span><span class="sxs-lookup"><span data-stu-id="86e28-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="86e28-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="86e28-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="86e28-191">設定裡把 ViewStateEncryptionMode 列舉的值。</span><span class="sxs-lookup"><span data-stu-id="86e28-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="86e28-192">可用的值為**永遠**，**自動**，並**永不**。</span><span class="sxs-lookup"><span data-stu-id="86e28-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="86e28-193">預設值是**自動**。當這個屬性設定為值**自動**，viewstate 會加密是控制項要求呼叫**RegisterRequiresViewStateEncryption**方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="86e28-194">設定公用屬性值透過 @ Page 指示詞</span><span class="sxs-lookup"><span data-stu-id="86e28-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="86e28-195">在 ASP.NET 2.0 中的 @ Page 指示詞的另一個新功能是能夠設定基底類別的公用屬性的初始值。</span><span class="sxs-lookup"><span data-stu-id="86e28-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="86e28-196">假設您擁有的公用屬性，例如呼叫**SomeText**基底類別和您喜歡 d 初始化為**Hello**網頁載入時。</span><span class="sxs-lookup"><span data-stu-id="86e28-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="86e28-197">您可以直接在 @ Page 指示詞中設定值來完成這就像這樣：</span><span class="sxs-lookup"><span data-stu-id="86e28-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="86e28-198">**SomeText** @ Page 指示詞屬性的基底類別中設定 SomeText 屬性的初始值*Hello ！*。</span><span class="sxs-lookup"><span data-stu-id="86e28-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="86e28-199">下列影片會逐步解說中使用 @ Page 指示詞的基底類別中設定的公用屬性的初始值。</span><span class="sxs-lookup"><span data-stu-id="86e28-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="86e28-200">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="86e28-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="86e28-201">頁面類別的新公用屬性</span><span class="sxs-lookup"><span data-stu-id="86e28-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="86e28-202">下列公用屬性是在 ASP.NET 2.0 新功能。</span><span class="sxs-lookup"><span data-stu-id="86e28-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="86e28-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="86e28-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="86e28-204">回到頁面或控制項的應用程式相對路徑。</span><span class="sxs-lookup"><span data-stu-id="86e28-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="86e28-205">例如，針對位於頁面http://app/folder/page.aspx，此屬性傳回 ~ / 資料夾 /。</span><span class="sxs-lookup"><span data-stu-id="86e28-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="86e28-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="86e28-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="86e28-207">回到頁面或控制項的相對虛擬目錄路徑。</span><span class="sxs-lookup"><span data-stu-id="86e28-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="86e28-208">例如對於頁面位於http://app/folder/page.aspx，此屬性傳回 ~ / folder/page.aspx。</span><span class="sxs-lookup"><span data-stu-id="86e28-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="86e28-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="86e28-209">AsyncTimeout</span></span>

<span data-ttu-id="86e28-210">取得或設定用於非同步頁面處理的逾時。</span><span class="sxs-lookup"><span data-stu-id="86e28-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="86e28-211">（稍後在本單元將說明非同步網頁）。</span><span class="sxs-lookup"><span data-stu-id="86e28-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="86e28-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="86e28-212">ClientQueryString</span></span>

<span data-ttu-id="86e28-213">唯讀屬性，傳回要求的 URL 查詢字串部分。</span><span class="sxs-lookup"><span data-stu-id="86e28-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="86e28-214">這個值是 URL 編碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-214">This value is URL encoded.</span></span> <span data-ttu-id="86e28-215">您可以使用 UrlDecode HttpServerUtility 類別方法，將它解碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="86e28-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="86e28-216">ClientScript</span></span>

<span data-ttu-id="86e28-217">這個屬性會傳回可用來管理用戶端指令碼 ASP.NETs 發出 ClientScriptManager 物件。</span><span class="sxs-lookup"><span data-stu-id="86e28-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="86e28-218">（ClientScriptManager 類別稍後在本單元涵蓋）。</span><span class="sxs-lookup"><span data-stu-id="86e28-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="86e28-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="86e28-219">EnableEventValidation</span></span>

<span data-ttu-id="86e28-220">此屬性控制啟用事件驗證回傳和回呼事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="86e28-221">啟用時，要回傳引數或回撥的事件會驗證以確保它們來自原本呈現它們的伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="86e28-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="86e28-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="86e28-222">EnableTheming</span></span>

<span data-ttu-id="86e28-223">此屬性會取得或設定布林值，指定 ASP.NET 2.0 主題套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="86e28-224">表單</span><span class="sxs-lookup"><span data-stu-id="86e28-224">Form</span></span>

<span data-ttu-id="86e28-225">這個屬性會傳回做為 HtmlForm 物件 ASPX 頁面上的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="86e28-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="86e28-226">頁首</span><span class="sxs-lookup"><span data-stu-id="86e28-226">Header</span></span>

<span data-ttu-id="86e28-227">這個屬性會傳回包含頁首的 HtmlHead 物件的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="86e28-228">您可以使用傳回的 HtmlHead 物件來取得/設定樣式表、 中繼標籤等等。</span><span class="sxs-lookup"><span data-stu-id="86e28-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="86e28-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="86e28-229">IdSeparator</span></span>

<span data-ttu-id="86e28-230">這個唯讀屬性取得用來分隔控制識別項，當 ASP.NET 建置頁面上控制項的唯一識別碼的字元。</span><span class="sxs-lookup"><span data-stu-id="86e28-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="86e28-231">但並不是針對直接從程式碼使用而設計。</span><span class="sxs-lookup"><span data-stu-id="86e28-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="86e28-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="86e28-232">IsAsync</span></span>

<span data-ttu-id="86e28-233">這個屬性允許非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="86e28-234">此模組稍後會討論非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="86e28-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="86e28-235">IsCallback</span></span>

<span data-ttu-id="86e28-236">這個唯讀屬性會傳回 **，則為 true**頁面是否為回呼的結果。</span><span class="sxs-lookup"><span data-stu-id="86e28-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="86e28-237">回撥會討論這個模組中的更新版本。</span><span class="sxs-lookup"><span data-stu-id="86e28-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="86e28-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="86e28-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="86e28-239">這個唯讀屬性會傳回 **，則為 true**如果頁面是跨網頁回傳的一部分。</span><span class="sxs-lookup"><span data-stu-id="86e28-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="86e28-240">稍後在本單元涵蓋跨網頁回傳。</span><span class="sxs-lookup"><span data-stu-id="86e28-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="86e28-241">項目</span><span class="sxs-lookup"><span data-stu-id="86e28-241">Items</span></span>

<span data-ttu-id="86e28-242">傳回 IDictionary 執行個體，其中包含儲存在頁面內容中的所有物件的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="86e28-243">您可以加入這個 IDictionary 物件中的項目，並將其內容的存留期使用。</span><span class="sxs-lookup"><span data-stu-id="86e28-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="86e28-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="86e28-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="86e28-245">此屬性控制 ASP.NET 會發出 JavaScript，維持頁面捲動瀏覽器中的位置之後會發生回傳。</span><span class="sxs-lookup"><span data-stu-id="86e28-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="86e28-246">（稍早在本單元中所討論的這個屬性的詳細資料）。</span><span class="sxs-lookup"><span data-stu-id="86e28-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="86e28-247">Master</span><span class="sxs-lookup"><span data-stu-id="86e28-247">Master</span></span>

<span data-ttu-id="86e28-248">這個唯讀屬性會傳回已套用主版頁面的頁面 MasterPage 執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="86e28-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="86e28-249">MasterPageFile</span></span>

<span data-ttu-id="86e28-250">取得或設定頁面的主版頁面檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="86e28-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="86e28-251">這個屬性只能設定 PreInit 方法中。</span><span class="sxs-lookup"><span data-stu-id="86e28-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="86e28-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="86e28-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="86e28-253">此屬性會取得或設定頁面狀態的最大長度 （位元組）。</span><span class="sxs-lookup"><span data-stu-id="86e28-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="86e28-254">如果屬性設定為正數，頁面檢視狀態將會分成多個隱藏的欄位，讓它不會超過指定的位元組數目。</span><span class="sxs-lookup"><span data-stu-id="86e28-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="86e28-255">如果屬性是負數，檢視狀態不會分割成區塊 （chunk）。</span><span class="sxs-lookup"><span data-stu-id="86e28-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="86e28-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="86e28-256">PageAdapter</span></span>

<span data-ttu-id="86e28-257">傳回修改的頁面要求的瀏覽器 PageAdapter 物件的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="86e28-258">上一頁</span><span class="sxs-lookup"><span data-stu-id="86e28-258">PreviousPage</span></span>

<span data-ttu-id="86e28-259">在 Server.Transfer 或跨網頁回傳的情況下會傳回前一頁的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="86e28-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="86e28-260">SkinID</span></span>

<span data-ttu-id="86e28-261">指定要套用至網頁的 ASP.NET 2.0 面板。</span><span class="sxs-lookup"><span data-stu-id="86e28-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="86e28-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="86e28-262">StyleSheetTheme</span></span>

<span data-ttu-id="86e28-263">此屬性會取得或設定套用至頁面的樣式表。</span><span class="sxs-lookup"><span data-stu-id="86e28-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="86e28-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="86e28-264">TemplateControl</span></span>

<span data-ttu-id="86e28-265">傳回包含控制項頁面的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="86e28-266">主題</span><span class="sxs-lookup"><span data-stu-id="86e28-266">Theme</span></span>

<span data-ttu-id="86e28-267">取得或設定套用至網頁的 ASP.NET 2.0 主題的名稱。</span><span class="sxs-lookup"><span data-stu-id="86e28-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="86e28-268">PreInit 方法之前，必須設定此值。</span><span class="sxs-lookup"><span data-stu-id="86e28-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="86e28-269">標題</span><span class="sxs-lookup"><span data-stu-id="86e28-269">Title</span></span>

<span data-ttu-id="86e28-270">此屬性會取得或設定頁面的標題，取得頁面標頭。</span><span class="sxs-lookup"><span data-stu-id="86e28-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="86e28-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="86e28-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="86e28-272">取得或設定頁面的裡把 ViewStateEncryptionMode。</span><span class="sxs-lookup"><span data-stu-id="86e28-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="86e28-273">請參閱稍早在本單元中的這個屬性的詳細的討論。</span><span class="sxs-lookup"><span data-stu-id="86e28-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="86e28-274">頁面類別的新受保護的內容</span><span class="sxs-lookup"><span data-stu-id="86e28-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="86e28-275">以下是在 ASP.NET 2.0 頁面類別的新受保護的內容。</span><span class="sxs-lookup"><span data-stu-id="86e28-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="86e28-276">配接器</span><span class="sxs-lookup"><span data-stu-id="86e28-276">Adapter</span></span>

<span data-ttu-id="86e28-277">傳回的參考會呈現網頁在裝置上的 ControlAdapter 提出此要求。</span><span class="sxs-lookup"><span data-stu-id="86e28-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="86e28-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="86e28-278">AsyncMode</span></span>

<span data-ttu-id="86e28-279">這個屬性會指出以非同步方式處理頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="86e28-280">它適用於執行階段，而非直接在程式碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="86e28-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="86e28-281">ClientIDSeparator</span></span>

<span data-ttu-id="86e28-282">這個屬性會傳回為控制項建立唯一的用戶端識別碼時，做為分隔符號的字元。</span><span class="sxs-lookup"><span data-stu-id="86e28-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="86e28-283">它適用於執行階段，而非直接在程式碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="86e28-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="86e28-284">PageStatePersister</span></span>

<span data-ttu-id="86e28-285">這個屬性會傳回頁面 PageStatePersister 物件。</span><span class="sxs-lookup"><span data-stu-id="86e28-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="86e28-286">這個屬性主要是由 ASP.NET 控制項開發人員使用。</span><span class="sxs-lookup"><span data-stu-id="86e28-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="86e28-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="86e28-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="86e28-288">這個屬性會傳回唯一的 suffic 附加至瀏覽器的快取的檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="86e28-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="86e28-289">預設值是\_ \_ufps = 和 6 位數的數字。</span><span class="sxs-lookup"><span data-stu-id="86e28-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="86e28-290">Page 類別的新公用方法</span><span class="sxs-lookup"><span data-stu-id="86e28-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="86e28-291">下列公用方法是在 ASP.NET 2.0 Page 類別的新項目。</span><span class="sxs-lookup"><span data-stu-id="86e28-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="86e28-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="86e28-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="86e28-293">這個方法會註冊為非同步頁面執行的事件處理常式委派。</span><span class="sxs-lookup"><span data-stu-id="86e28-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="86e28-294">此模組稍後會討論非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="86e28-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="86e28-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="86e28-296">頁面樣式表中的屬性套用至頁面中。</span><span class="sxs-lookup"><span data-stu-id="86e28-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="86e28-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="86e28-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="86e28-298">這個方法就非同步工作。</span><span class="sxs-lookup"><span data-stu-id="86e28-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="86e28-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="86e28-299">GetValidators</span></span>

<span data-ttu-id="86e28-300">如果未指定，則傳回驗證指定的驗證群組或預設的驗證群組的集合。</span><span class="sxs-lookup"><span data-stu-id="86e28-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="86e28-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="86e28-301">RegisterAsyncTask</span></span>

<span data-ttu-id="86e28-302">這個方法會註冊新的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="86e28-302">This method registers a new async task.</span></span> <span data-ttu-id="86e28-303">稍後在本單元涵蓋的非同步頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="86e28-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="86e28-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="86e28-305">這個方法會告訴 ASP.NET 頁面控制項狀態必須 persisted。</span><span class="sxs-lookup"><span data-stu-id="86e28-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="86e28-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="86e28-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="86e28-307">這個方法會告知 ASP.NET 頁面 viewstate，需要加密。</span><span class="sxs-lookup"><span data-stu-id="86e28-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="86e28-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="86e28-308">ResolveClientUrl</span></span>

<span data-ttu-id="86e28-309">傳回可用的映像等的用戶端要求的相對 URL。</span><span class="sxs-lookup"><span data-stu-id="86e28-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="86e28-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="86e28-310">SetFocus</span></span>

<span data-ttu-id="86e28-311">這個方法會將焦點設定至一開始載入頁面時指定的控制項。</span><span class="sxs-lookup"><span data-stu-id="86e28-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="86e28-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="86e28-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="86e28-313">這個方法會取消註冊為不再需要控制項狀態持續性傳遞給它的控制項。</span><span class="sxs-lookup"><span data-stu-id="86e28-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="86e28-314">變更網頁生命週期</span><span class="sxs-lookup"><span data-stu-id="86e28-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="86e28-315">在 ASP.NET 2.0 中的頁面週期尚未變動很大，但有一些您應該要注意的新方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="86e28-316">ASP.NET 2.0 頁面生命週期如下所述。</span><span class="sxs-lookup"><span data-stu-id="86e28-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="86e28-317">（新的 ASP.NET 2.0) 中的 PreInit</span><span class="sxs-lookup"><span data-stu-id="86e28-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="86e28-318">PreInit 事件是在開發人員可以存取的生命週期中最早的階段。</span><span class="sxs-lookup"><span data-stu-id="86e28-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="86e28-319">這個事件的加入可讓您能夠以程式設計方式變更 ASP.NET 2.0 主題，主版頁面、 存取 ASP.NET 2.0 設定檔等的屬性。如果您是在回傳狀態，其必須了解，Viewstate 具有尚未套用至目前生命週期中的控制項。</span><span class="sxs-lookup"><span data-stu-id="86e28-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="86e28-320">因此，如果開發人員變更控制項的屬性在這個階段，它可能被覆寫稍後在頁面生命週期中。</span><span class="sxs-lookup"><span data-stu-id="86e28-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="86e28-321">Init</span><span class="sxs-lookup"><span data-stu-id="86e28-321">Init</span></span>

<span data-ttu-id="86e28-322">尚未變更 Init 事件，這是從 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="86e28-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="86e28-323">這是您想要讀取或初始化的頁面上控制項的屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="86e28-324">在此階段、 主版頁面、 佈景主題等已套用至網頁。</span><span class="sxs-lookup"><span data-stu-id="86e28-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="86e28-325">緊隨在 InitComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="86e28-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="86e28-326">緊隨在 InitComplete 事件會在頁面初始化階段結束時呼叫。</span><span class="sxs-lookup"><span data-stu-id="86e28-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="86e28-327">此時在生命週期中，您可以存取控制項在頁面上，但尚未填入其狀態。</span><span class="sxs-lookup"><span data-stu-id="86e28-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="86e28-328">預先載入 （新功能 2.0）</span><span class="sxs-lookup"><span data-stu-id="86e28-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="86e28-329">已套用所有的回傳資料之後，頁面之前，會呼叫此事件\_負載。</span><span class="sxs-lookup"><span data-stu-id="86e28-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="86e28-330">Load</span><span class="sxs-lookup"><span data-stu-id="86e28-330">Load</span></span>

<span data-ttu-id="86e28-331">尚未變更的 Load 事件，這是從 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="86e28-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="86e28-332">LoadComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="86e28-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="86e28-333">LoadComplete 事件是在頁面載入階段的最後一個事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="86e28-334">在這個階段，回傳和 viewstate 的所有資料已都套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="86e28-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="86e28-335">PreRender</span></span>

<span data-ttu-id="86e28-336">如果您想要能夠正確保留會以動態方式加入至頁面的控制項的 viewstate，PreRender 事件就會是將它們加入的最後機會。</span><span class="sxs-lookup"><span data-stu-id="86e28-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="86e28-337">PreRenderComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="86e28-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="86e28-338">在 PreRenderComplete 階段中，所有控制項都已都加入至頁面，頁面可以開始呈現。</span><span class="sxs-lookup"><span data-stu-id="86e28-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="86e28-339">PreRenderComplete 事件是網頁檢視狀態儲存之前，所引發的最後一個事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="86e28-340">SaveStateComplete （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="86e28-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="86e28-341">SaveStateComplete 事件是只有在已儲存所有的頁面檢視狀態和控制項狀態之後，立即呼叫。</span><span class="sxs-lookup"><span data-stu-id="86e28-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="86e28-342">這是頁面實際轉譯至瀏覽器之前的最後一個事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="86e28-343">轉譯</span><span class="sxs-lookup"><span data-stu-id="86e28-343">Render</span></span>

<span data-ttu-id="86e28-344">Render 方法尚未變更自 ASP.NET 1.x。</span><span class="sxs-lookup"><span data-stu-id="86e28-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="86e28-345">這是其中的 HtmlTextWriter 已初始化，且頁面會呈現在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="86e28-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="86e28-346">ASP.NET 2.0 中的跨網頁回傳</span><span class="sxs-lookup"><span data-stu-id="86e28-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="86e28-347">在 ASP.NET 1.x 中，回傳都必須要張貼到相同的頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="86e28-348">不允許跨網頁回傳。</span><span class="sxs-lookup"><span data-stu-id="86e28-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="86e28-349">ASP.NET 2.0 新增回傳給不同的頁面，透過 IButtonControl 介面的能力。</span><span class="sxs-lookup"><span data-stu-id="86e28-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="86e28-350">實作新的 IButtonControl 介面 （按鈕、 LinkButton 和協力廠商自訂控制項除了 ImageButton） 的任何控制項可以利用這項新功能透過 PostBackUrl 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="86e28-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="86e28-351">下列程式碼顯示的按鈕控制項回傳至第二個頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="86e28-352">頁面回傳，啟始回傳網頁時，可透過第二個頁面的 PreviousPage 屬性存取。</span><span class="sxs-lookup"><span data-stu-id="86e28-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="86e28-353">這項功能透過新的 WebForm 實作\_DoPostBackWithOptions 用戶端函式，會呈現 ASP.NET 2.0 頁面，但控制項回傳至不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="86e28-354">這個 JavaScript 函式會提供新的 WebResource.axd 處理常式，這時用戶端的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="86e28-355">下列影片會跨網頁回傳的解說。</span><span class="sxs-lookup"><span data-stu-id="86e28-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="86e28-356">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="86e28-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="86e28-357">在跨網頁回傳的更多詳細資料</span><span class="sxs-lookup"><span data-stu-id="86e28-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="86e28-358">Viewstate</span><span class="sxs-lookup"><span data-stu-id="86e28-358">Viewstate</span></span>

<span data-ttu-id="86e28-359">您可能會要求您自己已經有關會發生什麼事 viewstate 在跨網頁回傳的案例中的第一頁從。</span><span class="sxs-lookup"><span data-stu-id="86e28-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="86e28-360">畢竟，任何控制項，不會實作 IPostBackDataHandler 會因此保存其狀態透過 viewstate，跨網頁回傳的第二個頁面上的該控制項的屬性存取，您必須能夠存取 viewstate 頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="86e28-361">ASP.NET 2.0 會處理此案例中使用新的隱藏的欄位，在呼叫的第二頁\_ \_PREVIOUSPAGE。</span><span class="sxs-lookup"><span data-stu-id="86e28-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="86e28-362">\_ \_PREVIOUSPAGE 表單欄位包含 viewstate 的第一頁，以便您可以在第二個頁面中的所有控制項的屬性的存取權。</span><span class="sxs-lookup"><span data-stu-id="86e28-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="86e28-363">規避 FindControl</span><span class="sxs-lookup"><span data-stu-id="86e28-363">Circumventing FindControl</span></span>

<span data-ttu-id="86e28-364">跨網頁回傳的視訊逐步解說中，在中，我可以使用的 FindControl 方法來取得第一頁上的文字方塊控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="86e28-365">這個方法適用於這個目的，但 FindControl 昂貴，而且需要撰寫額外程式碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="86e28-366">幸運的是，ASP.NET 2.0 提供可在許多情況下此用途 FindControl 的替代方案。</span><span class="sxs-lookup"><span data-stu-id="86e28-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="86e28-367">PreviousPageType 指示詞可讓您有使用 TypeName 或 VirtualPath 屬性的前一頁的強型別參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="86e28-368">TypeName 屬性可讓您指定先前的頁面類型，VirtualPath 屬性可讓您參考前一個頁面使用的虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="86e28-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="86e28-369">設定 PreviousPageType 指示詞之後，您必須再公開控制項等您要允許存取使用的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="86e28-370">實驗室 1 跨網頁回傳</span><span class="sxs-lookup"><span data-stu-id="86e28-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="86e28-371">在此實驗室中，您將建立使用 ASP.NET 2.0 新的跨網頁回傳功能的應用程式。</span><span class="sxs-lookup"><span data-stu-id="86e28-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="86e28-372">開啟 Visual Studio 2005 並建立新的 ASP.NET 網站。</span><span class="sxs-lookup"><span data-stu-id="86e28-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="86e28-373">加入新的 Webform 呼叫 page2.aspx。</span><span class="sxs-lookup"><span data-stu-id="86e28-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="86e28-374">在 [設計] 檢視中開啟 Default.aspx，並新增按鈕控制項和文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="86e28-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="86e28-375">提供按鈕控制項的 ID **SubmitButton**和文字方塊控制項的 ID **UserName**。</span><span class="sxs-lookup"><span data-stu-id="86e28-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="86e28-376">設 page2.aspx PostBackUrl 按鈕的屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="86e28-377">開啟 page2.aspx 來源檢視中。</span><span class="sxs-lookup"><span data-stu-id="86e28-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="86e28-378">加入 @ PreviousPageType 指示詞，如下所示：</span><span class="sxs-lookup"><span data-stu-id="86e28-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="86e28-379">將下列程式碼新增至頁面\_page2.aspx 的程式碼後置的負載：</span><span class="sxs-lookup"><span data-stu-id="86e28-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="86e28-380">在 [建置] 功能表上按一下組建中建置專案。</span><span class="sxs-lookup"><span data-stu-id="86e28-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="86e28-381">程式碼後置 Default.aspx 中加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="86e28-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="86e28-382">變更頁面\_負載 page2.aspx 如下：</span><span class="sxs-lookup"><span data-stu-id="86e28-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="86e28-383">建置專案。</span><span class="sxs-lookup"><span data-stu-id="86e28-383">Build the project.</span></span>
11. <span data-ttu-id="86e28-384">執行專案。</span><span class="sxs-lookup"><span data-stu-id="86e28-384">Run the project.</span></span>
12. <span data-ttu-id="86e28-385">在文字方塊中輸入您的名稱，然後按一下  按鈕。</span><span class="sxs-lookup"><span data-stu-id="86e28-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="86e28-386">什麼是結果？</span><span class="sxs-lookup"><span data-stu-id="86e28-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="86e28-387">ASP.NET 2.0 中的非同步頁面</span><span class="sxs-lookup"><span data-stu-id="86e28-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="86e28-388">在 ASP.NET 中的許多競爭問題被因外部呼叫 （例如 Web 服務或資料庫呼叫），檔案 IO 延遲等等的延遲。當要求是針對 ASP.NET 應用程式，ASP.NET 會使用其中一個背景工作執行緒來服務該要求。</span><span class="sxs-lookup"><span data-stu-id="86e28-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="86e28-389">該要求會擁有該執行緒，直到完成要求且在傳送回應。</span><span class="sxs-lookup"><span data-stu-id="86e28-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="86e28-390">ASP.NET 2.0 會設法加入以非同步方式執行頁面的功能來解決這類問題與延遲問題。</span><span class="sxs-lookup"><span data-stu-id="86e28-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="86e28-391">這表示工作者執行緒可啟動要求，並接著遞交到另一個執行緒，進而快速地返回可用的執行緒集區的其他執行。</span><span class="sxs-lookup"><span data-stu-id="86e28-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="86e28-392">檔案 IO、 資料庫呼叫等完成時，新的執行緒被取自來完成要求的執行緒集區中。</span><span class="sxs-lookup"><span data-stu-id="86e28-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="86e28-393">讓頁面，以非同步方式執行的第一個步驟是設定**非同步**頁面指示詞屬性就像這樣：</span><span class="sxs-lookup"><span data-stu-id="86e28-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="86e28-394">此屬性會告知 ASP.NET 實作 IHttpAsyncHandler 頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="86e28-395">下一個步驟是在生命週期中頁面在 PreRender 之前的時間點呼叫 AddOnPreRenderCompleteAsync 方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="86e28-396">(在頁面中通常會呼叫此方法\_負載。)AddOnPreRenderCompleteAsync 方法採用兩個參數;BeginEventHandler 和 EndEventHandler。</span><span class="sxs-lookup"><span data-stu-id="86e28-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="86e28-397">BeginEventHandler 傳回再做為參數傳遞至 EndEventHandler IAsyncResult。</span><span class="sxs-lookup"><span data-stu-id="86e28-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="86e28-398">下列影片會逐步解說中的非同步頁面要求。</span><span class="sxs-lookup"><span data-stu-id="86e28-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="86e28-399">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="86e28-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="86e28-400">非同步頁面不會呈現至瀏覽器，直到完成 EndEventHandler。</span><span class="sxs-lookup"><span data-stu-id="86e28-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="86e28-401">毫無疑問，但有些開發人員會將視為類似非同步回呼的非同步要求。</span><span class="sxs-lookup"><span data-stu-id="86e28-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="86e28-402">請務必了解它們不是。</span><span class="sxs-lookup"><span data-stu-id="86e28-402">It's important to realize that they are not.</span></span> <span data-ttu-id="86e28-403">非同步要求的好處是第一個背景工作執行緒可以回到執行緒集區，才能服務新要求，因此也降低因 IO 繫結的爭用等等。</span><span class="sxs-lookup"><span data-stu-id="86e28-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="86e28-404">Script Callbacks in ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="86e28-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="86e28-405">Web 開發人員一律尋找的方法可防止與回呼相關聯的閃爍。</span><span class="sxs-lookup"><span data-stu-id="86e28-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="86e28-406">在 ASP.NET 1.x，SmartNavigation 避免閃爍的最常見的方法卻 SmartNavigation 某些開發人員造成問題，因為其在用戶端上實作的複雜性。</span><span class="sxs-lookup"><span data-stu-id="86e28-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="86e28-407">ASP.NET 2.0 解決這個問題的指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="86e28-408">指令碼回呼使用 XMLHttp 對透過 JavaScript 的網頁伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="86e28-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="86e28-409">XMLHttp 要求會傳回 XML 資料，然後可操作透過瀏覽器的 dom。</span><span class="sxs-lookup"><span data-stu-id="86e28-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="86e28-410">XMLHttp 程式碼隱藏使用者從新的 WebResource.axd 處理常式。</span><span class="sxs-lookup"><span data-stu-id="86e28-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="86e28-411">有幾個步驟所需以 ASP.NET 2.0 中設定指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="86e28-412">步驟 1： 實作 ICallbackEventHandler 介面</span><span class="sxs-lookup"><span data-stu-id="86e28-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="86e28-413">為了讓 ASP.NET 才能辨識您的頁面為參與的指令碼回呼，您必須實作 ICallbackEventHandler 介面。</span><span class="sxs-lookup"><span data-stu-id="86e28-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="86e28-414">您可以在您的程式碼後置檔案就像這樣：</span><span class="sxs-lookup"><span data-stu-id="86e28-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="86e28-415">您也可以執行這讓使用 @ Implements 指示詞類似：</span><span class="sxs-lookup"><span data-stu-id="86e28-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="86e28-416">使用內嵌 ASP.NET 程式碼時，您通常會使用 @ Implements 指示詞。</span><span class="sxs-lookup"><span data-stu-id="86e28-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="86e28-417">步驟 2： 呼叫 GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="86e28-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="86e28-418">如先前所述，XMLHttp 呼叫封裝中的 WebResource.axd 處理常式。</span><span class="sxs-lookup"><span data-stu-id="86e28-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="86e28-419">ASP.NET 呈現您的頁面時，會將呼叫加入 WebForm\_DoCallback，由 WebResource.axd 提供用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="86e28-420">WebForm\_DoCallback 函式會取代\_ \_doPostBack 函式的回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="86e28-421">請記住\_ \_doPostBack 以程式設計方式提交頁面上的表單。</span><span class="sxs-lookup"><span data-stu-id="86e28-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="86e28-422">在回撥案例中，您想要防止回傳，因此\_ \_doPostBack 會不敷使用。</span><span class="sxs-lookup"><span data-stu-id="86e28-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="86e28-423">\_\_doPostBack 仍會呈現在用戶端指令碼回呼案例頁面。</span><span class="sxs-lookup"><span data-stu-id="86e28-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="86e28-424">不過，它不會使用回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="86e28-425">WebForm 的引數\_DoCallback 用戶端函式會提供透過伺服器端函式通常會在頁面中稱為 GetCallbackEventReference\_負載。</span><span class="sxs-lookup"><span data-stu-id="86e28-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="86e28-426">GetCallbackEventReference 的典型呼叫看起來可能像這樣：</span><span class="sxs-lookup"><span data-stu-id="86e28-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="86e28-427">在此情況下，cm 是 ClientScriptManager 執行個體。</span><span class="sxs-lookup"><span data-stu-id="86e28-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="86e28-428">本單元之後，將會說明 ClientScriptManager 類別。</span><span class="sxs-lookup"><span data-stu-id="86e28-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="86e28-429">有數個 GetCallbackEventReference 的多載的版本。</span><span class="sxs-lookup"><span data-stu-id="86e28-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="86e28-430">在此情況下，引數是的如下所示：</span><span class="sxs-lookup"><span data-stu-id="86e28-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="86e28-431">正在呼叫 GetCallbackEventReference 控制項的參考。</span><span class="sxs-lookup"><span data-stu-id="86e28-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="86e28-432">在此情況下，它是網頁本身。</span><span class="sxs-lookup"><span data-stu-id="86e28-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="86e28-433">字串引數會從用戶端程式碼傳遞至伺服器端事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="86e28-434">在此情況下，傳遞值的下拉式清單中的 Im 呼叫 ddlCompany。</span><span class="sxs-lookup"><span data-stu-id="86e28-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="86e28-435">（如字串） 要接受的傳回值的伺服器端回呼事件的用戶端函式名稱。</span><span class="sxs-lookup"><span data-stu-id="86e28-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="86e28-436">伺服器端回呼成功時，只會呼叫此函數。</span><span class="sxs-lookup"><span data-stu-id="86e28-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="86e28-437">因此，為了穩定性，通常建議使用 GetCallbackEventReference 採用指定的名稱，用戶端函式發生錯誤時所執行的其他字串引數的多載的版本。</span><span class="sxs-lookup"><span data-stu-id="86e28-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="86e28-438">字串，表示系統已起始與伺服器在回呼之前的用戶端函式。</span><span class="sxs-lookup"><span data-stu-id="86e28-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="86e28-439">在此情況下，沒有任何這類指令碼，因此引數為 null。</span><span class="sxs-lookup"><span data-stu-id="86e28-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="86e28-440">布林值，指定要以非同步方式進行回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="86e28-441">WebForm 呼叫\_DoCallback 在用戶端會將這些引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="86e28-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="86e28-442">因此，當用戶端上呈現此頁面時，該程式碼看起來就像這樣：</span><span class="sxs-lookup"><span data-stu-id="86e28-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="86e28-443">請注意用戶端上的函式的簽章會有點不同。</span><span class="sxs-lookup"><span data-stu-id="86e28-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="86e28-444">用戶端函式會將 5 個字串和布林值傳遞。</span><span class="sxs-lookup"><span data-stu-id="86e28-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="86e28-445">（這是在上述範例中為 null） 的其他字串包含用戶端函式會處理任何錯誤的伺服器端的回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="86e28-446">步驟 3： 將連結的用戶端控制項事件</span><span class="sxs-lookup"><span data-stu-id="86e28-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="86e28-447">請注意，上述 GetCallbackEventReference 的傳回值已指派給字串變數。</span><span class="sxs-lookup"><span data-stu-id="86e28-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="86e28-448">該字串用來將連結控制項初始化回呼用戶端事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="86e28-449">在此範例中，回呼由下拉式清單中的頁面上，我想要將連結*OnChange*事件。</span><span class="sxs-lookup"><span data-stu-id="86e28-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="86e28-450">若要攔截用戶端事件，只要將處理常式加入用戶端標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="86e28-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="86e28-451">請記得， *cbRef* GetCallbackEventReference 呼叫的傳回值。</span><span class="sxs-lookup"><span data-stu-id="86e28-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="86e28-452">它包含 WebForm 呼叫\_已如上所示的 DoCallback。</span><span class="sxs-lookup"><span data-stu-id="86e28-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="86e28-453">步驟 4： 註冊用戶端指令碼</span><span class="sxs-lookup"><span data-stu-id="86e28-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="86e28-454">回想一下，GetCallbackEventReference 呼叫指定的用戶端指令碼，呼叫**ShowCompanyName**會在伺服器端回呼成功時執行。</span><span class="sxs-lookup"><span data-stu-id="86e28-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="86e28-455">該指令碼必須加入至頁面使用 ClientScriptManager 執行個體。</span><span class="sxs-lookup"><span data-stu-id="86e28-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="86e28-456">（ClientScriptManager 類別將會是這稍後在本單元中的）。因此，您執行該類似：</span><span class="sxs-lookup"><span data-stu-id="86e28-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="86e28-457">步驟 5： 呼叫 ICallbackEventHandler 介面的方法</span><span class="sxs-lookup"><span data-stu-id="86e28-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="86e28-458">ICallbackEventHandler 包含您要在您的程式碼中實作的兩個方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="86e28-459">它們**RaiseCallbackEvent**並**GetCallbackEvent**。</span><span class="sxs-lookup"><span data-stu-id="86e28-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="86e28-460">**RaiseCallbackEvent**會做為引數的字串，並且會傳回任何內容。</span><span class="sxs-lookup"><span data-stu-id="86e28-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="86e28-461">字串引數會傳遞從用戶端呼叫 WebForm\_DoCallback。</span><span class="sxs-lookup"><span data-stu-id="86e28-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="86e28-462">在此案例中，該值是*值*呼叫 ddlCompany 下拉式清單中的屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="86e28-463">您的伺服器端程式碼應該置於 RaiseCallbackEvent 方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="86e28-464">例如，如果您的回呼正在對外部資源的 WebRequest，該程式碼應該置於 RaiseCallbackEvent。</span><span class="sxs-lookup"><span data-stu-id="86e28-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="86e28-465">**GetCallbackEvent**負責處理回呼的傳回至用戶端。</span><span class="sxs-lookup"><span data-stu-id="86e28-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="86e28-466">它會採用任何引數，並傳回字串。</span><span class="sxs-lookup"><span data-stu-id="86e28-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="86e28-467">它會傳回的字串會傳遞做為引數至用戶端函式，在此情況下*ShowCompanyName*。</span><span class="sxs-lookup"><span data-stu-id="86e28-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="86e28-468">完成上述步驟後，您已準備好在 ASP.NET 2.0 中執行的指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="86e28-469">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="86e28-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="86e28-470">支援在 ASP.NET 中的指令碼回呼支援讓 XMLHttp 呼叫任何瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="86e28-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="86e28-471">其中包含所有現代瀏覽器中使用今天。</span><span class="sxs-lookup"><span data-stu-id="86e28-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="86e28-472">其他 （包括即將推出 IE 7） 的現代瀏覽器使用的內建的 XMLHttp 物件時，Internet Explorer 會使用 XMLHttp ActiveX 物件。</span><span class="sxs-lookup"><span data-stu-id="86e28-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="86e28-473">若要以程式設計方式判斷是否瀏覽器支援回呼，您可以使用**Request.Browser.SupportCallback**屬性。</span><span class="sxs-lookup"><span data-stu-id="86e28-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="86e28-474">這個屬性會傳回 **，則為 true**如果要求的用戶端支援指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="86e28-475">使用 ASP.NET 2.0 中的用戶端指令碼</span><span class="sxs-lookup"><span data-stu-id="86e28-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="86e28-476">透過使用 ClientScriptManager 類別來管理 ASP.NET 2.0 中的用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="86e28-477">ClientScriptManager 類別會追蹤的用戶端指令碼使用的型別和名稱。</span><span class="sxs-lookup"><span data-stu-id="86e28-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="86e28-478">這可防止相同的指令碼要以程式設計方式插入頁面上一次以上。</span><span class="sxs-lookup"><span data-stu-id="86e28-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="86e28-479">指令碼已成功註冊頁面上之後，任何後續的嘗試註冊相同的指令碼只會在指令碼中未註冊第二次。</span><span class="sxs-lookup"><span data-stu-id="86e28-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="86e28-480">加入任何重複的指令碼，並沒有發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="86e28-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="86e28-481">若要避免不必要的計算，有可用來判斷指令碼是否已經註冊，讓您不會嘗試向一次以上的方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="86e28-482">ClientScriptManager 方法應該是所有目前的 ASP.NET 開發人員所熟悉的：</span><span class="sxs-lookup"><span data-stu-id="86e28-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="86e28-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="86e28-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="86e28-484">這個方法會將指令碼，來呈現頁面的頂端。</span><span class="sxs-lookup"><span data-stu-id="86e28-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="86e28-485">這可用於新增將用戶端明確呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="86e28-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="86e28-486">有兩個多載的版本，這個方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="86e28-487">三個四個引數是在它們之間通用的。</span><span class="sxs-lookup"><span data-stu-id="86e28-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="86e28-488">包括：</span><span class="sxs-lookup"><span data-stu-id="86e28-488">They are:</span></span>

`type (string)`

<span data-ttu-id="86e28-489">***型別***引數會識別指令碼的類型。</span><span class="sxs-lookup"><span data-stu-id="86e28-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="86e28-490">它通常是個不錯的主意，若要使用的頁面類型 （這點。GetType()) 類型。</span><span class="sxs-lookup"><span data-stu-id="86e28-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="86e28-491">***金鑰***引數是使用者定義索引鍵的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="86e28-492">這應該是唯一的每個指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-492">This should be unique for each script.</span></span> <span data-ttu-id="86e28-493">如果您嘗試加入具有相同的索引鍵和型別已新增的指令碼的指令碼，它將不會加入。</span><span class="sxs-lookup"><span data-stu-id="86e28-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="86e28-494">***指令碼***引數是字串，包含要新增的實際指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="86e28-495">建議您在建立指令碼，然後在指派的 StringBuilder 上使用 tostring （） 方法使用 StringBuilder***指令碼***引數。</span><span class="sxs-lookup"><span data-stu-id="86e28-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="86e28-496">如果您使用多載的 RegisterClientScriptBlock 只接受三個引數時，您必須包括指令碼項目 (&lt;指令碼&gt;並&lt;/script&gt;) 指令碼中。</span><span class="sxs-lookup"><span data-stu-id="86e28-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="86e28-497">您可以選擇使用 RegisterClientScriptBlock 接受第四個引數的多載。</span><span class="sxs-lookup"><span data-stu-id="86e28-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="86e28-498">第四個引數是布林值，指定 ASP.NET 應該為您新增指令碼項目。</span><span class="sxs-lookup"><span data-stu-id="86e28-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="86e28-499">如果這個引數是 **，則為 true**，您的指令碼不應包含的指令碼項目明確。</span><span class="sxs-lookup"><span data-stu-id="86e28-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="86e28-500">您可以使用 IsClientScriptBlockRegistered 方法來判斷是否已登錄的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="86e28-501">這可讓您避免嘗試重新註冊已註冊的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="86e28-502">RegisterClientScriptInclude （新功能 2.0)</span><span class="sxs-lookup"><span data-stu-id="86e28-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="86e28-503">RegisterClientScriptInclude 標記建立指令碼區塊連結至外部指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="86e28-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="86e28-504">它有兩個多載。</span><span class="sxs-lookup"><span data-stu-id="86e28-504">It has two overloads.</span></span> <span data-ttu-id="86e28-505">其中一個採用索引鍵和 URL。</span><span class="sxs-lookup"><span data-stu-id="86e28-505">One takes a key and a URL.</span></span> <span data-ttu-id="86e28-506">第二個加入的指定類型的第三個引數。</span><span class="sxs-lookup"><span data-stu-id="86e28-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="86e28-507">例如，下列程式碼會產生指令碼區塊連結至 jsfunctions.js 根目錄中的應用程式的 [指令碼] 資料夾：</span><span class="sxs-lookup"><span data-stu-id="86e28-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="86e28-508">此程式碼會產生下列程式碼在呈現的頁面：</span><span class="sxs-lookup"><span data-stu-id="86e28-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="86e28-509">在頁面底部轉譯的指令碼區塊。</span><span class="sxs-lookup"><span data-stu-id="86e28-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="86e28-510">您可以使用 IsClientScriptIncludeRegistered 方法來判斷是否已登錄的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="86e28-511">這可讓您避免嘗試重新註冊的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="86e28-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="86e28-512">RegisterStartupScript</span></span>

<span data-ttu-id="86e28-513">RegisterStartupScript 方法採用相同的引數，做為 RegisterClientScriptBlock 方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="86e28-514">在頁面載入之後但在這段用戶端之前，就會執行向 RegisterStartupScript 指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="86e28-515">在 1.X 中，向 RegisterStartupScript 的指令碼已置於結尾之前&lt;/形成&gt;標記時向 RegisterClientScriptBlock 的指令碼放置在開頭之後立即&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="86e28-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="86e28-516">在 ASP.NET 2.0 中，同時會放在結尾之前，立即&lt;/形成&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="86e28-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="86e28-517">如果您向 RegisterStartupScript 函式，該函式將不會執行，直到明確地在用戶端程式碼中呼叫。</span><span class="sxs-lookup"><span data-stu-id="86e28-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="86e28-518">您可以使用 IsStartupScriptRegistered 方法來判斷是否已登錄的指令碼，並避免嘗試重新註冊的指令碼。</span><span class="sxs-lookup"><span data-stu-id="86e28-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="86e28-519">其他 ClientScriptManager 方法</span><span class="sxs-lookup"><span data-stu-id="86e28-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="86e28-520">以下是一些 ClientScriptManager 類別的其他有用的方法。</span><span class="sxs-lookup"><span data-stu-id="86e28-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="86e28-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="86e28-522">請參閱稍早在本單元中的指令碼回呼。</span><span class="sxs-lookup"><span data-stu-id="86e28-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="86e28-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="86e28-524">取得 JavaScript 參考 (javascript:&lt;呼叫&gt;)，可用來從用戶端事件回傳。</span><span class="sxs-lookup"><span data-stu-id="86e28-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="86e28-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="86e28-526">取得字串，可用來起始從用戶端 post。</span><span class="sxs-lookup"><span data-stu-id="86e28-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="86e28-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="86e28-528">內嵌組件中的資源傳回的 URL。</span><span class="sxs-lookup"><span data-stu-id="86e28-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="86e28-529">必須用於搭配<strong>RegisterClientScriptResource</strong>。</span><span class="sxs-lookup"><span data-stu-id="86e28-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="86e28-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="86e28-531">向頁面註冊 Web 資源。</span><span class="sxs-lookup"><span data-stu-id="86e28-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="86e28-532">這些是內嵌在組件，而且新的 WebResource.axd 處理常式所處理的資源。</span><span class="sxs-lookup"><span data-stu-id="86e28-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="86e28-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="86e28-534">向頁面註冊隱藏的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="86e28-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="86e28-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="86e28-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="86e28-536">註冊用戶端程式碼的 HTML 表單提交時執行。</span><span class="sxs-lookup"><span data-stu-id="86e28-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

