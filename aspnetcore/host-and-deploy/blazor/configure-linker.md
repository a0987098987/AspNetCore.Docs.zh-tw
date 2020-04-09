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
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>設定 ASP.NET Core Blazor 的連結器

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor WebAssembly 在生成期間執行[中間語言 (IL)](/dotnet/standard/managed-code#intermediate-language--execution)連結,以從應用的輸出程式集中修剪不必要的 IL。 在調試配置中生成連結器時,連結器將禁用。 應用必須在發佈配置中生成才能啟用連結器。 我們建議在部署 Blazor WebAssembly 應用時在「發布」中構建。 

鏈接應用會優化大小,但可能會造成不利影響。 使用反射或相關動態功能的應用在修剪時可能會中斷,因為連結器不知道此動態行為,並且通常無法確定運行時反射需要哪些類型。 要修剪此類應用,必須告知連結程式在代碼和程式所依賴的包或框架中反射所需的任何類型。 

為了確保修剪的應用在部署後正常工作,在開發時經常測試應用的發佈版本非常重要。

可以使用以下 MSBuild 功能設定 Blazor 應用的連結:

* 使用[MSBuild 屬性](#control-linking-with-an-msbuild-property)配置全域連結。
* 使用[配置檔](#control-linking-with-a-configuration-file)控制每個程式集的連結。

## <a name="control-linking-with-an-msbuild-property"></a>使用 MSBuild 屬性進行連結

在配置中`Release`生成應用時啟用連結。 要變更此項目檔`BlazorWebAssemblyEnableLinking`中的 MSBuild 屬性:

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

*連結康菲格.xml*:

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

有關詳細資訊,請參閱連結[xml 檔範例(單聲道/連結器 GitHub 儲存庫)。](https://github.com/mono/linker#link-xml-file-examples)

## <a name="add-an-xml-linker-configuration-file-to-a-library"></a>新增 XML 連結器設定檔到函式庫

要設定特定庫的連結程式,請將 XML 連結器設定檔作為嵌入式資源添加到庫中。 嵌入的資源必須與程式集具有相同的名稱。

在下面的範例中 *,LinkerConfig.xml*檔指定為與函式庫程式集同名的嵌入式資源:

```xml
<ItemGroup>
  <EmbeddedResource Include="LinkerConfig.xml">
    <LogicalName>$(MSBuildProjectName).xml</LogicalName>
  </EmbeddedResource>
</ItemGroup>
```

### <a name="configure-the-linker-for-internationalization"></a>設定用於國際化的連結器

默認情況下,Blazor 針對 Blazor WebAssembly 應用的連結器配置會剝離國際化資訊,但明確請求區域設置除外。 刪除這些程式集可最大程度地減小應用的大小。

要控制保留哪些 I18N 程式集,`<MonoLinkerI18NAssemblies>`在專案 檔中設定 MSBuild 屬性:

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| 區域值     | 單聲道區域裝配    |
| ---------------- | ----------------------- |
| `all`            | 包括所有程式集 |
| `cjk`            | *I18N.CJK.dll*          |
| `mideast`        | *I18N.MidEast.dll*      |
| `none` (預設值) | None                    |
| `other`          | *I18N.Other.dll*        |
| `rare`           | *I18N.Rare.dll*         |
| `west`           | *I18N.West.dll*         |

使用逗號分隔多個值(例如, `mideast,west`

有關詳細資訊,請參閱[I18N:Pnetlib 國際化框架庫(單聲道/單聲道 GitHub 儲存庫)。](https://github.com/mono/mono/tree/master/mcs/class/I18N)
