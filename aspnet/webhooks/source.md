---
uid: webhooks/source
title: ASP.NET Webhook 原始程式碼和 NuGet 套件 |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook 原始程式碼和 NuGet 套件的連結
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825560"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Webhook 原始程式碼和 NuGet 套件

Microsoft ASP.NET Webhook 是模組的 Microsoft ASP.NET 系列的一部分，且裝載作為[GitHub 上的開放原始碼專案](https://github.com/aspnet/WebHooks)。 這表示我們接受參與，但請看看[貢獻指導方針](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交提取要求。

您正在閱讀此線上文件現在也做[GitHub 上的開放原始碼](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)並接受貢獻。

## <a name="nuget-packages"></a>NuGet 套件

[NuGet 套件](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)分為三個部分：

* [常見](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 寄件者與接收者之間共用的常見套件。

* [寄件者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一組套件支援給其他人傳送您自己的 Webhook。 傳送 Webhook 的功能更詳細地說明[傳送 Webhook](sending/index.md)。

* [接收者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一組支援 Webhook 接收其他人的套件。 接收 Webhook 的功能更詳細地說明[接收 Webhook](receiving/index.md)。
