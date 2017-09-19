---
title: "撰寫 ASP.NET Core 中的標記協助程式"
author: rick-anderson
description: "了解如何撰寫 ASP.NET Core 中標記協助程式。"
keywords: "ASP.NET Core，標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a5222da1380c2fe768b287bfa1a49b300c02f2b
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="ed211-104">撰寫 ASP.NET Core，範例與逐步解說中的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ed211-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="ed211-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed211-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="ed211-106">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="ed211-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="ed211-107">開始使用標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ed211-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="ed211-108">本教學課程介紹程式設計標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="ed211-109">[標記協助程式簡介](intro.md)描述標記協助程式提供的優點。</span><span class="sxs-lookup"><span data-stu-id="ed211-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="ed211-110">標記協助程式是實作任何類別`ITagHelper`介面。</span><span class="sxs-lookup"><span data-stu-id="ed211-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="ed211-111">不過，當您撰寫標記協助程式，您通常衍生自`TagHelper`，如此可讓您存取執行動作`Process`方法。</span><span class="sxs-lookup"><span data-stu-id="ed211-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="ed211-112">建立新的 ASP.NET Core 專案，稱為**AuthoringTagHelpers**。</span><span class="sxs-lookup"><span data-stu-id="ed211-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="ed211-113">您不需要驗證這個專案。</span><span class="sxs-lookup"><span data-stu-id="ed211-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="ed211-114">建立資料夾來存放呼叫標記協助程式*TagHelpers*。</span><span class="sxs-lookup"><span data-stu-id="ed211-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="ed211-115">*TagHelpers*資料夾是*不*必要，但它是合理的慣例。</span><span class="sxs-lookup"><span data-stu-id="ed211-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="ed211-116">現在讓我們開始撰寫一些簡單標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="ed211-117">最少的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ed211-117">A minimal Tag Helper</span></span>

<span data-ttu-id="ed211-118">在本節中，您可以撰寫更新電子郵件標籤標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="ed211-119">例如: </span><span class="sxs-lookup"><span data-stu-id="ed211-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="ed211-120">伺服器將使用我們的電子郵件標記協助程式，將該標記轉換成下列：</span><span class="sxs-lookup"><span data-stu-id="ed211-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="ed211-121">也就是錨點標籤，可讓此電子郵件 連結。</span><span class="sxs-lookup"><span data-stu-id="ed211-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="ed211-122">您可以執行這項操作，如果您要撰寫的部落格引擎，並且需要其所有以相同的網域傳送電子郵件行銷、 支援和其他連絡人。</span><span class="sxs-lookup"><span data-stu-id="ed211-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="ed211-123">加入下列`EmailTagHelper`類別*TagHelpers*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed211-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="ed211-124">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ed211-124">**Notes:**</span></span>
    
    * <span data-ttu-id="ed211-125">標記協助程式使用根類別名稱的項目為目標的命名慣例 (減去*TagHelper*類別名稱的一部分)。</span><span class="sxs-lookup"><span data-stu-id="ed211-125">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="ed211-126">在此範例中的根名稱**電子郵件**TagHelper 是*電子郵件*，因此`<email>`目標標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-126">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="ed211-127">此命名慣例應該適用於大部分的標記協助程式，我將示範如何覆寫它的更新版本上。</span><span class="sxs-lookup"><span data-stu-id="ed211-127">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="ed211-128">`EmailTagHelper` 類別衍生自 `TagHelper`。</span><span class="sxs-lookup"><span data-stu-id="ed211-128">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="ed211-129">`TagHelper`寫入標記協助程式類別提供方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="ed211-129">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="ed211-130">覆寫`Process`方法控制標記協助程式的用途時執行。</span><span class="sxs-lookup"><span data-stu-id="ed211-130">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="ed211-131">`TagHelper`類別也提供的非同步版本 (`ProcessAsync`) 具有相同參數。</span><span class="sxs-lookup"><span data-stu-id="ed211-131">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="ed211-132">內容參數`Process`(和`ProcessAsync`) 包含與目前的 HTML 標記的執行相關聯的資訊。</span><span class="sxs-lookup"><span data-stu-id="ed211-132">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="ed211-133">輸出參數`Process`(和`ProcessAsync`) 包含具狀態之 HTML 項目代表用來產生的 HTML 標記和內容的原始來源。</span><span class="sxs-lookup"><span data-stu-id="ed211-133">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="ed211-134">我們的類別名稱的後的置字元**TagHelper**，也就是*不*必要，但會被視為最佳的作法慣例。</span><span class="sxs-lookup"><span data-stu-id="ed211-134">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="ed211-135">您可以宣告為類別：</span><span class="sxs-lookup"><span data-stu-id="ed211-135">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="ed211-136">若要讓`EmailTagHelper`類別提供至所有我們 Razor 檢視中，加入`addTagHelper`指示詞加入*Views/_ViewImports.cshtml*檔案：[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="ed211-136">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="ed211-137">上述程式碼會使用的萬用字元語法來指定將可使用我們的組件中的所有標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-137">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="ed211-138">之後的第一個字串`@addTagHelper`指定載入標記協助程式 (使用"*"的所有標記協助程式)，而第二個字串"AuthoringTagHelpers 」 指定標記協助程式組件。</span><span class="sxs-lookup"><span data-stu-id="ed211-138">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="ed211-139">此外，請注意，第二行帶來中使用的萬用字元語法的 ASP.NET Core MVC 標記協助程式 (這些 helper 會討論[標記協助程式簡介](intro.md)。)它是`@addTagHelper`使標記協助程式可使用 Razor 檢視的指示詞。</span><span class="sxs-lookup"><span data-stu-id="ed211-139">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="ed211-140">或者，您可以提供完整的名稱 (FQN) 標記協助程式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed211-140">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    <span data-ttu-id="ed211-141">若要新增的檢視使用 FQN 標記協助程式，您先新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，並接著組件名稱 (*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="ed211-141">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="ed211-142">大部分的開發人員會想要使用的萬用字元語法。</span><span class="sxs-lookup"><span data-stu-id="ed211-142">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="ed211-143">[標記協助程式簡介](intro.md)進入 標記協助程式加入、 移除、 階層及萬用字元語法中的 詳細資料。</span><span class="sxs-lookup"><span data-stu-id="ed211-143">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="ed211-144">更新中的標記*Views/Home/Contact.cshtml*這些變更的檔案：</span><span class="sxs-lookup"><span data-stu-id="ed211-144">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="ed211-145">執行應用程式，並使用您最愛的瀏覽器檢視的 HTML 原始檔，以便您可以確認電子郵件標籤都會取代成錨定標記 (例如， `<a>Support</a>`)。</span><span class="sxs-lookup"><span data-stu-id="ed211-145">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="ed211-146">*支援*和*行銷*會轉譯為連結，但是沒有`href`屬性，使其功能。</span><span class="sxs-lookup"><span data-stu-id="ed211-146">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="ed211-147">我們將修正的下一節。</span><span class="sxs-lookup"><span data-stu-id="ed211-147">We'll fix that in the next section.</span></span>

<span data-ttu-id="ed211-148">注意： 如 HTML 標記和屬性、 標記、 類別名稱和 Razor，和 C# 中的屬性不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ed211-148">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="ed211-149">SetAttribute 和 SetContent</span><span class="sxs-lookup"><span data-stu-id="ed211-149">SetAttribute and SetContent</span></span>

<span data-ttu-id="ed211-150">在本節中，我們會將更新`EmailTagHelper`，這樣才會建立電子郵件的有效錨定標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-150">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="ed211-151">我們會將更新其需要從 Razor 檢視的資訊 (的形式`mail-to`屬性) 並使用該產生錨點。</span><span class="sxs-lookup"><span data-stu-id="ed211-151">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="ed211-152">更新`EmailTagHelper`具有下列類別：</span><span class="sxs-lookup"><span data-stu-id="ed211-152">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="ed211-153">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ed211-153">**Notes:**</span></span>

* <span data-ttu-id="ed211-154">依照 pascal 命名法的大小寫慣例類別和屬性名稱標記協助程式會轉譯成其[降低 kebab 案例](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。</span><span class="sxs-lookup"><span data-stu-id="ed211-154">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="ed211-155">因此，若要使用`MailTo`屬性，您將使用`<email mail-to="value"/>`相等。</span><span class="sxs-lookup"><span data-stu-id="ed211-155">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="ed211-156">最後一行會設定已完成的內容，我們使用最低限度功能標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-156">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="ed211-157">反白顯示的線條將會顯示加入屬性的語法：</span><span class="sxs-lookup"><span data-stu-id="ed211-157">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="ed211-158">該方法的效果 」 href"的屬性，只要它目前並不存在於屬性集合。</span><span class="sxs-lookup"><span data-stu-id="ed211-158">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="ed211-159">您也可以使用`output.Attributes.Add`方法，以加入標記屬性集合的結尾標記協助程式屬性。</span><span class="sxs-lookup"><span data-stu-id="ed211-159">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="ed211-160">更新中的標記*Views/Home/Contact.cshtml*這些變更的檔案：[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="ed211-160">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="ed211-161">執行應用程式，並確認它會產生正確的連結。</span><span class="sxs-lookup"><span data-stu-id="ed211-161">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="ed211-162">如果您要撰寫電子郵件標籤自我結尾 (`<email mail-to="Rick" />`)，最後的輸出也會自我結尾。</span><span class="sxs-lookup"><span data-stu-id="ed211-162">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="ed211-163">若要啟用寫入將標記與開始標記的能力 (`<email mail-to="Rick">`) 您也必須裝飾具有下列類別：</span><span class="sxs-lookup"><span data-stu-id="ed211-163">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="ed211-164">具有自我結尾的電子郵件標記協助程式，輸出會`<a href="mailto:Rick@contoso.com" />`。</span><span class="sxs-lookup"><span data-stu-id="ed211-164">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="ed211-165">自我結尾的錨定標記不是有效的 HTML，因此您不想要建立一個，但是您可能想要建立已自我結尾標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-165">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="ed211-166">標記協助程式設定的類型`TagMode`讀取標記之後的屬性。</span><span class="sxs-lookup"><span data-stu-id="ed211-166">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="ed211-167">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="ed211-167">ProcessAsync</span></span>

<span data-ttu-id="ed211-168">本節中，我們會撰寫的非同步的電子郵件協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-168">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="ed211-169">取代`EmailTagHelper`類別為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ed211-169">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="ed211-170">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ed211-170">**Notes:**</span></span>

    * <span data-ttu-id="ed211-171">此版本使用非同步`ProcessAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ed211-171">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="ed211-172">非同步`GetChildContentAsync`傳回`Task`包含`TagHelperContent`。</span><span class="sxs-lookup"><span data-stu-id="ed211-172">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="ed211-173">使用`output`參數，以取得 HTML 項目的內容。</span><span class="sxs-lookup"><span data-stu-id="ed211-173">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="ed211-174">請進行下列變更*Views/Home/Contact.cshtml*檔案標記協助程式可以取得目標電子郵件。</span><span class="sxs-lookup"><span data-stu-id="ed211-174">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="ed211-175">執行應用程式，並確認它會產生有效的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="ed211-175">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="ed211-176">RemoveAll、 PreContent.SetHtmlContent 和 PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="ed211-176">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="ed211-177">加入下列`BoldTagHelper`類別*TagHelpers*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed211-177">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="ed211-178">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ed211-178">**Notes:**</span></span>
    
    * <span data-ttu-id="ed211-179">`[HtmlTargetElement]`屬性的將屬性參數會指定任何包含的 HTML 屬性的 HTML 項目名稱為"bold"會比對，而`Process`類別中的覆寫方法會執行。</span><span class="sxs-lookup"><span data-stu-id="ed211-179">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="ed211-180">在我們的範例，`Process`方法移除 「 粗體 」 屬性，而且括住包含標記與`<strong></strong>`。</span><span class="sxs-lookup"><span data-stu-id="ed211-180">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="ed211-181">因為您不想要取代現有的標籤內容，您必須撰寫開啟`<strong>`標記`PreContent.SetHtmlContent`方法和關閉`</strong>`標記`PostContent.SetHtmlContent`方法。</span><span class="sxs-lookup"><span data-stu-id="ed211-181">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="ed211-182">修改*About.cshtml*檢視，以包含`bold`屬性值。</span><span class="sxs-lookup"><span data-stu-id="ed211-182">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="ed211-183">已完成的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="ed211-183">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="ed211-184">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-184">Run the app.</span></span> <span data-ttu-id="ed211-185">您可以使用您最愛的瀏覽器檢查來源，並確認標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-185">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="ed211-186">`[HtmlTargetElement]`上述的屬性只能為目標的 HTML 標記，提供的 「 粗體 」 的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="ed211-186">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="ed211-187">`<bold>`標記協助程式 」 所未修改項目。</span><span class="sxs-lookup"><span data-stu-id="ed211-187">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="ed211-188">標記為註解`[HtmlTargetElement]`屬性行，它會預設為目標`<bold>`標記，也就是表單的 HTML 標記`<bold>`。</span><span class="sxs-lookup"><span data-stu-id="ed211-188">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="ed211-189">請記住，預設命名慣例將符合類別名稱**粗體**至 TagHelper`<bold>`標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-189">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="ed211-190">執行應用程式，並確認`<bold>`標記協助程式 」 所處理標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-190">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="ed211-191">裝飾的類別具有多個`[HtmlTargetElement]`屬性的目標在邏輯 OR 結果。</span><span class="sxs-lookup"><span data-stu-id="ed211-191">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="ed211-192">例如，使用下列程式碼，粗體顯示標記或粗體屬性會比對。</span><span class="sxs-lookup"><span data-stu-id="ed211-192">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="ed211-193">當多個屬性加入至相同的陳述式時，執行階段會將它們視為邏輯 and。</span><span class="sxs-lookup"><span data-stu-id="ed211-193">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="ed211-194">例如，下列程式碼，HTML 項目必須命名為"bold"與屬性名稱為"bold"(`<bold bold />`) 來比對。</span><span class="sxs-lookup"><span data-stu-id="ed211-194">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="ed211-195">您也可以使用`[HtmlTargetElement]`若要變更目標項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="ed211-195">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="ed211-196">例如，如果您想`BoldTagHelper`目標`<MyBold>`標記，您可以使用下列屬性：</span><span class="sxs-lookup"><span data-stu-id="ed211-196">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="ed211-197">將模型傳遞至標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ed211-197">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="ed211-198">新增*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed211-198">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="ed211-199">將下列 `WebsiteContext` 類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="ed211-199">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="ed211-200">加入下列`WebsiteInformationTagHelper`類別*TagHelpers*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed211-200">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="ed211-201">**注意：**</span><span class="sxs-lookup"><span data-stu-id="ed211-201">**Notes:**</span></span>
    
    * <span data-ttu-id="ed211-202">如前所述，標記協助程式將使用 Pascal 大小寫 C# 類別名稱和屬性為標記協助程式至[降低 kebab 案例](http://wiki.c2.com/?KebabCase)。</span><span class="sxs-lookup"><span data-stu-id="ed211-202">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="ed211-203">因此，若要使用`WebsiteInformationTagHelper`Razor，在您將撰寫`<website-information />`。</span><span class="sxs-lookup"><span data-stu-id="ed211-203">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="ed211-204">明確地不識別目標項目與`[HtmlTargetElement]`屬性，因此預設值是`website-information`的目標。</span><span class="sxs-lookup"><span data-stu-id="ed211-204">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="ed211-205">如果您套用下列屬性 （請注意不 kebab 這種情況，但符合類別名稱）：</span><span class="sxs-lookup"><span data-stu-id="ed211-205">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="ed211-206">較低的 kebab case 標籤`<website-information />`也不會相符。</span><span class="sxs-lookup"><span data-stu-id="ed211-206">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="ed211-207">如果您想使用`[HtmlTargetElement]`屬性，您會使用 kebab 案例，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed211-207">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="ed211-208">不能自我結尾的項目有內容。</span><span class="sxs-lookup"><span data-stu-id="ed211-208">Elements that are self-closing have no content.</span></span> <span data-ttu-id="ed211-209">此範例中，Razor 標記將會使用自我結尾標記，但標記協助程式會建立[區段](http://www.w3.org/TR/html5/sections.html#the-section-element)項目 (不是自我結尾，而且您要撰寫內部內容`section`項目)。</span><span class="sxs-lookup"><span data-stu-id="ed211-209">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="ed211-210">因此，您需要設定`TagMode`至`StartTagAndEndTag`來寫入輸出。</span><span class="sxs-lookup"><span data-stu-id="ed211-210">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="ed211-211">或者，您可以註解線條設定`TagMode`和寫入結尾標記的標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-211">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="ed211-212">（稍後在本教學課程會提供範例標記）。</span><span class="sxs-lookup"><span data-stu-id="ed211-212">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="ed211-213">`$` （貨幣符號） 下面這一行會使用[以內插值取代字串](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="ed211-213">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="ed211-214">加入下列標記以*About.cshtml*檢視。</span><span class="sxs-lookup"><span data-stu-id="ed211-214">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="ed211-215">反白顯示的標記會顯示網站的資訊。</span><span class="sxs-lookup"><span data-stu-id="ed211-215">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="ed211-216">在 Razor 標記，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed211-216">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="ed211-217">Razor 知道`info`屬性類別，而不是字串，並且您想要撰寫 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="ed211-217">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="ed211-218">任何非字串標記協助程式屬性應該寫入不含`@`字元。</span><span class="sxs-lookup"><span data-stu-id="ed211-218">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="ed211-219">執行應用程式，並瀏覽到關於檢視來查看網站資訊。</span><span class="sxs-lookup"><span data-stu-id="ed211-219">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="ed211-220">您可以使用下列標記與結束標記，並移除那一行`TagMode.StartTagAndEndTag`標記協助程式中：</span><span class="sxs-lookup"><span data-stu-id="ed211-220">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="ed211-221">條件標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ed211-221">Condition Tag Helper</span></span>

<span data-ttu-id="ed211-222">條件標記協助程式轉譯輸出時，則為 true 的值傳遞。</span><span class="sxs-lookup"><span data-stu-id="ed211-222">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="ed211-223">加入下列`ConditionTagHelper`類別*TagHelpers*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed211-223">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="ed211-224">取代內容*Views/Home/Index.cshtml*檔案以下列標記：</span><span class="sxs-lookup"><span data-stu-id="ed211-224">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="ed211-225">取代`Index`方法中的`Home`控制器會執行下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ed211-225">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="ed211-226">執行應用程式，並瀏覽至首頁。</span><span class="sxs-lookup"><span data-stu-id="ed211-226">Run the app and browse to the home page.</span></span> <span data-ttu-id="ed211-227">在條件中的標記`div`將不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="ed211-227">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="ed211-228">將查詢字串附加`?approved=true`的 url (例如， `http://localhost:1235/Home/Index?approved=true`)。</span><span class="sxs-lookup"><span data-stu-id="ed211-228">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="ed211-229">`approved`設定為 true，且條件會顯示標記。</span><span class="sxs-lookup"><span data-stu-id="ed211-229">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="ed211-230">使用[nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)運算子，來指定目標，而不是指定字串，您可以如同使用粗體顯示標記協助程式屬性：</span><span class="sxs-lookup"><span data-stu-id="ed211-230">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="ed211-231">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)運算子將保護的程式碼應該它曾經重構 (我們可能會想要將名稱變更為`RedCondition`)。</span><span class="sxs-lookup"><span data-stu-id="ed211-231">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="ed211-232">避免衝突標記協助程式</span><span class="sxs-lookup"><span data-stu-id="ed211-232">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="ed211-233">本節中，您可以撰寫一組的自動連結標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-233">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="ed211-234">第一個將會取代標記包含 HTML 錨定標記包含相同的 URL （並因而產生連結至 URL） 從 HTTP URL。</span><span class="sxs-lookup"><span data-stu-id="ed211-234">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="ed211-235">第二個會執行相同動作 URL 的開頭為 WWW。</span><span class="sxs-lookup"><span data-stu-id="ed211-235">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="ed211-236">由於這些兩個 helper 密切相關，而且可能重構它們在未來，我們會讓它們保持在相同的檔案。</span><span class="sxs-lookup"><span data-stu-id="ed211-236">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="ed211-237">加入下列`AutoLinkerHttpTagHelper`類別*TagHelpers*資料夾。</span><span class="sxs-lookup"><span data-stu-id="ed211-237">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="ed211-238">`AutoLinkerHttpTagHelper`類別目標`p`項目，並使用[Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)建立錨點。</span><span class="sxs-lookup"><span data-stu-id="ed211-238">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="ed211-239">加入下列標記的結尾*Views/Home/Contact.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="ed211-239">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="ed211-240">執行應用程式，並確認標記協助程式會正確轉譯錨點。</span><span class="sxs-lookup"><span data-stu-id="ed211-240">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="ed211-241">更新`AutoLinker`類別，以包含`AutoLinkerWwwTagHelper`這會將 www 文字轉換也包含原始 www 文字的錨點標籤。</span><span class="sxs-lookup"><span data-stu-id="ed211-241">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="ed211-242">更新的程式碼會反白顯示，如下：</span><span class="sxs-lookup"><span data-stu-id="ed211-242">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="ed211-243">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-243">Run the app.</span></span> <span data-ttu-id="ed211-244">請注意 www 文字呈現為連結，但不是 HTTP 文字。</span><span class="sxs-lookup"><span data-stu-id="ed211-244">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="ed211-245">如果您將中斷點放在這兩個類別時，您可以看到第一個 HTTP 標記協助程式類別執行程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-245">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="ed211-246">快取標記協助程式輸出，而 WWW 標記協助程式執行時，它會覆寫快取的輸出從 HTTP 標記協助程式問題。</span><span class="sxs-lookup"><span data-stu-id="ed211-246">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="ed211-247">稍後在教學課程中，我們會看到如何控制標記協助程式執行中的順序。</span><span class="sxs-lookup"><span data-stu-id="ed211-247">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="ed211-248">我們將修正程式碼以下列：</span><span class="sxs-lookup"><span data-stu-id="ed211-248">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="ed211-249">在第一個版本中的自動連結標記協助程式，您會取得以下列程式碼目標的內容：</span><span class="sxs-lookup"><span data-stu-id="ed211-249">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="ed211-250">也就是說，呼叫`GetChildContentAsync`使用`TagHelperOutput`傳入`ProcessAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="ed211-250">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="ed211-251">如所述以前，因為快取輸出，則最後一個標記協助程式 」 執行 wins。</span><span class="sxs-lookup"><span data-stu-id="ed211-251">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="ed211-252">您已修正此問題，下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ed211-252">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="ed211-253">上述程式碼會檢查已修改的內容，如果有，從輸出緩衝區取得內容。</span><span class="sxs-lookup"><span data-stu-id="ed211-253">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="ed211-254">執行應用程式，並確認兩個連結在如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="ed211-254">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="ed211-255">雖然可能會顯示為我們自動連結器標記協助程式即會正確且完整，它有微妙的問題。</span><span class="sxs-lookup"><span data-stu-id="ed211-255">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="ed211-256">如果 WWW 標記協助程式會先執行，將無法正確 www 連結。</span><span class="sxs-lookup"><span data-stu-id="ed211-256">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="ed211-257">更新程式碼加入`Order`控制標記以執行順序的多載。</span><span class="sxs-lookup"><span data-stu-id="ed211-257">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="ed211-258">`Order`屬性會決定相對於其他標記協助程式，以相同的項目為目標的執行順序。</span><span class="sxs-lookup"><span data-stu-id="ed211-258">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="ed211-259">預設順序值為 0，且執行個體具有較低值會優先執行。</span><span class="sxs-lookup"><span data-stu-id="ed211-259">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="ed211-260">上述程式碼將會保證 HTTP 標記協助程式執行之前 WWW 標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="ed211-260">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="ed211-261">變更`Order`至`MaxValue`並確認產生 WWW 標記的標記不正確。</span><span class="sxs-lookup"><span data-stu-id="ed211-261">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="ed211-262">檢查及擷取子內容</span><span class="sxs-lookup"><span data-stu-id="ed211-262">Inspecting and retrieving child content</span></span>

<span data-ttu-id="ed211-263">標記協助程式提供數個屬性，擷取內容。</span><span class="sxs-lookup"><span data-stu-id="ed211-263">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="ed211-264">結果`GetChildContentAsync`可以附加至`output.Content`。</span><span class="sxs-lookup"><span data-stu-id="ed211-264">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="ed211-265">您可以檢查的結果`GetChildContentAsync`與`GetContent`。</span><span class="sxs-lookup"><span data-stu-id="ed211-265">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="ed211-266">如果您修改`output.Content`，不會執行或轉譯，除非您呼叫 TagHelper 主體`GetChildContentAsync`如我們自動連結器範例所示：</span><span class="sxs-lookup"><span data-stu-id="ed211-266">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="ed211-267">多個呼叫`GetChildContentAsync`會傳回相同的值，而不會重新執行`TagHelper`主體，除非您參數中傳遞 false 表示不使用快取的結果。</span><span class="sxs-lookup"><span data-stu-id="ed211-267">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
