---
title: ASP.NET Core 中的暫時資料保護提供者
author: rick-anderson
description: 瞭解 ASP.NET Core 暫時資料保護提供者的執行細節。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664735"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>ASP.NET Core 中的暫時資料保護提供者

<a name="data-protection-implementation-key-storage-ephemeral"></a>

在某些情況下，應用程式需要完即丟 `IDataProtectionProvider`。 例如，開發人員可能只是在一次性的主控台應用程式中進行實驗，或是應用程式本身是暫時性的（腳本或單元測試專案）。 為了支援這些案例， [AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)套件包含 `EphemeralDataProtectionProvider`的類型。 此類型提供 `IDataProtectionProvider` 的基本執行，其金鑰存放庫只保留在記憶體中，不會寫出至任何備份存放區。

`EphemeralDataProtectionProvider` 的每個實例都會使用自己唯一的主要金鑰。 因此，如果根目錄為 `EphemeralDataProtectionProvider` 的 `IDataProtector` 會產生受保護的承載，則該承載只能由根于相同 `EphemeralDataProtectionProvider` 實例的對等 `IDataProtector` （指定相同的[目的](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes)鏈）加以保護。

下列範例示範如何具現化 `EphemeralDataProtectionProvider`，並使用它來保護和解除保護資料。

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
