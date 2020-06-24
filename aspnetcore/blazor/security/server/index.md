---
title: 保護 ASP.NET Core Blazor 伺服器應用程式
author: guardrex
description: 瞭解如何以 Blazor ASP.NET Core 的應用程式來保護伺服器應用程式。
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
uid: blazor/security/server/index
ms.openlocfilehash: 2811e08fd2f6c66112ffa0bb40f474158f4c7a59
ms.sourcegitcommit: 5e462c3328c70f95969d02adce9c71592049f54c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85292681"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a>保護 ASP.NET Core Blazor 伺服器應用程式

作者：[Luke Latham](https://github.com/guardrex)

Blazor伺服器應用程式的安全性設定方式與 ASP.NET Core 應用程式相同。 如需詳細資訊，請參閱底下的文章 <xref:security/index> 。 本總覽底下的主題特別適用于 Blazor 伺服器。 

## <a name="blazor-server-project-template"></a>Blazor伺服器專案範本

Blazor建立專案時，可以設定伺服器專案範本進行驗證。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

遵循文章中的 Visual Studio 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案。

在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中選擇** Blazor 伺服器應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更**]。

對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：

* **無驗證**
* **個別使用者帳戶**：可以儲存使用者帳戶：
  * 在應用程式中使用 ASP.NET Core 的 [Identity](xref:security/authentication/identity) 系統。
  * 使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。
* **工作或學校帳戶**
* **Windows 驗證**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

請遵循本文中的 Visual Studio Code 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制 | Description |
| ------------------------ | ----------- |
| `None` (預設)         | 不需要驗證 |
| `Individual`             | 使用 ASP.NET Core 儲存在應用程式中的使用者Identity |
| `IndividualB2C`          | 儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者 |
| `SingleOrg`              | 單一租使用者的組織驗證 |
| `MultiOrg`               | 多個租使用者的組織驗證 |
| `Windows`                | Windows 驗證 |

使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：

* 建立專案的資料夾。
* 命名專案。

如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 遵循文章中的 Visual Studio for Mac 指導方針 <xref:blazor/get-started> 。

1. 在 [**設定新的 Blazor 伺服器應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。

1. 應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 Identity 。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

請遵循本文中的 .NET Core CLI 指導方針 <xref:blazor/get-started> ， Blazor 使用驗證機制建立新的伺服器專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制 | Description |
| ------------------------ | ----------- |
| `None` (預設)         | 不需要驗證 |
| `Individual`             | 使用 ASP.NET Core 儲存在應用程式中的使用者Identity |
| `IndividualB2C`          | 儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者 |
| `SingleOrg`              | 單一租使用者的組織驗證 |
| `MultiOrg`               | 多個租使用者的組織驗證 |
| `Windows`                | Windows 驗證 |

使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：

* 建立專案的資料夾。
* 命名專案。

如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。

---

## <a name="scaffold-identity"></a>ScaffoldIdentity

Scaffold Identity 至 Blazor 伺服器專案：

* [沒有現有的授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)。
* [具有授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)。
