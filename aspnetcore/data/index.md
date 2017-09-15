---
title: "在 ASP.NET Core 中處理資料"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe0ba2ef
ms.technology: aspnet
ms.prod: asp.net-core
ms.openlocfilehash: 3566127476289ae085a9161132b103638bc9b068
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-data-in-aspnet-core"></a><span data-ttu-id="9a2c1-103">在 ASP.NET Core 中處理資料</span><span class="sxs-lookup"><span data-stu-id="9a2c1-103">Working with Data in ASP.NET Core</span></span> 

*   [<span data-ttu-id="9a2c1-104">使用 Visual Studio 的 ASP.NET Core 與 Entity Framework Core 的使用者入門</span><span class="sxs-lookup"><span data-stu-id="9a2c1-104">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](ef-mvc/index.md)
    *   [<span data-ttu-id="9a2c1-105">快速入門</span><span class="sxs-lookup"><span data-stu-id="9a2c1-105">Getting started</span></span>](ef-mvc/intro.md)
    *   [<span data-ttu-id="9a2c1-106">建立、讀取、更新和刪除作業</span><span class="sxs-lookup"><span data-stu-id="9a2c1-106">Create, Read, Update, and Delete operations</span></span>](ef-mvc/crud.md)
    *   [<span data-ttu-id="9a2c1-107">排序、篩選、分頁與群組</span><span class="sxs-lookup"><span data-stu-id="9a2c1-107">Sorting, filtering, paging, and grouping</span></span>](ef-mvc/sort-filter-page.md)
    *   [<span data-ttu-id="9a2c1-108">移轉</span><span class="sxs-lookup"><span data-stu-id="9a2c1-108">Migrations</span></span>](ef-mvc/migrations.md)
    *   [<span data-ttu-id="9a2c1-109">建立複雜的資料模型</span><span class="sxs-lookup"><span data-stu-id="9a2c1-109">Creating a complex data model</span></span>](ef-mvc/complex-data-model.md)
    *   [<span data-ttu-id="9a2c1-110">讀取相關資料</span><span class="sxs-lookup"><span data-stu-id="9a2c1-110">Reading related data</span></span>](ef-mvc/read-related-data.md)
    *   [<span data-ttu-id="9a2c1-111">更新相關資料</span><span class="sxs-lookup"><span data-stu-id="9a2c1-111">Updating related data</span></span>](ef-mvc/update-related-data.md)
    *   [<span data-ttu-id="9a2c1-112">處理並行存取衝突</span><span class="sxs-lookup"><span data-stu-id="9a2c1-112">Handling concurrency conflicts</span></span>](ef-mvc/concurrency.md)
    *   [<span data-ttu-id="9a2c1-113">繼承</span><span class="sxs-lookup"><span data-stu-id="9a2c1-113">Inheritance</span></span>](ef-mvc/inheritance.md)
    *   [<span data-ttu-id="9a2c1-114">進階主題</span><span class="sxs-lookup"><span data-stu-id="9a2c1-114">Advanced topics</span></span>](ef-mvc/advanced.md)
* <span data-ttu-id="9a2c1-115">[ASP.NET Core 與 EF Core - 新的資料庫](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core 文件網站)</span><span class="sxs-lookup"><span data-stu-id="9a2c1-115">[ASP.NET Core with EF Core - new database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core documentation site)</span></span>
* <span data-ttu-id="9a2c1-116">[ASP.NET Core 與 EF Core - 現有資料庫](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core 文件網站)</span><span class="sxs-lookup"><span data-stu-id="9a2c1-116">[ASP.NET Core with EF Core - existing database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core documentation site)</span></span>
*   [<span data-ttu-id="9a2c1-117">ASP.NET Core 與 Entity Framework 6 使用者入門</span><span class="sxs-lookup"><span data-stu-id="9a2c1-117">Getting Started with ASP.NET Core and Entity Framework 6</span></span>](entity-framework-6.md)
*   [<span data-ttu-id="9a2c1-118">Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="9a2c1-118">Azure Storage</span></span>](azure-storage/index.md)
    *   [<span data-ttu-id="9a2c1-119">使用 Visual Studio 連線服務新增 Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="9a2c1-119">Adding Azure Storage by Using Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-azure-tools-connected-services-storage/)
    *   [<span data-ttu-id="9a2c1-120">開始使用 Azure Blob 儲存體和 Visual Studio 連線服務</span><span class="sxs-lookup"><span data-stu-id="9a2c1-120">Get Started with Azure Blob storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)
    *   [<span data-ttu-id="9a2c1-121">開始使用佇列儲存體和 Visual Studio 連線服務</span><span class="sxs-lookup"><span data-stu-id="9a2c1-121">Get Started with Queue Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-queues/)
    *   [<span data-ttu-id="9a2c1-122">如何：開始使用 Azure 資料表儲存體和 Visual Studio 連線的服務</span><span class="sxs-lookup"><span data-stu-id="9a2c1-122">How to Get Started with Azure Table Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-tables/)
