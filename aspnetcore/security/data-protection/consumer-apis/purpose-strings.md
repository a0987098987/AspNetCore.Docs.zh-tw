---
title: ASP.NET Core 中的目的字串
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 資料保護 Api 中使用目的字串。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664721"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET Core 中的目的字串

<a name="data-protection-consumer-apis-purposes"></a>

使用 `IDataProtectionProvider` 的元件必須將唯一的*目的*參數傳遞至 `CreateProtector` 方法。 「目的」*參數*是資料保護系統的安全性固有的，因為它在密碼編譯取用者之間提供隔離，即使根密碼編譯金鑰相同也是一樣。

當取用者指定用途時，會使用目的字串搭配根密碼編譯金鑰來衍生該取用者獨有的密碼編譯子機碼。 這會將取用者與應用程式中所有其他的密碼編譯取用者隔離：其他元件都無法讀取其裝載，而且無法讀取任何其他元件的裝載。 這項隔離也會針對元件呈現不可行的整個類別攻擊。

![用途圖表範例](purpose-strings/_static/purposes.png)

在上圖中，`IDataProtector` 實例 A 和 B**無法**讀取彼此的裝載，而只有自己的裝載。

目的字串不一定是秘密。 這應該是唯一的，因為沒有其他正常運作的元件會提供相同的目的字串。

>[!TIP]
> 使用取用資料保護 Api 之元件的命名空間和類型名稱是很好的經驗法則，因為實際上，這項資訊絕對不會衝突。
>
>負責 minting 持有人權杖的 Contoso 撰寫元件，可能會使用 BearerToken 作為其目的字串。 更棒的是，它可能會使用 BearerToken 作為其目的字串。 附加版本號碼可讓未來版本使用 BearerToken 做為其用途，而不同的版本會與裝載的位置完全隔離。

因為 `CreateProtector` 的目的參數是字串陣列，所以可以改為指定為 `[ "Contoso.Security.BearerToken", "v1" ]`。 這可讓您建立階層的用途，並在資料保護系統中開啟多租使用者案例的可能性。

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> 元件不應允許未受信任的使用者輸入作為目的鏈的唯一輸入來源。
>
>例如，假設有一個 SecureMessage 的元件，負責儲存安全訊息。 如果安全訊息元件呼叫 `CreateProtector([ username ])`，惡意使用者可能會在嘗試取得元件以呼叫 `CreateProtector([ "Contoso.Security.BearerToken" ])`時，建立名稱為 "BearerToken" 的帳戶，因此不小心導致安全訊息系統 mint 可能被視為驗證權杖的裝載。
>
>訊息元件的較佳目的鏈會 `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`，以提供適當的隔離。

提供的隔離和 `IDataProtectionProvider`、`IDataProtector`和用途的行為如下所示：

* 針對指定的 `IDataProtectionProvider` 物件，`CreateProtector` 方法會建立唯一系結至建立它之 `IDataProtectionProvider` 物件的 `IDataProtector` 物件，以及傳遞至方法的目的參數。

* 目的參數不得為 null。 （如果將目的指定為數組，這表示陣列的長度不能為零，而且陣列的所有元素都必須為非 null）。在技術上，您可以使用空的字串目的，但不建議您這麼做。

* 只有在以相同順序包含相同的字串（使用序數比較子）時，兩個目的引數才是相等的。 單一目的引數相當於對應的單一元素目的陣列。

* 只有當兩個 `IDataProtector` 物件是從具有對等用途參數的對等 `IDataProtectionProvider` 物件所建立時，才會是相等的。

* 對於指定的 `IDataProtector` 物件而言，只有在對等的 `IDataProtector` 物件 `protectedData := Protect(unprotectedData)` 時，`Unprotect(protectedData)` 的呼叫才會傳回原始的 `unprotectedData`。

> [!NOTE]
> 我們不會考慮某些元件刻意選擇與其他元件衝突的目的字串。 這種元件基本上會被視為惡意，而此系統並不是為了在工作者進程內已執行惡意程式碼的情況下提供安全性保證。
