---
title: "錨點標籤協助程式"
author: pkellner
description: "探索 ASP.NET Core 錨點標籤協助程式屬性，以及各屬性在 HTML 錨點標籤的延伸行為中所扮演的角色。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a>錨點標籤協助程式

由 [Peter Kellner](http://peterkellner.net) 與 [Scott Addie](https://github.com/scottaddie) 撰寫

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

[錨點標籤協助程式](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper)藉由新增新的屬性，來增強標準 HTML 錨點 (`<a ... ></a>`) 標籤。 依照慣例，屬性名稱的開頭會加上 `asp-`。 所轉譯錨點元素的 `href` 屬性值取決於 `asp-` 屬性的值。

這整份文件的範例皆使用 *SpeakerController*：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

`asp-` 屬性如下所列。

## <a name="asp-controller"></a>asp-controller

[asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) 屬性指派用於產生 URL 的控制器。 下列標記列出所有喇叭：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

產生的 HTML：

```html
<a href="/Speaker">All Speakers</a>
```

若僅指定 `asp-controller` 屬性而未指定 `asp-action`，則預設的 `asp-action` 值即為與目前所執行之檢視相關的控制器動作。 若在上述的標記中省略 `asp-action`，且在 *HomeController*'s *Index* 檢視 (*/Home*) 使用錨點標籤協助程式，則產生的 HTML 即為：

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

[asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) 屬性值代表所產生 `href` 屬性中包含的控制器動作名稱。 下列標記會將產生的 `href` 屬性值設為喇叭評估頁面：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

產生的 HTML：

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

若未指定任何 `asp-controller` 屬性，則會使用呼叫執行目前檢視之檢視的預設控制器。

若 `asp-action` 屬性值為 `Index`，就不會將任何動作附加至 URL，而導致引動預設的 `Index` 動作。 指定的動作 (或預設的動作) 必須存在於 `asp-controller` 所參考的控制器中。

## <a name="asp-route-value"></a>asp-route-{value}

[asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 屬性可使用萬用字元路由首碼。 任何佔用 `{value}` 預留位置的值都會解譯為潛在的路由參數。 如果找不到預設路由，則會將此路由首碼附加至產生的 `href` 屬性，作為要求參數與值。 否則會在路由範本中加以取代。

請考慮下列控制器動作：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

使用 *Startup.Configure* 中定義的預設路由範本：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

MVC 檢視會使用動作提供的模型，如下所示：

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

預設路由的 `{id?}` 預留位置相符。 產生的 HTML：

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

假設路由首碼不屬於相符路由範本的一部份，如同下列 MVC 檢視：

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

因為在相符路由中找不到 `speakerid`，所以產生了下列 HTML：

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

如果未指定 `asp-controller` 或 `asp-action`，則會遵循與 `asp-route` 屬性中相同的預設處理。

## <a name="asp-route"></a>asp-route

[asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) 屬性用於建立直接連結至具名路由的 URL。 使用[路由屬性](xref:mvc/controllers/routing#attribute-routing)，路由可以如 `SpeakerController` 中所示命名並用於其 `Evaluations` 動作：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

在下列標記中，`asp-route` 屬性參考了具名路由：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

錨點標記協助程式會使用 URL */Speaker/Evaluations*，直接對該控制器動作產生路由。 產生的 HTML：

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

如果除了 `asp-route`，還指定了 `asp-controller` 或 `asp-action`，產生的路由可能不如預期。 為避免路由衝突，請勿將 `asp-route` 與 `asp-controller` 及 `asp-action` 屬性搭配使用。

## <a name="asp-all-route-data"></a>asp-all-route-data

[asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 屬性支援建立機碼值組的字典。 機碼為參數名稱，值則為參數值。

在下列範例中，字典會經過初始化並傳遞至 Razor 檢視。 或者，您也可以使用自己的模型來傳遞資料。

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

上述程式碼會產生下列 HTML：

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` 字典已經過扁平化，以產生符合多載 `Evaluations` 動作之需求的查詢字串。

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

若字典中有任何機碼符合路由參數，這些值在路由中會受到適當地取代。 其他不相符的值則產生作為要求參數。

## <a name="asp-fragment"></a>asp-fragment

[asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) 屬性定義要附加至 URL 的 URL 片段。 錨點標籤協助程式會新增雜湊字元 (#)。 請考慮下列標記：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

產生的 HTML：

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

雜湊標籤在建置用戶端應用程式時很實用。 例如，您可以使用這些標籤在 JavaScript 中輕鬆標記和搜尋。

## <a name="asp-area"></a>asp-area

[asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) 屬性設定區域名稱，用以設定合適的路由。 下列範例描述了區域屬性如何造成路由重新對應。 將 `asp-area` 設定為 "Blogs" 會在 此錨點標籤之相關控制器和檢視的路由前面加上目錄 *Areas/Blogs*。

* **<專案名稱\>**
  * **wwwroot**
  * **區域**
    * **部落格**
      * **控制器**
        * *HomeController.cs*
      * **檢視**
        * **Home**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **控制器**

以上述目錄階層為例，要參考 *AboutBlog.cshtml* 檔案的標記即為：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

產生的 HTML：

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> 若要讓區域在 MVC 應用程式中正常運作，路由範本必須包含區域參考 (若其存在)。 該範本將以 *Startup.Configure* 中 `routes.MapRoute` 方法呼叫的第二個參數表示：[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

[asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) 屬性用於在您的 URL 中指定通訊協定 (例如 `https`)。 例如: 

[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]

產生的 HTML：

```html
<a href="https://localhost/Home/About">About</a>
```

此範例中的主機名稱為 localhost，但錨點標籤協助程式在產生 URL 時使用網站的公用網域。

## <a name="asp-host"></a>asp-host

[asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) 屬性用於在您的 URL 中指定主機名稱。 例如: 

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

產生的 HTML：

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

[asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) 屬性與 Razor 頁面搭配使用。 您可用其將錨點標籤的 `href` 屬性值設定為特定頁面。 在頁面名稱的開頭加上正斜線 ("/") 即可建立 URL。

下列範例指向出席者 Razor 頁面：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

產生的 HTML：

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` 屬性與 `asp-route`、`asp-controller` 以及 `asp-action` 屬性互斥。 但是，`asp-page` 可以搭配 `asp-route-{value}` 使用來控制路由，如下列標記所示：

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

產生的 HTML：

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

[asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) 屬性與 Razor 頁面搭配使用。 其用途為建立特定頁面處理常式的連結。

請考慮下列頁面處理常式：

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

頁面模型的相關標記會連結到 `OnGetProfile` 頁面處理常式。 請注意，`asp-page-handler` 屬性值中會省略頁面處理常式方法名稱的 `On<Verb>` 首碼。 如果這是非同步方法，則 `Async` 尾碼也會一併省略。

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

產生的 HTML：

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>其他資源

* [區域](xref:mvc/controllers/areas)
* [Razor 頁面簡介](xref:mvc/razor-pages/index)
