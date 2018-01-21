---
title: "個別使用者帳戶建立的專案為基礎的發行項"
author: rick-anderson
description: "本文件列出個別的使用者帳戶建立的專案為基礎的發行項。"
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>個別使用者帳戶建立的專案為基礎的發行項

ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。

驗證範本可用於使用.NET 核心 CLI `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：

* [使用 SMS 的雙因素驗證](xref:security/authentication/2fa)
* [ASP.NET Core 中的帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [建立 ASP.NET Core 應用程式與受保護的授權的使用者資料](xref:security/authorization/secure-data)
