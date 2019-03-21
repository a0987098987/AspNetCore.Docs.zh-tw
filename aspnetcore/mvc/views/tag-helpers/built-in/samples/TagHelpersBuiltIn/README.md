---
ms.openlocfilehash: 3113220696eb3e3cb8c126cd1287e60203d96378
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208330"
---
# <a name="aspnet-core-built-in-tag-helpers-sample"></a><span data-ttu-id="a022e-101">ASP.NET Core 內建的標記協助程式範例</span><span class="sxs-lookup"><span data-stu-id="a022e-101">ASP.NET Core Built-in Tag Helpers Sample</span></span>

<span data-ttu-id="a022e-102">此範例說明搭配 MVC 和 Razor 頁面使用內建標籤協助程式的各種變化：</span><span class="sxs-lookup"><span data-stu-id="a022e-102">This sample illustrates variations of the built-in Tag Helpers with both MVC and Razor Pages:</span></span>

- [<span data-ttu-id="a022e-103">錨點標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a022e-103">Anchor Tag Helper</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper)
  - [<span data-ttu-id="a022e-104">asp-action</span><span class="sxs-lookup"><span data-stu-id="a022e-104">asp-action</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-action)
  - [<span data-ttu-id="a022e-105">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="a022e-105">asp-all-route-data</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-all-route-data)
  - [<span data-ttu-id="a022e-106">asp-area</span><span class="sxs-lookup"><span data-stu-id="a022e-106">asp-area</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-area)
  - [<span data-ttu-id="a022e-107">asp-controller</span><span class="sxs-lookup"><span data-stu-id="a022e-107">asp-controller</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-controller)
  - [<span data-ttu-id="a022e-108">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="a022e-108">asp-fragment</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-fragment)
  - [<span data-ttu-id="a022e-109">asp-host</span><span class="sxs-lookup"><span data-stu-id="a022e-109">asp-host</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-host)
  - [<span data-ttu-id="a022e-110">asp-page</span><span class="sxs-lookup"><span data-stu-id="a022e-110">asp-page</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-page)
  - [<span data-ttu-id="a022e-111">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="a022e-111">asp-page-handler</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-page-handler)
  - [<span data-ttu-id="a022e-112">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="a022e-112">asp-protocol</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-protocol)
  - [<span data-ttu-id="a022e-113">asp-route</span><span class="sxs-lookup"><span data-stu-id="a022e-113">asp-route</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-route)
  - [<span data-ttu-id="a022e-114">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="a022e-114">asp-route-{value}</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper#asp-route-value)
- [<span data-ttu-id="a022e-115">部分標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a022e-115">Partial Tag Helper</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/partial-tag-helper)
  - [<span data-ttu-id="a022e-116">for</span><span class="sxs-lookup"><span data-stu-id="a022e-116">for</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/partial-tag-helper#for)
  - [<span data-ttu-id="a022e-117">model</span><span class="sxs-lookup"><span data-stu-id="a022e-117">model</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/partial-tag-helper#model)
  - [<span data-ttu-id="a022e-118">name</span><span class="sxs-lookup"><span data-stu-id="a022e-118">name</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/partial-tag-helper#name)
  - [<span data-ttu-id="a022e-119">view-data</span><span class="sxs-lookup"><span data-stu-id="a022e-119">view-data</span></span>](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/built-in/partial-tag-helper#view-data)
