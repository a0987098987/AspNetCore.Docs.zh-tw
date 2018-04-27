---
title: 在 ASP.NET Core 授權簡介
author: rick-anderson
description: 了解授權和授權 ASP.NET Core 應用程式中的運作方式的基本概念。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>在 ASP.NET Core 授權簡介

<a name="security-authorization-introduction"></a>

授權是指程序，決定使用者是否能夠執行。 比方說，允許系統管理使用者建立文件庫，加入文件、 編輯文件，並刪除它們。 使用 程式庫非系統管理使用者只能讀取文件的權限。

授權是垂直和獨立於驗證。 不過，授權要求的驗證機制。 驗證是演練使用者的程序。 驗證可能會建立一或多個目前使用者的身分識別。

## <a name="authorization-types"></a>授權類型

ASP.NET Core 授權提供簡單、 宣告式[角色](xref:security/authorization/roles)和 rich[以原則為基礎](xref:security/authorization/policies)模型。 授權需求，以表示和處理常式評估根據需求的使用者宣告。 命令式檢查可以根據簡單的原則或原則評估使用者識別與使用者嘗試存取資源的內容。

## <a name="namespaces"></a>命名空間

授權元件，包括`AuthorizeAttribute`和`AllowAnonymousAttribute`，找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。

參考的文件上[簡單授權](xref:security/authorization/simple)。
