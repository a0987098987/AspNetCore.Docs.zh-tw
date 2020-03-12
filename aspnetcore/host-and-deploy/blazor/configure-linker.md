---
title: 設定 ASP.NET Core Blazor 的連結器
author: guardrex
description: 瞭解如何在建立 Blazor 應用程式時，控制中繼語言（IL）連結器。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b08ec26fb8d139223c57774600bc3cb19a56ac49
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083297"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="f37ef-103">設定 ASP.NET Core Blazor 的連結器</span><span class="sxs-lookup"><span data-stu-id="f37ef-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="f37ef-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f37ef-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f37ef-105">Blazor WebAssembly 會在組建期間執行[中繼語言（IL）](/dotnet/standard/managed-code#intermediate-language--execution)連結，以從應用程式的輸出元件修剪不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="f37ef-105">Blazor WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="f37ef-106">在 Debug 設定中建立時，會停用連結器。</span><span class="sxs-lookup"><span data-stu-id="f37ef-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="f37ef-107">應用程式必須在發行設定中建立，才能啟用連結器。</span><span class="sxs-lookup"><span data-stu-id="f37ef-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="f37ef-108">我們建議您在部署 Blazor WebAssembly 應用程式時建立發行。</span><span class="sxs-lookup"><span data-stu-id="f37ef-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="f37ef-109">連結應用程式會優化大小，但可能會有不利的影響。</span><span class="sxs-lookup"><span data-stu-id="f37ef-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="f37ef-110">使用反映或相關動態功能的應用程式可能會在修剪時中斷，因為連結器不知道此動態行為，而且無法判斷在執行時間的反映需要何種類型。</span><span class="sxs-lookup"><span data-stu-id="f37ef-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="f37ef-111">若要修剪這類應用程式，連結器必須通知程式碼中的反映所需的任何類型，以及應用程式所相依的封裝或架構。</span><span class="sxs-lookup"><span data-stu-id="f37ef-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="f37ef-112">若要確保已修剪的應用程式在部署後能正常運作，請務必在開發時經常測試應用程式的發行組建。</span><span class="sxs-lookup"><span data-stu-id="f37ef-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="f37ef-113">您可以使用下列 MSBuild 功能來設定 Blazor 應用程式的連結：</span><span class="sxs-lookup"><span data-stu-id="f37ef-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="f37ef-114">使用[MSBuild 屬性](#control-linking-with-an-msbuild-property)來設定全域連結。</span><span class="sxs-lookup"><span data-stu-id="f37ef-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="f37ef-115">使用[設定檔](#control-linking-with-a-configuration-file)來根據組件控制連結。</span><span class="sxs-lookup"><span data-stu-id="f37ef-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="f37ef-116">使用 MSBuild 屬性的控制項連結</span><span class="sxs-lookup"><span data-stu-id="f37ef-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="f37ef-117">`Release` 組態內建應用程式時，會啟用連結。</span><span class="sxs-lookup"><span data-stu-id="f37ef-117">Linking is enabled when an app is built in `Release` configuation.</span></span> <span data-ttu-id="f37ef-118">若要變更此項，請在專案檔中設定 `BlazorWebAssemblyEnableLinking` MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="f37ef-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="f37ef-119">使用組態檔控制連結</span><span class="sxs-lookup"><span data-stu-id="f37ef-119">Control linking with a configuration file</span></span>

<span data-ttu-id="f37ef-120">透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：</span><span class="sxs-lookup"><span data-stu-id="f37ef-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="f37ef-121">*Linker.xml*：</span><span class="sxs-lookup"><span data-stu-id="f37ef-121">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="f37ef-122">如需詳細資訊，請參閱[IL 連結器： xml 描述元的語法](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor)。</span><span class="sxs-lookup"><span data-stu-id="f37ef-122">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="f37ef-123">設定國際化的連結器</span><span class="sxs-lookup"><span data-stu-id="f37ef-123">Configure the linker for internationalization</span></span>

<span data-ttu-id="f37ef-124">根據預設，Blazor WebAssembly 應用程式的 Blazor 連結器設定會去除國際化資訊，但不包括明確要求的地區設定。</span><span class="sxs-lookup"><span data-stu-id="f37ef-124">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="f37ef-125">移除這些元件會將應用程式的大小降到最低。</span><span class="sxs-lookup"><span data-stu-id="f37ef-125">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="f37ef-126">若要控制要保留哪些國際化元件，請在專案檔中設定 `<MonoLinkerI18NAssemblies>` MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="f37ef-126">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="f37ef-127">區域值</span><span class="sxs-lookup"><span data-stu-id="f37ef-127">Region Value</span></span>     | <span data-ttu-id="f37ef-128">Mono 區域元件</span><span class="sxs-lookup"><span data-stu-id="f37ef-128">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="f37ef-129">包含的所有元件</span><span class="sxs-lookup"><span data-stu-id="f37ef-129">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="f37ef-130">*I18N.CJK .dll*</span><span class="sxs-lookup"><span data-stu-id="f37ef-130">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="f37ef-131">*I18N.務必 .dll*</span><span class="sxs-lookup"><span data-stu-id="f37ef-131">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="f37ef-132">`none` (預設值)</span><span class="sxs-lookup"><span data-stu-id="f37ef-132">`none` (default)</span></span> | <span data-ttu-id="f37ef-133">無</span><span class="sxs-lookup"><span data-stu-id="f37ef-133">None</span></span>                    |
| `other`          | <span data-ttu-id="f37ef-134">*I18N.其他 .dll*</span><span class="sxs-lookup"><span data-stu-id="f37ef-134">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="f37ef-135">*I18N.罕見的 .dll*</span><span class="sxs-lookup"><span data-stu-id="f37ef-135">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="f37ef-136">*I18N.West .dll*</span><span class="sxs-lookup"><span data-stu-id="f37ef-136">*I18N.West.dll*</span></span>         |

<span data-ttu-id="f37ef-137">使用逗號來分隔多個值（例如，`mideast,west`）。</span><span class="sxs-lookup"><span data-stu-id="f37ef-137">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="f37ef-138">如需詳細資訊，請參閱[I18N： Pnetlib 國際化架構程式庫（mono/Mono GitHub 儲存機制）](https://github.com/mono/mono/tree/master/mcs/class/I18N)。</span><span class="sxs-lookup"><span data-stu-id="f37ef-138">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
