---
title: ASP.NET Core 中的目的字串
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 資料保護 Api 中使用目的字串。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b52961fd33ce2d3708754f73ea38456d8d5f8f3c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404284"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET Core 中的目的字串

<a name="data-protection-consumer-apis-purposes"></a>

使用的元件 `IDataProtectionProvider` 必須將唯一的*目的*參數傳遞至 `CreateProtector` 方法。 「目的」*參數*是資料保護系統的安全性固有的，因為它在密碼編譯取用者之間提供隔離，即使根密碼編譯金鑰相同也是一樣。

當取用者指定用途時，會使用目的字串搭配根密碼編譯金鑰來衍生該取用者獨有的密碼編譯子機碼。 這會將取用者與應用程式中所有其他的密碼編譯取用者隔離：其他元件都無法讀取其裝載，而且無法讀取任何其他元件的裝載。 這項隔離也會針對元件呈現不可行的整個類別攻擊。

![用途圖表範例](purpose-strings/_static/purposes.png)

在上圖中， `IDataProtector` 實例 A 和 B**無法**讀取彼此的裝載，只有自己的裝載。

目的字串不一定是秘密。 這應該是唯一的，因為沒有其他正常運作的元件會提供相同的目的字串。

>[!TIP]
> 使用取用資料保護 Api 之元件的命名空間和類型名稱是很好的經驗法則，因為實際上，這項資訊絕對不會衝突。
>
>負責 minting 持有人權杖的 Contoso 撰寫元件，可能會使用 BearerToken 作為其目的字串。 更棒的是，它可能會使用 BearerToken 作為其目的字串。 附加版本號碼可讓未來版本使用 BearerToken 做為其用途，而不同的版本會與裝載的位置完全隔離。

因為的目的參數 `CreateProtector` 是字串陣列，所以可以改為將上述指定為 `[ "Contoso.Security.BearerToken", "v1" ]` 。 這可讓您建立階層的用途，並在資料保護系統中開啟多租使用者案例的可能性。

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> 元件不應允許未受信任的使用者輸入作為目的鏈的唯一輸入來源。
>
>例如，假設有一個 SecureMessage 的元件，負責儲存安全訊息。 如果要呼叫安全訊息元件 `CreateProtector([ username ])` ，惡意使用者可能會在嘗試取得要呼叫的元件時，建立具有使用者名稱 "BearerToken" 的帳戶 `CreateProtector([ "Contoso.Security.BearerToken" ])` ，因此不小心導致安全訊息系統 mint 可視為驗證權杖的裝載。
>
>訊息元件的較佳目的鏈是 `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])` ，它會提供適當的隔離。

提供的隔離和、和的行為如下所示 `IDataProtectionProvider` `IDataProtector` ：

* 針對指定的 `IDataProtectionProvider` 物件， `CreateProtector` 方法會建立唯一系結 `IDataProtector` 至 `IDataProtectionProvider` 建立它之物件的物件，以及傳遞至方法的目的參數。

* 目的參數不得為 null。 （如果將目的指定為數組，這表示陣列的長度不能為零，而且陣列的所有元素都必須為非 null）。在技術上，您可以使用空的字串目的，但不建議您這麼做。

* 只有在以相同順序包含相同的字串（使用序數比較子）時，兩個目的引數才是相等的。 單一目的引數相當於對應的單一元素目的陣列。

* `IDataProtector`只有在從 `IDataProtectionProvider` 具有對等用途參數的對等物件建立時，才會有兩個物件相等。

* 針對指定的 `IDataProtector` 物件， `Unprotect(protectedData)` `unprotectedData` 只有在 `protectedData := Protect(unprotectedData)` 對等 `IDataProtector` 物件的呼叫時，才會傳回原始的。

> [!NOTE]
> 我們不會考慮某些元件刻意選擇與其他元件衝突的目的字串。 這種元件基本上會被視為惡意，而此系統並不是為了在工作者進程內已執行惡意程式碼的情況下提供安全性保證。
