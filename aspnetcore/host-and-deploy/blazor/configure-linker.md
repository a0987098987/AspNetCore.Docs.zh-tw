---
title: 設定ASP.NET核心連結器Blazor
author: guardrex
description: 瞭解如何在構建Blazor應用時控制中間語言 (IL) 連結器。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 109da5ef400c3b9d64ccf3ceb33a5387ea6b5618
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218657"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="8d24b-103">設定 ASP.NET Core Blazor 的連結器</span><span class="sxs-lookup"><span data-stu-id="8d24b-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="8d24b-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d24b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="8d24b-105">Blazor WebAssembly 在生成期間執行[中間語言 (IL)](/dotnet/standard/managed-code#intermediate-language--execution)連結,以從應用的輸出程式集中修剪不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="8d24b-105">Blazor WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="8d24b-106">在調試配置中生成連結器時,連結器將禁用。</span><span class="sxs-lookup"><span data-stu-id="8d24b-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="8d24b-107">應用必須在發佈配置中生成才能啟用連結器。</span><span class="sxs-lookup"><span data-stu-id="8d24b-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="8d24b-108">我們建議在部署 Blazor WebAssembly 應用時在「發布」中構建。</span><span class="sxs-lookup"><span data-stu-id="8d24b-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="8d24b-109">鏈接應用會優化大小,但可能會造成不利影響。</span><span class="sxs-lookup"><span data-stu-id="8d24b-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="8d24b-110">使用反射或相關動態功能的應用在修剪時可能會中斷,因為連結器不知道此動態行為,並且通常無法確定運行時反射需要哪些類型。</span><span class="sxs-lookup"><span data-stu-id="8d24b-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="8d24b-111">要修剪此類應用,必須告知連結程式在代碼和程式所依賴的包或框架中反射所需的任何類型。</span><span class="sxs-lookup"><span data-stu-id="8d24b-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="8d24b-112">為了確保修剪的應用在部署後正常工作,在開發時經常測試應用的發佈版本非常重要。</span><span class="sxs-lookup"><span data-stu-id="8d24b-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="8d24b-113">可以使用以下 MSBuild 功能設定 Blazor 應用的連結:</span><span class="sxs-lookup"><span data-stu-id="8d24b-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="8d24b-114">使用[MSBuild 屬性](#control-linking-with-an-msbuild-property)配置全域連結。</span><span class="sxs-lookup"><span data-stu-id="8d24b-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="8d24b-115">使用[配置檔](#control-linking-with-a-configuration-file)控制每個程式集的連結。</span><span class="sxs-lookup"><span data-stu-id="8d24b-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="8d24b-116">使用 MSBuild 屬性進行連結</span><span class="sxs-lookup"><span data-stu-id="8d24b-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="8d24b-117">在配置中`Release`生成應用時啟用連結。</span><span class="sxs-lookup"><span data-stu-id="8d24b-117">Linking is enabled when an app is built in `Release` configuration.</span></span> <span data-ttu-id="8d24b-118">要變更此項目檔`BlazorWebAssemblyEnableLinking`中的 MSBuild 屬性:</span><span class="sxs-lookup"><span data-stu-id="8d24b-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="8d24b-119">使用組態檔控制連結</span><span class="sxs-lookup"><span data-stu-id="8d24b-119">Control linking with a configuration file</span></span>

<span data-ttu-id="8d24b-120">透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：</span><span class="sxs-lookup"><span data-stu-id="8d24b-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="LinkerConfig.xml" />
</ItemGroup>
```

<span data-ttu-id="8d24b-121">*連結康菲格.xml*:</span><span class="sxs-lookup"><span data-stu-id="8d24b-121">*LinkerConfig.xml*:</span></span>

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

<span data-ttu-id="8d24b-122">有關詳細資訊,請參閱連結[xml 檔範例(單聲道/連結器 GitHub 儲存庫)。](https://github.com/mono/linker#link-xml-file-examples)</span><span class="sxs-lookup"><span data-stu-id="8d24b-122">For more information, see [Link xml file examples (mono/linker GitHub repository)](https://github.com/mono/linker#link-xml-file-examples).</span></span>

## <a name="add-an-xml-linker-configuration-file-to-a-library"></a><span data-ttu-id="8d24b-123">新增 XML 連結器設定檔到函式庫</span><span class="sxs-lookup"><span data-stu-id="8d24b-123">Add an XML linker configuration file to a library</span></span>

<span data-ttu-id="8d24b-124">要設定特定庫的連結程式,請將 XML 連結器設定檔作為嵌入式資源添加到庫中。</span><span class="sxs-lookup"><span data-stu-id="8d24b-124">To configure the linker for a specific library, add an XML linker configuration file into the library as an embedded resource.</span></span> <span data-ttu-id="8d24b-125">嵌入的資源必須與程式集具有相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="8d24b-125">The embedded resource must have the same name as the assembly.</span></span>

<span data-ttu-id="8d24b-126">在下面的範例中 *,LinkerConfig.xml*檔指定為與函式庫程式集同名的嵌入式資源:</span><span class="sxs-lookup"><span data-stu-id="8d24b-126">In the following example, the *LinkerConfig.xml* file is specified as an embedded resource that has the same name as the library's assembly:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="LinkerConfig.xml">
    <LogicalName>$(MSBuildProjectName).xml</LogicalName>
  </EmbeddedResource>
</ItemGroup>
```

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="8d24b-127">設定用於國際化的連結器</span><span class="sxs-lookup"><span data-stu-id="8d24b-127">Configure the linker for internationalization</span></span>

<span data-ttu-id="8d24b-128">默認情況下,Blazor 針對 Blazor WebAssembly 應用的連結器配置會剝離國際化資訊,但明確請求區域設置除外。</span><span class="sxs-lookup"><span data-stu-id="8d24b-128">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="8d24b-129">刪除這些程式集可最大程度地減小應用的大小。</span><span class="sxs-lookup"><span data-stu-id="8d24b-129">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="8d24b-130">要控制保留哪些 I18N 程式集,`<MonoLinkerI18NAssemblies>`在專案 檔中設定 MSBuild 屬性:</span><span class="sxs-lookup"><span data-stu-id="8d24b-130">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="8d24b-131">區域值</span><span class="sxs-lookup"><span data-stu-id="8d24b-131">Region Value</span></span>     | <span data-ttu-id="8d24b-132">單聲道區域裝配</span><span class="sxs-lookup"><span data-stu-id="8d24b-132">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="8d24b-133">包括所有程式集</span><span class="sxs-lookup"><span data-stu-id="8d24b-133">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="8d24b-134">*I18N.CJK.dll*</span><span class="sxs-lookup"><span data-stu-id="8d24b-134">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="8d24b-135">*I18N.MidEast.dll*</span><span class="sxs-lookup"><span data-stu-id="8d24b-135">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="8d24b-136">`none` (預設值)</span><span class="sxs-lookup"><span data-stu-id="8d24b-136">`none` (default)</span></span> | <span data-ttu-id="8d24b-137">None</span><span class="sxs-lookup"><span data-stu-id="8d24b-137">None</span></span>                    |
| `other`          | <span data-ttu-id="8d24b-138">*I18N.Other.dll*</span><span class="sxs-lookup"><span data-stu-id="8d24b-138">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="8d24b-139">*I18N.Rare.dll*</span><span class="sxs-lookup"><span data-stu-id="8d24b-139">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="8d24b-140">*I18N.West.dll*</span><span class="sxs-lookup"><span data-stu-id="8d24b-140">*I18N.West.dll*</span></span>         |

<span data-ttu-id="8d24b-141">使用逗號分隔多個值(例如, `mideast,west`</span><span class="sxs-lookup"><span data-stu-id="8d24b-141">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="8d24b-142">有關詳細資訊,請參閱[I18N:Pnetlib 國際化框架庫(單聲道/單聲道 GitHub 儲存庫)。](https://github.com/mono/mono/tree/master/mcs/class/I18N)</span><span class="sxs-lookup"><span data-stu-id="8d24b-142">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
