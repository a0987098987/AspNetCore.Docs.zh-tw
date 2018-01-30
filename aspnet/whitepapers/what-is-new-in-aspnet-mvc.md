---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: "新 ASP.NET MVC 2 |Microsoft 文件"
author: rick-anderson
description: "本文件說明新功能和 ASP.NET MVC 2 中引進的增強功能。 也可以下載這份文件。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-mvc-2"></a><span data-ttu-id="45a79-104">ASP.NET MVC 2 中最新消息</span><span class="sxs-lookup"><span data-stu-id="45a79-104">What's New in ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="45a79-105">本文件說明新功能和 ASP.NET MVC 2 中引進的增強功能。</span><span class="sxs-lookup"><span data-stu-id="45a79-105">This document describes new features and improvements introduced in ASP.NET MVC 2.</span></span> <span data-ttu-id="45a79-106">本文件也會提供如[下載](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)</span><span class="sxs-lookup"><span data-stu-id="45a79-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)</span></span>


<span data-ttu-id="45a79-107">[簡介](#_TOC1) </span><span class="sxs-lookup"><span data-stu-id="45a79-107">[Introduction](#_TOC1) </span></span>  
<span data-ttu-id="45a79-108">[ASP.NET MVC 1.0 專案升級至 ASP.NET MVC 2](#_TOC2) </span><span class="sxs-lookup"><span data-stu-id="45a79-108">[Upgrading an ASP.NET MVC 1.0 Project to ASP.NET MVC 2](#_TOC2) </span></span>  
<span data-ttu-id="45a79-109">[新功能](#_TOC3) </span><span class="sxs-lookup"><span data-stu-id="45a79-109">[New Features](#_TOC3) </span></span>  
<span data-ttu-id="45a79-110">[樣板化 Helper](#_TOC3_1) </span><span class="sxs-lookup"><span data-stu-id="45a79-110">[Templated Helpers](#_TOC3_1) </span></span>  
<span data-ttu-id="45a79-111">[區域](#_TOC3_2) </span><span class="sxs-lookup"><span data-stu-id="45a79-111">[Areas](#_TOC3_2) </span></span>  
<span data-ttu-id="45a79-112">[支援非同步控制器](#_TOC3_3) </span><span class="sxs-lookup"><span data-stu-id="45a79-112">[Support for Asynchronous Controllers](#_TOC3_3) </span></span>  
<span data-ttu-id="45a79-113">[DefaultValueAttribute 在動作方法參數的支援](#_TOC3_4) </span><span class="sxs-lookup"><span data-stu-id="45a79-113">[Support for DefaultValueAttribute in Action-Method Parameters](#_TOC3_4) </span></span>  
<span data-ttu-id="45a79-114">[繫結模型繫結器的二進位資料支援](#_TOC3_5) </span><span class="sxs-lookup"><span data-stu-id="45a79-114">[Support for Binding Binary Data with Model Binders](#_TOC3_5) </span></span>  
<span data-ttu-id="45a79-115">[ModelMetadata 和 ModelMetadataProvider 類別](#_TOC3_6) </span><span class="sxs-lookup"><span data-stu-id="45a79-115">[ModelMetadata and ModelMetadataProvider Classes](#_TOC3_6) </span></span>  
<span data-ttu-id="45a79-116">[支援 DataAnnotations 屬性](#_TOC3_7) </span><span class="sxs-lookup"><span data-stu-id="45a79-116">[Support for DataAnnotations Attributes](#_TOC3_7) </span></span>  
<span data-ttu-id="45a79-117">[模型驗證程式提供者](#_TOC3_8) </span><span class="sxs-lookup"><span data-stu-id="45a79-117">[Model-Validator Providers](#_TOC3_8) </span></span>  
<span data-ttu-id="45a79-118">[用戶端驗證](#_TOC3_9) </span><span class="sxs-lookup"><span data-stu-id="45a79-118">[Client-Side Validation](#_TOC3_9) </span></span>  
<span data-ttu-id="45a79-119">[適用於 Visual Studio 2010 的新程式碼片段](#_TOC3_10) </span><span class="sxs-lookup"><span data-stu-id="45a79-119">[New Code Snippets for Visual Studio 2010](#_TOC3_10) </span></span>  
<span data-ttu-id="45a79-120">[新的 RequireHttpsAttribute 動作篩選條件](#_TOC3_11) </span><span class="sxs-lookup"><span data-stu-id="45a79-120">[New RequireHttpsAttribute Action Filter](#_TOC3_11) </span></span>  
<span data-ttu-id="45a79-121">[覆寫的 HTTP 方法的動詞命令](#_TOC3_12) </span><span class="sxs-lookup"><span data-stu-id="45a79-121">[Overriding the HTTP Method Verb](#_TOC3_12) </span></span>  
<span data-ttu-id="45a79-122">[樣板化 Helper 的新 HiddenInputAttribute 類別](#_TOC3_13) </span><span class="sxs-lookup"><span data-stu-id="45a79-122">[New HiddenInputAttribute Class for Templated Helpers](#_TOC3_13) </span></span>  
<span data-ttu-id="45a79-123">[Html.ValidationSummary Helper 方法可顯示模型層級錯誤](#_TOC3_14) </span><span class="sxs-lookup"><span data-stu-id="45a79-123">[Html.ValidationSummary Helper Method Can Display Model-Level Errors](#_TOC3_14) </span></span>  
<span data-ttu-id="45a79-124">[T4 範本在 Visual Studio 產生程式碼在特定的.NET framework 目標版本](#_TOC3_15)[應用程式開發介面改進](#_TOC4)</span><span class="sxs-lookup"><span data-stu-id="45a79-124">[T4 Templates in Visual Studio Generate Code that is Specific to the Target Version of the .NET Framework](#_TOC3_15)[API Improvements](#_TOC4)</span></span>  
[<span data-ttu-id="45a79-125">重大變更</span><span class="sxs-lookup"><span data-stu-id="45a79-125">Breaking Changes</span></span>](#_TOC5)  
[<span data-ttu-id="45a79-126">Disclaimer</span><span class="sxs-lookup"><span data-stu-id="45a79-126">Disclaimer</span></span>](#_TOC6)  

## <a id="_TOC1"></a><span data-ttu-id="45a79-127">簡介</span><span class="sxs-lookup"><span data-stu-id="45a79-127">Introduction</span></span>

<span data-ttu-id="45a79-128">ASP.NET MVC 2 ASP.NET MVC 1.0 為基礎，並介紹增強功能和功能，將焦點放在可以提升產能的大型集合。</span><span class="sxs-lookup"><span data-stu-id="45a79-128">ASP.NET MVC 2 builds on ASP.NET MVC 1.0 and introduces a large set of enhancements and features that are focused on increasing productivity.</span></span> <span data-ttu-id="45a79-129">此版本是與 ASP.NET MVC 1.0 相容，因此所有您知識、 技術、 程式碼和 ASP.NET MVC 1.0 延伸仍會繼續套用。</span><span class="sxs-lookup"><span data-stu-id="45a79-129">This release is compatible with ASP.NET MVC 1.0, so all your knowledge, skills, code, and extensions for ASP.NET MVC 1.0 continue to apply.</span></span>

<span data-ttu-id="45a79-130">如需有關 ASP.NET MVC 的詳細資訊，請瀏覽下列資源：</span><span class="sxs-lookup"><span data-stu-id="45a79-130">For more information about ASP.NET MVC, visit the following resources:</span></span>

- [<span data-ttu-id="45a79-131">MSDN 上的 ASP.NET MVC 文件</span><span class="sxs-lookup"><span data-stu-id="45a79-131">ASP.NET MVC documentation on MSDN</span></span>](https://go.microsoft.com/fwlink/?LinkId=159758)
- [<span data-ttu-id="45a79-132">ASP.NET MVC 網站</span><span class="sxs-lookup"><span data-stu-id="45a79-132">The ASP.NET MVC Web site</span></span>](https://asp.net/mvc/)
- [<span data-ttu-id="45a79-133">ASP.NET MVC 論壇</span><span class="sxs-lookup"><span data-stu-id="45a79-133">The ASP.NET MVC forums</span></span>](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a><span data-ttu-id="45a79-134">ASP.NET MVC 1.0 專案升級至 ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="45a79-134">Upgrading an ASP.NET MVC 1.0 Project to ASP.NET MVC 2</span></span>

<span data-ttu-id="45a79-135">ASP.NET MVC 2 可以與 ASP.NET MVC 1.0 並存安裝在相同伺服器上，讓應用程式開發人員在選擇何時要升級至 ASP.NET MVC 2 ASP.NET MVC 1.0 應用程式的彈性。</span><span class="sxs-lookup"><span data-stu-id="45a79-135">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server, which gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span> <span data-ttu-id="45a79-136">如需如何升級的資訊，請參閱文件[升級至 ASP.NET MVC 2 1.0 ASP.NET MVC 應用程式](https://go.microsoft.com/fwlink/?LinkID=185459)。</span><span class="sxs-lookup"><span data-stu-id="45a79-136">For information on how to upgrade, see the document [Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).</span></span>

## <a id="_TOC3"></a><span data-ttu-id="45a79-137">新功能</span><span class="sxs-lookup"><span data-stu-id="45a79-137">New Features</span></span>

<span data-ttu-id="45a79-138">本節說明已引進的功能在 MVC 2 版本。</span><span class="sxs-lookup"><span data-stu-id="45a79-138">This section describes features that have been introduced in the MVC 2 release.</span></span>

### <a id="_TOC3_1"></a><span data-ttu-id="45a79-139">樣板化 Helper</span><span class="sxs-lookup"><span data-stu-id="45a79-139">Templated Helpers</span></span>

<span data-ttu-id="45a79-140">樣板化 helper 可讓您自動產生關聯以編輯的 HTML 項目，並顯示與資料型別。</span><span class="sxs-lookup"><span data-stu-id="45a79-140">Templated helpers let you automatically associate HTML elements for edit and display with data types.</span></span> <span data-ttu-id="45a79-141">例如，在檢視中顯示 System.DateTime 類型的資料時，日期選擇器 UI 項目可以自動轉譯。</span><span class="sxs-lookup"><span data-stu-id="45a79-141">For example, when data of type System.DateTime is displayed in a view, a date-picker UI element can be automatically rendered.</span></span> <span data-ttu-id="45a79-142">這是類似於在 ASP.NET 動態資料欄位範本的運作方式。</span><span class="sxs-lookup"><span data-stu-id="45a79-142">This is similar to how field templates work in ASP.NET Dynamic Data.</span></span> <span data-ttu-id="45a79-143">如需詳細資訊，請參閱[來顯示的資料使用樣板化 Helper](https://go.microsoft.com/fwlink/?LinkId=159062) MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="45a79-143">For more information, see [Using Templated Helpers to Display Data](https://go.microsoft.com/fwlink/?LinkId=159062) on the MSDN Web site.</span></span>

### <a id="_TOC3_2"></a><span data-ttu-id="45a79-144">區域</span><span class="sxs-lookup"><span data-stu-id="45a79-144">Areas</span></span>

<span data-ttu-id="45a79-145">區域可讓您將大型專案組織成多個較小區段，才能管理大型的 Web 應用程式的複雜性。</span><span class="sxs-lookup"><span data-stu-id="45a79-145">Areas let you organize a large project into multiple smaller sections in order to manage the complexity of a large Web application.</span></span> <span data-ttu-id="45a79-146">每個區段 （「 區域 」） 通常代表不同區段的大型網站，並將用來分組的控制器和檢視的相關的設定。</span><span class="sxs-lookup"><span data-stu-id="45a79-146">Each section ("area") typically represents a separate section of a large Web site and is used to group related sets of controllers and views.</span></span> <span data-ttu-id="45a79-147">如需詳細資訊，請參閱[逐步解說： 組織 ASP.NET MVC 應用程式依區域](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="45a79-147">For more information, see [Walkthrough: Organizing an ASP.NET MVC Application by Areas](https://go.microsoft.com/fwlink/?LinkId=158978) on the MSDN Web site.</span></span>

<span data-ttu-id="45a79-148">若要建立新的區域，請在 [方案總管] 中，以滑鼠右鍵按一下專案，按一下 [新增]，然後按一下區域。</span><span class="sxs-lookup"><span data-stu-id="45a79-148">To create a new area, in Solution Explorer, right-click the project, click Add, and then click Area.</span></span> <span data-ttu-id="45a79-149">這會顯示對話方塊提示您輸入的區域名稱。</span><span class="sxs-lookup"><span data-stu-id="45a79-149">This displays a dialog box that prompts you for the area name.</span></span> <span data-ttu-id="45a79-150">輸入區域名稱之後，Visual Studio 將專案加入新的區域。</span><span class="sxs-lookup"><span data-stu-id="45a79-150">After you enter the area name, Visual Studio adds a new area to the project.</span></span>

<span data-ttu-id="45a79-151">下圖顯示具有兩個區域，系統管理員和部落格專案配置範例。</span><span class="sxs-lookup"><span data-stu-id="45a79-151">The following figure shows an example layout for a project with two areas, Admin and Blogs.</span></span>

![](what-is-new-in-aspnet-mvc/_static/image1.png)

<span data-ttu-id="45a79-152">當您建立區域時，Visual Studio 會加入至每個區域從 AreaRegistration 衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="45a79-152">When you create an area, Visual Studio adds a class that derives from AreaRegistration to each area.</span></span> <span data-ttu-id="45a79-153">這個類別，才能註冊區域和其路由中，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="45a79-153">This class is required in order to register the area and its routes, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

<span data-ttu-id="45a79-154">ASP.NET MVC 2 的預設專案範本在 Global.asax 檔案的程式碼中包含 RegisterAllAreas 方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="45a79-154">The default project template for ASP.NET MVC 2 includes a call to the RegisterAllAreas method in the code for the Global.asax file.</span></span> <span data-ttu-id="45a79-155">這個方法會尋找所有衍生自 AreaRegistration 類別的類型、 具現化類型的執行個體以及執行個體上呼叫 RegisterArea 方法註冊在專案中的每個區域。</span><span class="sxs-lookup"><span data-stu-id="45a79-155">This method registers each area in the project by looking for all types that derive from the AreaRegistration class, instantiating an instance of the type, and then calling the RegisterArea method on the instance.</span></span> <span data-ttu-id="45a79-156">下列範例會示範方式。</span><span class="sxs-lookup"><span data-stu-id="45a79-156">The following example shows how this is done.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

<span data-ttu-id="45a79-157">如果您未藉由呼叫內容 RegisterArea 方法中指定的命名空間。預設會使用 Namespaces.Add 方法，登錄類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="45a79-157">If you do not specify the namespace in the RegisterArea method by calling the context.Namespaces.Add method, the namespace of the registration class is used by default.</span></span>

### <a id="_TOC3_3"></a><span data-ttu-id="45a79-158">支援非同步控制器</span><span class="sxs-lookup"><span data-stu-id="45a79-158">Support for Asynchronous Controllers</span></span>

<span data-ttu-id="45a79-159">ASP.NET MVC 2 現在可讓以非同步方式處理要求的控制站。</span><span class="sxs-lookup"><span data-stu-id="45a79-159">ASP.NET MVC 2 now allows controllers to process requests asynchronously.</span></span> <span data-ttu-id="45a79-160">藉由使用伺服器經常呼叫改為呼叫非封鎖的對應項目 （如網路要求） 的封鎖作業，這可能會導致效能提升。</span><span class="sxs-lookup"><span data-stu-id="45a79-160">This can lead to performance gains by allowing servers which frequently call blocking operations (like network requests) to call non-blocking counterparts instead.</span></span> <span data-ttu-id="45a79-161">如需詳細資訊，請參閱[在 ASP.NET MVC 中使用非同步控制器](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)MSDN 上的主題。</span><span class="sxs-lookup"><span data-stu-id="45a79-161">For more information, see the [Using an Asynchronous Controller in ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) topic on MSDN.</span></span>

### <a id="_TOC3_4"></a><span data-ttu-id="45a79-162">DefaultValueAttribute 在動作方法參數的支援</span><span class="sxs-lookup"><span data-stu-id="45a79-162">Support for DefaultValueAttribute in Action-Method Parameters</span></span>

<span data-ttu-id="45a79-163">System.ComponentModel.DefaultValueAttribute 類別可讓動作方法的引數參數所提供的預設值。</span><span class="sxs-lookup"><span data-stu-id="45a79-163">The System.ComponentModel.DefaultValueAttribute class allows a default value to be supplied for the argument parameter to an action method.</span></span> <span data-ttu-id="45a79-164">例如，假設下列預設路由，定義：</span><span class="sxs-lookup"><span data-stu-id="45a79-164">For example, assume that the following default route is defined:</span></span>

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

<span data-ttu-id="45a79-165">也假設您已定義下列的控制器與動作方法：</span><span class="sxs-lookup"><span data-stu-id="45a79-165">Also assume that the following controller and action method is defined:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

<span data-ttu-id="45a79-166">任何下列要求 Url 會叫用前一個範例中定義的檢視動作方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-166">Any of the following request URLs will invoke the View action method that is defined in the preceding example.</span></span>

- <span data-ttu-id="45a79-167">/ 文件/檢視/123</span><span class="sxs-lookup"><span data-stu-id="45a79-167">/Article/View/123</span></span>
- <span data-ttu-id="45a79-168">/ 文件/檢視/123？ 頁面 = 1 （有效地如同前一個要求）</span><span class="sxs-lookup"><span data-stu-id="45a79-168">/Article/View/123?page=1 (Effectively the same as the previous request)</span></span>
- <span data-ttu-id="45a79-169">/Article/View/123?page=2</span><span class="sxs-lookup"><span data-stu-id="45a79-169">/Article/View/123?page=2</span></span>

<span data-ttu-id="45a79-170">若沒有 DefaultValueAttribute 屬性從上述清單的第一個 URL 不會執行，因為頁面引數是尚未提供其值不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="45a79-170">Without the DefaultValueAttribute attribute, the first URL from the preceding list would not work, because the page argument is a non-nullable value type whose value has not been provided.</span></span>

<span data-ttu-id="45a79-171">如果在 Visual Basic 2010 或 Visual C# 2010年中撰寫的程式碼，您可以使用選擇性參數，而不是 DefaultValueAttribute 屬性，如下列範例所示即可：</span><span class="sxs-lookup"><span data-stu-id="45a79-171">If your code is written in Visual Basic 2010 or Visual C# 2010, you can use optional parameters instead of the DefaultValueAttribute attribute, as shown in the following example:</span></span>

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a><span data-ttu-id="45a79-172">繫結模型繫結器的二進位資料支援</span><span class="sxs-lookup"><span data-stu-id="45a79-172">Support for Binding Binary Data with Model Binders</span></span>

<span data-ttu-id="45a79-173">有兩個新的多載 Html.Hidden helper 編碼為 base-64 編碼字串的二進位值：</span><span class="sxs-lookup"><span data-stu-id="45a79-173">There are two new overloads of the Html.Hidden helper that encode binary values as base-64-encoded strings:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

<span data-ttu-id="45a79-174">一般用法是內嵌在檢視中物件的時間戳記。</span><span class="sxs-lookup"><span data-stu-id="45a79-174">A typical use is to embed a timestamp for an object in the view.</span></span> <span data-ttu-id="45a79-175">例如，您的應用程式可能包含下列產品物件：</span><span class="sxs-lookup"><span data-stu-id="45a79-175">For example, your application might include the following Product object:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

<span data-ttu-id="45a79-176">編輯表單可以轉譯格式，如下列範例所示的時間戳記屬性：</span><span class="sxs-lookup"><span data-stu-id="45a79-176">An edit form can render the TimeStamp property in the form as shown in the following example:</span></span>

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

<span data-ttu-id="45a79-177">這個標記會隱藏輸入的元素具有時間戳記值轉譯為 base 64 編碼的字串，類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="45a79-177">This markup renders a hidden input element with the timestamp value as a base-64-encoded string that resembles the following example:</span></span>

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

<span data-ttu-id="45a79-178">此表單可能會張貼至動作方法具有產品類型的引數，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="45a79-178">This form might be posted to an action method that has an argument of type Product, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

<span data-ttu-id="45a79-179">在動作方法，將時間戳記屬性就會擴展正確因為張貼的 base-64 編碼字串轉換成位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="45a79-179">In the action method, the TimeStamp property is populated correctly because the posted base-64-encoded string is converted to a byte array.</span></span>

### <a id="_TOC3_6"></a><span data-ttu-id="45a79-180">ModelMetadata 和 ModelMetadataProvider 類別</span><span class="sxs-lookup"><span data-stu-id="45a79-180">ModelMetadata and ModelMetadataProvider Classes</span></span>

<span data-ttu-id="45a79-181">ModelMetadataProvider 類別提供用來取得內檢視的模型中繼資料的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="45a79-181">The ModelMetadataProvider class provides an abstraction for obtaining metadata for the model within a view.</span></span> <span data-ttu-id="45a79-182">MVC 2 包含預設提供者，可提供 System.ComponentModel.DataAnnotations 命名空間中的屬性所公開的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="45a79-182">MVC 2 includes a default provider that makes available the metadata that is exposed by the attributes in the System.ComponentModel.DataAnnotations namespace.</span></span> <span data-ttu-id="45a79-183">很可能會建立提供從其他資料存放區，例如資料庫或 XML 檔案的中繼資料的中繼資料提供者。</span><span class="sxs-lookup"><span data-stu-id="45a79-183">It is possible to create metadata providers that provide metadata from other data stores, such as databases or XML files.</span></span>

<span data-ttu-id="45a79-184">ViewDataDictionary 類別會公開包含由 ModelMetadataProvider 類別從模型擷取中繼資料的 ModelMetadata 物件。</span><span class="sxs-lookup"><span data-stu-id="45a79-184">The ViewDataDictionary class exposes a ModelMetadata object that contains the metadata that is extracted from the model by the ModelMetadataProvider class.</span></span> <span data-ttu-id="45a79-185">這可讓取用這個中繼資料，並據以調整其輸出的樣板化 helper。</span><span class="sxs-lookup"><span data-stu-id="45a79-185">This enables the templated helpers to consume this metadata and adjust their output accordingly.</span></span>

<span data-ttu-id="45a79-186">如需詳細資訊，請參閱文件[ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)和[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="45a79-186">For more information, see the documentation for the [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) and [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) classes.</span></span>

### <a id="_TOC3_7"></a><span data-ttu-id="45a79-187">支援 DataAnnotations 屬性</span><span class="sxs-lookup"><span data-stu-id="45a79-187">Support for DataAnnotations Attributes</span></span>

<span data-ttu-id="45a79-188">ASP.NET MVC 2 支援使用 RangeAttribute、 出現 RequiredAttribute、 StringLengthAttribute 和 RegexAttribute 驗證屬性 （System.ComponentModel.DataAnnotations 命名空間中定義） 當您繫結至模型以提供的輸入驗證。</span><span class="sxs-lookup"><span data-stu-id="45a79-188">ASP.NET MVC 2 supports using the RangeAttribute, RequiredAttribute, StringLengthAttribute, and RegexAttribute validation attributes (defined in the System.ComponentModel.DataAnnotations namespace) when you bind to a model in order to provide input validation.</span></span>

<span data-ttu-id="45a79-189">如需詳細資訊，請參閱[How to： 驗證模型資料使用 DataAnnotations 屬性](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="45a79-189">For more information, see [How to: Validate Model Data Using DataAnnotations Attributes](https://go.microsoft.com/fwlink/?LinkId=159063) on the MSDN Web site.</span></span> <span data-ttu-id="45a79-190">說明如何使用這些屬性的範例專案會下載[https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753)。</span><span class="sxs-lookup"><span data-stu-id="45a79-190">A sample project that illustrates the use of these attributes is available for download at [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).</span></span>

### <a id="_TOC3_8"></a><span data-ttu-id="45a79-191">模型驗證程式提供者</span><span class="sxs-lookup"><span data-stu-id="45a79-191">Model-Validator Providers</span></span>

<span data-ttu-id="45a79-192">模型驗證提供者類別代表模型提供驗證邏輯的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="45a79-192">The model-validation provider class represents an abstraction that provides validation logic for the model.</span></span> <span data-ttu-id="45a79-193">ASP.NET MVC 包括根據 System.ComponentModel.DataAnnotations 命名空間中包含的驗證屬性的預設提供者。</span><span class="sxs-lookup"><span data-stu-id="45a79-193">ASP.NET MVC includes a default provider based on validation attributes that are included in the System.ComponentModel.DataAnnotations namespace.</span></span> <span data-ttu-id="45a79-194">您也可以建立自己的驗證提供者定義模型的自訂驗證規則和自訂驗證規則的對應。</span><span class="sxs-lookup"><span data-stu-id="45a79-194">You can also create your own validation providers that define custom validation rules and custom mappings of validation rules to the model.</span></span> <span data-ttu-id="45a79-195">如需詳細資訊，請參閱文件[ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="45a79-195">For more information, see the documentation for the [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) class.</span></span>

### <a id="_TOC3_9"></a><span data-ttu-id="45a79-196">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="45a79-196">Client-Side Validation</span></span>

<span data-ttu-id="45a79-197">模型驗證程式提供者類別公開給可供用戶端驗證程式庫的 JSON 序列化資料的形式瀏覽器的驗證中繼資料。</span><span class="sxs-lookup"><span data-stu-id="45a79-197">The model-validator provider class exposes validation metadata to the browser in the form of JSON-serialized data that can be consumed by a client-side validation library.</span></span> <span data-ttu-id="45a79-198">ASP.NET MVC 2 包含用戶端驗證程式庫和配接器支援先前所述的 DataAnnotations 命名空間驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-198">ASP.NET MVC 2 includes a client validation library and adapter that supports the DataAnnotations namespace validation attributes noted earlier.</span></span> <span data-ttu-id="45a79-199">提供者類別也可讓您使用其他用戶端驗證程式庫所撰寫的配接器處理的 JSON 資料和其他程式庫的呼叫。</span><span class="sxs-lookup"><span data-stu-id="45a79-199">The provider class also enables you to use other client-validation libraries by writing an adapter that processes the JSON data and calls into the alternate library.</span></span>

### <a id="_TOC3_10"></a><span data-ttu-id="45a79-200">適用於 Visual Studio 2010 的新程式碼片段</span><span class="sxs-lookup"><span data-stu-id="45a79-200">New Code Snippets for Visual Studio 2010</span></span>

<span data-ttu-id="45a79-201">ASP.NET MVC 2 的 HTML 程式碼片段的一組會隨 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="45a79-201">A set of HTML code snippets for ASP.NET MVC 2 is installed with Visual Studio 2010.</span></span> <span data-ttu-id="45a79-202">若要檢視的這些片段，在 [工具] 功能表中，選取 [程式碼片段管理員。</span><span class="sxs-lookup"><span data-stu-id="45a79-202">To view a list of these snippets, in the Tools menu, select Code Snippets Manager.</span></span> <span data-ttu-id="45a79-203">語言中，選取 [HTML] 和位置，請選取 [ASP.NET MVC 2。</span><span class="sxs-lookup"><span data-stu-id="45a79-203">For the language, select HTML, and for location, select ASP.NET MVC 2.</span></span> <span data-ttu-id="45a79-204">如需如何使用程式碼片段的詳細資訊，請參閱 Visual Studio 文件。</span><span class="sxs-lookup"><span data-stu-id="45a79-204">For more information about how to use code snippets, see the Visual Studio documentation.</span></span>

### <a id="_TOC3_11"></a><span data-ttu-id="45a79-205">新的 RequireHttpsAttribute 動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="45a79-205">New RequireHttpsAttribute Action Filter</span></span>

<span data-ttu-id="45a79-206">ASP.NET MVC 2 包含新 RequireHttpsAttribute 類別，可以套用至動作方法和控制器。</span><span class="sxs-lookup"><span data-stu-id="45a79-206">ASP.NET MVC 2 includes a new RequireHttpsAttribute class that can be applied to action methods and controllers.</span></span> <span data-ttu-id="45a79-207">根據預設，篩選要求重新導向至非 SSL HTTP 啟用 SSL (HTTPS) 對等項目。</span><span class="sxs-lookup"><span data-stu-id="45a79-207">By default, the filter redirects a non-SSL (HTTP) request to the SSL-enabled (HTTPS) equivalent.</span></span>

### <a id="_TOC3_12"></a><span data-ttu-id="45a79-208">覆寫的 HTTP 方法的動詞命令</span><span class="sxs-lookup"><span data-stu-id="45a79-208">Overriding the HTTP Method Verb</span></span>

<span data-ttu-id="45a79-209">當您使用 rest 架構建置的網站時，可用來判斷何種動作來執行資源的 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="45a79-209">When you build a Web site by using the REST architectural style, HTTP verbs are used to determine which action to perform for a resource.</span></span> <span data-ttu-id="45a79-210">REST 要求該應用程式支援完整的通用的 HTTP 動詞命令，其中包括 GET、 PUT、 POST 和刪除。</span><span class="sxs-lookup"><span data-stu-id="45a79-210">REST requires that applications support the full range of common HTTP verbs, including GET, PUT, POST, and DELETE.</span></span>

<span data-ttu-id="45a79-211">ASP.NET MVC 2 包含新屬性，您可以套用至動作方法和該功能的精簡語法。</span><span class="sxs-lookup"><span data-stu-id="45a79-211">ASP.NET MVC 2 includes new attributes that you can apply to action methods and that feature compact syntax.</span></span> <span data-ttu-id="45a79-212">這些屬性可讓 ASP.NET MVC 選取動作方法，根據 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="45a79-212">These attributes enable ASP.NET MVC to select an action method based on the HTTP verb.</span></span> <span data-ttu-id="45a79-213">在下列範例中，POST 要求會呼叫第一個動作方法和 PUT 要求會呼叫第二個動作方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-213">In the following example, a POST request will call the first action method and a PUT request will call the second action method.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

<span data-ttu-id="45a79-214">在舊版 ASP.NET MVC 中，這些動作方法所需更詳細的語法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="45a79-214">In earlier versions of ASP.NET MVC, these action methods required more verbose syntax, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

<span data-ttu-id="45a79-215">瀏覽器都支援只有 GET 和 POST HTTP 動詞命令，因為它不張貼到需要不同的指令動詞的動作。</span><span class="sxs-lookup"><span data-stu-id="45a79-215">Because browsers support only the GET and POST HTTP verbs, it is not possible to post to an action that requires a different verb.</span></span> <span data-ttu-id="45a79-216">因此它是不可能原本就支援 rest 式的所有要求。</span><span class="sxs-lookup"><span data-stu-id="45a79-216">Thus it is not possible to natively support all RESTful requests.</span></span>

<span data-ttu-id="45a79-217">不過，以支援 RESTful 要求在 POST 作業時，ASP.NET MVC 2 導入新的 HttpMethodOverride HTML helper 方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-217">However, to support RESTful requests during POST operations, ASP.NET MVC 2 introduces a new HttpMethodOverride HTML helper method.</span></span> <span data-ttu-id="45a79-218">這個方法會呈現隱藏的 input 項目，會導致表單，以有效地模擬任何 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-218">This method renders a hidden input element that causes the form to effectively emulate any HTTP method.</span></span> <span data-ttu-id="45a79-219">例如，使用 HttpMethodOverride HTML helper 方法，您可以有出現的表單提交是 PUT 或 DELETE 要求。</span><span class="sxs-lookup"><span data-stu-id="45a79-219">For example, by using the HttpMethodOverride HTML helper method, you can have a form submission appear be a PUT or DELETE request.</span></span> <span data-ttu-id="45a79-220">HttpMethodOverride 的行為會影響下列屬性：</span><span class="sxs-lookup"><span data-stu-id="45a79-220">The behavior of HttpMethodOverride affects the following attributes:</span></span>

- <span data-ttu-id="45a79-221">HttpPostAttribute</span><span class="sxs-lookup"><span data-stu-id="45a79-221">HttpPostAttribute</span></span>
- <span data-ttu-id="45a79-222">HttpPutAttribute</span><span class="sxs-lookup"><span data-stu-id="45a79-222">HttpPutAttribute</span></span>
- <span data-ttu-id="45a79-223">HttpGetAttribute</span><span class="sxs-lookup"><span data-stu-id="45a79-223">HttpGetAttribute</span></span>
- <span data-ttu-id="45a79-224">HttpDeleteAttribute</span><span class="sxs-lookup"><span data-stu-id="45a79-224">HttpDeleteAttribute</span></span>
- <span data-ttu-id="45a79-225">AcceptVerbsAttribute</span><span class="sxs-lookup"><span data-stu-id="45a79-225">AcceptVerbsAttribute</span></span>

<span data-ttu-id="45a79-226">隱藏的 input 項目具有 X-HTTP-方法-覆寫其名稱和其值設為 HTTP 動詞命令，以模擬。</span><span class="sxs-lookup"><span data-stu-id="45a79-226">The hidden input element has its name X-HTTP-Method-Override and its value set to the HTTP verb to emulate.</span></span> <span data-ttu-id="45a79-227">覆寫值也可以指定在 HTTP 標頭或查詢字串值做為名稱/值組。</span><span class="sxs-lookup"><span data-stu-id="45a79-227">The override value can also be specified in an HTTP header or in a query string value as a name/value pair.</span></span>

<span data-ttu-id="45a79-228">POST 要求實際要求時，只可以使用覆寫。</span><span class="sxs-lookup"><span data-stu-id="45a79-228">The override can only be used when the real request is a POST request.</span></span> <span data-ttu-id="45a79-229">使用任何其他 HTTP 動詞命令的要求，將會忽略覆寫值。</span><span class="sxs-lookup"><span data-stu-id="45a79-229">The override value will be ignored for requests that use any other HTTP verb.</span></span>

### <a id="_TOC3_13"></a><span data-ttu-id="45a79-230">樣板化 Helper 的新 HiddenInputAttribute 類別</span><span class="sxs-lookup"><span data-stu-id="45a79-230">New HiddenInputAttribute Class for Templated Helpers</span></span>

<span data-ttu-id="45a79-231">您可以將新 HiddenInputAttribute 屬性套用至模型屬性，指出編輯器範本中顯示此模型時是否應該呈現隱藏的 input 的元素。</span><span class="sxs-lookup"><span data-stu-id="45a79-231">You can apply the new HiddenInputAttribute attribute to a model property to indicate whether a hidden input element should be rendered when displaying the model in an editor template.</span></span> <span data-ttu-id="45a79-232">（此屬性設定 HiddenInput 隱含 UIHint 值）。</span><span class="sxs-lookup"><span data-stu-id="45a79-232">(The attribute sets an implicit UIHint value of HiddenInput).</span></span> <span data-ttu-id="45a79-233">屬性的 DisplayValue 屬性可讓您指定的值編輯器中會顯示與顯示模式。</span><span class="sxs-lookup"><span data-stu-id="45a79-233">The attribute's DisplayValue property lets you specify whether the value is displayed in editor and display modes.</span></span> <span data-ttu-id="45a79-234">當 DisplayValue 設定為 false 時，不會顯示不，即使通常括住欄位的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="45a79-234">When DisplayValue is set to false, nothing is displayed, not even the HTML markup that normally surrounds a field.</span></span> <span data-ttu-id="45a79-235">DisplayValue 的預設值為 true。</span><span class="sxs-lookup"><span data-stu-id="45a79-235">The default value for DisplayValue is true.</span></span>

<span data-ttu-id="45a79-236">在下列案例中，您可以使用 HiddenInputAttribute 屬性：</span><span class="sxs-lookup"><span data-stu-id="45a79-236">You might use HiddenInputAttribute attribute in the following scenarios:</span></span>

- <span data-ttu-id="45a79-237">當檢視可讓使用者編輯物件的識別碼，而且是要提供包含舊的識別碼，因此可以將它傳遞至控制器的隱藏 input 的元素顯示值，以及所需。</span><span class="sxs-lookup"><span data-stu-id="45a79-237">When a view lets users edit the ID of an object and it is necessary to display the value as well as to provide a hidden input element that contains the old ID so that it can be passed back to the controller.</span></span>
- <span data-ttu-id="45a79-238">檢視可讓使用者編輯的二進位屬性應永遠不會顯示，例如時間戳記屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-238">When a view lets users edit a binary property that should never be displayed, such as a timestamp property.</span></span> <span data-ttu-id="45a79-239">在此情況下，不顯示的值與周圍的 HTML 標記 （例如標籤和值）。</span><span class="sxs-lookup"><span data-stu-id="45a79-239">In that case, the value and surrounding HTML markup (such as the label and value) are not displayed.</span></span>

<span data-ttu-id="45a79-240">下列範例會示範如何使用 HiddenInputAttribute 類別。</span><span class="sxs-lookup"><span data-stu-id="45a79-240">The following example shows how to use the HiddenInputAttribute class.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

<span data-ttu-id="45a79-241">當屬性設定為 true （或未指定任何參數），則發生下列情況：</span><span class="sxs-lookup"><span data-stu-id="45a79-241">When the attribute is set to true (or no parameter is specified), the following occurs:</span></span>

- <span data-ttu-id="45a79-242">顯示範本中轉譯標籤並向使用者顯示的值。</span><span class="sxs-lookup"><span data-stu-id="45a79-242">In display templates, a label is rendered and the value is displayed to the user.</span></span>
- <span data-ttu-id="45a79-243">編輯器範本中轉譯的標籤，並在隱藏的 input 項目中呈現的值。</span><span class="sxs-lookup"><span data-stu-id="45a79-243">In editor templates, a label is rendered and the value is rendered in a hidden input element.</span></span>

<span data-ttu-id="45a79-244">當屬性設定為 false，則發生下列情況：</span><span class="sxs-lookup"><span data-stu-id="45a79-244">When the attribute is set to false, the following occurs:</span></span>

- <span data-ttu-id="45a79-245">顯示範本中不會呈現該欄位。</span><span class="sxs-lookup"><span data-stu-id="45a79-245">In display templates, nothing is rendered for that field.</span></span>
- <span data-ttu-id="45a79-246">編輯器範本中沒有標籤轉譯，而且在隱藏的 input 項目中呈現的值。</span><span class="sxs-lookup"><span data-stu-id="45a79-246">In editor templates, no label is rendered and the value is rendered in a hidden input element.</span></span>

### <a id="_TOC3_14"></a><span data-ttu-id="45a79-247">Html.ValidationSummary Helper 方法可顯示模型層級錯誤</span><span class="sxs-lookup"><span data-stu-id="45a79-247">Html.ValidationSummary Helper Method Can Display Model-Level Errors</span></span>

<span data-ttu-id="45a79-248">而不是永遠顯示的所有驗證錯誤，Html.ValidationSummary helper 方法會具有新的選項，以顯示只有模型層級錯誤。</span><span class="sxs-lookup"><span data-stu-id="45a79-248">Instead of always displaying all validation errors, the Html.ValidationSummary helper method has a new option to display only model-level errors.</span></span> <span data-ttu-id="45a79-249">這可讓模型層級錯誤，驗證摘要中顯示和顯示每個欄位旁邊的特定欄位的錯誤。</span><span class="sxs-lookup"><span data-stu-id="45a79-249">This enables model-level errors to be displayed in the validation summary and field-specific errors to be displayed next to each field.</span></span>

### <a id="_TOC3_15"></a><span data-ttu-id="45a79-250">T4 範本在 Visual Studio 產生程式碼在特定的.NET framework 目標版本</span><span class="sxs-lookup"><span data-stu-id="45a79-250">T4 Templates in Visual Studio Generate Code that is Specific to the Target Version of the .NET Framework</span></span>

<span data-ttu-id="45a79-251">新的屬性是可用的 T4 檔案從指定的應用程式使用.NET Framework 版本的 ASP.NET MVC 的 T4 主機。</span><span class="sxs-lookup"><span data-stu-id="45a79-251">A new property is available to T4 files from the ASP.NET MVC T4 host that specifies the version of the .NET Framework that is used by the application.</span></span> <span data-ttu-id="45a79-252">這可讓 T4 範本產生程式碼與特定的.NET Framework 版本的標記。</span><span class="sxs-lookup"><span data-stu-id="45a79-252">This enables T4 templates to generate code and markup that is specific to a version of the .NET Framework.</span></span> <span data-ttu-id="45a79-253">在 Visual Studio 2008 中，這個值一律是.NET 3.5。</span><span class="sxs-lookup"><span data-stu-id="45a79-253">In Visual Studio 2008, the value is always .NET 3.5.</span></span> <span data-ttu-id="45a79-254">在 Visual Studio 2010 中，值會是.NET 3.5 或.NET 4。</span><span class="sxs-lookup"><span data-stu-id="45a79-254">In Visual Studio 2010, the value is either .NET 3.5 or .NET 4.</span></span>

## <a id="_TOC4"></a><span data-ttu-id="45a79-255">應用程式開發介面改進</span><span class="sxs-lookup"><span data-stu-id="45a79-255">API Improvements</span></span>

<span data-ttu-id="45a79-256">本章節描述對現有的 ASP.NET MVC 型別和成員。</span><span class="sxs-lookup"><span data-stu-id="45a79-256">This section describes changes to existing ASP.NET MVC types and members.</span></span>

- <span data-ttu-id="45a79-257">加入控制器類別中的受保護的虛擬 CreateActionInvoker 方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-257">Added a protected virtual CreateActionInvoker method in the Controller class.</span></span> <span data-ttu-id="45a79-258">這個方法會叫用控制器的 ActionInvoker 屬性，並允許延遲的具現化的啟動程式如果已經不設定了任何啟動程式。</span><span class="sxs-lookup"><span data-stu-id="45a79-258">This method is invoked by the ActionInvoker property of Controller and allows for lazy instantiation of the invoker if no invoker is already set.</span></span>
- <span data-ttu-id="45a79-259">新增受保護的虛擬 HandleUnauthorizedRequest 方法 AuthorizeAttribute 類別中。</span><span class="sxs-lookup"><span data-stu-id="45a79-259">Added a protected virtual HandleUnauthorizedRequest method in the AuthorizeAttribute class.</span></span> <span data-ttu-id="45a79-260">這可讓衍生自 AuthorizeAttribute 授權失敗時，控制行為的篩選器。</span><span class="sxs-lookup"><span data-stu-id="45a79-260">This enables filters that derive from AuthorizeAttribute to control the behavior when authorization fails.</span></span>
- <span data-ttu-id="45a79-261">ValueProviderDictionary 類別中加入一個 Add （字串、 物件索引鍵值） 方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-261">Added an Add(string key, object value) method in the ValueProviderDictionary class.</span></span> <span data-ttu-id="45a79-262">這可讓您使用字典初始設定式語法 ValueProviderDictionary，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="45a79-262">This enables you to use the dictionary initializer syntax for ValueProviderDictionary, as in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- <span data-ttu-id="45a79-263">加入 get\_Sys.Mvc.AjaxContext 類別中的方法的物件。</span><span class="sxs-lookup"><span data-stu-id="45a79-263">Added a get\_object method in the Sys.Mvc.AjaxContext class.</span></span> <span data-ttu-id="45a79-264">這是 JavaScript 方法類似於 get\_取得資料的方式，但如果回應的內容類型是應用程式 /json\_物件傳回的 JSON 物件。</span><span class="sxs-lookup"><span data-stu-id="45a79-264">This is a JavaScript method that is similar to the get\_data method, but if the content type of the response is application/json, get\_object returns the JSON object.</span></span>
- <span data-ttu-id="45a79-265">加入了 ActionDescriptor 屬性 AuthorizationContext 類別中。</span><span class="sxs-lookup"><span data-stu-id="45a79-265">Added an ActionDescriptor property in the AuthorizationContext class.</span></span>
- <span data-ttu-id="45a79-266">加入 UrlParameter.Optional 語彙基元可用來解決問題，到包含 ID 屬性的屬性不存在時的模型繫結時，在表單張貼中。</span><span class="sxs-lookup"><span data-stu-id="45a79-266">Added a UrlParameter.Optional token that can be used to work around problems when binding to a model that contains an ID property when the property is absent in a form post.</span></span> <span data-ttu-id="45a79-267">如需詳細資訊，請參閱文章[ASP.NET MVC 2 選擇性 URL 參數](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx)Phil Haack 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="45a79-267">For more detail, see the entry [ASP.NET MVC 2 Optional URL Parameters](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) on Phil Haack's blog.</span></span>

## <a id="_TOC5"></a><span data-ttu-id="45a79-268">重大變更</span><span class="sxs-lookup"><span data-stu-id="45a79-268">Breaking Changes</span></span>

<span data-ttu-id="45a79-269">下列變更可能會導致現有的 ASP.NET MVC 1.0 應用程式的錯誤。</span><span class="sxs-lookup"><span data-stu-id="45a79-269">The following changes might cause errors in existing ASP.NET MVC 1.0 applications.</span></span>

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a><span data-ttu-id="45a79-270">變更中屬性驗證行為的類別可實作 IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="45a79-270">Change in property validation behavior for classes that implement IDataErrorInfo</span></span>

<span data-ttu-id="45a79-271">對於使用 IDataErrorInfo 來執行驗證的模型物件，會驗證每個屬性，不論是否有設定新的值。</span><span class="sxs-lookup"><span data-stu-id="45a79-271">For model objects that use IDataErrorInfo to perform validation, every property is validated, regardless of whether a new value was set.</span></span> <span data-ttu-id="45a79-272">在 ASP.NET MVC 1.0 中，已驗證已設定的新值的屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-272">In ASP.NET MVC 1.0, only properties that had new values set were validated.</span></span> <span data-ttu-id="45a79-273">在 ASP.NET MVC 2 中，所有的屬性驗證成功時，才呼叫 IDataErrorInfo 的錯誤屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-273">In ASP.NET MVC 2, the Error property of IDataErrorInfo is called only if all the property validators were successful.</span></span>

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a><span data-ttu-id="45a79-274">IIS 指令碼對應指令碼已無法再使用在安裝程式</span><span class="sxs-lookup"><span data-stu-id="45a79-274">IIS script mapping script is no longer available in the installer</span></span>

<span data-ttu-id="45a79-275">IIS 指令碼對應指令碼是用來以傳統模式針對 IIS 6 和 IIS 7 設定指令碼對應的命令列指令碼。</span><span class="sxs-lookup"><span data-stu-id="45a79-275">The IIS script-mapping script is a command-line script that is used to configure script maps for IIS 6 and for IIS 7 in Classic mode.</span></span> <span data-ttu-id="45a79-276">如果您使用 Visual Studio 程式開發伺服器，或如果您使用 IIS 7 以整合模式，則不需要指令碼對應指令碼。</span><span class="sxs-lookup"><span data-stu-id="45a79-276">The script-mapping script is not needed if you use the Visual Studio Development Server or if you use IIS 7 in Integrated mode.</span></span> <span data-ttu-id="45a79-277">上不支援個別下載的指令碼可用[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。</span><span class="sxs-lookup"><span data-stu-id="45a79-277">The scripts are available as a separate unsupported download on the [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).</span></span>

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a><span data-ttu-id="45a79-278">Html.Substitute helper 方法，在 MVC 未來已無法再使用</span><span class="sxs-lookup"><span data-stu-id="45a79-278">The Html.Substitute helper method in MVC Futures is no longer available</span></span>

<span data-ttu-id="45a79-279">因為 MVC 檢視引擎的轉譯行為已變更，Html.Substitute helper 方法會無法運作，而已經移除。</span><span class="sxs-lookup"><span data-stu-id="45a79-279">Due to changes in the rendering behavior of MVC view engines, the Html.Substitute helper method does not work and has been removed.</span></span>

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a><span data-ttu-id="45a79-280">IValueProvider 介面取代 IDictionary 的所有使用</span><span class="sxs-lookup"><span data-stu-id="45a79-280">The IValueProvider interface replaces all uses of IDictionary</span></span>

<span data-ttu-id="45a79-281">現在接受 IDictionary MVC 1.0 中的每個屬性或方法引數會接受 IValueProvider。</span><span class="sxs-lookup"><span data-stu-id="45a79-281">Every property or method argument that accepted IDictionary in MVC 1.0 now accepts IValueProvider.</span></span> <span data-ttu-id="45a79-282">這項變更會影響包含自訂值提供者或自訂的模型繫結器的應用程式。</span><span class="sxs-lookup"><span data-stu-id="45a79-282">This change affects only applications that include custom value providers or custom model binders.</span></span> <span data-ttu-id="45a79-283">會受到這項變更的屬性和方法的範例包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="45a79-283">Examples of properties and methods that are affected by this change include the following:</span></span>

- <span data-ttu-id="45a79-284">ValueProvider ControllerBase 和 ModelBindingContext 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-284">The ValueProvider property of the ControllerBase and ModelBindingContext classes.</span></span>
- <span data-ttu-id="45a79-285">控制器類別 TryUpdateModel 方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-285">The TryUpdateModel methods of the Controller class.</span></span>

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a><span data-ttu-id="45a79-286">Site.css 檔案中已加入新的 CSS 類別</span><span class="sxs-lookup"><span data-stu-id="45a79-286">New CSS classes were added in the Site.css file</span></span>

<span data-ttu-id="45a79-287">Site.css 檔案中的 ASP.NET MVC 專案範本已更新為包含新的樣式和樣板化 helper 的驗證功能所使用。</span><span class="sxs-lookup"><span data-stu-id="45a79-287">The Site.css file in the ASP.NET MVC project templates has been updated to include new styles used by the validation functionality and by the templated helpers.</span></span>

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a><span data-ttu-id="45a79-288">協助程式現在會傳回 MvcHtmlString 物件</span><span class="sxs-lookup"><span data-stu-id="45a79-288">Helpers now return an MvcHtmlString object</span></span>

<span data-ttu-id="45a79-289">若要利用新的 HTML 編碼運算式語法的 ASP.NET 4 中，HTML helper 的傳回型別是現在 MvcHtmlString 而不是字串。</span><span class="sxs-lookup"><span data-stu-id="45a79-289">In order to take advantage of the new HTML-encoding expression syntax in ASP.NET 4, the return type for HTML helpers is now MvcHtmlString instead of a string.</span></span> <span data-ttu-id="45a79-290">如果您使用 ASP.NET MVC 2 和新的協助程式在 ASP.NET 3.5 版時，您將無法利用 HTML 編碼的語法。只有當您在 ASP.NET 4 執行 ASP.NET MVC 2 時，才使用新語法。</span><span class="sxs-lookup"><span data-stu-id="45a79-290">If you use ASP.NET MVC 2 and the new helpers on ASP.NET 3.5, you will not be able to take advantage of the HTML-encoding syntax; the new syntax is available only when you run ASP.NET MVC 2 on ASP.NET 4.</span></span>

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a><span data-ttu-id="45a79-291">JsonResult 現在只回應 HTTP POST 要求</span><span class="sxs-lookup"><span data-stu-id="45a79-291">JsonResult now responds only to HTTP POST requests</span></span>

<span data-ttu-id="45a79-292">若要減輕 JSON 劫持資訊洩露，可能會有預設的攻擊 JsonResult 類別現在只回應 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="45a79-292">In order to mitigate JSON hijacking attacks that have the potential for information disclosure, by default, the JsonResult class now responds only to HTTP POST requests.</span></span> <span data-ttu-id="45a79-293">取得 Ajax 呼叫動作方法會傳回 JsonResult 物件應該變更為改用 POST。</span><span class="sxs-lookup"><span data-stu-id="45a79-293">Ajax GET calls to action methods that return a JsonResult object should be changed to use POST instead.</span></span> <span data-ttu-id="45a79-294">如有必要，您可以設定 JsonResult 新 JsonRequestBehavior 屬性覆寫此行為。</span><span class="sxs-lookup"><span data-stu-id="45a79-294">If necessary, you can override this behavior by setting the new JsonRequestBehavior property of JsonResult.</span></span> <span data-ttu-id="45a79-295">如需可能的攻擊的詳細資訊，請參閱部落格文章[JSON 劫持](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="45a79-295">For more information about the potential exploit, see the blog post [JSON Hijacking](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) on Phil Haack's blog.</span></span>

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a><span data-ttu-id="45a79-296">模型和 ModelType 屬性 setter 上 ModelBindingContext 已過時</span><span class="sxs-lookup"><span data-stu-id="45a79-296">Model and ModelType property setters on ModelBindingContext are obsolete</span></span>

<span data-ttu-id="45a79-297">新的可設定 ModelMetadata 屬性已加入至 ModelBindingContext 類別。</span><span class="sxs-lookup"><span data-stu-id="45a79-297">A new settable ModelMetadata property has been added to the ModelBindingContext class.</span></span> <span data-ttu-id="45a79-298">新的屬性封裝模型和 ModelType 屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-298">The new property encapsulates both the Model and the ModelType properties.</span></span> <span data-ttu-id="45a79-299">雖然模型和 ModelType 屬性已經過時，回溯相容性的屬性 getter 還能運作;這些委派給 ModelMetadata 屬性來擷取的值。</span><span class="sxs-lookup"><span data-stu-id="45a79-299">Although the Model and ModelType properties are obsolete, for backward compatibility the property getters still work; they delegate to the ModelMetadata property to retrieve the value.</span></span>

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a><span data-ttu-id="45a79-300">DefaultControllerFactory 類別的變更會中斷從它衍生的自訂控制器 factory</span><span class="sxs-lookup"><span data-stu-id="45a79-300">Changes to the DefaultControllerFactory class break custom controller factories that derive from it</span></span>

<span data-ttu-id="45a79-301">DefaultControllerFactory 類別已修正藉由移除 RequestContext 屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-301">The DefaultControllerFactory class was fixed by removing the RequestContext property.</span></span> <span data-ttu-id="45a79-302">取代這個屬性，要求內容執行個體傳遞給受保護的虛擬 GetControllerInstance 和 GetControllerType 方法。</span><span class="sxs-lookup"><span data-stu-id="45a79-302">In place of this property, the request context instance is passed to the protected virtual GetControllerInstance and GetControllerType methods.</span></span> <span data-ttu-id="45a79-303">這項變更會影響從 DefaultControllerFactory 衍生的自訂控制器 factory。</span><span class="sxs-lookup"><span data-stu-id="45a79-303">This change affects custom controller factories that derive from DefaultControllerFactory.</span></span>

<span data-ttu-id="45a79-304">自訂的控制器 factory 通常用於提供相依性插入 ASP.NET mvc 應用程式。</span><span class="sxs-lookup"><span data-stu-id="45a79-304">Custom controller factories are often used to provide dependency injection for ASP.NET MVC applications.</span></span> <span data-ttu-id="45a79-305">若要更新的自訂控制器處理站，以支援 ASP.NET MVC 2，變更方法簽章或簽章，以符合新的簽章，並使用要求的內容參數而非屬性。</span><span class="sxs-lookup"><span data-stu-id="45a79-305">To update the custom controller factories to support ASP.NET MVC 2, change the method signature or signatures to match the new signatures, and use the request context parameter instead of the property.</span></span>

#### <a name="area-is-a-now-a-reserved-route-value-key"></a><span data-ttu-id="45a79-306">「 區域 」 現在是保留的路由值的索引鍵</span><span class="sxs-lookup"><span data-stu-id="45a79-306">"Area" is a now a reserved route-value key</span></span>

<span data-ttu-id="45a79-307">字串 「 區域 」 中的路由值現在會有在 ASP.NET MVC 中，特殊的意義相同的方式執行 「 控制器 」 和 「 動作 」。</span><span class="sxs-lookup"><span data-stu-id="45a79-307">The string "area" in Route values now has special meaning in ASP.NET MVC, in the same way that "controller" and "action" do.</span></span> <span data-ttu-id="45a79-308">一個含意為，如果路由值字典，其中包含 「 區域 」 提供 HTML helper，協助程式將不會再附加 「 區域 」 查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="45a79-308">One implication is that if HTML helpers are supplied with a route-value dictionary containing "area", the helpers will no longer append "area" in the query string.</span></span>

<span data-ttu-id="45a79-309">如果您使用 「 區域 」 功能，請務必使用 {區域} 做為路由 URL 的一部分。</span><span class="sxs-lookup"><span data-stu-id="45a79-309">If you are using the Areas feature, make sure to not use {area} as part of your route URL.</span></span>


## <a id="_TOC6"></a>  <span data-ttu-id="45a79-310">Disclaimer</span><span class="sxs-lookup"><span data-stu-id="45a79-310">Disclaimer</span></span>

<span data-ttu-id="45a79-311">這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。</span><span class="sxs-lookup"><span data-stu-id="45a79-311">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="45a79-312">本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。</span><span class="sxs-lookup"><span data-stu-id="45a79-312">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="45a79-313">Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。</span><span class="sxs-lookup"><span data-stu-id="45a79-313">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="45a79-314">本技術白皮書僅供參考。</span><span class="sxs-lookup"><span data-stu-id="45a79-314">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="45a79-315">MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。</span><span class="sxs-lookup"><span data-stu-id="45a79-315">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="45a79-316">承諾遵守所有適用的著作權法是使用者的責任。</span><span class="sxs-lookup"><span data-stu-id="45a79-316">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="45a79-317">著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。</span><span class="sxs-lookup"><span data-stu-id="45a79-317">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="45a79-318">本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。</span><span class="sxs-lookup"><span data-stu-id="45a79-318">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="45a79-319">除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。</span><span class="sxs-lookup"><span data-stu-id="45a79-319">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="45a79-320">除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人員、 地點及事件屬虛構，以及與任何真實的公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件純屬巧合。</span><span class="sxs-lookup"><span data-stu-id="45a79-320">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="45a79-321">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="45a79-321">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="45a79-322">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="45a79-322">All rights reserved.</span></span>

<span data-ttu-id="45a79-323">Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。</span><span class="sxs-lookup"><span data-stu-id="45a79-323">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="45a79-324">本文件中所提實際公司和產品，可能為各所有人所有之商標。</span><span class="sxs-lookup"><span data-stu-id="45a79-324">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
