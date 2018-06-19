---
title: 在 ASP.NET Core 目的字串
author: rick-anderson
description: 了解 ASP.NET Core 資料保護 Api 中使用目的字串的方式。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 8fe0020256d3a105b1968db693b0c667244957ec
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077649"
---
# <a name="purpose-strings-in-aspnet-core"></a><span data-ttu-id="e2110-103">在 ASP.NET Core 目的字串</span><span class="sxs-lookup"><span data-stu-id="e2110-103">Purpose strings in ASP.NET Core</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="e2110-104">取用元件`IDataProtectionProvider`必須通過唯一*用途*參數`CreateProtector`方法。</span><span class="sxs-lookup"><span data-stu-id="e2110-104">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="e2110-105">目的*參數*是固有的安全性資料保護系統中，因為它會提供密碼編譯的取用者之間的隔離，即使根密碼編譯金鑰都相同。</span><span class="sxs-lookup"><span data-stu-id="e2110-105">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="e2110-106">當取用者指定的目的時，目的字串用於根密碼編譯金鑰以及衍生密碼編譯子機碼唯一給該取用者。</span><span class="sxs-lookup"><span data-stu-id="e2110-106">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="e2110-107">這會隔離與所有其他密碼編譯取用者應用程式中取用者： 沒有其他元件可以讀取其裝載，而且它無法讀取任何其他元件的裝載。</span><span class="sxs-lookup"><span data-stu-id="e2110-107">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="e2110-108">這種隔離也會呈現不可行的整個類別針對元件的攻擊。</span><span class="sxs-lookup"><span data-stu-id="e2110-108">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![目的圖範例](purpose-strings/_static/purposes.png)

<span data-ttu-id="e2110-110">在上圖中，`IDataProtector`執行個體 A 和 B**無法**讀取其他的裝載，只有自己。</span><span class="sxs-lookup"><span data-stu-id="e2110-110">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="e2110-111">目的字串不一定要是密碼。</span><span class="sxs-lookup"><span data-stu-id="e2110-111">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="e2110-112">它只應該是唯一的因為沒有其他行為良好的元件將曾經提供相同的目的字串。</span><span class="sxs-lookup"><span data-stu-id="e2110-112">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="e2110-113">使用元件取用的資料保護 Api 的命名空間和類型名稱是最佳經驗法則，與這項資訊會永遠不會發生衝突的練習。</span><span class="sxs-lookup"><span data-stu-id="e2110-113">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="e2110-114">Contoso 撰寫的元件，負責 minting 持有者權杖可能會使用 Contoso.Security.BearerToken 以其用途的字串。</span><span class="sxs-lookup"><span data-stu-id="e2110-114">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="e2110-115">或者-更好的-它可能會使用 Contoso.Security.BearerToken.v1 以其用途的字串。</span><span class="sxs-lookup"><span data-stu-id="e2110-115">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="e2110-116">附加的版本號碼可以使用 Contoso.Security.BearerToken.v2 做為其用途，未來版本，並為裝載到不同的版本會是彼此完全隔離。</span><span class="sxs-lookup"><span data-stu-id="e2110-116">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="e2110-117">因為目的參數`CreateProtector`是字串陣列，上述可能已改為指定為`[ "Contoso.Security.BearerToken", "v1" ]`。</span><span class="sxs-lookup"><span data-stu-id="e2110-117">Since the purposes parameter to `CreateProtector` is a string array, the above could've been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="e2110-118">這樣可讓您建立階層的用途，並開啟與資料保護系統的多租用戶案例的可能性。</span><span class="sxs-lookup"><span data-stu-id="e2110-118">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="e2110-119">元件不應該允許不受信任的使用者輸入的目的鏈結輸入唯一的來源。</span><span class="sxs-lookup"><span data-stu-id="e2110-119">Components shouldn't allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="e2110-120">例如，請考慮 Contoso.Messaging.SecureMessage 負責儲存安全訊息的元件。</span><span class="sxs-lookup"><span data-stu-id="e2110-120">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="e2110-121">如果安全訊息的元件是呼叫`CreateProtector([ username ])`，則惡意使用者可能會在嘗試取得要呼叫的元件中使用者名稱為"Contoso.Security.BearerToken 」 中建立帳戶`CreateProtector([ "Contoso.Security.BearerToken" ])`，因此不小心造成安全的訊息無法為驗證 token 看得見的迷你澆裝載系統。</span><span class="sxs-lookup"><span data-stu-id="e2110-121">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="e2110-122">訊息元件的更佳用途鏈結會`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`，這樣會提供適當的隔離。</span><span class="sxs-lookup"><span data-stu-id="e2110-122">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="e2110-123">提供隔離和行為`IDataProtectionProvider`， `IDataProtector`，用途如下：</span><span class="sxs-lookup"><span data-stu-id="e2110-123">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="e2110-124">針對給定`IDataProtectionProvider`物件，`CreateProtector`方法會建立`IDataProtector`物件密切同時`IDataProtectionProvider`物件建立它並已傳遞至方法的目的參數。</span><span class="sxs-lookup"><span data-stu-id="e2110-124">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="e2110-125">目的參數不能為 null。</span><span class="sxs-lookup"><span data-stu-id="e2110-125">The purpose parameter must not be null.</span></span> <span data-ttu-id="e2110-126">（如果目的指定為陣列，這表示，陣列不能為零長度陣列的所有項目必須為非 null）。技術上允許非空字串的目的，但建議您不要使用。</span><span class="sxs-lookup"><span data-stu-id="e2110-126">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="e2110-127">兩個目的引數是相等的如果且只有它們包含相同的字串 （使用序數比較子） 相同的順序。</span><span class="sxs-lookup"><span data-stu-id="e2110-127">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="e2110-128">單一目的引數就相當於對應的單一項目的陣列。</span><span class="sxs-lookup"><span data-stu-id="e2110-128">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="e2110-129">兩個`IDataProtector`物件相等，如果且只有從對等項目建立`IDataProtectionProvider`具有對等用途的參數物件。</span><span class="sxs-lookup"><span data-stu-id="e2110-129">Two `IDataProtector` objects are equivalent if and only if they're created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="e2110-130">針對給定`IDataProtector`物件、 呼叫`Unprotect(protectedData)`會傳回原始`unprotectedData`如果且只有`protectedData := Protect(unprotectedData)`如需參考相等`IDataProtector`物件。</span><span class="sxs-lookup"><span data-stu-id="e2110-130">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="e2110-131">我們正在不考慮某些元件刻意選擇目的字串，也就是與另一個元件發生衝突的情況。</span><span class="sxs-lookup"><span data-stu-id="e2110-131">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="e2110-132">這類元件會被視為惡意，基本上，此系統不想要在背景工作處理序內的惡意程式碼已在執行時提供安全性保證。</span><span class="sxs-lookup"><span data-stu-id="e2110-132">Such a component would essentially be considered malicious, and this system isn't intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
