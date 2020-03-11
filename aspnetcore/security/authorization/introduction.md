---
title: ASP.NET Core 中的授權簡介
author: rick-anderson
description: 瞭解授權的基本概念，以及授權在 ASP.NET Core 應用程式中的運作方式。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660710"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>ASP.NET Core 中的授權簡介

<a name="security-authorization-introduction"></a>

授權是指決定使用者能夠執行之作業的程式。 例如，系統管理使用者可建立文件庫、加入檔、編輯檔，以及將它們刪除。 使用媒體櫃的非系統管理使用者只會獲得讀取檔的授權。

授權是與驗證無關且獨立的。 不過，授權需要驗證機制。 「驗證」（Authentication）是查明使用者的處理常式。 驗證可為目前使用者建立一或多個身分識別。

如需 ASP.NET Core 中驗證的詳細資訊，請參閱 <xref:security/authentication/index>。

## <a name="authorization-types"></a>授權類型

ASP.NET Core 授權提供簡單、宣告式[角色](xref:security/authorization/roles)和以[原則為基礎](xref:security/authorization/policies)的豐富模型。 授權會以需求表示，而處理常式會根據需求評估使用者的宣告。 命令式檢查可以根據簡單的原則或原則，評估使用者嘗試存取的資源的使用者身分識別和屬性。

## <a name="namespaces"></a>命名空間

授權元件（包括 `AuthorizeAttribute` 和 `AllowAnonymousAttribute` 屬性）可在 `Microsoft.AspNetCore.Authorization` 命名空間中找到。

請參閱有關[簡單授權](xref:security/authorization/simple)的檔。
