---
title: ASP.NET Core 的資料保護
author: rick-anderson
description: 本文件是各種 ASP.NET Core 資料保護主題的目錄。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071690"
---
# <a name="data-protection-in-aspnet-core"></a>ASP.NET Core 的資料保護

* [資料保護簡介](xref:security/data-protection/introduction)

* [資料保護 API 使用者入門](xref:security/data-protection/using-data-protection)

* [取用者 API](xref:security/data-protection/consumer-apis/index)

  * [取用者 API 概觀](xref:security/data-protection/consumer-apis/overview)

  * [目的字串](xref:security/data-protection/consumer-apis/purpose-strings)

  * [目的階層和多租用戶](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [雜湊密碼](xref:security/data-protection/consumer-apis/password-hashing)

  * [限制受保護承載的存留期](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [取消索引鍵已撤銷之承載的保護](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [組態](xref:security/data-protection/configuration/index)

  * [設定 ASP.NET Core 資料保護](xref:security/data-protection/configuration/overview)

  * [預設設定](xref:security/data-protection/configuration/default-settings)

  * [整個電腦的原則](xref:security/data-protection/configuration/machine-wide-policy)

  * [非 DI 感知案例](xref:security/data-protection/configuration/non-di-scenarios)

* [擴充性 API](xref:security/data-protection/extensibility/index)

  * [核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)

  * [金鑰管理擴充性](xref:security/data-protection/extensibility/key-management)

  * [其他 API](xref:security/data-protection/extensibility/misc-apis)

* [實作](xref:security/data-protection/implementation/index)

  * [已驗證的加密詳細資料](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [子機碼衍生和驗證的加密](xref:security/data-protection/implementation/subkeyderivation)

  * [內容標頭](xref:security/data-protection/implementation/context-headers)

  * [金鑰管理](xref:security/data-protection/implementation/key-management)

  * [金鑰儲存體提供者](xref:security/data-protection/implementation/key-storage-providers)

  * [待用時加密金鑰](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [金鑰的不變性和設定](xref:security/data-protection/implementation/key-immutability)

  * [金鑰儲存體格式](xref:security/data-protection/implementation/key-storage-format)

  * [暫時資料保護提供者](xref:security/data-protection/implementation/key-storage-ephemeral)

* [相容性](xref:security/data-protection/compatibility/index)

  * [取代 ASP.NET Core 中的 ASP.NET <machineKey>](xref:security/data-protection/compatibility/replacing-machinekey)
