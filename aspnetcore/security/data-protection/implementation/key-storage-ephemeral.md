---
title: ASP.NET Core 中的暫時資料保護提供者
author: rick-anderson
description: 瞭解 ASP.NET Core 暫時資料保護提供者的執行細節。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 22a332230e15256dc33fd1d06f2da3ea8d34d3bc
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776886"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="c1163-103">ASP.NET Core 中的暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="c1163-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="c1163-104">在某些情況下，應用程式需要完即丟`IDataProtectionProvider`。</span><span class="sxs-lookup"><span data-stu-id="c1163-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="c1163-105">例如，開發人員可能只是在一次性的主控台應用程式中進行實驗，或是應用程式本身是暫時性的（腳本或單元測試專案）。</span><span class="sxs-lookup"><span data-stu-id="c1163-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="c1163-106">為了支援這些案例， [AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)套件包含類型`EphemeralDataProtectionProvider`。</span><span class="sxs-lookup"><span data-stu-id="c1163-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="c1163-107">此類型提供的基本執行， `IDataProtectionProvider`其金鑰存放庫只會保留在記憶體中，而且不會寫出至任何備份存放區。</span><span class="sxs-lookup"><span data-stu-id="c1163-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="c1163-108">的每一個`EphemeralDataProtectionProvider`實例都會使用自己唯一的主要金鑰。</span><span class="sxs-lookup"><span data-stu-id="c1163-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="c1163-109">因此， `IDataProtector`如果根目錄為的會`EphemeralDataProtectionProvider`產生受保護的承載，則該承載只能由根在相同`IDataProtector` `EphemeralDataProtectionProvider`實例上的對等（指定相同的[目的](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes)鏈）加以保護。</span><span class="sxs-lookup"><span data-stu-id="c1163-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="c1163-110">下列範例示範如何具現`EphemeralDataProtectionProvider`化，並使用它來保護和解除保護資料。</span><span class="sxs-lookup"><span data-stu-id="c1163-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
