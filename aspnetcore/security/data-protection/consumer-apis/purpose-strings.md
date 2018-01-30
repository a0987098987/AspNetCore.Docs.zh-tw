---
title: "目的字串"
author: rick-anderson
description: "本文件詳述目的字串中的 ASP.NET Core 資料保護 Api 的使用方式。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b4a0db801ecc1c4ba0762f0c9faf7429b4ac097b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="purpose-strings"></a>目的字串

<a name="data-protection-consumer-apis-purposes"></a>

取用元件`IDataProtectionProvider`必須通過唯一*用途*參數`CreateProtector`方法。 目的*參數*是固有的安全性資料保護系統中，因為它會提供密碼編譯的取用者之間的隔離，即使根密碼編譯金鑰都相同。

當取用者指定的目的時，目的字串用於根密碼編譯金鑰以及衍生密碼編譯子機碼唯一給該取用者。 這會隔離與所有其他密碼編譯取用者應用程式中取用者： 沒有其他元件可以讀取其裝載，而且它無法讀取任何其他元件的裝載。 這種隔離也會呈現不可行的整個類別針對元件的攻擊。

![目的圖範例](purpose-strings/_static/purposes.png)

在上圖中，`IDataProtector`執行個體 A 和 B**無法**讀取其他的裝載，只有自己。

目的字串不一定要是密碼。 它只應該是唯一的因為沒有其他行為良好的元件將曾經提供相同的目的字串。

>[!TIP]
> 使用元件取用的資料保護 Api 的命名空間和類型名稱是最佳經驗法則，與這項資訊會永遠不會發生衝突的練習。
>
>Contoso 撰寫的元件，負責 minting 持有者權杖可能會使用 Contoso.Security.BearerToken 以其用途的字串。 或者-更好的-它可能會使用 Contoso.Security.BearerToken.v1 以其用途的字串。 附加的版本號碼可以使用 Contoso.Security.BearerToken.v2 做為其用途，未來版本，並為裝載到不同的版本會是彼此完全隔離。

因為目的參數`CreateProtector`是字串陣列，上述可能已改為指定為`[ "Contoso.Security.BearerToken", "v1" ]`。 這樣可讓您建立階層的用途，並開啟與資料保護系統的多租用戶案例的可能性。

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> 元件不應該允許不受信任的使用者輸入的目的鏈結輸入唯一的來源。
>
>例如，請考慮 Contoso.Messaging.SecureMessage 負責儲存安全訊息的元件。 如果安全訊息的元件是呼叫`CreateProtector([ username ])`，則惡意使用者可能會在嘗試取得要呼叫的元件中使用者名稱為"Contoso.Security.BearerToken 」 中建立帳戶`CreateProtector([ "Contoso.Security.BearerToken" ])`，因此不小心造成安全的訊息無法為驗證 token 看得見的迷你澆裝載系統。
>
>訊息元件的更佳用途鏈結會`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`，這樣會提供適當的隔離。

提供隔離和行為`IDataProtectionProvider`， `IDataProtector`，用途如下：

* 針對給定`IDataProtectionProvider`物件，`CreateProtector`方法會建立`IDataProtector`物件密切同時`IDataProtectionProvider`物件建立它並已傳遞至方法的目的參數。

* 目的參數不能為 null。 （如果目的指定為陣列，這表示，陣列不能為零長度陣列的所有項目必須為非 null）。技術上允許非空字串的目的，但建議您不要使用。

* 兩個目的引數是相等的如果且只有它們包含相同的字串 （使用序數比較子） 相同的順序。 單一目的引數就相當於對應的單一項目的陣列。

* 兩個`IDataProtector`物件相等，如果且只有從對等項目建立`IDataProtectionProvider`具有對等用途的參數物件。

* 針對給定`IDataProtector`物件、 呼叫`Unprotect(protectedData)`會傳回原始`unprotectedData`如果且只有`protectedData := Protect(unprotectedData)`如需參考相等`IDataProtector`物件。

> [!NOTE]
> 我們正在不考慮某些元件刻意選擇目的字串，也就是與另一個元件發生衝突的情況。 這類元件會被視為惡意，基本上，此系統不想要在背景工作處理序內的惡意程式碼已在執行時提供安全性保證。
