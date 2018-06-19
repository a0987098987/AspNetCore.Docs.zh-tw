---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: UI 和瀏覽 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: d2d4101455a85c53e016e567c0cf1337642f1863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890238"
---
<a name="ui-and-navigation"></a><span data-ttu-id="20c5e-103">UI 和導覽</span><span class="sxs-lookup"><span data-stu-id="20c5e-103">UI and Navigation</span></span>
====================
<span data-ttu-id="20c5e-104">由[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="20c5e-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="20c5e-105">[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="20c5e-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="20c5e-106">此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="20c5e-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="20c5e-107">Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。</span><span class="sxs-lookup"><span data-stu-id="20c5e-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="20c5e-108">在本教學課程中，您將修改的預設 Web 應用程式，以支援功能 Wingtip Toys store 前端應用程式的 UI。</span><span class="sxs-lookup"><span data-stu-id="20c5e-108">In this tutorial, you will modify the UI of the default Web application to support features of the Wingtip Toys store front application.</span></span> <span data-ttu-id="20c5e-109">此外，您會將簡單和資料繫結功能。</span><span class="sxs-lookup"><span data-stu-id="20c5e-109">Also, you will add simple and data bound navigation.</span></span> <span data-ttu-id="20c5e-110">本教學課程上一個教學課程 」 建立資料存取層 」 為基礎，並是 Wingtip Toys 教學課程系列的一部分。</span><span class="sxs-lookup"><span data-stu-id="20c5e-110">This tutorial builds on the previous tutorial "Create the Data Access Layer" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="20c5e-111">您將學習：</span><span class="sxs-lookup"><span data-stu-id="20c5e-111">What you'll learn:</span></span>

- <span data-ttu-id="20c5e-112">如何變更 UI，以支援 Wingtip Toys store 前端應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="20c5e-112">How to change the UI to support features of the Wingtip Toys store front application.</span></span>
- <span data-ttu-id="20c5e-113">如何設定要包含頁面導覽的 HTML5 項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-113">How to configure an HTML5 element to include page navigation.</span></span>
- <span data-ttu-id="20c5e-114">如何建立資料驅動的控制項來瀏覽至特定產品的資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-114">How to create a data-driven control to navigate to specific product data.</span></span>
- <span data-ttu-id="20c5e-115">如何顯示來自資料庫使用 Entity Framework Code First 所建立的資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-115">How to display data from a database created using Entity Framework Code First.</span></span>

<span data-ttu-id="20c5e-116">ASP.NET Web Forms 可讓您建立 Web 應用程式的動態內容。</span><span class="sxs-lookup"><span data-stu-id="20c5e-116">ASP.NET Web Forms allow you to create dynamic content for your Web application.</span></span> <span data-ttu-id="20c5e-117">每個 ASP.NET Web 網頁會建立靜態 HTML 網頁 （網頁不包含伺服器架構處理），類似的方式，但 ASP.NET 網頁包括 ASP.NET 辨識，並產生 HTML 網頁執行時處理的額外項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-117">Each ASP.NET Web page is created in a manner similar to a static HTML Web page (a page that does not include server-based processing), but ASP.NET Web page includes extra elements that ASP.NET recognizes and processes to generate HTML when the page runs.</span></span>

<span data-ttu-id="20c5e-118">與靜態 HTML 頁面 (*.htm*或 *.html*檔案)，伺服器具備`Web`讀取檔案，並將其傳送的要求-至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="20c5e-118">With a static HTML page (*.htm* or *.html* file), the server fulfills a `Web` request by reading the file and sending it as-is to the browser.</span></span> <span data-ttu-id="20c5e-119">相反地，當使用者要求的 ASP.NET 網頁 (*.aspx*檔案)，頁面當成程式執行 Web 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="20c5e-119">In contrast, when someone requests an ASP.NET Web page (*.aspx* file), the page runs as a program on the Web server.</span></span> <span data-ttu-id="20c5e-120">當網頁正在執行時，它可以執行任何需要的工作，您的網站，包括計算的值、 讀取或寫入資料庫的詳細資訊，或是呼叫其他程式。</span><span class="sxs-lookup"><span data-stu-id="20c5e-120">While the page is running, it can perform any task that your Web site requires, including calculating values, reading or writing database information, or calling other programs.</span></span> <span data-ttu-id="20c5e-121">輸出時，網頁，以動態方式產生標記 （例如 HTML 中的項目），並將這個動態的輸出傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="20c5e-121">As its output, the page dynamically produces markup (such as elements in HTML) and sends this dynamic output to the browser.</span></span>

## <a name="modifying-the-ui"></a><span data-ttu-id="20c5e-122">修改 UI</span><span class="sxs-lookup"><span data-stu-id="20c5e-122">Modifying the UI</span></span>

<span data-ttu-id="20c5e-123">您將會繼續這個教學課程系列藉由修改*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-123">You'll continue this tutorial series by modifying the *Default.aspx* page.</span></span> <span data-ttu-id="20c5e-124">您將修改用來建立應用程式的預設範本已建立的 UI。</span><span class="sxs-lookup"><span data-stu-id="20c5e-124">You will modify the UI that's already established by the default template used to create the application.</span></span> <span data-ttu-id="20c5e-125">建立任何 Web Form 應用程式時，您將會執行的修改的類型是一般。</span><span class="sxs-lookup"><span data-stu-id="20c5e-125">The type of modifications you'll do are typical when creating any Web Forms application.</span></span> <span data-ttu-id="20c5e-126">您要進行此變更標題、 一些內容，並移除不必要的預設內容。</span><span class="sxs-lookup"><span data-stu-id="20c5e-126">You'll do this by changing the title, replacing some content, and removing unneeded default content.</span></span>

1. <span data-ttu-id="20c5e-127">開啟或切換至*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-127">Open or switch to the *Default.aspx* page.</span></span>
2. <span data-ttu-id="20c5e-128">如果頁面隨即出現在**設計**檢視中，切換至**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="20c5e-128">If the page appears in **Design** view, switch to **Source** view.</span></span>
3. <span data-ttu-id="20c5e-129">在頁面頂端`@Page`指示詞，變更`Title`中下方的黃色反白顯示 「 歡迎 」，此屬性。</span><span class="sxs-lookup"><span data-stu-id="20c5e-129">At the top of the page in the `@Page` directive, change the `Title` attribute to "Welcome", as shown highlighted in yellow below.</span></span> 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. <span data-ttu-id="20c5e-130">同時在*Default.aspx*頁面上，會取代預設的內容中所包含的所有`<asp:Content>`標記，標記會顯示為下面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-130">Also on the *Default.aspx* page, replace all of the default content contained in the `<asp:Content>` tag so that the markup appears as below.</span></span> 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. <span data-ttu-id="20c5e-131">儲存*Default.aspx*頁面中的選取**儲存 Default.aspx**從**檔案**功能表。</span><span class="sxs-lookup"><span data-stu-id="20c5e-131">Save the *Default.aspx* page by selecting **Save Default.aspx** from the **File** menu.</span></span>

   <span data-ttu-id="20c5e-132">產生*Default.aspx*頁面會出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="20c5e-132">The resulting *Default.aspx* page will appear as follows:</span></span> 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

<span data-ttu-id="20c5e-133">在範例中，您已設定`Title`屬性`@Page`指示詞。</span><span class="sxs-lookup"><span data-stu-id="20c5e-133">In the example, you have set the `Title` attribute of the `@Page` directive.</span></span> <span data-ttu-id="20c5e-134">在瀏覽器，伺服端程式碼中顯示 HTML 時`<%: Page.Title %>`中所包含的內容來解析`Title`屬性。</span><span class="sxs-lookup"><span data-stu-id="20c5e-134">When the HTML is displayed in a browser, the server code `<%: Page.Title %>` resolves to the content contained in the `Title` attribute.</span></span>

<span data-ttu-id="20c5e-135">範例頁面包含構成 ASP.NET Web 網頁的基本項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-135">The example page includes the basic elements that constitute an ASP.NET Web page.</span></span> <span data-ttu-id="20c5e-136">此頁面包含靜態文字，因為您可能需要在 HTML 網頁，以及 ASP.NET 特定的項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-136">The page contains static text as you might have in an HTML page, along with elements that are specific to ASP.NET.</span></span> <span data-ttu-id="20c5e-137">中所包含的內容*Default.aspx*頁面將會與主版頁面內容，這將在稍後在本教學課程說明整合。</span><span class="sxs-lookup"><span data-stu-id="20c5e-137">The content contained in the *Default.aspx* page will be integrated with the master page content, which will be explained later in this tutorial.</span></span>

### <a name="page-directive"></a><span data-ttu-id="20c5e-138">@Page 指示詞</span><span class="sxs-lookup"><span data-stu-id="20c5e-138">@Page Directive</span></span>

<span data-ttu-id="20c5e-139">ASP.NET Web Form 通常會包含指示詞可讓您指定網頁的網頁內容和組態資訊。</span><span class="sxs-lookup"><span data-stu-id="20c5e-139">ASP.NET Web Forms usually contain directives that allow you to specify page properties and configuration information for the page.</span></span> <span data-ttu-id="20c5e-140">指示詞會使用 ASP.NET 作為指示如何處理序 頁面上，但是不會轉譯傳送到瀏覽器標記的一部分。</span><span class="sxs-lookup"><span data-stu-id="20c5e-140">The directives are used by ASP.NET as instructions for how to process the page, but they are not rendered as part of the markup that is sent to the browser.</span></span>

<span data-ttu-id="20c5e-141">最常使用的指示詞`@Page`指示詞，可讓您指定的頁面上，包括許多組態選項：</span><span class="sxs-lookup"><span data-stu-id="20c5e-141">The most commonly used directive is the `@Page` directive, which allows you to specify many configuration options for the page, including the following:</span></span>

1. <span data-ttu-id="20c5e-142">伺服器程式 頁面上，例如 C# 中的程式碼的語言。</span><span class="sxs-lookup"><span data-stu-id="20c5e-142">The server programming language for code in the page, such as C#.</span></span>
2. <span data-ttu-id="20c5e-143">不論是透過伺服器程式碼直接在頁面中，會呼叫單一檔案網頁，或在個別的類別檔案中，也就所謂的程式碼後置頁面的程式碼的頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-143">Whether the page is a page with server code directly in the page, which is called a single-file page, or whether it is a page with code in a separate class file, which is called a code-behind page.</span></span>
3. <span data-ttu-id="20c5e-144">頁面是否有相關聯的主版頁面，並因此應視為內容頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-144">Whether the page has an associated master page and should therefore be treated as a content page.</span></span>
4. <span data-ttu-id="20c5e-145">偵錯和追蹤選項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-145">Debugging and tracing options.</span></span>

<span data-ttu-id="20c5e-146">如果您未包含`@Page`指示詞在頁面中，或是如果指示詞不包含特定的設定，設定將會繼承自*Web.config*組態檔或從*Machine.config*組態檔。</span><span class="sxs-lookup"><span data-stu-id="20c5e-146">If you do not include an `@Page` directive in the page, or if the directive does not include a specific setting, a setting will be inherited from the *Web.config* configuration file or from the *Machine.config* configuration file.</span></span> <span data-ttu-id="20c5e-147">*Machine.config*檔案提供至所有電腦上執行的應用程式的其他組態設定。</span><span class="sxs-lookup"><span data-stu-id="20c5e-147">The *Machine.config* file provides additional configuration settings to all applications running on a machine.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="20c5e-148">*Machine.config*也會提供所有可能的組態設定的相關詳細資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-148">The *Machine.config* also provides details about all possible configuration settings.</span></span>


### <a name="web-server-controls"></a><span data-ttu-id="20c5e-149">Web 伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="20c5e-149">Web Server Controls</span></span>

<span data-ttu-id="20c5e-150">在大部分的 ASP.NET Web Form 應用程式，您將加入控制項，可讓使用者互動，請在頁面上，例如按鈕、 文字方塊、 清單和等等。</span><span class="sxs-lookup"><span data-stu-id="20c5e-150">In most ASP.NET Web Forms applications, you will add controls that allow the user to interact with the page, such as buttons, text boxes, lists, and so on.</span></span> <span data-ttu-id="20c5e-151">這些 Web 伺服器控制項是類似於 HTML 按鈕與輸入項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-151">These Web server controls are similar to HTML buttons and input elements.</span></span> <span data-ttu-id="20c5e-152">不過，它們會處理在伺服器上，讓您可以使用伺服端程式碼來設定其屬性。</span><span class="sxs-lookup"><span data-stu-id="20c5e-152">However, they are processed on the server, allowing you to use server code to set their properties.</span></span> <span data-ttu-id="20c5e-153">這些控制項也會引發事件，您可以在伺服器程式碼中處理。</span><span class="sxs-lookup"><span data-stu-id="20c5e-153">These controls also raise events that you can handle in server code.</span></span>

<span data-ttu-id="20c5e-154">伺服器控制項使用 ASP.NET 辨識網頁執行時的特殊語法。</span><span class="sxs-lookup"><span data-stu-id="20c5e-154">Server controls use a special syntax that ASP.NET recognizes when the page runs.</span></span> <span data-ttu-id="20c5e-155">ASP.NET 伺服器控制項的標記名稱開頭`asp:`前置詞。</span><span class="sxs-lookup"><span data-stu-id="20c5e-155">The tag name for ASP.NET server controls starts with an `asp:` prefix.</span></span> <span data-ttu-id="20c5e-156">這可讓 ASP.NET 識別及處理這些伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-156">This allows ASP.NET to recognize and process these server controls.</span></span> <span data-ttu-id="20c5e-157">前置詞可能會不同，如果控制項不是.NET Framework 的一部分。</span><span class="sxs-lookup"><span data-stu-id="20c5e-157">The prefix might be different if the control is not part of the .NET Framework.</span></span> <span data-ttu-id="20c5e-158">除了`asp:`前置詞，也包含 ASP.NET 伺服器控制項`runat="server"`屬性和`ID`可用來參考伺服器程式碼中的控制項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-158">In addition to the `asp:` prefix, ASP.NET server controls also include the `runat="server"` attribute and an `ID` that you can use to reference the control in server code.</span></span>

<span data-ttu-id="20c5e-159">頁面執行時，ASP.NET 識別的伺服器控制項，並執行這些控制項相關聯的程式碼。</span><span class="sxs-lookup"><span data-stu-id="20c5e-159">When the page runs, ASP.NET identifies the server controls and runs the code that is associated with those controls.</span></span> <span data-ttu-id="20c5e-160">許多控制項呈現 HTML 或其他標記至網頁瀏覽器中顯示時。</span><span class="sxs-lookup"><span data-stu-id="20c5e-160">Many controls render some HTML or other markup into the page when it is displayed in a browser.</span></span>

### <a name="server-code"></a><span data-ttu-id="20c5e-161">伺服端程式碼</span><span class="sxs-lookup"><span data-stu-id="20c5e-161">Server Code</span></span>

<span data-ttu-id="20c5e-162">大部分的 ASP.NET Web Form 應用程式會包含在處理網頁時，在伺服器執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="20c5e-162">Most ASP.NET Web Forms applications include code that runs on the server when the page is processed.</span></span> <span data-ttu-id="20c5e-163">如上面所述，伺服端程式碼可以用於執行各種不同的項目，例如將資料加入至 ListView 控制項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-163">As mentioned above, server code can be used to do a variety of things, such as adding data to a ListView control.</span></span> <span data-ttu-id="20c5e-164">ASP.NET 支援包括 C#、 Visual Basic、 J# 及其他的伺服器上執行的許多語言。</span><span class="sxs-lookup"><span data-stu-id="20c5e-164">ASP.NET supports many languages to run on the server, including C#, Visual Basic, J#, and others.</span></span>

<span data-ttu-id="20c5e-165">ASP.NET 支援兩種模型撰寫的 Web 網頁的伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="20c5e-165">ASP.NET supports two models for writing server code for a Web page.</span></span> <span data-ttu-id="20c5e-166">在單一檔案模式中，網頁的程式碼是在指令碼項目開頭標記，包含`runat="server"`屬性。</span><span class="sxs-lookup"><span data-stu-id="20c5e-166">In the single-file model, the code for the page is in a script element where the opening tag includes the `runat="server"` attribute.</span></span> <span data-ttu-id="20c5e-167">或者，您可以在個別的類別檔案中，稱為 「 程式碼後置模型，建立網頁的程式碼。</span><span class="sxs-lookup"><span data-stu-id="20c5e-167">Alternatively, you can create the code for the page in a separate class file, which is referred to as the code-behind model.</span></span> <span data-ttu-id="20c5e-168">在此情況下，ASP.NET Web Form 網頁通常不包含伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="20c5e-168">In this case, the ASP.NET Web Forms page generally contains no server code.</span></span> <span data-ttu-id="20c5e-169">相反地，`@Page`指示詞包含連結的資訊 *.aspx*頁面與它相關聯的程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-169">Instead, the `@Page` directive includes information that links the *.aspx* page with its associated code-behind file.</span></span>

<span data-ttu-id="20c5e-170">`CodeBehind`屬性中包含`@Page`指示詞指定的不同類別檔案名稱和`Inherits`屬性會指定對應至頁面的程式碼後置檔案內的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="20c5e-170">The `CodeBehind` attribute contained in the `@Page` directive specifies the name of the separate class file, and the `Inherits` attribute specifies the name of the class within the code-behind file that corresponds to the page.</span></span>

### <a name="updating-the-master-page"></a><span data-ttu-id="20c5e-171">更新主版頁面</span><span class="sxs-lookup"><span data-stu-id="20c5e-171">Updating the Master Page</span></span>

<span data-ttu-id="20c5e-172">在 ASP.NET Web Form、 主版頁面可讓您在您的應用程式中建立一致的版面配置頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-172">In ASP.NET Web Forms, master pages allow you to create a consistent layout for the pages in your application.</span></span> <span data-ttu-id="20c5e-173">單一的主版頁面定義應用程式中的外觀和您想要針對所有的頁面 （或頁面群組） 的標準行為。</span><span class="sxs-lookup"><span data-stu-id="20c5e-173">A single master page defines the look and feel and standard behavior that you want for all of the pages (or a group of pages) in your application.</span></span> <span data-ttu-id="20c5e-174">然後，您可以建立個別的內容頁面包含您想要顯示，以上所述的內容。</span><span class="sxs-lookup"><span data-stu-id="20c5e-174">You can then create individual content pages that contain the content you want to display, as explained above.</span></span> <span data-ttu-id="20c5e-175">當使用者要求內容的網頁時，ASP.NET 會將它們與主要的頁面，即可產生結合的主版頁面配置與內容的頁面內容輸出。</span><span class="sxs-lookup"><span data-stu-id="20c5e-175">When users request the content pages, ASP.NET merges them with the master page to produce output that combines the layout of the master page with the content from the content page.</span></span>

<span data-ttu-id="20c5e-176">新的站台需要單一商標將顯示每個頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-176">The new site needs a single logo to display on every page.</span></span> <span data-ttu-id="20c5e-177">若要加入這個標誌，您可以修改主版頁面的 HTML。</span><span class="sxs-lookup"><span data-stu-id="20c5e-177">To add this logo, you can modify the HTML on the master page.</span></span>

1. <span data-ttu-id="20c5e-178">在**方案總管 中**、 尋找和開啟**Site.Master**頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-178">In **Solution Explorer**, find and open the **Site.Master** page.</span></span>
2. <span data-ttu-id="20c5e-179">若頁面位於**設計**檢視中，切換至**來源**檢視。</span><span class="sxs-lookup"><span data-stu-id="20c5e-179">If the page is in **Design** view, switch to **Source** view.</span></span>
3. <span data-ttu-id="20c5e-180">更新由主版頁面**修改或加入**以黃色反白顯示的標記：</span><span class="sxs-lookup"><span data-stu-id="20c5e-180">Update the master page by **modifying or adding** the markup highlighted in yellow:</span></span> 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

<span data-ttu-id="20c5e-181">此 HTML 會顯示名為映像*logo.jpg*從*映像*Web 應用程式，您稍後會加入資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-181">This HTML will display the image named *logo.jpg* from the *Images* folder of the Web application, which you'll add later.</span></span> <span data-ttu-id="20c5e-182">使用主版頁面的網頁瀏覽器中顯示時，將會顯示標誌。</span><span class="sxs-lookup"><span data-stu-id="20c5e-182">When a page that uses the master page is displayed in a browser, the logo will be displayed.</span></span> <span data-ttu-id="20c5e-183">如果使用者按一下標誌，使用者會瀏覽回到*Default.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-183">If a user clicks on the logo, the user will navigate back to the *Default.aspx* page.</span></span> <span data-ttu-id="20c5e-184">HTML 錨點標籤`<a>`包裝映像的伺服器控制項，並可讓要包含在連結的映像。</span><span class="sxs-lookup"><span data-stu-id="20c5e-184">The HTML anchor tag `<a>` wraps the image server control and allows the image to be included as part of the link.</span></span> <span data-ttu-id="20c5e-185">`href`屬性的錨定標記指定的根"`~/`"做為連結位置的網站。</span><span class="sxs-lookup"><span data-stu-id="20c5e-185">The `href` attribute for the anchor tag specifies the root "`~/`" of the Web site as the link location.</span></span> <span data-ttu-id="20c5e-186">根據預設， *Default.aspx*使用者導覽至網站的根目錄時，會顯示頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-186">By default, the *Default.aspx* page is displayed when the user navigates to the root of the Web site.</span></span> <span data-ttu-id="20c5e-187">**映像**`<asp:Image>`伺服器控制項包括新增屬性，例如`BorderStyle`，可呈現為 HTML 瀏覽器中顯示時。</span><span class="sxs-lookup"><span data-stu-id="20c5e-187">The **Image** `<asp:Image>` server control includes addition properties, such as `BorderStyle`, that render as HTML when displayed in a browser.</span></span>

### <a name="master-pages"></a><span data-ttu-id="20c5e-188">主版頁面</span><span class="sxs-lookup"><span data-stu-id="20c5e-188">Master Pages</span></span>

<span data-ttu-id="20c5e-189">主版頁面是副檔名.master ASP.NET 檔案 (例如， *Site.Master*) 可以包含靜態文字、 HTML 項目和伺服器控制項的預先定義版面配置。</span><span class="sxs-lookup"><span data-stu-id="20c5e-189">A master page is an ASP.NET file with the extension .master (for example, *Site.Master*) with a predefined layout that can include static text, HTML elements, and server controls.</span></span> <span data-ttu-id="20c5e-190">主版頁面由特殊`@Master`指示詞，以取代`@Page`用於一般的指示詞 *.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-190">The master page is identified by a special `@Master` directive that replaces the `@Page` directive that is used for ordinary *.aspx* pages.</span></span>

<span data-ttu-id="20c5e-191">除了`@Master`指示詞，主版頁面也包含所有最上層的 HTML 項目頁面，例如`html`， `head`，和`form`。</span><span class="sxs-lookup"><span data-stu-id="20c5e-191">In addition to the `@Master` directive, the master page also contains all of the top-level HTML elements for a page, such as `html`, `head`, and `form`.</span></span> <span data-ttu-id="20c5e-192">例如，在上述您新增主要頁面上，您使用 HTML`table`配置，請`img`公司標誌、 靜態文字，以及處理常見的成員資格，您站台伺服器控制項的項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-192">For example, on the master page you added above, you use an HTML `table` for the layout, an `img` element for the company logo, static text, and server controls to handle common membership for your site.</span></span> <span data-ttu-id="20c5e-193">您可以使用任何 HTML 及 ASP.NET 中的任何項目，做為您的主版頁面的一部分。</span><span class="sxs-lookup"><span data-stu-id="20c5e-193">You can use any HTML and any ASP.NET elements as part of your master page.</span></span>

<span data-ttu-id="20c5e-194">除了靜態文字和會出現在所有頁面的控制項、 主版頁面也包含一或多個**ContentPlaceHolder**控制項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-194">In addition to static text and controls that will appear on all pages, the master page also includes one or more **ContentPlaceHolder** controls.</span></span> <span data-ttu-id="20c5e-195">這些預留位置控制項定義可取代內容的顯示位置的區域。</span><span class="sxs-lookup"><span data-stu-id="20c5e-195">These placeholder controls define regions where replaceable content will appear.</span></span> <span data-ttu-id="20c5e-196">接著，可取代內容定義在內容頁中，例如*Default.aspx*，並使用**內容**伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-196">In turn, the replaceable content is defined in content pages, such as *Default.aspx*, using the **Content** server control.</span></span>

#### <a name="adding-image-files"></a><span data-ttu-id="20c5e-197">將影像檔</span><span class="sxs-lookup"><span data-stu-id="20c5e-197">Adding Image Files</span></span>

<span data-ttu-id="20c5e-198">標誌影像上方，參考的所有產品映像，以及必須新增 Web 應用程式，以便在瀏覽器中顯示專案時可以看到它們。</span><span class="sxs-lookup"><span data-stu-id="20c5e-198">The logo image that is referenced above, along with all the product images, must be added to the Web application so that they can be seen when the project is displayed in a browser.</span></span>

#### <a name="download-from-msdn-samples-site"></a><span data-ttu-id="20c5e-199">從 MSDN 範例網站下載：</span><span class="sxs-lookup"><span data-stu-id="20c5e-199">Download from MSDN Samples site:</span></span>

<span data-ttu-id="20c5e-200">[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="20c5e-200">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

<span data-ttu-id="20c5e-201">此下載包含中的資源*WingtipToys 資產*用來建立範例應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-201">The download includes resources in the *WingtipToys-Assets* folder that are used to create the sample application.</span></span>

1. <span data-ttu-id="20c5e-202">如果您尚未這樣做，請下載從 MSDN 範例網站在使用上述連結的壓縮的範例檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-202">If you haven't already done so, download the compressed sample files using the above link from the MSDN Samples site.</span></span>
2. <span data-ttu-id="20c5e-203">在下載後，開啟.zip 檔案，並將內容複製到您的電腦上的本機資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-203">Once downloaded, open the .zip file and copy the contents to a local folder on your machine.</span></span>
3. <span data-ttu-id="20c5e-204">尋找和開啟*WingtipToys 資產*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-204">Find and open the *WingtipToys-Assets* folder.</span></span>
4. <span data-ttu-id="20c5e-205">藉由拖放、 複製*目錄*從您的本機資料夾的資料夾中的 Web 應用程式專案的根目錄**方案總管 中**的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="20c5e-205">By dragging and dropping, copy the *Catalog* folder from your local folder to the root of the Web application project in the **Solution Explorer** of Visual Studio.</span></span> 

    ![UI 和瀏覽-檔案複製](ui_and_navigation/_static/image1.png)
5. <span data-ttu-id="20c5e-207">接下來，建立新的資料夾，名為*映像*以滑鼠右鍵按一下**WingtipToys**專案中**方案總管 中**，然後選取**新增** - &gt; **新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-207">Next, create a new folder named *Images* by right-clicking the **WingtipToys** project in **Solution Explorer** and selecting **Add** -&gt; **New Folder**.</span></span>
6. <span data-ttu-id="20c5e-208">複製*logo.jpg*檔案從*WingtipToys 資產*資料夾中的**檔案總管**至*映像*Web 應用程式的資料夾專案中**方案總管 中**的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="20c5e-208">Copy the *logo.jpg* file from the *WingtipToys-Assets* folder in **File Explorer** to the *Images* folder of the Web application project in **Solution Explorer** of Visual Studio.</span></span>
7. <span data-ttu-id="20c5e-209">按一下**顯示所有檔案**選項上方的**方案總管 中**更新的檔案清單，如果您沒有看到新的檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-209">Click the **Show All Files** option at the top of **Solution Explorer** to update the list of files if you don't see the new files.</span></span>  
  
    <span data-ttu-id="20c5e-210">**方案總管 中**現在會顯示更新的專案檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-210">**Solution Explorer** now shows the updated project files.</span></span> 

    ![UI 和瀏覽-方案總管](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a><span data-ttu-id="20c5e-212">新增頁</span><span class="sxs-lookup"><span data-stu-id="20c5e-212">Adding Pages</span></span>

<span data-ttu-id="20c5e-213">然後再加入瀏覽至 Web 應用程式，您要先加入兩個新的頁面，您會瀏覽至。</span><span class="sxs-lookup"><span data-stu-id="20c5e-213">Before adding navigation to the Web application, you'll first add two new pages that you'll navigate to.</span></span> <span data-ttu-id="20c5e-214">稍後在本教學課程系列，您會在這些新的頁面上顯示產品和產品詳細資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-214">Later in this tutorial series, you'll display products and product details on these new pages.</span></span>

1. <span data-ttu-id="20c5e-215">在**方案總管 中**，以滑鼠右鍵按一下**WingtipToys**，按一下 **新增**，然後按一下 **新項目**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-215">In **Solution Explorer**, right-click **WingtipToys**, click **Add**, and then click **New Item**.</span></span>   
 <span data-ttu-id="20c5e-216">隨即顯示 [ 新增項目] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="20c5e-216">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="20c5e-217">選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。</span><span class="sxs-lookup"><span data-stu-id="20c5e-217">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="20c5e-218">然後，選取**使用主版頁面的 Web Form**中間清單並將其命名*ProductList.aspx*。</span><span class="sxs-lookup"><span data-stu-id="20c5e-218">Then, select **Web Form with Master Page** from the middle list and name it *ProductList.aspx*.</span></span> 

    ![UI 和瀏覽-加入新項目 對話方塊](ui_and_navigation/_static/image3.png)
3. <span data-ttu-id="20c5e-220">選取**Site.Master**附加到新建立的主版頁面 *.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-220">Select **Site.Master** to attach the master page to the newly created *.aspx* page.</span></span> 

    ![UI 和瀏覽-選取主版頁面](ui_and_navigation/_static/image4.png)
4. <span data-ttu-id="20c5e-222">加入額外的頁面，名為*ProductDetails.aspx*遵循這些相同的步驟。</span><span class="sxs-lookup"><span data-stu-id="20c5e-222">Add an additional page named *ProductDetails.aspx* by following these same steps.</span></span>

### <a name="updating-bootstrap"></a><span data-ttu-id="20c5e-223">更新啟動程序</span><span class="sxs-lookup"><span data-stu-id="20c5e-223">Updating Bootstrap</span></span>

<span data-ttu-id="20c5e-224">Visual Studio 2013 專案範本使用[Bootstrap](http://getbootstrap.com/)，Twitter 所建立的配置和佈景主題的架構。</span><span class="sxs-lookup"><span data-stu-id="20c5e-224">The Visual Studio 2013 project templates use [Bootstrap](http://getbootstrap.com/), a layout and theming framework created by Twitter.</span></span> <span data-ttu-id="20c5e-225">啟動程序會使用 CSS3 提供回應式設計，這表示配置可以動態的方式適應不同的瀏覽器視窗大小。</span><span class="sxs-lookup"><span data-stu-id="20c5e-225">Bootstrap uses CSS3 to provide responsive design, which means layouts can dynamically adapt to different browser window sizes.</span></span> <span data-ttu-id="20c5e-226">您也可以使用啟動程序的主題設定功能，以輕鬆地促使應用程式的外觀及操作中的變更。</span><span class="sxs-lookup"><span data-stu-id="20c5e-226">You can also use Bootstrap's theming feature to easily effect a change in the application's look and feel.</span></span> <span data-ttu-id="20c5e-227">根據預設，Visual Studio 2013 中的 ASP.NET Web 應用程式範本會以 NuGet 套件包含啟動程序。</span><span class="sxs-lookup"><span data-stu-id="20c5e-227">By default, the ASP.NET Web Application template in Visual Studio 2013 includes Bootstrap as a NuGet package.</span></span>

<span data-ttu-id="20c5e-228">在本教學課程中，您會藉由取代啟動載入器的 CSS 檔案變更 Wingtip Toys 應用程式的外觀與風格。</span><span class="sxs-lookup"><span data-stu-id="20c5e-228">In this tutorial, you will change look and feel of the Wingtip Toys application by replacing the Bootstrap CSS files.</span></span>

1. <span data-ttu-id="20c5e-229">在**方案總管 中**，開啟*內容*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-229">In **Solution Explorer**, open the *Content* folder.</span></span>
2. <span data-ttu-id="20c5e-230">以滑鼠右鍵按一下*bootstrap.css*檔案，並在它重新命名為*bootstrap original.css*。</span><span class="sxs-lookup"><span data-stu-id="20c5e-230">Right-click the *bootstrap.css* file and rename it to *bootstrap-original.css*.</span></span>
3. <span data-ttu-id="20c5e-231">重新命名*bootstrap.min.css*至*bootstrap original.min.css*。</span><span class="sxs-lookup"><span data-stu-id="20c5e-231">Rename the *bootstrap.min.css* to *bootstrap-original.min.css*.</span></span>
4. <span data-ttu-id="20c5e-232">在**方案總管 中**，以滑鼠右鍵按一下*內容*資料夾，然後選取**在檔案總管 中開啟資料夾**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-232">In **Solution Explorer**, right-click the *Content* folder and select **Open Folder in File Explorer**.</span></span>  
   <span data-ttu-id="20c5e-233">[檔案總管] 隨即出現。</span><span class="sxs-lookup"><span data-stu-id="20c5e-233">The File Explorer will be displayed.</span></span> <span data-ttu-id="20c5e-234">您會將下載的啟動程序 CSS 檔案儲存至這個位置。</span><span class="sxs-lookup"><span data-stu-id="20c5e-234">You will save a downloaded bootstrap CSS files to this location.</span></span>
5. <span data-ttu-id="20c5e-235">在您的瀏覽器，移至[ http://Bootswatch.com ](http://bootswatch.com/)。</span><span class="sxs-lookup"><span data-stu-id="20c5e-235">In your browser, go to [http://Bootswatch.com](http://bootswatch.com/).</span></span>
6. <span data-ttu-id="20c5e-236">捲動瀏覽器視窗中，直到您看到 Cerulean 佈景主題。</span><span class="sxs-lookup"><span data-stu-id="20c5e-236">Scroll the browser window until you see the Cerulean theme.</span></span> 

    ![UI 和瀏覽-Cerulean 佈景主題](ui_and_navigation/_static/image5.png)
7. <span data-ttu-id="20c5e-238">下載兩者*bootstrap.css*檔案和*bootstrap.min.css*檔案*內容*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-238">Download both the *bootstrap.css* file and the *bootstrap.min.css* file to the *Content* folder.</span></span> <span data-ttu-id="20c5e-239">使用路徑中所顯示的內容資料夾**檔案總管**先前開啟的視窗。</span><span class="sxs-lookup"><span data-stu-id="20c5e-239">Use the path to the content folder that is displayed in the **File Explorer** window that you previously opened.</span></span>
8. <span data-ttu-id="20c5e-240">在**Visual Studio**頂端**方案總管 中**，選取**顯示所有檔案**選項，以顯示新的檔案內容資料夾中。</span><span class="sxs-lookup"><span data-stu-id="20c5e-240">In **Visual Studio** at the top of **Solution Explorer**, select the **Show All Files** option to display the new files in the Content folder.</span></span> 

    ![UI 和瀏覽-方案總管](ui_and_navigation/_static/image6.png)

   <span data-ttu-id="20c5e-242">您會看到兩個新的 CSS 檔案中**內容**資料夾，但請注意，每個檔案名稱旁邊的圖示會變成灰色。這表示，該檔案尚未尚未加入至專案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-242">You will see the two new CSS files in the **Content** folder, but notice that the icon next to each file name is grayed out. This means that the file has not yet been added to the project.</span></span>
9. <span data-ttu-id="20c5e-243">以滑鼠右鍵按一下*bootstrap.css*和*bootstrap.min.css*檔案，然後選取**包含在專案**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-243">Right-click the *bootstrap.css* and the *bootstrap.min.css* files and select **Include In Project**.</span></span>   
   <span data-ttu-id="20c5e-244">當您稍後在本教學課程執行 Wingtip Toys 應用程式時，將會顯示新的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-244">When you run the Wingtip Toys application later in this tutorial, the new UI will be displayed.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="20c5e-245">ASP.NET Web 應用程式範本使用*Bundle.config* Bootstrap CSS 檔案的路徑儲存至專案根目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-245">The ASP.NET Web Application template uses the *Bundle.config* file at the root of the project to store the path of the Bootstrap CSS files.</span></span>


### <a name="modifying-the-default-navigation"></a><span data-ttu-id="20c5e-246">修改預設的巡覽</span><span class="sxs-lookup"><span data-stu-id="20c5e-246">Modifying the Default Navigation</span></span>

<span data-ttu-id="20c5e-247">變更未排序的瀏覽清單項目。 也可以修改預設的巡覽應用程式中的每個分頁*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-247">The default navigation for every page in the application can be modified by changing the unordered navigation list element that's in the *Site.Master* page.</span></span>

1. <span data-ttu-id="20c5e-248">在**方案總管 中**，找出並開啟*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-248">In **Solution Explorer**, locate and open the *Site.Master* page.</span></span>
2. <span data-ttu-id="20c5e-249">新增以未排序的清單，如下所示的黃色反白顯示的其他導覽連結：</span><span class="sxs-lookup"><span data-stu-id="20c5e-249">Add the additional navigation link highlighted in yellow to the unordered list shown below:</span></span>   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

<span data-ttu-id="20c5e-250">您可以看到上述的 HTML 中，您必須修改每個明細項目`<li>`包含錨定標記`<a>`連結`href`屬性。</span><span class="sxs-lookup"><span data-stu-id="20c5e-250">As you can see in the above HTML, you modified each line item `<li>` containing an anchor tag `<a>` with a link `href` attribute.</span></span> <span data-ttu-id="20c5e-251">每個`href`指向 Web 應用程式中的頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-251">Each `href` points to a page in the Web application.</span></span> <span data-ttu-id="20c5e-252">當使用者按一下這些連結的其中一個瀏覽器中 (例如**產品**)，它們會瀏覽至網頁中包含`href`(例如**ProductList.aspx**)。</span><span class="sxs-lookup"><span data-stu-id="20c5e-252">In the browser, when a user clicks on one of these links (such as **Products**), they will navigate to the page contained in the `href` (such as **ProductList.aspx**).</span></span> <span data-ttu-id="20c5e-253">在本教學課程結尾處，您將執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="20c5e-253">You will run the application at the end of this tutorial.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="20c5e-254">波狀符號 (`~`) 字元用來指定`href`路徑根目錄中的專案開始。</span><span class="sxs-lookup"><span data-stu-id="20c5e-254">The tilde (`~`) character is used to specify that the `href` path begins at the root of the project.</span></span>


### <a name="adding-a-data-control-to-display-navigation-data"></a><span data-ttu-id="20c5e-255">加入資料控制項以顯示 瀏覽資料</span><span class="sxs-lookup"><span data-stu-id="20c5e-255">Adding a Data Control to Display Navigation Data</span></span>

<span data-ttu-id="20c5e-256">接下來，您要加入控制項以顯示所有資料庫中的類別。</span><span class="sxs-lookup"><span data-stu-id="20c5e-256">Next, you'll add a control to display all of the categories from the database.</span></span> <span data-ttu-id="20c5e-257">每個類別目錄做為連結*ProductList.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="20c5e-257">Each category will act as a link to the *ProductList.aspx* page.</span></span> <span data-ttu-id="20c5e-258">當使用者按一下瀏覽器中的類別連結時，他們瀏覽至 [產品] 頁面，請參閱只與所選類別目錄相關聯的產品。</span><span class="sxs-lookup"><span data-stu-id="20c5e-258">When a user clicks on a category link in the browser, they will navigate to the products page and see only the products associated with the selected category.</span></span>

<span data-ttu-id="20c5e-259">您將使用**ListView**控制項來顯示資料庫中包含的所有類別目錄。</span><span class="sxs-lookup"><span data-stu-id="20c5e-259">You'll use a **ListView** control to display all the categories contained in the database.</span></span> <span data-ttu-id="20c5e-260">若要加入**ListView**主版頁面的控制項：</span><span class="sxs-lookup"><span data-stu-id="20c5e-260">To add a **ListView** control to the master page:</span></span>

1. <span data-ttu-id="20c5e-261">在*Site.Master*頁面上，加入下列反白顯示`<div>`元素**之後**`<div>`項目包含`id="TitleContent"`您先前加入的：</span><span class="sxs-lookup"><span data-stu-id="20c5e-261">In the *Site.Master* page, add the following highlighted `<div>` element **after** the `<div>` element containing the `id="TitleContent"` that you added earlier:</span></span>  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

<span data-ttu-id="20c5e-262">此程式碼會顯示資料庫中的所有類別目錄。</span><span class="sxs-lookup"><span data-stu-id="20c5e-262">This code will display all the categories from the database.</span></span> <span data-ttu-id="20c5e-263">**ListView**控制項做為連結文字顯示每個分類名稱和包含的連結*ProductList.aspx*頁面的查詢字串值包含`ID`類別目錄。</span><span class="sxs-lookup"><span data-stu-id="20c5e-263">The **ListView** control displays each category name as link text and includes a link to the *ProductList.aspx* page with a query-string value containing the `ID` of the category.</span></span> <span data-ttu-id="20c5e-264">藉由設定`ItemType`屬性**ListView**控制項，資料繫結運算式`Item`內`ItemTemplate`節點和控制項變成強型別。</span><span class="sxs-lookup"><span data-stu-id="20c5e-264">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available within the `ItemTemplate` node and the control becomes strongly typed.</span></span> <span data-ttu-id="20c5e-265">您可以選取的詳細資料`Item`物件使用 IntelliSense，例如指定`CategoryName`。</span><span class="sxs-lookup"><span data-stu-id="20c5e-265">You can select details of the `Item` object using IntelliSense, such as specifying the `CategoryName`.</span></span> <span data-ttu-id="20c5e-266">此程式碼會包含容器內`<%#: %>`符號資料繫結運算式。</span><span class="sxs-lookup"><span data-stu-id="20c5e-266">This code is contained inside the container `<%#: %>` that marks a data-binding expression.</span></span> <span data-ttu-id="20c5e-267">藉由新增 （:） 的結尾`<%#`前置詞，資料繫結運算式的結果是 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="20c5e-267">By adding the (:) to the end of the `<%#` prefix, the result of the data-binding expression is HTML-encoded.</span></span> <span data-ttu-id="20c5e-268">結果是 HTML 編碼，您的應用程式進一步保護可避免跨網站指令碼資料隱碼 (XSS) 和 HTML 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="20c5e-268">When the result is HTML-encoded, your application is better protected against cross-site script injection (XSS) and HTML injection attacks.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="20c5e-269">**Tip**</span><span class="sxs-lookup"><span data-stu-id="20c5e-269">**Tip**</span></span>
> 
> <span data-ttu-id="20c5e-270">當您在開發期間輸入加入程式碼時，您就可以確定物件的有效成員找到，因為強型別資料控制項顯示可用 IntelliSense 所根據的成員。</span><span class="sxs-lookup"><span data-stu-id="20c5e-270">When you add code by typing during development, you can be certain that a valid member of an object is found because strongly typed data controls show the available members based on IntelliSense.</span></span> <span data-ttu-id="20c5e-271">IntelliSense 提供了適當的內容程式碼選項，當您輸入程式碼，例如屬性、 方法和物件。</span><span class="sxs-lookup"><span data-stu-id="20c5e-271">IntelliSense offers context-appropriate code choices as you type code, such as properties, methods, and objects.</span></span>


<span data-ttu-id="20c5e-272">在下一個步驟中，您將實作`GetCategories`方法來擷取資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-272">In the next step, you will implement the `GetCategories` method to retrieve data.</span></span>

### <a name="linking-the-data-control-to-the-database"></a><span data-ttu-id="20c5e-273">將資料控制項連結至資料庫</span><span class="sxs-lookup"><span data-stu-id="20c5e-273">Linking the Data Control to the Database</span></span>

<span data-ttu-id="20c5e-274">您可以在資料控制項中顯示資料之前，您需要連線到資料庫的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-274">Before you can display data in the data control, you need to link the data control to the database.</span></span> <span data-ttu-id="20c5e-275">若要進行連結，您可以修改的程式碼後置*Site.Master.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-275">To make the link, you can modify the code behind of the *Site.Master.cs* file.</span></span>

1. <span data-ttu-id="20c5e-276">在**方案總管 中**，以滑鼠右鍵按一下*Site.Master*頁面，然後按一下**檢視程式碼**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-276">In **Solution Explorer**, right-click the *Site.Master* page and then click **View Code**.</span></span> <span data-ttu-id="20c5e-277">*Site.Master.cs*在編輯器中開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-277">The *Site.Master.cs* file is opened in the editor.</span></span>
2. <span data-ttu-id="20c5e-278">接近開頭*Site.Master.cs*檔案中加入兩個額外的命名空間，使所有包含的命名空間出現，如下所示：</span><span class="sxs-lookup"><span data-stu-id="20c5e-278">Near the beginning of the *Site.Master.cs* file, add two additional namespaces so that all the included namespaces appear as follows:</span></span>  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. <span data-ttu-id="20c5e-279">將反白顯示`GetCategories`方法之後`Page_Load`事件處理常式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="20c5e-279">Add the highlighted `GetCategories` method after the `Page_Load` event handler as follows:</span></span>  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

<span data-ttu-id="20c5e-280">上述程式碼會執行任何會使用主版頁面的頁面載入瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="20c5e-280">The above code is executed when any page that uses the master page is loaded in the browser.</span></span> <span data-ttu-id="20c5e-281">`ListView`您稍早在本教學課程中加入的控制項 (具名"categoryList 」) 會使用模型繫結選取的資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-281">The `ListView` control (named "categoryList") that you added earlier in this tutorial uses model binding to select data.</span></span> <span data-ttu-id="20c5e-282">在標記中`ListView`將控制項的控制項`SelectMethod`屬性`GetCategories`上面顯示的方法。</span><span class="sxs-lookup"><span data-stu-id="20c5e-282">In the markup of the `ListView` control you set the control's `SelectMethod` property to the `GetCategories` method, shown above.</span></span> <span data-ttu-id="20c5e-283">`ListView`控制呼叫`GetCategories`方法頁面生命週期中的適當時間循環，並自動繫結傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-283">The `ListView` control calls the `GetCategories` method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="20c5e-284">您將進一步了解下一個教學課程中的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="20c5e-284">You will learn more about binding data in the next tutorial.</span></span>

### <a name="running-the-application-and-creating-the-database"></a><span data-ttu-id="20c5e-285">執行應用程式和建立資料庫</span><span class="sxs-lookup"><span data-stu-id="20c5e-285">Running the Application and Creating the Database</span></span>

<span data-ttu-id="20c5e-286">稍早在本教學課程系列方法，您可以建立 （名為"ProductDatabaseInitializer"） 的初始設定式類別，並指定此類別中的*global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-286">Earlier in this tutorial series you created an initializer class (named "ProductDatabaseInitializer") and specified this class in the *global.asax.cs* file.</span></span> <span data-ttu-id="20c5e-287">當應用程式會執行第一次，因為 Entity Framework 將會產生資料庫`Application_Start`方法，包含在*global.asax.cs*檔將呼叫的初始設定式類別。</span><span class="sxs-lookup"><span data-stu-id="20c5e-287">The Entity Framework will generate the database when the application is run the first time because the `Application_Start` method contained in the *global.asax.cs* file will call the initializer class.</span></span> <span data-ttu-id="20c5e-288">初始設定式類別會使用模型類別 (`Category`和`Product`) 您稍早在本教學課程的序列，以建立資料庫中加入。</span><span class="sxs-lookup"><span data-stu-id="20c5e-288">The initializer class will use the model classes (`Category` and `Product`) that you added earlier in this tutorial series to create the database.</span></span>

1. <span data-ttu-id="20c5e-289">在**方案總管 中**，以滑鼠右鍵按一下*Default.aspx*頁面，然後選取**設定為起始頁**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-289">In **Solution Explorer**, right-click the *Default.aspx* page and select **Set As Start Page**.</span></span>
2. <span data-ttu-id="20c5e-290">在 Visual Studio **F5**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-290">In Visual Studio press **F5**.</span></span>   
 <span data-ttu-id="20c5e-291">需要一段時間才能在第一次執行此設定的所有項目。</span><span class="sxs-lookup"><span data-stu-id="20c5e-291">It will take a little time to set everything up during this first run.</span></span>   
    <span data-ttu-id="20c5e-292">![UI 和瀏覽-瀏覽器視窗](ui_and_navigation/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="20c5e-292">![UI and Navigation - Browser Windows](ui_and_navigation/_static/image7.png)</span></span>  
 <span data-ttu-id="20c5e-293">當您執行應用程式時，將編譯的應用程式和資料庫名為*wingtiptoys.mdf*將建立在*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-293">When you run the application, the application will be compiled and the database named *wingtiptoys.mdf* will be created in the *App\_Data* folder.</span></span> <span data-ttu-id="20c5e-294">在瀏覽器中，您會看到類別目錄瀏覽功能表。</span><span class="sxs-lookup"><span data-stu-id="20c5e-294">In the browser, you will see a category navigation menu.</span></span> <span data-ttu-id="20c5e-295">這個功能表是由從資料庫擷取的類別產生。</span><span class="sxs-lookup"><span data-stu-id="20c5e-295">This menu was generated by retrieving the categories from the database.</span></span> <span data-ttu-id="20c5e-296">在下一個教學課程中，您將實作瀏覽。</span><span class="sxs-lookup"><span data-stu-id="20c5e-296">In the next tutorial, you will implement the navigation.</span></span>
3. <span data-ttu-id="20c5e-297">關閉瀏覽器以停止執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="20c5e-297">Close the browser to stop the running application.</span></span>

### <a name="reviewing-the-database"></a><span data-ttu-id="20c5e-298">檢閱資料庫</span><span class="sxs-lookup"><span data-stu-id="20c5e-298">Reviewing the Database</span></span>

<span data-ttu-id="20c5e-299">開啟*Web.config*檔案，並查看連接字串部分。</span><span class="sxs-lookup"><span data-stu-id="20c5e-299">Open the *Web.config* file and look at the connection string section.</span></span> <span data-ttu-id="20c5e-300">您可以看到`AttachDbFilename`連接字串中的值指向`DataDirectory`Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="20c5e-300">You can see that the `AttachDbFilename` value in the connection string points to the `DataDirectory` for the Web application project.</span></span> <span data-ttu-id="20c5e-301">值`|DataDirectory|`是保留的值，代表*應用程式\_資料*專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="20c5e-301">The value `|DataDirectory|` is a reserved value that represents the *App\_Data* folder in the project.</span></span> <span data-ttu-id="20c5e-302">此資料夾是由實體類別所建立的資料庫所在的位置。</span><span class="sxs-lookup"><span data-stu-id="20c5e-302">This folder is where the database that was created from your entity classes is located.</span></span>

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> <span data-ttu-id="20c5e-303">如果*應用程式\_資料*看不到資料夾或資料夾是空的如果選取**重新整理**圖示，然後**顯示所有檔案**上方圖示**方案總管 中**視窗。</span><span class="sxs-lookup"><span data-stu-id="20c5e-303">If the *App\_Data* folder is not visible or if the folder is empty, select the **Refresh** icon and then the **Show All Files** icon at the top of the **Solution Explorer** window.</span></span> <span data-ttu-id="20c5e-304">展開的寬度**方案總管 中**windows 可能需要以顯示所有可用的圖示。</span><span class="sxs-lookup"><span data-stu-id="20c5e-304">Expanding the width of the **Solution Explorer** windows may be required to show all available icons.</span></span>


<span data-ttu-id="20c5e-305">現在您可以檢查所包含的資料*wingtiptoys.mdf*資料庫檔案使用**伺服器總管**視窗。</span><span class="sxs-lookup"><span data-stu-id="20c5e-305">Now you can inspect the data contained in the *wingtiptoys.mdf* database file by using the **Server Explorer** window.</span></span>

1. <span data-ttu-id="20c5e-306">展開*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-306">Expand the *App\_Data* folder.</span></span> <span data-ttu-id="20c5e-307">如果*應用程式\_資料*看不到資料夾，請參閱上述的注意事項。</span><span class="sxs-lookup"><span data-stu-id="20c5e-307">If the *App\_Data* folder is not visible, see the note above.</span></span>
2. <span data-ttu-id="20c5e-308">如果*wingtiptoys.mdf*資料庫檔案是不可見，並選取**重新整理**圖示，然後**顯示所有檔案**上方的圖示**方案總管**視窗。</span><span class="sxs-lookup"><span data-stu-id="20c5e-308">If the *wingtiptoys.mdf* database file is not visible, select the **Refresh** icon and then the **Show All Files** icon at the top of the **Solution Explorer** window.</span></span>
3. <span data-ttu-id="20c5e-309">以滑鼠右鍵按一下*wingtiptoys.mdf*資料庫檔案，然後選取**開啟**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-309">Right-click the *wingtiptoys.mdf* database file and select **Open**.</span></span>  
    <span data-ttu-id="20c5e-310">**伺服器總管**隨即出現。</span><span class="sxs-lookup"><span data-stu-id="20c5e-310">**Server Explorer** is displayed.</span></span> 

    ![UI 和瀏覽-伺服器總管](ui_and_navigation/_static/image8.png)
4. <span data-ttu-id="20c5e-312">展開*資料表*資料夾。</span><span class="sxs-lookup"><span data-stu-id="20c5e-312">Expand the *Tables* folder.</span></span>
5. <span data-ttu-id="20c5e-313">以滑鼠右鍵按一下**產品**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-313">Right-click the **Products**table and select **Show Table Data**.</span></span>  
 <span data-ttu-id="20c5e-314">**產品**資料表就會顯示。</span><span class="sxs-lookup"><span data-stu-id="20c5e-314">The **Products** table is displayed.</span></span> 

    ![UI 和瀏覽-Products 資料表](ui_and_navigation/_static/image9.png)
6. <span data-ttu-id="20c5e-316">此檢視可讓您檢視及修改中的資料**產品**資料表以手動方式。</span><span class="sxs-lookup"><span data-stu-id="20c5e-316">This view lets you see and modify the data in the **Products** table by hand.</span></span>
7. <span data-ttu-id="20c5e-317">關閉**產品**資料表視窗中。</span><span class="sxs-lookup"><span data-stu-id="20c5e-317">Close the **Products** table window.</span></span>
8. <span data-ttu-id="20c5e-318">在**伺服器總管**，以滑鼠右鍵按一下**產品**資料表一次，然後選取**開啟資料表定義**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-318">In the **Server Explorer**, right-click the **Products** table again and select **Open Table Definition**.</span></span>  
 <span data-ttu-id="20c5e-319">資料設計**產品**資料表就會顯示。</span><span class="sxs-lookup"><span data-stu-id="20c5e-319">The data design for the **Products** table is displayed.</span></span> 

    ![UI 和瀏覽-產品設計](ui_and_navigation/_static/image10.png)
9. <span data-ttu-id="20c5e-321">在**T-SQL**  索引標籤，您會看到用來建立資料表的 SQL DDL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="20c5e-321">In the **T-SQL** tab you will see the SQL DDL statement that was used to create the table.</span></span> <span data-ttu-id="20c5e-322">您也可以使用中的 UI**設計**索引標籤來修改結構描述。</span><span class="sxs-lookup"><span data-stu-id="20c5e-322">You can also use the UI in the **Design** tab to modify the schema.</span></span>
10. <span data-ttu-id="20c5e-323">在**伺服器總管**，以滑鼠右鍵按一下**WingtipToys**資料庫，並選取**關閉連接**。</span><span class="sxs-lookup"><span data-stu-id="20c5e-323">In the **Server Explorer**, right-click **WingtipToys** database and select **Close Connection**.</span></span>   
 <span data-ttu-id="20c5e-324">卸離資料庫。 請從 Visual Studio，資料庫結構描述將能夠修改稍後在本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="20c5e-324">By detaching the database from Visual Studio, the database schema will be able to be modified later in this tutorial series.</span></span>
11. <span data-ttu-id="20c5e-325">返回**方案總管 中**選取**方案總管 中** 索引標籤底部**伺服器總管**視窗。</span><span class="sxs-lookup"><span data-stu-id="20c5e-325">Return to **Solution Explorer**by selecting the **Solution Explorer** tab at the bottom of the **Server Explorer** window.</span></span>

## <a name="summary"></a><span data-ttu-id="20c5e-326">總結</span><span class="sxs-lookup"><span data-stu-id="20c5e-326">Summary</span></span>

<span data-ttu-id="20c5e-327">您已經在本教學課程中的數列加入一些基本的 UI、 圖形、 頁面和導覽。</span><span class="sxs-lookup"><span data-stu-id="20c5e-327">In this tutorial of the series you have added some basic UI, graphics, pages, and navigation.</span></span> <span data-ttu-id="20c5e-328">此外，您要執行 Web 應用程式，從您在上一個教學課程中加入的資料類別建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="20c5e-328">Additionally, you ran the Web application, which created the database from the data classes that you added in the previous tutorial.</span></span> <span data-ttu-id="20c5e-329">您也可以檢視的內容*產品*藉由直接檢視資料庫的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="20c5e-329">You also viewed the contents of the *Products* table of the database by viewing the database directly.</span></span> <span data-ttu-id="20c5e-330">在下一個教學課程中，您要顯示資料的項目以及從資料庫的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="20c5e-330">In the next tutorial, you'll display data items and details from the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20c5e-331">其他資源</span><span class="sxs-lookup"><span data-stu-id="20c5e-331">Additional Resources</span></span>

<span data-ttu-id="20c5e-332">[介紹程式設計 ASP.NET Web Pages](https://msdn.microsoft.com/library/ms178125.aspx) </span><span class="sxs-lookup"><span data-stu-id="20c5e-332">[Introduction to Programming ASP.NET Web Pages](https://msdn.microsoft.com/library/ms178125.aspx) </span></span>  
<span data-ttu-id="20c5e-333">[ASP.NET Web 伺服器控制項概觀](https://msdn.microsoft.com/library/zsyt68f1.aspx) </span><span class="sxs-lookup"><span data-stu-id="20c5e-333">[ASP.NET Web Server Controls Overview](https://msdn.microsoft.com/library/zsyt68f1.aspx) </span></span>  
[<span data-ttu-id="20c5e-334">CSS 教學課程</span><span class="sxs-lookup"><span data-stu-id="20c5e-334">CSS Tutorial</span></span>](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> <span data-ttu-id="20c5e-335">[上一頁](create_the_data_access_layer.md)
> [下一頁](display_data_items_and_details.md)</span><span class="sxs-lookup"><span data-stu-id="20c5e-335">[Previous](create_the_data_access_layer.md)
[Next](display_data_items_and_details.md)</span></span>
