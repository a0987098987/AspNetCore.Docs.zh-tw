---
title: "授權簡介"
author: rick-anderson
description: "本文件提供授權的基本說明，並說明授權與 ASP.NET Core 關聯的方式。"
keywords: "ASP.NET Core 授權"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a>簡介

<a name="security-authorization-introduction"></a>

授權是指程序，決定使用者是否能夠執行。 比方說，允許系統管理使用者建立文件庫，加入文件、 編輯文件，並刪除它們。 使用 程式庫非系統管理使用者只能讀取文件的權限。

授權是垂直和獨立於驗證，這是使用者確實的程序。 驗證可能會建立一或多個目前使用者的身分識別。

## <a name="authorization-types"></a>授權類型

ASP.NET Core 授權提供簡單的宣告式[角色](roles.md)和[豐富的原則型](policies.md)模型。 授權需求，以表示和處理常式評估根據需求的使用者宣告。 命令式檢查可以根據簡單的原則或原則評估使用者識別與使用者嘗試存取資源的內容。

## <a name="namespaces"></a>命名空間

授權元件，包括`AuthorizeAttribute`和`AllowAnonymousAttribute`找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。
