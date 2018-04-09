---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: 將成員資格資料庫部署至企業環境 |Microsoft 文件
author: jrjlee
description: 本主題說明重要的考量和挑戰需要克服時提供 ASP.NET 應用程式服務資料庫 （多個一般...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>將成員資格資料庫部署至企業環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明重要的考量和挑戰需要克服時提供 ASP.NET 應用程式服務 （通常稱為成員資格資料庫） 的測試、 暫存或實際執行環境中的資料庫。 它也會說明的方法，您可以用來克服這些挑戰。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>當您部署成員資格資料庫時，為何的問題？

在大部分情況下，您設計部署策略的資料庫，您需要考慮的第一件事時您想要部署哪些的資料。 在開發或測試環境中，您可能想要部署使用者帳戶資料，以促進快速和輕鬆測試。 在預備或生產環境中，為幾乎不太可能您會想要部署使用者帳戶資料。

不幸的是，ASP.NET 成員資格資料庫導入做出此決策更加複雜的一些特定挑戰：

- 僅限結構描述的部署將會保留成員資格資料庫處於非運作狀態。 這是因為成員資格資料庫所包含的某些組態資料 (在**aspnet\_SchemaVersions**資料表) 的資料庫所需才能運作。 因此，如果您執行僅限結構描述部署的成員資格資料庫，以排除使用者帳戶資料，您必須在執行部署後指令碼中加入必要的設定資料。
- 根據成員資格資料庫的設定方式，成員資格提供者可能會使用電腦金鑰加密的密碼，並將它們儲存在資料庫中。 在此情況下，您部署與資料庫的任何使用者帳戶資料將會導致在目的地伺服器上使用。 基於這個理由，部署使用者帳戶的資料不支援的案例。

## <a name="choosing-a-membership-database-strategy"></a>選擇成員資格資料庫策略

當您選擇如何佈建企業伺服器環境中的成員資格資料庫，請使用下列方針：

- 可能的情況下，不會部署成員資格資料庫。 相反地，手動在目標資料庫伺服器上建立成員資格資料庫。 如果您未自訂成員資格資料庫結構描述，您可以只建立一個新的 situ 在目的地使用[ASP.NET SQL Server 註冊工具 (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。
- 如果您不有任何選項，但若要部署成員資格資料庫&#x2014;比方說，如果您所做的資料庫結構描述的大量修改&#x2014;您應該執行的成員資格資料庫，以排除使用者帳戶資料，僅限結構描述部署，然後執行部署後指令碼中加入任何必要的組態資料。 您可以在這些方法中找到廣泛指導[How to： 部署 ASP.NET 成員資格資料庫但不包括使用者帳戶](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)。

請務必記住*的成員資格資料庫結構描述很可能是相當靜態*。 即使您已自訂成員資格資料庫，也不太可能，您必須更新以規則為基礎的結構描述&#x2014;不會變更與 web 應用程式或資料庫專案中的程式碼的頻率相同。 因此，您應該不需要包含任何自動或逐步部署程序中的成員資格資料庫。

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>使用 VSDBCMD 更新成員資格資料庫結構描述

如果您在第一個部署後修改的成員資格資料庫結構，您可能不想要重新部署資料庫使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy)。 Web 部署的資料庫部署功能不包含的功能，可讓目的地資料庫的差異更新&#x2014;相反地，Web Deploy 必須卸除並重新建立資料庫。 這表示，就會失去任何現有的使用者帳戶資料，也就是在預備或生產環境中通常不需要。

替代方案是使用 VSDBCMD 公用程式來更新目的地資料庫的結構描述。 VSDBCMD 包括兩個重要的功能。 首先，它可以將現有的資料庫結構描述匯.dbschema 檔案。 第二，它可以部署.dbschema 檔案至現有的資料庫差異的更新，這表示它只會使目標資料庫的最新狀態所需的變更，而且不會遺失任何資料。

您可以使用下列高階步驟來更新成員資格資料庫結構描述：

1. 使用 VSDBCMD**匯入**產生.dbschema 來源成員資格資料庫檔案的動作。 此程序所述[How to： 從命令提示字元中匯入結構描述](https://msdn.microsoft.com/library/dd172135.aspx)。
2. 使用 VSDBCMD**部署**部署.dbschema 檔案到目的地的成員資格資料庫中的動作。 此程序所述[VSDBCMD 的命令列參考。EXE （部署和結構描述匯入）](https://msdn.microsoft.com/library/dd193283.aspx)。

## <a name="conclusion"></a>結論

本主題將說明一些您需要佈建在各種目標環境中的 ASP.NET 成員資格資料庫時可能面臨的挑戰。 特別是，說明為什麼僅限結構描述部署將會保留成員資格資料庫處於非運作狀態，而且為什麼部署使用者帳戶的資料不支援。 本主題也會顯示如何佈建、 部署及更新成員資格資料庫中不同的案例的指引。

## <a name="further-reading"></a>進一步閱讀

如需詳細指引和 VSDBCMD 的使用方式的範例，請參閱[VSDBCMD 的命令列參考。EXE （部署和結構描述匯入）](https://msdn.microsoft.com/library/dd193283.aspx)和[How to： 從命令提示字元中匯入結構描述](https://msdn.microsoft.com/library/dd172135.aspx)。 如需有關使用 aspnet\_regsql.exe 建立成員資格資料庫，請參閱[ASP.NET SQL Server 註冊工具 (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。 多個一般指引部署成員資格資料庫的詳細資訊，請參閱[How to： 部署 ASP.NET 成員資格資料庫但不包括使用者帳戶](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)。

> [!div class="step-by-step"]
> [上一頁](deploying-database-role-memberships-to-test-environments.md)
> [下一頁](excluding-files-and-folders-from-deployment.md)
