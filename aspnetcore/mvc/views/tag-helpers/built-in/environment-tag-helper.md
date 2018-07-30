---
title: ASP.NET Core 中的環境標籤協助程式
author: pkellner
description: 定義了 ASP.NET Core 環境標籤協助程式，包括所有屬性
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342220"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的環境標籤協助程式

作者：[Peter Kellner](http://peterkellner.net) 和 [Hisham Bin Ateya](https://twitter.com/hishambinateya)

環境標籤協助程式依據目前的主控環境，有條件地呈現含括內容。 其單一屬性 `names` 是以逗號分隔的環境名稱清單，如果有任一項符合目前的環境，將會觸發呈現含括的內容。

## <a name="environment-tag-helper-attributes"></a>環境標籤協助程式屬性

### <a name="names"></a>名稱

接受單一主控環境名稱或以逗號分隔的主控環境名稱清單，這些名稱會觸發呈現含括的內容。

這些值會與從 ASP.NET Core 靜態屬性 `HostingEnvironment.EnvironmentName` 傳回的目前值進行比較。  這個值可以是下列其中一項：**Staging**、**Development** 或 **Production**。 比較會忽略大小寫。

有效的 `environment` 標籤協助程式範例為：

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>include 和 exclude 屬性

ASP.NET Core 2.x 新增了 `include`  &  `exclude` 屬性。 這些屬性會依據包含或排除的主控環境名稱來控制含括內容的呈現。

### <a name="include-aspnet-core-20-and-later"></a>include - ASP.NET Core 2.0 和更新版本

`include` 屬性的行為類似於 ASP.NET Core 1.0 中的 `names` 屬性。

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>exclude - ASP.NET Core 2.0 和更新版本

相較之下，`exclude` 屬性可讓 `EnvironmentTagHelper` 呈現所有主控環境名稱 (除了您指定的主控環境名稱之外) 的含括內容。

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/environments>
