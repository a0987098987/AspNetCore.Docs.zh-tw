---
title: 將設定移轉到 ASP.NET Core
author: ardalis
description: 了解如何將組態從 ASP.NET MVC 專案移轉至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205908"
---
# <a name="migrate-configuration-to-aspnet-core"></a>將設定移轉到 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)

在上一篇文章中，我們已開始[將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC](xref:migration/mvc)。 在本文中，我們可以移轉設定。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>設定組態

ASP.NET Core 不會再使用*Global.asax*並*web.config*舊版 ASP.NET 使用的檔案。 在舊版 ASP.NET 中，應用程式啟動邏輯已放置在`Application_StartUp`方法內*Global.asax*。 稍後，在 ASP.NET MVC *Startup.cs*檔案會包含在專案中; 的根目錄，並啟動應用程式時，它所呼叫。 ASP.NET Core 已完全採用這種方法，藉由將放在所有的啟動邏輯*Startup.cs*檔案。

*Web.config*檔案也已取代 ASP.NET Core 中。 組態本身可以現在設定，做為應用程式啟動程序中所述的一部分*Startup.cs*。 組態仍然可以利用 XML 檔案，但通常是 ASP.NET Core 專案都會將組態值置於 JSON 格式的檔案，例如*appsettings.json*。 ASP.NET Core 組態系統也很容易可以存取環境變數，其可提供[更安全、 穩健的位置](xref:security/app-secrets)環境特定值。 這特別適用於祕密，例如連接字串和應該不會簽入原始檔控制的 API 金鑰。 請參閱[組態](xref:fundamentals/configuration/index)若要深入了解 ASP.NET Core 中的組態。

在本文中，我們將開始與部分已移轉的 ASP.NET Core 專案，從[前一篇文章](xref:migration/mvc)。 若要設定的組態，請新增下列建構函式和屬性，以*Startup.cs*檔案位於專案的根目錄：

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

請注意，此時， *Startup.cs*檔案不會編譯，因為我們仍需要加入下列`using`陳述式：

```csharp
using Microsoft.Extensions.Configuration;
```

新增*appsettings.json*使用適當的項目範本的專案根目錄的檔案：

![將 AppSettings JSON 新增](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>從 web.config 中移轉的組態設定

我們的 ASP.NET MVC 專案包含必要的資料庫連接字串中*web.config*，請在`<connectionStrings>`項目。 在 ASP.NET Core 專案中，我們將此資訊儲存在*appsettings.json*檔案。 開啟*appsettings.json*，並記下它已經包含下列：

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

在上述的反白顯示列，變更的資料庫名稱 **_CHANGE_ME**到您的資料庫名稱。

## <a name="summary"></a>總結

ASP.NET Core 會將所有的啟動邏輯應用程式置於單一檔案，以必要的服務和相依性可定義和設定。 它會取代*web.config*可以利用各種不同的檔案格式，例如 JSON，如環境變數的彈性設定功能的檔案。
