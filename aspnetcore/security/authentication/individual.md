---
title: 個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項
author: rick-anderson
description: 探索與個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523060"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項

ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。

驗證範本可用於使用.NET Core CLI `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a>沒有驗證

在.NET Core CLI，以指定驗證`-au`選項。 在 Visual Studio 中，**變更驗證**對話是適用於新的 web 應用程式。 在 Visual Studio 中的新 web 應用程式的預設值是**不需要驗證**。

沒有驗證所建立的專案：

* 不包含 web 網頁和登入和登出的 UI。
* 不包含驗證碼。

<a name="win"></a>
## <a name="windows-authentication"></a>Windows 驗證

指定 Windows 驗證用於新的 web 應用程式中使用.NET Core CLI`-au Windows`選項。 在 Visual Studio 中，**變更驗證** 對話方塊提供**Windows 驗證**選項。

如果已選取 Windows 驗證，應用程式已設定為使用[Windows 驗證的 IIS 模組](xref:host-and-deploy/iis/modules)。 Windows 驗證適用於內部網路網站。

## <a name="additional-resources"></a>其他資源

下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：

* [使用 SMS 的雙因素驗證](xref:security/authentication/2fa)
* [ASP.NET Core 中的帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [建立 ASP.NET Core 應用程式與受保護的授權的使用者資料](xref:security/authorization/secure-data)
