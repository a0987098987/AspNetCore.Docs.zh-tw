---
title: ASP.NET Core 中的目的字串
author: rick-anderson
description: 了解如何在 ASP.NET Core 資料保護 Api 中使用目的字串。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087535"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET Core 中的目的字串

<a name="data-protection-consumer-apis-purposes"></a>

取用元件`IDataProtectionProvider`必須傳遞唯一*用途*參數來`CreateProtector`方法。 為了*參數*是固有資料保護系統的安全性，因為它會提供密碼編譯的取用者之間的隔離，即使根密碼編譯金鑰都相同。

當取用者指定的目的時，目的字串搭配根密碼編譯金鑰衍生給該取用者的唯一密碼編譯子機碼。 這會隔離與所有其他密碼編譯取用者應用程式中取用者： 沒有其他元件可以讀取其裝載中，而且它無法閱讀任何其他元件的裝載。 這種隔離也會呈現不可行的整個類別目錄的針對元件的攻擊。

![目的圖範例](purpose-strings/_static/purposes.png)

在上圖中，`IDataProtector`執行個體 A 和 B**無法**讀取其他人的裝載，只有自己。

目的字串不一定要是祕密。 它應該只是唯一的因為沒有其他行為良好的元件會不斷提供相同的目的字串。

>[!TIP]
> 使用元件取用的資料保護 Api 的命名空間和類型名稱是很好的經驗法則，與這項資訊將永遠不會產生衝突的練習。
>
>Contoso 所撰寫的元件，負責 minting 持有人權杖可能會使用 Contoso.Security.BearerToken 作為它的目的字串。 或者-更棒的是-它可能會讓 Contoso.Security.BearerToken.v1 作為它的目的字串。 附加版本號碼可讓未來的版本將 Contoso.Security.BearerToken.v2 作為它的目的，就裝載到不同的版本會彼此完全隔離。

因為目的參數`CreateProtector`是一個字串陣列，上述可能已改為指定為`[ "Contoso.Security.BearerToken", "v1" ]`。 這樣可讓您建立的目的階層架構，並開啟與資料保護系統的多租用戶案例的可能性。

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> 元件不應該允許未受信任的使用者輸入的用途鏈輸入唯一的來源。
>
>例如，請考慮 Contoso.Messaging.SecureMessage 負責儲存安全訊息的元件。 當安全的傳訊元件呼叫`CreateProtector([ username ])`，則惡意使用者可能會在嘗試取得要呼叫的元件具有使用者名稱 」 Contoso.Security.BearerToken 」 中建立帳戶`CreateProtector([ "Contoso.Security.BearerToken" ])`，而不小心導致安全傳訊要被視為驗證權杖的 mint 裝載系統。
>
>訊息元件的更佳用途鏈結會`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`，以提供適當的隔離。

所提供的隔離和行為`IDataProtectionProvider`， `IDataProtector`，用途如下：

* 針對給定`IDataProtectionProvider`物件，`CreateProtector`方法會建立`IDataProtector`物件唯一繫結至同時`IDataProtectionProvider`物件建立它，並已傳遞至方法的目的參數。

* 目的參數不能為 null。 （如果目的指定為陣列，這表示，此陣列必須不是長度為零的陣列的所有項目必須為非 null）。非空字串的目的可就技術上而言，但是建議您不要使用。

* 兩個目的引數是相等的如果且只有它們包含相同的字串 （使用序數比較子） 相同的順序。 單一目的引數相當於對應的單一項目目的陣列。

* 兩個`IDataProtector`物件相等，如果且只有在建立對等項目從`IDataProtectionProvider`具有同等用途的參數物件。

* 針對給定`IDataProtector`物件，呼叫`Unprotect(protectedData)`會傳回原始`unprotectedData`才`protectedData := Protect(unprotectedData)`如需參考相等`IDataProtector`物件。

> [!NOTE]
> 不，我們正在考慮的一些元件刻意選擇目的字串，也就是與另一個元件發生衝突的情況。 這類元件會被視為惡意，基本上，此系統不想要在背景工作處理序內的惡意程式碼已在執行時提供安全性保證。
