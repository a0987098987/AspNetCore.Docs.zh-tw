---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: 將成員資格資料庫部署至企業環境 |Microsoft Docs
author: jrjlee
description: 本主題說明的重要考量和挑戰必須克服當您佈建的 ASP.NET 應用程式服務資料庫 （多個一般...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 307375843c51f31d3d8ae0f2ef0a17a3e58d3a64
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831738"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>將成員資格資料庫部署至企業環境
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題說明的重要考量和挑戰必須克服您佈建的 ASP.NET 應用程式服務 （通常稱為 「 成員資格資料庫 」） 的測試、 預備或生產環境中的資料庫時。 此外，本文也將說明您可用來克服這些挑戰的方法。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>當您部署的成員資格資料庫時，為何的問題？

在大部分情況下，您設計的部署策略，是資料庫，您需要考慮的第一件事時，您想要部署哪些的資料。 在開發或測試環境中，您可能想要將使用者帳戶資料，以便輕鬆快速地測試部署。 在預備或生產環境中，它是非常不太可能，您會想要部署使用者帳戶資料。

不幸的是，ASP.NET 成員資格資料庫導入一些特殊的挑戰，讓這項決定更加複雜：

- 僅限結構描述的部署會讓成員資格資料庫處於非運作狀態。 這是因為成員資格資料庫所包含的某些組態資料 (在**aspnet\_SchemaVersions**資料表)，資料庫會正常運作。 因此，如果您執行僅限結構描述的成員資格資料庫部署，以排除使用者帳戶資料，您必須在執行部署後指令碼中新增必要的設定資料。
- 根據您的成員資格資料庫的設定方式，成員資格提供者可能會使用電腦金鑰加密的密碼，並將它們儲存在資料庫中。 在此情況下，您將部署與資料庫的任何使用者帳戶資料將會變成目的地伺服器上無法使用。 基於這個理由，部署使用者帳戶資料是不支援的案例。

## <a name="choosing-a-membership-database-strategy"></a>選擇成員資格資料庫策略

當您選擇如何佈建企業伺服器環境中的成員資格資料庫時，請使用下列方針：

- 可能的話，不會部署成員資格資料庫。 相反地，手動在目標資料庫伺服器上建立成員資格資料庫。 如果您未自訂的成員資格資料庫結構描述，您可以只建立一個新的 situ 在目的地中使用[ASP.NET SQL Server 註冊工具 (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。
- 如果您不有任何選項，但若要部署的成員資格資料庫&#x2014;例如，如果您已大量修改資料庫結構描述&#x2014;您應該執行僅限結構描述部署的成員資格資料庫中，若要排除使用者帳戶資料，然後執行部署後指令碼加入任何必要的組態資料。 您可以找到這些方法在廣泛的指導方針[如何： 部署 ASP.NET 成員資格資料庫但不包括使用者帳戶](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)。

請務必記住*的成員資格資料庫結構描述很可能是相當靜態*。 即使您已自訂成員資格資料庫，也不太可能，您必須更新以規則為基礎的結構描述&#x2014;它不會隨著與 web 應用程式或資料庫專案中的程式碼的頻率相同。 因此，您應該不需要包含任何自動或單一步驟的部署程序中的成員資格資料庫。

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>使用 VSDBCMD 更新成員資格資料庫結構描述

如果您在您第一次部署之後修改成員資格資料庫的結構，可能不想要使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 來重新部署的資料庫。 Web Deploy 的資料庫部署功能不包含能夠進行到目的地資料庫的差異更新&#x2014;相反地，Web Deploy 必須卸除並重新建立資料庫。 這表示您會遺失任何現有使用者帳戶的資料，也就是通常不想要在預備或生產環境中。

替代方法是使用 VSDBCMD 公用程式來更新您的目的地資料庫的結構描述。 VSDBCMD 包含兩個重要的功能。 首先，它可以將現有的資料庫結構描述匯.dbschema 檔案。 第二，它可以部署到現有的資料庫的.dbschema 檔案，做為差異的更新，這表示它只會將目標資料庫的最新狀態所需的變更，而且您不會遺失任何資料。

若要更新成員資格資料庫結構描述，您可以使用下列高階步驟：

1. 使用 VSDBCMD**匯入**產生.dbschema 檔案來源的成員資格資料庫的動作。 此程序所述[如何： 從命令提示字元中匯入結構描述](https://msdn.microsoft.com/library/dd172135.aspx)。
2. 使用 VSDBCMD**部署**部署.dbschema 檔案到目的地的成員資格資料庫中的動作。 此程序所述[VSDBCMD 的命令列參考。（「 部署 」 和 「 結構描述匯入 」） 的 EXE](https://msdn.microsoft.com/library/dd193283.aspx)。

## <a name="conclusion"></a>結論

本主題將描述一些您需要佈建在各種目標環境中的 ASP.NET 成員資格資料庫時，您可能會面臨的挑戰。 特別是，它會說明為什麼僅限結構描述的部署會將保留在成員資格資料庫處於的狀態，以及為什麼要部署使用者帳戶資料不支援。 本主題也會呈現有關如何佈建、 部署及更新成員資格資料庫在不同案例中的指引。

## <a name="further-reading"></a>進一步閱讀

如需詳細的指引和範例，示範如何使用 VSDBCMD，請參閱[VSDBCMD 的命令列參考。（「 部署 」 和 「 結構描述匯入 」） 的 EXE](https://msdn.microsoft.com/library/dd193283.aspx)並[如何： 從命令提示字元中匯入結構描述](https://msdn.microsoft.com/library/dd172135.aspx)。 如需有關使用 aspnet\_regsql.exe 建立成員資格資料庫，請參閱[ASP.NET SQL Server 註冊工具 (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)。 如需更多一般指引部署成員資格資料庫的詳細資訊，請參閱[如何： 部署 ASP.NET 成員資格資料庫但不包括使用者帳戶](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx)。

> [!div class="step-by-step"]
> [上一頁](deploying-database-role-memberships-to-test-environments.md)
> [下一頁](excluding-files-and-folders-from-deployment.md)
