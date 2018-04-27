---
title: 在 ASP.NET Core 授權簡介
author: rick-anderson
description: 了解授權和授權 ASP.NET Core 應用程式中的運作方式的基本概念。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="680d3-103">在 ASP.NET Core 授權簡介</span><span class="sxs-lookup"><span data-stu-id="680d3-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="680d3-104">授權是指程序，決定使用者是否能夠執行。</span><span class="sxs-lookup"><span data-stu-id="680d3-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="680d3-105">比方說，允許系統管理使用者建立文件庫，加入文件、 編輯文件，並刪除它們。</span><span class="sxs-lookup"><span data-stu-id="680d3-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="680d3-106">使用 程式庫非系統管理使用者只能讀取文件的權限。</span><span class="sxs-lookup"><span data-stu-id="680d3-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="680d3-107">授權是垂直和獨立於驗證。</span><span class="sxs-lookup"><span data-stu-id="680d3-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="680d3-108">不過，授權要求的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="680d3-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="680d3-109">驗證是演練使用者的程序。</span><span class="sxs-lookup"><span data-stu-id="680d3-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="680d3-110">驗證可能會建立一或多個目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="680d3-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="680d3-111">授權類型</span><span class="sxs-lookup"><span data-stu-id="680d3-111">Authorization types</span></span>

<span data-ttu-id="680d3-112">ASP.NET Core 授權提供簡單、 宣告式[角色](xref:security/authorization/roles)和 rich[以原則為基礎](xref:security/authorization/policies)模型。</span><span class="sxs-lookup"><span data-stu-id="680d3-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="680d3-113">授權需求，以表示和處理常式評估根據需求的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="680d3-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="680d3-114">命令式檢查可以根據簡單的原則或原則評估使用者識別與使用者嘗試存取資源的內容。</span><span class="sxs-lookup"><span data-stu-id="680d3-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="680d3-115">命名空間</span><span class="sxs-lookup"><span data-stu-id="680d3-115">Namespaces</span></span>

<span data-ttu-id="680d3-116">授權元件，包括`AuthorizeAttribute`和`AllowAnonymousAttribute`，找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。</span><span class="sxs-lookup"><span data-stu-id="680d3-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="680d3-117">參考的文件上[簡單授權](xref:security/authorization/simple)。</span><span class="sxs-lookup"><span data-stu-id="680d3-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
