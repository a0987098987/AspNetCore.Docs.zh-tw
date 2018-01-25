---
title: "移轉設定"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 51665059d1d803cbe57bc9a884a0e91eac9e7cb4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-configuration"></a><span data-ttu-id="fb905-102">移轉設定</span><span class="sxs-lookup"><span data-stu-id="fb905-102">Migrating Configuration</span></span>

<span data-ttu-id="fb905-103">由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="fb905-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="fb905-104">前一個發行項，開始[將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC](mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="fb905-104">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="fb905-105">在本文中，我們可以移轉設定。</span><span class="sxs-lookup"><span data-stu-id="fb905-105">In this article, we migrate configuration.</span></span>

<span data-ttu-id="fb905-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb905-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="fb905-107">安裝程式組態</span><span class="sxs-lookup"><span data-stu-id="fb905-107">Setup Configuration</span></span>

<span data-ttu-id="fb905-108">不會再使用 ASP.NET Core *Global.asax*和*web.config*舊版 ASP.NET 所使用的檔案。</span><span class="sxs-lookup"><span data-stu-id="fb905-108">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="fb905-109">在舊版 ASP.NET 中，應用程式啟動邏輯已放置在`Application_StartUp`方法內*Global.asax*。</span><span class="sxs-lookup"><span data-stu-id="fb905-109">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="fb905-110">接著，在 ASP.NET MVC *Startup.cs*檔案會包含在專案的根目錄，和應用程式啟動時呼叫。</span><span class="sxs-lookup"><span data-stu-id="fb905-110">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="fb905-111">ASP.NET Core 已完全採用這種方法，藉由放置在所有的啟動邏輯*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="fb905-111">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="fb905-112">*Web.config*也已取代檔案中 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="fb905-112">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="fb905-113">設定本身可以現在設定，做為應用程式的啟動程序中所述的一部分*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="fb905-113">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="fb905-114">組態仍然可以利用 XML 檔案，但通常 ASP.NET Core 專案將組態值的檔案中放置 JSON 格式，例如*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="fb905-114">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="fb905-115">ASP.NET Core 組態系統可以也很容易存取環境變數，可提供更安全完善的位置環境專屬的值。</span><span class="sxs-lookup"><span data-stu-id="fb905-115">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="fb905-116">這是特別有用，例如連接字串和不應該簽入原始檔控制的 API 金鑰的密碼。</span><span class="sxs-lookup"><span data-stu-id="fb905-116">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="fb905-117">請參閱[組態](xref:fundamentals/configuration/index)若要深入了解 ASP.NET Core 中組態。</span><span class="sxs-lookup"><span data-stu-id="fb905-117">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="fb905-118">如本文中，我們開始使用中的部分移轉 ASP.NET Core 專案[前一篇文章](mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="fb905-118">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="fb905-119">安裝程式組態，將下列建構函式和屬性加入至*Startup.cs*專案的根目錄中找到的檔案：</span><span class="sxs-lookup"><span data-stu-id="fb905-119">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="fb905-120">請注意，此時， *Startup.cs*檔案將不會進行編譯，因為我們還是需要新增下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="fb905-120">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="fb905-121">新增*appsettings.json*使用適當的項目範本的專案根目錄的檔案：</span><span class="sxs-lookup"><span data-stu-id="fb905-121">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![新增 AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="fb905-123">從 web.config 移轉組態設定</span><span class="sxs-lookup"><span data-stu-id="fb905-123">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="fb905-124">我們的 ASP.NET MVC 專案包含必要的資料庫連接字串中*web.config*，請在`<connectionStrings>`項目。</span><span class="sxs-lookup"><span data-stu-id="fb905-124">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="fb905-125">在 ASP.NET Core 專案中，我們將此資訊儲存在*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="fb905-125">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="fb905-126">開啟*appsettings.json*，並注意它已經包含下列：</span><span class="sxs-lookup"><span data-stu-id="fb905-126">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="fb905-127">在上面所述的反白顯示列，請變更資料庫的名稱**_CHANGE_ME**至您的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="fb905-127">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="fb905-128">總結</span><span class="sxs-lookup"><span data-stu-id="fb905-128">Summary</span></span>

<span data-ttu-id="fb905-129">ASP.NET Core 置於單一檔案，以必要的服務和相依性可以定義，以及設定的所有應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="fb905-129">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="fb905-130">它會取代*web.config*彈性設定功能，可以運用各種不同的檔案格式，例如 JSON，如環境變數的檔案。</span><span class="sxs-lookup"><span data-stu-id="fb905-130">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
