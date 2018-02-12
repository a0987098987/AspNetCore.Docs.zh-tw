---
title: "ASP.NET Core 中的錨點標籤協助程式"
author: pkellner
description: "示範如何使用錨點標籤協助程式"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>錨點標籤協助程式

由 [Peter Kellner](http://peterkellner.net) 提供 

錨點標籤協助程式藉由新增新的屬性，來增強 HTML 錨點 (`<a ... ></a>`) 標籤。 產生的連結 (在 `href` 標籤上) 是使用新的屬性所建立。 該 URL 可包含選擇性通訊協定，例如 https。

本文件的所有範例中都會使用下列喇叭控制器。

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>錨點標籤協助程式屬性

### <a name="asp-controller"></a>asp-controller

`asp-controller` 可用來與用於產生 URL 的控制器建立關聯。 指定的控制器必須存在於目前的專案中。 下列程式碼列出所有喇叭： 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

產生的標記會是：

```html
<a href="/Speaker">All Speakers</a>
```

如果指定了 `asp-controller` 但未指定 `asp-action`，預設的 `asp-action` 會是目前執行檢視的預設控制器方法。 換句話說，在上述範例中，如果省略 `asp-action`，而且此錨點標籤協助程式是從 *HomeController* 的 `Index` 檢視 (**/Home**) 所產生，則產生的標記會是：

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` 是控制器中的動作方法名稱，將會包含在產生的 `href` 中。 例如，下列程式碼會設定產生的 `href` 以指向喇叭詳細資料頁面：

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

產生的標記會是：

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

如果未指定任何 `asp-controller` 屬性，則會使用呼叫檢視並執行目前檢視的預設控制器。  
 
如果屬性 `asp-action` 為 `Index`，則不會將任何動作附加至 URL，而導致呼叫預設的 `Index` 方法。 指定的動作 (或預設的動作) 必須存在於 `asp-controller` 所參考的控制器中。

### <a name="asp-page"></a>asp-page

在錨點標籤中使用 `asp-page` 屬性可設定其 URL 以指向特定頁面。 在頁面名稱前面加上正斜線 "/" 以建立 URL。 下面範例中的 URL 會指向目前目錄中的「喇叭」頁面。

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

上述程式碼範例中的 `asp-page` 屬性會在檢視中轉譯 HTML 輸出，如下列程式碼片段所示：

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

`asp-page` 屬性與 `asp-route`、`asp-controller` 以及 `asp-action` 屬性互斥。 不過，`asp-page` 可以搭配 `asp-route-id` 來控制路由，如下列程式碼範例所示：

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` 會產生下列輸出：

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> 若要在 Razor 頁面中使用 `asp-page` 屬性，URL 必須是相對路徑，例如 `"./Speaker"`。 `asp-page` 屬性中的相對路徑無法在 MVC 檢視中使用。 針對 MVC 檢視，請改用 "/" 語法。

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` 是路由前置萬用字元。 您放在行尾破折號後面的任何值都會解譯為潛在路由參數。 如果找不到預設路由，則會在產生的 href 附加此路由前置字元作為要求參數和值。 否則會在路由範本中加以取代。

假設您有定義如下的控制器方法：

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

並在 *Startup.cs* 中定義了預設路由範本，如下所示：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

下列 **cshtml** 檔案包含使用從控制器傳入檢視的 **speaker** 模型參數所需的錨點標籤協助程式：

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

由於預設路由中找不到 **id**，因此產生的 HTML 會如下所示。

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

如果路由前置字元不屬於所找到的路由範本，在本例中為下列 **cshtml** 檔案：

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

由於符合的路由中找不到 **speakerid**，因此產生的 HTML 會如下所示：

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

如果未指定 `asp-controller` 或 `asp-action`，則會遵循與 `asp-route` 屬性相同的預設處理。

### <a name="asp-route"></a>asp-route

`asp-route` 可讓您建立直接連結至具名路由的 URL。 使用路由屬性，路由可以如 `SpeakerController` 中所示命名並用於其 `Evaluations` 方法。

`Name = "speakerevals"` 會指示錨點標籤協助程式使用 URL `/Speaker/Evaluations`，來產生該控制器方法的直接路由。 如果除了 `asp-route`，還指定了 `asp-controller` 或 `asp-action`，產生的路由可能不如預期。 `asp-route` 不應該搭配屬性 `asp-controller` 或 `asp-action` 使用，以免發生路由衝突。

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` 可建立索引鍵/值組的字典，其中索引鍵是參數名稱，而值是與該索引鍵建立關聯的值。

如下範例所示，會建立內嵌字典並將資料傳遞至 Razor 檢視。 您也可以使用模型來傳入資料。

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

上述程式碼會產生下列 URL：http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

按一下連結時，會呼叫控制器方法 `EvaluationsCurrent`。 因為該控制器有兩個字串參數符合 `asp-all-route-data` 字典中已建立的字串參數，所以會呼叫此方法。

如果字典中有任何索引鍵符合路由參數，這些值會在路由中被適當取代，並會產生其他不相符的值作為要求參數。

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` 定義要附加至 URL 的 URL 片段。 錨點標籤協助程式會新增雜湊字元 (#)。 如果您建立一個標籤：

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

產生的 URL 會是：http://localhost/Speaker/Evaluations#SpeakerEvaluations

# 標籤在建置用戶端應用程式時會很有用。 例如，您可以使用這些標籤在 JavaScript 中輕鬆標記和搜尋。

### <a name="asp-area"></a>asp-area

`asp-area` 會設定 ASP.NET Core 用來設定適當路由的區域名稱。 以下是區域屬性如何造成路由重新對應的範例。 將 `asp-area` 設定為 Blogs 會在 此錨點標籤的相關聯控制器和檢視路由前面加上字典 `Areas/Blogs`。

* 專案名稱
  * wwwroot
  * 區域
    * 部落格
      * 控制器
        * HomeController.cs
      * 檢視
        * 首頁
          * Index.cshtml
          * AboutBlog.cshtml
  * 控制器

參考 ```AboutBlog.cshtml``` 檔案時指定有效的區域標籤 (例如 ```area="Blogs"```) 會類似使用下列的錨點標籤協助程式。

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

產生的 HTML 會包含區域區段，如下所示：

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> 若要讓 MVC 區域在 Web 應用程式中正常運作，路由範本必須包含區域的參考 (若存在)。 該範本 (也就是 `routes.MapRoute` 方法呼叫的第二個參數) 會顯示為：`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

`asp-protocol` 適用於在您的 URL 中指定通訊協定 (例如 `https`)。 包含通訊協定的範例錨點標籤協助程式如下所示：

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

並將產生 HTML，如下所示：

```<a href="https://localhost/Home/About">About</a>```

此範例中的網域為 localhost，但錨點標籤協助程式使用網站的公用網域來產生 URL。

## <a name="additional-resources"></a>其他資源

* [區域](xref:mvc/controllers/areas)
