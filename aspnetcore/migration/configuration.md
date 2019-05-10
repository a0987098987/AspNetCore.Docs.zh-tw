---
title: 將設定移轉到 ASP.NET Core
author: ardalis
description: 了解如何將組態從 ASP.NET MVC 專案移轉至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: c379f1f64dc5ab8aeb48055124e86e4e60d93785
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894925"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="eba5e-103">將設定移轉到 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eba5e-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="eba5e-104">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="eba5e-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="eba5e-105">在上一篇文章中，我們已開始[將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC](xref:migration/mvc)。</span><span class="sxs-lookup"><span data-stu-id="eba5e-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="eba5e-106">在本文中，我們可以移轉設定。</span><span class="sxs-lookup"><span data-stu-id="eba5e-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="eba5e-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eba5e-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="eba5e-108">設定組態</span><span class="sxs-lookup"><span data-stu-id="eba5e-108">Setup configuration</span></span>

<span data-ttu-id="eba5e-109">ASP.NET Core 不會再使用*Global.asax*並*web.config*舊版 ASP.NET 使用的檔案。</span><span class="sxs-lookup"><span data-stu-id="eba5e-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="eba5e-110">在舊版 ASP.NET 中，應用程式啟動邏輯已放置在`Application_StartUp`方法內*Global.asax*。</span><span class="sxs-lookup"><span data-stu-id="eba5e-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="eba5e-111">稍後，在 ASP.NET MVC *Startup.cs*檔案會包含在專案中; 的根目錄，並啟動應用程式時，它所呼叫。</span><span class="sxs-lookup"><span data-stu-id="eba5e-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="eba5e-112">ASP.NET Core 已完全採用這種方法，藉由將放在所有的啟動邏輯*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="eba5e-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="eba5e-113">*Web.config*檔案也已取代 ASP.NET Core 中。</span><span class="sxs-lookup"><span data-stu-id="eba5e-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="eba5e-114">組態本身可以現在設定，做為應用程式啟動程序中所述的一部分*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="eba5e-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="eba5e-115">組態仍然可以利用 XML 檔案，但通常是 ASP.NET Core 專案都會將組態值置於 JSON 格式的檔案，例如*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="eba5e-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="eba5e-116">ASP.NET Core 組態系統也很容易可以存取環境變數，其可提供[更安全、 穩健的位置](xref:security/app-secrets)環境特定值。</span><span class="sxs-lookup"><span data-stu-id="eba5e-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="eba5e-117">這特別適用於祕密，例如連接字串和應該不會簽入原始檔控制的 API 金鑰。</span><span class="sxs-lookup"><span data-stu-id="eba5e-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="eba5e-118">請參閱[組態](xref:fundamentals/configuration/index)若要深入了解 ASP.NET Core 中的組態。</span><span class="sxs-lookup"><span data-stu-id="eba5e-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="eba5e-119">在本文中，我們將開始與部分已移轉的 ASP.NET Core 專案，從[前一篇文章](xref:migration/mvc)。</span><span class="sxs-lookup"><span data-stu-id="eba5e-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="eba5e-120">若要設定的組態，請新增下列建構函式和屬性，以*Startup.cs*檔案位於專案的根目錄：</span><span class="sxs-lookup"><span data-stu-id="eba5e-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="eba5e-121">請注意，此時， *Startup.cs*檔案不會編譯，因為我們仍需要加入下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="eba5e-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="eba5e-122">新增*appsettings.json*使用適當的項目範本的專案根目錄的檔案：</span><span class="sxs-lookup"><span data-stu-id="eba5e-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![將 AppSettings JSON 新增](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="eba5e-124">從 web.config 中移轉的組態設定</span><span class="sxs-lookup"><span data-stu-id="eba5e-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="eba5e-125">我們的 ASP.NET MVC 專案包含必要的資料庫連接字串中*web.config*，請在`<connectionStrings>`項目。</span><span class="sxs-lookup"><span data-stu-id="eba5e-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="eba5e-126">在 ASP.NET Core 專案中，我們將此資訊儲存在*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="eba5e-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="eba5e-127">開啟*appsettings.json*，並記下它已經包含下列：</span><span class="sxs-lookup"><span data-stu-id="eba5e-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="eba5e-128">在上述的反白顯示列，變更的資料庫名稱 **_CHANGE_ME**到您的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="eba5e-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="eba5e-129">總結</span><span class="sxs-lookup"><span data-stu-id="eba5e-129">Summary</span></span>

<span data-ttu-id="eba5e-130">ASP.NET Core 會將所有的啟動邏輯應用程式置於單一檔案，以必要的服務和相依性可定義和設定。</span><span class="sxs-lookup"><span data-stu-id="eba5e-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="eba5e-131">它會取代*web.config*可以利用各種不同的檔案格式，例如 JSON，如環境變數的彈性設定功能的檔案。</span><span class="sxs-lookup"><span data-stu-id="eba5e-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
