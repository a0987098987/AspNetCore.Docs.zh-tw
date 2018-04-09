---
title: 在 ASP.NET Core 暫時資料保護提供者
author: rick-anderson
description: 了解 ASP.NET Core 暫時資料保護提供者的實作詳細資料。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9cdd3949826091807f3139f51aa0c05ddaac696a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="2c608-103">在 ASP.NET Core 暫時資料保護提供者</span><span class="sxs-lookup"><span data-stu-id="2c608-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="2c608-104">應用程式需要 throwaway 的情況下`IDataProtectionProvider`。</span><span class="sxs-lookup"><span data-stu-id="2c608-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="2c608-105">例如，開發人員可能只實驗一次性的主控台應用程式，或應用程式本身是暫時性 （會編寫指令碼或單元測試專案）。</span><span class="sxs-lookup"><span data-stu-id="2c608-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="2c608-106">若要支援這些案例[Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)封裝包含類型`EphemeralDataProtectionProvider`。</span><span class="sxs-lookup"><span data-stu-id="2c608-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="2c608-107">此類型提供的基本實作`IDataProtectionProvider`其索引鍵的儲存機制會保留單獨記憶體中，並不寫入任何備份存放區。</span><span class="sxs-lookup"><span data-stu-id="2c608-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="2c608-108">每個執行個體`EphemeralDataProtectionProvider`會使用它自己唯一的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2c608-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="2c608-109">因此，如果`IDataProtector`根目錄`EphemeralDataProtectionProvider`產生受保護的內容，裝載可以只未受保護的對等項目`IDataProtector`(指定相同[用途](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes)鏈結) 在相同根目錄`EphemeralDataProtectionProvider`執行個體。</span><span class="sxs-lookup"><span data-stu-id="2c608-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="2c608-110">下列範例將示範如何具現化`EphemeralDataProtectionProvider`並使用它來保護且取消保護資料。</span><span class="sxs-lookup"><span data-stu-id="2c608-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
