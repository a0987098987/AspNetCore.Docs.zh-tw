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
# <a name="purpose-strings-in-aspnet-core"></a><span data-ttu-id="05c46-103">ASP.NET Core 中的目的字串</span><span class="sxs-lookup"><span data-stu-id="05c46-103">Purpose strings in ASP.NET Core</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="05c46-104">取用元件`IDataProtectionProvider`必須傳遞唯一*用途*參數來`CreateProtector`方法。</span><span class="sxs-lookup"><span data-stu-id="05c46-104">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="05c46-105">為了*參數*是固有資料保護系統的安全性，因為它會提供密碼編譯的取用者之間的隔離，即使根密碼編譯金鑰都相同。</span><span class="sxs-lookup"><span data-stu-id="05c46-105">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="05c46-106">當取用者指定的目的時，目的字串搭配根密碼編譯金鑰衍生給該取用者的唯一密碼編譯子機碼。</span><span class="sxs-lookup"><span data-stu-id="05c46-106">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="05c46-107">這會隔離與所有其他密碼編譯取用者應用程式中取用者： 沒有其他元件可以讀取其裝載中，而且它無法閱讀任何其他元件的裝載。</span><span class="sxs-lookup"><span data-stu-id="05c46-107">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="05c46-108">這種隔離也會呈現不可行的整個類別目錄的針對元件的攻擊。</span><span class="sxs-lookup"><span data-stu-id="05c46-108">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![目的圖範例](purpose-strings/_static/purposes.png)

<span data-ttu-id="05c46-110">在上圖中，`IDataProtector`執行個體 A 和 B**無法**讀取其他人的裝載，只有自己。</span><span class="sxs-lookup"><span data-stu-id="05c46-110">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="05c46-111">目的字串不一定要是祕密。</span><span class="sxs-lookup"><span data-stu-id="05c46-111">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="05c46-112">它應該只是唯一的因為沒有其他行為良好的元件會不斷提供相同的目的字串。</span><span class="sxs-lookup"><span data-stu-id="05c46-112">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="05c46-113">使用元件取用的資料保護 Api 的命名空間和類型名稱是很好的經驗法則，與這項資訊將永遠不會產生衝突的練習。</span><span class="sxs-lookup"><span data-stu-id="05c46-113">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="05c46-114">Contoso 所撰寫的元件，負責 minting 持有人權杖可能會使用 Contoso.Security.BearerToken 作為它的目的字串。</span><span class="sxs-lookup"><span data-stu-id="05c46-114">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="05c46-115">或者-更棒的是-它可能會讓 Contoso.Security.BearerToken.v1 作為它的目的字串。</span><span class="sxs-lookup"><span data-stu-id="05c46-115">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="05c46-116">附加版本號碼可讓未來的版本將 Contoso.Security.BearerToken.v2 作為它的目的，就裝載到不同的版本會彼此完全隔離。</span><span class="sxs-lookup"><span data-stu-id="05c46-116">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="05c46-117">因為目的參數`CreateProtector`是一個字串陣列，上述可能已改為指定為`[ "Contoso.Security.BearerToken", "v1" ]`。</span><span class="sxs-lookup"><span data-stu-id="05c46-117">Since the purposes parameter to `CreateProtector` is a string array, the above could've been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="05c46-118">這樣可讓您建立的目的階層架構，並開啟與資料保護系統的多租用戶案例的可能性。</span><span class="sxs-lookup"><span data-stu-id="05c46-118">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="05c46-119">元件不應該允許未受信任的使用者輸入的用途鏈輸入唯一的來源。</span><span class="sxs-lookup"><span data-stu-id="05c46-119">Components shouldn't allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="05c46-120">例如，請考慮 Contoso.Messaging.SecureMessage 負責儲存安全訊息的元件。</span><span class="sxs-lookup"><span data-stu-id="05c46-120">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="05c46-121">當安全的傳訊元件呼叫`CreateProtector([ username ])`，則惡意使用者可能會在嘗試取得要呼叫的元件具有使用者名稱 」 Contoso.Security.BearerToken 」 中建立帳戶`CreateProtector([ "Contoso.Security.BearerToken" ])`，而不小心導致安全傳訊要被視為驗證權杖的 mint 裝載系統。</span><span class="sxs-lookup"><span data-stu-id="05c46-121">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="05c46-122">訊息元件的更佳用途鏈結會`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`，以提供適當的隔離。</span><span class="sxs-lookup"><span data-stu-id="05c46-122">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="05c46-123">所提供的隔離和行為`IDataProtectionProvider`， `IDataProtector`，用途如下：</span><span class="sxs-lookup"><span data-stu-id="05c46-123">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="05c46-124">針對給定`IDataProtectionProvider`物件，`CreateProtector`方法會建立`IDataProtector`物件唯一繫結至同時`IDataProtectionProvider`物件建立它，並已傳遞至方法的目的參數。</span><span class="sxs-lookup"><span data-stu-id="05c46-124">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="05c46-125">目的參數不能為 null。</span><span class="sxs-lookup"><span data-stu-id="05c46-125">The purpose parameter must not be null.</span></span> <span data-ttu-id="05c46-126">（如果目的指定為陣列，這表示，此陣列必須不是長度為零的陣列的所有項目必須為非 null）。非空字串的目的可就技術上而言，但是建議您不要使用。</span><span class="sxs-lookup"><span data-stu-id="05c46-126">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="05c46-127">兩個目的引數是相等的如果且只有它們包含相同的字串 （使用序數比較子） 相同的順序。</span><span class="sxs-lookup"><span data-stu-id="05c46-127">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="05c46-128">單一目的引數相當於對應的單一項目目的陣列。</span><span class="sxs-lookup"><span data-stu-id="05c46-128">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="05c46-129">兩個`IDataProtector`物件相等，如果且只有在建立對等項目從`IDataProtectionProvider`具有同等用途的參數物件。</span><span class="sxs-lookup"><span data-stu-id="05c46-129">Two `IDataProtector` objects are equivalent if and only if they're created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="05c46-130">針對給定`IDataProtector`物件，呼叫`Unprotect(protectedData)`會傳回原始`unprotectedData`才`protectedData := Protect(unprotectedData)`如需參考相等`IDataProtector`物件。</span><span class="sxs-lookup"><span data-stu-id="05c46-130">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="05c46-131">不，我們正在考慮的一些元件刻意選擇目的字串，也就是與另一個元件發生衝突的情況。</span><span class="sxs-lookup"><span data-stu-id="05c46-131">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="05c46-132">這類元件會被視為惡意，基本上，此系統不想要在背景工作處理序內的惡意程式碼已在執行時提供安全性保證。</span><span class="sxs-lookup"><span data-stu-id="05c46-132">Such a component would essentially be considered malicious, and this system isn't intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
