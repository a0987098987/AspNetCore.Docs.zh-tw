---
title: "在 ASP.NET Core 環境標記協助程式"
author: pkellner
description: "ASP.Net Core 環境標記協助程式定義包括所有屬性"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: e16f942b4a42ef495c7a7daff6724ff071bb35a1
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>在 ASP.NET Core 環境標記協助程式

由[Peter Kellner](http://peterkellner.net)和[Hisham Bin Ateya](https://twitter.com/hishambinateya)

環境標記協助程式有條件地呈現其括住之目前裝載環境為基礎的內容。 其單一屬性`names`是以逗號分隔清單的環境名稱，如果任何符合目前的環境，將會觸發要呈現括住的內容。

## <a name="environment-tag-helper-attributes"></a>環境標記協助程式屬性

### <a name="names"></a>名稱

接受單一裝載環境名稱或裝載觸發括住的內容轉譯的環境名稱的逗號分隔清單。

這些值會從 ASP.NET Core 靜態屬性傳回的目前值比較`HostingEnvironment.EnvironmentName`。  這個值可以是下列其中之一：**臨時**;**開發**或**生產**。 比較會忽略大小寫。

有效範例`environment`標記協助程式：

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>包含與排除屬性

2.x 加入 ASP.NET Core `include`  &  `exclude`屬性。 這些屬性會控制轉譯括住包含或排除裝載環境名稱為基礎的內容。

### <a name="include-aspnet-core-20-and-later"></a>加入 ASP.NET Core 2.0 和更新版本

`include`屬性具有類似行為`names`ASP.NET Core 1.0 中的屬性。

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>排除 2.0 和更新版本的 ASP.NET Core

相反地，`exclude`屬性可讓`EnvironmentTagHelper`轉譯為所有的裝載環境名稱，除非您指定的結束括住的內容。

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
