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
<a name="whats-new-in-aspnet-mvc-2"></a>ASP.NET MVC 2 中最新消息
====================
> 本文件說明新功能和 ASP.NET MVC 2 中引進的增強功能。 本文件也會提供如[下載](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[簡介](#_TOC1)   
[ASP.NET MVC 1.0 專案升級至 ASP.NET MVC 2](#_TOC2)   
[新功能](#_TOC3)   
[樣板化 Helper](#_TOC3_1)   
[區域](#_TOC3_2)   
[支援非同步控制器](#_TOC3_3)   
[DefaultValueAttribute 在動作方法參數的支援](#_TOC3_4)   
[繫結模型繫結器的二進位資料支援](#_TOC3_5)   
[ModelMetadata 和 ModelMetadataProvider 類別](#_TOC3_6)   
[支援 DataAnnotations 屬性](#_TOC3_7)   
[模型驗證程式提供者](#_TOC3_8)   
[用戶端驗證](#_TOC3_9)   
[適用於 Visual Studio 2010 的新程式碼片段](#_TOC3_10)   
[新的 RequireHttpsAttribute 動作篩選條件](#_TOC3_11)   
[覆寫的 HTTP 方法的動詞命令](#_TOC3_12)   
[樣板化 Helper 的新 HiddenInputAttribute 類別](#_TOC3_13)   
[Html.ValidationSummary Helper 方法可顯示模型層級錯誤](#_TOC3_14)   
[T4 範本在 Visual Studio 產生程式碼在特定的.NET framework 目標版本](#_TOC3_15)[應用程式開發介面改進](#_TOC4)  
[重大變更](#_TOC5)  
[Disclaimer](#_TOC6)  

## <a id="_TOC1"></a>簡介

ASP.NET MVC 2 ASP.NET MVC 1.0 為基礎，並介紹增強功能和功能，將焦點放在可以提升產能的大型集合。 此版本是與 ASP.NET MVC 1.0 相容，因此所有您知識、 技術、 程式碼和 ASP.NET MVC 1.0 延伸仍會繼續套用。

如需有關 ASP.NET MVC 的詳細資訊，請瀏覽下列資源：

- [MSDN 上的 ASP.NET MVC 文件](https://go.microsoft.com/fwlink/?LinkId=159758)
- [ASP.NET MVC 網站](https://asp.net/mvc/)
- [ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>ASP.NET MVC 1.0 專案升級至 ASP.NET MVC 2

ASP.NET MVC 2 可以與 ASP.NET MVC 1.0 並存安裝在相同伺服器上，讓應用程式開發人員在選擇何時要升級至 ASP.NET MVC 2 ASP.NET MVC 1.0 應用程式的彈性。 如需如何升級的資訊，請參閱文件[升級至 ASP.NET MVC 2 1.0 ASP.NET MVC 應用程式](https://go.microsoft.com/fwlink/?LinkID=185459)。

## <a id="_TOC3"></a>新功能

本節說明已引進的功能在 MVC 2 版本。

### <a id="_TOC3_1"></a>樣板化 Helper

樣板化 helper 可讓您自動產生關聯以編輯的 HTML 項目，並顯示與資料型別。 例如，在檢視中顯示 System.DateTime 類型的資料時，日期選擇器 UI 項目可以自動轉譯。 這是類似於在 ASP.NET 動態資料欄位範本的運作方式。 如需詳細資訊，請參閱[來顯示的資料使用樣板化 Helper](https://go.microsoft.com/fwlink/?LinkId=159062) MSDN 網站上。

### <a id="_TOC3_2"></a>區域

區域可讓您將大型專案組織成多個較小區段，才能管理大型的 Web 應用程式的複雜性。 每個區段 （「 區域 」） 通常代表不同區段的大型網站，並將用來分組的控制器和檢視的相關的設定。 如需詳細資訊，請參閱[逐步解說： 組織 ASP.NET MVC 應用程式依區域](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN 網站上。

若要建立新的區域，請在 [方案總管] 中，以滑鼠右鍵按一下專案，按一下 [新增]，然後按一下區域。 這會顯示對話方塊提示您輸入的區域名稱。 輸入區域名稱之後，Visual Studio 將專案加入新的區域。

下圖顯示具有兩個區域，系統管理員和部落格專案配置範例。

![](what-is-new-in-aspnet-mvc/_static/image1.png)

當您建立區域時，Visual Studio 會加入至每個區域從 AreaRegistration 衍生的類別。 這個類別，才能註冊區域和其路由中，如下列範例所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

ASP.NET MVC 2 的預設專案範本在 Global.asax 檔案的程式碼中包含 RegisterAllAreas 方法的呼叫。 這個方法會尋找所有衍生自 AreaRegistration 類別的類型、 具現化類型的執行個體以及執行個體上呼叫 RegisterArea 方法註冊在專案中的每個區域。 下列範例會示範方式。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

如果您未藉由呼叫內容 RegisterArea 方法中指定的命名空間。預設會使用 Namespaces.Add 方法，登錄類別的命名空間。

### <a id="_TOC3_3"></a>支援非同步控制器

ASP.NET MVC 2 現在可讓以非同步方式處理要求的控制站。 藉由使用伺服器經常呼叫改為呼叫非封鎖的對應項目 （如網路要求） 的封鎖作業，這可能會導致效能提升。 如需詳細資訊，請參閱[在 ASP.NET MVC 中使用非同步控制器](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)MSDN 上的主題。

### <a id="_TOC3_4"></a>DefaultValueAttribute 在動作方法參數的支援

System.ComponentModel.DefaultValueAttribute 類別可讓動作方法的引數參數所提供的預設值。 例如，假設下列預設路由，定義：

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

也假設您已定義下列的控制器與動作方法：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

任何下列要求 Url 會叫用前一個範例中定義的檢視動作方法。

- / 文件/檢視/123
- / 文件/檢視/123？ 頁面 = 1 （有效地如同前一個要求）
- /Article/View/123?page=2

若沒有 DefaultValueAttribute 屬性從上述清單的第一個 URL 不會執行，因為頁面引數是尚未提供其值不可為 null 的實值型別。

如果在 Visual Basic 2010 或 Visual C# 2010年中撰寫的程式碼，您可以使用選擇性參數，而不是 DefaultValueAttribute 屬性，如下列範例所示即可：

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>繫結模型繫結器的二進位資料支援

有兩個新的多載 Html.Hidden helper 編碼為 base-64 編碼字串的二進位值：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

一般用法是內嵌在檢視中物件的時間戳記。 例如，您的應用程式可能包含下列產品物件：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

編輯表單可以轉譯格式，如下列範例所示的時間戳記屬性：

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

這個標記會隱藏輸入的元素具有時間戳記值轉譯為 base 64 編碼的字串，類似下列範例：

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

此表單可能會張貼至動作方法具有產品類型的引數，如下列範例所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

在動作方法，將時間戳記屬性就會擴展正確因為張貼的 base-64 編碼字串轉換成位元組陣列。

### <a id="_TOC3_6"></a>ModelMetadata 和 ModelMetadataProvider 類別

ModelMetadataProvider 類別提供用來取得內檢視的模型中繼資料的抽象概念。 MVC 2 包含預設提供者，可提供 System.ComponentModel.DataAnnotations 命名空間中的屬性所公開的中繼資料。 很可能會建立提供從其他資料存放區，例如資料庫或 XML 檔案的中繼資料的中繼資料提供者。

ViewDataDictionary 類別會公開包含由 ModelMetadataProvider 類別從模型擷取中繼資料的 ModelMetadata 物件。 這可讓取用這個中繼資料，並據以調整其輸出的樣板化 helper。

如需詳細資訊，請參閱文件[ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)和[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)類別。

### <a id="_TOC3_7"></a>支援 DataAnnotations 屬性

ASP.NET MVC 2 支援使用 RangeAttribute、 出現 RequiredAttribute、 StringLengthAttribute 和 RegexAttribute 驗證屬性 （System.ComponentModel.DataAnnotations 命名空間中定義） 當您繫結至模型以提供的輸入驗證。

如需詳細資訊，請參閱[How to： 驗證模型資料使用 DataAnnotations 屬性](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN 網站上。 說明如何使用這些屬性的範例專案會下載[https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753)。

### <a id="_TOC3_8"></a>模型驗證程式提供者

模型驗證提供者類別代表模型提供驗證邏輯的抽象概念。 ASP.NET MVC 包括根據 System.ComponentModel.DataAnnotations 命名空間中包含的驗證屬性的預設提供者。 您也可以建立自己的驗證提供者定義模型的自訂驗證規則和自訂驗證規則的對應。 如需詳細資訊，請參閱文件[ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)類別。

### <a id="_TOC3_9"></a>用戶端驗證

模型驗證程式提供者類別公開給可供用戶端驗證程式庫的 JSON 序列化資料的形式瀏覽器的驗證中繼資料。 ASP.NET MVC 2 包含用戶端驗證程式庫和配接器支援先前所述的 DataAnnotations 命名空間驗證屬性。 提供者類別也可讓您使用其他用戶端驗證程式庫所撰寫的配接器處理的 JSON 資料和其他程式庫的呼叫。

### <a id="_TOC3_10"></a>適用於 Visual Studio 2010 的新程式碼片段

ASP.NET MVC 2 的 HTML 程式碼片段的一組會隨 Visual Studio 2010。 若要檢視的這些片段，在 [工具] 功能表中，選取 [程式碼片段管理員。 語言中，選取 [HTML] 和位置，請選取 [ASP.NET MVC 2。 如需如何使用程式碼片段的詳細資訊，請參閱 Visual Studio 文件。

### <a id="_TOC3_11"></a>新的 RequireHttpsAttribute 動作篩選條件

ASP.NET MVC 2 包含新 RequireHttpsAttribute 類別，可以套用至動作方法和控制器。 根據預設，篩選要求重新導向至非 SSL HTTP 啟用 SSL (HTTPS) 對等項目。

### <a id="_TOC3_12"></a>覆寫的 HTTP 方法的動詞命令

當您使用 rest 架構建置的網站時，可用來判斷何種動作來執行資源的 HTTP 動詞命令。 REST 要求該應用程式支援完整的通用的 HTTP 動詞命令，其中包括 GET、 PUT、 POST 和刪除。

ASP.NET MVC 2 包含新屬性，您可以套用至動作方法和該功能的精簡語法。 這些屬性可讓 ASP.NET MVC 選取動作方法，根據 HTTP 指令動詞。 在下列範例中，POST 要求會呼叫第一個動作方法和 PUT 要求會呼叫第二個動作方法。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

在舊版 ASP.NET MVC 中，這些動作方法所需更詳細的語法，如下列範例所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

瀏覽器都支援只有 GET 和 POST HTTP 動詞命令，因為它不張貼到需要不同的指令動詞的動作。 因此它是不可能原本就支援 rest 式的所有要求。

不過，以支援 RESTful 要求在 POST 作業時，ASP.NET MVC 2 導入新的 HttpMethodOverride HTML helper 方法。 這個方法會呈現隱藏的 input 項目，會導致表單，以有效地模擬任何 HTTP 方法。 例如，使用 HttpMethodOverride HTML helper 方法，您可以有出現的表單提交是 PUT 或 DELETE 要求。 HttpMethodOverride 的行為會影響下列屬性：

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

隱藏的 input 項目具有 X-HTTP-方法-覆寫其名稱和其值設為 HTTP 動詞命令，以模擬。 覆寫值也可以指定在 HTTP 標頭或查詢字串值做為名稱/值組。

POST 要求實際要求時，只可以使用覆寫。 使用任何其他 HTTP 動詞命令的要求，將會忽略覆寫值。

### <a id="_TOC3_13"></a>樣板化 Helper 的新 HiddenInputAttribute 類別

您可以將新 HiddenInputAttribute 屬性套用至模型屬性，指出編輯器範本中顯示此模型時是否應該呈現隱藏的 input 的元素。 （此屬性設定 HiddenInput 隱含 UIHint 值）。 屬性的 DisplayValue 屬性可讓您指定的值編輯器中會顯示與顯示模式。 當 DisplayValue 設定為 false 時，不會顯示不，即使通常括住欄位的 HTML 標記。 DisplayValue 的預設值為 true。

在下列案例中，您可以使用 HiddenInputAttribute 屬性：

- 當檢視可讓使用者編輯物件的識別碼，而且是要提供包含舊的識別碼，因此可以將它傳遞至控制器的隱藏 input 的元素顯示值，以及所需。
- 檢視可讓使用者編輯的二進位屬性應永遠不會顯示，例如時間戳記屬性。 在此情況下，不顯示的值與周圍的 HTML 標記 （例如標籤和值）。

下列範例會示範如何使用 HiddenInputAttribute 類別。

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

當屬性設定為 true （或未指定任何參數），則發生下列情況：

- 顯示範本中轉譯標籤並向使用者顯示的值。
- 編輯器範本中轉譯的標籤，並在隱藏的 input 項目中呈現的值。

當屬性設定為 false，則發生下列情況：

- 顯示範本中不會呈現該欄位。
- 編輯器範本中沒有標籤轉譯，而且在隱藏的 input 項目中呈現的值。

### <a id="_TOC3_14"></a>Html.ValidationSummary Helper 方法可顯示模型層級錯誤

而不是永遠顯示的所有驗證錯誤，Html.ValidationSummary helper 方法會具有新的選項，以顯示只有模型層級錯誤。 這可讓模型層級錯誤，驗證摘要中顯示和顯示每個欄位旁邊的特定欄位的錯誤。

### <a id="_TOC3_15"></a>T4 範本在 Visual Studio 產生程式碼在特定的.NET framework 目標版本

新的屬性是可用的 T4 檔案從指定的應用程式使用.NET Framework 版本的 ASP.NET MVC 的 T4 主機。 這可讓 T4 範本產生程式碼與特定的.NET Framework 版本的標記。 在 Visual Studio 2008 中，這個值一律是.NET 3.5。 在 Visual Studio 2010 中，值會是.NET 3.5 或.NET 4。

## <a id="_TOC4"></a>應用程式開發介面改進

本章節描述對現有的 ASP.NET MVC 型別和成員。

- 加入控制器類別中的受保護的虛擬 CreateActionInvoker 方法。 這個方法會叫用控制器的 ActionInvoker 屬性，並允許延遲的具現化的啟動程式如果已經不設定了任何啟動程式。
- 新增受保護的虛擬 HandleUnauthorizedRequest 方法 AuthorizeAttribute 類別中。 這可讓衍生自 AuthorizeAttribute 授權失敗時，控制行為的篩選器。
- ValueProviderDictionary 類別中加入一個 Add （字串、 物件索引鍵值） 方法。 這可讓您使用字典初始設定式語法 ValueProviderDictionary，如下列範例所示：

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- 加入 get\_Sys.Mvc.AjaxContext 類別中的方法的物件。 這是 JavaScript 方法類似於 get\_取得資料的方式，但如果回應的內容類型是應用程式 /json\_物件傳回的 JSON 物件。
- 加入了 ActionDescriptor 屬性 AuthorizationContext 類別中。
- 加入 UrlParameter.Optional 語彙基元可用來解決問題，到包含 ID 屬性的屬性不存在時的模型繫結時，在表單張貼中。 如需詳細資訊，請參閱文章[ASP.NET MVC 2 選擇性 URL 參數](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx)Phil Haack 的部落格上。

## <a id="_TOC5"></a>重大變更

下列變更可能會導致現有的 ASP.NET MVC 1.0 應用程式的錯誤。

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>變更中屬性驗證行為的類別可實作 IDataErrorInfo

對於使用 IDataErrorInfo 來執行驗證的模型物件，會驗證每個屬性，不論是否有設定新的值。 在 ASP.NET MVC 1.0 中，已驗證已設定的新值的屬性。 在 ASP.NET MVC 2 中，所有的屬性驗證成功時，才呼叫 IDataErrorInfo 的錯誤屬性。

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>IIS 指令碼對應指令碼已無法再使用在安裝程式

IIS 指令碼對應指令碼是用來以傳統模式針對 IIS 6 和 IIS 7 設定指令碼對應的命令列指令碼。 如果您使用 Visual Studio 程式開發伺服器，或如果您使用 IIS 7 以整合模式，則不需要指令碼對應指令碼。 上不支援個別下載的指令碼可用[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)。

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Html.Substitute helper 方法，在 MVC 未來已無法再使用

因為 MVC 檢視引擎的轉譯行為已變更，Html.Substitute helper 方法會無法運作，而已經移除。

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>IValueProvider 介面取代 IDictionary 的所有使用

現在接受 IDictionary MVC 1.0 中的每個屬性或方法引數會接受 IValueProvider。 這項變更會影響包含自訂值提供者或自訂的模型繫結器的應用程式。 會受到這項變更的屬性和方法的範例包括下列各項：

- ValueProvider ControllerBase 和 ModelBindingContext 類別的屬性。
- 控制器類別 TryUpdateModel 方法。

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Site.css 檔案中已加入新的 CSS 類別

Site.css 檔案中的 ASP.NET MVC 專案範本已更新為包含新的樣式和樣板化 helper 的驗證功能所使用。

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>協助程式現在會傳回 MvcHtmlString 物件

若要利用新的 HTML 編碼運算式語法的 ASP.NET 4 中，HTML helper 的傳回型別是現在 MvcHtmlString 而不是字串。 如果您使用 ASP.NET MVC 2 和新的協助程式在 ASP.NET 3.5 版時，您將無法利用 HTML 編碼的語法。只有當您在 ASP.NET 4 執行 ASP.NET MVC 2 時，才使用新語法。

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult 現在只回應 HTTP POST 要求

若要減輕 JSON 劫持資訊洩露，可能會有預設的攻擊 JsonResult 類別現在只回應 HTTP POST 要求。 取得 Ajax 呼叫動作方法會傳回 JsonResult 物件應該變更為改用 POST。 如有必要，您可以設定 JsonResult 新 JsonRequestBehavior 屬性覆寫此行為。 如需可能的攻擊的詳細資訊，請參閱部落格文章[JSON 劫持](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack 的部落格上。

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>模型和 ModelType 屬性 setter 上 ModelBindingContext 已過時

新的可設定 ModelMetadata 屬性已加入至 ModelBindingContext 類別。 新的屬性封裝模型和 ModelType 屬性。 雖然模型和 ModelType 屬性已經過時，回溯相容性的屬性 getter 還能運作;這些委派給 ModelMetadata 屬性來擷取的值。

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>DefaultControllerFactory 類別的變更會中斷從它衍生的自訂控制器 factory

DefaultControllerFactory 類別已修正藉由移除 RequestContext 屬性。 取代這個屬性，要求內容執行個體傳遞給受保護的虛擬 GetControllerInstance 和 GetControllerType 方法。 這項變更會影響從 DefaultControllerFactory 衍生的自訂控制器 factory。

自訂的控制器 factory 通常用於提供相依性插入 ASP.NET mvc 應用程式。 若要更新的自訂控制器處理站，以支援 ASP.NET MVC 2，變更方法簽章或簽章，以符合新的簽章，並使用要求的內容參數而非屬性。

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>「 區域 」 現在是保留的路由值的索引鍵

字串 「 區域 」 中的路由值現在會有在 ASP.NET MVC 中，特殊的意義相同的方式執行 「 控制器 」 和 「 動作 」。 一個含意為，如果路由值字典，其中包含 「 區域 」 提供 HTML helper，協助程式將不會再附加 「 區域 」 查詢字串中。

如果您使用 「 區域 」 功能，請務必使用 {區域} 做為路由 URL 的一部分。


## <a id="_TOC6"></a>  Disclaimer

這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。

本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。 Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。

本技術白皮書僅供參考。 MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。

承諾遵守所有適用的著作權法是使用者的責任。 著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。

本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。 除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。

除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人員、 地點及事件屬虛構，以及與任何真實的公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件純屬巧合。

© 2010 Microsoft Corporation. 著作權所有，並保留一切權利。

Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。

本文件中所提實際公司和產品，可能為各所有人所有之商標。
