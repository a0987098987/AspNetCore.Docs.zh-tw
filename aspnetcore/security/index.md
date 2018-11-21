---
title: ASP.NET Core 安全性概觀
author: tdykstra
description: 了解 ASP.NET Core 的驗證、授權和安全性基本概念。
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 579e472e01efd08bbafe949e37a3b655a42a5b46
ms.sourcegitcommit: 04b55a5ce9d649ff2df926157ec28ae47afe79e2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "52156915"
---
# <a name="overview-of-aspnet-core-security"></a>ASP.NET Core 安全性概觀

ASP.NET Core 可讓開發人員輕鬆設定應用程式的安全性並進行管理。 ASP.NET Core 包含針對認證、授權、資料保護、強制使用 SSL、應用程式密碼、防偽要求保護及 CORS 的管理功能。 這些安全性功能可讓您打造強固又安全的 ASP.NET Core 應用程式。

## <a name="aspnet-core-security-features"></a>ASP.NET Core 安全性功能

ASP.NET Core 提供許多工具和程式庫來保護應用程式的安全，包括內建的識別提供者，但您也可以使用協力廠商的識別服務 (如 Facebook、Twitter 或 LinkedIn)。 使用 ASP.NET Core 時，您可以輕鬆管理應用程式密碼，以便使用機密資訊而不在程式碼中公開這些資訊。

## <a name="authentication-vs-authorization"></a>驗證與授權

驗證是一道程序，其會將使用者提供的認證與作業系統、資料庫、應用程式或資源中儲存的認證進行比對。 如果相符的話，使用者就能成功通過驗證，並可執行他們在授權程序期間取得授權的動作。 授權則是決定使用者得以執行哪些動作的程序。

您也可以將驗證想成是進入伺服器、資料庫、應用程式或資源這類空間的方法，而授權則表示使用者得以針對空間 (伺服器、資料庫或應用程式) 內哪些物件執行哪些動作。

## <a name="common-vulnerabilities-in-software"></a>軟體的常見弱點

ASP.NET Core 和 EF 包含的功能有助您保護應用程式的安全，並防範安全性缺口的發生。 下列連結清單可帶您前往如何避免 Web 應用程式中最常見資訊安全漏洞的技術詳細文件：

* [Cross site scripting attacks](xref:security/cross-site-scripting) (跨網站指令碼攻擊)
* [SQL Injection attacks](/ef/core/querying/raw-sql) (SQL 插入式攻擊)
* [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) (跨網站偽造要求 (CSRF))
* [Open redirect attacks](xref:security/preventing-open-redirects) (開啟重新導向攻擊)

除此之外，還有許多弱點是您必須提防的。 如需詳細資訊，請參閱目錄**安全性與身分識別**一節中的其他文章。
