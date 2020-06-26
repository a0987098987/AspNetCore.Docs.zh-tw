---
title: ASP.NET Core 中的目的階層和多租使用者
author: rick-anderson
description: 瞭解目的字串階層和多租使用者，因為它與 ASP.NET Core 資料保護 Api 相關。
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
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 8f069da500e7bc06e4b8712fbf7b86d90a815758
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404375"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a><span data-ttu-id="8f3ea-103">ASP.NET Core 中的目的階層和多租使用者</span><span class="sxs-lookup"><span data-stu-id="8f3ea-103">Purpose hierarchy and multi-tenancy in ASP.NET Core</span></span>

<span data-ttu-id="8f3ea-104">由於 `IDataProtector` 也是隱含的 `IDataProtectionProvider` ，因此可以將目的連結在一起。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-104">Since an `IDataProtector` is also implicitly an `IDataProtectionProvider`, purposes can be chained together.</span></span> <span data-ttu-id="8f3ea-105">就這一點而言， `provider.CreateProtector([ "purpose1", "purpose2" ])` 相當於 `provider.CreateProtector("purpose1").CreateProtector("purpose2")` 。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-105">In this sense, `provider.CreateProtector([ "purpose1", "purpose2" ])` is equivalent to `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span></span>

<span data-ttu-id="8f3ea-106">這可讓您透過資料保護系統來進行一些有趣的階層式關聯性。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-106">This allows for some interesting hierarchical relationships through the data protection system.</span></span> <span data-ttu-id="8f3ea-107">在先前的[SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose)範例中，SecureMessage 元件可以 `provider.CreateProtector("Contoso.Messaging.SecureMessage")` 事先呼叫一次，並將結果快取至私用 `_myProvider` 欄位。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-107">In the earlier example of [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), the SecureMessage component can call `provider.CreateProtector("Contoso.Messaging.SecureMessage")` once up-front and cache the result into a private `_myProvider` field.</span></span> <span data-ttu-id="8f3ea-108">接著，您可以透過對的呼叫來建立未來的保護裝置 `_myProvider.CreateProtector("User: username")` ，並使用這些保護裝置來保護個別訊息。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-108">Future protectors can then be created via calls to `_myProvider.CreateProtector("User: username")`, and these protectors would be used for securing the individual messages.</span></span>

<span data-ttu-id="8f3ea-109">這也可以翻轉。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-109">This can also be flipped.</span></span> <span data-ttu-id="8f3ea-110">假設有一個裝載多個租使用者的單一邏輯應用程式（CMS 看似合理），而且每個租使用者都可以使用自己的驗證和狀態管理系統來設定。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-110">Consider a single logical application which hosts multiple tenants (a CMS seems reasonable), and each tenant can be configured with its own authentication and state management system.</span></span> <span data-ttu-id="8f3ea-111">傘應用程式具有單一主要提供者，它會呼叫 `provider.CreateProtector("Tenant 1")` 並 `provider.CreateProtector("Tenant 2")` 為每個租使用者提供自己的資料保護系統隔離配量。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-111">The umbrella application has a single master provider, and it calls `provider.CreateProtector("Tenant 1")` and `provider.CreateProtector("Tenant 2")` to give each tenant its own isolated slice of the data protection system.</span></span> <span data-ttu-id="8f3ea-112">然後，租使用者可以根據自己的需求來衍生自己的個別保護裝置，但不管他們嘗試如何建立與系統中任何其他租使用者相衝突的保護裝置。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-112">The tenants could then derive their own individual protectors based on their own needs, but no matter how hard they try they cannot create protectors which collide with any other tenant in the system.</span></span> <span data-ttu-id="8f3ea-113">以圖形方式呈現，如下所示。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-113">Graphically, this is represented as below.</span></span>

![多租使用者用途](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> <span data-ttu-id="8f3ea-115">這會假設您的「傘」應用程式會控制哪些 Api 可供個別租使用者使用，而且租使用者無法在伺服器上執行任意程式碼。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-115">This assumes the umbrella application controls which APIs are available to individual tenants and that tenants cannot execute arbitrary code on the server.</span></span> <span data-ttu-id="8f3ea-116">如果租使用者可以執行任意程式碼，他們可能會執行私用反映以中斷隔離保證，或直接讀取主要金鑰內容，並衍生所需的任何子機碼。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-116">If a tenant can execute arbitrary code, they could perform private reflection to break the isolation guarantees, or they could just read the master keying material directly and derive whatever subkeys they desire.</span></span>

<span data-ttu-id="8f3ea-117">資料保護系統實際上會在預設的現成設定中使用一種多租使用者。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-117">The data protection system actually uses a sort of multi-tenancy in its default out-of-the-box configuration.</span></span> <span data-ttu-id="8f3ea-118">根據預設，主要金鑰材料會儲存在背景工作進程帳戶的使用者設定檔資料夾（或登錄中，適用于 IIS 應用程式集區身分識別）。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-118">By default master keying material is stored in the worker process account's user profile folder (or the registry, for IIS application pool identities).</span></span> <span data-ttu-id="8f3ea-119">但是，使用單一帳戶來執行多個應用程式是很常見的，因此所有這些應用程式最後都會共用主要金鑰內容。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-119">But it's actually fairly common to use a single account to run multiple applications, and thus all these applications would end up sharing the master keying material.</span></span> <span data-ttu-id="8f3ea-120">為了解決此問題，資料保護系統會自動插入每個應用程式唯一識別碼，做為整體目的鏈中的第一個元素。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-120">To solve this, the data protection system automatically inserts a unique-per-application identifier as the first element in the overall purpose chain.</span></span> <span data-ttu-id="8f3ea-121">這個隱含的目的是要將每個應用程式有效地視為系統內的唯一租使用者，以[隔離個別應用](xref:security/data-protection/configuration/overview#per-application-isolation)程式，而保護裝置的建立程式看起來與上圖相同。</span><span class="sxs-lookup"><span data-stu-id="8f3ea-121">This implicit purpose serves to [isolate individual applications](xref:security/data-protection/configuration/overview#per-application-isolation) from one another by effectively treating each application as a unique tenant within the system, and the protector creation process looks identical to the image above.</span></span>
