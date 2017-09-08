---
title: "簡介"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 040525505a982fc1be1901effb9186a8fe1cdbdf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="introduction"></a>簡介

<a name=security-authorization-introduction></a>

授權是指程序，決定使用者是否能夠執行。 比方說，允許系統管理使用者建立文件庫，加入文件、 編輯文件，並刪除它們。 使用 程式庫非系統管理使用者只能讀取文件的權限。

授權是垂直和獨立於驗證，這是使用者確實的程序。 驗證可能會建立一或多個目前使用者的身分識別。

## <a name="authorization-types"></a>授權類型

ASP.NET Core 授權提供簡單的宣告式[角色](roles.md#security-authorization-role-based)和[豐富的原則型](policies.md#security-authorization-policies-based)模型。 授權需求，以表示和處理常式評估根據需求的使用者宣告。 命令式檢查可以根據簡單的原則或原則評估使用者識別與使用者嘗試存取資源的內容。

## <a name="namespaces"></a>命名空間

授權元件，包括`AuthorizeAttribute`和`AllowAnonymousAttribute`找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。
