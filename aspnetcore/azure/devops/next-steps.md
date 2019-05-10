---
title: 下一步-使用 ASP.NET Core 和 Azure 的 DevOps
author: CamSoper
description: 其他適用於使用 ASP.NET Core 和 Azure 的 DevOps 學習路徑。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892005"
---
# <a name="next-steps"></a><span data-ttu-id="5f367-103">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5f367-103">Next steps</span></span>

<span data-ttu-id="5f367-104">在本指南中，您可以建立 ASP.NET Core 範例應用程式的 DevOps 管線。</span><span class="sxs-lookup"><span data-stu-id="5f367-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="5f367-105">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="5f367-105">Congratulations!</span></span> <span data-ttu-id="5f367-106">我們希望您滿意學習服務來發佈至 Azure App Service 的 ASP.NET Core web 應用程式，並自動執行變更的持續整合。</span><span class="sxs-lookup"><span data-stu-id="5f367-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="5f367-107">除了 web 裝載及 DevOps，Azure 會有各式各樣的平台為-即服務 (PaaS) 服務的 ASP.NET Core 開發人員很有用。</span><span class="sxs-lookup"><span data-stu-id="5f367-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="5f367-108">本節提供的一些最常使用的服務的簡短概觀。</span><span class="sxs-lookup"><span data-stu-id="5f367-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="5f367-109">儲存體和資料庫</span><span class="sxs-lookup"><span data-stu-id="5f367-109">Storage and databases</span></span>

<span data-ttu-id="5f367-110">[Redis 快取](/azure/redis-cache/)是高輸送量、 低延遲的資料快取 以服務形式提供。</span><span class="sxs-lookup"><span data-stu-id="5f367-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="5f367-111">它可以用於快取頁面輸出，而降低資料庫的要求，並且提供多個執行個體的應用程式的 ASP.NET Core 工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="5f367-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="5f367-112">[Azure 儲存體](/azure/storage/)是 Azure 的可大幅調整的雲端儲存體。</span><span class="sxs-lookup"><span data-stu-id="5f367-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="5f367-113">開發人員可以利用[佇列儲存體](/azure/storage/queues/storage-queues-introduction)可靠的訊息佇列，並[表格儲存體](/azure/storage/tables/table-storage-overview)是專為使用大量半結構化資料集的快速開發所設計的 NoSQL 索引鍵-值存放區。</span><span class="sxs-lookup"><span data-stu-id="5f367-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="5f367-114">[Azure SQL Database](/azure/sql-database/)提供熟悉的關聯式資料庫即服務，使用 Microsoft SQL Server 引擎的功能。</span><span class="sxs-lookup"><span data-stu-id="5f367-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="5f367-115">[Cosmos DB](/azure/cosmos-db/)全域散發的多模型 NoSQL 資料庫服務。</span><span class="sxs-lookup"><span data-stu-id="5f367-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="5f367-116">多個 Api 已推出，包括 SQL API （先前稱為 DocumentDB）、 Cassandra 和 MongoDB。</span><span class="sxs-lookup"><span data-stu-id="5f367-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="5f367-117">身分識別</span><span class="sxs-lookup"><span data-stu-id="5f367-117">Identity</span></span>

<span data-ttu-id="5f367-118">[Azure Active Directory](/azure/active-directory/)並[Azure Active Directory B2C](/azure/active-directory-b2c/)是這兩個身分識別服務。</span><span class="sxs-lookup"><span data-stu-id="5f367-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="5f367-119">Azure Active Directory 專為企業案例，和預期的商務-客戶案例，包括社交網路登入 Azure Active Directory B2C 時，可讓 Azure AD B2B （企業對企業） 共同作業。</span><span class="sxs-lookup"><span data-stu-id="5f367-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="5f367-120">行動訊息</span><span class="sxs-lookup"><span data-stu-id="5f367-120">Mobile</span></span>

<span data-ttu-id="5f367-121">[通知中樞](/azure/notification-hubs/)是多平台、 可調整的推播通知引擎，快速地將數百萬則訊息傳送至各種類型的裝置上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f367-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="5f367-122">Web 基礎結構</span><span class="sxs-lookup"><span data-stu-id="5f367-122">Web infrastructure</span></span>

<span data-ttu-id="5f367-123">[Azure Container Service](/azure/aks/)管理裝載的 Kubernetes 的環境，讓您快速而且容易部署及管理容器化應用程式，而不需要容器協調流程專業知識。</span><span class="sxs-lookup"><span data-stu-id="5f367-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="5f367-124">[Azure 搜尋服務](/azure/search/)用來建立私人和異質性內容上的企業搜尋解決方案。</span><span class="sxs-lookup"><span data-stu-id="5f367-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="5f367-125">[Service Fabric](/azure/service-fabric/)是分散式的系統平台，可讓您輕鬆封裝、 部署及管理可調整且可靠的微服務以及容器。</span><span class="sxs-lookup"><span data-stu-id="5f367-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
