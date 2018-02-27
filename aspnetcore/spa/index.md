---
title: "使用單頁應用程式範本"
author: SteveSandersonMS
description: "了解如何安裝與開始使用 ASP.NET Core 單頁應用程式 (SPA) 專案範本。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a><span data-ttu-id="e1978-103">使用單頁應用程式範本</span><span class="sxs-lookup"><span data-stu-id="e1978-103">Use the Single Page Application templates</span></span>

> [!NOTE]
> <span data-ttu-id="e1978-104">發行的 .NET Core 2.0.x SDK 中包含了 Angular、React 與 React with Redux 的舊版專案範本。</span><span class="sxs-lookup"><span data-stu-id="e1978-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="e1978-105">本文件與這些舊版的專案範本無關。</span><span class="sxs-lookup"><span data-stu-id="e1978-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="e1978-106">本文件適用於最新的 Angular、React 與 React with Redux 專案範本，這些範本可手動安裝到 ASP.NET Core 2.0 中。</span><span class="sxs-lookup"><span data-stu-id="e1978-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="e1978-107">根據預設，範本會隨附於 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="e1978-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1978-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="e1978-108">Prerequisites</span></span>

* <span data-ttu-id="e1978-109">[.NET Core SDK](https://www.microsoft.com/net/download)，2.0.0 版或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1978-109">[.NET Core SDK](https://www.microsoft.com/net/download), version 2.0.0 or later</span></span>
* <span data-ttu-id="e1978-110">[Node.js](https://nodejs.org)，版本 6 或更新版本</span><span class="sxs-lookup"><span data-stu-id="e1978-110">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="e1978-111">安裝</span><span class="sxs-lookup"><span data-stu-id="e1978-111">Installation</span></span>

<span data-ttu-id="e1978-112">如果您有 ASP.NET Core 2.0，請執行下列命令以安裝 Angular、React 與 React with Redux 的已更新 ASP.NET Core 範本：</span><span class="sxs-lookup"><span data-stu-id="e1978-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="e1978-113">使用範本</span><span class="sxs-lookup"><span data-stu-id="e1978-113">Use the templates</span></span>

- [<span data-ttu-id="e1978-114">使用 Angular 專案範本</span><span class="sxs-lookup"><span data-stu-id="e1978-114">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="e1978-115">使用 React 專案範本</span><span class="sxs-lookup"><span data-stu-id="e1978-115">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="e1978-116">使用 React with Redux 專案範本</span><span class="sxs-lookup"><span data-stu-id="e1978-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
