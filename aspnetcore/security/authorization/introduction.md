---
title: 介紹 ASP.NET Core 的授權
author: rick-anderson
description: 了解授權和 ASP.NET Core 應用程式中授權的運作方式的基本概念。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896975"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="c14b5-103">介紹 ASP.NET Core 的授權</span><span class="sxs-lookup"><span data-stu-id="c14b5-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="c14b5-104">授權是指程序，決定使用者是否能夠執行。</span><span class="sxs-lookup"><span data-stu-id="c14b5-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="c14b5-105">比方說，系統管理使用者可建立文件庫、 將文件新增、 編輯文件，並加以刪除。</span><span class="sxs-lookup"><span data-stu-id="c14b5-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="c14b5-106">使用程式庫的非系統管理使用者只閱讀文件的權限。</span><span class="sxs-lookup"><span data-stu-id="c14b5-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="c14b5-107">授權是與驗證彼此獨立且互不。</span><span class="sxs-lookup"><span data-stu-id="c14b5-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="c14b5-108">不過，授權要求的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="c14b5-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="c14b5-109">驗證是查明使用者是誰的程序。</span><span class="sxs-lookup"><span data-stu-id="c14b5-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="c14b5-110">驗證可能會建立一或多個目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="c14b5-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="c14b5-111">授權類型</span><span class="sxs-lookup"><span data-stu-id="c14b5-111">Authorization types</span></span>

<span data-ttu-id="c14b5-112">ASP.NET Core 授權提供簡單、 宣告式[角色](xref:security/authorization/roles)，以及豐富[以原則為基礎](xref:security/authorization/policies)模型。</span><span class="sxs-lookup"><span data-stu-id="c14b5-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="c14b5-113">授權需求，以表示和處理常式會評估針對要求的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="c14b5-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="c14b5-114">命令式檢查可以根據簡單的原則或原則評估的使用者身分識別 」 和 「 使用者嘗試存取資源的屬性。</span><span class="sxs-lookup"><span data-stu-id="c14b5-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="c14b5-115">命名空間</span><span class="sxs-lookup"><span data-stu-id="c14b5-115">Namespaces</span></span>

<span data-ttu-id="c14b5-116">授權元件，包括`AuthorizeAttribute`並`AllowAnonymousAttribute`，找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。</span><span class="sxs-lookup"><span data-stu-id="c14b5-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="c14b5-117">參閱文件上[簡單授權](xref:security/authorization/simple)。</span><span class="sxs-lookup"><span data-stu-id="c14b5-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
