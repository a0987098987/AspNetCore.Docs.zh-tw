---
title: "設定可攜式物件當地語系化"
author: sebastienros
description: "本文介紹可攜式物件檔案，並且摘要說明 Orchard 核心架構的 ASP.NET Core 應用程式中使用它們的步驟。"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="5f089-103">設定可攜式物件當地語系化 Orchard 核心</span><span class="sxs-lookup"><span data-stu-id="5f089-103">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="5f089-104">由[Sébastien Ros](https://github.com/sebastienros)和[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="5f089-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="5f089-105">本文逐步使用可攜式物件 (PO) 檔案的 ASP.NET Core 應用程式中的步驟[Orchard 核心](https://github.com/OrchardCMS/OrchardCore)架構。</span><span class="sxs-lookup"><span data-stu-id="5f089-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="5f089-106">**注意：** Orchard 核心不是一項 Microsoft 產品。</span><span class="sxs-lookup"><span data-stu-id="5f089-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="5f089-107">因此，Microsoft 不提供支援這項功能。</span><span class="sxs-lookup"><span data-stu-id="5f089-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="5f089-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f089-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="5f089-109">什麼是 PO 檔案？</span><span class="sxs-lookup"><span data-stu-id="5f089-109">What is a PO file?</span></span>

<span data-ttu-id="5f089-110">發佈 PO 檔案為文字檔案，其中包含給定語言的翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="5f089-111">改為使用 PO 檔案的一些優點*.resx*檔案包括：</span><span class="sxs-lookup"><span data-stu-id="5f089-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="5f089-112">PO 檔案支援複數表示。*.resx*檔案不支援複數表示。</span><span class="sxs-lookup"><span data-stu-id="5f089-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="5f089-113">PO 檔案不像編譯*.resx*檔案。</span><span class="sxs-lookup"><span data-stu-id="5f089-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="5f089-114">因此，特製化的工具與建置步驟不需要。</span><span class="sxs-lookup"><span data-stu-id="5f089-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="5f089-115">PO 檔案適用於的共同作業的線上編輯工具。</span><span class="sxs-lookup"><span data-stu-id="5f089-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="5f089-116">範例</span><span class="sxs-lookup"><span data-stu-id="5f089-116">Example</span></span>

<span data-ttu-id="5f089-117">以下是範例 PO 檔案包含兩個字串以法文顯示，包括另一個複數形式的翻譯：</span><span class="sxs-lookup"><span data-stu-id="5f089-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="5f089-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="5f089-118">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="5f089-119">這個範例會使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="5f089-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="5f089-120">`#:`: 指出要翻譯的字串內容的註解。</span><span class="sxs-lookup"><span data-stu-id="5f089-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="5f089-121">相同的字串可能會以不同的方式轉譯，其中它會依據所使用。</span><span class="sxs-lookup"><span data-stu-id="5f089-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="5f089-122">`msgid`: 轉譯的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="5f089-123">`msgstr`： 用來轉換的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="5f089-124">在複數表示支援的情況下可以定義多個項目。</span><span class="sxs-lookup"><span data-stu-id="5f089-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="5f089-125">`msgid_plural`： 用來轉譯複數字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="5f089-126">`msgstr[0]`： 0 的情況已翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="5f089-127">`msgstr[N]`: 大小 n 翻譯的字串</span><span class="sxs-lookup"><span data-stu-id="5f089-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="5f089-128">您可以找到 PO 檔案規格[這裡](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html)。</span><span class="sxs-lookup"><span data-stu-id="5f089-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="5f089-129">設定在 ASP.NET Core PO 檔案支援</span><span class="sxs-lookup"><span data-stu-id="5f089-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="5f089-130">這個範例根據從 Visual Studio 2017 專案範本產生 ASP.NET Core MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f089-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="5f089-131">參考封裝</span><span class="sxs-lookup"><span data-stu-id="5f089-131">Referencing the package</span></span>

<span data-ttu-id="5f089-132">將參考加入`OrchardCore.Localization.Core`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="5f089-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="5f089-133">它是用於[MyGet](https://www.myget.org/)在下列的封裝來源： https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="5f089-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="5f089-134">*.Csproj*檔案現在會包含與下列相似的一行 （版本號碼可能不同）：</span><span class="sxs-lookup"><span data-stu-id="5f089-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="5f089-135">註冊服務</span><span class="sxs-lookup"><span data-stu-id="5f089-135">Registering the service</span></span>

<span data-ttu-id="5f089-136">加入必要的服務，可`ConfigureServices`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5f089-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="5f089-137">加入必要的中介軟體到`Configure`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="5f089-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="5f089-138">將下列程式碼加入至所選擇的 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="5f089-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="5f089-139">*About.cshtml*此範例中使用。</span><span class="sxs-lookup"><span data-stu-id="5f089-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="5f089-140">`IViewLocalizer`執行個體是插入的程式，用於翻譯文字"Hello world ！"。</span><span class="sxs-lookup"><span data-stu-id="5f089-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="5f089-141">建立 PO 檔案</span><span class="sxs-lookup"><span data-stu-id="5f089-141">Creating a PO file</span></span>

<span data-ttu-id="5f089-142">建立名為 *<culture code>.po*應用程式根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5f089-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="5f089-143">在此範例中，檔案名稱是*fr.po*因為法文語言用來：</span><span class="sxs-lookup"><span data-stu-id="5f089-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="5f089-144">這個檔案會儲存要翻譯的字串和法文翻譯的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="5f089-145">翻譯會還原為其父文化特性，如有必要的範圍。</span><span class="sxs-lookup"><span data-stu-id="5f089-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="5f089-146">在此範例中， *fr.po*如果要求的文化特性，則會使用檔案`fr-FR`或`fr-CA`。</span><span class="sxs-lookup"><span data-stu-id="5f089-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="5f089-147">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="5f089-147">Testing the application</span></span>

<span data-ttu-id="5f089-148">執行您的應用程式，並瀏覽至 URL `/Home/About`。</span><span class="sxs-lookup"><span data-stu-id="5f089-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="5f089-149">文字**Hello world ！**</span><span class="sxs-lookup"><span data-stu-id="5f089-149">The text **Hello world!**</span></span> <span data-ttu-id="5f089-150">隨即出現。</span><span class="sxs-lookup"><span data-stu-id="5f089-150">is displayed.</span></span>

<span data-ttu-id="5f089-151">瀏覽至 URL `/Home/About?culture=fr-FR`。</span><span class="sxs-lookup"><span data-stu-id="5f089-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="5f089-152">文字**le、 全球、 宇宙 Bonjour ！**</span><span class="sxs-lookup"><span data-stu-id="5f089-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="5f089-153">隨即出現。</span><span class="sxs-lookup"><span data-stu-id="5f089-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="5f089-154">複數表示</span><span class="sxs-lookup"><span data-stu-id="5f089-154">Pluralization</span></span>

<span data-ttu-id="5f089-155">PO 檔案支援複數表示表單會在相同的字串必須轉譯根據基數時很有用。</span><span class="sxs-lookup"><span data-stu-id="5f089-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="5f089-156">這項工作中進行複雜的每一種語言定義自訂規則以選取要使用的字串會根據基數的事實。</span><span class="sxs-lookup"><span data-stu-id="5f089-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="5f089-157">Orchard 當地語系化套件會提供 API，以自動叫用這些不同的複數形式。</span><span class="sxs-lookup"><span data-stu-id="5f089-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="5f089-158">建立複數表示 PO 檔案</span><span class="sxs-lookup"><span data-stu-id="5f089-158">Creating pluralization PO files</span></span>

<span data-ttu-id="5f089-159">將下列內容加入至先前所述*fr.po*檔案：</span><span class="sxs-lookup"><span data-stu-id="5f089-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="5f089-160">請參閱[PO 檔案是什麼？](#what-is-a-po-file)需在此範例中的每個項目所代表的意義的說明。</span><span class="sxs-lookup"><span data-stu-id="5f089-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="5f089-161">新增語言，使用不同的複數表示表單</span><span class="sxs-lookup"><span data-stu-id="5f089-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="5f089-162">在上述範例中，已使用英文和法文字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="5f089-163">英文和法文有只有兩個複數表示表單而共用相同的格式規則，這是基數為一會對應到第一個複數形式。</span><span class="sxs-lookup"><span data-stu-id="5f089-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="5f089-164">任何其他基數被對應到第二個複數形式。</span><span class="sxs-lookup"><span data-stu-id="5f089-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="5f089-165">並非所有語言都共用相同的規則。</span><span class="sxs-lookup"><span data-stu-id="5f089-165">Not all languages share the same rules.</span></span> <span data-ttu-id="5f089-166">以下是使用捷克文語言有三個複數形式。</span><span class="sxs-lookup"><span data-stu-id="5f089-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="5f089-167">建立`cs.po`檔案，如下所示，並記下複數表示需要三個不同的轉譯的方式：</span><span class="sxs-lookup"><span data-stu-id="5f089-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="5f089-168">若要接受捷克文的當地語系化資源，請加入`"cs"`中支援的文化特性的清單`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="5f089-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="5f089-169">編輯*Views/Home/About.cshtml*來呈現數個基數的當地語系化的複數字串的檔案：</span><span class="sxs-lookup"><span data-stu-id="5f089-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="5f089-170">**注意：**在真實世界案例中，變數會用來代表的計數。</span><span class="sxs-lookup"><span data-stu-id="5f089-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="5f089-171">在這裡，我們會重複相同的程式碼具有三個不同的值，以公開非常特定的大小寫。</span><span class="sxs-lookup"><span data-stu-id="5f089-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="5f089-172">切換文化特性而有所不同，您可以看到下列：</span><span class="sxs-lookup"><span data-stu-id="5f089-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="5f089-173">針對 `/Home/About`：</span><span class="sxs-lookup"><span data-stu-id="5f089-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="5f089-174">針對 `/Home/About?culture=fr`：</span><span class="sxs-lookup"><span data-stu-id="5f089-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="5f089-175">針對 `/Home/About?culture=cs`：</span><span class="sxs-lookup"><span data-stu-id="5f089-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="5f089-176">請注意，捷克文文化特性中，三個翻譯不同。</span><span class="sxs-lookup"><span data-stu-id="5f089-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="5f089-177">法文和英文的文化特性會共用相同的兩個的最後一個翻譯字串的建構。</span><span class="sxs-lookup"><span data-stu-id="5f089-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="5f089-178">進階的工作</span><span class="sxs-lookup"><span data-stu-id="5f089-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="5f089-179">Contextualizing 字串</span><span class="sxs-lookup"><span data-stu-id="5f089-179">Contextualizing strings</span></span>

<span data-ttu-id="5f089-180">應用程式通常會包含要在幾個位置中轉譯的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="5f089-181">相同的字串中 （Razor 檢視或類別檔案） 的應用程式內的特定位置，可能有不同的轉譯。</span><span class="sxs-lookup"><span data-stu-id="5f089-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="5f089-182">PO 檔案支援的檔案內容，可用來分類所表示的字串。</span><span class="sxs-lookup"><span data-stu-id="5f089-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="5f089-183">使用檔案內容，字串將不同的轉譯，根據檔案內容 （或缺乏的檔案內容）。</span><span class="sxs-lookup"><span data-stu-id="5f089-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="5f089-184">PO 當地語系化服務使用完整的類別或檢視轉譯的字串時所使用的名稱。</span><span class="sxs-lookup"><span data-stu-id="5f089-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="5f089-185">這是上設定的值`msgctxt`項目。</span><span class="sxs-lookup"><span data-stu-id="5f089-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="5f089-186">考慮先前的次要新增*fr.po*範例。</span><span class="sxs-lookup"><span data-stu-id="5f089-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="5f089-187">Razor 檢視位於*Views/Home/About.cshtml*可以藉由設定保留定義的檔案內容為`msgctxt`項目的值：</span><span class="sxs-lookup"><span data-stu-id="5f089-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="5f089-188">與`msgctxt`設定在這種情況，文字轉譯時發生瀏覽至`/Home/About?culture=fr-FR`。</span><span class="sxs-lookup"><span data-stu-id="5f089-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="5f089-189">若要瀏覽時，就不會發生轉譯`/Home/Contact?culture=fr-FR`。</span><span class="sxs-lookup"><span data-stu-id="5f089-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="5f089-190">沒有特定的項目會與指定的檔案內容比對，Orchard 核心後援機制會尋找適當的 PO 檔案沒有內容。</span><span class="sxs-lookup"><span data-stu-id="5f089-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="5f089-191">假設有已定義的任何特定的檔案內容*Views/Home/Contact.cshtml*、 瀏覽至`/Home/Contact?culture=fr-FR`載入 PO 檔案，例如：</span><span class="sxs-lookup"><span data-stu-id="5f089-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="5f089-192">變更 PO 檔案的位置</span><span class="sxs-lookup"><span data-stu-id="5f089-192">Changing the location of PO files</span></span>

<span data-ttu-id="5f089-193">PO 檔案的預設位置可以變更在`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5f089-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="5f089-194">在此範例中，從載入 PO 檔案*當地語系化*資料夾。</span><span class="sxs-lookup"><span data-stu-id="5f089-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="5f089-195">實作自訂的邏輯，以尋找當地語系化檔案</span><span class="sxs-lookup"><span data-stu-id="5f089-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="5f089-196">要尋找 PO 檔案需要更複雜的邏輯時`OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider`介面可以實作，並註冊為服務。</span><span class="sxs-lookup"><span data-stu-id="5f089-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="5f089-197">當 PO 檔案可以儲存在不同的位置或檔案具有要尋找的資料夾階層內時，這非常有用。</span><span class="sxs-lookup"><span data-stu-id="5f089-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="5f089-198">使用 pluralized 不同預設語言</span><span class="sxs-lookup"><span data-stu-id="5f089-198">Using a different default pluralized language</span></span>

<span data-ttu-id="5f089-199">封裝包含`Plural`屬於兩個複數形式的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="5f089-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="5f089-200">對於需要更多的複數形式的語言，建立擴充方法。</span><span class="sxs-lookup"><span data-stu-id="5f089-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="5f089-201">使用擴充方法，您不需要提供任何預設語言的當地語系化檔案&mdash;的原始字串已直接在程式碼中，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="5f089-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="5f089-202">您可以使用多個泛型`Plural(int count, string[] pluralForms, params object[] arguments)`多載會接受字串陣列中的翻譯。</span><span class="sxs-lookup"><span data-stu-id="5f089-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
