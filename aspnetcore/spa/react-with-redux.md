---
title: React-with-Redux 專案範本搭配 ASP.NET Core 使用
author: SteveSandersonMS
description: 了解如何開始使用 React with Redux 和 create-react-app 適用的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
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
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076287"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="99280-103">React-with-Redux 專案範本搭配 ASP.NET Core 使用</span><span class="sxs-lookup"><span data-stu-id="99280-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="99280-104">本文件與 ASP.NET Core 2.0 包含的 React-with-Redux 專案範本無關。</span><span class="sxs-lookup"><span data-stu-id="99280-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="99280-105">而是討論您可以手動更新的新版 React-with-Redux 範本。</span><span class="sxs-lookup"><span data-stu-id="99280-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="99280-106">ASP.NET Core 2.1 預設會包含此範本。</span><span class="sxs-lookup"><span data-stu-id="99280-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="99280-107">更新的 React-with-Redux 專案範本很適合用來設計 ASP.NET Core 應用程式，而且可利用 React、Redux 和 [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 慣例來實作一個功能多樣的用戶端使用者介面 (UI)。</span><span class="sxs-lookup"><span data-stu-id="99280-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="99280-108">除了專案建立命令，所有與 React-with-Redux 範本有關的資訊，都與 React 範本相同。</span><span class="sxs-lookup"><span data-stu-id="99280-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="99280-109">若要建立這種專案類型，請執行 `dotnet new reactredux` 而不是 `dotnet new react`。</span><span class="sxs-lookup"><span data-stu-id="99280-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="99280-110">對於這兩個以 React 為基礎的範本，如需深入了解其通用功能，請參閱 [React 範本文件](xref:spa/react)。</span><span class="sxs-lookup"><span data-stu-id="99280-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
