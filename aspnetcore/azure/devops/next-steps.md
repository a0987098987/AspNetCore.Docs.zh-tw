---
title: 後續步驟-使用 ASP.NET Core 和 Azure DevOps
author: CamSoper
description: 使用 ASP.NET Core 和 Azure 進行 DevOps 的其他學習路徑。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659471"
---
# <a name="next-steps"></a>後續步驟

在本指南中，您已建立 ASP.NET Core 範例應用程式的 DevOps 管線。 恭喜！ 我們希望您喜歡學習如何發佈 ASP.NET Core web 應用程式來 Azure App Service，並將變更的持續整合自動化。

除了 web 裝載和 DevOps 之外，Azure 還提供各式各樣的平臺即服務（PaaS）服務，適用于 ASP.NET Core 開發人員。 本節提供一些最常用服務的簡要概述。

## <a name="storage-and-databases"></a>儲存體和資料庫

[Redis Cache](/azure/redis-cache/)是以服務形式提供的高輸送量、低延遲的資料快取。 它可以用來快取頁面輸出、減少資料庫要求，以及在多個應用程式實例之間提供 ASP.NET Core 會話狀態。

[Azure 儲存體](/azure/storage/)是 Azure 可大規模調整的雲端儲存體。 開發人員可以利用[佇列儲存體](/azure/storage/queues/storage-queues-introduction)來提供可靠的訊息佇列，而[表格儲存體](/azure/storage/tables/table-storage-overview)則是 NoSQL 索引鍵值存放區，其設計目的是要使用大量的半結構化資料集進行快速開發。

[Azure SQL Database](/azure/sql-database/)使用 Microsoft SQL Server 引擎，提供熟悉的關係資料庫功能做為服務。

[Cosmos DB](/azure/cosmos-db/)全域散發的多模型 NoSQL 資料庫服務。 有多個 Api 可供使用，包括 SQL API （先前稱為 DocumentDB）、Cassandra 和 MongoDB。

## <a name="identity"></a>身分識別

[Azure Active Directory](/azure/active-directory/)和[Azure Active Directory B2C](/azure/active-directory-b2c/)都是身分識別服務。 Azure Active Directory 是針對企業案例而設計，可讓 Azure AD B2B （企業對企業）共同作業，而 Azure Active Directory B2C 則是預期的企業對客戶案例，包括社交網路登入。

## <a name="mobile"></a>行動

[通知中樞](/azure/notification-hubs/)是多平臺、可擴充的推播通知引擎，可快速地將數百萬個訊息傳送到在各種類型的裝置上執行的應用程式。

## <a name="web-infrastructure"></a>Web 基礎結構

[Azure Container Service](/azure/aks/)會管理您託管的 Kubernetes 環境，讓您快速且輕鬆地部署和管理容器化應用程式，而不需要容器協調流程專業知識。

[Azure 搜尋服務](/azure/search/)是用來透過私人的非公開內容來建立企業搜尋解決方案。

[Service Fabric](/azure/service-fabric/)是分散式系統平臺，可讓您輕鬆封裝、部署及管理可調整和可靠的微服務和容器。
