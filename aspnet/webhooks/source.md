---
uid: webhooks/source
title: "ASP.NET Webhook 原始碼和 NuGet 套件 |Microsoft 文件"
author: rick-anderson
description: "ASP.NET Webhook 原始碼和 NuGet 套件的連結"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Webhook 原始碼和 NuGet 套件

Microsoft ASP.NET Webhook 屬於模組的 Microsoft ASP.NET 系列和裝載為[GitHub 上的開放原始碼專案](https://github.com/aspnet/WebHooks)。 這表示我們接受參與，但請查看[比重指導方針](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交提取要求。

您正在閱讀這個線上文件現在也裝載為[GitHub 上的開放原始碼](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)和也可接受的貢獻。

## <a name="nuget-packages"></a>Nuget 封裝

Microsoft ASP.NET Webhook 也是可以找到預覽 Nuget 封裝，這表示您必須選取 Visual Studio 中的預覽旗標，才能看到它們。

[Nuget 封裝](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)devided 分成三個部分：

* [一般](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 常見的封裝傳送者與接收者之間共用。

* [寄件者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一組支援您自己的 Webhook 傳送給其他人的封裝。 傳送 Webhook 功能更詳細地說明[傳送 Webhook](sending/index.md)。

* [接收者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一組支援接收 Webhook 與其他的封裝。 接收 Webhook 功能更詳細地說明[接收 Webhook](receiving/index.md)。
