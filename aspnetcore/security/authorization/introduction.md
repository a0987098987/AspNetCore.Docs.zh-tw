---
title: ASP.NET Core 中的授權簡介
author: rick-anderson
description: 瞭解授權的基本概念，以及授權在 ASP.NET Core 應用程式中的運作方式。
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660710"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="2e872-103">ASP.NET Core 中的授權簡介</span><span class="sxs-lookup"><span data-stu-id="2e872-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="2e872-104">授權是指決定使用者能夠執行之作業的程式。</span><span class="sxs-lookup"><span data-stu-id="2e872-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="2e872-105">例如，系統管理使用者可建立文件庫、加入檔、編輯檔，以及將它們刪除。</span><span class="sxs-lookup"><span data-stu-id="2e872-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="2e872-106">使用媒體櫃的非系統管理使用者只會獲得讀取檔的授權。</span><span class="sxs-lookup"><span data-stu-id="2e872-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="2e872-107">授權是與驗證無關且獨立的。</span><span class="sxs-lookup"><span data-stu-id="2e872-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="2e872-108">不過，授權需要驗證機制。</span><span class="sxs-lookup"><span data-stu-id="2e872-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="2e872-109">「驗證」（Authentication）是查明使用者的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2e872-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="2e872-110">驗證可為目前使用者建立一或多個身分識別。</span><span class="sxs-lookup"><span data-stu-id="2e872-110">Authentication may create one or more identities for the current user.</span></span>

<span data-ttu-id="2e872-111">如需 ASP.NET Core 中驗證的詳細資訊，請參閱 <xref:security/authentication/index>。</span><span class="sxs-lookup"><span data-stu-id="2e872-111">For more information about authentication in ASP.NET Core, see <xref:security/authentication/index>.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="2e872-112">授權類型</span><span class="sxs-lookup"><span data-stu-id="2e872-112">Authorization types</span></span>

<span data-ttu-id="2e872-113">ASP.NET Core 授權提供簡單、宣告式[角色](xref:security/authorization/roles)和以[原則為基礎](xref:security/authorization/policies)的豐富模型。</span><span class="sxs-lookup"><span data-stu-id="2e872-113">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="2e872-114">授權會以需求表示，而處理常式會根據需求評估使用者的宣告。</span><span class="sxs-lookup"><span data-stu-id="2e872-114">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="2e872-115">命令式檢查可以根據簡單的原則或原則，評估使用者嘗試存取的資源的使用者身分識別和屬性。</span><span class="sxs-lookup"><span data-stu-id="2e872-115">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="2e872-116">命名空間</span><span class="sxs-lookup"><span data-stu-id="2e872-116">Namespaces</span></span>

<span data-ttu-id="2e872-117">授權元件（包括 `AuthorizeAttribute` 和 `AllowAnonymousAttribute` 屬性）可在 `Microsoft.AspNetCore.Authorization` 命名空間中找到。</span><span class="sxs-lookup"><span data-stu-id="2e872-117">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="2e872-118">請參閱有關[簡單授權](xref:security/authorization/simple)的檔。</span><span class="sxs-lookup"><span data-stu-id="2e872-118">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
