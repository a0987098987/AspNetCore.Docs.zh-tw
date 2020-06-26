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
# <a name="purpose-strings-in-aspnet-core"></a><span data-ttu-id="5a663-103">ASP.NET Core 中的目的字串</span><span class="sxs-lookup"><span data-stu-id="5a663-103">Purpose strings in ASP.NET Core</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="5a663-104">使用的元件 `IDataProtectionProvider` 必須將唯一的*目的*參數傳遞至 `CreateProtector` 方法。</span><span class="sxs-lookup"><span data-stu-id="5a663-104">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="5a663-105">「目的」*參數*是資料保護系統的安全性固有的，因為它在密碼編譯取用者之間提供隔離，即使根密碼編譯金鑰相同也是一樣。</span><span class="sxs-lookup"><span data-stu-id="5a663-105">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="5a663-106">當取用者指定用途時，會使用目的字串搭配根密碼編譯金鑰來衍生該取用者獨有的密碼編譯子機碼。</span><span class="sxs-lookup"><span data-stu-id="5a663-106">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="5a663-107">這會將取用者與應用程式中所有其他的密碼編譯取用者隔離：其他元件都無法讀取其裝載，而且無法讀取任何其他元件的裝載。</span><span class="sxs-lookup"><span data-stu-id="5a663-107">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="5a663-108">這項隔離也會針對元件呈現不可行的整個類別攻擊。</span><span class="sxs-lookup"><span data-stu-id="5a663-108">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![用途圖表範例](purpose-strings/_static/purposes.png)

<span data-ttu-id="5a663-110">在上圖中， `IDataProtector` 實例 A 和 B**無法**讀取彼此的裝載，只有自己的裝載。</span><span class="sxs-lookup"><span data-stu-id="5a663-110">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="5a663-111">目的字串不一定是秘密。</span><span class="sxs-lookup"><span data-stu-id="5a663-111">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="5a663-112">這應該是唯一的，因為沒有其他正常運作的元件會提供相同的目的字串。</span><span class="sxs-lookup"><span data-stu-id="5a663-112">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="5a663-113">使用取用資料保護 Api 之元件的命名空間和類型名稱是很好的經驗法則，因為實際上，這項資訊絕對不會衝突。</span><span class="sxs-lookup"><span data-stu-id="5a663-113">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="5a663-114">負責 minting 持有人權杖的 Contoso 撰寫元件，可能會使用 BearerToken 作為其目的字串。</span><span class="sxs-lookup"><span data-stu-id="5a663-114">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="5a663-115">更棒的是，它可能會使用 BearerToken 作為其目的字串。</span><span class="sxs-lookup"><span data-stu-id="5a663-115">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="5a663-116">附加版本號碼可讓未來版本使用 BearerToken 做為其用途，而不同的版本會與裝載的位置完全隔離。</span><span class="sxs-lookup"><span data-stu-id="5a663-116">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="5a663-117">因為的目的參數 `CreateProtector` 是字串陣列，所以可以改為將上述指定為 `[ "Contoso.Security.BearerToken", "v1" ]` 。</span><span class="sxs-lookup"><span data-stu-id="5a663-117">Since the purposes parameter to `CreateProtector` is a string array, the above could've been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="5a663-118">這可讓您建立階層的用途，並在資料保護系統中開啟多租使用者案例的可能性。</span><span class="sxs-lookup"><span data-stu-id="5a663-118">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="5a663-119">元件不應允許未受信任的使用者輸入作為目的鏈的唯一輸入來源。</span><span class="sxs-lookup"><span data-stu-id="5a663-119">Components shouldn't allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="5a663-120">例如，假設有一個 SecureMessage 的元件，負責儲存安全訊息。</span><span class="sxs-lookup"><span data-stu-id="5a663-120">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="5a663-121">如果要呼叫安全訊息元件 `CreateProtector([ username ])` ，惡意使用者可能會在嘗試取得要呼叫的元件時，建立具有使用者名稱 "BearerToken" 的帳戶 `CreateProtector([ "Contoso.Security.BearerToken" ])` ，因此不小心導致安全訊息系統 mint 可視為驗證權杖的裝載。</span><span class="sxs-lookup"><span data-stu-id="5a663-121">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="5a663-122">訊息元件的較佳目的鏈是 `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])` ，它會提供適當的隔離。</span><span class="sxs-lookup"><span data-stu-id="5a663-122">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="5a663-123">提供的隔離和、和的行為如下所示 `IDataProtectionProvider` `IDataProtector` ：</span><span class="sxs-lookup"><span data-stu-id="5a663-123">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="5a663-124">針對指定的 `IDataProtectionProvider` 物件， `CreateProtector` 方法會建立唯一系結 `IDataProtector` 至 `IDataProtectionProvider` 建立它之物件的物件，以及傳遞至方法的目的參數。</span><span class="sxs-lookup"><span data-stu-id="5a663-124">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="5a663-125">目的參數不得為 null。</span><span class="sxs-lookup"><span data-stu-id="5a663-125">The purpose parameter must not be null.</span></span> <span data-ttu-id="5a663-126">（如果將目的指定為數組，這表示陣列的長度不能為零，而且陣列的所有元素都必須為非 null）。在技術上，您可以使用空的字串目的，但不建議您這麼做。</span><span class="sxs-lookup"><span data-stu-id="5a663-126">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="5a663-127">只有在以相同順序包含相同的字串（使用序數比較子）時，兩個目的引數才是相等的。</span><span class="sxs-lookup"><span data-stu-id="5a663-127">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="5a663-128">單一目的引數相當於對應的單一元素目的陣列。</span><span class="sxs-lookup"><span data-stu-id="5a663-128">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="5a663-129">`IDataProtector`只有在從 `IDataProtectionProvider` 具有對等用途參數的對等物件建立時，才會有兩個物件相等。</span><span class="sxs-lookup"><span data-stu-id="5a663-129">Two `IDataProtector` objects are equivalent if and only if they're created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="5a663-130">針對指定的 `IDataProtector` 物件， `Unprotect(protectedData)` `unprotectedData` 只有在 `protectedData := Protect(unprotectedData)` 對等 `IDataProtector` 物件的呼叫時，才會傳回原始的。</span><span class="sxs-lookup"><span data-stu-id="5a663-130">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="5a663-131">我們不會考慮某些元件刻意選擇與其他元件衝突的目的字串。</span><span class="sxs-lookup"><span data-stu-id="5a663-131">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="5a663-132">這種元件基本上會被視為惡意，而此系統並不是為了在工作者進程內已執行惡意程式碼的情況下提供安全性保證。</span><span class="sxs-lookup"><span data-stu-id="5a663-132">Such a component would essentially be considered malicious, and this system isn't intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
