---
title: ASP.NET Core 中的編寫標籤協助程式
author: rick-anderson
description: 了解如何在 ASP.NET Core 中編寫標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: c21decd39b7855cf2eefb2bb482e5e91b9487863
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889934"
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="edfcb-103">ASP.NET Core 中的編寫標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="edfcb-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="edfcb-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="edfcb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="edfcb-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="edfcb-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="edfcb-106">開始使用標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="edfcb-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="edfcb-107">本教學課程介紹如何以程式設計標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="edfcb-108">[標籤協助程式簡介](intro.md)描述標籤協助程式所提供的優點。</span><span class="sxs-lookup"><span data-stu-id="edfcb-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="edfcb-109">標籤協助程式是任何可實作 `ITagHelper` 介面的類別。</span><span class="sxs-lookup"><span data-stu-id="edfcb-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="edfcb-110">不過，當您編寫標籤協助程式時，通常是衍生自 `TagHelper`，如此可讓您存取 `Process` 方法。</span><span class="sxs-lookup"><span data-stu-id="edfcb-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="edfcb-111">建立稱為 **AuthoringTagHelpers** 的新 ASP.NET Core 專案。</span><span class="sxs-lookup"><span data-stu-id="edfcb-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="edfcb-112">您不需要驗證此專案。</span><span class="sxs-lookup"><span data-stu-id="edfcb-112">You won't need authentication for this project.</span></span>

1. <span data-ttu-id="edfcb-113">建立資料夾以保存稱為 *TagHelpers* 的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="edfcb-114">*TagHelpers* 資料夾「不」是必要的，但是為合理的慣例。</span><span class="sxs-lookup"><span data-stu-id="edfcb-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="edfcb-115">現在開始撰寫一些簡單的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="edfcb-116">最精簡的標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="edfcb-116">A minimal Tag Helper</span></span>

<span data-ttu-id="edfcb-117">在本節中，您會撰寫可更新電子郵件標籤的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="edfcb-118">例如：</span><span class="sxs-lookup"><span data-stu-id="edfcb-118">For example:</span></span>

```html
<email>Support</email>
```

<span data-ttu-id="edfcb-119">伺服器將使用我們的電子郵件標籤 (tag) 協助程式，將該標籤 (markup) 轉換成下列項目：</span><span class="sxs-lookup"><span data-stu-id="edfcb-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="edfcb-120">也就是，將此設為電子郵件連結的錨點標籤。</span><span class="sxs-lookup"><span data-stu-id="edfcb-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="edfcb-121">如果您要撰寫部落格引擎，並且需要它才能傳送行銷、支援和其他連絡人 (全都位在相同的網域) 的電子郵件，則可能會想要執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="edfcb-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="edfcb-122">將下列 `EmailTagHelper` 類別新增至 *TagHelpers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * <span data-ttu-id="edfcb-123">標籤協助程式使用以根類別名稱的項目為目標的命名慣例 (去掉類別名稱的 *TagHelper* 部分)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-123">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="edfcb-124">在此範例中，**EmailTagHelper** 的根名稱是 *email*，因此將會以 `<email>` 標籤為目標。</span><span class="sxs-lookup"><span data-stu-id="edfcb-124">In this example, the root name of **EmailTagHelper** is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="edfcb-125">此命名慣例應該適用於大部分的標籤協助程式，稍後將示範如何行覆寫它。</span><span class="sxs-lookup"><span data-stu-id="edfcb-125">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>

   * <span data-ttu-id="edfcb-126">`EmailTagHelper` 類別衍生自 `TagHelper`。</span><span class="sxs-lookup"><span data-stu-id="edfcb-126">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="edfcb-127">`TagHelper` 類別提供撰寫標籤協助程式的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="edfcb-127">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>

   * <span data-ttu-id="edfcb-128">覆寫的 `Process` 方法控制標籤協助程式在其執行時的作用。</span><span class="sxs-lookup"><span data-stu-id="edfcb-128">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="edfcb-129">`TagHelper` 類別也會使用相同的參數來提供非同步版本 (`ProcessAsync`)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-129">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>

   * <span data-ttu-id="edfcb-130">`Process` (和 `ProcessAsync`) 的內容參數包含與目前 HTML 標籤執行建立關聯的資訊。</span><span class="sxs-lookup"><span data-stu-id="edfcb-130">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>

   * <span data-ttu-id="edfcb-131">`Process` (和 `ProcessAsync`) 的輸出參數包含具狀態 HTML 項目，以呈現用來產生 HTML 標籤和內容的原始來源。</span><span class="sxs-lookup"><span data-stu-id="edfcb-131">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>

   * <span data-ttu-id="edfcb-132">類別名稱的尾碼為 **TagHelper**，這「不」是必要的，但視為最佳做法慣例。</span><span class="sxs-lookup"><span data-stu-id="edfcb-132">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="edfcb-133">您可以將類別宣告為：</span><span class="sxs-lookup"><span data-stu-id="edfcb-133">You could declare the class as:</span></span>

   ```csharp
   public class Email : TagHelper
   ```

1. <span data-ttu-id="edfcb-134">若要讓 `EmailTagHelper` 類別可供所有 Razor 檢視使用，請將 `addTagHelper` 指示詞新增至 *Views/_ViewImports.cshtml* 檔案：[!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="edfcb-134">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>

   <span data-ttu-id="edfcb-135">上述程式碼使用萬用字元語法，指定將使用組件中的所有標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-135">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="edfcb-136">`@addTagHelper` 後面的第一個字串指定要載入的標籤協助程式 (使用 "\*" 表示所有標籤協助程式)，而第二個字串 "AuthoringTagHelpers" 指定標籤協助程式所在的組件。</span><span class="sxs-lookup"><span data-stu-id="edfcb-136">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="edfcb-137">另請注意，第二行使用萬用字元語法帶入 ASP.NET Core MVC 標籤協助程式 ([標籤協助程式簡介](intro.md)會討論這些協助程式)。它是 `@addTagHelper` 指示詞，讓標籤協助程式可供 Razor 檢視使用。</span><span class="sxs-lookup"><span data-stu-id="edfcb-137">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="edfcb-138">或者，您可以提供標籤協助程式的完整名稱 (FQN)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="edfcb-138">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

<span data-ttu-id="edfcb-139">若要使用 FQN 將標籤協助程式新增至檢視，請依序新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 與**組件名稱** (*AuthoringTagHelpers*，它不一定是 `namespace`)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-139">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the **assembly name** (*AuthoringTagHelpers*, not necessarly the `namespace`).</span></span> <span data-ttu-id="edfcb-140">大部分開發人員都會想要使用萬用字元語法。</span><span class="sxs-lookup"><span data-stu-id="edfcb-140">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="edfcb-141">[標籤協助程式簡介](intro.md)會詳述標籤協助程式新增、移除、階層和萬用字元語法。</span><span class="sxs-lookup"><span data-stu-id="edfcb-141">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>

1. <span data-ttu-id="edfcb-142">使用下列變更來更新 *Views/Home/Contact.cshtml* 檔案中的標記：</span><span class="sxs-lookup"><span data-stu-id="edfcb-142">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="edfcb-143">執行應用程式，並使用慣用的瀏覽器來檢視 HTML 原始檔，讓您可以驗證電子郵件標籤已取代為錨點標籤 (例如，`<a>Support</a>`)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-143">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="edfcb-144">*Support* 和 *Marketing* 會轉譯為連結，但沒有 `href` 屬性可讓它們運作。</span><span class="sxs-lookup"><span data-stu-id="edfcb-144">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="edfcb-145">我們將在下節修正該問題。</span><span class="sxs-lookup"><span data-stu-id="edfcb-145">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="edfcb-146">SetAttribute 和 SetContent</span><span class="sxs-lookup"><span data-stu-id="edfcb-146">SetAttribute and SetContent</span></span>

<span data-ttu-id="edfcb-147">在本節中，我們將更新 `EmailTagHelper`，以建立電子郵件的有效錨點標籤。</span><span class="sxs-lookup"><span data-stu-id="edfcb-147">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="edfcb-148">我們將會更新它，以從 Razor 檢視取得資訊 (以 `mail-to` 屬性形式)，並使用這項資訊來產生錨點。</span><span class="sxs-lookup"><span data-stu-id="edfcb-148">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="edfcb-149">使用下列程式碼更新 `EmailTagHelper` 類別：</span><span class="sxs-lookup"><span data-stu-id="edfcb-149">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* <span data-ttu-id="edfcb-150">標籤協助程式依照 Pascal 命名法大小寫慣例的類別和屬性名稱會轉譯成其[小寫 kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-150">Pascal-cased class and property names for tag helpers are translated into their [kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="edfcb-151">因此，若要使用 `MailTo` 屬性，您將使用 `<email mail-to="value"/>` 對等項目。</span><span class="sxs-lookup"><span data-stu-id="edfcb-151">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="edfcb-152">最後一行設定功能最小之標籤協助程式的已完成內容。</span><span class="sxs-lookup"><span data-stu-id="edfcb-152">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="edfcb-153">反白顯示的線條會顯示新增屬性的語法：</span><span class="sxs-lookup"><span data-stu-id="edfcb-153">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="edfcb-154">只要 attributes 集合中目前沒有 "href" 屬性，該方法就適用於 "href" 屬性。</span><span class="sxs-lookup"><span data-stu-id="edfcb-154">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="edfcb-155">您也可以使用 `output.Attributes.Add` 方法，將標籤協助程式屬性新增至標籤屬性集合結尾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-155">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="edfcb-156">使用下列變更來更新 *Views/Home/Contact.cshtml* 檔案中的標記：[!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="edfcb-156">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

1. <span data-ttu-id="edfcb-157">執行應用程式，並驗證它會產生正確的連結。</span><span class="sxs-lookup"><span data-stu-id="edfcb-157">Run the app and verify that it generates the correct links.</span></span>

<a name="self-closing"></a>

   > [!NOTE]
   > <span data-ttu-id="edfcb-158">如果您要撰寫電子郵件標籤自我結尾 (`<email mail-to="Rick" />`)，則最終輸出也會是自我結尾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-158">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="edfcb-159">若要啟用可以寫入只有開始標籤的標籤 (`<email mail-to="Rick">`)，您必須以下列項目來裝飾此類別：</span><span class="sxs-lookup"><span data-stu-id="edfcb-159">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   <span data-ttu-id="edfcb-160">使用自我結尾的電子郵件標籤協助程式時，輸出會是 `<a href="mailto:Rick@contoso.com" />`。</span><span class="sxs-lookup"><span data-stu-id="edfcb-160">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="edfcb-161">自我結尾的錨點標籤不是有效的 HTML，因此您不會想要建立它；但您可能會想要建立自我結尾的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-161">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="edfcb-162">標籤協助程式會在讀取標籤之後設定 `TagMode` 屬性的類型。</span><span class="sxs-lookup"><span data-stu-id="edfcb-162">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>

### <a name="processasync"></a><span data-ttu-id="edfcb-163">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="edfcb-163">ProcessAsync</span></span>

<span data-ttu-id="edfcb-164">在本節中，我們將撰寫非同步電子郵件協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-164">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="edfcb-165">將 `EmailTagHelper` 類別取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="edfcb-165">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="edfcb-166">**注意：**</span><span class="sxs-lookup"><span data-stu-id="edfcb-166">**Notes:**</span></span>

   * <span data-ttu-id="edfcb-167">此版本使用非同步 `ProcessAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="edfcb-167">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="edfcb-168">非同步 `GetChildContentAsync` 會傳回包含 `TagHelperContent` 的 `Task`。</span><span class="sxs-lookup"><span data-stu-id="edfcb-168">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="edfcb-169">使用 `output` 參數，以取得 HTML 項目的內容。</span><span class="sxs-lookup"><span data-stu-id="edfcb-169">Use the `output` parameter to get contents of the HTML element.</span></span>

1. <span data-ttu-id="edfcb-170">對 *Views/Home/Contact.cshtml* 檔案進行下列變更，讓標籤協助程式可以取得目標電子郵件。</span><span class="sxs-lookup"><span data-stu-id="edfcb-170">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. <span data-ttu-id="edfcb-171">執行應用程式，並驗證它產生有效的電子郵件連結。</span><span class="sxs-lookup"><span data-stu-id="edfcb-171">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="edfcb-172">RemoveAll、PreContent.SetHtmlContent 和 PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="edfcb-172">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="edfcb-173">將下列 `BoldTagHelper` 類別新增至 *TagHelpers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-173">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * <span data-ttu-id="edfcb-174">`[HtmlTargetElement]` 屬性會傳遞屬性參數，以指定是否有任何包含名為 "bold" 之 HTML 屬性的 HTML 項目相符，並且執行類別中的 `Process` 覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="edfcb-174">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="edfcb-175">在我們的範例中，`Process` 方法會移除 "bold" 屬性，並使用 `<strong></strong>` 括住包含標記。</span><span class="sxs-lookup"><span data-stu-id="edfcb-175">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>

   * <span data-ttu-id="edfcb-176">因為您不想要取代現有標籤內容，所以必須使用 `PreContent.SetHtmlContent` 方法來撰寫開頭 `<strong>` 標籤，並使用 `PostContent.SetHtmlContent` 方法來撰寫結尾 `</strong>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="edfcb-176">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>

1. <span data-ttu-id="edfcb-177">修改 *About.cshtml* 檢視，以包含 `bold` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="edfcb-177">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="edfcb-178">已完成的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="edfcb-178">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. <span data-ttu-id="edfcb-179">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-179">Run the app.</span></span> <span data-ttu-id="edfcb-180">您可以使用慣用的瀏覽器來檢查原始檔，並驗證標記。</span><span class="sxs-lookup"><span data-stu-id="edfcb-180">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="edfcb-181">上面的 `[HtmlTargetElement]` 屬性只會將目標設為提供屬性名稱 "bold" 的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="edfcb-181">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="edfcb-182">標籤協助程式未曾修改 `<bold>` 項目。</span><span class="sxs-lookup"><span data-stu-id="edfcb-182">The `<bold>` element wasn't modified by the tag helper.</span></span>

1. <span data-ttu-id="edfcb-183">將 `[HtmlTargetElement]` 屬性行標籤為註解，而且它會預設為目標 `<bold>` 標籤 (tag)，也就是表單 `<bold>` 的 HTML 標記 (markup)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-183">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="edfcb-184">請記住，預設命名慣例將符合類別名稱 **Bold**TagHelper 與 `<bold>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="edfcb-184">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

1. <span data-ttu-id="edfcb-185">執行應用程式，並驗證標籤協助程式已處理 `<bold>` 標籤。</span><span class="sxs-lookup"><span data-stu-id="edfcb-185">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="edfcb-186">以多個 `[HtmlTargetElement]` 屬性裝飾類別，會導致目標的邏輯 OR。</span><span class="sxs-lookup"><span data-stu-id="edfcb-186">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="edfcb-187">例如，使用下列程式碼，粗體標籤或粗體屬性會相符。</span><span class="sxs-lookup"><span data-stu-id="edfcb-187">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="edfcb-188">將多個屬性新增至相同的陳述式時，執行階段會將它們視為邏輯 AND。</span><span class="sxs-lookup"><span data-stu-id="edfcb-188">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="edfcb-189">例如，在下列程式碼中，HTML 項目必須命名為具有 "bold" 屬性的 "bold" (`<bold bold />`) 才能相符。</span><span class="sxs-lookup"><span data-stu-id="edfcb-189">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="edfcb-190">您也可以使用 `[HtmlTargetElement]` 來變更目標項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="edfcb-190">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="edfcb-191">例如，如果您想要 `BoldTagHelper` 將目標設為 `<MyBold>` 標籤，您可以使用下列屬性：</span><span class="sxs-lookup"><span data-stu-id="edfcb-191">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="edfcb-192">將模型傳遞至標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="edfcb-192">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="edfcb-193">新增 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-193">Add a *Models* folder.</span></span>

1. <span data-ttu-id="edfcb-194">將下列 `WebsiteContext` 類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="edfcb-194">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. <span data-ttu-id="edfcb-195">將下列 `WebsiteInformationTagHelper` 類別新增至 *TagHelpers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-195">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * <span data-ttu-id="edfcb-196">如前所述，標籤協助程式會將標籤協助程式依照 Pascal 大小寫 C# 命名法大小寫慣例的類別名稱和屬性轉譯成[小寫 kebab](http://wiki.c2.com/?KebabCase)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-196">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="edfcb-197">因此，若要在 Razor 中使用 `WebsiteInformationTagHelper`，您將撰寫 `<website-information />`。</span><span class="sxs-lookup"><span data-stu-id="edfcb-197">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>

   * <span data-ttu-id="edfcb-198">您未明確識別具有 `[HtmlTargetElement]` 屬性的目標項目，因此會將預設值 `website-information` 設為目標。</span><span class="sxs-lookup"><span data-stu-id="edfcb-198">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="edfcb-199">如果您已套用下列屬性 (請注意，它不是 Kebab 大小寫，但符合類別名稱)：</span><span class="sxs-lookup"><span data-stu-id="edfcb-199">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   <span data-ttu-id="edfcb-200">小寫 kebab 標籤 `<website-information />` 將不相符。</span><span class="sxs-lookup"><span data-stu-id="edfcb-200">The kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="edfcb-201">如果您想要使用 `[HtmlTargetElement]` 屬性，請使用 Kebab 大小寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="edfcb-201">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * <span data-ttu-id="edfcb-202">自我結尾項目沒有內容。</span><span class="sxs-lookup"><span data-stu-id="edfcb-202">Elements that are self-closing have no content.</span></span> <span data-ttu-id="edfcb-203">在此範例中，Razor 標記 (markup) 將使用自我結尾標籤 (tag)，但標籤 (tag) 協助程式將會建立 [section](http://www.w3.org/TR/html5/sections.html#the-section-element) 項目 (此項目不是自我結尾的，而且您將在 `section` 項目內撰寫內容)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-203">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="edfcb-204">因此，您需要將 `TagMode` 設定為 `StartTagAndEndTag`，才能撰寫輸出。</span><span class="sxs-lookup"><span data-stu-id="edfcb-204">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="edfcb-205">或者，您可以將設定 `TagMode` 的行設定為註解，並撰寫含結尾標籤 (tag) 的標籤 (markup) </span><span class="sxs-lookup"><span data-stu-id="edfcb-205">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="edfcb-206">(本教學課程稍後會提供範例標記)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-206">(Example markup is provided later in this tutorial.)</span></span>

   * <span data-ttu-id="edfcb-207">下行中的 `$` (貨幣符號) 使用[插入字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)：</span><span class="sxs-lookup"><span data-stu-id="edfcb-207">The `$` (dollar sign) in the following line uses an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. <span data-ttu-id="edfcb-208">將下列標記新增至 *About.cshtml* 檢視。</span><span class="sxs-lookup"><span data-stu-id="edfcb-208">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="edfcb-209">反白顯示的標記會顯示網站資訊。</span><span class="sxs-lookup"><span data-stu-id="edfcb-209">The highlighted markup displays the web site information.</span></span>

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]

   > [!NOTE]
   > <span data-ttu-id="edfcb-210">在下面所顯示的 Razor 標記中：</span><span class="sxs-lookup"><span data-stu-id="edfcb-210">In the Razor markup shown below:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   >
   > <span data-ttu-id="edfcb-211">Razor 知道 `info` 屬性是類別，而非字串，而且您想要撰寫 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="edfcb-211">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="edfcb-212">應該撰寫任何無 `@` 字元的非字串標籤協助程式屬性。</span><span class="sxs-lookup"><span data-stu-id="edfcb-212">Any non-string tag helper attribute should be written without the `@` character.</span></span>

1. <span data-ttu-id="edfcb-213">執行應用程式，並巡覽至 About 檢視來查看網站資訊。</span><span class="sxs-lookup"><span data-stu-id="edfcb-213">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="edfcb-214">您可以搭配使用下列標籤 (markup) 與結尾標籤 (tag)，並移除標籤 (tag) 協助程式中包含 `TagMode.StartTagAndEndTag` 的行：</span><span class="sxs-lookup"><span data-stu-id="edfcb-214">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="edfcb-215">條件標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="edfcb-215">Condition Tag Helper</span></span>

<span data-ttu-id="edfcb-216">傳遞 true 值時，條件標籤協助程式會轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="edfcb-216">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="edfcb-217">將下列 `ConditionTagHelper` 類別新增至 *TagHelpers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-217">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. <span data-ttu-id="edfcb-218">將 *Views/Home/Index.cshtml* 檔案的內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="edfcb-218">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. <span data-ttu-id="edfcb-219">將 `Home` 控制器中的 `Index` 方法取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="edfcb-219">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. <span data-ttu-id="edfcb-220">執行應用程式，並瀏覽至首頁。</span><span class="sxs-lookup"><span data-stu-id="edfcb-220">Run the app and browse to the home page.</span></span> <span data-ttu-id="edfcb-221">將不會轉譯條件式 `div` 中的標記。</span><span class="sxs-lookup"><span data-stu-id="edfcb-221">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="edfcb-222">將查詢字串 `?approved=true` 附加至 URL (例如，`http://localhost:1235/Home/Index?approved=true`)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-222">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="edfcb-223">`approved` 設定為 true，並且顯示條件式標記。</span><span class="sxs-lookup"><span data-stu-id="edfcb-223">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="edfcb-224">使用 [nameof](/dotnet/csharp/language-reference/keywords/nameof) 運算子來指定要設為目標的屬性，而不是指定字串，就像使用粗體標籤協助程式一樣：</span><span class="sxs-lookup"><span data-stu-id="edfcb-224">Use the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> <span data-ttu-id="edfcb-225">[nameof](/dotnet/csharp/language-reference/keywords/nameof) 運算子將會保護已重構的程式碼 (我們可能會想要將名稱變更為 `RedCondition`)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-225">The [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="edfcb-226">避免標籤協助程式衝突</span><span class="sxs-lookup"><span data-stu-id="edfcb-226">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="edfcb-227">在本節中，您會撰寫一對自動連結標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-227">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="edfcb-228">第一個會將包含開頭為 HTTP 之 URL 的標籤 (markup) 取代為包含相同 URL 的 HTML 錨點標籤 (tag) (因此產生 URL 的連結)。</span><span class="sxs-lookup"><span data-stu-id="edfcb-228">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="edfcb-229">第二個會針對開頭為 WWW 的 URL 執行相同的動作。</span><span class="sxs-lookup"><span data-stu-id="edfcb-229">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="edfcb-230">因為這兩個協助程式密切相關，而且您未來可能會將它們重構，所以我們將它們保留在相同的檔案中。</span><span class="sxs-lookup"><span data-stu-id="edfcb-230">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="edfcb-231">將下列 `AutoLinkerHttpTagHelper` 類別新增至 *TagHelpers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="edfcb-231">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="edfcb-232">`AutoLinkerHttpTagHelper` 類別將目標設為 `p` 項目，並使用 [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) 來建立錨點。</span><span class="sxs-lookup"><span data-stu-id="edfcb-232">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

1. <span data-ttu-id="edfcb-233">將下列標記新增至 *Views/Home/Contact.cshtml* 檔案結尾：</span><span class="sxs-lookup"><span data-stu-id="edfcb-233">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. <span data-ttu-id="edfcb-234">執行應用程式，並驗證標籤協助程式正確地轉譯錨點。</span><span class="sxs-lookup"><span data-stu-id="edfcb-234">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

1. <span data-ttu-id="edfcb-235">更新 `AutoLinker` 類別以包括 `AutoLinkerWwwTagHelper`，這會將 www 文字轉換成也包含原始 www 文字的錨點標籤。</span><span class="sxs-lookup"><span data-stu-id="edfcb-235">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="edfcb-236">已更新的程式碼反白顯示如下：</span><span class="sxs-lookup"><span data-stu-id="edfcb-236">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. <span data-ttu-id="edfcb-237">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-237">Run the app.</span></span> <span data-ttu-id="edfcb-238">請注意，www 文字會轉譯為連結，但 HTTP 文字則否。</span><span class="sxs-lookup"><span data-stu-id="edfcb-238">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="edfcb-239">如果您將中斷點放在兩個類別中，則可以看到先執行 HTTP 標籤協助程式類別。</span><span class="sxs-lookup"><span data-stu-id="edfcb-239">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="edfcb-240">問題在於已快取標籤協助程式輸出，而且執行 WWW 標籤協助程式時，會覆寫 HTTP 標籤協助程式的已快取輸出。</span><span class="sxs-lookup"><span data-stu-id="edfcb-240">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="edfcb-241">在教學課程稍後，將了解如何控制標籤協助程式的執行順序。</span><span class="sxs-lookup"><span data-stu-id="edfcb-241">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="edfcb-242">我們將使用下列內容來修正程式碼：</span><span class="sxs-lookup"><span data-stu-id="edfcb-242">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="edfcb-243">在第一版的自動連結標籤協助程式中，您已使用下列程式碼取得目標的內容：</span><span class="sxs-lookup"><span data-stu-id="edfcb-243">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > <span data-ttu-id="edfcb-244">也就是說，使用傳入 `ProcessAsync` 方法的 `TagHelperOutput`，即可呼叫 `GetChildContentAsync`。</span><span class="sxs-lookup"><span data-stu-id="edfcb-244">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="edfcb-245">如前所述，因為已快取輸出，所以會執行最後一個標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-245">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="edfcb-246">您已使用下列程式碼修正該問題：</span><span class="sxs-lookup"><span data-stu-id="edfcb-246">You fixed that problem with the following code:</span></span>
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > <span data-ttu-id="edfcb-247">上述程式碼會確認已修改內容；如果已修改，則會從輸出緩衝區中取得內容。</span><span class="sxs-lookup"><span data-stu-id="edfcb-247">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

1. <span data-ttu-id="edfcb-248">執行應用程式，並驗證兩個連結如預期運作。</span><span class="sxs-lookup"><span data-stu-id="edfcb-248">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="edfcb-249">雖然可能會顯示自動連結器標籤協助程式正確且完整，但是它有些微小問題。</span><span class="sxs-lookup"><span data-stu-id="edfcb-249">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="edfcb-250">如果先執行 WWW 標籤協助程式，則 www 連結會不正確。</span><span class="sxs-lookup"><span data-stu-id="edfcb-250">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="edfcb-251">新增 `Order` 多載以控制標籤執行順序，來更新程式碼。</span><span class="sxs-lookup"><span data-stu-id="edfcb-251">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="edfcb-252">`Order` 屬性可決定相對於以相同項目為目標的其他標籤協助程式的執行順序。</span><span class="sxs-lookup"><span data-stu-id="edfcb-252">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="edfcb-253">預設順序值為零，而且會先執行值較小的執行個體。</span><span class="sxs-lookup"><span data-stu-id="edfcb-253">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   <span data-ttu-id="edfcb-254">上述程式碼保證先執行 HTTP 標籤協助程式，再執行 WWW 標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="edfcb-254">The preceding code guarantees that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="edfcb-255">將 `Order` 變更為 `MaxValue`，並驗證針對 WWW 標籤 (tag) 所產生的標籤 (markup) 不正確。</span><span class="sxs-lookup"><span data-stu-id="edfcb-255">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="edfcb-256">檢查並擷取子內容</span><span class="sxs-lookup"><span data-stu-id="edfcb-256">Inspect and retrieve child content</span></span>

<span data-ttu-id="edfcb-257">標籤協助程式提供數個屬性來擷取內容。</span><span class="sxs-lookup"><span data-stu-id="edfcb-257">The tag helpers provide several properties to retrieve content.</span></span>

* <span data-ttu-id="edfcb-258">`GetChildContentAsync` 的結果可以附加至 `output.Content`。</span><span class="sxs-lookup"><span data-stu-id="edfcb-258">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
* <span data-ttu-id="edfcb-259">您可以使用 `GetContent` 來檢查 `GetChildContentAsync` 的結果。</span><span class="sxs-lookup"><span data-stu-id="edfcb-259">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
* <span data-ttu-id="edfcb-260">如果您修改 `output.Content`，則除非您呼叫 `GetChildContentAsync` 作為自動連結器範例，否則不會執行或轉譯 TagHelper 本文：</span><span class="sxs-lookup"><span data-stu-id="edfcb-260">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* <span data-ttu-id="edfcb-261">多次呼叫 `GetChildContentAsync` 會傳回相同的值，而且除非您傳入 false 參數以指出不使用已快取內容，否則不會重新執行 `TagHelper` 本文。</span><span class="sxs-lookup"><span data-stu-id="edfcb-261">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
