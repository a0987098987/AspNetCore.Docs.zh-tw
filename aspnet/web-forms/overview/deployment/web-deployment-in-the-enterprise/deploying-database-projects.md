---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: 部署資料庫專案 |Microsoft 文件
author: jrjlee
description: 注意： 在許多企業部署案例，您需要將累加更新發行到已部署資料庫的能力。 替代方法是重新建立...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: a0b3871ea098b549271bce2b9d5f0c24f9ca8a9c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882497"
---
<a name="deploying-database-projects"></a>部署資料庫專案
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> 在許多企業部署案例，您必須要能夠將累加更新發行到已部署的資料庫。 替代方法是重新建立每個部署，這表示您會遺失任何資料，在現有的資料庫上的資料庫。 當您使用 Visual Studio 2010，請使用 VSDBCMD 是增量資料庫發行的建議的方法。 不過下, 一版的 Visual Studio 和 Web 發行管線 (WPP) 會包含支援累加發行直接的工具。


如果您在 Visual Studio 2010 中開啟連絡人管理員範例方案，您會看到資料庫專案包含四個檔案內容資料夾。

![](deploying-database-projects/_static/image1.png)

與專案檔 (*ContactManager.Database.dbproj*在此情況下)，這些檔案可控制建置和部署處理程序的各個層面：

- *Database.sqlcmdvars*檔案提供可用來部署專案時，您使用任何 SQLCMD 變數的值。 每個方案組態 （例如，偵錯和發行） 可以指定不同的.sqlcmdvars 檔案。
- *Database.sqldeployment*檔案提供部署特定設定，例如是否要使用您的專案或目的地伺服器的定序中定義的定序是否要重新建立目的地資料庫，每個時間，或直接修改現有的資料庫，使其維持最新狀態，以此類推。 每個方案組態時，可以指定不同的.sqldeployment 檔案。
- *Database.sqlpermissions*檔案是 XML 文件可讓您定義您想要新增到目標資料庫的任何權限。 所有的方案組態共用相同.sqlpermissions 檔。
- *Database.sqlsettings*檔案會指定建立資料庫，例如要使用的比較運算子的行為和等等的定序時要使用的資料庫層級屬性。 所有的方案組態共用相同.sqlsettings 檔案。

它是值得花一點時間來進行 Visual Studio 中開啟這些檔案，並熟悉內容。

當您建置資料庫專案時，建置程序會建立兩個檔案：

- A*資料庫結構描述*（.dbschema 檔案）。 這會描述您想要建立 XML 格式的資料庫結構描述。
- A*部署資訊清單*（.deploymanifest 檔案）。 這包含建立及部署資料庫所需的所有資訊。 它會參考.dbschema 檔案以及其他資源，例如部署指示 （.sqldeployment 檔案） 和任何的預先部署或部署後 SQL 指令碼。

這會顯示這些資源之間的關聯性：

![](deploying-database-projects/_static/image2.png)

如您所見，.sqlsettings 和.sqlpermissions 檔案會在建置程序的輸入。 資料庫專案檔，以及這些檔案可用來建立資料庫結構描述檔案。 .Sqldeployment 和.sqlcmdvars 檔案通過建置處理序保持不變。 部署資訊清單指出資料庫結構描述、.sqldeployment 檔案、.sqlcmdvars 檔案，與任何預先部署或部署後 SQL 指令碼的位置。

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>為何要使用 VSDBCMD 部署資料庫專案嗎？

有各種不同的方法來部署資料庫專案。 不過，並非所有都適用於將資料庫專案部署至企業環境中的遠端伺服器。 請考慮要從資料庫專案部署。 在企業部署案例中，您可能想要：

- 部署資料庫專案，從遠端位置的能力。
- 提供對現有的資料庫進行累加式更新的能力。
- 提供包含預先部署指令碼或部署後指令碼的能力。
- 可以修改部署至多個目的地環境的能力。
- 部署資料庫專案的較大一部分的能力通常編寫指令碼，逐步執行方案部署。

有三個主要的方式，您可以使用來部署資料庫專案：

- 您可以使用部署功能與在 Visual Studio 2010 資料庫專案類型。 當您建置並部署在 Visual Studio 2010 資料庫專案時，部署程序會使用部署資訊清單產生特定的組建組態的 SQL 為基礎的部署檔案。 如果它已不存在或對資料庫進行任何必要的變更，如果已經存在的話，這會建立資料庫。 您可以使用 SQLCMD.exe 來執行在目的地伺服器上，這個檔案，或您可以設定 Visual Studio 來建立和執行檔案。 這個方法的缺點是，僅有有限控制部署設定。 您通常也可能需要修改 SQL 部署檔案，以提供環境特定變數的值。 您只能使用與 Visual Studio 2010 安裝，這種方法，從電腦和開發人員會想要知道，並提供所有目的地環境中的連接字串和認證。
- 您可以使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 至[將資料庫部署為 web 應用程式專案的一部分](https://msdn.microsoft.com/library/dd465343.aspx)。 不過，這個方法是如果您想要部署資料庫專案，而不是只複寫到目的地伺服器上現有的本機資料庫更加複雜。 您可以設定 Web Deploy 才能執行 SQL 部署指令碼的資料庫專案所產生，但若要這樣做，您必須建立您的 web 應用程式專案的自訂 WPP 目標檔案。 這會將大量的複雜度加入部署程序。 此外，Web Deploy 不直接支援累加式更新至現有的資料庫。 如需這種方法的詳細資訊，請參閱[擴充封裝資料庫專案將 Web 發行管線部署 SQL 檔案](https://go.microsoft.com/?linkid=9805121)。
- 您可以使用 VSDBCMD 公用程式來部署資料庫，使用的資料庫結構描述或部署資訊清單。 您可以呼叫 VSDBCMD.exe 從 MSBuild 目標，這可讓您將資料庫發行更大、 已編寫指令碼的部署程序的一部分。 您可以覆寫您.sqlcmdvars 檔案與許多其他的資料庫內容從 VSDBCMD 命令，可讓您自訂您的部署不同環境，而不需要建立多個組建組態中的變數。 VSDBCMD 提供差異化功能，這表示它會變更只需要對齊資料庫結構描述的目的地資料庫。 VSDBCMD 也提供各種不同的命令列選項，可讓您在部署程序更細微的控制。

從本概觀中，您可以發現，使用 MSBuild VSDBCMD 最適合典型的企業部署案例的方法：

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| 支援遠端部署嗎？ | [是] | 是 | [是] |
| 支援累加式更新嗎？ | [是] | 否 | [是] |
| 支援預先/部署後指令碼嗎？ | [是] | 是 | [是] |
| 支援多重環境部署嗎？ | 有限 | 有限 | [是] |
| 支援已編寫指令碼的部署嗎？ | 有限 | [是] | [是] |

本主題的其餘部分說明如何使用部署資料庫專案的 MSBuild VSDBCMD 使用。

## <a name="understanding-the-deployment-process"></a>了解部署程序

VSDBCMD 公用程式可讓您使用部署資料庫的資料庫結構描述 （.dbschema 檔案） 或部署資訊清單 （.deploymanifest 檔案）。 在實務上，您幾乎都將使用部署資訊清單中，當部署資訊清單可讓您針對不同的部署屬性提供預設值，並識別任何的預先部署或部署後 SQL 指令碼想要執行。 例如，此 VSDBCMD 命令可用來部署**ContactManager**測試環境中的資料庫伺服器的資料庫：


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


在此情況下：

- **/A** (或 **/Action**) 參數指定您想要 VSDBCMD。 您可以設定為**匯入**或**部署**。 **匯入**選項用來產生.dbschema 檔案從現有的資料庫，而**部署**選項用來將.dbschema 檔案部署到目標資料庫。
- **/資訊清單**(或 **/ManifestFile**) 參數識別您想要部署的.deploymanifest 檔案。 如果您想要改用.dbschema 檔案，您會使用 **/模型**(或 **/ModelFile**) 切換。
- **/Cs** (或 **/ConnectionString**) 交換器提供目標資料庫伺服器的連接字串。 請注意，這不包含資料庫的名稱&#x2014;VSDBCMD 需要連接到伺服器，以建立資料庫。它不需要連接到個別的資料庫。 如果.deploymanifest 檔案包含連接字串，您可以省略這個參數。 如果您要使用參數，參數值會覆寫.deploymanifest 值。
- <strong>/P:TargetDatabase</strong>屬性提供您想要指派到目標資料庫上建立的名稱。 這會覆寫的值<strong>TargetDatabase</strong> .deploymanifest 檔案中的屬性。 您可以使用<strong>/p:</strong> <em>[屬性名稱]</em>.sqlcmdvars 檔案中宣告的語法來設定各種不同的部署屬性，並覆寫任何 SQLCMD 變數。
- **/Dd+** (或 **/DeployToDatabase+**) 參數表示您想要建立部署，並將它部署至目標環境。 如果您指定 **/dd-**，或省略這個參數，VSDBCMD 將會產生部署指令碼，但不是將它部署至目標環境。 此參數通常是混淆的來源，並且會在下一節中詳細說明。
- **/指令碼**(或 **/DeploymentScriptFile**) 參數指定您想要用來產生部署指令碼。 此值不會影響部署程序。

如需有關 VSDBCMD 的詳細資訊，請參閱[VSDBCMD 的命令列參考。EXE （部署和結構描述匯入）](https://msdn.microsoft.com/library/dd193283.aspx)和[如何： 準備資料庫以進行部署的命令提示字元使用 VSDBCMD。EXE](https://msdn.microsoft.com/library/dd193258.aspx)。

如需如何使用 VSDBCMD 從 MSBuild 專案檔的範例，請參閱[瞭解建置程序](understanding-the-build-process.md)。 如需如何設定資料庫部署設定多個環境的範例，請參閱[自訂資料庫部署多個環境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。

## <a name="understanding-the-deploytodatabase-switch"></a>了解 DeployToDatabase 交換器

行為 **/dd**或 **/DeployToDatabase**交換器取決於您是否使用 VSDBCMD 搭配.dbschema 檔案或.deploymanifest 檔案。 如果您使用.dbschema 檔案，則行為是相當簡單：

- 如果您指定 **/dd+** 或 **/dd**，VSDBCMD 會產生部署指令碼及部署資料庫。
- 如果您指定 **/dd-** 或省略參數，VSDBCMD 皆會產生部署指令碼只。

如果您使用.deploymanifest 檔案，則行為是很複雜。 這是因為.deploymanifest 檔案包含屬性名稱**DeployToDatabase** ，也會決定是否要部署資料庫。


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


根據資料庫專案的屬性來設定這個屬性的值。 如果您設定**部署動作**至**建立部署指令碼 (.sql)**，此值會是**False**。 如果您設定**部署動作**至**建立部署指令碼 (.sql) 並部署到資料庫**，值將是**True**。

> [!NOTE]
> 這些設定會與特定組建組態與平台關聯。 比方說，如果您設定**偵錯**組態，然後將發行使用**發行**組態中，將不會使用您的設定。


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> 在此案例中，**部署動作**應該一律設定為**建立部署指令碼 (.sql)**，因為您不想將資料庫部署 Visual Studio 2010。 換句話說， **DeployToDatabase**屬性應永遠為**False**。


當**DeployToDatabase**指定屬性， **/dd**交換器將只覆寫屬性的屬性值是否**false**:

- 如果**DeployToDatabase**屬性是**False**，而且您指定 **/dd+** 或 **/dd**，將會覆寫 VSDBCMD **DeployToDatabase**屬性及部署資料庫。
- 如果**DeployToDatabase**屬性是**False**，而且您指定 **/dd-** 或省略參數，VSDBCMD 不會將資料庫部署。
- 如果**DeployToDatabase**屬性是**True**，VSDBCMD 將會忽略此參數，部署資料庫。
- 在各案例中，不論是否您要部署的資料庫也會產生部署指令碼。

## <a name="conclusion"></a>結論

本主題提供在 Visual Studio 2010 資料庫專案的組建和部署程序的概觀。 它也說明如何使用 VSDBCMD.exe 的 MSBuild 支援企業級資料庫部署。

如需有關這實際上的運作方式的詳細資訊，請參閱[自訂資料庫部署多個環境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。

## <a name="further-reading"></a>進一步閱讀

如需如何建立不同的部署組態檔，每個環境中自訂資料庫部署資訊，請參閱[自訂資料庫部署多個環境](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)。 如需有關如何執行部署後指令碼來設定資料庫角色成員資格的指引，請參閱[部署資料庫角色成員資格測試環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)。 如需指引管理某些項挑戰，那該成員資格資料庫強制執行，請參閱[部署至企業環境的成員資格資料庫](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)。

MSDN 上的這些主題會提供更廣泛的指導方針和 Visual Studio 資料庫專案的背景資訊和資料庫部署程序：

- [Visual Studio 2010 SQL Server 資料庫專案](https://msdn.microsoft.com/library/ff678491.aspx)
- [管理資料庫變更](https://msdn.microsoft.com/library/aa833404.aspx)
- [如何： 準備資料庫以進行部署的命令提示字元使用 VSDBCMD。EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [資料庫建置和部署的概觀](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [上一頁](deploying-web-packages.md)
> [下一頁](creating-and-running-a-deployment-command-file.md)
