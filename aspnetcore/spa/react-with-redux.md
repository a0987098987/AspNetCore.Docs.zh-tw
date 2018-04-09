---
title: 使用 ASP.NET Core React 與 Redux 專案範本
author: SteveSandersonMS
description: 了解如何開始使用 ASP.NET Core 單一頁面應用程式 (SPA) 專案範本進行 React Redux 與建立 react 應用程式。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 9abfbfe5be69d3145de453d9d9e56ea35eec64ed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="726e0-103">使用 ASP.NET Core React 與 Redux 專案範本</span><span class="sxs-lookup"><span data-stu-id="726e0-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="726e0-104">這份文件不需 React 與 Redux 專案範本包含 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="726e0-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="726e0-105">所以，您可以手動更新較新的回應與 Redux 範本的相關。</span><span class="sxs-lookup"><span data-stu-id="726e0-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="726e0-106">此範本預設包含在 ASP.NET Core 2.1 之中。</span><span class="sxs-lookup"><span data-stu-id="726e0-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="726e0-107">使用 ASP.NET Core 應用程式回應，Redux，更新的 React 與 Redux 專案範本提供方便的起點和[建立 react 應用程式](https://github.com/facebookincubator/create-react-app)(CRA) 慣例，來實作豐富的用戶端使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="726e0-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="726e0-108">除了專案建立命令中，所有的相關資訊回應與 Redux 範本 React 範本相同。</span><span class="sxs-lookup"><span data-stu-id="726e0-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="726e0-109">若要建立這種專案類型，請執行`dotnet new reactredux`而不是`dotnet new react`。</span><span class="sxs-lookup"><span data-stu-id="726e0-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="726e0-110">如需這兩個 React 為基礎的範本通用的功能的詳細資訊，請參閱[回應範本文件](xref:spa/react)。</span><span class="sxs-lookup"><span data-stu-id="726e0-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
