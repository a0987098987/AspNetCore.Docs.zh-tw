---
title: 將設定移轉到 ASP.NET Core
author: ardalis
description: 了解如何將設定從 ASP.NET MVC 專案移轉到 ASP.NET Core MVC 專案。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: ead4f96aa0041cd919caa972d3bb05bd94a857b3
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851718"
---
# <a name="migrate-configuration-to-aspnet-core"></a>將設定移轉到 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)

前一個發行項，開始[ASP.NET MVC 專案移轉至 ASP.NET Core MVC](xref:migration/mvc)。 在本文中，我們可以移轉設定。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>安裝程式組態

不會再使用 ASP.NET Core *Global.asax*和*web.config*舊版 ASP.NET 所使用的檔案。 在舊版 ASP.NET 中，應用程式啟動邏輯已放置在`Application_StartUp`方法內*Global.asax*。 接著，在 ASP.NET MVC *Startup.cs*檔案會包含在專案的根目錄，和應用程式啟動時呼叫。 ASP.NET Core 已完全採用這種方法，藉由放置在所有的啟動邏輯*Startup.cs*檔案。

*Web.config*也已取代檔案中 ASP.NET Core。 設定本身可以現在設定，做為應用程式的啟動程序中所述的一部分*Startup.cs*。 組態仍然可以利用 XML 檔案，但通常 ASP.NET Core 專案將組態值的檔案中放置 JSON 格式，例如*appsettings.json*。 ASP.NET Core 組態系統可以輕鬆存取環境變數，可提供[更安全完善的位置](xref:security/app-secrets)環境專用的值。 這是特別有用，例如連接字串和不應該簽入原始檔控制的 API 金鑰的密碼。 請參閱[組態](xref:fundamentals/configuration/index)若要深入了解 ASP.NET Core 中組態。

如本文中，我們開始部分移轉的 ASP.NET Core 專案，從使用[前一篇文章](xref:migration/mvc)。 安裝程式組態，將下列建構函式和屬性加入至*Startup.cs*專案的根目錄中找到的檔案：

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

請注意，此時， *Startup.cs*檔案將不會進行編譯，因為我們還是需要新增下列`using`陳述式：

```csharp
using Microsoft.Extensions.Configuration;
```

新增*appsettings.json*使用適當的項目範本的專案根目錄的檔案：

![新增 AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>從 web.config 移轉組態設定

我們的 ASP.NET MVC 專案包含必要的資料庫連接字串中*web.config*，請在`<connectionStrings>`項目。 在 ASP.NET Core 專案中，我們將此資訊儲存在*appsettings.json*檔案。 開啟*appsettings.json*，並注意它已經包含下列：

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

在上面所述的反白顯示列，請變更資料庫的名稱 **_CHANGE_ME**至您的資料庫名稱。

## <a name="summary"></a>總結

ASP.NET Core 置於單一檔案，以必要的服務和相依性可以定義，以及設定的所有應用程式的啟動邏輯。 它會取代*web.config*彈性設定功能，可以運用各種不同的檔案格式，例如 JSON，如環境變數的檔案。
