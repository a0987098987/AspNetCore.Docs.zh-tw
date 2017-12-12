---
title: "授權簡介"
author: rick-anderson
description: "本文件提供授權的基本說明，並說明授權與 ASP.NET Core 關聯的方式。"
keywords: "ASP.NET Core 授權"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a><span data-ttu-id="ca41a-104">簡介</span><span class="sxs-lookup"><span data-stu-id="ca41a-104">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="ca41a-105">授權是指程序，決定使用者是否能夠執行。</span><span class="sxs-lookup"><span data-stu-id="ca41a-105">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="ca41a-106">比方說，允許系統管理使用者建立文件庫，加入文件、 編輯文件，並刪除它們。</span><span class="sxs-lookup"><span data-stu-id="ca41a-106">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="ca41a-107">使用 程式庫非系統管理使用者只能讀取文件的權限。</span><span class="sxs-lookup"><span data-stu-id="ca41a-107">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="ca41a-108">授權是垂直和獨立於驗證，這是使用者確實的程序。</span><span class="sxs-lookup"><span data-stu-id="ca41a-108">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="ca41a-109">驗證可能會建立一或多個目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="ca41a-109">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="ca41a-110">授權類型</span><span class="sxs-lookup"><span data-stu-id="ca41a-110">Authorization Types</span></span>

<span data-ttu-id="ca41a-111">ASP.NET Core 授權提供簡單的宣告式[角色](roles.md)和[豐富的原則型](policies.md)模型。</span><span class="sxs-lookup"><span data-stu-id="ca41a-111">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="ca41a-112">授權需求，以表示和處理常式評估根據需求的使用者宣告。</span><span class="sxs-lookup"><span data-stu-id="ca41a-112">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="ca41a-113">命令式檢查可以根據簡單的原則或原則評估使用者識別與使用者嘗試存取資源的內容。</span><span class="sxs-lookup"><span data-stu-id="ca41a-113">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="ca41a-114">命名空間</span><span class="sxs-lookup"><span data-stu-id="ca41a-114">Namespaces</span></span>

<span data-ttu-id="ca41a-115">授權元件，包括`AuthorizeAttribute`和`AllowAnonymousAttribute`找不到屬性中`Microsoft.AspNetCore.Authorization`命名空間。</span><span class="sxs-lookup"><span data-stu-id="ca41a-115">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
