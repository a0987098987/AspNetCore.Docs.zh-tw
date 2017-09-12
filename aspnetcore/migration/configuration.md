---
title: "移轉設定"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 62660f7e58467a69f540966df188747b6fde57fe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-configuration"></a>移轉設定

由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)

前一個發行項，開始[將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC](mvc.md)。 在本文中，我們可以移轉設定。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a>安裝程式組態

不會再使用 ASP.NET Core *Global.asax*和*web.config*舊版 ASP.NET 所使用的檔案。 在舊版 ASP.NET 中，應用程式啟動邏輯已放置在`Application_StartUp`方法內*Global.asax*。 接著，在 ASP.NET MVC *Startup.cs*檔案會包含在專案的根目錄，和應用程式啟動時呼叫。 ASP.NET Core 已完全採用這種方法，藉由放置在所有的啟動邏輯*Startup.cs*檔案。

*Web.config*也已取代檔案中 ASP.NET Core。 設定本身可以現在設定，做為應用程式的啟動程序中所述的一部分*Startup.cs*。 組態仍然可以利用 XML 檔案，但通常 ASP.NET Core 專案將組態值的檔案中放置 JSON 格式，例如*appsettings.json*。 ASP.NET Core 組態系統可以也很容易存取環境變數，可提供更安全完善的位置環境專屬的值。 這是特別有用，例如連接字串和不能簽入至原始檔控制的 API 金鑰的密碼。 請參閱[組態](../fundamentals/configuration.md)若要深入了解 ASP.NET Core 中組態。

如本文中，我們開始使用中的部分移轉 ASP.NET Core 專案[前一篇文章](mvc.md)。 安裝程式組態，將下列建構函式和屬性加入至*Startup.cs*專案的根目錄中找到的檔案：

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

請注意，此時， *Startup.cs*檔案將不會編譯，因為我們還是需要新增下列`using`陳述式：

```csharp
using Microsoft.Extensions.Configuration;
```

新增*appsettings.json*使用適當的項目範本的專案根目錄的檔案：

![新增 AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>從 web.config 移轉組態設定

我們的 ASP.NET MVC 專案包含必要的資料庫連接字串中*web.config*，請在`<connectionStrings>`項目。 在 ASP.NET Core 專案中，我們將此資訊儲存在*appsettings.json*檔案。 開啟*appsettings.json*，並注意它已經包含下列：

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


在上面所述的反白顯示列，請變更資料庫的名稱**_CHANGE_ME**至您的資料庫名稱。

## <a name="summary"></a>總結

ASP.NET Core 置於單一檔案，以必要的服務和相依性可以定義，以及設定的所有應用程式的啟動邏輯。 它會取代*web.config*彈性設定功能，可以運用各種不同的檔案格式，例如 JSON，如環境變數的檔案。
