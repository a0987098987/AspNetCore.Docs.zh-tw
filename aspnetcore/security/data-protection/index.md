---
title: "ASP.NET Core 的資料保護"
author: rick-anderson
description: "本文件是各種 ASP.NET Core 資料保護主題的目錄。"
keywords: "ASP.NET Core, 資料保護"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>ASP.NET Core 中的資料保護：取用者 API、組態、擴充性 API 和實作

* [資料保護簡介](introduction.md)

* [資料保護 API 使用者入門](using-data-protection.md)

* [取用者 API](consumer-apis/index.md)

  * [取用者 API 概觀](consumer-apis/overview.md)

  * [目的字串](consumer-apis/purpose-strings.md)

  * [目的階層和多租用戶](consumer-apis/purpose-strings-multitenancy.md)

  * [密碼雜湊](consumer-apis/password-hashing.md)

  * [限制受保護承載的存留期](consumer-apis/limited-lifetime-payloads.md)

  * [取消索引鍵已撤銷之承載的保護](consumer-apis/dangerous-unprotect.md)

* [組態](configuration/index.md)

  * [設定資料保護](configuration/overview.md)

  * [預設設定](configuration/default-settings.md)

  * [整個電腦的原則](configuration/machine-wide-policy.md)

  * [非 DI 感知案例](configuration/non-di-scenarios.md)

* [擴充性 API](extensibility/index.md)

  * [核心加密擴充性](extensibility/core-crypto.md)

  * [金鑰管理擴充性](extensibility/key-management.md)

  * [其他 API](extensibility/misc-apis.md)

* [實作](implementation/index.md)

  * [已驗證的加密詳細資料](implementation/authenticated-encryption-details.md)

  * [子機碼衍生和驗證的加密](implementation/subkeyderivation.md)

  * [內容標頭](implementation/context-headers.md)

  * [金鑰管理](implementation/key-management.md)

  * [金鑰儲存體提供者](implementation/key-storage-providers.md)

  * [待用時加密金鑰](implementation/key-encryption-at-rest.md)

  * [金鑰的不變性和變更設定](implementation/key-immutability.md)

  * [金鑰儲存體格式](implementation/key-storage-format.md)

  * [暫時資料保護提供者](implementation/key-storage-ephemeral.md)

* [相容性](compatibility/index.md)

  * [應用程式間共用 Cookie](compatibility/cookie-sharing.md)

  * [取代 ASP.NET 中的 <machineKey>](compatibility/replacing-machinekey.md)
