---
title: 設定 ASP.NET Core 的連結器Blazor
author: guardrex
description: 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/host-and-deploy/configure-linker
ms.openlocfilehash: 76af450df70fe666ea1b951cb4b41696057c5e67
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243573"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>設定 ASP.NET Core 的連結器Blazor

作者：[Luke Latham](https://github.com/guardrex)

BlazorWebAssembly 會在組建期間執行[中繼語言（IL）](/dotnet/standard/managed-code#intermediate-language--execution)連結，以從應用程式的輸出元件修剪不必要的 IL。 在 Debug 設定中建立時，會停用連結器。 應用程式必須在發行設定中建立，才能啟用連結器。 我們建議您在部署 WebAssembly apps 時建立發行 Blazor 。 

連結應用程式會優化大小，但可能會有不利的影響。 使用反映或相關動態功能的應用程式可能會在修剪時中斷，因為連結器不知道此動態行為，而且無法判斷在執行時間的反映需要何種類型。 若要修剪這類應用程式，連結器必須通知程式碼中的反映所需的任何類型，以及應用程式所相依的封裝或架構。 

若要確保已修剪的應用程式在部署後能正常運作，請務必在開發時經常測試應用程式的發行組建。

您 Blazor 可以使用下列 MSBuild 功能來設定應用程式的連結：

* 使用[MSBuild 屬性](#control-linking-with-an-msbuild-property)來設定全域連結。
* 使用[設定檔](#control-linking-with-a-configuration-file)來控制每個元件的連結。

## <a name="control-linking-with-an-msbuild-property"></a>使用 MSBuild 屬性的控制項連結

建立應用程式時，會啟用連結 `Release` 。 若要變更此項，請 `BlazorWebAssemblyEnableLinking` 在專案檔中設定 MSBuild 屬性：

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>使用組態檔控制連結

透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="LinkerConfig.xml" />
</ItemGroup>
```

`LinkerConfig.xml`:

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

如需詳細資訊和範例，請參閱[資料格式（mono/連結器 GitHub 存放庫）](https://github.com/mono/linker/blob/master/docs/data-formats.md)。

## <a name="add-an-xml-linker-configuration-file-to-a-library"></a>將 XML 連結器設定檔加入至程式庫

若要設定特定程式庫的連結器，請將 XML 連結器設定檔新增至程式庫做為內嵌資源。 內嵌資源的名稱必須與元件相同。

在下列範例中，會將檔案 `LinkerConfig.xml` 指定為與程式庫的元件同名的內嵌資源：

```xml
<ItemGroup>
  <EmbeddedResource Include="LinkerConfig.xml">
    <LogicalName>$(MSBuildProjectName).xml</LogicalName>
  </EmbeddedResource>
</ItemGroup>
```

### <a name="configure-the-linker-for-internationalization"></a>設定國際化的連結器

根據預設， Blazor Blazor WebAssembly 應用程式的連結器設定會去除國際化資訊，但不包括明確要求的地區設定。 移除這些元件會將應用程式的大小降到最低。

若要控制要保留哪些國際化元件，請 `<BlazorWebAssemblyI18NAssemblies>` 在專案檔中設定 MSBuild 屬性：

```xml
<PropertyGroup>
  <BlazorWebAssemblyI18NAssemblies>{all|none|REGION1,REGION2,...}</BlazorWebAssemblyI18NAssemblies>
</PropertyGroup>
```

| 區域值     | Mono 區域元件    |
| ---------------- | ----------------------- |
| `all`            | 包含的所有元件 |
| `cjk`            | `I18N.CJK.dll`          |
| `mideast`        | `I18N.MidEast.dll`      |
| `none` (預設) | None                    |
| `other`          | `I18N.Other.dll`        |
| `rare`           | `I18N.Rare.dll`         |
| `west`           | `I18N.West.dll`         |

使用逗號來分隔多個值（例如 `mideast,west` ）。

如需詳細資訊，請參閱[I18N： Pnetlib 國際化架構程式庫（mono/Mono GitHub 儲存機制）](https://github.com/mono/mono/tree/master/mcs/class/I18N)。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/webassembly-performance-best-practices#intermediate-language-il-linking>
