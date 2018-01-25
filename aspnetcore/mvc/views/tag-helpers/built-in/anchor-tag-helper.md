---
title: "錨定標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用錨定標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 74609b515936ec7da8bfc133c27cb69f51311924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="anchor-tag-helper"></a>錨定標記協助程式

由 [Peter Kellner](http://peterkellner.net) 提供 

錨定標記協助程式增強 HTML 錨定 (`<a ... ></a>`) 藉由加入新屬性的標記。 所產生的連結 (在`href`標記) 會建立使用新的屬性。 該 URL 可以包含選擇性通訊協定，例如 https。

在這份文件中的範例中用於下列的喇叭控制器。

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>錨定標記協助程式屬性

### <a name="asp-controller"></a>asp-controller

`asp-controller`用來建立關聯的控制器將會用來產生 URL。 指定的控制器必須存在於目前的專案。 下列程式碼列出所有的喇叭： 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

將產生的標記：

```html
<a href="/Speaker">All Speakers</a>
```

如果`asp-controller`指定和`asp-action`不是預設`asp-action`將目前正在執行檢視的預設控制器方法。 是，在上述範例中，如果`asp-action`被省略，而且此錨定標記協助程式從產生的*HomeController*的`Index`檢視 (**/首頁**)，將會產生的標記：

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action`將會包含控制器動作方法的名稱在產生`href`。 例如，下列程式碼產生設定`href`指向喇叭詳細資料頁面：

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

將產生的標記：

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

如果沒有`asp-controller`屬性已指定，會使用預設的控制站呼叫執行目前檢視的檢視。  
 
如果屬性`asp-action`是`Index`，然後採取任何動作會不附加至的 URL，導致預設`Index`所呼叫方法。 動作指定 （或預設），必須在控制器中參考`asp-controller`。

### <a name="asp-page"></a>asp-page

使用`asp-page`屬性中設定其 URL，以指向特定頁面的錨點標籤。 以正斜線的頁面名稱前面加上"/"來建立 URL。 下面範例中的 URL 會指向目前的目錄中的"喇叭 」 頁面。

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

`asp-page`屬性在上述程式碼範例中的呈現 HTML 輸出中的檢視，類似於下列程式碼片段：

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

`asp-page`屬性是互斥的`asp-route`， `asp-controller`，和`asp-action`屬性。 不過，`asp-page`可以搭配`asp-route-id`來控制路由，如下列程式碼範例所示：

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id`會產生下列輸出：

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> 若要使用`asp-page`Razor 頁面中，Url 屬性必須是相對路徑，例如`"./Speaker"`。 中的相對路徑`asp-page`屬性不適用於 MVC 檢視。 請改用 MVC 檢視的"/"語法。

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-`是萬用字元路由前置詞。 尾端的虛線會被解譯為潛在的路由參數之後，您將任何值。 如果找不到預設路由，此路由前置字元會附加至要求參數和值為產生的 href。 否則將會取代中的路由範本。

假設您有控制器方法定義，如下所示：

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

而且有預設路由範本中定義您*Startup.cs* ，如下所示：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

**Cshtml**檔案，其中包含錨定標記協助程式需要使用**喇叭**傳入從控制器到檢視的模型參數如下所示：

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

產生的 HTML 會如下所示因為**識別碼**預設路由中找不到。

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

如果路由前置字元不屬於找到的路由範本中，也就是以下列**cshtml**檔案：

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

產生的 HTML 會如下所示因為**speakerid**找不到相符的路由中：

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

如果有任一個`asp-controller`或`asp-action`未指定，則相同的預設處理後，因為處於`asp-route`屬性。

### <a name="asp-route"></a>asp-route

`asp-route`可用來建立直接連結到具名路由的 URL。 使用路由的屬性，路由可以具名中所示`SpeakerController`和用於其`Evaluations`方法。

`Name = "speakerevals"`會告知錨定標記協助程式，來產生該控制器方法，使用 URL 直接路由`/Speaker/Evaluations`。 如果`asp-controller`或`asp-action`指定除了`asp-route`，產生的路由可能不如預期。 `asp-route`不應使用屬性的其中一種`asp-controller`或`asp-action`為避免發生路由衝突。

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data`允許建立所在的索引鍵是參數名稱和值是該索引鍵相關聯的值的機碼值組的字典。

如下範例所示，內嵌字典會建立並將資料傳遞到 razor 檢視。 或者，資料無法傳遞入您的模型。

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

上述程式碼會產生下列 URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

按一下連結時，控制器方法`EvaluationsCurrent`呼叫。 因為該控制器有兩個字串參數相符項目已從建立呼叫`asp-all-route-data`字典。

如果任何索引鍵字典比對路由的參數，這些值將會由適當地路由和其他不相符的值將會產生做為要求參數。

### <a name="asp-fragment"></a>asp 片段

`asp-fragment`定義要附加至 URL 的 URL 片段。 錨定標記協助程式，將雜湊字元 （#）。 如果您建立的標記：

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

將產生的 URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations

建置用戶端應用程式時，適合使用雜湊標記。 它們可以用於簡單的標記，並在 JavaScript 中，例如搜尋。

### <a name="asp-area"></a>asp-area

`asp-area`設定 ASP.NET Core 使用來設定適當的路由的區域名稱。 以下是如何區域屬性會造成重新對應路由的範例。 設定`asp-area`部落格的前置詞目錄`Areas/Blogs`路由相關聯的控制器與檢視此錨定標記。

* 專案名稱
  * wwwroot
  * 區域
    * 部落格
      * 控制站
        * HomeController.cs
      * 檢視
        * 首頁
          * Index.cshtml
          * AboutBlog.cshtml
  * 控制站

指定有效的例如區域標記```area="Blogs"```參考時```AboutBlog.cshtml```檔案將會如下所示使用錨定標記協助程式。

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

產生的 HTML 將包含區域區段，並會顯示如下：

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> MVC web 應用程式中工作的區域，如果它存在路由範本時，必須包含區域的參考。 該範本，也就是第二個參數的`routes.MapRoute`呼叫方法時，將會顯示為：`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

`asp-protocol`可用於指定通訊協定 (例如`https`) 在您的 URL。 範例包含通訊協定的錨定標記協助程式看起來如下：

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

而且，將會產生 HTML，如下所示：

```<a href="https://localhost/Home/About">About</a>```

此範例中的網域為 localhost，但產生 URL 時，錨定標記協助程式會使用網站的公用網域。

## <a name="additional-resources"></a>其他資源

* [區域](xref:mvc/controllers/areas)
