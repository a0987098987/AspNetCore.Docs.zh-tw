---
title: 針對 ASP.NET Core 當地語系化進行疑難排解
author: hishamco
description: 了解如何診斷 ASP.NET Core 應用程式的當地語系化問題。
ms.date: 01/24/2019
uid: fundamentals/troubleshoot-aspnet-core-localization
ms.openlocfilehash: 73405f539c89d79096e7b536407cd9730679d478
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488989"
---
# <a name="troubleshoot-aspnet-core-localization"></a><span data-ttu-id="eaaad-103">針對 ASP.NET Core 當地語系化進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="eaaad-103">Troubleshoot ASP.NET Core Localization</span></span>

<span data-ttu-id="eaaad-104">作者為 [Hisham Bin Ateya](https://github.com/hishamco)</span><span class="sxs-lookup"><span data-stu-id="eaaad-104">By [Hisham Bin Ateya](https://github.com/hishamco)</span></span>

<span data-ttu-id="eaaad-105">本文提供如何診斷 ASP.NET Core 應用程式當地語系化問題的指示。</span><span class="sxs-lookup"><span data-stu-id="eaaad-105">This article provides instructions on how to diagnose ASP.NET Core app localization issues.</span></span>

## <a name="localization-configuration-issues"></a><span data-ttu-id="eaaad-106">當地語系化組態問題</span><span class="sxs-lookup"><span data-stu-id="eaaad-106">Localization configuration issues</span></span>

<span data-ttu-id="eaaad-107">**當地語系化中介軟體順序**</span><span class="sxs-lookup"><span data-stu-id="eaaad-107">**Localization middleware order**</span></span>  
<span data-ttu-id="eaaad-108">應用程式可能會因為當地語系化中介軟體並未如預期般排序而無法當地語系化。</span><span class="sxs-lookup"><span data-stu-id="eaaad-108">The app may not localize because the localization middleware isn't ordered as expected.</span></span>

<span data-ttu-id="eaaad-109">若要解決此問題，請確定當地語系化中介軟體的註冊順序早於 MVC 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="eaaad-109">To resolve this issue, ensure that localization middleware is registered before MVC middleware.</span></span> <span data-ttu-id="eaaad-110">否則不會套用當地語系化中介軟體。</span><span class="sxs-lookup"><span data-stu-id="eaaad-110">Otherwise, the localization middleware isn't applied.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

<span data-ttu-id="eaaad-111">**找不到當地語系化資源路徑**</span><span class="sxs-lookup"><span data-stu-id="eaaad-111">**Localization resources path not found**</span></span>

<span data-ttu-id="eaaad-112">**RequestCultureProvider 中支援的文化特性 (Culture) 與註冊的不相符**</span><span class="sxs-lookup"><span data-stu-id="eaaad-112">**Supported Cultures in RequestCultureProvider don't match with registered once**</span></span>  

## <a name="resource-file-naming-issues"></a><span data-ttu-id="eaaad-113">資源檔命名問題</span><span class="sxs-lookup"><span data-stu-id="eaaad-113">Resource file naming issues</span></span>

<span data-ttu-id="eaaad-114">ASP.NET Core 已為當地語系化資源檔命名預先定義了規則與方針，其詳細說明請參閱[此處](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)。</span><span class="sxs-lookup"><span data-stu-id="eaaad-114">ASP.NET Core has predefined rules and guidelines for localization resources file naming, which are described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span></span>

## <a name="missing-resources"></a><span data-ttu-id="eaaad-115">缺少資源</span><span class="sxs-lookup"><span data-stu-id="eaaad-115">Missing resources</span></span>

<span data-ttu-id="eaaad-116">找不到資源的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="eaaad-116">Common causes of resources not being found include:</span></span>

- <span data-ttu-id="eaaad-117">資源名稱在 `resx` 檔案或當地語系化工具要求中拼錯。</span><span class="sxs-lookup"><span data-stu-id="eaaad-117">Resource names are misspelled in either the `resx` file or the localizer request.</span></span>
- <span data-ttu-id="eaaad-118">某些語言的 `resx` 中缺少這項資源，但其他語言則有。</span><span class="sxs-lookup"><span data-stu-id="eaaad-118">The resource is missing from the `resx` for some languages, but exists in others.</span></span>
- <span data-ttu-id="eaaad-119">如果您仍持續發生問題，請查看當地語系化記錄訊息 (在 `Debug` 記錄層級)，以獲取所缺少資源的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="eaaad-119">If you're still having trouble, check the localization log messages (which are at `Debug` log level) for more details about the missing resources.</span></span>

<span data-ttu-id="eaaad-120">**提示：** 當使用 `CookieRequestCultureProvider` 時，請確認當地語系化 Cookie 值中的文化特性 (Culture) 未使用單引號。例如，`c='en-UK'|uic='en-US'` 是無效的 Cookie 值，而 `c=en-UK|uic=en-US` 則有效。</span><span class="sxs-lookup"><span data-stu-id="eaaad-120">_**Hint:** When using `CookieRequestCultureProvider`, verify single quotes are not used with the cultures inside the localization cookie value. For example, `c='en-UK'|uic='en-US'` is an invalid cookie value, while `c=en-UK|uic=en-US` is a valid._</span></span>

## <a name="resources--class-libraries-issues"></a><span data-ttu-id="eaaad-121">資源與類別庫的問題</span><span class="sxs-lookup"><span data-stu-id="eaaad-121">Resources & Class Libraries issues</span></span>

<span data-ttu-id="eaaad-122">ASP.NET Core 根據預設會提供讓類別庫能透過 [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1) 找到資源檔的方法。</span><span class="sxs-lookup"><span data-stu-id="eaaad-122">ASP.NET Core by default provides a way to allow the class libraries to find their resource files via [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="eaaad-123">類別庫的常見問題包括：</span><span class="sxs-lookup"><span data-stu-id="eaaad-123">Common issues with class libraries include:</span></span>
- <span data-ttu-id="eaaad-124">類別庫中缺少 `ResourceLocationAttribute` 會導致 `ResourceManagerStringLocalizerFactory` 找不到資源。</span><span class="sxs-lookup"><span data-stu-id="eaaad-124">Missing the `ResourceLocationAttribute` in a class library will prevent `ResourceManagerStringLocalizerFactory` from discovering the resources.</span></span>
- <span data-ttu-id="eaaad-125">資源檔命名。</span><span class="sxs-lookup"><span data-stu-id="eaaad-125">Resource file naming.</span></span> <span data-ttu-id="eaaad-126">如需詳細資訊，請參閱[資源檔命名問題](#resource-file-naming-issues)一節。</span><span class="sxs-lookup"><span data-stu-id="eaaad-126">For more information, see [Resource file naming issues](#resource-file-naming-issues) section.</span></span>
- <span data-ttu-id="eaaad-127">變更類別庫的根命名空間。</span><span class="sxs-lookup"><span data-stu-id="eaaad-127">Changing the root namespace of the class library.</span></span> <span data-ttu-id="eaaad-128">如需詳細資訊，請參閱[根命名空間問題](#root-namespace-issues)一節。</span><span class="sxs-lookup"><span data-stu-id="eaaad-128">For more information, see [Root Namespace issues](#root-namespace-issues) section.</span></span>

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a><span data-ttu-id="eaaad-129">CustomRequestCultureProvider 未如預期運作</span><span class="sxs-lookup"><span data-stu-id="eaaad-129">CustomRequestCultureProvider doesn't work as expected</span></span>

<span data-ttu-id="eaaad-130">`RequestLocalizationOptions` 類別有三個預設提供者：</span><span class="sxs-lookup"><span data-stu-id="eaaad-130">The `RequestLocalizationOptions` class has three default providers:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="eaaad-131">[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) 可讓您自訂如何在應用程式中提供當地語系化文化特性 (Culture)。</span><span class="sxs-lookup"><span data-stu-id="eaaad-131">The [CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) allows you to customize how the localization culture is provided in your app.</span></span> <span data-ttu-id="eaaad-132">當預設提供者不符合您的需求時，會使用 `CustomRequestCultureProvider`。</span><span class="sxs-lookup"><span data-stu-id="eaaad-132">The `CustomRequestCultureProvider` is used when the default providers don't meet your requirements.</span></span>

- <span data-ttu-id="eaaad-133">自訂提供者無法正常運作的常見原因在於它不是 `RequestCultureProviders` 清單中的第一個提供者。</span><span class="sxs-lookup"><span data-stu-id="eaaad-133">A common reason custom provider don't work properly is that it isn't the first provider in the `RequestCultureProviders` list.</span></span> <span data-ttu-id="eaaad-134">若要解決此問題︰</span><span class="sxs-lookup"><span data-stu-id="eaaad-134">To resolve this issue:</span></span>

- <span data-ttu-id="eaaad-135">將自訂提供者插入 `RequestCultureProviders` 清單中的位置 0，如下所示：</span><span class="sxs-lookup"><span data-stu-id="eaaad-135">Insert the custom provider at the position 0 in the `RequestCultureProviders` list as the following:</span></span>

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- <span data-ttu-id="eaaad-136">使用 `AddInitialRequestCultureProvider` 擴充方法將自訂提供者設定為初始提供者。</span><span class="sxs-lookup"><span data-stu-id="eaaad-136">Use `AddInitialRequestCultureProvider` extension method to set the custom provider as initial provider.</span></span>

## <a name="root-namespace-issues"></a><span data-ttu-id="eaaad-137">根命名空間問題</span><span class="sxs-lookup"><span data-stu-id="eaaad-137">Root Namespace issues</span></span>

<span data-ttu-id="eaaad-138">當組件的根命名空間與組件名稱不同時，當地語系化根據預設無法運作。</span><span class="sxs-lookup"><span data-stu-id="eaaad-138">When the root namespace of an assembly is different than the assembly name, localization doesn't work by default.</span></span> <span data-ttu-id="eaaad-139">若要避免此問題，請使用 [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1)，其詳細說明請參閱[這裡](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span><span class="sxs-lookup"><span data-stu-id="eaaad-139">To avoid this issue use [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), which is described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span></span>

## <a name="resources--build-action"></a><span data-ttu-id="eaaad-140">資源與建置動作</span><span class="sxs-lookup"><span data-stu-id="eaaad-140">Resources & Build Action</span></span>

<span data-ttu-id="eaaad-141">如果您使用資源檔進行當地語系化，務必讓他們有適當的建置動作。</span><span class="sxs-lookup"><span data-stu-id="eaaad-141">If you use resource files for localization, it's important that they have an appropriate build action.</span></span> <span data-ttu-id="eaaad-142">它們必須是**內嵌資源**，否則 `ResourceStringLocalizer` 找不到這些資源。</span><span class="sxs-lookup"><span data-stu-id="eaaad-142">They should be **Embedded Resource**, otherwise the `ResourceStringLocalizer` is not able to find these resources.</span></span>
