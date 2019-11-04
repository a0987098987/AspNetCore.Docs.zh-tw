---
title: 以個別使用者帳戶所建立 ASP.NET Core 專案為基礎的文章
author: rick-anderson
description: 根據使用個別使用者帳戶所建立 ASP.NET Core 專案來探索文章。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463029"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>以個別使用者帳戶所建立 ASP.NET Core 專案為基礎的文章

ASP.NET Core 身分識別包含在具有 [個別使用者帳戶] 選項之 Visual Studio 的專案範本中。

驗證範本可在 .NET Core CLI 中使用 `-au Individual`：

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/5833)以取得 Web API 驗證。

<a name="no"></a>

## <a name="no-authentication"></a>無驗證

使用 `-au` 選項在 .NET Core CLI 中指定驗證。 在 Visual Studio 中，新的 web 應用程式可以使用 [**變更驗證**] 對話方塊。 Visual Studio 中新 web 應用程式的預設值為 [**無驗證**]。

建立不含驗證的專案：

* 請勿包含網頁和 UI 以進行登入和登出。
* 不包含驗證碼。

<a name="win"></a>

## <a name="windows-authentication"></a>Windows 驗證

使用 `-au Windows` 選項，為 .NET Core CLI 中的新 web 應用程式指定 Windows 驗證。 在 Visual Studio 中，[**變更驗證**] 對話方塊會提供**Windows 驗證**選項。

如果已選取 [Windows 驗證]，則會將應用程式設定為使用[Windows 驗證 IIS 模組](xref:host-and-deploy/iis/modules)。 Windows 驗證適用于內部網路網站。

## <a name="additional-resources"></a>其他資源

下列文章示範如何使用在 ASP.NET Core 範本中產生的程式碼，以使用個別使用者帳戶：

* [ASP.NET Core 中的帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式](xref:security/authorization/secure-data)
