---
title: 使用包含 ASP.NET Core 的單頁應用程式範本
author: SteveSandersonMS
description: 了解如何安裝與開始使用 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291441"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="5e7eb-103">使用包含 ASP.NET Core 的單頁應用程式範本</span><span class="sxs-lookup"><span data-stu-id="5e7eb-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="5e7eb-104">發行的 .NET Core 2.0.x SDK 中包含了 Angular、React 與 React with Redux 的舊版專案範本。</span><span class="sxs-lookup"><span data-stu-id="5e7eb-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="5e7eb-105">本文件與這些舊版的專案範本無關。</span><span class="sxs-lookup"><span data-stu-id="5e7eb-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="5e7eb-106">本文件適用於最新的 Angular、React 與 React with Redux 專案範本，這些範本可手動安裝到 ASP.NET Core 2.0 中。</span><span class="sxs-lookup"><span data-stu-id="5e7eb-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="5e7eb-107">根據預設，範本會隨附於 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="5e7eb-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="5e7eb-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="5e7eb-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="5e7eb-109">[Node.js](https://nodejs.org)，版本 6 或更新版本</span><span class="sxs-lookup"><span data-stu-id="5e7eb-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="5e7eb-110">安裝</span><span class="sxs-lookup"><span data-stu-id="5e7eb-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5e7eb-111">範本已隨 ASP.NET Core 2.1 安裝。</span><span class="sxs-lookup"><span data-stu-id="5e7eb-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5e7eb-112">如果您有 ASP.NET Core 2.0，請執行下列命令以安裝 Angular、React 與 React with Redux 的已更新 ASP.NET Core 範本：</span><span class="sxs-lookup"><span data-stu-id="5e7eb-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="5e7eb-113">使用範本</span><span class="sxs-lookup"><span data-stu-id="5e7eb-113">Use the templates</span></span>

* [<span data-ttu-id="5e7eb-114">使用 Angular 專案範本</span><span class="sxs-lookup"><span data-stu-id="5e7eb-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="5e7eb-115">使用 React 專案範本</span><span class="sxs-lookup"><span data-stu-id="5e7eb-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="5e7eb-116">使用 React with Redux 專案範本</span><span class="sxs-lookup"><span data-stu-id="5e7eb-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
