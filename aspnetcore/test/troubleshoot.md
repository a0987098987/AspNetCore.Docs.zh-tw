---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274589"
---
# <a name="troubleshoot-aspnet-core-projects"></a>疑難排解 ASP.NET Core 專案

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

下列連結提供疑難排解指導方針：

* [針對 Azure App Service 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/azure-apps/troubleshoot)
* [針對 IIS 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/iis/troubleshoot)
* [Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC 會議 （倫敦 2018年）： 在 ASP.NET Core 應用程式診斷問題](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET 部落格： ASP.NET Core 效能問題的疑難排解](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK 警告

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>會安裝 32 位元和 64 位元版本的.NET Core SDK

在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：

> 會安裝 32 和 64 位元版本的.NET Core sdk。 只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。 這兩個版本可能會安裝的常見原因包括：

* 您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。
* 32 位元.NET Core SDK 已安裝另一個應用程式。
* 錯誤的版本已下載及安裝。

解除安裝 32 位元.NET Core SDK，以避免這個警告。 從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。 如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK 安裝在多個位置

在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：

> .NET Core SDK 會安裝在多個位置。 只有安裝在同時從樣板 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。 通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。

解除安裝 32 位元.NET Core SDK，以避免這個警告。 從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。 如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。

### <a name="no-net-core-sdks-were-detected"></a>偵測到.NET Core Sdk

在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：

> 偵測到.NET Core Sdk，請確定它們包含在環境變數 'PATH'。

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/NoNetCore.png)

時，會出現這個警告的環境變數`PATH`未指向任何.NET Core Sdk，電腦上。 若要解決這個問題：

* 安裝或檢查已安裝.NET Core SDK。
* 確認`PATH`環境變數指向安裝 SDK 的位置。 安裝程式通常設定`PATH`。

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>使用 IHtmlHelper.Partial 可能導致應用程式死結

在 ASP.NET Core 2.1 和更新版本，呼叫`Html.Partial`導致分析器警告，因為可能有死結。 警告訊息是：

> 使用 IHtmlHelper.Partial 可能會導致應用程式死結。 請考慮使用`<partial>`標記協助程式或`IHtmlHelper.PartialAsync`。

呼叫`@Html.Partial`應取代為`@await Html.PartialAsync`或部分標記協助程式`<partial name="_Partial" />`。

::: moniker-end
