---
title: 設定 ASP.NET Core Blazor 的連結器
author: guardrex
description: 瞭解如何在建立 Blazor 應用程式時，控制中繼語言（IL）連結器。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 0bc987d72d2f684b1ecbd4a883e9a09fac7c801e
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317283"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="3e42d-103">設定 ASP.NET Core Blazor 的連結器</span><span class="sxs-lookup"><span data-stu-id="3e42d-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="3e42d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3e42d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="3e42d-105"> 會在組建期間執行[中繼語言（IL）](/dotnet/standard/managed-code#intermediate-language--execution)連結，以從應用程式的輸出元件中移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="3e42d-105"> performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="3e42d-106">請使用下列其中一種方法來控制組件連結：</span><span class="sxs-lookup"><span data-stu-id="3e42d-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="3e42d-107">使用 [MSBuild 屬性](#disable-linking-with-a-msbuild-property)來全域停用連結。</span><span class="sxs-lookup"><span data-stu-id="3e42d-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="3e42d-108">使用[設定檔](#control-linking-with-a-configuration-file)來根據組件控制連結。</span><span class="sxs-lookup"><span data-stu-id="3e42d-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="3e42d-109">使用 MSBuild 屬性來停用連結</span><span class="sxs-lookup"><span data-stu-id="3e42d-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="3e42d-110">建立應用程式時，預設會啟用連結，其中包括發佈。</span><span class="sxs-lookup"><span data-stu-id="3e42d-110">Linking is enabled by default when an app is built, which includes publishing.</span></span> <span data-ttu-id="3e42d-111">若要停用所有組件的連結，請在專案檔中將 `BlazorLinkOnBuild` MSBuild 屬性設為 `false`：</span><span class="sxs-lookup"><span data-stu-id="3e42d-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="3e42d-112">使用組態檔控制連結</span><span class="sxs-lookup"><span data-stu-id="3e42d-112">Control linking with a configuration file</span></span>

<span data-ttu-id="3e42d-113">透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：</span><span class="sxs-lookup"><span data-stu-id="3e42d-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="3e42d-114">*Linker.xml*：</span><span class="sxs-lookup"><span data-stu-id="3e42d-114">*Linker.xml*:</span></span>

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
      Fixes: https://github.com/aspnet/Blazor/issues/239
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

<span data-ttu-id="3e42d-115">如需詳細資訊，請參閱[IL 連結器： xml 描述元的語法](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor)。</span><span class="sxs-lookup"><span data-stu-id="3e42d-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="3e42d-116">設定國際化的連結器</span><span class="sxs-lookup"><span data-stu-id="3e42d-116">Configure the linker for internationalization</span></span>

<span data-ttu-id="3e42d-117">根據預設，Blazor WebAssembly 應用程式的 Blazor連結器設定會去除國際化資訊，但不包括明確要求的地區設定。</span><span class="sxs-lookup"><span data-stu-id="3e42d-117">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="3e42d-118">移除這些元件會將應用程式的大小降到最低。</span><span class="sxs-lookup"><span data-stu-id="3e42d-118">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="3e42d-119">若要控制要保留哪些國際化元件，請在專案檔中設定 `<MonoLinkerI18NAssemblies>` MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="3e42d-119">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="3e42d-120">區域值</span><span class="sxs-lookup"><span data-stu-id="3e42d-120">Region Value</span></span>     | <span data-ttu-id="3e42d-121">Mono 區域元件</span><span class="sxs-lookup"><span data-stu-id="3e42d-121">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="3e42d-122">包含的所有元件</span><span class="sxs-lookup"><span data-stu-id="3e42d-122">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="3e42d-123">*I18N.CJK .dll*</span><span class="sxs-lookup"><span data-stu-id="3e42d-123">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="3e42d-124">*I18N.務必 .dll*</span><span class="sxs-lookup"><span data-stu-id="3e42d-124">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="3e42d-125">`none` (預設)</span><span class="sxs-lookup"><span data-stu-id="3e42d-125">`none` (default)</span></span> | <span data-ttu-id="3e42d-126">無</span><span class="sxs-lookup"><span data-stu-id="3e42d-126">None</span></span>                    |
| `other`          | <span data-ttu-id="3e42d-127">*I18N.其他 .dll*</span><span class="sxs-lookup"><span data-stu-id="3e42d-127">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="3e42d-128">*I18N.罕見的 .dll*</span><span class="sxs-lookup"><span data-stu-id="3e42d-128">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="3e42d-129">*I18N.West .dll*</span><span class="sxs-lookup"><span data-stu-id="3e42d-129">*I18N.West.dll*</span></span>         |

<span data-ttu-id="3e42d-130">使用逗號來分隔多個值（例如，`mideast,west`）。</span><span class="sxs-lookup"><span data-stu-id="3e42d-130">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="3e42d-131">如需詳細資訊，請參閱[I18N： Pnetlib 國際化架構程式庫（mono/Mono GitHub 存放庫）](https://github.com/mono/mono/tree/master/mcs/class/I18N)。</span><span class="sxs-lookup"><span data-stu-id="3e42d-131">For more information, see [I18N: Pnetlib Internationalization Framework Libary (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
