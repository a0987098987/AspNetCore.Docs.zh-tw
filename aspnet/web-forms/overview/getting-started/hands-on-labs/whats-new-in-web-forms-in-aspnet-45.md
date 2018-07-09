---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: 在 ASP.NET 4.5 的新 Web Form |Microsoft Docs
author: rick-anderson
description: ASP.NET Web Form 的新版本導入了一些重點在於提升使用者經驗，使用資料時的增強功能。 在舊版的...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830408"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a><span data-ttu-id="bec79-104">在 asp.net 4.5 Web Form 中最新消息</span><span class="sxs-lookup"><span data-stu-id="bec79-104">What's New in Web Forms in ASP.NET 4.5</span></span>
====================
<span data-ttu-id="bec79-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="bec79-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="bec79-106">ASP.NET Web Form 的新版本導入了一些重點在於提升使用者經驗，使用資料時的增強功能。</span><span class="sxs-lookup"><span data-stu-id="bec79-106">The new version of ASP.NET Web Forms introduces a number of improvements focused on improving user experience when working with data.</span></span>
> 
> <span data-ttu-id="bec79-107">在舊版的 Web Form，以發出物件成員的值中使用資料繫結時，您可以使用 bind （） 或 eval （） 的資料繫結運算式。</span><span class="sxs-lookup"><span data-stu-id="bec79-107">In previous versions of Web Forms, when using data-binding to emit the value of an object member, you used the data-binding expressions Bind() or Eval().</span></span> <span data-ttu-id="bec79-108">在新的 ASP.NET 版本中，您就能夠宣告哪些類型的資料控制項即將使用新的 ItemType 屬性繫結至項目。</span><span class="sxs-lookup"><span data-stu-id="bec79-108">In the new version of ASP.NET, you are able to declare what type of data a control is going to be bound to by using a new ItemType property.</span></span> <span data-ttu-id="bec79-109">設定這個屬性可讓您使用強型別變數，以接收之完整優勢的 Visual Studio 開發經驗中，例如 IntelliSense、 成員瀏覽，以及編譯時間檢查。</span><span class="sxs-lookup"><span data-stu-id="bec79-109">Setting this property will enable you to use a strongly-typed variable to receive the full benefits of the Visual Studio development experience, such as IntelliSense, member navigation, and compile-time checking.</span></span>
> 
> <span data-ttu-id="bec79-110">資料繫結控制項中，您現在也可以指定您自己自訂的方法，如選取、 更新、 刪除和插入資料，簡化頁面控制項和應用程式邏輯之間的互動。</span><span class="sxs-lookup"><span data-stu-id="bec79-110">With the data-bound controls, you can now also specify your own custom methods for selecting, updating, deleting and inserting data, simplifying the interaction between the page controls and your application logic.</span></span> <span data-ttu-id="bec79-111">此外，ASP.NET 中，這表示您可以從頁面的資料直接對應至方法的型別參數已新增模型繫結功能。</span><span class="sxs-lookup"><span data-stu-id="bec79-111">Additionally, model binding capabilities have been added to ASP.NET, which means you can map data from the page directly into method type parameters.</span></span>
> 
> <span data-ttu-id="bec79-112">驗證使用者輸入也應該容易，因為最新版的 Web Form。</span><span class="sxs-lookup"><span data-stu-id="bec79-112">Validating user input should also be easier with the latest version of Web Forms.</span></span> <span data-ttu-id="bec79-113">您現在可以加上註解您的模型類別，以驗證屬性從**System.ComponentModel.DataAnnotations**命名空間和您的網站控制的要求驗證使用者輸入使用該資訊。</span><span class="sxs-lookup"><span data-stu-id="bec79-113">You can now annotate your model classes with validation attributes from the **System.ComponentModel.DataAnnotations** namespace and request that all your site controls validate user input using that information.</span></span> <span data-ttu-id="bec79-114">與 jQuery，提供清除器用戶端程式碼和不顯眼的 JavaScript 功能，現在已整合在 Web Form 中的用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-114">Client-side validation in Web Forms is now integrated with jQuery, providing cleaner client-side code and unobtrusive JavaScript features.</span></span>
> 
> <span data-ttu-id="bec79-115">在要求驗證區域中，進行了改進以便選擇性地關閉您的應用程式的特定部分的要求驗證，或讀取無效的要求資料變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="bec79-115">In the request validation area, improvements have been made to make it easier to selectively turn off request validation for specific parts of your applications or read invalidated request data.</span></span>
> 
> <span data-ttu-id="bec79-116">一些改善已對 Web Form 伺服器控制項，以充分利用的 HTML5 的新功能：</span><span class="sxs-lookup"><span data-stu-id="bec79-116">Some improvements have been made to Web Forms server controls to take advantage of new features of HTML5:</span></span>
> 
> - <span data-ttu-id="bec79-117">TextBox 控制項的 TextMode 屬性已更新以支援新 HTML5 輸入的類型，例如電子郵件、 datetime、 依此類推。</span><span class="sxs-lookup"><span data-stu-id="bec79-117">The TextMode property of the TextBox control has been updated to support the new HTML5 input types like email, datetime, and so on.</span></span>
> - <span data-ttu-id="bec79-118">FileUpload 控制項現在支援從瀏覽器支援此 HTML5 功能的多個檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="bec79-118">The FileUpload control now supports multiple file uploads from browsers that support this HTML5 feature.</span></span>
> - <span data-ttu-id="bec79-119">驗證程式控制項現在支援驗證 HTML5 輸入項目。</span><span class="sxs-lookup"><span data-stu-id="bec79-119">Validator controls now support validating HTML5 input elements.</span></span>
> - <span data-ttu-id="bec79-120">新的 HTML5 項目具有表示屬性的 URL 現在支援 runat =&quot;server&quot;。</span><span class="sxs-lookup"><span data-stu-id="bec79-120">New HTML5 elements that have attributes that represent a URL now support runat=&quot;server&quot;.</span></span> <span data-ttu-id="bec79-121">如此一來，您可以使用 ASP.NET 慣例在 URL 路徑，例如 ~ 來代表應用程式根目錄運算子 (例如&lt;視訊 runat =&quot;伺服器&quot;src =&quot;~/myVideo.wmv&quot; &gt; &lt;/視訊&gt;)。</span><span class="sxs-lookup"><span data-stu-id="bec79-121">As a result, you can use ASP.NET conventions in URL paths, like the ~ operator to represent the application root (for example, &lt;video runat=&quot;server&quot; src=&quot;~/myVideo.wmv&quot;&gt;&lt;/video&gt;).</span></span>
> - <span data-ttu-id="bec79-122">已修正 UpdatePanel 控制項來支援張貼的 HTML5 輸入的欄位。</span><span class="sxs-lookup"><span data-stu-id="bec79-122">The UpdatePanel control has been fixed to support posting HTML5 input fields.</span></span>
> 
> <span data-ttu-id="bec79-123">您可以在官方 ASP.NET 入口網站中找到的新功能的更多範例 ASP.NET WebForms 4.5 中： [What's New in ASP.NET 4.5 和 Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)</span><span class="sxs-lookup"><span data-stu-id="bec79-123">In the official ASP.NET portal you can find more examples of the new features in ASP.NET WebForms 4.5: [What's New in ASP.NET 4.5 and Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)</span></span>
> 
> <span data-ttu-id="bec79-124">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="bec79-124">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="bec79-125">目標</span><span class="sxs-lookup"><span data-stu-id="bec79-125">Objectives</span></span>

<span data-ttu-id="bec79-126">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="bec79-126">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="bec79-127">使用強型別資料繫結運算式</span><span class="sxs-lookup"><span data-stu-id="bec79-127">Use strongly-typed data-binding expressions</span></span>
- <span data-ttu-id="bec79-128">在 Web Form 中使用模型繫結的新功能</span><span class="sxs-lookup"><span data-stu-id="bec79-128">Use new model binding features in Web Forms</span></span>
- <span data-ttu-id="bec79-129">將頁面資料對應到程式碼後置方法時，用於值提供者</span><span class="sxs-lookup"><span data-stu-id="bec79-129">Use value providers for mapping page data to code-behind methods</span></span>
- <span data-ttu-id="bec79-130">使用資料註解來驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="bec79-130">Use Data Annotations for user input validation</span></span>
- <span data-ttu-id="bec79-131">在 Web Form 中採取 unobstrusive 用戶端驗證，與 jQuery 的 advange</span><span class="sxs-lookup"><span data-stu-id="bec79-131">Take advange of unobstrusive client-side validation with jQuery in Web Forms</span></span>
- <span data-ttu-id="bec79-132">實作詳細的要求驗證</span><span class="sxs-lookup"><span data-stu-id="bec79-132">Implement granular request validation</span></span>
- <span data-ttu-id="bec79-133">實作非同步頁面處理 Web Form 中</span><span class="sxs-lookup"><span data-stu-id="bec79-133">Implement asynchronous page processing in Web Forms</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="bec79-134">必要條件</span><span class="sxs-lookup"><span data-stu-id="bec79-134">Prerequisites</span></span>

<span data-ttu-id="bec79-135">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="bec79-135">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="bec79-136">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。</span><span class="sxs-lookup"><span data-stu-id="bec79-136">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="bec79-137">安裝程式</span><span class="sxs-lookup"><span data-stu-id="bec79-137">Setup</span></span>

<span data-ttu-id="bec79-138">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="bec79-138">**Installing Code Snippets**</span></span>

<span data-ttu-id="bec79-139">為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="bec79-139">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="bec79-140">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="bec79-140">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="bec79-141">如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。</span><span class="sxs-lookup"><span data-stu-id="bec79-141">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="bec79-142">練習</span><span class="sxs-lookup"><span data-stu-id="bec79-142">Exercises</span></span>

<span data-ttu-id="bec79-143">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="bec79-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="bec79-144">練習 1： 建立 ASP.NET Web Form 中的繫結的模型</span><span class="sxs-lookup"><span data-stu-id="bec79-144">Exercise 1: Model Binding in ASP.NET Web Forms</span></span>](#Exercise1)
2. [<span data-ttu-id="bec79-145">練習 2： 資料驗證</span><span class="sxs-lookup"><span data-stu-id="bec79-145">Exercise 2: Data Validation</span></span>](#Exercise2)
3. [<span data-ttu-id="bec79-146">練習 3： 非同步頁面處理在 ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="bec79-146">Exercise 3: Asynchronous Page Processing in ASP.NET Web Forms</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="bec79-147">每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="bec79-147">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="bec79-148">如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。</span><span class="sxs-lookup"><span data-stu-id="bec79-148">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="bec79-149">完成這個實驗室估計時間： **60 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="bec79-149">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a><span data-ttu-id="bec79-150">練習 1： 建立 ASP.NET Web Form 中的繫結的模型</span><span class="sxs-lookup"><span data-stu-id="bec79-150">Exercise 1: Model Binding in ASP.NET Web Forms</span></span>

<span data-ttu-id="bec79-151">ASP.NET Web Form 的新版本導入了一些增強功能，著重於使用資料時，改善經驗。</span><span class="sxs-lookup"><span data-stu-id="bec79-151">The new version of ASP.NET Web Forms introduces a number of enhancements focused on improving the experience when working with data.</span></span> <span data-ttu-id="bec79-152">在這個練習中，您將了解強型別資料控制項，並模型繫結。</span><span class="sxs-lookup"><span data-stu-id="bec79-152">Throughout this exercise, you will learn about strongly typed data-controls and model binding.</span></span>

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a><span data-ttu-id="bec79-153">工作 1-使用強型別資料繫結</span><span class="sxs-lookup"><span data-stu-id="bec79-153">Task 1 - Using Strongly-Typed Data-Bindings</span></span>

<span data-ttu-id="bec79-154">在這個工作中，您會發現的新強型別繫結 ASP.NET 4.5 中所提供。</span><span class="sxs-lookup"><span data-stu-id="bec79-154">In this task, you will discover the new strongly-typed bindings available in ASP.NET 4.5.</span></span>

1. <span data-ttu-id="bec79-155">開啟**開始**解決方案位於**來源/Ex1-ModelBinding/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="bec79-155">Open the **Begin** solution located at **Source/Ex1-ModelBinding/Begin/** folder.</span></span>

   1. <span data-ttu-id="bec79-156">您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="bec79-156">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bec79-157">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="bec79-157">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bec79-158">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="bec79-158">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="bec79-159">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="bec79-159">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bec79-160">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="bec79-160">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bec79-161">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="bec79-161">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bec79-162">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="bec79-162">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="bec79-163">開啟**Customers.aspx**頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-163">Open the **Customers.aspx** page.</span></span> <span data-ttu-id="bec79-164">將未編號的清單中的主控制項，並包含重複項控制項內用於列出每個客戶。</span><span class="sxs-lookup"><span data-stu-id="bec79-164">Place an unnumbered list in the main control and include a repeater control inside for listing each customer.</span></span> <span data-ttu-id="bec79-165">中繼器的名稱設為**customersRepeater**如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="bec79-165">Set the repeater name to **customersRepeater** as shown in the following code.</span></span>

    <span data-ttu-id="bec79-166">在舊版的 Web Form，以發出物件成員的值中使用資料繫結時您是資料繫結，您會使用資料繫結運算式，以及呼叫 Eval 方法，傳遞該成員的名稱做為字串。</span><span class="sxs-lookup"><span data-stu-id="bec79-166">In previous versions of Web Forms, when using data-binding to emit the value of a member on an object you're data-binding to, you would use a data-binding expression, along with a call to the Eval method, passing in the name of the member as a string.</span></span>

    <span data-ttu-id="bec79-167">在執行階段，評估這些呼叫會使用反映，對目前繫結的物件來讀取具有指定名稱，該成員的值和 html 格式顯示結果。</span><span class="sxs-lookup"><span data-stu-id="bec79-167">At runtime, these calls to Eval will use reflection against the currently bound object to read the value of the member with the given name, and display the result in the HTML.</span></span> <span data-ttu-id="bec79-168">這種方法可讓您很容易針對任意、 unshaped 資料進行資料繫結。</span><span class="sxs-lookup"><span data-stu-id="bec79-168">This approach makes it very easy to data-bind against arbitrary, unshaped data.</span></span>

    <span data-ttu-id="bec79-169">不幸的是，您會遺失的許多絕佳的開發階段體驗功能，在 Visual Studio 中的成員名稱，讓您瀏覽 （例如移至定義），並編譯時間檢查，包括 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="bec79-169">Unfortunately, you lose many of the great development-time experience features in Visual Studio, including IntelliSense for member names, support for navigation (like Go To Definition), and compile-time checking.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. <span data-ttu-id="bec79-170">開啟**Customers.aspx.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="bec79-170">Open the **Customers.aspx.cs** file.</span></span>
4. <span data-ttu-id="bec79-171">新增下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bec79-171">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. <span data-ttu-id="bec79-172">在 **網頁\_負載**方法，加入程式碼以填入客戶清單與重複項。</span><span class="sxs-lookup"><span data-stu-id="bec79-172">In the **Page\_Load** method, add code to populate the repeater with the list of customers.</span></span>

    <span data-ttu-id="bec79-173">(程式碼片段- *Web Form 實驗室-Ex01-繫結的客戶資料來源*)</span><span class="sxs-lookup"><span data-stu-id="bec79-173">(Code Snippet - *Web Forms Lab - Ex01 - Bind Customers Data Source*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    <span data-ttu-id="bec79-174">解決方案會使用 EntityFramework CodeFirst 以及建立及存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="bec79-174">The solution uses EntityFramework together with CodeFirst to create and access the database.</span></span> <span data-ttu-id="bec79-175">下列程式碼，customersRepeater 繫結至具體化的查詢，傳回資料庫中的所有客戶。</span><span class="sxs-lookup"><span data-stu-id="bec79-175">In the following code, the customersRepeater is bound to a materialized query that returns all the customers from the database.</span></span>
6. <span data-ttu-id="bec79-176">按下**F5**來執行方案，然後前往**客戶**頁面，以查看作用中的重複項。</span><span class="sxs-lookup"><span data-stu-id="bec79-176">Press **F5** to run the solution and go to the **Customers** page to see the repeater in action.</span></span> <span data-ttu-id="bec79-177">解決方案是使用 CodeFirst，就會建立並執行應用程式時，本機 SQL Express 執行個體中填入資料庫。</span><span class="sxs-lookup"><span data-stu-id="bec79-177">As the solution is using CodeFirst, the database will be created and populated in your local SQL Express instance when running the application.</span></span>

    <span data-ttu-id="bec79-178">![列出的客戶 repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "列出 repeater 的客戶")</span><span class="sxs-lookup"><span data-stu-id="bec79-178">![Listing the customers with a repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Listing the customers with a repeater")</span></span>

    <span data-ttu-id="bec79-179">*列出 repeater 的客戶*</span><span class="sxs-lookup"><span data-stu-id="bec79-179">*Listing the customers with a repeater*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-180">在 Visual Studio 2012 中，IIS Express 是預設的 Web 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="bec79-180">In Visual Studio 2012, IIS Express is the default Web development server.</span></span>
7. <span data-ttu-id="bec79-181">關閉瀏覽器並移回至 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bec79-181">Close the browser and go back to Visual Studio.</span></span>
8. <span data-ttu-id="bec79-182">現在將使用強型別繫結的實作。</span><span class="sxs-lookup"><span data-stu-id="bec79-182">Now replace the implementation to use strongly typed bindings.</span></span> <span data-ttu-id="bec79-183">開啟**Customers.aspx**頁面上，然後使用新**ItemType**屬性設定中繼器中的**客戶**繫結類型為的型別。</span><span class="sxs-lookup"><span data-stu-id="bec79-183">Open the **Customers.aspx** page and use the new **ItemType** attribute in the repeater to set the **Customer** type as the binding type.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    <span data-ttu-id="bec79-184">ItemType 屬性可讓您宣告的資料類型是要繫結至控制項和可讓您使用強型別內的資料繫結控制項繫結。</span><span class="sxs-lookup"><span data-stu-id="bec79-184">The ItemType property enables you to declare which type of data the control is going to be bound to and allows you to use strongly-typed binding inside the data-bound control.</span></span>
9. <span data-ttu-id="bec79-185">取代為下列程式碼內容 ItemTemplate。</span><span class="sxs-lookup"><span data-stu-id="bec79-185">Replace the ItemTemplate content with the following code.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    <span data-ttu-id="bec79-186">使用上述方法的一項缺點是，eval 和 bind （） 呼叫晚期繫結-這表示您傳遞來表示屬性名稱的字串。</span><span class="sxs-lookup"><span data-stu-id="bec79-186">One downside with the above approaches is that the calls to Eval() and Bind() are late-bound - meaning you pass strings to represent the property names.</span></span> <span data-ttu-id="bec79-187">這表示您未取得成員名稱、 支援程式碼瀏覽 （例如移至定義），也不是編譯時期檢查支援 Intellisense。</span><span class="sxs-lookup"><span data-stu-id="bec79-187">This means you don't get Intellisense for the member names, support for code navigation (like Go To Definition), nor compile-time checking support.</span></span>

    <span data-ttu-id="bec79-188">設定 ItemType 屬性會導致兩個新的具型別的變數，來產生資料繫結運算式的範圍內：**項目**並**BindItem**。</span><span class="sxs-lookup"><span data-stu-id="bec79-188">Setting the ItemType property causes two new typed variables to be generated in the scope of the data-binding expressions: **Item** and **BindItem**.</span></span> <span data-ttu-id="bec79-189">您可以在資料繫結運算式中使用這些強類型的變數，並取得之完整優勢的 Visual Studio 開發經驗。</span><span class="sxs-lookup"><span data-stu-id="bec79-189">You can use these strongly typed variables in the data-binding expressions and get the full benefits of the Visual Studio development experience.</span></span>

    <span data-ttu-id="bec79-190">&quot; **:** &quot;運算式中使用會自動進行 HTML 編碼的輸出，以避免發生安全性問題 （例如，跨網站指令碼攻擊）。</span><span class="sxs-lookup"><span data-stu-id="bec79-190">The &quot;**:** &quot; used in the expression will automatically HTML-encode the output to avoid security issues (for example, cross-site scripting attacks).</span></span> <span data-ttu-id="bec79-191">這個標記法是自.NET 4 回應撰寫，但現在也會提供資料繫結運算式。</span><span class="sxs-lookup"><span data-stu-id="bec79-191">This notation was available since .NET 4 for response writing, but now is also available in data-binding expressions.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-192">項目成員適用於單向繫結。</span><span class="sxs-lookup"><span data-stu-id="bec79-192">The Item member works for one-way binding.</span></span> <span data-ttu-id="bec79-193">如果您想要執行使用雙向繫結**BindItem**成員。</span><span class="sxs-lookup"><span data-stu-id="bec79-193">If you want to perform two-way binding use the **BindItem** member.</span></span>

    <span data-ttu-id="bec79-194">![強型別繫結中的 IntelliSense 支援](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "強型別繫結中的 IntelliSense 支援")</span><span class="sxs-lookup"><span data-stu-id="bec79-194">![IntelliSense support in strongly-typed binding](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "IntelliSense support in strongly-typed binding")</span></span>

    <span data-ttu-id="bec79-195">*強型別繫結中的 IntelliSense 支援*</span><span class="sxs-lookup"><span data-stu-id="bec79-195">*IntelliSense support in strongly-typed binding*</span></span>
10. <span data-ttu-id="bec79-196">按下**F5**執行方案，並移至 [客戶] 頁面，以確定所做的變更在如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="bec79-196">Press **F5** to run the solution and go to the Customers page to make sure the changes work as expected.</span></span>

    <span data-ttu-id="bec79-197">![列出客戶詳細資料](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "列出客戶詳細資料")</span><span class="sxs-lookup"><span data-stu-id="bec79-197">![Listing customer details](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Listing customer details")</span></span>

    <span data-ttu-id="bec79-198">*列出客戶詳細資料*</span><span class="sxs-lookup"><span data-stu-id="bec79-198">*Listing customer details*</span></span>
11. <span data-ttu-id="bec79-199">關閉瀏覽器並移回至 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bec79-199">Close the browser and go back to Visual Studio.</span></span>

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a><span data-ttu-id="bec79-200">工作 2-在 Web Form 中繫結的模型簡介</span><span class="sxs-lookup"><span data-stu-id="bec79-200">Task 2 - Introducing Model Binding in Web Forms</span></span>

<span data-ttu-id="bec79-201">在舊版的 ASP.NET Web Form 中，當您想要執行雙向資料繫結，擷取和更新資料，您需要使用資料來源物件。</span><span class="sxs-lookup"><span data-stu-id="bec79-201">In previous versions of ASP.NET Web Forms, when you wanted to perform two-way data-binding, both retrieving and updating data, you needed to use a Data Source object.</span></span> <span data-ttu-id="bec79-202">這可能是物件資料來源、 SQL 資料來源、 LINQ 資料來源，依此類推。</span><span class="sxs-lookup"><span data-stu-id="bec79-202">This could be an Object Data Source, a SQL Data Source, a LINQ Data Source and so on.</span></span> <span data-ttu-id="bec79-203">不過如果您的案例需要自訂程式碼來處理資料，您必須使用物件資料來源中，這會帶來一些缺點。</span><span class="sxs-lookup"><span data-stu-id="bec79-203">However if your scenario required custom code for handling the data, you needed to use the Object Data Source and this brought some drawbacks.</span></span> <span data-ttu-id="bec79-204">例如，您需要避免使用複雜類型，而您需要執行驗證邏輯時處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="bec79-204">For example, you needed to avoid complex types and you needed to handle exceptions when executing validation logic.</span></span>

<span data-ttu-id="bec79-205">在新版本中的資料繫結控制項支援的 ASP.NET Web Form 模型繫結。</span><span class="sxs-lookup"><span data-stu-id="bec79-205">In the new version of ASP.NET Web Forms the data-bound controls support model binding.</span></span> <span data-ttu-id="bec79-206">這表示，您可以指定選取、 更新、 插入和刪除方法直接在呼叫邏輯，從您的程式碼後置檔案，或從另一個類別的資料繫結控制項中。</span><span class="sxs-lookup"><span data-stu-id="bec79-206">This means that you can specify select, update, insert and delete methods directly in the data-bound control to call logic from your code-behind file or from another class.</span></span>

<span data-ttu-id="bec79-207">若要深入了解，您將使用 GridView 列出使用新的產品類別**SelectMethod**屬性。</span><span class="sxs-lookup"><span data-stu-id="bec79-207">To learn about this, you will use a GridView to list the product categories using the new **SelectMethod** attribute.</span></span> <span data-ttu-id="bec79-208">這個屬性可讓您指定的擷取 GridView 資料方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-208">This attribute enables you to specify a method for retrieving the GridView data.</span></span>

1. <span data-ttu-id="bec79-209">開啟**Products.aspx**頁面上，而且包含**GridView**。</span><span class="sxs-lookup"><span data-stu-id="bec79-209">Open the **Products.aspx** page and include a **GridView**.</span></span> <span data-ttu-id="bec79-210">若要使用強型別繫結並啟用排序和分頁，如下所示，請設定 GridView。</span><span class="sxs-lookup"><span data-stu-id="bec79-210">Configure the GridView as shown below to use strongly-typed bindings and enable sorting and paging.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. <span data-ttu-id="bec79-211">使用新**SelectMethod**屬性來設定要呼叫 GridView **GetCategories**方法，以選取的資料。</span><span class="sxs-lookup"><span data-stu-id="bec79-211">Use the new **SelectMethod** attribute to configure the GridView to call a **GetCategories** method to select the data.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. <span data-ttu-id="bec79-212">開啟**Products.aspx.cs**程式碼後置檔案，並新增下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bec79-212">Open the **Products.aspx.cs** code-behind file and add the following using statements.</span></span>

    <span data-ttu-id="bec79-213">(程式碼片段- *Web Form 實驗室-Ex01-命名空間*)</span><span class="sxs-lookup"><span data-stu-id="bec79-213">(Code Snippet - *Web Forms Lab - Ex01 - Namespaces*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. <span data-ttu-id="bec79-214">加入私用成員**產品**類別，將指派的新執行個體**ProductsContext**。</span><span class="sxs-lookup"><span data-stu-id="bec79-214">Add a private member in the **Products** class and assign a new instance of **ProductsContext**.</span></span> <span data-ttu-id="bec79-215">這個屬性會儲存 Entity Framework 資料內容，可讓您連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="bec79-215">This property will store the Entity Framework data context that enables you to connect to the database.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. <span data-ttu-id="bec79-216">建立**GetCategories**方法來擷取使用 LINQ 的類別目錄的清單。</span><span class="sxs-lookup"><span data-stu-id="bec79-216">Create a **GetCategories** method to retrieve the list of categories using LINQ.</span></span> <span data-ttu-id="bec79-217">查詢將包含**產品**屬性，好 GridView 可以顯示每個類別目錄的產品數量。</span><span class="sxs-lookup"><span data-stu-id="bec79-217">The query will include the **Products** property so the GridView can show the amount of products for each category.</span></span> <span data-ttu-id="bec79-218">請注意，此方法會傳回未經處理的 IQueryable 物件，代表查詢之後執行網頁生命週期。</span><span class="sxs-lookup"><span data-stu-id="bec79-218">Notice that the method returns a raw IQueryable object that represent the query to be executed later on the page lifecycle.</span></span>

    <span data-ttu-id="bec79-219">(程式碼片段- *Web Form 實驗室-Ex01-GetCategories*)</span><span class="sxs-lookup"><span data-stu-id="bec79-219">(Code Snippet - *Web Forms Lab - Ex01 - GetCategories*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > <span data-ttu-id="bec79-220">在舊版的 ASP.NET Web Form，啟用排序和分頁使用您自己的存放庫邏輯，在物件資料來源內容，才能撰寫您自己的自訂程式碼，並接收所有必要的參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-220">In previous versions of ASP.NET Web Forms, enabling sorting and paging using your own repository logic within an Object Data Source context, required to write your own custom code and receive all the necessary parameters.</span></span> <span data-ttu-id="bec79-221">現在，當資料繫結方法可以傳回 IQueryable，而這表示查詢仍然是用來執行，ASP.NET 可以處理修改查詢，以便將適當的排序和分頁的參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-221">Now, as the data-binding methods can return IQueryable and this represents a query still to be executed, ASP.NET can take care of modifying the query to add the proper sorting and paging parameters.</span></span>
6. <span data-ttu-id="bec79-222">按下**F5**開始偵錯網站並移至 [產品] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-222">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="bec79-223">您應該會看到 GridView 會填入 GetCategories 方法所傳回的類別。</span><span class="sxs-lookup"><span data-stu-id="bec79-223">You should see that the GridView is populated with the categories returned by the GetCategories method.</span></span>

    <span data-ttu-id="bec79-224">![填入使用模型繫結 GridView](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "填入 GridView，使用模型繫結")</span><span class="sxs-lookup"><span data-stu-id="bec79-224">![Populating a GridView using model binding](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Populating a GridView using model binding")</span></span>

    <span data-ttu-id="bec79-225">*填入 GridView，使用模型繫結*</span><span class="sxs-lookup"><span data-stu-id="bec79-225">*Populating a GridView using model binding*</span></span>
7. <span data-ttu-id="bec79-226">按下**SHIFT**+**F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="bec79-226">Press **SHIFT**+**F5** Stop debugging.</span></span>

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a><span data-ttu-id="bec79-227">工作 3-在模型繫結中的值提供者</span><span class="sxs-lookup"><span data-stu-id="bec79-227">Task 3 - Value Providers in Model Binding</span></span>

<span data-ttu-id="bec79-228">模型繫結不僅可讓您指定自訂的方法，以使用您直接在資料繫結控制項中的資料，也可讓您將從頁面的資料對應至從這些方法的參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-228">Model binding not only enables you to specify custom methods to work with your data directly in the data-bound control, but also allows you to map data from the page into parameters from these methods.</span></span> <span data-ttu-id="bec79-229">在方法參數，您可以使用值提供者屬性指定值的資料來源。</span><span class="sxs-lookup"><span data-stu-id="bec79-229">On the method parameter, you can use value provider attributes to specify the value's data source.</span></span> <span data-ttu-id="bec79-230">例如: </span><span class="sxs-lookup"><span data-stu-id="bec79-230">For example:</span></span>

- <span data-ttu-id="bec79-231">在頁面上的控制項</span><span class="sxs-lookup"><span data-stu-id="bec79-231">Controls on the page</span></span>
- <span data-ttu-id="bec79-232">查詢字串值</span><span class="sxs-lookup"><span data-stu-id="bec79-232">Query string values</span></span>
- <span data-ttu-id="bec79-233">檢視資料</span><span class="sxs-lookup"><span data-stu-id="bec79-233">View data</span></span>
- <span data-ttu-id="bec79-234">工作階段狀態</span><span class="sxs-lookup"><span data-stu-id="bec79-234">Session state</span></span>
- <span data-ttu-id="bec79-235">Cookie</span><span class="sxs-lookup"><span data-stu-id="bec79-235">Cookies</span></span>
- <span data-ttu-id="bec79-236">已張貼的表單資料</span><span class="sxs-lookup"><span data-stu-id="bec79-236">Posted form data</span></span>
- <span data-ttu-id="bec79-237">檢視狀態</span><span class="sxs-lookup"><span data-stu-id="bec79-237">View state</span></span>
- <span data-ttu-id="bec79-238">此外，也支援自訂值提供者</span><span class="sxs-lookup"><span data-stu-id="bec79-238">Custom value providers are supported as well</span></span>

<span data-ttu-id="bec79-239">如果您已使用 ASP.NET MVC 4，您會注意到的模型繫結支援類似。</span><span class="sxs-lookup"><span data-stu-id="bec79-239">If you have used ASP.NET MVC 4, you will notice the model binding support is similar.</span></span> <span data-ttu-id="bec79-240">事實上，這些功能已取自 ASP.NET MVC，並移到**System.Web**可以也在 Web Form 上使用這些組件。</span><span class="sxs-lookup"><span data-stu-id="bec79-240">Indeed, these features were taken from ASP.NET MVC and moved into the **System.Web** assembly to be able to use them on Web Forms as well.</span></span>

<span data-ttu-id="bec79-241">在這個工作中，您將會更新 GridView，以篩選結果的每個類別中的產品數量來使用模型繫結接收 filter 參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-241">In this task, you will update the GridView to filter its results by the amount of products for each category, receiving the filter parameter with model binding.</span></span>

1. <span data-ttu-id="bec79-242">請返回**Products.aspx**頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-242">Go back to the **Products.aspx** page.</span></span>
2. <span data-ttu-id="bec79-243">在 GridView 的頂端，新增**標籤**並**ComboBox**選取每個類別目錄的產品數目，如下所示。</span><span class="sxs-lookup"><span data-stu-id="bec79-243">At the top of the GridView, add a **Label** and a **ComboBox** to select the number of products for each category as shown below.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. <span data-ttu-id="bec79-244">新增**EmptyDataTemplate**至 GridView 與選取的產品數目沒有任何類別時，顯示一則訊息。</span><span class="sxs-lookup"><span data-stu-id="bec79-244">Add an **EmptyDataTemplate** to the GridView to show a message when there are no categories with the selected number of products.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. <span data-ttu-id="bec79-245">開啟**Products.aspx.cs**程式碼後置，並新增下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bec79-245">Open the **Products.aspx.cs** code-behind and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. <span data-ttu-id="bec79-246">修改**GetCategories**方法來接收整數**minProductsCount**引數和篩選傳回的結果。</span><span class="sxs-lookup"><span data-stu-id="bec79-246">Modify the **GetCategories** method to receive an integer **minProductsCount** argument and filter the returned results.</span></span> <span data-ttu-id="bec79-247">若要這樣做，請以下列程式碼取代方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-247">To do this, replace the method with the following code.</span></span>

    <span data-ttu-id="bec79-248">(程式碼片段- *Web Form 實驗室-Ex01-GetCategories 2*)</span><span class="sxs-lookup"><span data-stu-id="bec79-248">(Code Snippet - *Web Forms Lab - Ex01 - GetCategories 2*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    <span data-ttu-id="bec79-249">新 **[控制項]** 屬性上**minProductsCount**引數可讓 ASP.NET 知道必須在網頁中使用控制項填入其值。</span><span class="sxs-lookup"><span data-stu-id="bec79-249">The new **[Control]** attribute on the **minProductsCount** argument will let ASP.NET know its value must be populated using a control in the page.</span></span> <span data-ttu-id="bec79-250">ASP.NET 會尋找相符的引數 (minProductsCount) 名稱的任何控制項，並執行必要的對應和轉換来填入的控制項值的參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-250">ASP.NET will look for any control matching the name of the argument (minProductsCount) and perform the necessary mapping and conversion to fill the parameter with the control value.</span></span>

    <span data-ttu-id="bec79-251">或者，屬性會提供可讓您指定控制項位置，以取得此值的多載建構函式。</span><span class="sxs-lookup"><span data-stu-id="bec79-251">Alternatively, the attribute provides an overloaded constructor that enables you to specify the control from where to get the value.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-252">資料繫結功能的其中一個目標是減少需要寫入的頁面互動的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="bec79-252">One goal of the data-binding features is to reduce the amount of code that needs to be written for page interaction.</span></span> <span data-ttu-id="bec79-253">除了 [控制項] 值提供者，您可以使用其他的模型繫結提供者在方法參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-253">Apart from the [Control] value provider, you can use other model-binding providers in your method parameters.</span></span> <span data-ttu-id="bec79-254">其中部分所述工作的簡介。</span><span class="sxs-lookup"><span data-stu-id="bec79-254">Some of them are listed in the task introduction.</span></span>
6. <span data-ttu-id="bec79-255">按下**F5**開始偵錯網站並移至 [產品] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-255">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="bec79-256">選取下拉式清單中的產品數目，並注意如何 GridView 現在已更新。</span><span class="sxs-lookup"><span data-stu-id="bec79-256">Select a number of products in the drop-down list and notice how the GridView is now updated.</span></span>

    <span data-ttu-id="bec79-257">![篩選下拉式清單值 GridView](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "篩選下拉式清單值 GridView")</span><span class="sxs-lookup"><span data-stu-id="bec79-257">![Filtering the GridView with a drop-down list value](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtering the GridView with a drop-down list value")</span></span>

    <span data-ttu-id="bec79-258">*篩選下拉式清單值 GridView*</span><span class="sxs-lookup"><span data-stu-id="bec79-258">*Filtering the GridView with a drop-down list value*</span></span>
7. <span data-ttu-id="bec79-259">停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="bec79-259">Stop debugging.</span></span>

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a><span data-ttu-id="bec79-260">工作 4-使用模型繫結進行篩選</span><span class="sxs-lookup"><span data-stu-id="bec79-260">Task 4 - Using Model Binding for Filtering</span></span>

<span data-ttu-id="bec79-261">在這個工作中，您會將第二個，子 GridView，以顯示所選分類內的產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-261">In this task, you will add a second, child GridView to show the products within the selected category.</span></span>

1. <span data-ttu-id="bec79-262">開啟**Products.aspx**頁面上，並更新分類來自動產生 [選取] 按鈕的 GridView。</span><span class="sxs-lookup"><span data-stu-id="bec79-262">Open the **Products.aspx** page and update the categories GridView to auto-generate the Select button.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. <span data-ttu-id="bec79-263">新增第二**GridView**名為**productsGrid**底部。</span><span class="sxs-lookup"><span data-stu-id="bec79-263">Add a second **GridView** named **productsGrid** at the bottom.</span></span> <span data-ttu-id="bec79-264">設定**ItemType**要**WebFormsLab.Model.Product**，則**DataKeyNames**至**ProductId**和**SelectMethod**要**GetProducts**。</span><span class="sxs-lookup"><span data-stu-id="bec79-264">Set the **ItemType** to **WebFormsLab.Model.Product**, the **DataKeyNames** to **ProductId** and the **SelectMethod** to **GetProducts**.</span></span> <span data-ttu-id="bec79-265">設定**AutoGenerateColumns**要**false**和 ProductId、 ProductName、 描述和 UnitPrice 加入資料行。</span><span class="sxs-lookup"><span data-stu-id="bec79-265">Set **AutoGenerateColumns** to **false** and add the columns for ProductId, ProductName, Description and UnitPrice.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. <span data-ttu-id="bec79-266">開啟**Products.aspx.cs**程式碼後置檔案。</span><span class="sxs-lookup"><span data-stu-id="bec79-266">Open the **Products.aspx.cs** code-behind file.</span></span> <span data-ttu-id="bec79-267">實作**GetProducts**方法，以獲取 GridView 的類別目錄中的類別目錄識別碼，並篩選產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-267">Implement the **GetProducts** method to receive the category ID from the category GridView and filter the products.</span></span> <span data-ttu-id="bec79-268">模型繫結會設定使用選取的資料列中的參數值**categoriesGrid**。</span><span class="sxs-lookup"><span data-stu-id="bec79-268">Model binding will set the parameter value using the selected row in the **categoriesGrid**.</span></span> <span data-ttu-id="bec79-269">因為不相符的引數名稱和控制項名稱，您應該在控制項的值提供者中指定控制項的名稱。</span><span class="sxs-lookup"><span data-stu-id="bec79-269">Since the argument name and control name do not match, you should specify the name of the control in the Control value provider.</span></span>

    <span data-ttu-id="bec79-270">(程式碼片段- *Web Form 實驗室-Ex01-GetProducts*)</span><span class="sxs-lookup"><span data-stu-id="bec79-270">(Code Snippet - *Web Forms Lab - Ex01 - GetProducts*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="bec79-271">這種方法能夠簡化單元測試這些方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-271">This approach makes it easier to unit test these methods.</span></span> <span data-ttu-id="bec79-272">單元測試內容，其中未執行 Web Form，在 [控制項] 屬性不會執行任何特定動作。</span><span class="sxs-lookup"><span data-stu-id="bec79-272">On a unit test context, where Web Forms is not executing, the [Control] attribute will not perform any specific action.</span></span>
4. <span data-ttu-id="bec79-273">開啟**Products.aspx**頁面上，並找出產品 GridView。</span><span class="sxs-lookup"><span data-stu-id="bec79-273">Open the **Products.aspx** page and locate the products GridView.</span></span> <span data-ttu-id="bec79-274">更新 GridView，以顯示連結，以編輯所選的產品的產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-274">Update the products GridView to show a link for editing the selected product.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. <span data-ttu-id="bec79-275">開啟**ProductDetails.aspx**頁面上的程式碼後置，並取代**SelectProduct**為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-275">Open the **ProductDetails.aspx** page code-behind and replace the **SelectProduct** method with the following code.</span></span>

    <span data-ttu-id="bec79-276">(程式碼片段- *Web Form 實驗室-Ex01-SelectProduct 方法*)</span><span class="sxs-lookup"><span data-stu-id="bec79-276">(Code Snippet - *Web Forms Lab - Ex01 - SelectProduct Method*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="bec79-277">請注意， **[查詢字串]** 屬性用來填滿的查詢字串中的 productId 參數的方法參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-277">Notice that the **[QueryString]** attribute is used to fill the method parameter from a productId parameter in the query string.</span></span>
6. <span data-ttu-id="bec79-278">按下**F5**開始偵錯網站並移至 [產品] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-278">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="bec79-279">從 GridView 的類別中選取任何分類，並請注意，要更新的產品 GridView。</span><span class="sxs-lookup"><span data-stu-id="bec79-279">Select any category from the categories GridView and notice that the products GridView is updated.</span></span>

    <span data-ttu-id="bec79-280">![顯示所選分類的產品](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "顯示所選分類的產品")</span><span class="sxs-lookup"><span data-stu-id="bec79-280">![Showing products from the selected category](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Showing products from the selected category")</span></span>

    <span data-ttu-id="bec79-281">*顯示所選分類的產品*</span><span class="sxs-lookup"><span data-stu-id="bec79-281">*Showing products from the selected category*</span></span>
7. <span data-ttu-id="bec79-282">按一下 [**檢視**產品，以開啟 ProductDetails.aspx] 頁面上的連結。</span><span class="sxs-lookup"><span data-stu-id="bec79-282">Click the **View** link on a product to open the ProductDetails.aspx page.</span></span>

    <span data-ttu-id="bec79-283">請注意頁面與使用 productId 參數從查詢字串 SelectMethod，擷取產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-283">Notice that the page is retrieving the product with the SelectMethod using the productId parameter from the query string.</span></span>

    <span data-ttu-id="bec79-284">![檢視產品詳細資料](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "檢視產品詳細資料")</span><span class="sxs-lookup"><span data-stu-id="bec79-284">![Viewing the product details](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Viewing the product details")</span></span>

    <span data-ttu-id="bec79-285">*檢視產品詳細資料*</span><span class="sxs-lookup"><span data-stu-id="bec79-285">*Viewing the product details*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-286">在下一個練習中，將會實作能夠輸入 HTML 的描述。</span><span class="sxs-lookup"><span data-stu-id="bec79-286">The ability to type an HTML description will be implemented in the next exercise.</span></span>

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a><span data-ttu-id="bec79-287">工作 5-使用模型繫結進行更新作業</span><span class="sxs-lookup"><span data-stu-id="bec79-287">Task 5 - Using Model Binding for Update Operations</span></span>

<span data-ttu-id="bec79-288">在先前工作中，您有使用模型繫結主要是針對選取的資料，在這項工作中，您將學習如何在更新作業中使用模型繫結。</span><span class="sxs-lookup"><span data-stu-id="bec79-288">In the previous task, you have used model binding mainly for selecting data, in this task you will learn how to use model binding in update operations.</span></span>

<span data-ttu-id="bec79-289">您將會更新，讓使用者更新類別目錄的 GridView 的類別。</span><span class="sxs-lookup"><span data-stu-id="bec79-289">You will update the categories GridView to let the user update categories.</span></span>

1. <span data-ttu-id="bec79-290">開啟**Products.aspx**頁面上，並更新分類來自動產生 [編輯] 按鈕，並使用新的 GridView **UpdateMethod**屬性來指定**UpdateCategory**方法來更新選取的項目。</span><span class="sxs-lookup"><span data-stu-id="bec79-290">Open the **Products.aspx** page and update the categories GridView to auto-generate the Edit button and use the new **UpdateMethod** attribute to specify an **UpdateCategory** method to update the selected item.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    <span data-ttu-id="bec79-291">在 gridview 裡的 DataKeyNames 屬性會定義哪些是可唯一識別的模型繫結物件的成員，因此，這應該至少會收到更新方法的參數。</span><span class="sxs-lookup"><span data-stu-id="bec79-291">The DataKeyNames attribute in the GridView define which are the members that uniquely identify the model-bound object and therefore, which are the parameters the update method should at least receive.</span></span>
2. <span data-ttu-id="bec79-292">開啟**Products.aspx.cs**程式碼後置檔案，並實作**UpdateCategory**方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-292">Open the **Products.aspx.cs** code-behind file and implement the **UpdateCategory** method.</span></span> <span data-ttu-id="bec79-293">此方法應該會收到載入目前的類別填入 GridView 中的值，然後更新分類的類別目錄識別碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-293">The method should receive the category ID to load the current category, populate the values from the GridView and then update the category.</span></span>

    <span data-ttu-id="bec79-294">(程式碼片段- *Web Form 實驗室-Ex01-UpdateCategory*)</span><span class="sxs-lookup"><span data-stu-id="bec79-294">(Code Snippet - *Web Forms Lab - Ex01 - UpdateCategory*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    <span data-ttu-id="bec79-295">新**TryUpdateModel**頁面類別中的方法會負責擴展模型物件使用中的頁面從控制項的值。</span><span class="sxs-lookup"><span data-stu-id="bec79-295">The new **TryUpdateModel** method in the Page class is responsible of populating the model object using the values from the controls in the page.</span></span> <span data-ttu-id="bec79-296">在此情況下，它會取代為正在編輯目前的 GridView 資料列的更新的值**分類**物件。</span><span class="sxs-lookup"><span data-stu-id="bec79-296">In this case, it will replace the updated values from the current GridView row being edited into the **category** object.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-297">下一個練習中會說明來驗證使用者輸入時編輯的物件資料 ModelState.IsValid 的使用方式。</span><span class="sxs-lookup"><span data-stu-id="bec79-297">The next exercise will explain the usage of the ModelState.IsValid for validating the data entered by the user when editing the object.</span></span>
3. <span data-ttu-id="bec79-298">執行網站並移至 [產品] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-298">Run the site and go to the Products page.</span></span> <span data-ttu-id="bec79-299">編輯類別目錄。</span><span class="sxs-lookup"><span data-stu-id="bec79-299">Edit a category.</span></span> <span data-ttu-id="bec79-300">輸入新名稱，然後按一下**更新**來保存變更。</span><span class="sxs-lookup"><span data-stu-id="bec79-300">Type a new name and then click **Update** to persist the changes.</span></span>

    <span data-ttu-id="bec79-301">![編輯類別目錄](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "編輯類別目錄")</span><span class="sxs-lookup"><span data-stu-id="bec79-301">![Editing categories](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Editing categories")</span></span>

    <span data-ttu-id="bec79-302">*編輯類別目錄*</span><span class="sxs-lookup"><span data-stu-id="bec79-302">*Editing categories*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a><span data-ttu-id="bec79-303">練習 2： 資料驗證</span><span class="sxs-lookup"><span data-stu-id="bec79-303">Exercise 2: Data Validation</span></span>

<span data-ttu-id="bec79-304">在此練習中，您將了解 ASP.NET 4.5 中的新資料驗證功能。</span><span class="sxs-lookup"><span data-stu-id="bec79-304">In this exercise, you will learn about the new data validation features in ASP.NET 4.5.</span></span> <span data-ttu-id="bec79-305">您將會檢查在 Web Form 中的新低調驗證功能。</span><span class="sxs-lookup"><span data-stu-id="bec79-305">You will check out the new unobtrusive validation features in Web Forms.</span></span> <span data-ttu-id="bec79-306">您可以使用資料註解應用程式模型類別中的使用者輸入驗證，以及最後，您將了解如何開啟或關閉在網頁中的個別控制項的要求驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-306">You will use data annotations in the application model classes for user input validation, and finally, you will learn how to turn on or off request validation to individual controls in a page.</span></span>

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a><span data-ttu-id="bec79-307">工作 1-低調驗證</span><span class="sxs-lookup"><span data-stu-id="bec79-307">Task 1 - Unobtrusive Validation</span></span>

<span data-ttu-id="bec79-308">具有複雜的資料，包括驗證程式的表單通常會在頁面中，可以代表約 60%的程式碼產生太多的 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-308">Forms with complex data including validators tend to generate too much JavaScript code in the page, which can represent about 60% of the code.</span></span> <span data-ttu-id="bec79-309">啟用不顯眼的驗證，您的 HTML 程式碼看起來更簡潔且一個更有條理。</span><span class="sxs-lookup"><span data-stu-id="bec79-309">With unobtrusive validation enabled, your HTML code will look cleaner and tidier.</span></span>

<span data-ttu-id="bec79-310">在本節中，您將啟用低調驗證在 ASP.NET 中要比較這兩個組態所產生的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-310">In this section, you will enable unobtrusive validation in ASP.NET to compare the HTML code generated by both configurations.</span></span>

1. <span data-ttu-id="bec79-311">開啟**Visual Studio 2012** ，然後開啟**開始**解決方案位於**Source\Ex2 Validation\Begin**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="bec79-311">Open **Visual Studio 2012** and open the **Begin** solution located in the **Source\Ex2-Validation\Begin** folder of this lab.</span></span> <span data-ttu-id="bec79-312">或者，您可以繼續使用您現有的方案，從上一個練習。</span><span class="sxs-lookup"><span data-stu-id="bec79-312">Alternatively, you can continue working on your existing solution from the previous exercise.</span></span>

   1. <span data-ttu-id="bec79-313">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="bec79-313">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bec79-314">若要這樣做，請在 [方案總管] 中，按一下**WebFormsLab**專案**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="bec79-314">To do this, in the Solution Explorer, click the **WebFormsLab** project **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bec79-315">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="bec79-315">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="bec79-316">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="bec79-316">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bec79-317">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="bec79-317">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bec79-318">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="bec79-318">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bec79-319">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="bec79-319">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="bec79-320">按下**F5**啟動 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-320">Press **F5** to start the web application.</span></span> <span data-ttu-id="bec79-321">請移至客戶頁面，然後按一下**新增客戶**連結。</span><span class="sxs-lookup"><span data-stu-id="bec79-321">Go to the Customers page and click the **Add a New Customer** link.</span></span>
3. <span data-ttu-id="bec79-322">在瀏覽器 頁面上，以滑鼠右鍵按一下並選取**檢視原始檔**選項來開啟應用程式所產生的 HTML 程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-322">Right-click on the browser page, and select **View Source** option to open the HTML code generated by the application.</span></span>

    <span data-ttu-id="bec79-323">![顯示頁面的 HTML 程式碼](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "顯示網頁的 HTML 程式碼")</span><span class="sxs-lookup"><span data-stu-id="bec79-323">![Showing the page HTML code](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Showing the page HTML code")</span></span>

    <span data-ttu-id="bec79-324">*顯示頁面的 HTML 程式碼*</span><span class="sxs-lookup"><span data-stu-id="bec79-324">*Showing the page HTML code*</span></span>
4. <span data-ttu-id="bec79-325">捲動頁面原始碼，並注意 ASP.NET 已插入 JavaScript 程式碼和資料驗證頁面中執行的驗證，並顯示錯誤清單。</span><span class="sxs-lookup"><span data-stu-id="bec79-325">Scroll through the page source code and notice that ASP.NET has injected JavaScript code and data validators in the page to perform the validations and show the error list.</span></span>

    <span data-ttu-id="bec79-326">![驗證 CustomerDetails 頁面中的 JavaScript 程式碼](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails 頁面中的 JavaScript 驗證程式碼")</span><span class="sxs-lookup"><span data-stu-id="bec79-326">![Validation JavaScript code in CustomerDetails page](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Validation JavaScript code in CustomerDetails page")</span></span>

    <span data-ttu-id="bec79-327">*驗證 CustomerDetails 頁面中的 JavaScript 程式碼*</span><span class="sxs-lookup"><span data-stu-id="bec79-327">*Validation JavaScript code in CustomerDetails page*</span></span>
5. <span data-ttu-id="bec79-328">關閉瀏覽器並移回至 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bec79-328">Close the browser and go back to Visual Studio.</span></span>
6. <span data-ttu-id="bec79-329">現在您將啟用低調驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-329">Now you will enable unobtrusive validation.</span></span> <span data-ttu-id="bec79-330">開啟**Web.Config**並找出**ValidationSettings:UnobtrusiveValidationMode**中的索引鍵**AppSettings**一節 **。**</span><span class="sxs-lookup"><span data-stu-id="bec79-330">Open **Web.Config** and locate **ValidationSettings:UnobtrusiveValidationMode** key in the **AppSettings** section **.**</span></span> <span data-ttu-id="bec79-331">將索引鍵的值設定為**WebForms**。</span><span class="sxs-lookup"><span data-stu-id="bec79-331">Set the key value to **WebForms**.</span></span>

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > <span data-ttu-id="bec79-332">您也可以設定這個屬性&quot;**頁\_負載**&quot;萬一您想要啟用低調驗證只會針對某些頁面的事件。</span><span class="sxs-lookup"><span data-stu-id="bec79-332">You can also set this property in the &quot;**Page\_Load**&quot; event in case you want to enable Unobtrusive Validation only for some pages.</span></span>
7. <span data-ttu-id="bec79-333">開啟**CustomerDetails.aspx**然後按**F5**啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-333">Open **CustomerDetails.aspx** and press **F5** to start the Web application.</span></span>
8. <span data-ttu-id="bec79-334">按下 F12 鍵以開啟 IE 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="bec79-334">Press the F12 key to open the IE developer tools.</span></span> <span data-ttu-id="bec79-335">開發人員工具開啟後，選取 指令碼 索引標籤。選取  **CustomerDetails.aspx**功能表中進行指令碼才能執行頁面上的 jQuery 的附註已經載入瀏覽器的本機站台。</span><span class="sxs-lookup"><span data-stu-id="bec79-335">Once the developer tools is open, select the script tab. Select **CustomerDetails.aspx** from the menu and take note that the scripts required to run jQuery on the page have been loaded into the browser from the local site.</span></span>

    <span data-ttu-id="bec79-336">![直接從本機 IIS 伺服器載入 jQuery JavaScript 檔案](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "載入 jQuery JavaScript 檔案直接從本機 IIS 伺服器")</span><span class="sxs-lookup"><span data-stu-id="bec79-336">![Loading the jQuery JavaScript files directly from the local IIS server](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Loading the jQuery JavaScript files directly from the local IIS server")</span></span>

    <span data-ttu-id="bec79-337">*直接從本機 IIS 伺服器載入 jQuery JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="bec79-337">*Loading the jQuery JavaScript files directly from the local IIS server*</span></span>
9. <span data-ttu-id="bec79-338">關閉瀏覽器，若要返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bec79-338">Close the browser to return to Visual Studio.</span></span> <span data-ttu-id="bec79-339">開啟**Site.Master**檔案一次，並找出**ScriptManager**。</span><span class="sxs-lookup"><span data-stu-id="bec79-339">Open the **Site.Master** file again and locate the **ScriptManager**.</span></span> <span data-ttu-id="bec79-340">將屬性加入**加入 EnableCdn**屬性具有值 **，則為 True**。</span><span class="sxs-lookup"><span data-stu-id="bec79-340">Add the attribute **EnableCdn** property with the value **True**.</span></span> <span data-ttu-id="bec79-341">這會強制 jQuery 載入從線上的 URL，不是從本機站台的 URL。</span><span class="sxs-lookup"><span data-stu-id="bec79-341">This will force jQuery to be loaded from the online URL, not from the local site's URL.</span></span>
10. <span data-ttu-id="bec79-342">開啟**CustomerDetails.aspx** Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="bec79-342">Open **CustomerDetails.aspx** in Visual Studio.</span></span> <span data-ttu-id="bec79-343">按下 F5 鍵以執行網站。</span><span class="sxs-lookup"><span data-stu-id="bec79-343">Press the F5 key to run the site.</span></span> <span data-ttu-id="bec79-344">Internet Explorer 開啟後，請按 F12 鍵以開啟 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="bec79-344">Once Internet Explorer opens, press the F12 key to open the developer tools.</span></span> <span data-ttu-id="bec79-345">選取 **指令碼**索引標籤，然後再看看下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="bec79-345">Select the **Script** tab, and then take a look at the drop-down list.</span></span> <span data-ttu-id="bec79-346">請注意 jQuery JavaScript 檔案不會再載入本機的站台，但而不是從線上 jQuery CDN。</span><span class="sxs-lookup"><span data-stu-id="bec79-346">Note the jQuery JavaScript files are no longer being loaded from the local site, but rather from the online jQuery CDN.</span></span>

    <span data-ttu-id="bec79-347">![正在載入 jQuery JavaScript 檔案從 CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "載入 jQuery JavaScript 檔案從 CDN")</span><span class="sxs-lookup"><span data-stu-id="bec79-347">![Loading the jQuery JavaScript files from the CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Loading the jQuery JavaScript files from the CDN")</span></span>

    <span data-ttu-id="bec79-348">*載入來自 CDN 的 jQuery JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="bec79-348">*Loading the jQuery JavaScript files from the CDN*</span></span>
11. <span data-ttu-id="bec79-349">開啟一次使用瀏覽器中的 檢視來源選項的 HTML 頁面程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-349">Open the HTML page source code again using the View source option in the browser.</span></span> <span data-ttu-id="bec79-350">請注意，藉由啟用低調驗證 ASP.NET 已取代為插入的 JavaScript 程式碼的資料-\*屬性。</span><span class="sxs-lookup"><span data-stu-id="bec79-350">Notice that by enabling the unobtrusive validation ASP.NET has replaced the injected JavaScript code with data- \*attributes.</span></span>

    <span data-ttu-id="bec79-351">![低調驗證程式碼](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "低調驗證程式碼")</span><span class="sxs-lookup"><span data-stu-id="bec79-351">![Unobtrusive validation code](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Unobtrusive validation code")</span></span>

    <span data-ttu-id="bec79-352">*低調驗證程式碼*</span><span class="sxs-lookup"><span data-stu-id="bec79-352">*Unobtrusive validation code*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-353">在此範例中，您會看到如何驗證摘要使用資料註解已簡化，以少數的 HTML 和 JavaScript 行。</span><span class="sxs-lookup"><span data-stu-id="bec79-353">In this example, you saw how a validation summary with Data annotations was simplified to only a few HTML and JavaScript lines.</span></span> <span data-ttu-id="bec79-354">先前，而不需低調驗證，更多的驗證控制項新增，愈大 JavaScript 驗證程式碼會變大。</span><span class="sxs-lookup"><span data-stu-id="bec79-354">Previously, without unobtrusive validation, the more validation controls you add, the bigger your JavaScript validation code will grow.</span></span>

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a><span data-ttu-id="bec79-355">工作 2-驗證模型使用資料註解</span><span class="sxs-lookup"><span data-stu-id="bec79-355">Task 2 - Validating the Model with Data Annotations</span></span>

<span data-ttu-id="bec79-356">ASP.NET 4.5 介紹 Web Form 的資料註解驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-356">ASP.NET 4.5 introduces data annotations validation for Web Forms.</span></span> <span data-ttu-id="bec79-357">您不需要在每個輸入的驗證控制項，現在可以在您的模型類別中定義條件約束，並跨所有 web 應用程式中使用它們。</span><span class="sxs-lookup"><span data-stu-id="bec79-357">Instead of having a validation control on each input, you can now define constraints in your model classes and use them across all your web application.</span></span> <span data-ttu-id="bec79-358">在本節中，您將學習如何使用資料註解驗證新增/編輯客戶表單。</span><span class="sxs-lookup"><span data-stu-id="bec79-358">In this section, you will learn how to use data annotations for validating a new/edit customer form.</span></span>

1. <span data-ttu-id="bec79-359">開啟**CustomerDetail.aspx**頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-359">Open **CustomerDetail.aspx** page.</span></span> <span data-ttu-id="bec79-360">請注意，客戶名字和中的第二個名稱**EditItemTemplate**並**InsertItemTemplate**區段會驗證使用 RequiredFieldValidator 控制項。</span><span class="sxs-lookup"><span data-stu-id="bec79-360">Notice that the customer first name and second name in the **EditItemTemplate** and **InsertItemTemplate** sections are validated using a RequiredFieldValidator controls.</span></span> <span data-ttu-id="bec79-361">每一個驗證程式是關聯至特定的條件，因此您需要包含多個驗證做為要檢查的條件。</span><span class="sxs-lookup"><span data-stu-id="bec79-361">Each validator is associated to a particular condition, so you need to include as many validators as conditions to check.</span></span>
2. <span data-ttu-id="bec79-362">新增資料註解，以驗證客戶模型類別。</span><span class="sxs-lookup"><span data-stu-id="bec79-362">Add data annotations to validate the Customer model class.</span></span> <span data-ttu-id="bec79-363">開啟**Customer.cs**類別中**模型**資料夾以及*裝飾*每個使用資料註解屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="bec79-363">Open **Customer.cs** class in the **Model** folder and *decorate* each property using data annotation attributes.</span></span>

    <span data-ttu-id="bec79-364">(程式碼片段- *Web Form 實驗室-Ex02-資料註解*)</span><span class="sxs-lookup"><span data-stu-id="bec79-364">(Code Snippet - *Web Forms Lab - Ex02 - Data Annotations*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > <span data-ttu-id="bec79-365">.NET framework 4.5 已擴充現有的資料註釋集合。</span><span class="sxs-lookup"><span data-stu-id="bec79-365">.NET Framework 4.5 has extended the existing data annotation collection.</span></span> <span data-ttu-id="bec79-366">以下是一些您可以使用資料註解: [信用卡]，[Phone]，[EmailAddress]，[範圍]，[比較]，[Url]，[FileExtensions]、 [Required]、[Key]，[RegularExpression]。</span><span class="sxs-lookup"><span data-stu-id="bec79-366">These are some of the data annotations you can use: [CreditCard], [Phone], [EmailAddress], [Range], [Compare], [Url], [FileExtensions], [Required], [Key], [RegularExpression].</span></span>
    > 
    > <span data-ttu-id="bec79-367">某些使用方式範例：</span><span class="sxs-lookup"><span data-stu-id="bec79-367">Some usage examples:</span></span>
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > <span data-ttu-id="bec79-369">您也可以定義您自己的錯誤訊息中的每個屬性。</span><span class="sxs-lookup"><span data-stu-id="bec79-369">You can also define your own error messages within each attribute.</span></span>
3. <span data-ttu-id="bec79-370">開啟**CustomerDetails.aspx**和 FormView 控制項的 EditItemTemplate 和 InsertItemTemplate 各節中移除所有 RequiredFieldvalidators 名字和姓氏欄位。</span><span class="sxs-lookup"><span data-stu-id="bec79-370">Open **CustomerDetails.aspx** and remove all the RequiredFieldvalidators for the first and last name fields in the in EditItemTemplate and InsertItemTemplate sections of the FormView control.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > <span data-ttu-id="bec79-371">使用資料註解的優點之一是您的應用程式頁面中沒有重複，驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="bec79-371">One advantage of using data annotations is that validation logic is not duplicated in your application pages.</span></span> <span data-ttu-id="bec79-372">您可以定義一次在模型中，並使用操作資料的所有應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-372">You define it once in the model, and use it across all the application pages that manipulate data.</span></span>
4. <span data-ttu-id="bec79-373">開啟**CustomerDetails.aspx**程式碼後置，並找出 SaveCustomer 方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-373">Open **CustomerDetails.aspx** code-behind and locate the SaveCustomer method.</span></span> <span data-ttu-id="bec79-374">插入新客戶時，會呼叫這個方法，並接收客戶參數從 FormView 控制項的值。</span><span class="sxs-lookup"><span data-stu-id="bec79-374">This method is called when inserting a new customer and receives the Customer parameter from the FormView control values.</span></span> <span data-ttu-id="bec79-375">頁面控制項與參數物件就會發生之間的對應，ASP.NET 會在執行時對資料註釋的模型驗證屬性，並填入此 ModelState 字典錯誤發生，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="bec79-375">When the mapping between the page controls and the parameter object occurrs, ASP.NET will execute the model validation against all the data annotation attributes and fill the ModelState dictionary with the errors encountered, if any.</span></span>

    <span data-ttu-id="bec79-376">ModelState.IsValid 只會傳回 true，如果執行驗證之後，您的模型上的所有欄位都都有效。</span><span class="sxs-lookup"><span data-stu-id="bec79-376">The ModelState.IsValid will only return true if all the fields on your model are valid after performing the validation.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. <span data-ttu-id="bec79-377">新增**ValidationSummary** CustomerDetails 頁面以顯示模型錯誤的清單結尾處的控制項。</span><span class="sxs-lookup"><span data-stu-id="bec79-377">Add a **ValidationSummary** control at the end of the CustomerDetails page to show the list of model errors.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    <span data-ttu-id="bec79-378">**ShowModelStateErrors** ValidationSummary 的新屬性控制，設定為 **，則為 true**，控制項才會顯示從 ModelState 字典的錯誤。</span><span class="sxs-lookup"><span data-stu-id="bec79-378">The **ShowModelStateErrors** is a new property on the ValidationSummary control that when set to **true**, the control will show the errors from the ModelState dictionary.</span></span> <span data-ttu-id="bec79-379">這些錯誤是來自於資料註解驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-379">These errors come from the data annotations validation.</span></span>
6. <span data-ttu-id="bec79-380">按下**F5**執行 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-380">Press **F5** to run the Web application.</span></span> <span data-ttu-id="bec79-381">完成表單與某些錯誤的值，然後按一下**儲存**執行驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-381">Complete the form with some erroneous values and click **Save** to execute validation.</span></span> <span data-ttu-id="bec79-382">請注意底部的摘要時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="bec79-382">Notice the error summary at the bottom.</span></span>

    <span data-ttu-id="bec79-383">![使用資料註解驗證](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "使用資料註解驗證")</span><span class="sxs-lookup"><span data-stu-id="bec79-383">![Validation with Data Annotations](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validation with Data Annotations")</span></span>

    <span data-ttu-id="bec79-384">*使用資料註解驗證*</span><span class="sxs-lookup"><span data-stu-id="bec79-384">*Validation with Data Annotations*</span></span>

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a><span data-ttu-id="bec79-385">工作 3-ModelState 處理自訂的資料庫錯誤</span><span class="sxs-lookup"><span data-stu-id="bec79-385">Task 3 - Handling Custom Database Errors with ModelState</span></span>

<span data-ttu-id="bec79-386">在舊版的 Web Form 中，處理資料庫錯誤，例如太長的字串或唯一索引鍵違規可能牽涉到在您的存放庫程式碼中擲回例外狀況，並在您的程式碼後置來顯示錯誤的例外狀況，接著處理。</span><span class="sxs-lookup"><span data-stu-id="bec79-386">In previous version of Web Forms, handling database errors such as a too long string or a unique key violation could involve throwing exceptions in your repository code and then handling the exceptions on your code-behind to display an error.</span></span> <span data-ttu-id="bec79-387">大量的程式碼，才能執行一些相當簡單。</span><span class="sxs-lookup"><span data-stu-id="bec79-387">A great amount of code is required to do something relatively simple.</span></span>

<span data-ttu-id="bec79-388">在 Web Form 4.5 中，ModelState 物件可用來以一致的方式顯示在頁面上，從您的模型，或是從資料庫中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="bec79-388">In Web Forms 4.5, the ModelState object can be used to display the errors on the page, either from your model or from the database, in a consistent manner.</span></span>

<span data-ttu-id="bec79-389">在這個工作中，您將加入程式碼，才能正確處理資料庫例外狀況，並使用 ModelState 物件對使用者顯示適當的訊息。</span><span class="sxs-lookup"><span data-stu-id="bec79-389">In this task, you will add code to properly handle database exceptions and show the appropriate message to the user using the ModelState object.</span></span>

1. <span data-ttu-id="bec79-390">雖然應用程式仍在執行中，嘗試更新使用重複的值分類的名稱。</span><span class="sxs-lookup"><span data-stu-id="bec79-390">While the application is still running, try to update the name of a category using a duplicated value.</span></span>

    <span data-ttu-id="bec79-391">![使用重複的名稱更新分類](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "更新具有重複名稱的類別")</span><span class="sxs-lookup"><span data-stu-id="bec79-391">![Updating a category with a duplicated name](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Updating a category with a duplicated name")</span></span>

    <span data-ttu-id="bec79-392">*更新具有重複名稱的類別*</span><span class="sxs-lookup"><span data-stu-id="bec79-392">*Updating a category with a duplicated name*</span></span>

    <span data-ttu-id="bec79-393">請注意，因為擲回例外狀況&quot;唯一&quot;條件約束**CategoryName**資料行。</span><span class="sxs-lookup"><span data-stu-id="bec79-393">Notice that an exception is thrown due to the &quot;unique&quot; constraint of the **CategoryName** column.</span></span>

    <span data-ttu-id="bec79-394">![重複的類別目錄名稱的例外狀況](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "為重複的類別名稱的例外狀況")</span><span class="sxs-lookup"><span data-stu-id="bec79-394">![Exception for duplicated category names](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Exception for duplicated category names")</span></span>

    <span data-ttu-id="bec79-395">*重複的類別目錄名稱的例外狀況*</span><span class="sxs-lookup"><span data-stu-id="bec79-395">*Exception for duplicated category names*</span></span>
2. <span data-ttu-id="bec79-396">停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="bec79-396">Stop debugging.</span></span> <span data-ttu-id="bec79-397">在  **Products.aspx.cs**程式碼後置檔案、 update **UpdateCategory**方法來處理資料庫擲回例外狀況。Savechanges （） 方法呼叫，並加入錯誤**ModelState**物件。</span><span class="sxs-lookup"><span data-stu-id="bec79-397">In the **Products.aspx.cs** code-behind file, update the **UpdateCategory** method to handle the exceptions thrown by the db.SaveChanges() method call and add an error to the **ModelState** object.</span></span>

    <span data-ttu-id="bec79-398">新**TryUpdateModel**方法會更新使用使用者所提供的表單資料，從資料庫擷取的類別目錄物件。</span><span class="sxs-lookup"><span data-stu-id="bec79-398">The new **TryUpdateModel** method updates the category object retrieved from the database using the form data provided by the user.</span></span>

    <span data-ttu-id="bec79-399">(程式碼片段- *Web Form 實驗室-Ex02-UpdateCategory 處理錯誤*)</span><span class="sxs-lookup"><span data-stu-id="bec79-399">(Code Snippet - *Web Forms Lab - Ex02 - UpdateCategory Handle Errors*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > <span data-ttu-id="bec79-400">在理想情況下，您必須找出 DbUpdateException 的原因，並檢查根本原因是否是唯一的索引鍵條件約束的違規。</span><span class="sxs-lookup"><span data-stu-id="bec79-400">Ideally, you would have to identify the cause of the DbUpdateException and check if the root cause is the violation of a unique key constraint.</span></span>
3. <span data-ttu-id="bec79-401">開啟**Products.aspx**並加入**Aggregate**類別 GridView，以顯示的模型錯誤清單 下的控制項。</span><span class="sxs-lookup"><span data-stu-id="bec79-401">Open **Products.aspx** and add a **ValidationSummary** control below the categories GridView to show the list of model errors.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. <span data-ttu-id="bec79-402">執行網站並移至 [產品] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-402">Run the site and go to the Products page.</span></span> <span data-ttu-id="bec79-403">嘗試更新使用重複的值分類的名稱。</span><span class="sxs-lookup"><span data-stu-id="bec79-403">Try to update the name of a category using an duplicated value.</span></span>

    <span data-ttu-id="bec79-404">請注意，已處理的例外狀況，並會出現錯誤訊息**ValidationSummary**控制項。</span><span class="sxs-lookup"><span data-stu-id="bec79-404">Notice that the exception was handled and the error message appears in the **ValidationSummary** control.</span></span>

    <span data-ttu-id="bec79-405">![複製類別目錄錯誤](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "複製類別目錄錯誤")</span><span class="sxs-lookup"><span data-stu-id="bec79-405">![Duplicated category error](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Duplicated category error")</span></span>

    <span data-ttu-id="bec79-406">*重複的類別目錄錯誤*</span><span class="sxs-lookup"><span data-stu-id="bec79-406">*Duplicated category error*</span></span>

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a><span data-ttu-id="bec79-407">工作 4-要求在 ASP.NET Web Form 4.5 中的驗證</span><span class="sxs-lookup"><span data-stu-id="bec79-407">Task 4 - Request Validation in ASP.NET Web Forms 4.5</span></span>

<span data-ttu-id="bec79-408">在 ASP.NET 要求驗證功能提供特定層級的預設保護來抵禦跨網站指令碼 (XSS) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="bec79-408">The request validation feature in ASP.NET provides a certain level of default protection against cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="bec79-409">在舊版 ASP.NET 中，依預設會啟用要求驗證，並只會停用整個頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-409">In previous versions of ASP.NET, request validation was enabled by default and could only be disabled for an entire page.</span></span> <span data-ttu-id="bec79-410">使用新版本的 ASP.NET Web Form 現在可以停用要求驗證的單一控制項、 執行延遲的要求驗證或存取 （務必小心，如果您這麼做 ！） 的未驗證的要求資料。</span><span class="sxs-lookup"><span data-stu-id="bec79-410">With the new version of ASP.NET Web Forms you can now disable the request validation for a single control, perform lazy request validation or access un-validated request data (be careful if you do so!).</span></span>

1. <span data-ttu-id="bec79-411">按下**Ctrl + F5**啟動網站，但不偵錯，並移至 [產品] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-411">Press **Ctrl+F5** to start the site without debugging and go to the Products page.</span></span> <span data-ttu-id="bec79-412">選取類別目錄，然後按一下**編輯**連結上的任何產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-412">Select a category and then click the **Edit** link on any of the products.</span></span>
2. <span data-ttu-id="bec79-413">輸入包含有潛在危險的內容，例如包含 HTML 標記的描述。</span><span class="sxs-lookup"><span data-stu-id="bec79-413">Type a description containing potentially dangerous content, for instance including HTML tags.</span></span> <span data-ttu-id="bec79-414">需要請注意，因為要求驗證擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="bec79-414">Take notice of the exception thrown due to the request validation.</span></span>

    <span data-ttu-id="bec79-415">![編輯具有潛在危險的內容之產品](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "編輯產品有潛在危險的內容")</span><span class="sxs-lookup"><span data-stu-id="bec79-415">![Editing a product with potentially dangerous content](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Editing a product with potentially dangerous content")</span></span>

    <span data-ttu-id="bec79-416">*編輯的產品有潛在危險的內容*</span><span class="sxs-lookup"><span data-stu-id="bec79-416">*Editing a product with potentially dangerous content*</span></span>

    <span data-ttu-id="bec79-417">![因為要求的驗證所以擲回的例外狀況](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "因為要求的驗證所以擲回的例外狀況")</span><span class="sxs-lookup"><span data-stu-id="bec79-417">![Exception thrown due to request validation](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Exception thrown due to request validation")</span></span>

    <span data-ttu-id="bec79-418">*因為要求的驗證所以擲回的例外狀況*</span><span class="sxs-lookup"><span data-stu-id="bec79-418">*Exception thrown due to request validation*</span></span>
3. <span data-ttu-id="bec79-419">關閉網頁，並在 Visual Studio 中按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="bec79-419">Close the page and, in Visual Studio, press **SHIFT+F5** to stop debugging.</span></span>
4. <span data-ttu-id="bec79-420">開啟**ProductDetails.aspx**頁面上，並找出**描述**文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="bec79-420">Open the **ProductDetails.aspx** page and locate the **Description** TextBox.</span></span>
5. <span data-ttu-id="bec79-421">加入新**ValidateRequestMode**文字方塊中的屬性並將其值設定為**停用**。</span><span class="sxs-lookup"><span data-stu-id="bec79-421">Add the new **ValidateRequestMode** property to the TextBox and set its value to **Disabled**.</span></span>

    <span data-ttu-id="bec79-422">新**ValidateRequestMode**屬性可讓您停用要求驗證，細微的方式在每個控制項上。</span><span class="sxs-lookup"><span data-stu-id="bec79-422">The new **ValidateRequestMode** attribute allows you to disable the request validation granularly on each control.</span></span> <span data-ttu-id="bec79-423">當您想要使用的輸入，可能會收到 HTML 程式碼，但想要保留的頁面的其餘部分使用的驗證，這非常有用。</span><span class="sxs-lookup"><span data-stu-id="bec79-423">This is useful when you want to use an input that may receive HTML code, but want to keep the validation working for the rest of the page.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. <span data-ttu-id="bec79-424">按下**F5**執行 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-424">Press **F5** to run the web application.</span></span> <span data-ttu-id="bec79-425">再次開啟編輯產品頁面上，並完成產品描述，包括 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="bec79-425">Open the edit product page again and complete a product description including HTML tags.</span></span> <span data-ttu-id="bec79-426">請注意，您現在可以將 HTML 內容加入描述。</span><span class="sxs-lookup"><span data-stu-id="bec79-426">Notice that you can now add HTML content to the description.</span></span>

    <span data-ttu-id="bec79-427">![要求的產品描述停用的驗證](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "要求驗證停用的產品描述")</span><span class="sxs-lookup"><span data-stu-id="bec79-427">![Request validation disabled for the product description](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Request validation disabled for the product description")</span></span>

    <span data-ttu-id="bec79-428">*產品說明停用要求驗證*</span><span class="sxs-lookup"><span data-stu-id="bec79-428">*Request validation disabled for the product description*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-429">在生產環境應用程式中，您應該處理，請確定輸入唯一安全的 HTML 標記的使用者輸入的 HTML 程式碼 (例如，有沒有&lt;指令碼&gt;標記)。</span><span class="sxs-lookup"><span data-stu-id="bec79-429">In a production application, you should sanitize the HTML code entered by the user to make sure only safe HTML tags are entered (for example, there are no &lt;script&gt; tags).</span></span> <span data-ttu-id="bec79-430">若要這樣做，您可以使用[Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS)。</span><span class="sxs-lookup"><span data-stu-id="bec79-430">To do this, you can use [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).</span></span>
7. <span data-ttu-id="bec79-431">再次編輯的產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-431">Edit the product again.</span></span> <span data-ttu-id="bec79-432">在 [名稱] 欄位中輸入 HTML 程式碼，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="bec79-432">Type HTML code in the Name field and click **Save**.</span></span> <span data-ttu-id="bec79-433">請注意，[描述] 欄位只停用要求驗證和重新欄位的其餘部分仍會驗證有潛在危險的內容。</span><span class="sxs-lookup"><span data-stu-id="bec79-433">Notice that Request Validation is only disabled for the Description field and the rest of the fields re still validated against the potentially dangerous content.</span></span>

    <span data-ttu-id="bec79-434">![要求在其餘的欄位中啟用的驗證](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "要求在其餘的欄位中啟用驗證")</span><span class="sxs-lookup"><span data-stu-id="bec79-434">![Request validation enabled in the rest of the fields](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Request validation enabled in the rest of the fields")</span></span>

    <span data-ttu-id="bec79-435">*在其餘的欄位中啟用要求驗證*</span><span class="sxs-lookup"><span data-stu-id="bec79-435">*Request validation enabled in the rest of the fields*</span></span>

    <span data-ttu-id="bec79-436">ASP.NET Web Forms 4.5 包含新的要求驗證模式來延遲執行要求驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-436">ASP.NET Web Forms 4.5 includes a new request validation mode to perform request validation lazily.</span></span> <span data-ttu-id="bec79-437">要求的驗證模式設定為**4.5**，如果一段程式碼存取*Request.Form [&quot;金鑰&quot;]*，ASP.NET 4.5 要求驗證會只有觸發要求驗證該表單集合中的特定元素。</span><span class="sxs-lookup"><span data-stu-id="bec79-437">With the request validation mode set to **4.5**, if a piece of code accesses *Request.Form[&quot;key&quot;]*, ASP.NET 4.5's request validation will only trigger request validation for that specific element in the form collection.</span></span>

    <span data-ttu-id="bec79-438">此外，ASP.NET 4.5 現在會包含從 Microsoft ANTI-XSS 程式庫 v4.0 的核心編碼常式。</span><span class="sxs-lookup"><span data-stu-id="bec79-438">Additionally, ASP.NET 4.5 now includes core encoding routines from the Microsoft Anti-XSS Library v4.0.</span></span> <span data-ttu-id="bec79-439">ANTI-XSS 編碼常式的實作方式的新*AntiXssEncoder*找到在新的型別**System.Web.Security.AntiXss**命名空間。</span><span class="sxs-lookup"><span data-stu-id="bec79-439">The Anti-XSS encoding routines are implemented by the new *AntiXssEncoder* type found in the new **System.Web.Security.AntiXss** namespace.</span></span> <span data-ttu-id="bec79-440">具有**encoderType**參數設定為使用*AntiXssEncoder*，所有輸出的編碼方式在 ASP.NET 中自動使用新的編碼常式。</span><span class="sxs-lookup"><span data-stu-id="bec79-440">With the **encoderType** parameter configured to use *AntiXssEncoder*, all output encoding within ASP.NET automatically uses the new encoding routines.</span></span>
8. <span data-ttu-id="bec79-441">ASP.NET 4.5 的要求驗證也支援未驗證的要求資料的存取。</span><span class="sxs-lookup"><span data-stu-id="bec79-441">ASP.NET 4.5 request validation also supports un-validated access to request data.</span></span> <span data-ttu-id="bec79-442">ASP.NET 4.5 新集合將屬性加入至**HttpRequest**呼叫的物件**Unvalidated**。</span><span class="sxs-lookup"><span data-stu-id="bec79-442">ASP.NET 4.5 adds a new collection property to the **HttpRequest** object called **Unvalidated**.</span></span> <span data-ttu-id="bec79-443">當您瀏覽到**HttpRequest.Unvalidated**您可以存取所有的要求資料，包括表單、 QueryStrings、 Cookie、 Url 和等等的常見項目。</span><span class="sxs-lookup"><span data-stu-id="bec79-443">When you navigate into **HttpRequest.Unvalidated** you have access to all of the common pieces of request data, including Forms, QueryStrings, Cookies, URLs, and so on.</span></span>

    <span data-ttu-id="bec79-444">![Request.Unvalidated 物件](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated 物件")</span><span class="sxs-lookup"><span data-stu-id="bec79-444">![Request.Unvalidated object](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated object")</span></span>

    <span data-ttu-id="bec79-445">*Request.Unvalidated 物件*</span><span class="sxs-lookup"><span data-stu-id="bec79-445">*Request.Unvalidated object*</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-446">**請謹慎使用 HttpRequest.Unvalidated 屬性 ！**</span><span class="sxs-lookup"><span data-stu-id="bec79-446">**Please use the HttpRequest.Unvalidated property with caution!**</span></span> <span data-ttu-id="bec79-447">請確定您仔細對自訂驗證原始要求資料，以確保危險的文字不往返並呈現遲疑不前的客戶 ！</span><span class="sxs-lookup"><span data-stu-id="bec79-447">Make sure you carefully perform custom validation on the raw request data to ensure that dangerous text is not round-tripped and rendered back to unsuspecting customers!</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a><span data-ttu-id="bec79-448">練習 3： 非同步頁面處理在 ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="bec79-448">Exercise 3: Asynchronous Page Processing in ASP.NET Web Forms</span></span>

<span data-ttu-id="bec79-449">在此練習中，您將介紹新的非同步頁面處理 ASP.NET Web Form 中的功能。</span><span class="sxs-lookup"><span data-stu-id="bec79-449">In this exercise, you will be introduced to the new asynchronous page processing features in ASP.NET Web Forms.</span></span>

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a><span data-ttu-id="bec79-450">工作 1-更新產品詳細資料頁面，即可上傳和顯示的映像</span><span class="sxs-lookup"><span data-stu-id="bec79-450">Task 1 - Updating the Product Details Page to Upload and Show Images</span></span>

<span data-ttu-id="bec79-451">在這個工作中，您將會更新以允許使用者指定產品的影像 URL，並顯示在 [唯讀] 檢視中的產品詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-451">In this task, you will update the product details page to allow the user to specify an image URL for the product and display it in the read-only view.</span></span> <span data-ttu-id="bec79-452">您將建立指定的映像的本機複本，下載並以同步方式。</span><span class="sxs-lookup"><span data-stu-id="bec79-452">You will create a local copy of the specified image by downloading it synchronously.</span></span> <span data-ttu-id="bec79-453">在下一個工作中，您將會更新這項實作，讓它以非同步方式運作。</span><span class="sxs-lookup"><span data-stu-id="bec79-453">In the next task, you will update this implementation to make it work asynchronously.</span></span>

1. <span data-ttu-id="bec79-454">開啟**Visual Studio 2012**並載入**開始**解決方案位於**Source\Ex3 Async\Begin**從這個實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="bec79-454">Open **Visual Studio 2012** and load the **Begin** solution located in **Source\Ex3-Async\Begin** from this lab's folder.</span></span> <span data-ttu-id="bec79-455">或者，您可以繼續使用您現有的方案，從之前的練習。</span><span class="sxs-lookup"><span data-stu-id="bec79-455">Alternatively, you can continue working on your existing solution from the previous exercises.</span></span>

   1. <span data-ttu-id="bec79-456">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="bec79-456">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="bec79-457">若要這樣做，請在 [方案總管] 中，按一下**WebFormsLab**專案，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="bec79-457">To do this, in the Solution Explorer, click the **WebFormsLab** project and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="bec79-458">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="bec79-458">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="bec79-459">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="bec79-459">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bec79-460">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="bec79-460">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="bec79-461">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="bec79-461">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="bec79-462">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="bec79-462">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="bec79-463">開啟**ProductDetails.aspx**頁面來源，然後將欄位加入 FormView 的 itemtemplate，以顯示產品影像。</span><span class="sxs-lookup"><span data-stu-id="bec79-463">Open the **ProductDetails.aspx** page source and add a field in the FormView's ItemTemplate to show the product image.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. <span data-ttu-id="bec79-464">加入欄位，以在 FormView 的 EditTemplate 指定影像 URL。</span><span class="sxs-lookup"><span data-stu-id="bec79-464">Add a field to specify the image URL in the FormView's EditTemplate.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. <span data-ttu-id="bec79-465">開啟**ProductDetails.aspx.cs**程式碼後置檔案，並新增下列命名空間指示詞。</span><span class="sxs-lookup"><span data-stu-id="bec79-465">Open the **ProductDetails.aspx.cs** code-behind file and add the following namespace directives.</span></span>

    <span data-ttu-id="bec79-466">(程式碼片段- *Web Form 實驗室-Ex03-命名空間*)</span><span class="sxs-lookup"><span data-stu-id="bec79-466">(Code Snippet - *Web Forms Lab - Ex03 - Namespaces*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. <span data-ttu-id="bec79-467">建立**UpdateProductImage**方法，以將遠端的映像儲存在本機**映像**資料夾，然後更新為新的映像的位置值的 [產品] 實體。</span><span class="sxs-lookup"><span data-stu-id="bec79-467">Create an **UpdateProductImage** method to store remote images in the local **Images** folder and update the product entity with the new image location value.</span></span>

    <span data-ttu-id="bec79-468">(程式碼片段- *Web Form 實驗室-Ex03-UpdateProductImage*)</span><span class="sxs-lookup"><span data-stu-id="bec79-468">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. <span data-ttu-id="bec79-469">更新**UpdateProduct**方法來呼叫**UpdateProductImage**方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-469">Update the **UpdateProduct** method to call the **UpdateProductImage** method.</span></span>

    <span data-ttu-id="bec79-470">(程式碼片段- *Web Form 實驗室-Ex03-UpdateProductImage 呼叫*)</span><span class="sxs-lookup"><span data-stu-id="bec79-470">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Call*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. <span data-ttu-id="bec79-471">執行應用程式，然後再次嘗試上傳一項產品的影像。</span><span class="sxs-lookup"><span data-stu-id="bec79-471">Run the application and try to upload an image for a product.</span></span> <span data-ttu-id="bec79-472">例如，您可以使用下列映像 URL 從 Office 剪輯藝術： [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)</span><span class="sxs-lookup"><span data-stu-id="bec79-472">For example, you can use the following image URL from Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)</span></span>

    <span data-ttu-id="bec79-473">![設定產品的映像](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "設定產品的映像")</span><span class="sxs-lookup"><span data-stu-id="bec79-473">![Setting an image for a product](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Setting an image for a product")</span></span>

    <span data-ttu-id="bec79-474">*設定產品的映像*</span><span class="sxs-lookup"><span data-stu-id="bec79-474">*Setting an image for a product*</span></span>

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a><span data-ttu-id="bec79-475">工作 2-新增非同步處理產品詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="bec79-475">Task 2 - Adding Asynchronous Processing to the Product Details Page</span></span>

<span data-ttu-id="bec79-476">在這個工作中，您將會更新，讓它以非同步方式運作的產品詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-476">In this task, you will update the product details page to make it work asynchronously.</span></span> <span data-ttu-id="bec79-477">您將會增強長時間執行的工作-映像下載程序-使用 ASP.NET 4.5 的非同步頁面處理。</span><span class="sxs-lookup"><span data-stu-id="bec79-477">You will enhance a long running task - the image download process - by using ASP.NET 4.5 asynchronous page processing.</span></span>

<span data-ttu-id="bec79-478">Web 應用程式中的非同步方法可用來最佳化 ASP.NET 執行緒集區使用的方式。</span><span class="sxs-lookup"><span data-stu-id="bec79-478">Asynchronous methods in web applications can be used to optimize the way ASP.NET thread pools are used.</span></span> <span data-ttu-id="bec79-479">在那里的 ASP.NET 中會有限的數目的執行緒上的執行緒集區中要求，因此時所有執行緒都都忙碌中，, ASP.NET，開始拒絕新的要求、 傳送應用程式錯誤訊息並將您的網站無法使用。</span><span class="sxs-lookup"><span data-stu-id="bec79-479">In ASP.NET there are a limited number of threads in the thread pool for attending requests, thus, when all the threads are busy, ASP.NET starts to reject new requests, sends application error messages and makes your site unavailable.</span></span>

<span data-ttu-id="bec79-480">耗時的作業，在您的網站上的非同步程式設計，因為它們在長時間佔用指派的執行緒是絕佳候選項目。</span><span class="sxs-lookup"><span data-stu-id="bec79-480">Time-consuming operations on your web site are great candidates for asynchronous programming because they occupy the assigned thread for a long time.</span></span> <span data-ttu-id="bec79-481">這包括長時間執行的要求，有許多不同的項目頁面，以及需要離線作業，這類查詢資料庫或存取外部 web 伺服器的頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-481">This includes long running requests, pages with lots of different elements and pages that require offline operations, such querying a database or accessing an external web server.</span></span> <span data-ttu-id="bec79-482">優點是，如果頁面處理時，您可以使用這些作業的非同步方法，執行緒會釋出，並回到執行緒集區，而且可用來處理新的頁面要求。</span><span class="sxs-lookup"><span data-stu-id="bec79-482">The advantage is that if you use asynchronous methods for these operations, while the page is processing, the thread is freed and returned to the thread pool and can be used to attend to a new page request.</span></span> <span data-ttu-id="bec79-483">這表示，網頁會開始處理執行緒集區的一個執行緒中，並可能在非同步處理完成之後完成中的另一處理。</span><span class="sxs-lookup"><span data-stu-id="bec79-483">This means, the page will start processing in one thread from the thread pool and might complete processing in a different one, after the async processing completes.</span></span>

1. <span data-ttu-id="bec79-484">開啟**ProductDetails.aspx**頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-484">Open the **ProductDetails.aspx** page.</span></span> <span data-ttu-id="bec79-485">新增**非同步**屬性中**頁**項目並將它設定為**true**。</span><span class="sxs-lookup"><span data-stu-id="bec79-485">Add the **Async** attribute in the **Page** element and set it to **true**.</span></span> <span data-ttu-id="bec79-486">此屬性會告知 ASP.NET 實作 IHttpAsyncHandler 介面。</span><span class="sxs-lookup"><span data-stu-id="bec79-486">This attribute tells ASP.NET to implement the IHttpAsyncHandler interface.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. <span data-ttu-id="bec79-487">新增標籤以顯示頁面上執行的執行緒的詳細資料頁面的底部。</span><span class="sxs-lookup"><span data-stu-id="bec79-487">Add a Label at the bottom of the page to show the details of the threads running the page.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. <span data-ttu-id="bec79-488">開啟**ProductDetails.aspx.cs**並新增下列命名空間指示詞。</span><span class="sxs-lookup"><span data-stu-id="bec79-488">Open up **ProductDetails.aspx.cs** and add the following namespace directives.</span></span>

    <span data-ttu-id="bec79-489">(程式碼片段- *Web Form 實驗室-Ex03-命名空間 2*)</span><span class="sxs-lookup"><span data-stu-id="bec79-489">(Code Snippet - *Web Forms Lab - Ex03 - Namespaces 2*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. <span data-ttu-id="bec79-490">修改**UpdateProductImage**下載映像與非同步工作的方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-490">Modify the **UpdateProductImage** method to download the image with an asynchronous task.</span></span> <span data-ttu-id="bec79-491">您將會取代**WebClient** **DownloadFile**方法**DownloadFileTaskAsync**方法，並包含**await**關鍵字。</span><span class="sxs-lookup"><span data-stu-id="bec79-491">You will replace the **WebClient** **DownloadFile** method with the **DownloadFileTaskAsync** method and include the **await** keyword.</span></span>

    <span data-ttu-id="bec79-492">(程式碼片段- *Web Form 實驗室-Ex03-UpdateProductImage 非同步*)</span><span class="sxs-lookup"><span data-stu-id="bec79-492">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Async*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    <span data-ttu-id="bec79-493">RegisterAsyncTask 註冊要在不同的執行緒中執行的新頁面非同步工作。</span><span class="sxs-lookup"><span data-stu-id="bec79-493">The RegisterAsyncTask registers a new page asynchronous task to be executed in a different thread.</span></span> <span data-ttu-id="bec79-494">它會接收要執行的工作 (t) 的 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="bec79-494">It receives a lambda expression with the Task (t) to be executed.</span></span> <span data-ttu-id="bec79-495">**Await**中的關鍵字**DownloadFileTaskAsync**方法會將方法的其餘部分轉換後以非同步方式叫用回呼**DownloadFileTaskAsync**方法完成。</span><span class="sxs-lookup"><span data-stu-id="bec79-495">The **await** keyword in the **DownloadFileTaskAsync** method converts the remainder of the method into a callback that is invoked asynchronously after the **DownloadFileTaskAsync** method has completed.</span></span> <span data-ttu-id="bec79-496">ASP.NET 會繼續執行方法，藉由自動維護所有 HTTP 要求原始值。</span><span class="sxs-lookup"><span data-stu-id="bec79-496">ASP.NET will resume the execution of the method by automatically maintaining all the HTTP request original values.</span></span> <span data-ttu-id="bec79-497">在.NET 4.5 中新的非同步程式設計模型可讓您撰寫非同步程式碼看起來非常類似同步程式碼，並讓編譯器處理複雜的回呼函式或接續程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-497">The new asynchronous programming model in .NET 4.5 enables you to write asynchronous code that looks very much like synchronous code, and let the compiler handle the complications of callback functions or continuation code.</span></span>
    > [!NOTE]
    > <span data-ttu-id="bec79-498">RegisterAsyncTask PageAsyncTask 都已可使用和自.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="bec79-498">RegisterAsyncTask and PageAsyncTask were already available since .NET 2.0.</span></span> <span data-ttu-id="bec79-499">從.NET 4.5 的非同步程式設計模型的新 await 關鍵字，並可搭配新 TaskAsync 方法從.NET WebClient 物件。</span><span class="sxs-lookup"><span data-stu-id="bec79-499">The await keyword is new from the .NET 4.5 asynchronous programming model and can be used together with the new TaskAsync methods from the .NET WebClient object.</span></span>
5. <span data-ttu-id="bec79-500">加入程式碼顯示程式碼開始和完成執行所在的執行緒。</span><span class="sxs-lookup"><span data-stu-id="bec79-500">Add code to display the threads on which the code started and finished executing.</span></span> <span data-ttu-id="bec79-501">若要這樣做，更新**UpdateProductImage**為下列程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="bec79-501">To do this, update the **UpdateProductImage** method with the following code.</span></span>

    <span data-ttu-id="bec79-502">(程式碼片段- *Web Form 實驗室-Ex03-Show 執行緒*)</span><span class="sxs-lookup"><span data-stu-id="bec79-502">(Code Snippet - *Web Forms Lab - Ex03 - Show threads*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. <span data-ttu-id="bec79-503">開啟網站的**Web.config**檔案。</span><span class="sxs-lookup"><span data-stu-id="bec79-503">Open the web site's **Web.config** file.</span></span> <span data-ttu-id="bec79-504">加入下列的 appSetting 變數。</span><span class="sxs-lookup"><span data-stu-id="bec79-504">Add the following appSetting variable.</span></span>

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. <span data-ttu-id="bec79-505">按下**F5**來執行應用程式，並上傳映像的產品。</span><span class="sxs-lookup"><span data-stu-id="bec79-505">Press **F5** to run the application and upload an image for the product.</span></span> <span data-ttu-id="bec79-506">請注意，開始和完成的程式碼可能不同的執行緒識別碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-506">Notice the threads ID where the code started and finished may be different.</span></span> <span data-ttu-id="bec79-507">這是因為個別的執行緒上執行 ASP.NET 執行緒集區中的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="bec79-507">This is because asynchronous tasks run on a separate thread from ASP.NET thread pool.</span></span> <span data-ttu-id="bec79-508">工作完成時，ASP.NET 會將工作放回佇列，並指派任何可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="bec79-508">When the task completes, ASP.NET puts the task back in the queue and assigns any of the available threads.</span></span>

    <span data-ttu-id="bec79-509">![以非同步方式下載映像](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "非同步下載映像")</span><span class="sxs-lookup"><span data-stu-id="bec79-509">![Downloading an image asynchronously](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Downloading an image asynchronously")</span></span>

    <span data-ttu-id="bec79-510">*以非同步方式下載映像*</span><span class="sxs-lookup"><span data-stu-id="bec79-510">*Downloading an image asynchronously*</span></span>

> [!NOTE]
> <span data-ttu-id="bec79-511">此外，您可以在其中部署此應用程式至 Azure 的下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。</span><span class="sxs-lookup"><span data-stu-id="bec79-511">Additionally, you can deploy this application to Azure following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="bec79-512">總結</span><span class="sxs-lookup"><span data-stu-id="bec79-512">Summary</span></span>

<span data-ttu-id="bec79-513">在這個實際操作實驗室中，有已解決和示範下列概念：</span><span class="sxs-lookup"><span data-stu-id="bec79-513">In this hands-on lab, the following concepts have been addressed and demonstrated:</span></span>

- <span data-ttu-id="bec79-514">使用強型別資料繫結運算式</span><span class="sxs-lookup"><span data-stu-id="bec79-514">Use strongly-typed data-binding expressions</span></span>
- <span data-ttu-id="bec79-515">在 Web Form 中使用模型繫結的新功能</span><span class="sxs-lookup"><span data-stu-id="bec79-515">Use new model binding features in Web Forms</span></span>
- <span data-ttu-id="bec79-516">將頁面資料對應到程式碼後置方法時，用於值提供者</span><span class="sxs-lookup"><span data-stu-id="bec79-516">Use value providers for mapping page data to code-behind methods</span></span>
- <span data-ttu-id="bec79-517">使用資料註解來驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="bec79-517">Use Data Annotations for user input validation</span></span>
- <span data-ttu-id="bec79-518">在 Web Form 中採取 unobstrusive 用戶端驗證，與 jQuery 的 advange</span><span class="sxs-lookup"><span data-stu-id="bec79-518">Take advange of unobstrusive client-side validation with jQuery in Web Forms</span></span>
- <span data-ttu-id="bec79-519">實作詳細的要求驗證</span><span class="sxs-lookup"><span data-stu-id="bec79-519">Implement granular request validation</span></span>
- <span data-ttu-id="bec79-520">實作非同步頁面處理 Web Form 中</span><span class="sxs-lookup"><span data-stu-id="bec79-520">Implement asynchronous page processing in Web Forms</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="bec79-521">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="bec79-521">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="bec79-522">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="bec79-522">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="bec79-523">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="bec79-523">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="bec79-524">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="bec79-524">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="bec79-525">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="bec79-525">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="bec79-526">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="bec79-526">Click on **Install Now**.</span></span> <span data-ttu-id="bec79-527">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="bec79-527">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="bec79-528">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-528">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="bec79-529">![安裝 Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="bec79-529">![Install Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="bec79-530">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="bec79-530">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="bec79-531">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="bec79-531">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    <span data-ttu-id="bec79-533">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="bec79-533">*Accepting the license terms*</span></span>
5. <span data-ttu-id="bec79-534">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="bec79-534">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    <span data-ttu-id="bec79-536">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="bec79-536">*Installation progress*</span></span>
6. <span data-ttu-id="bec79-537">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="bec79-537">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    <span data-ttu-id="bec79-539">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="bec79-539">*Installation completed*</span></span>
7. <span data-ttu-id="bec79-540">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="bec79-540">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="bec79-541">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="bec79-541">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    <span data-ttu-id="bec79-543">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="bec79-543">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="bec79-544">附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="bec79-544">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="bec79-545">本附錄將說明如何從 Azure 入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行所提供的功能由 Azure 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-545">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="bec79-546">工作 1： 從 Azure 入口網站中建立新的網站</span><span class="sxs-lookup"><span data-stu-id="bec79-546">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="bec79-547">移至[Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="bec79-547">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-548">使用 Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。</span><span class="sxs-lookup"><span data-stu-id="bec79-548">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="bec79-549">您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="bec79-549">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="bec79-550">![登入 Windows Azure 入口網站](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="bec79-550">![Log on to Windows Azure portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="bec79-551">*登入入口網站*</span><span class="sxs-lookup"><span data-stu-id="bec79-551">*Log on to the Portal*</span></span>
2. <span data-ttu-id="bec79-552">按一下 **新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="bec79-552">Click **New** on the command bar.</span></span>

    <span data-ttu-id="bec79-553">![建立新的 Web 站台](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="bec79-553">![Creating a new Web Site](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="bec79-554">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="bec79-554">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="bec79-555">按一下 **計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="bec79-555">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="bec79-556">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="bec79-556">Then select **Quick Create** option.</span></span> <span data-ttu-id="bec79-557">新的網站提供可用的 URL，然後按**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="bec79-557">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-558">Azure 是您可以控制和管理雲端中執行的 web 應用程式的主應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-558">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="bec79-559">[快速建立] 選項可讓您從 azure 入口網站外部的已完成的 web 應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="bec79-559">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="bec79-560">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="bec79-560">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="bec79-561">![建立新的網站上，使用 快速建立](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "建立新的網站上，使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="bec79-561">![Creating a new Web Site using Quick Create](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="bec79-562">*建立新的網站上，使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="bec79-562">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="bec79-563">等到新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="bec79-563">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="bec79-564">建立網站後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="bec79-564">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="bec79-565">檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="bec79-565">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="bec79-566">![瀏覽至新的 web 站台](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="bec79-566">![Browsing to the new web site](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="bec79-567">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="bec79-567">*Browsing to the new web site*</span></span>

    <span data-ttu-id="bec79-568">![執行的 web 站台](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="bec79-568">![Web site running](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Web site running")</span></span>

    <span data-ttu-id="bec79-569">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="bec79-569">*Web site running*</span></span>
6. <span data-ttu-id="bec79-570">返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="bec79-570">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="bec79-571">![開啟 網站管理頁面](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "開啟的網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="bec79-571">![Opening the web site management pages](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="bec79-572">*開啟 網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="bec79-572">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="bec79-573">在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="bec79-573">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-574">*發行設定檔*包含所有 web 應用程式發行至 Azure 的每個已啟用的發行方法所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="bec79-574">The *publish profile* contains all of the information required to publish a web application to Azure for each enabled publication method.</span></span> <span data-ttu-id="bec79-575">發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。</span><span class="sxs-lookup"><span data-stu-id="bec79-575">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="bec79-576">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Azure web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bec79-576">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="bec79-577">![正在下載網站發行設定檔](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="bec79-577">![Downloading the web site publish profile](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="bec79-578">*正在下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="bec79-578">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="bec79-579">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="bec79-579">Download the publish profile file to a known location.</span></span> <span data-ttu-id="bec79-580">進一步在這個練習中，您會看到如何使用此檔案來發佈 Azure web 應用程式從 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bec79-580">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="bec79-581">![儲存發行設定檔](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "儲存發佈設定檔")</span><span class="sxs-lookup"><span data-stu-id="bec79-581">![Saving the publish profile file](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Saving the publish profile")</span></span>

    <span data-ttu-id="bec79-582">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="bec79-582">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="bec79-583">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="bec79-583">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="bec79-584">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="bec79-584">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="bec79-585">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="bec79-585">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="bec79-586">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="bec79-586">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="bec79-587">您可以從您的訂用帳戶，在 Azure 管理入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器的儀表板**.</span><span class="sxs-lookup"><span data-stu-id="bec79-587">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="bec79-588">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="bec79-588">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="bec79-589">請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="bec79-589">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="bec79-590">並未建立資料庫，因為它會建立在稍後的階段。</span><span class="sxs-lookup"><span data-stu-id="bec79-590">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="bec79-591">![SQL Database 伺服器儀表板](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="bec79-591">![SQL Database Server Dashboard](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="bec79-592">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="bec79-592">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="bec79-593">下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="bec79-593">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="bec79-594">若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="bec79-594">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) button.</span></span>

    ![新增用戶端 IP 位址](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    <span data-ttu-id="bec79-596">*新增用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="bec79-596">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="bec79-597">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="bec79-597">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    <span data-ttu-id="bec79-599">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="bec79-599">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="bec79-600">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="bec79-600">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="bec79-601">返回至 ASP.NET MVC 4 的方案。</span><span class="sxs-lookup"><span data-stu-id="bec79-601">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="bec79-602">在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="bec79-602">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="bec79-603">![發行應用程式](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="bec79-603">![Publishing the Application](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publishing the Application")</span></span>

    <span data-ttu-id="bec79-604">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="bec79-604">*Publishing the web site*</span></span>
2. <span data-ttu-id="bec79-605">匯入發行設定檔儲存在第一項工作。</span><span class="sxs-lookup"><span data-stu-id="bec79-605">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="bec79-606">![匯入發行設定檔](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="bec79-606">![Importing the publish profile](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importing the publish profile")</span></span>

    <span data-ttu-id="bec79-607">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="bec79-607">*Importing publish profile*</span></span>
3. <span data-ttu-id="bec79-608">按一下 **驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="bec79-608">Click **Validate Connection**.</span></span> <span data-ttu-id="bec79-609">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="bec79-609">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bec79-610">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="bec79-610">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="bec79-611">![驗證連接](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="bec79-611">![Validating connection](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Validating connection")</span></span>

    <span data-ttu-id="bec79-612">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="bec79-612">*Validating connection*</span></span>
4. <span data-ttu-id="bec79-613">在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="bec79-613">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="bec79-614">![Web 部署組態](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="bec79-614">![Web deploy configuration](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web deploy configuration")</span></span>

    <span data-ttu-id="bec79-615">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="bec79-615">*Web deploy configuration*</span></span>
5. <span data-ttu-id="bec79-616">設定資料庫連線，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bec79-616">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="bec79-617">在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。</span><span class="sxs-lookup"><span data-stu-id="bec79-617">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="bec79-618">在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="bec79-618">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="bec79-619">在 **密碼**輸入您的伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-619">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="bec79-620">輸入新的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="bec79-620">Type a new database name.</span></span>

     <span data-ttu-id="bec79-621">![設定目的地連接字串](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="bec79-621">![Configuring destination connection string](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="bec79-622">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="bec79-622">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="bec79-623">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="bec79-623">Then click **OK**.</span></span> <span data-ttu-id="bec79-624">當系統提示您建立資料庫時，按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="bec79-624">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="bec79-625">![建立資料庫](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="bec79-625">![Creating the database](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Creating the database string")</span></span>

    <span data-ttu-id="bec79-626">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="bec79-626">*Creating the database*</span></span>
7. <span data-ttu-id="bec79-627">您將用來連接到 SQL Database Azure 的連接字串會顯示在文字方塊中的預設連線。</span><span class="sxs-lookup"><span data-stu-id="bec79-627">The connection string you will use to connect to SQL Database in Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="bec79-628">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="bec79-628">Then click **Next**.</span></span>

    <span data-ttu-id="bec79-629">![連接字串指向 SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "連接字串指向 SQL Database")</span><span class="sxs-lookup"><span data-stu-id="bec79-629">![Connection string pointing to SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="bec79-630">*連接字串指向 SQL Database*</span><span class="sxs-lookup"><span data-stu-id="bec79-630">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="bec79-631">在  **Preview**頁面上，按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="bec79-631">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="bec79-632">![Web 應用程式發行](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="bec79-632">![Publishing the web application](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publishing the web application")</span></span>

    <span data-ttu-id="bec79-633">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="bec79-633">*Publishing the web application*</span></span>
9. <span data-ttu-id="bec79-634">當發行程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="bec79-634">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="bec79-635">附錄 c： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="bec79-635">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="bec79-636">使用程式碼片段，您會有您需要在隨手可得的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-636">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="bec79-637">實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="bec79-637">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="bec79-638">![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="bec79-638">![Using Visual Studio code snippets to insert code into your project](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="bec79-639">*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="bec79-639">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="bec79-640">***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="bec79-640">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="bec79-641">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="bec79-641">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="bec79-642">開始輸入程式碼片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="bec79-642">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="bec79-643">觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="bec79-643">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="bec79-644">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="bec79-644">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="bec79-645">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="bec79-645">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="bec79-646">![開始鍵入程式碼片段名稱](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "開始輸入程式碼片段名稱")</span><span class="sxs-lookup"><span data-stu-id="bec79-646">![Start typing the snippet name](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Start typing the snippet name")</span></span>

<span data-ttu-id="bec79-647">*開始鍵入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="bec79-647">*Start typing the snippet name*</span></span>

<span data-ttu-id="bec79-648">![按 Tab 鍵以選取反白顯示的程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="bec79-648">![Press Tab to select the highlighted snippet](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="bec79-649">*按 Tab 鍵以選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="bec79-649">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="bec79-650">![再次按 Tab 鍵和程式碼片段會依序展開](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "再次按 Tab 鍵和程式碼片段會展開")</span><span class="sxs-lookup"><span data-stu-id="bec79-650">![Press Tab again and the snippet will expand](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="bec79-651">*再次按 Tab 鍵和程式碼片段會展開*</span><span class="sxs-lookup"><span data-stu-id="bec79-651">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="bec79-652">***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="bec79-652">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="bec79-653">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="bec79-653">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="bec79-654">選取 **插入程式碼片段**後面**My Code Snippets**。</span><span class="sxs-lookup"><span data-stu-id="bec79-654">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="bec79-655">選擇相關的程式片段，從清單中，對它按一下。</span><span class="sxs-lookup"><span data-stu-id="bec79-655">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="bec79-656">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="bec79-656">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="bec79-657">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="bec79-657">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="bec79-658">![對它按一下挑選清單中，相關程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "對它按一下挑選清單中，相關程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="bec79-658">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="bec79-659">*對它按一下挑選清單中，相關程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="bec79-659">*Pick the relevant snippet from the list, by clicking on it*</span></span>
