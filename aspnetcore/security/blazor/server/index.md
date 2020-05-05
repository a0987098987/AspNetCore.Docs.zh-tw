---
title: 保護 ASP.NET Core Blazor伺服器應用程式
author: guardrex
description: 瞭解如何以 ASP.NET Core Blazor的應用程式來保護伺服器應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/server/index
ms.openlocfilehash: bbd8b6fcd357b8929bf097450854d98fbea2570e
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82772631"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a>保護 ASP.NET Core Blazor 伺服器應用程式

作者：[Luke Latham](https://github.com/guardrex)

## <a name="blazor-server-project-template"></a>Blazor 伺服器專案範本

建立專案時，可以設定 Blazor 伺服器專案範本進行驗證。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

遵循<xref:blazor/get-started>文章中的 Visual Studio 指導方針，使用驗證機制建立新的 Blazor 伺服器專案。

在 [建立新的 ASP.NET Core Web 應用程式]**** 對話方塊中選擇 [Blazor 伺服器應用程式]**** 範本之後，請選取 [驗證]**** 下的 [變更]****。

對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：

* **無驗證**
* **個別使用者帳戶** &ndash; 使用者帳戶能以下列方式儲存：
  * 使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。
  * 使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。
* **工作或學校帳戶**
* **Windows 驗證**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

請遵循<xref:blazor/get-started>本文中的 Visual Studio Code 指導方針，使用驗證機制建立新的 Blazor 伺服器專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制 | 說明 |
| ------------------------ | ----------- |
| `None` (預設值)         | 不需要驗證 |
| `Individual`             | 以 ASP.NET Core 身分識別儲存在應用程式中的使用者 |
| `IndividualB2C`          | 儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者 |
| `SingleOrg`              | 單一租使用者的組織驗證 |
| `MultiOrg`               | 多個租使用者的組織驗證 |
| `Windows`                | Windows 驗證 |

使用`-o|--output`選項時，此命令會使用提供給`{APP NAME}`預留位置的值來執行下列動作：

* 建立專案的資料夾。
* 命名專案。

如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 遵循<xref:blazor/get-started>文章中的 Visual Studio for Mac 指導方針。

1. 在 [**設定您的新 Blazor 伺服器應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。

1. 應用程式是針對儲存在應用程式中 ASP.NET Core 身分識別的個別使用者所建立。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

請遵循<xref:blazor/get-started>本文中的 .NET Core CLI 指導方針，使用驗證機制建立新的 Blazor 伺服器專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制 | 說明 |
| ------------------------ | ----------- |
| `None` (預設值)         | 不需要驗證 |
| `Individual`             | 以 ASP.NET Core 身分識別儲存在應用程式中的使用者 |
| `IndividualB2C`          | 儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者 |
| `SingleOrg`              | 單一租使用者的組織驗證 |
| `MultiOrg`               | 多個租使用者的組織驗證 |
| `Windows`                | Windows 驗證 |

使用`-o|--output`選項時，此命令會使用提供給`{APP NAME}`預留位置的值來執行下列動作：

* 建立專案的資料夾。
* 命名專案。

如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。

---

## <a name="secure-an-existing-app"></a>保護現有的應用程式

Blazor伺服器應用程式的安全性設定方式與 ASP.NET Core 應用程式相同。 如需詳細資訊，請參閱底下<xref:security/index>的文章。
