---
title: 個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項
author: rick-anderson
description: 探索與個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080695"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>個別使用者帳戶建立的 ASP.NET Core 專案為基礎的發行項

ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。

驗證範本可用於使用.NET Core CLI `-au Individual`:

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

使用`-au`選項在 .NET Core CLI 中指定驗證。 在 Visual Studio 中，新的 web 應用程式可以使用 [**變更驗證**] 對話方塊。 Visual Studio 中新 web 應用程式的預設值為 [**無驗證**]。

建立不含驗證的專案：

* 請勿包含網頁和 UI 以進行登入和登出。
* 不包含驗證碼。

<a name="win"></a>

## <a name="windows-authentication"></a>Windows 驗證

在 .NET Core CLI 中使用`-au Windows`選項，為新的 web 應用程式指定 Windows 驗證。 在 Visual Studio 中，[**變更驗證**] 對話方塊會提供**Windows 驗證**選項。

如果已選取 [Windows 驗證]，則會將應用程式設定為使用[Windows 驗證 IIS 模組](xref:host-and-deploy/iis/modules)。 Windows 驗證適用于內部網路網站。

## <a name="additional-resources"></a>其他資源

下列文件顯示如何使用 ASP.NET Core 範本使用個別的使用者帳戶中產生的程式碼：

* [使用 SMS 的雙因素驗證](xref:security/authentication/2fa)
* [ASP.NET Core 中的帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [建立 ASP.NET Core 應用程式與受保護的授權的使用者資料](xref:security/authorization/secure-data)
