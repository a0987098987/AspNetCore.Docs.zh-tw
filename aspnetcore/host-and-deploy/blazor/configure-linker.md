---
title: 設定 ASP.NET Core Blazor 的連結器
author: guardrex
description: 了解如何在組建 Blazor 應用程式時，控制中繼語言 (IL) 連結器。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: bdddae16885f45df2c10e4d98b1c33eb11dfdf24
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153218"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>設定 ASP.NET Core Blazor 的連結器

作者：[Luke Latham](https://github.com/guardrex)

Blazor 會在發行組建期間執行[中繼語言 (IL)](/dotnet/standard/managed-code#intermediate-language--execution) 連結，以從應用程式的輸出組件中移除不必要的 IL。

請使用下列其中一種方法來控制組件連結：

* 使用 [MSBuild 屬性](#disable-linking-with-a-msbuild-property)來全域停用連結。
* 使用[設定檔](#control-linking-with-a-configuration-file)來根據組件控制連結。

## <a name="disable-linking-with-a-msbuild-property"></a>使用 MSBuild 屬性來停用連結

組建應用程式時，預設會在版本模式中啟用連結，其中包括發佈。 若要停用所有組件的連結，請在專案檔中將 `<BlazorLinkOnBuild>` MSBuild 屬性設為 `false`：

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>使用組態檔控制連結

透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Linker.xml*：

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

如需詳細資訊，請參閱 [IL 連結器：XML 描述項的語法](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor)。
