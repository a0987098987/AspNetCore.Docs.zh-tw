---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: 什麼是 ASP.NET MVC 2 的新功能 |Microsoft Docs
author: rick-anderson
description: 本文件說明新功能和 ASP.NET MVC 2 中引進的增強功能。 這份文件也可供下載。
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 82a3fd4fe74202ed9a23298390322458cfc029f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831807"
---
<a name="whats-new-in-aspnet-mvc-2"></a><span data-ttu-id="04636-104">在 ASP.NET MVC 2 中最新消息</span><span class="sxs-lookup"><span data-stu-id="04636-104">What's New in ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="04636-105">本文件說明新功能和 ASP.NET MVC 2 中引進的增強功能。</span><span class="sxs-lookup"><span data-stu-id="04636-105">This document describes new features and improvements introduced in ASP.NET MVC 2.</span></span> <span data-ttu-id="04636-106">這份文件也已開放[下載](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)</span><span class="sxs-lookup"><span data-stu-id="04636-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)</span></span>


<span data-ttu-id="04636-107">[簡介](#_TOC1) </span><span class="sxs-lookup"><span data-stu-id="04636-107">[Introduction](#_TOC1) </span></span>  
<span data-ttu-id="04636-108">[將 ASP.NET MVC 1.0 專案升級至 ASP.NET MVC 2](#_TOC2) </span><span class="sxs-lookup"><span data-stu-id="04636-108">[Upgrading an ASP.NET MVC 1.0 Project to ASP.NET MVC 2](#_TOC2) </span></span>  
<span data-ttu-id="04636-109">[新功能](#_TOC3) </span><span class="sxs-lookup"><span data-stu-id="04636-109">[New Features](#_TOC3) </span></span>  
<span data-ttu-id="04636-110">[樣板化 Helper](#_TOC3_1) </span><span class="sxs-lookup"><span data-stu-id="04636-110">[Templated Helpers](#_TOC3_1) </span></span>  
<span data-ttu-id="04636-111">[區域](#_TOC3_2) </span><span class="sxs-lookup"><span data-stu-id="04636-111">[Areas](#_TOC3_2) </span></span>  
<span data-ttu-id="04636-112">[非同步控制器的的支援](#_TOC3_3) </span><span class="sxs-lookup"><span data-stu-id="04636-112">[Support for Asynchronous Controllers](#_TOC3_3) </span></span>  
<span data-ttu-id="04636-113">[DefaultValueAttribute 在動作方法參數的支援](#_TOC3_4) </span><span class="sxs-lookup"><span data-stu-id="04636-113">[Support for DefaultValueAttribute in Action-Method Parameters](#_TOC3_4) </span></span>  
<span data-ttu-id="04636-114">[繫結的二進位資料，模型繫結器的支援](#_TOC3_5) </span><span class="sxs-lookup"><span data-stu-id="04636-114">[Support for Binding Binary Data with Model Binders](#_TOC3_5) </span></span>  
<span data-ttu-id="04636-115">[ModelMetadata 和 ModelMetadataProvider 類別](#_TOC3_6) </span><span class="sxs-lookup"><span data-stu-id="04636-115">[ModelMetadata and ModelMetadataProvider Classes](#_TOC3_6) </span></span>  
<span data-ttu-id="04636-116">[DataAnnotations 屬性的支援](#_TOC3_7) </span><span class="sxs-lookup"><span data-stu-id="04636-116">[Support for DataAnnotations Attributes](#_TOC3_7) </span></span>  
<span data-ttu-id="04636-117">[模型驗證程式提供者](#_TOC3_8) </span><span class="sxs-lookup"><span data-stu-id="04636-117">[Model-Validator Providers](#_TOC3_8) </span></span>  
<span data-ttu-id="04636-118">[用戶端驗證](#_TOC3_9) </span><span class="sxs-lookup"><span data-stu-id="04636-118">[Client-Side Validation](#_TOC3_9) </span></span>  
<span data-ttu-id="04636-119">[適用於 Visual Studio 2010 的新程式碼片段](#_TOC3_10) </span><span class="sxs-lookup"><span data-stu-id="04636-119">[New Code Snippets for Visual Studio 2010](#_TOC3_10) </span></span>  
<span data-ttu-id="04636-120">[新的 RequireHttpsAttribute 動作篩選條件](#_TOC3_11) </span><span class="sxs-lookup"><span data-stu-id="04636-120">[New RequireHttpsAttribute Action Filter](#_TOC3_11) </span></span>  
<span data-ttu-id="04636-121">[覆寫的 HTTP 方法的動詞命令](#_TOC3_12) </span><span class="sxs-lookup"><span data-stu-id="04636-121">[Overriding the HTTP Method Verb](#_TOC3_12) </span></span>  
<span data-ttu-id="04636-122">[樣板化 Helper 的新 HiddenInputAttribute 類別](#_TOC3_13) </span><span class="sxs-lookup"><span data-stu-id="04636-122">[New HiddenInputAttribute Class for Templated Helpers](#_TOC3_13) </span></span>  
<span data-ttu-id="04636-123">[Html.ValidationSummary 協助程式方法可以顯示模型層級錯誤](#_TOC3_14) </span><span class="sxs-lookup"><span data-stu-id="04636-123">[Html.ValidationSummary Helper Method Can Display Model-Level Errors](#_TOC3_14) </span></span>  
<span data-ttu-id="04636-124">[在 Visual Studio 產生程式碼特定的 T4 範本至.NET framework 目標版本](#_TOC3_15)[API 增強功能](#_TOC4)</span><span class="sxs-lookup"><span data-stu-id="04636-124">[T4 Templates in Visual Studio Generate Code that is Specific to the Target Version of the .NET Framework](#_TOC3_15)[API Improvements](#_TOC4)</span></span>  
[<span data-ttu-id="04636-125">重大變更</span><span class="sxs-lookup"><span data-stu-id="04636-125">Breaking Changes</span></span>](#_TOC5)  
[<span data-ttu-id="04636-126">免責聲明</span><span class="sxs-lookup"><span data-stu-id="04636-126">Disclaimer</span></span>](#_TOC6)  

## <a id="_TOC1"></a>  <span data-ttu-id="04636-127">簡介</span><span class="sxs-lookup"><span data-stu-id="04636-127">Introduction</span></span>

<span data-ttu-id="04636-128">ASP.NET MVC 2 是根據 ASP.NET MVC 1.0，並引進大量的增強功能和功能，著重於提升產能。</span><span class="sxs-lookup"><span data-stu-id="04636-128">ASP.NET MVC 2 builds on ASP.NET MVC 1.0 and introduces a large set of enhancements and features that are focused on increasing productivity.</span></span> <span data-ttu-id="04636-129">此版本是與 ASP.NET MVC 1.0 中，相容的因此所有您知識、 技能、 程式碼和適用於 ASP.NET MVC 1.0 仍會繼續套用。</span><span class="sxs-lookup"><span data-stu-id="04636-129">This release is compatible with ASP.NET MVC 1.0, so all your knowledge, skills, code, and extensions for ASP.NET MVC 1.0 continue to apply.</span></span>

<span data-ttu-id="04636-130">如需有關 ASP.NET MVC 的詳細資訊，請瀏覽下列資源：</span><span class="sxs-lookup"><span data-stu-id="04636-130">For more information about ASP.NET MVC, visit the following resources:</span></span>

- [<span data-ttu-id="04636-131">MSDN 上的 ASP.NET MVC 文件</span><span class="sxs-lookup"><span data-stu-id="04636-131">ASP.NET MVC documentation on MSDN</span></span>](https://go.microsoft.com/fwlink/?LinkId=159758)
- [<span data-ttu-id="04636-132">ASP.NET MVC 網站</span><span class="sxs-lookup"><span data-stu-id="04636-132">The ASP.NET MVC Web site</span></span>](https://asp.net/mvc/)
- [<span data-ttu-id="04636-133">ASP.NET MVC 論壇</span><span class="sxs-lookup"><span data-stu-id="04636-133">The ASP.NET MVC forums</span></span>](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  <span data-ttu-id="04636-134">將 ASP.NET MVC 1.0 專案升級至 ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="04636-134">Upgrading an ASP.NET MVC 1.0 Project to ASP.NET MVC 2</span></span>

<span data-ttu-id="04636-135">ASP.NET MVC 2 可以與 ASP.NET MVC 1.0 並存安裝在相同伺服器上，讓應用程式開發人員彈性來選擇升級至 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式的時機。</span><span class="sxs-lookup"><span data-stu-id="04636-135">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server, which gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span> <span data-ttu-id="04636-136">如需如何升級的詳細資訊，請參閱文件[ASP.NET MVC 1.0 應用程式升級為 ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459)。</span><span class="sxs-lookup"><span data-stu-id="04636-136">For information on how to upgrade, see the document [Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).</span></span>

## <a id="_TOC3"></a>  <span data-ttu-id="04636-137">新功能</span><span class="sxs-lookup"><span data-stu-id="04636-137">New Features</span></span>

<span data-ttu-id="04636-138">本節說明已引進的功能中的 MVC 2 版。</span><span class="sxs-lookup"><span data-stu-id="04636-138">This section describes features that have been introduced in the MVC 2 release.</span></span>

### <a id="_TOC3_1"></a>  <span data-ttu-id="04636-139">樣板化 Helper</span><span class="sxs-lookup"><span data-stu-id="04636-139">Templated Helpers</span></span>

<span data-ttu-id="04636-140">樣板化 helper 可讓您自動產生關聯的 HTML 項目進行編輯，並顯示與資料型別。</span><span class="sxs-lookup"><span data-stu-id="04636-140">Templated helpers let you automatically associate HTML elements for edit and display with data types.</span></span> <span data-ttu-id="04636-141">例如，當資料類型為 System.DateTime 會顯示在檢視中，日期選擇器 UI 項目可以自動轉譯。</span><span class="sxs-lookup"><span data-stu-id="04636-141">For example, when data of type System.DateTime is displayed in a view, a date-picker UI element can be automatically rendered.</span></span> <span data-ttu-id="04636-142">這是在 ASP.NET Dynamic Data 欄位範本的運作方式類似。</span><span class="sxs-lookup"><span data-stu-id="04636-142">This is similar to how field templates work in ASP.NET Dynamic Data.</span></span> <span data-ttu-id="04636-143">如需詳細資訊，請參閱 <<c0> [ 來顯示資料使用樣板化 Helper](https://go.microsoft.com/fwlink/?LinkId=159062) MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="04636-143">For more information, see [Using Templated Helpers to Display Data](https://go.microsoft.com/fwlink/?LinkId=159062) on the MSDN Web site.</span></span>

### <a id="_TOC3_2"></a>  <span data-ttu-id="04636-144">區域</span><span class="sxs-lookup"><span data-stu-id="04636-144">Areas</span></span>

<span data-ttu-id="04636-145">區域可讓您大型專案分成多個較小的區段，以管理大型的 Web 應用程式的複雜度。</span><span class="sxs-lookup"><span data-stu-id="04636-145">Areas let you organize a large project into multiple smaller sections in order to manage the complexity of a large Web application.</span></span> <span data-ttu-id="04636-146">每個區段 （「 區域 」） 通常代表大網站的個別區段，並可用來分組相關的控制器和檢視集。</span><span class="sxs-lookup"><span data-stu-id="04636-146">Each section ("area") typically represents a separate section of a large Web site and is used to group related sets of controllers and views.</span></span> <span data-ttu-id="04636-147">如需詳細資訊，請參閱 <<c0> [ 逐步解說： 組織 ASP.NET MVC 應用程式透過區域](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="04636-147">For more information, see [Walkthrough: Organizing an ASP.NET MVC Application by Areas](https://go.microsoft.com/fwlink/?LinkId=158978) on the MSDN Web site.</span></span>

<span data-ttu-id="04636-148">若要建立新的區域，請在 [方案總管] 中，以滑鼠右鍵按一下專案，按一下 [新增]，然後按一下區域。</span><span class="sxs-lookup"><span data-stu-id="04636-148">To create a new area, in Solution Explorer, right-click the project, click Add, and then click Area.</span></span> <span data-ttu-id="04636-149">這會顯示對話方塊，提示您輸入的區域名稱。</span><span class="sxs-lookup"><span data-stu-id="04636-149">This displays a dialog box that prompts you for the area name.</span></span> <span data-ttu-id="04636-150">輸入區域名稱之後，Visual Studio 會將新的區域加入專案中。</span><span class="sxs-lookup"><span data-stu-id="04636-150">After you enter the area name, Visual Studio adds a new area to the project.</span></span>

<span data-ttu-id="04636-151">下圖顯示具有兩個區域，系統管理員和部落格專案配置範例。</span><span class="sxs-lookup"><span data-stu-id="04636-151">The following figure shows an example layout for a project with two areas, Admin and Blogs.</span></span>

![](what-is-new-in-aspnet-mvc/_static/image1.png)

<span data-ttu-id="04636-152">當您建立區域時，Visual Studio 會新增至每個區域從 AreaRegistration 衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="04636-152">When you create an area, Visual Studio adds a class that derives from AreaRegistration to each area.</span></span> <span data-ttu-id="04636-153">這個類別，才能註冊區域和其路由中，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04636-153">This class is required in order to register the area and its routes, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

<span data-ttu-id="04636-154">ASP.NET MVC 2 的預設專案範本在 Global.asax 檔案的程式碼中包含 RegisterAllAreas 方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="04636-154">The default project template for ASP.NET MVC 2 includes a call to the RegisterAllAreas method in the code for the Global.asax file.</span></span> <span data-ttu-id="04636-155">藉由尋找衍生自 AreaRegistration 類別的所有類型的具現化型別的執行個體，然後呼叫執行個體上的 RegisterArea 方法，這個方法會註冊每個區域中的專案。</span><span class="sxs-lookup"><span data-stu-id="04636-155">This method registers each area in the project by looking for all types that derive from the AreaRegistration class, instantiating an instance of the type, and then calling the RegisterArea method on the instance.</span></span> <span data-ttu-id="04636-156">下列範例會示範如何做到這點。</span><span class="sxs-lookup"><span data-stu-id="04636-156">The following example shows how this is done.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

<span data-ttu-id="04636-157">如果您未藉由呼叫內容 RegisterArea 方法中指定的命名空間。預設會使用 Namespaces.Add 方法，註冊類別的命名空間。</span><span class="sxs-lookup"><span data-stu-id="04636-157">If you do not specify the namespace in the RegisterArea method by calling the context.Namespaces.Add method, the namespace of the registration class is used by default.</span></span>

### <a id="_TOC3_3"></a>  <span data-ttu-id="04636-158">非同步控制器的的支援</span><span class="sxs-lookup"><span data-stu-id="04636-158">Support for Asynchronous Controllers</span></span>

<span data-ttu-id="04636-159">ASP.NET MVC 2 現在可讓控制器以非同步方式處理要求。</span><span class="sxs-lookup"><span data-stu-id="04636-159">ASP.NET MVC 2 now allows controllers to process requests asynchronously.</span></span> <span data-ttu-id="04636-160">藉由使用伺服器會經常要改為呼叫非封鎖式的對應項目呼叫 （例如網路要求） 的封鎖作業，這可能會導致效能提升。</span><span class="sxs-lookup"><span data-stu-id="04636-160">This can lead to performance gains by allowing servers which frequently call blocking operations (like network requests) to call non-blocking counterparts instead.</span></span> <span data-ttu-id="04636-161">如需詳細資訊，請參閱 <<c0> [ 在 ASP.NET MVC 中使用非同步控制器](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)MSDN 上的主題。</span><span class="sxs-lookup"><span data-stu-id="04636-161">For more information, see the [Using an Asynchronous Controller in ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) topic on MSDN.</span></span>

### <a id="_TOC3_4"></a>  <span data-ttu-id="04636-162">DefaultValueAttribute 在動作方法參數的支援</span><span class="sxs-lookup"><span data-stu-id="04636-162">Support for DefaultValueAttribute in Action-Method Parameters</span></span>

<span data-ttu-id="04636-163">System.ComponentModel.DefaultValueAttribute 類別可讓動作方法的引數參數所提供的預設值。</span><span class="sxs-lookup"><span data-stu-id="04636-163">The System.ComponentModel.DefaultValueAttribute class allows a default value to be supplied for the argument parameter to an action method.</span></span> <span data-ttu-id="04636-164">例如，假設下列的預設路由，定義：</span><span class="sxs-lookup"><span data-stu-id="04636-164">For example, assume that the following default route is defined:</span></span>

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

<span data-ttu-id="04636-165">也會假設下列控制器和動作方法的定義：</span><span class="sxs-lookup"><span data-stu-id="04636-165">Also assume that the following controller and action method is defined:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

<span data-ttu-id="04636-166">下列要求的任何 Url 將會叫用上述範例中所定義的檢視動作方法。</span><span class="sxs-lookup"><span data-stu-id="04636-166">Any of the following request URLs will invoke the View action method that is defined in the preceding example.</span></span>

- <span data-ttu-id="04636-167">/ 文件/檢視/123</span><span class="sxs-lookup"><span data-stu-id="04636-167">/Article/View/123</span></span>
- <span data-ttu-id="04636-168">/ 文件/檢視/123？ 頁面 = 1 （效力等同於先前的要求）</span><span class="sxs-lookup"><span data-stu-id="04636-168">/Article/View/123?page=1 (Effectively the same as the previous request)</span></span>
- <span data-ttu-id="04636-169">/ 文件/檢視/123？ 頁面 = 2</span><span class="sxs-lookup"><span data-stu-id="04636-169">/Article/View/123?page=2</span></span>

<span data-ttu-id="04636-170">如果沒有 DefaultValueAttribute 屬性中，從上述清單的第一個 URL 不是可行，因為頁面引數是尚未提供其值不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="04636-170">Without the DefaultValueAttribute attribute, the first URL from the preceding list would not work, because the page argument is a non-nullable value type whose value has not been provided.</span></span>

<span data-ttu-id="04636-171">如果您的程式碼以 Visual Basic 2010 或 Visual C# 2010年撰寫，您可以使用選擇性參數，而不是 DefaultValueAttribute 屬性，如下列範例所示即可：</span><span class="sxs-lookup"><span data-stu-id="04636-171">If your code is written in Visual Basic 2010 or Visual C# 2010, you can use optional parameters instead of the DefaultValueAttribute attribute, as shown in the following example:</span></span>

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  <span data-ttu-id="04636-172">繫結的二進位資料，模型繫結器的支援</span><span class="sxs-lookup"><span data-stu-id="04636-172">Support for Binding Binary Data with Model Binders</span></span>

<span data-ttu-id="04636-173">有兩個新的多載 Html.Hidden helper，編碼為 base-64 編碼字串的二進位值：</span><span class="sxs-lookup"><span data-stu-id="04636-173">There are two new overloads of the Html.Hidden helper that encode binary values as base-64-encoded strings:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

<span data-ttu-id="04636-174">典型的用法是內嵌在檢視中的物件時的時間戳記。</span><span class="sxs-lookup"><span data-stu-id="04636-174">A typical use is to embed a timestamp for an object in the view.</span></span> <span data-ttu-id="04636-175">比方說，您的應用程式可能會包含下列產品物件：</span><span class="sxs-lookup"><span data-stu-id="04636-175">For example, your application might include the following Product object:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

<span data-ttu-id="04636-176">編輯表單可以轉譯時間戳記屬性，格式如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04636-176">An edit form can render the TimeStamp property in the form as shown in the following example:</span></span>

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

<span data-ttu-id="04636-177">此標記會隱藏的 input 的元素，以時間戳記值轉譯成 base-64 編碼的字串，類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="04636-177">This markup renders a hidden input element with the timestamp value as a base-64-encoded string that resembles the following example:</span></span>

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

<span data-ttu-id="04636-178">此表單可能會張貼至動作方法具有類型產品的引數，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04636-178">This form might be posted to an action method that has an argument of type Product, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

<span data-ttu-id="04636-179">在動作方法中，因為已張貼的 base-64 編碼字串轉換成位元組陣列，已正確地填入時間戳記屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-179">In the action method, the TimeStamp property is populated correctly because the posted base-64-encoded string is converted to a byte array.</span></span>

### <a id="_TOC3_6"></a>  <span data-ttu-id="04636-180">ModelMetadata 和 ModelMetadataProvider 類別</span><span class="sxs-lookup"><span data-stu-id="04636-180">ModelMetadata and ModelMetadataProvider Classes</span></span>

<span data-ttu-id="04636-181">ModelMetadataProvider 類別提供取得檢視內之資料模型的中繼資料的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="04636-181">The ModelMetadataProvider class provides an abstraction for obtaining metadata for the model within a view.</span></span> <span data-ttu-id="04636-182">MVC 2 包含預設的提供者，可提供 System.ComponentModel.DataAnnotations 命名空間中的屬性所公開的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="04636-182">MVC 2 includes a default provider that makes available the metadata that is exposed by the attributes in the System.ComponentModel.DataAnnotations namespace.</span></span> <span data-ttu-id="04636-183">可以建立提供從其他資料存放區，例如資料庫或 XML 檔案的中繼資料的中繼資料提供者。</span><span class="sxs-lookup"><span data-stu-id="04636-183">It is possible to create metadata providers that provide metadata from other data stores, such as databases or XML files.</span></span>

<span data-ttu-id="04636-184">ViewDataDictionary 類別會公開包含由 ModelMetadataProvider 類別從模型擷取中繼資料的 ModelMetadata 物件。</span><span class="sxs-lookup"><span data-stu-id="04636-184">The ViewDataDictionary class exposes a ModelMetadata object that contains the metadata that is extracted from the model by the ModelMetadataProvider class.</span></span> <span data-ttu-id="04636-185">這可讓使用此中繼資料，並據以調整其輸出的樣板化 helper。</span><span class="sxs-lookup"><span data-stu-id="04636-185">This enables the templated helpers to consume this metadata and adjust their output accordingly.</span></span>

<span data-ttu-id="04636-186">如需詳細資訊，請參閱文件[ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)並[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="04636-186">For more information, see the documentation for the [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) and [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) classes.</span></span>

### <a id="_TOC3_7"></a>  <span data-ttu-id="04636-187">DataAnnotations 屬性的支援</span><span class="sxs-lookup"><span data-stu-id="04636-187">Support for DataAnnotations Attributes</span></span>

<span data-ttu-id="04636-188">ASP.NET MVC 2 支援使用 RangeAttribute、 出現 RequiredAttribute、 StringLengthAttribute 和 RegexAttribute 驗證屬性 （定義在 System.ComponentModel.DataAnnotations 命名空間） 當您繫結至模型以提供輸入驗證。</span><span class="sxs-lookup"><span data-stu-id="04636-188">ASP.NET MVC 2 supports using the RangeAttribute, RequiredAttribute, StringLengthAttribute, and RegexAttribute validation attributes (defined in the System.ComponentModel.DataAnnotations namespace) when you bind to a model in order to provide input validation.</span></span>

<span data-ttu-id="04636-189">如需詳細資訊，請參閱 <<c0> [ 如何： 驗證模型的資料使用 DataAnnotations 屬性](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="04636-189">For more information, see [How to: Validate Model Data Using DataAnnotations Attributes](https://go.microsoft.com/fwlink/?LinkId=159063) on the MSDN Web site.</span></span> <span data-ttu-id="04636-190">說明如何使用這些屬性的範例專案是下載[ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753)。</span><span class="sxs-lookup"><span data-stu-id="04636-190">A sample project that illustrates the use of these attributes is available for download at [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).</span></span>

### <a id="_TOC3_8"></a>  <span data-ttu-id="04636-191">模型驗證程式提供者</span><span class="sxs-lookup"><span data-stu-id="04636-191">Model-Validator Providers</span></span>

<span data-ttu-id="04636-192">模型驗證提供者類別表示提供模型的驗證邏輯的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="04636-192">The model-validation provider class represents an abstraction that provides validation logic for the model.</span></span> <span data-ttu-id="04636-193">ASP.NET MVC 包含根據 System.ComponentModel.DataAnnotations 命名空間中包含的驗證屬性的預設提供者。</span><span class="sxs-lookup"><span data-stu-id="04636-193">ASP.NET MVC includes a default provider based on validation attributes that are included in the System.ComponentModel.DataAnnotations namespace.</span></span> <span data-ttu-id="04636-194">您也可以建立自己的驗證提供者模型中定義自訂驗證規則和自訂驗證規則的對應。</span><span class="sxs-lookup"><span data-stu-id="04636-194">You can also create your own validation providers that define custom validation rules and custom mappings of validation rules to the model.</span></span> <span data-ttu-id="04636-195">如需詳細資訊，請參閱文件[ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="04636-195">For more information, see the documentation for the [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) class.</span></span>

### <a id="_TOC3_9"></a>  <span data-ttu-id="04636-196">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="04636-196">Client-Side Validation</span></span>

<span data-ttu-id="04636-197">模型驗證程式提供者類別會公開可供用戶端驗證程式庫的 JSON 序列化資料的形式中的瀏覽器來驗證中繼資料。</span><span class="sxs-lookup"><span data-stu-id="04636-197">The model-validator provider class exposes validation metadata to the browser in the form of JSON-serialized data that can be consumed by a client-side validation library.</span></span> <span data-ttu-id="04636-198">ASP.NET MVC 2 包含用戶端驗證程式庫和支援先前所述的 DataAnnotations 命名空間驗證屬性的配接器。</span><span class="sxs-lookup"><span data-stu-id="04636-198">ASP.NET MVC 2 includes a client validation library and adapter that supports the DataAnnotations namespace validation attributes noted earlier.</span></span> <span data-ttu-id="04636-199">提供者類別也可讓您使用其他用戶端驗證程式庫所撰寫的配接器會在處理 JSON 資料和呼叫其他程式庫。</span><span class="sxs-lookup"><span data-stu-id="04636-199">The provider class also enables you to use other client-validation libraries by writing an adapter that processes the JSON data and calls into the alternate library.</span></span>

### <a id="_TOC3_10"></a>  <span data-ttu-id="04636-200">適用於 Visual Studio 2010 的新程式碼片段</span><span class="sxs-lookup"><span data-stu-id="04636-200">New Code Snippets for Visual Studio 2010</span></span>

<span data-ttu-id="04636-201">使用 Visual Studio 2010 會安裝 ASP.NET MVC 2 的 HTML 程式碼片段的一組。</span><span class="sxs-lookup"><span data-stu-id="04636-201">A set of HTML code snippets for ASP.NET MVC 2 is installed with Visual Studio 2010.</span></span> <span data-ttu-id="04636-202">若要檢視清單中的這些程式碼片段，在 [工具] 功能表中，選取 [程式碼的程式碼片段管理員]。</span><span class="sxs-lookup"><span data-stu-id="04636-202">To view a list of these snippets, in the Tools menu, select Code Snippets Manager.</span></span> <span data-ttu-id="04636-203">針對語言，選取 HTML，並針對位置，選取 ASP.NET MVC 2。</span><span class="sxs-lookup"><span data-stu-id="04636-203">For the language, select HTML, and for location, select ASP.NET MVC 2.</span></span> <span data-ttu-id="04636-204">如需如何使用程式碼片段的詳細資訊，請參閱 Visual Studio 文件。</span><span class="sxs-lookup"><span data-stu-id="04636-204">For more information about how to use code snippets, see the Visual Studio documentation.</span></span>

### <a id="_TOC3_11"></a>  <span data-ttu-id="04636-205">新的 RequireHttpsAttribute 動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="04636-205">New RequireHttpsAttribute Action Filter</span></span>

<span data-ttu-id="04636-206">ASP.NET MVC 2 包含新的 RequireHttpsAttribute 類別，可以套用至動作方法和控制器。</span><span class="sxs-lookup"><span data-stu-id="04636-206">ASP.NET MVC 2 includes a new RequireHttpsAttribute class that can be applied to action methods and controllers.</span></span> <span data-ttu-id="04636-207">根據預設，篩選條件會將非 SSL HTTP 要求重新導向啟用 SSL (HTTPS) 對等項目。</span><span class="sxs-lookup"><span data-stu-id="04636-207">By default, the filter redirects a non-SSL (HTTP) request to the SSL-enabled (HTTPS) equivalent.</span></span>

### <a id="_TOC3_12"></a>  <span data-ttu-id="04636-208">覆寫的 HTTP 方法的動詞命令</span><span class="sxs-lookup"><span data-stu-id="04636-208">Overriding the HTTP Method Verb</span></span>

<span data-ttu-id="04636-209">您可以使用 REST 架構樣式，以建置網站，當 HTTP 動詞命令用來判斷要針對資源執行哪些動作。</span><span class="sxs-lookup"><span data-stu-id="04636-209">When you build a Web site by using the REST architectural style, HTTP verbs are used to determine which action to perform for a resource.</span></span> <span data-ttu-id="04636-210">REST 要求該應用程式支援各種常見的 HTTP 動詞命令，其中包括 GET、 PUT、 POST 和 DELETE。</span><span class="sxs-lookup"><span data-stu-id="04636-210">REST requires that applications support the full range of common HTTP verbs, including GET, PUT, POST, and DELETE.</span></span>

<span data-ttu-id="04636-211">ASP.NET MVC 2 包含新的屬性，您可以套用至動作方法和該功能的精簡語法。</span><span class="sxs-lookup"><span data-stu-id="04636-211">ASP.NET MVC 2 includes new attributes that you can apply to action methods and that feature compact syntax.</span></span> <span data-ttu-id="04636-212">這些屬性可讓 ASP.NET MVC 到選取的 HTTP 動詞命令為基礎的動作方法。</span><span class="sxs-lookup"><span data-stu-id="04636-212">These attributes enable ASP.NET MVC to select an action method based on the HTTP verb.</span></span> <span data-ttu-id="04636-213">在下列範例中，POST 要求會呼叫第一個動作方法，PUT 要求會呼叫第二個動作方法。</span><span class="sxs-lookup"><span data-stu-id="04636-213">In the following example, a POST request will call the first action method and a PUT request will call the second action method.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

<span data-ttu-id="04636-214">在舊版的 ASP.NET MVC 中，這些動作方法所需更詳細的語法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04636-214">In earlier versions of ASP.NET MVC, these action methods required more verbose syntax, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

<span data-ttu-id="04636-215">瀏覽器都支援只有 GET 和 POST HTTP 動詞命令，因為它不可能要張貼到需要不同的動詞命令的動作。</span><span class="sxs-lookup"><span data-stu-id="04636-215">Because browsers support only the GET and POST HTTP verbs, it is not possible to post to an action that requires a different verb.</span></span> <span data-ttu-id="04636-216">因此它是不可能原本就支援符合 rest 限制的所有要求。</span><span class="sxs-lookup"><span data-stu-id="04636-216">Thus it is not possible to natively support all RESTful requests.</span></span>

<span data-ttu-id="04636-217">不過，支援的 RESTful POST 作業期間，ASP.NET MVC 2 的要求會引進一個新的 HttpMethodOverride HTML 協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="04636-217">However, to support RESTful requests during POST operations, ASP.NET MVC 2 introduces a new HttpMethodOverride HTML helper method.</span></span> <span data-ttu-id="04636-218">這個方法會呈現隱藏的 input 項目，會導致表單有效地模擬任何 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="04636-218">This method renders a hidden input element that causes the form to effectively emulate any HTTP method.</span></span> <span data-ttu-id="04636-219">例如，使用 HttpMethodOverride HTML 協助程式方法，就可以出現的表單提交是 PUT 或 DELETE 要求。</span><span class="sxs-lookup"><span data-stu-id="04636-219">For example, by using the HttpMethodOverride HTML helper method, you can have a form submission appear be a PUT or DELETE request.</span></span> <span data-ttu-id="04636-220">HttpMethodOverride 的行為會影響下列屬性：</span><span class="sxs-lookup"><span data-stu-id="04636-220">The behavior of HttpMethodOverride affects the following attributes:</span></span>

- <span data-ttu-id="04636-221">HttpPostAttribute</span><span class="sxs-lookup"><span data-stu-id="04636-221">HttpPostAttribute</span></span>
- <span data-ttu-id="04636-222">HttpPutAttribute</span><span class="sxs-lookup"><span data-stu-id="04636-222">HttpPutAttribute</span></span>
- <span data-ttu-id="04636-223">HttpGetAttribute</span><span class="sxs-lookup"><span data-stu-id="04636-223">HttpGetAttribute</span></span>
- <span data-ttu-id="04636-224">HttpDeleteAttribute</span><span class="sxs-lookup"><span data-stu-id="04636-224">HttpDeleteAttribute</span></span>
- <span data-ttu-id="04636-225">AcceptVerbsAttribute</span><span class="sxs-lookup"><span data-stu-id="04636-225">AcceptVerbsAttribute</span></span>

<span data-ttu-id="04636-226">隱藏的 input 項目有 X-HTTP-方法-覆寫其名稱和其值設為 HTTP 指令動詞，來模擬。</span><span class="sxs-lookup"><span data-stu-id="04636-226">The hidden input element has its name X-HTTP-Method-Override and its value set to the HTTP verb to emulate.</span></span> <span data-ttu-id="04636-227">覆寫值也可以指定在 HTTP 標頭或查詢字串值做為名稱/值配對。</span><span class="sxs-lookup"><span data-stu-id="04636-227">The override value can also be specified in an HTTP header or in a query string value as a name/value pair.</span></span>

<span data-ttu-id="04636-228">覆寫僅適用於當實際的要求是 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="04636-228">The override can only be used when the real request is a POST request.</span></span> <span data-ttu-id="04636-229">使用任何其他 HTTP 動詞命令的要求，將會忽略覆寫值。</span><span class="sxs-lookup"><span data-stu-id="04636-229">The override value will be ignored for requests that use any other HTTP verb.</span></span>

### <a id="_TOC3_13"></a>  <span data-ttu-id="04636-230">樣板化 Helper 的新 HiddenInputAttribute 類別</span><span class="sxs-lookup"><span data-stu-id="04636-230">New HiddenInputAttribute Class for Templated Helpers</span></span>

<span data-ttu-id="04636-231">您可以將新的 HiddenInputAttribute 屬性套用至模型屬性，表示顯示在編輯器範本中的模型時，是否應該呈現隱藏的 input 項目中。</span><span class="sxs-lookup"><span data-stu-id="04636-231">You can apply the new HiddenInputAttribute attribute to a model property to indicate whether a hidden input element should be rendered when displaying the model in an editor template.</span></span> <span data-ttu-id="04636-232">（此屬性設定 HiddenInput 隱含 UIHint 值）。</span><span class="sxs-lookup"><span data-stu-id="04636-232">(The attribute sets an implicit UIHint value of HiddenInput).</span></span> <span data-ttu-id="04636-233">屬性的 DisplayValue 屬性可讓您指定的值是否會顯示在編輯器中，並顯示模式。</span><span class="sxs-lookup"><span data-stu-id="04636-233">The attribute's DisplayValue property lets you specify whether the value is displayed in editor and display modes.</span></span> <span data-ttu-id="04636-234">當 DisplayValue 設定為 false 時，不會顯示，甚至不會正常括住欄位的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="04636-234">When DisplayValue is set to false, nothing is displayed, not even the HTML markup that normally surrounds a field.</span></span> <span data-ttu-id="04636-235">DisplayValue 的預設值為 true。</span><span class="sxs-lookup"><span data-stu-id="04636-235">The default value for DisplayValue is true.</span></span>

<span data-ttu-id="04636-236">在下列情況下，您可以使用 HiddenInputAttribute 屬性：</span><span class="sxs-lookup"><span data-stu-id="04636-236">You might use HiddenInputAttribute attribute in the following scenarios:</span></span>

- <span data-ttu-id="04636-237">當檢視可讓使用者編輯物件的識別碼，而且顯示的值，以及提供隱藏的 input 的元素，其中包含舊的識別碼，以便將它傳遞回控制器所需。</span><span class="sxs-lookup"><span data-stu-id="04636-237">When a view lets users edit the ID of an object and it is necessary to display the value as well as to provide a hidden input element that contains the old ID so that it can be passed back to the controller.</span></span>
- <span data-ttu-id="04636-238">檢視可讓使用者編輯應該永遠不會顯示，例如時間戳記屬性的二進位屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-238">When a view lets users edit a binary property that should never be displayed, such as a timestamp property.</span></span> <span data-ttu-id="04636-239">在此情況下，值和周圍的 HTML 標記 （例如標籤和值） 不會顯示。</span><span class="sxs-lookup"><span data-stu-id="04636-239">In that case, the value and surrounding HTML markup (such as the label and value) are not displayed.</span></span>

<span data-ttu-id="04636-240">下列範例示範如何使用 HiddenInputAttribute 類別。</span><span class="sxs-lookup"><span data-stu-id="04636-240">The following example shows how to use the HiddenInputAttribute class.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

<span data-ttu-id="04636-241">當屬性設定為 true （或未指定任何參數），會發生下列情況：</span><span class="sxs-lookup"><span data-stu-id="04636-241">When the attribute is set to true (or no parameter is specified), the following occurs:</span></span>

- <span data-ttu-id="04636-242">在顯示範本中，標籤會轉譯，並會向使用者顯示的值。</span><span class="sxs-lookup"><span data-stu-id="04636-242">In display templates, a label is rendered and the value is displayed to the user.</span></span>
- <span data-ttu-id="04636-243">在編輯器範本中，標籤會轉譯，而且在隱藏的 input 項目中呈現的值。</span><span class="sxs-lookup"><span data-stu-id="04636-243">In editor templates, a label is rendered and the value is rendered in a hidden input element.</span></span>

<span data-ttu-id="04636-244">當屬性設定為 false，則發生下列情況：</span><span class="sxs-lookup"><span data-stu-id="04636-244">When the attribute is set to false, the following occurs:</span></span>

- <span data-ttu-id="04636-245">顯示範本中不會呈現該欄位。</span><span class="sxs-lookup"><span data-stu-id="04636-245">In display templates, nothing is rendered for that field.</span></span>
- <span data-ttu-id="04636-246">編輯器範本中沒有標籤會轉譯，而且在隱藏的 input 項目中呈現的值。</span><span class="sxs-lookup"><span data-stu-id="04636-246">In editor templates, no label is rendered and the value is rendered in a hidden input element.</span></span>

### <a id="_TOC3_14"></a>  <span data-ttu-id="04636-247">Html.ValidationSummary 協助程式方法可以顯示模型層級錯誤</span><span class="sxs-lookup"><span data-stu-id="04636-247">Html.ValidationSummary Helper Method Can Display Model-Level Errors</span></span>

<span data-ttu-id="04636-248">而不是一律顯示所有驗證錯誤，Html.ValidationSummary helper 方法會具有新的選項，以只顯示模型層級錯誤。</span><span class="sxs-lookup"><span data-stu-id="04636-248">Instead of always displaying all validation errors, the Html.ValidationSummary helper method has a new option to display only model-level errors.</span></span> <span data-ttu-id="04636-249">這可讓模型層級的錯誤，要在驗證摘要中顯示和顯示每個欄位旁邊的欄位特定錯誤。</span><span class="sxs-lookup"><span data-stu-id="04636-249">This enables model-level errors to be displayed in the validation summary and field-specific errors to be displayed next to each field.</span></span>

### <a id="_TOC3_15"></a>  <span data-ttu-id="04636-250">在 Visual Studio 產生程式碼特定的 T4 範本至.NET framework 目標版本</span><span class="sxs-lookup"><span data-stu-id="04636-250">T4 Templates in Visual Studio Generate Code that is Specific to the Target Version of the .NET Framework</span></span>

<span data-ttu-id="04636-251">新的屬性是使用 T4 檔案從指定的應用程式所使用的.NET framework 版本的 ASP.NET MVC T4 主機。</span><span class="sxs-lookup"><span data-stu-id="04636-251">A new property is available to T4 files from the ASP.NET MVC T4 host that specifies the version of the .NET Framework that is used by the application.</span></span> <span data-ttu-id="04636-252">這可讓 T4 範本產生程式碼與.NET Framework 的版本特定的標記。</span><span class="sxs-lookup"><span data-stu-id="04636-252">This enables T4 templates to generate code and markup that is specific to a version of the .NET Framework.</span></span> <span data-ttu-id="04636-253">在 Visual Studio 2008 中，這個值一律是.NET 3.5。</span><span class="sxs-lookup"><span data-stu-id="04636-253">In Visual Studio 2008, the value is always .NET 3.5.</span></span> <span data-ttu-id="04636-254">在 Visual Studio 2010 中，值會是.NET 3.5 或.NET 4。</span><span class="sxs-lookup"><span data-stu-id="04636-254">In Visual Studio 2010, the value is either .NET 3.5 or .NET 4.</span></span>

## <a id="_TOC4"></a>  <span data-ttu-id="04636-255">API 增強功能</span><span class="sxs-lookup"><span data-stu-id="04636-255">API Improvements</span></span>

<span data-ttu-id="04636-256">本章節描述現有的 ASP.NET MVC 型別和成員的變更。</span><span class="sxs-lookup"><span data-stu-id="04636-256">This section describes changes to existing ASP.NET MVC types and members.</span></span>

- <span data-ttu-id="04636-257">新增控制器類別中的受保護的虛擬 CreateActionInvoker 方法。</span><span class="sxs-lookup"><span data-stu-id="04636-257">Added a protected virtual CreateActionInvoker method in the Controller class.</span></span> <span data-ttu-id="04636-258">這個方法會叫用控制器的 ActionInvoker 屬性，並允許叫用者延遲具現化如果已經不設定了任何啟動程式。</span><span class="sxs-lookup"><span data-stu-id="04636-258">This method is invoked by the ActionInvoker property of Controller and allows for lazy instantiation of the invoker if no invoker is already set.</span></span>
- <span data-ttu-id="04636-259">新增受保護的虛擬 HandleUnauthorizedRequest 方法 AuthorizeAttribute 類別中。</span><span class="sxs-lookup"><span data-stu-id="04636-259">Added a protected virtual HandleUnauthorizedRequest method in the AuthorizeAttribute class.</span></span> <span data-ttu-id="04636-260">這可讓衍生自 AuthorizeAttribute 授權失敗時，控制行為的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="04636-260">This enables filters that derive from AuthorizeAttribute to control the behavior when authorization fails.</span></span>
- <span data-ttu-id="04636-261">ValueProviderDictionary 類別中加入一個 Add （字串、 物件索引鍵值） 方法。</span><span class="sxs-lookup"><span data-stu-id="04636-261">Added an Add(string key, object value) method in the ValueProviderDictionary class.</span></span> <span data-ttu-id="04636-262">這可讓您針對 ValueProviderDictionary，使用字典初始設定式語法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="04636-262">This enables you to use the dictionary initializer syntax for ValueProviderDictionary, as in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- <span data-ttu-id="04636-263">新增 get\_物件 Sys.Mvc.AjaxContext 類別中的方法。</span><span class="sxs-lookup"><span data-stu-id="04636-263">Added a get\_object method in the Sys.Mvc.AjaxContext class.</span></span> <span data-ttu-id="04636-264">這是 JavaScript 方法類似於 get\_取得資料的方式，但如果回應的內容類型為 application/json，\_物件傳回的 JSON 物件。</span><span class="sxs-lookup"><span data-stu-id="04636-264">This is a JavaScript method that is similar to the get\_data method, but if the content type of the response is application/json, get\_object returns the JSON object.</span></span>
- <span data-ttu-id="04636-265">AuthorizationContext 類別中新增與 ActionDescriptor 屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-265">Added an ActionDescriptor property in the AuthorizationContext class.</span></span>
- <span data-ttu-id="04636-266">新增 UrlParameter.Optional 語彙基元可用來解決問題，到包含 ID 屬性，屬性不存在時的模型繫結時，在表單 post。</span><span class="sxs-lookup"><span data-stu-id="04636-266">Added a UrlParameter.Optional token that can be used to work around problems when binding to a model that contains an ID property when the property is absent in a form post.</span></span> <span data-ttu-id="04636-267">如需詳細資訊，請參閱文章[ASP.NET MVC 2 選擇性 URL 參數](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx)Phil Haack 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="04636-267">For more detail, see the entry [ASP.NET MVC 2 Optional URL Parameters](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) on Phil Haack's blog.</span></span>

## <a id="_TOC5"></a>  <span data-ttu-id="04636-268">重大變更</span><span class="sxs-lookup"><span data-stu-id="04636-268">Breaking Changes</span></span>

<span data-ttu-id="04636-269">下列變更可能會在現有的 ASP.NET MVC 1.0 應用程式中造成錯誤。</span><span class="sxs-lookup"><span data-stu-id="04636-269">The following changes might cause errors in existing ASP.NET MVC 1.0 applications.</span></span>

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a><span data-ttu-id="04636-270">在類別可實作 IDataErrorInfo 屬性驗證行為變更</span><span class="sxs-lookup"><span data-stu-id="04636-270">Change in property validation behavior for classes that implement IDataErrorInfo</span></span>

<span data-ttu-id="04636-271">對於使用 IDataErrorInfo 來執行驗證的模型物件，會驗證每個屬性，不論是否已設定新的值。</span><span class="sxs-lookup"><span data-stu-id="04636-271">For model objects that use IDataErrorInfo to perform validation, every property is validated, regardless of whether a new value was set.</span></span> <span data-ttu-id="04636-272">在 ASP.NET MVC 1.0 中，已驗證已設定的新值的屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-272">In ASP.NET MVC 1.0, only properties that had new values set were validated.</span></span> <span data-ttu-id="04636-273">在 ASP.NET MVC 2 中，只有當所有的屬性驗證成功時，才呼叫 IDataErrorInfo 的錯誤屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-273">In ASP.NET MVC 2, the Error property of IDataErrorInfo is called only if all the property validators were successful.</span></span>

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a><span data-ttu-id="04636-274">IIS 指令碼對應指令碼已無法再使用安裝程式中</span><span class="sxs-lookup"><span data-stu-id="04636-274">IIS script mapping script is no longer available in the installer</span></span>

<span data-ttu-id="04636-275">IIS 指令碼對應指令碼是用來在傳統模式中針對 IIS 6 和 IIS 7 設定指令碼對應的命令列指令碼。</span><span class="sxs-lookup"><span data-stu-id="04636-275">The IIS script-mapping script is a command-line script that is used to configure script maps for IIS 6 and for IIS 7 in Classic mode.</span></span> <span data-ttu-id="04636-276">如果您使用 Visual Studio 程式開發伺服器，或如果您使用 IIS 7 整合模式中，就不需要指令碼對應指令碼。</span><span class="sxs-lookup"><span data-stu-id="04636-276">The script-mapping script is not needed if you use the Visual Studio Development Server or if you use IIS 7 in Integrated mode.</span></span> <span data-ttu-id="04636-277">指令碼可在個別不支援下載[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。</span><span class="sxs-lookup"><span data-stu-id="04636-277">The scripts are available as a separate unsupported download on the [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).</span></span>

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a><span data-ttu-id="04636-278">在 MVC Futures Html.Substitute 協助程式方法不再提供</span><span class="sxs-lookup"><span data-stu-id="04636-278">The Html.Substitute helper method in MVC Futures is no longer available</span></span>

<span data-ttu-id="04636-279">由於在 MVC 檢視引擎的轉譯行為的變更，Html.Substitute helper 方法會無法運作，並已移除。</span><span class="sxs-lookup"><span data-stu-id="04636-279">Due to changes in the rendering behavior of MVC view engines, the Html.Substitute helper method does not work and has been removed.</span></span>

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a><span data-ttu-id="04636-280">IValueProvider 介面會取代所有使用 IDictionary</span><span class="sxs-lookup"><span data-stu-id="04636-280">The IValueProvider interface replaces all uses of IDictionary</span></span>

<span data-ttu-id="04636-281">現在接受 IDictionary MVC 1.0 中的每個屬性或方法引數會接受 IValueProvider。</span><span class="sxs-lookup"><span data-stu-id="04636-281">Every property or method argument that accepted IDictionary in MVC 1.0 now accepts IValueProvider.</span></span> <span data-ttu-id="04636-282">這項變更會影響僅包含自訂值提供者或自訂模型繫結器的應用程式。</span><span class="sxs-lookup"><span data-stu-id="04636-282">This change affects only applications that include custom value providers or custom model binders.</span></span> <span data-ttu-id="04636-283">屬性和方法，會受到這項變更的範例包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="04636-283">Examples of properties and methods that are affected by this change include the following:</span></span>

- <span data-ttu-id="04636-284">ValueProvider ControllerBase 和 ModelBindingContext 類別屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-284">The ValueProvider property of the ControllerBase and ModelBindingContext classes.</span></span>
- <span data-ttu-id="04636-285">控制器類別 TryUpdateModel 方法。</span><span class="sxs-lookup"><span data-stu-id="04636-285">The TryUpdateModel methods of the Controller class.</span></span>

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a><span data-ttu-id="04636-286">Site.css 檔案中加入新的 CSS 類別</span><span class="sxs-lookup"><span data-stu-id="04636-286">New CSS classes were added in the Site.css file</span></span>

<span data-ttu-id="04636-287">Site.css 檔案中的 ASP.NET MVC 專案範本已更新為包含新的樣式和樣板化 helper 的驗證功能所使用。</span><span class="sxs-lookup"><span data-stu-id="04636-287">The Site.css file in the ASP.NET MVC project templates has been updated to include new styles used by the validation functionality and by the templated helpers.</span></span>

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a><span data-ttu-id="04636-288">協助程式立即傳回 MvcHtmlString 物件</span><span class="sxs-lookup"><span data-stu-id="04636-288">Helpers now return an MvcHtmlString object</span></span>

<span data-ttu-id="04636-289">若要利用新的 HTML 編碼運算式語法的 ASP.NET 4 中，HTML helper 的傳回型別是現在 MvcHtmlString 而非字串。</span><span class="sxs-lookup"><span data-stu-id="04636-289">In order to take advantage of the new HTML-encoding expression syntax in ASP.NET 4, the return type for HTML helpers is now MvcHtmlString instead of a string.</span></span> <span data-ttu-id="04636-290">如果您使用 ASP.NET MVC 2 和新的協助程式在 ASP.NET 3.5 版時，您將無法利用 HTML 編碼的語法。只有當您在 ASP.NET 4 執行 ASP.NET MVC 2 時，才使用新的語法。</span><span class="sxs-lookup"><span data-stu-id="04636-290">If you use ASP.NET MVC 2 and the new helpers on ASP.NET 3.5, you will not be able to take advantage of the HTML-encoding syntax; the new syntax is available only when you run ASP.NET MVC 2 on ASP.NET 4.</span></span>

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a><span data-ttu-id="04636-291">JsonResult 現在只回應 HTTP POST 要求</span><span class="sxs-lookup"><span data-stu-id="04636-291">JsonResult now responds only to HTTP POST requests</span></span>

<span data-ttu-id="04636-292">若要減輕劫持攻擊，根據預設，有資訊洩漏的可能性的 JSON JsonResult 類別現在只回應 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="04636-292">In order to mitigate JSON hijacking attacks that have the potential for information disclosure, by default, the JsonResult class now responds only to HTTP POST requests.</span></span> <span data-ttu-id="04636-293">取得 Ajax 呼叫動作方法會傳回 JsonResult 物件應該要改為使用 POST 變更。</span><span class="sxs-lookup"><span data-stu-id="04636-293">Ajax GET calls to action methods that return a JsonResult object should be changed to use POST instead.</span></span> <span data-ttu-id="04636-294">如有必要，您可以設定 JsonResult 新 JsonRequestBehavior 屬性來覆寫此行為。</span><span class="sxs-lookup"><span data-stu-id="04636-294">If necessary, you can override this behavior by setting the new JsonRequestBehavior property of JsonResult.</span></span> <span data-ttu-id="04636-295">如需有關潛在惡意探索的詳細資訊，請參閱部落格文章[JSON 劫持](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="04636-295">For more information about the potential exploit, see the blog post [JSON Hijacking](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) on Phil Haack's blog.</span></span>

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a><span data-ttu-id="04636-296">模型和模型類型的屬性 setter ModelBindingContext 上已過時</span><span class="sxs-lookup"><span data-stu-id="04636-296">Model and ModelType property setters on ModelBindingContext are obsolete</span></span>

<span data-ttu-id="04636-297">已新增到 ModelBindingContext 類別的新的可設定 ModelMetadata 屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-297">A new settable ModelMetadata property has been added to the ModelBindingContext class.</span></span> <span data-ttu-id="04636-298">新的屬性會封裝模型和模型類型的屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-298">The new property encapsulates both the Model and the ModelType properties.</span></span> <span data-ttu-id="04636-299">雖然模型和模型類型的屬性已經過時，回溯相容性的屬性 getter 仍然運作;它們會委派至 ModelMetadata 屬性來擷取值。</span><span class="sxs-lookup"><span data-stu-id="04636-299">Although the Model and ModelType properties are obsolete, for backward compatibility the property getters still work; they delegate to the ModelMetadata property to retrieve the value.</span></span>

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a><span data-ttu-id="04636-300">DefaultControllerFactory 類別的變更會中斷從它衍生的自訂控制器 factory</span><span class="sxs-lookup"><span data-stu-id="04636-300">Changes to the DefaultControllerFactory class break custom controller factories that derive from it</span></span>

<span data-ttu-id="04636-301">藉由移除 RequestContext 屬性，已修正 DefaultControllerFactory 類別。</span><span class="sxs-lookup"><span data-stu-id="04636-301">The DefaultControllerFactory class was fixed by removing the RequestContext property.</span></span> <span data-ttu-id="04636-302">取代這個屬性，要求內容執行個體傳遞給受保護的虛擬 GetControllerInstance 和 GetControllerType 方法。</span><span class="sxs-lookup"><span data-stu-id="04636-302">In place of this property, the request context instance is passed to the protected virtual GetControllerInstance and GetControllerType methods.</span></span> <span data-ttu-id="04636-303">這項變更會影響從 DefaultControllerFactory 衍生的自訂控制器 factory。</span><span class="sxs-lookup"><span data-stu-id="04636-303">This change affects custom controller factories that derive from DefaultControllerFactory.</span></span>

<span data-ttu-id="04636-304">自訂的控制器 factory 通常用來提供相依性插入，ASP.NET mvc 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04636-304">Custom controller factories are often used to provide dependency injection for ASP.NET MVC applications.</span></span> <span data-ttu-id="04636-305">若要更新自訂的控制器 factory，以支援 ASP.NET MVC 2，變更方法簽章或簽章，以符合新的簽章，並使用要求的內容參數而非屬性。</span><span class="sxs-lookup"><span data-stu-id="04636-305">To update the custom controller factories to support ASP.NET MVC 2, change the method signature or signatures to match the new signatures, and use the request context parameter instead of the property.</span></span>

#### <a name="area-is-a-now-a-reserved-route-value-key"></a><span data-ttu-id="04636-306">「 區域 」 是目前保留的路由值索引鍵</span><span class="sxs-lookup"><span data-stu-id="04636-306">"Area" is a now a reserved route-value key</span></span>

<span data-ttu-id="04636-307">字串 「 區域 」 中的路由值現在會有在 ASP.NET MVC 中，特殊的意義相同的方式，「 控制器 」 和 「 動作 」。</span><span class="sxs-lookup"><span data-stu-id="04636-307">The string "area" in Route values now has special meaning in ASP.NET MVC, in the same way that "controller" and "action" do.</span></span> <span data-ttu-id="04636-308">其中一個含義為，如果路由值字典，其中包含 「 區域 」 提供 HTML 協助程式，協助程式將不會再附加 「 區域 」 的查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="04636-308">One implication is that if HTML helpers are supplied with a route-value dictionary containing "area", the helpers will no longer append "area" in the query string.</span></span>

<span data-ttu-id="04636-309">如果您使用的 「 區域 」 功能，請確定您的路由 URL 的一部分使用 {區域}。</span><span class="sxs-lookup"><span data-stu-id="04636-309">If you are using the Areas feature, make sure to not use {area} as part of your route URL.</span></span>


## <a id="_TOC6"></a>  <span data-ttu-id="04636-310">免責聲明</span><span class="sxs-lookup"><span data-stu-id="04636-310">Disclaimer</span></span>

<span data-ttu-id="04636-311">這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。</span><span class="sxs-lookup"><span data-stu-id="04636-311">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="04636-312">本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。</span><span class="sxs-lookup"><span data-stu-id="04636-312">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="04636-313">Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。</span><span class="sxs-lookup"><span data-stu-id="04636-313">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="04636-314">本技術白皮書僅供參考。</span><span class="sxs-lookup"><span data-stu-id="04636-314">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="04636-315">MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。</span><span class="sxs-lookup"><span data-stu-id="04636-315">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="04636-316">承諾遵守所有適用的著作權法是使用者的責任。</span><span class="sxs-lookup"><span data-stu-id="04636-316">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="04636-317">著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。</span><span class="sxs-lookup"><span data-stu-id="04636-317">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="04636-318">本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。</span><span class="sxs-lookup"><span data-stu-id="04636-318">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="04636-319">除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。</span><span class="sxs-lookup"><span data-stu-id="04636-319">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="04636-320">除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人物、 地點及事件本文所述之屬虛構，以及與任何真實公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件是純屬巧合。</span><span class="sxs-lookup"><span data-stu-id="04636-320">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="04636-321">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="04636-321">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="04636-322">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="04636-322">All rights reserved.</span></span>

<span data-ttu-id="04636-323">Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。</span><span class="sxs-lookup"><span data-stu-id="04636-323">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="04636-324">本文件中所提實際公司和產品，可能為各所有人所有之商標。</span><span class="sxs-lookup"><span data-stu-id="04636-324">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
