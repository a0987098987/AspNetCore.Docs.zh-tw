---
title: 在 ASP.NET Core 暫時資料保護提供者
author: rick-anderson
description: 了解 ASP.NET Core 暫時資料保護提供者的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279463"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>在 ASP.NET Core 暫時資料保護提供者

<a name="data-protection-implementation-key-storage-ephemeral"></a>

應用程式需要 throwaway 的情況下`IDataProtectionProvider`。 例如，開發人員可能只實驗一次性的主控台應用程式，或應用程式本身是暫時性 （會編寫指令碼或單元測試專案）。 若要支援這些案例[Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)封裝包含類型`EphemeralDataProtectionProvider`。 此類型提供的基本實作`IDataProtectionProvider`其索引鍵的儲存機制會保留單獨記憶體中，並不寫入任何備份存放區。

每個執行個體`EphemeralDataProtectionProvider`會使用它自己唯一的主索引鍵。 因此，如果`IDataProtector`根目錄`EphemeralDataProtectionProvider`產生受保護的內容，裝載可以只未受保護的對等項目`IDataProtector`(指定相同[用途](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes)鏈結) 在相同根目錄`EphemeralDataProtectionProvider`執行個體。

下列範例將示範如何具現化`EphemeralDataProtectionProvider`並使用它來保護且取消保護資料。

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
