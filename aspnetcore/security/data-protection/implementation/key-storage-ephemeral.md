---
title: "暫時資料保護提供者"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 6b564f082eefad993ac938407e0f3b657d83fb52
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="ephemeral-data-protection-providers"></a>暫時資料保護提供者

<a name=data-protection-implementation-key-storage-ephemeral></a>

沒有應用程式需要 throwaway IDataProtectionProvider 的案例。 例如，開發人員可能只實驗一次性的主控台應用程式，或應用程式本身是暫時性 （會編寫指令碼或單元測試專案）。 若要支援這些案例封裝 Microsoft.AspNetCore.DataProtection 會包括 EphemeralDataProtectionProvider 的型別。 此類型提供的 IDataProtectionProvider 其索引鍵的儲存機制會保留單獨記憶體中，並不寫入任何備份存放區的基本實作。

EphemeralDataProtectionProvider 每個執行個體使用它自己唯一的主索引鍵。 因此，如果根目錄 EphemeralDataProtectionProvider IDataProtector 產生受保護的內容，裝載可以只未受保護的對等的 IDataProtector (指定相同[用途](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes)鏈結) 在相同的根項目EphemeralDataProtectionProvider 執行個體。

下列範例將示範 EphemeralDataProtectionProvider 具現化，並使用它來保護且取消保護資料。

```none
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
