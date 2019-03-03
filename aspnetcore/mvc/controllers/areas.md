---
title: ASP.NET Core 中的區域
author: rick-anderson
description: 了解其為 ASP.NET MVC 功能的區域，如何用來將相關功能組織成群組，作為個別命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833523"
---
# <a name="areas-in-aspnet-core"></a>ASP.NET Core 中的區域

作者：[Dhananjay Kumar](https://twitter.com/debug_mode) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

區域是 ASP.NET 功能，可用來將相關功能組織為群組，以作為個別的命名空間 (適用於路由) 和資料夾結構 (適用於檢視)。 使用區域可基於路由的目的，藉由將另一個路由參數 `area` 新增至 `controller` 和 `action` 或 Razor 頁面 `page` 來建立階層。

區域可提供一種方式來將 ASP.NET Core Web 應用程式分割成較小的功能群組，每個都有一組自己的 Razor Pages、控制器、檢視和模型。 一個區域基本上是應用程式內的一個結構。 在 ASP.NET Core Web 專案中，Pages、模型、控制器和檢視等邏輯元件都會保留在不同的資料夾中。 ASP.NET Core 執行階段會使用命名慣例來建立這些元件之間的關聯性。 針對大型應用程式，將應用程式分割成個別高功能層級區域可能較有利。 舉例來說，一個電子商務應用程式可具有多個業務單位，例如結帳、計費和搜尋。 這其中的每個單位都有自己的區域，以包含檢視、控制器、Razor Pages 和模型。

處於下列情況時，請考慮在專案中使用區域：

* 應用程式是由可以邏輯方式區隔的多個高階功能性元件所組成的。
* 您想要分割應用程式，讓每個功能區域都可獨立運作。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([如何下載](xref:index#how-to-download-a-sample))。 下載範例提供基本的應用程式來測試區域。

## <a name="areas-for-controllers-with-views"></a>適用於控制器與檢視的區域

使用區域、控制器及檢視的典型 ASP.NET Core Web 應用程式包含下列項目：

* 一個[區域資料夾結構](#area-folder-structure)。
* 使用 [&lbrack;Area&rbrack;](#attribute) 屬性裝飾的控制器可使控制器與區域產生關聯：[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]
* [已新增至啟動的區域路由](#add-area-route)：[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

## <a name="area-folder-structure"></a>區域資料夾結構
假設應用程式具有兩個邏輯群組：「產品」和「服務」。 使用區域，資料夾結構應該如下：

* Project name
  * 區域
    * 產品
      * Controllers
        * HomeController.cs
        * ManageController.cs
      * 檢視
        * 首頁
          * Index.cshtml
        * 管理
          * Index.cshtml
          * About.cshtml
    * 服務
      * Controllers
        * HomeController.cs
      * 檢視
        * 首頁
          * Index.cshtml

儘管上述配置通常會在使用區域時使用，但是只需有檢視檔案，就能使用此資料夾結構。 檢視探索會依下列順序搜尋相符的區域檢視檔案：

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

非檢視資料夾的位置 (例如「控制站」和「模型」) 並**不**重要。 例如，「控制站」和「模型」資料夾並非必要項。 「控制器」和「模型」的內容都是要編譯為 .dll 的程式碼。 「檢視」的內容要在向該檢視發出要求之後才會編譯。

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a>使控制器與區域產生關聯

區域控制器會透過 [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) 屬性來指定：

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a>新增區域路由

區域路由通常會使用慣例路由，而非屬性路由。 慣例路由與順序息息相關。 一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。

如果 URL 空間在所有區域中都是統一的，則可使用 `{area:...}` 作為路由範本中的語彙基元：

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

在上述程式碼中，`exists` 會套用路由必須與區域相符的條件約束。 若要將路由新增至區域，使用 `{area:...}` 是最簡單的機制。

下列程式碼會使用 <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> 來建立兩個具名的區域路由：

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

搭配 ASP.NET Core 2.2 使用 `MapAreaRoute` 時，請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/7772) \(英文\)。

如需詳細資訊，請參閱[區域路由](xref:mvc/controllers/routing#areas)。

### <a name="link-generation-with-areas"></a>使用區域產生連結

以下來自[範例下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)的程式碼會顯示使用指定的區域來產生連結：

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

使用上述程式碼產生的連結，在應用程式中的任何位置都是有效的。

範例下載包括[部分檢視](xref:mvc/views/partial)，其中可在未指定區域的情況下包含上述連結和相同連結。 部分檢視會在[配置檔案]()中進行參考，因此，應用程式中的每個頁面都會顯示產生的連結。 在未指定區域的情況下產生的連結，只有在從相同區域與控制器中的頁面進行參考時才有效。

未指定區域或控制站時，路由即會取決於「環境」值。 目前要求的目前路由值被視為用於連結產生的環境值。 在許多適用於範例應用程式的案例中，使用環境值會產生不正確的連結。

如需詳細資訊，請參閱[路由至控制器動作](xref:mvc/controllers/routing)。

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a>使用 _ViewStart.cshtml 檔案共用區域的配置

若要針對整個應用程式共用通用的配置，請將 *_ViewStart.cshtml* 移至應用程式根資料夾。

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a>變更檢視儲存所在的預設區域資料夾

下列程式碼會將預設的區域資料夾從 `"Areas"` 變更為 `"MyAreas"`：

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a>發行區域

*.csproj* 檔案中包含 `<Project Sdk="Microsoft.NET.Sdk.Web">` 時，所有 `*.cshtml` 和 `wwwroot/**` 檔案都會發行至輸出。
