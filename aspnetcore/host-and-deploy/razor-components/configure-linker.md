---
title: 設定 Blazor 的連結器
author: guardrex
description: 了解如何在組建 Blazor 應用程式時，控制中繼語言 (IL) 連結器。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668013"
---
# <a name="configure-the-linker-for-blazor"></a>設定 Blazor 的連結器

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor 會在每個版本模式組建期間執行[中繼語言 (IL)](/dotnet/standard/managed-code#intermediate-language--execution) 連結，以從輸出組件移除不必要的 IL。

您可以使用下列方法之一控制組件連結：

* 使用 MSBuild 屬性全域停用連結。
* 使用組態檔根據組件控制連結。

## <a name="disable-linking-with-an-msbuild-property"></a>使用 MSBuild 屬性停用連結

組建應用程式時，預設會在版本模式中啟用連結，其中包括發佈。 若要停用所有組件的連結，請在專案檔中將 `<BlazorLinkOnBuild>` MSBuild 屬性設為 `false`：

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>使用組態檔控制連結

您可以根據組件控制連結，方法為提供 XML 組態檔，並將此檔案指定為專案檔中的 MSBuild 項目。

下面是一個範例組態檔 (*Linker.xml*)：

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

若要深入了解組態檔的檔案格式，請參閱 [IL 連結器：XML 描述項的語法](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor)。

在專案檔中使用 `BlazorLinkerDescriptor` 項目指定組態檔：

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
