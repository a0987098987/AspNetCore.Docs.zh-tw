---
title: ASP.NET Core 中的資料保護的非 DI 注意案例
author: rick-anderson
description: 了解如何支援資料保護案例，您無法或不想要使用相依性插入所提供的服務。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 62280a9f911b003383cbe348b9b62942766a2b99
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048235"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>ASP.NET Core 中的資料保護的非 DI 注意案例

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

通常是 ASP.NET Core 資料保護系統[加入至服務容器](xref:security/data-protection/consumer-apis/overview)且由透過相依性插入 (DI) 的相依元件。 不過，一些情況下，這不可行或您想要的尤其是當系統匯入現有的應用程式。

若要支援這些案例中， [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)套件提供具象類型[DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider)，提供簡單的方式來使用資料保護完全不用仰賴 DI。 `DataProtectionProvider`型別會實作[IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider)。 建構`DataProtectionProvider`只需要提供[DirectoryInfo](/dotnet/api/system.io.directoryinfo)執行個體指出提供者的密碼編譯金鑰儲存的位置，如下列程式碼範例所示：

[!code-csharp[](non-di-scenarios/_static/nodisample1.cs)]

根據預設，`DataProtectionProvider`具象型別不會加密原始的金鑰資料之前將它保存至檔案系統。 這是為了支援的案例，開發人員會指向網路共用資料夾和資料保護系統無法自動推算待用適當的金鑰加密機制。

此外，`DataProtectionProvider`具象型別不[隔離的應用程式](xref:security/data-protection/configuration/overview#per-application-isolation)預設。 使用相同的索引鍵目錄的所有應用程式可以共用裝載，只要他們[用途參數](xref:security/data-protection/consumer-apis/purpose-strings)比對。

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)建構函式會接受選用的組態回呼，可用來調整系統的行為。 下列範例示範如何還原隔離的明確呼叫[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)。 此範例也會示範設定系統以自動加密這些檔案持續性索引鍵，使用 Windows DPAPI。 如果目錄指向 UNC 共用，您可能想將相關的所有電腦上的共用的憑證，並設定系統，以使用憑證加密，藉由呼叫[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)。

[!code-csharp[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> 執行個體`DataProtectionProvider`具象型別會耗費大量資源。 如果應用程式會維護此類型的多個執行個體，而且如果它們全部使用相同的金鑰儲存體目錄，可能會降低應用程式效能。 如果您使用`DataProtectionProvider`類型，我們建議您一次建立此類型，並重複使用它盡量。 `DataProtectionProvider`型別和所有[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)從它建立的執行個體是安全執行緒的多個呼叫端。
