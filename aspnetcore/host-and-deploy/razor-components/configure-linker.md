---
title: 設定 Blazor 的連結器
author: guardrex
description: 了解如何在組建 Blazor 應用程式時，控制中繼語言 (IL) 連結器。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: 7c53e7912ec3b0ae471ea38777f874f55a32487d
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647937"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="5153d-103">設定 Blazor 的連結器</span><span class="sxs-lookup"><span data-stu-id="5153d-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="5153d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5153d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="5153d-105">Blazor 會在每個版本模式組建期間執行[中繼語言 (IL)](/dotnet/standard/managed-code#intermediate-language--execution) 連結，以從輸出組件移除不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="5153d-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="5153d-106">您可以使用下列方法之一控制組件連結：</span><span class="sxs-lookup"><span data-stu-id="5153d-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="5153d-107">使用 MSBuild 屬性全域停用連結。</span><span class="sxs-lookup"><span data-stu-id="5153d-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="5153d-108">使用組態檔根據組件控制連結。</span><span class="sxs-lookup"><span data-stu-id="5153d-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="5153d-109">使用 MSBuild 屬性停用連結</span><span class="sxs-lookup"><span data-stu-id="5153d-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="5153d-110">組建應用程式時，預設會在版本模式中啟用連結，其中包括發佈。</span><span class="sxs-lookup"><span data-stu-id="5153d-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="5153d-111">若要停用所有組件的連結，請在專案檔中將 `<BlazorLinkOnBuild>` MSBuild 屬性設為 `false`：</span><span class="sxs-lookup"><span data-stu-id="5153d-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="5153d-112">使用組態檔控制連結</span><span class="sxs-lookup"><span data-stu-id="5153d-112">Control linking with a configuration file</span></span>

<span data-ttu-id="5153d-113">您可以根據組件控制連結，方法為提供 XML 組態檔，並將此檔案指定為專案檔中的 MSBuild 項目。</span><span class="sxs-lookup"><span data-stu-id="5153d-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="5153d-114">下面是一個範例組態檔 (*Linker.xml*)：</span><span class="sxs-lookup"><span data-stu-id="5153d-114">The following is an example configuration file (*Linker.xml*):</span></span>

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

<span data-ttu-id="5153d-115">若要深入了解組態檔的檔案格式，請參閱 [IL 連結器：XML 描述項的語法](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor)。</span><span class="sxs-lookup"><span data-stu-id="5153d-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="5153d-116">在專案檔中使用 `BlazorLinkerDescriptor` 項目指定組態檔：</span><span class="sxs-lookup"><span data-stu-id="5153d-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
