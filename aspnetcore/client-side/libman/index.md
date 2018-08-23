---
title: 使用 LibMan 讓 ASP.NET Core 取得用戶端程式庫
author: scottaddie
description: 了解如何使用程式庫 (LibMan) 在 ASP.NET Core 專案中安裝用戶端程式庫資產。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: b21ab0dbeda043dceda5376bcd95ccd334707c5a
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909980"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="3997e-103">使用 LibMan 讓 ASP.NET Core 取得用戶端程式庫</span><span class="sxs-lookup"><span data-stu-id="3997e-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="3997e-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3997e-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3997e-105">程式庫管理員 (LibMan) 是輕量型的用戶端程式庫取得工具。</span><span class="sxs-lookup"><span data-stu-id="3997e-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="3997e-106">LibMan 會從檔案系統或[內容傳遞網路 (CDN)](https://wikipedia.org/wiki/Content_delivery_network)下載熱門的程式庫和架構。</span><span class="sxs-lookup"><span data-stu-id="3997e-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="3997e-107">支援的 CDN 包含 [CDNJS](https://cdnjs.com/) 和 [unpkg](https://unpkg.com/#/)。</span><span class="sxs-lookup"><span data-stu-id="3997e-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="3997e-108">所選程式庫檔會擷取並放置在 ASP.NET Core 專案中的適當位置。</span><span class="sxs-lookup"><span data-stu-id="3997e-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="3997e-109">LibMan 使用案例</span><span class="sxs-lookup"><span data-stu-id="3997e-109">LibMan use cases</span></span>

<span data-ttu-id="3997e-110">LibMan 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="3997e-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="3997e-111">只會下載您所需的程式庫檔。</span><span class="sxs-lookup"><span data-stu-id="3997e-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="3997e-112">其他工具，例如 [Node.js](https://nodejs.org)、[npm](https://www.npmjs.com)和 [WebPack](https://webpack.js.org)，並不需取得程式庫中的部分檔案。</span><span class="sxs-lookup"><span data-stu-id="3997e-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="3997e-113">檔案可以放在特定位置，而不必依靠建置工作，或手動複製檔案。</span><span class="sxs-lookup"><span data-stu-id="3997e-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="3997e-114">如需 LibMan 優點的詳細資訊，請觀看[Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s) (Visual Studio 2017 中的新式前端 Web 開發：LibMan 區段)。</span><span class="sxs-lookup"><span data-stu-id="3997e-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="3997e-115">LibMan 不是套件管理系統。</span><span class="sxs-lookup"><span data-stu-id="3997e-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="3997e-116">如果您已使用套件管理員，如 npm 或 [yarn](https://yarnpkg.com)，請繼續使用。</span><span class="sxs-lookup"><span data-stu-id="3997e-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="3997e-117">LibMan 的開發目的並非在於取代這些工具。</span><span class="sxs-lookup"><span data-stu-id="3997e-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3997e-118">其他資源</span><span class="sxs-lookup"><span data-stu-id="3997e-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="3997e-119">LibMan GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="3997e-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
