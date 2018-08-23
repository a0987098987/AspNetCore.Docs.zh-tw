---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: 部署資料庫專案 |Microsoft Docs
author: jrjlee
description: 注意： 在許多企業部署案例，您需要能夠將累加式更新發佈至已部署的資料庫。 替代方法是重新建立...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 43fa197a1d5a3cf521f4d2202754ff0d121cebe3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825727"
---
<a name="deploying-database-projects"></a>部署資料庫專案
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> 在許多企業部署案例，您必須能夠將累加式更新發行至已部署的資料庫。 替代方法是重新建立每個部署，這表示您會遺失任何資料，在現有的資料庫上的資料庫。 當您使用 Visual Studio 2010 時，使用 VSDBCMD 是增量資料庫發行的建議的方法。 不過，Visual Studio 和 Web 發行管線 (WPP) 的下一個版本會包含支援累加發行直接的工具。


如果您在 Visual Studio 2010 中開啟 Contactmanager 範例方案，您會看到資料庫專案，包含 [屬性] 資料夾包含四個檔案。

![](deploying-database-projects/_static/image1.png)

搭配專案檔 (*ContactManager.Database.dbproj*在此情況下)，這些檔案來控制組建和部署程序的各個層面：

- *Database.sqlcmdvars*檔案提供的任何部署專案時，您使用的 SQLCMD 變數的值。 每個方案組態 （例如，偵錯和發行） 可以指定不同的.sqlcmdvars 檔案。
- *Database.sqldeployment*檔案提供部署專屬的設定，例如是否要使用您的專案或目的地伺服器的定序中所定義的定序是否要重新建立目的地資料庫每個時間，或只是修改現有的資料庫，使其最新版本，然後依此類推。 每個方案組態，可以指定不同的.sqldeployment 檔案。
- *Database.sqlpermissions*檔案是 XML 文件，您可以使用來定義您想要新增到目標資料庫的任何權限。 所有的方案組態共用相同的.sqlpermissions 檔。
- *Database.sqlsettings*檔案會指定建立資料庫，例如要使用的比較運算子的行為和等等的定序時要使用的資料庫層級屬性。 所有的方案組態共用相同的.sqlsettings 檔案。

倒是值得花點時間在 Visual Studio 中開啟這些檔案，並熟悉內容。

當您建置資料庫專案時，建置程序會建立兩個檔案：

- A*資料庫結構描述*（.dbschema 檔案）。 這會描述您想要建立 XML 格式的資料庫結構描述。
- A*部署資訊清單*（.deploymanifest 檔案）。 這包含建立和部署您的資料庫所需的所有資訊。 它會參考.dbschema 檔案，以及其他資源，例如部署指示 （.sqldeployment 檔案） 和任何預先部署或部署後 SQL 指令碼。

這會顯示這些資源之間的關聯性：

![](deploying-database-projects/_static/image2.png)

如您所見，.sqlsettings 檔案和.sqlpermissions 檔會在建置程序的輸入。 資料庫專案檔，以及這些檔案用來建立資料庫結構描述檔案。 .Sqldeployment 檔案和.sqlcmdvars 檔案經過建置程序不變。 部署資訊清單會指出資料庫結構描述、.sqldeployment 檔案、.sqlcmdvars 檔案，以及任何預先部署或部署後 SQL 指令碼的位置。

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>為什麼使用 VSDBCMD 來部署資料庫專案？

有各種不同的方法，來部署資料庫專案。 不過，不是全部都適用於將資料庫專案部署至企業環境中的遠端伺服器。 請考慮要從資料庫專案部署。 在企業部署案例中，您很可能會想：

- 部署資料庫專案，從遠端位置的能力。
- 能夠對現有的資料庫進行累加式更新。
- 提供包含預先部署指令碼或部署後指令碼的能力。
- 有能力量身打造的多個目的地環境的部署。
- 部署資料庫專案一部分的較大的能力通常編寫指令碼，逐步執行方案部署。

有三種主要的方法，可用來部署資料庫專案：

- 您可以使用的部署功能與在 Visual Studio 2010 資料庫專案類型。 當您建置及部署 Visual Studio 2010 中的資料庫專案時，部署程序會使用部署資訊清單來產生特定的組建組態的 SQL 為基礎的部署檔案。 如果它已不存在，或如果已經存在的話，對資料庫進行任何必要的變更，這會建立資料庫。 您可以使用 SQLCMD.exe 來執行此檔案在目的地伺服器，或者您可以設定 Visual Studio 建立和執行檔案。 此方法的缺點是您只受限於部署設定控制。 您通常也可能需要修改 SQL 部署檔案，來提供環境特定變數的值。 您只能使用這種方法，從電腦與 Visual Studio 2010 安裝，而且開發人員就必須了解，並提供適用於所有的目的地環境的連接字串和認證。
- 您可以使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 至[將資料庫部署為 web 應用程式專案的一部分](https://msdn.microsoft.com/library/dd465343.aspx)。 不過，這種方法是更複雜，如果您想要部署資料庫專案，而不是只複寫到目的地伺服器上現有的本機資料庫。 您可以設定 Web Deploy 才能執行 SQL 部署指令碼會產生資料庫專案，但若要這樣做，您必須建立 web 應用程式專案的自訂 WPP 目標檔案。 這會相當長的複雜度加入部署程序。 此外，Web Deploy 不直接支援累加式更新現有的資料庫。 如需有關這種方法的詳細資訊，請參閱[擴充 Web 發行管線，以封裝資料庫專案部署 SQL 檔案](https://go.microsoft.com/?linkid=9805121)。
- 您可以使用 VSDBCMD 公用程式來部署資料庫，使用資料庫結構描述或部署資訊清單。 您可以呼叫 VSDBCMD.exe 從 MSBuild 目標，它可讓您將資料庫發行更大的指令碼的部署程序的一部分。 您可以覆寫您.sqlcmdvars 檔案與許多其他的資料庫內容從 VSDBCMD 命令，可讓您自訂您的部署，針對不同的環境，而不需要建立多個組建組態中的變數。 VSDBCMD 提供差異化功能，這表示它將只有必要的變更以符合您的資料庫結構描述的目的地資料庫。 VSDBCMD 也提供各種不同的命令列選項，可讓您精細控制部署程序。

本概觀中，您可以看到，使用 MSBuild VSDBCMD 是典型的企業部署案例最適合的方法：

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| 支援遠端部署嗎？ | [是] | 是 | [是] |
| 支援累加式更新？ | [是] | 否 | [是] |
| 支援預先/後部署部署指令碼？ | [是] | 是 | [是] |
| 支援多重環境部署嗎？ | 有限 | 有限 | [是] |
| 支援指令碼的部署嗎？ | 有限 | [是] | [是] |

本主題的其餘部分說明如何使用部署資料庫專案的 MSBuild VSDBCMD 使用。

## <a name="understanding-the-deployment-process"></a>了解部署程序

VSDBCMD 公用程式可讓您部署使用的資料庫結構描述 （.dbschema 檔案）] 或 [部署資訊清單 （.deploymanifest 檔案） 的資料庫。 在實務上，您幾乎一律會使用部署資訊清單中，為部署資訊清單可讓您提供各種不同的部署屬性的預設值，並找出任何的預先部署或部署後 SQL 指令碼想要執行。 例如，此 VSDBCMD 命令用來部署**ContactManager**測試環境中的資料庫伺服器的資料庫：


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


在此情況下：

- **/A** (或 **/Action**) 參數會指定您想要 VSDBCMD。 您可以設定為**匯入**或是**部署**。 **匯入**選項用來產生.dbschema 檔案從現有的資料庫，而**部署**選項用來將.dbschema 檔案部署到目標資料庫。
- **/Manifest** (或 **/ManifestFile**) 參數會識別您想要部署的.deploymanifest 檔案。 如果您想要改為使用.dbschema 檔案，您會使用 **/模型**(或 **/ModelFile**) 切換。
- **/Cs** (或 **/ConnectionString**) 交換器提供目標資料庫伺服器的連接字串。 請注意，這並不包含資料庫的名稱&#x2014;VSDBCMD 需要連線到伺服器，以建立資料庫。它不需要連接到個別的資料庫。 如果.deploymanifest 檔案包含連接字串，您可以省略此參數。 如果您仍要使用參數，參數值會覆寫.deploymanifest 值。
- <strong>/P:TargetDatabase</strong>屬性提供您想要指派給目標資料庫上建立的名稱。 這會覆寫的值<strong>TargetDatabase</strong> .deploymanifest 檔案中的屬性。 您可以使用<strong>/p:</strong> <em>[屬性名稱]</em>.sqlcmdvars 檔案中宣告的語法來設定各種不同的部署屬性，並覆寫任何 SQLCMD 變數。
- **/Dd+** (或 **/DeployToDatabase+**) 參數會指示您想要建立部署，並將它部署到目標環境。 如果您指定 **/dd-**，或省略此參數，VSDBCMD 會產生部署指令碼，但不是將它部署到目標環境。 這個參數通常會造成混淆的來源，並會在下一節中詳細說明。
- **/Script** (或 **/DeploymentScriptFile**) 參數會指定您想要用來產生部署指令碼。 此值不會影響部署程序。

如需有關 VSDBCMD 的詳細資訊，請參閱[VSDBCMD 的命令列參考。（「 部署 」 和 「 結構描述匯入 」） 的 EXE](https://msdn.microsoft.com/library/dd193283.aspx)和[如何： 準備資料庫以進行部署從命令提示字元使用 VSDBCMD。EXE](https://msdn.microsoft.com/library/dd193258.aspx)。

如需如何使用 VSDBCMD，以及在從 MSBuild 專案檔的範例，請參閱[了解建置程序](understanding-the-build-process.md)。 如需如何設定多個環境的資料庫部署設定的範例，請參閱[自訂多個環境的資料庫部署](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。

## <a name="understanding-the-deploytodatabase-switch"></a>了解 DeployToDatabase 交換器

行為 **/dd**或是 **/DeployToDatabase**交換器取決於您使用 VSDBCMD.dbschema 檔案或.deploymanifest 檔案。 如果您使用.dbschema 檔案，則行為會是相當簡單：

- 如果您指定 **/dd+** 或是 **/dd**，VSDBCMD 會產生部署指令碼，並將資料庫部署。
- 如果您指定 **/dd-** 或省略此參數，VSDBCMD 會產生只部署指令碼。

如果您使用.deploymanifest 檔案，則行為是複雜多了。 這是因為.deploymanifest 檔案包含的屬性名稱**DeployToDatabase** ，也會決定是否要部署資料庫。


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


這個屬性的值是根據資料庫專案的屬性設定。 如果您設定**部署動作**要**建立部署指令碼 (.sql)**，此值會是**False**。 如果您設定**部署動作**要**建立部署指令碼 (.sql)，並部署至資料庫**，此值會是 **，則為 True**。

> [!NOTE]
> 這些設定是一種特定的組建組態及平台相關聯。 比方說，如果您設定**偵錯**組態，然後再發佈使用**發行**組態中，將不會使用您的設定。


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> 在此案例中，**部署動作**一律設為**建立部署指令碼 (.sql)**，因為您不想將資料庫部署的 Visual Studio 2010。 亦即**DeployToDatabase**屬性應一律**False**。


當**DeployToDatabase**指定屬性，則 **/dd**交換器將只能覆寫屬性的屬性值是否**false**:

- 如果**DeployToDatabase**屬性是**False**，和您指定 **/dd+** 或 **/dd**，將會覆寫 VSDBCMD **DeployToDatabase**屬性及部署資料庫。
- 如果**DeployToDatabase**屬性是**False**，而且您指定 **/dd-** 或省略此參數，VSDBCMD 不會將資料庫部署。
- 如果**DeployToDatabase**屬性是 **，則為 True**，VSDBCMD 會忽略此參數，並將資料庫部署。
- 在每個案例中，不論是否您要部署的資料庫，以及產生部署指令碼。

## <a name="conclusion"></a>結論

本主題提供在 Visual Studio 2010 資料庫專案的組建和部署程序的概觀。 它也會說明如何使用 VSDBCMD.exe 使用 MSBuild 來支援企業規模的資料庫部署。

如需有關這在實務上的運作方式的詳細資訊，請參閱[自訂多個環境的資料庫部署](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。

## <a name="further-reading"></a>進一步閱讀

如需如何以自訂資料庫部署建立不同的部署組態檔的每個環境的資訊，請參閱[自訂多個環境的資料庫部署](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。 如需有關如何執行部署後指令碼來設定資料庫角色成員資格的指引，請參閱 <<c0> [ 部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 如需管理一些獨特的挑戰，成員資格的指引，資料庫會造成，請參閱 <<c0> [ 部署至企業環境的成員資格資料庫](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

MSDN 上的這些主題會提供更廣泛的指導方針和 Visual Studio 資料庫專案的背景資訊和資料庫部署程序：

- [Visual Studio 2010 SQL Server 資料庫專案](https://msdn.microsoft.com/library/ff678491.aspx)
- [管理資料庫變更](https://msdn.microsoft.com/library/aa833404.aspx)
- [如何： 準備資料庫以進行部署從命令提示字元使用 VSDBCMD。EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [資料庫建置和部署的概觀](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [上一頁](deploying-web-packages.md)
> [下一頁](creating-and-running-a-deployment-command-file.md)
