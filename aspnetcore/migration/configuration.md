---
title: 將設定遷移至 ASP.NET Core
author: ardalis
description: 瞭解如何將設定從 ASP.NET MVC 專案遷移至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 2c50ea768a42aa38d14c55d8c403fea4176b3650
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659324"
---
# <a name="migrate-configuration-to-aspnet-core"></a>將設定遷移至 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)

在前一篇文章中，我們開始將[ASP.NET mvc 專案遷移至 ASP.NET CORE mvc](xref:migration/mvc)。 在本文中，我們會遷移設定。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>設定組態

ASP.NET Core 不再使用舊版 ASP.NET所利用的 global.asax*和 web.config 檔案。* 在舊版的 ASP.NET 中，應用程式啟動邏輯會放在*global.asax*內的 `Application_StartUp` 方法中。 之後，在 ASP.NET MVC 中， *Startup.cs*檔案會包含在專案的根目錄中;而且，它是在應用程式啟動時呼叫。 ASP.NET Core 已藉由將所有啟動邏輯放在*Startup.cs*檔案中，來完全採用這種方法。

*Web.config*檔案在 ASP.NET Core 中也已被取代。 設定本身現在可以在*Startup.cs*中所述的應用程式啟動過程中設定。 設定仍然可以利用 XML 檔案，但通常 ASP.NET Core 專案會將設定值放在 JSON 格式的檔案中，例如*appsettings。* ASP.NET Core 的設定系統也可以輕鬆地存取環境變數，這可以為環境特定的值提供[更安全且更穩固的位置](xref:security/app-secrets)。 尤其適用于不應簽入原始檔控制的秘密（例如連接字串和 API 金鑰）。 若要深入瞭解 ASP.NET Core[中的設定](xref:fundamentals/configuration/index)，請參閱設定。

在本文中，我們會從[上一篇文章](xref:migration/mvc)中的部分遷移 ASP.NET Core 專案開始。 若要安裝設定，請將下列函數和屬性新增至位於專案根目錄中的*Startup.cs*檔案：

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

請注意，此時不會編譯*Startup.cs*檔案，因為我們仍然需要新增下列 `using` 語句：

```csharp
using Microsoft.Extensions.Configuration;
```

使用適當的專案範本，將*appsettings*新增至專案的根目錄：

![新增 AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>從 web.config 遷移配置設定

在 `<connectionStrings>` 專案*的 web.config 中，我們*的 ASP.NET MVC 專案包含必要的資料庫連接字串。 在我們的 ASP.NET Core 專案中，我們會將這項資訊儲存在*appsettings*中。 開啟*appsettings*，並請注意它已經包含下列內容：

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

在上面所述的反白顯示行中，將資料庫的名稱從 **_CHANGE_ME**變更為您的資料庫名稱。

## <a name="summary"></a>摘要

ASP.NET Core 會將應用程式的所有啟動邏輯放在單一檔案中，您可以在其中定義和設定必要的服務和相依性。 它會以彈性的設定功能取代*web.config*檔案，可利用各種不同的檔案格式，例如 JSON，以及環境變數。
