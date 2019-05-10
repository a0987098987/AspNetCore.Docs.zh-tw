---
title: 針對 ASP.NET Core 當地語系化進行疑難排解
author: hishamco
description: 了解如何診斷 ASP.NET Core 應用程式的當地語系化問題。
ms.author: riande
ms.date: 01/24/2019
uid: fundamentals/troubleshoot-aspnet-core-localization
ms.openlocfilehash: c76732c1a0389818f8f9efae8fe384ca0f9ca308
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087397"
---
# <a name="troubleshoot-aspnet-core-localization"></a>針對 ASP.NET Core 當地語系化進行疑難排解

作者為 [Hisham Bin Ateya](https://github.com/hishamco)

本文提供如何診斷 ASP.NET Core 應用程式當地語系化問題的指示。

## <a name="localization-configuration-issues"></a>當地語系化組態問題

**當地語系化中介軟體順序**  
應用程式可能會因為當地語系化中介軟體並未如預期般排序而無法當地語系化。

若要解決此問題，請確定當地語系化中介軟體的註冊順序早於 MVC 中介軟體。 否則不會套用當地語系化中介軟體。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

**找不到當地語系化資源路徑**

**RequestCultureProvider 中支援的文化特性 (Culture) 與註冊的不相符**  

## <a name="resource-file-naming-issues"></a>資源檔命名問題

ASP.NET Core 已為當地語系化資源檔命名預先定義了規則與方針，其詳細說明請參閱[此處](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)。

## <a name="missing-resources"></a>缺少資源

找不到資源的常見原因包括：

- 資源名稱在 `resx` 檔案或當地語系化工具要求中拼錯。
- 某些語言的 `resx` 中缺少這項資源，但其他語言則有。
- 如果您仍持續發生問題，請查看當地語系化記錄訊息 (在 `Debug` 記錄層級)，以獲取所缺少資源的詳細資料。

**提示：** 當使用 `CookieRequestCultureProvider` 時，請確認當地語系化 Cookie 值中的文化特性 (Culture) 未使用單引號。例如，`c='en-UK'|uic='en-US'` 是無效的 Cookie 值，而 `c=en-UK|uic=en-US` 則有效。_

## <a name="resources--class-libraries-issues"></a>資源與類別庫的問題

ASP.NET Core 根據預設會提供讓類別庫能透過 [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1) 找到資源檔的方法。

類別庫的常見問題包括：
- 類別庫中缺少 `ResourceLocationAttribute` 會導致 `ResourceManagerStringLocalizerFactory` 找不到資源。
- 資源檔命名。 如需詳細資訊，請參閱[資源檔命名問題](#resource-file-naming-issues)一節。
- 變更類別庫的根命名空間。 如需詳細資訊，請參閱[根命名空間問題](#root-namespace-issues)一節。

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a>CustomRequestCultureProvider 未如預期運作

`RequestLocalizationOptions` 類別有三個預設提供者：

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) 可讓您自訂如何在應用程式中提供當地語系化文化特性 (Culture)。 當預設提供者不符合您的需求時，會使用 `CustomRequestCultureProvider`。

- 自訂提供者無法正常運作的常見原因在於它不是 `RequestCultureProviders` 清單中的第一個提供者。 若要解決此問題︰

- 將自訂提供者插入 `RequestCultureProviders` 清單中的位置 0，如下所示：

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- 使用 `AddInitialRequestCultureProvider` 擴充方法將自訂提供者設定為初始提供者。

## <a name="root-namespace-issues"></a>根命名空間問題

當組件的根命名空間與組件名稱不同時，當地語系化根據預設無法運作。 若要避免此問題，請使用 [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1)，其詳細說明請參閱[這裡](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)

## <a name="resources--build-action"></a>資源與建置動作

如果您使用資源檔進行當地語系化，務必讓他們有適當的建置動作。 它們必須是**內嵌資源**，否則 `ResourceStringLocalizer` 找不到這些資源。
