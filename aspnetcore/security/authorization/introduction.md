---
title: 介紹 ASP.NET Core 的授權
author: rick-anderson
description: 了解授權和 ASP.NET Core 應用程式中授權的運作方式的基本概念。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896975"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>介紹 ASP.NET Core 的授權

<a name="security-authorization-introduction"></a>

授權是指程序，決定使用者是否能夠執行。 比方說，系統管理使用者可建立文件庫、 將文件新增、 編輯文件，並加以刪除。 使用程式庫的非系統管理使用者只閱讀文件的權限。

授權是與驗證彼此獨立且互不。 不過，授權要求的驗證機制。 驗證是查明使用者是誰的程序。 驗證可能會建立一或多個目前使用者的身分識別。

## <a name="authorization-types"></a>授權類型

ASP.NET Core 授權提供簡單、 宣告式[角色](xref:security/authorization/roles)，以及豐富[以原則為基礎](xref:security/authorization/policies)模型。 授權需求，以表示和處理常式會評估針對要求的使用者宣告。 命令式檢查可以根據簡單的原則或原則評估的使用者身分識別 」 和 「 使用者嘗試存取資源的屬性。

## <a name="namespaces"></a>命名空間

授權元件，包括`AuthorizeAttribute`並`AllowAnonymousAttribute`，找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。

參閱文件上[簡單授權](xref:security/authorization/simple)。
