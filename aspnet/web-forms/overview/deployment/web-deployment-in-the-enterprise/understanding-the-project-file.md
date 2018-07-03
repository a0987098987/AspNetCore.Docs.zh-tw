---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: 了解專案檔 |Microsoft Docs
author: jrjlee
description: Microsoft Build Engine (MSBuild) 專案檔之間的組建和部署程序的核心。 本主題開頭的 MSBuild 概觀為...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 89c5c7906ccfc453195b788cbc6393dc74cda1fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377200"
---
<a name="understanding-the-project-file"></a>了解專案檔
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) 專案檔之間的組建和部署程序的核心。 本主題開頭的 MSBuild 和專案檔案的概念性概觀。 它會描述當您使用專案檔和它的運作方式說明如何使用專案檔，以及在部署真實世界應用程式的範例將會遇到的重要元件。
> 
> 您將學到什麼：
> 
> - 如何 MSBuild 會使用 MSBuild 專案檔來建置專案。
> - 如何 MSBuild 整合與部署技術，例如 Internet Information Services (IIS) Web Deployment Tool (Web Deploy)。
> - 如何了解專案檔的重要元件。
> - 您如何也可以使用專案檔建置及部署複雜的應用程式。


## <a name="msbuild-and-the-project-file"></a>MSBuild 和專案檔

當您建立並在 Visual Studio 中建置解決方案時，Visual Studio 會使用 MSBuild 來建置您的方案中的每個專案。 每個 Visual Studio 專案包含 MSBuild 專案檔，以反映的專案類型的副檔名&#x2014;，例如 C# 專案 (.csproj)、 Visual Basic.NET 專案 (.vbproj) 或資料庫專案 (.dbproj)。 若要建置專案時，MSBuild 必須處理與專案相關聯的專案檔。 專案檔是 XML 文件，其中包含所有資訊和指示 MSBuild 需要以建置專案，若要加入，平台需求、 版本設定資訊、 web 伺服器或資料庫伺服器設定，內容和必須執行的工作。

MSBuild 專案檔根據[MSBuild XML 結構描述](https://msdn.microsoft.com/library/5dy88c2e.aspx)，並因此建置程序是完全開放且透明。 此外，您不需要安裝 Visual Studio 才能使用 MSBuild 引擎&#x2014;MSBuild.exe 的可執行檔是.NET framework 的一部分，而且您就可以從命令提示字元執行它。 身為開發人員，您可以製作您自己的 MSBuild 專案檔，使用 MSBuild XML 結構描述強加精密且更精細控制如何建置及部署您的專案。 這些自訂的專案檔在 Visual Studio 會自動產生的專案檔案完全相同的方式。

> [!NOTE]
> 您也可以使用與 Team Build 的服務在 Team Foundation Server (TFS) 中的 MSBuild 專案檔。 例如，您可以使用持續整合 (CI) 案例中的專案檔案來自動化部署到測試環境，當新的程式碼簽入。 如需詳細資訊，請參閱 <<c0> [ 自動化 Web 部署設定 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。


### <a name="project-file-naming-conventions"></a>專案檔命名慣例

當您建立您自己的專案檔時，您可以使用任何您喜歡的副檔名。 不過，若要簡化您的解決方案供其他人了解，您應該使用這些常見的慣例：

- 當您建立專案檔來建置專案時，請使用.proj 延伸模組。
- 當您建立可重複使用的專案檔匯入至其他專案檔案時，請使用.targets 副檔名。 副檔名為.targets 檔案通常不建立任何項目本身，它們只包含您可以匯入至.proj 檔案的指示。

### <a name="integration-with-deployment-technologies"></a>與部署技術的整合

如果您先前曾經使用 web 應用程式專案，在 Visual Studio 2010 中，例如 ASP.NET web 應用程式和 ASP.NET MVC web 應用程式，就會知道這些專案包含封裝和部署至目標環境的 web 應用程式的內建支援。 **屬性**頁面包含這些專案**封裝/發行 Web**和**封裝/發行 SQL**索引標籤可供您設定如何組成您封裝及部署應用程式。 這會顯示**封裝/發行 Web**  索引標籤：

![](understanding-the-project-file/_static/image1.png)

這些功能的基礎技術稱為做為 Web 發行管線 (WPP)。 WPP 基本上會將 MSBuild 與[Web Deploy](https://go.microsoft.com/?linkid=9805122)一起，以提供完整的建置、 封裝及部署程序，為 web 應用程式。

好消息是，您可以利用 WPP 提供當您建立 web 專案的自訂專案檔案的整合點。 您可以在專案檔案中，可讓您建置專案、 建立 web 部署套件，並透過單一的專案檔和 MSBuild 的單一呼叫，在遠端伺服器上安裝這些套件包含部署指示。 您也可以呼叫任何其他可執行檔做為建置流程的一部分。 例如，您可以執行 VSDBCMD.exe 命令列工具，從部署資料庫結構描述檔案。 在本主題的過程中，您會看到如何使用這些功能，以符合您企業的部署案例的需求的利用。

> [!NOTE]
> 如需有關 web 應用程式部署程序的運作方式的詳細資訊，請參閱 < [ASP.NET Web 應用程式專案部署概觀](https://msdn.microsoft.com/library/dd394698.aspx)。


## <a name="the-anatomy-of-a-project-file"></a>專案檔的結構

您查看更詳細地建置處理序之前，倒是值得花一些時間才能熟悉 MSBuild 專案檔的基本結構。 本節概述時檢閱、 編輯或建立的專案檔，將會遇到的常見項目。 特別是，您將了解：

- 如何使用*屬性*管理建置程序的變數。
- 如何使用*項目*找出建置處理序的輸入，例如程式碼檔案。
- 如何使用*目標*並*工作*以提供執行指示 msbuild，使用*屬性*和*項目*中其他地方定義專案檔中。

這會顯示在 MSBuild 專案檔中的索引鍵項目之間的關聯性：

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>專案項目

[專案](https://msdn.microsoft.com/library/bcxfsh87.aspx)項目是每個專案檔的根項目。 除了找出專案檔的 XML 結構描述**專案**元素可以包含屬性，以指定進入點，供建置程序。 例如，在[連絡管理員範例解決方案](the-contact-manager-solution.md)，則*Publish.proj*檔案會指定開始組建時，應該先呼叫名為目標**FullPublish**。


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>屬性和條件

通常需要提供許多不同的功能，才能成功建置及部署您的專案資訊的專案檔。 這項資訊可能包括伺服器名稱、 連接字串、 認證、 組建組態、 來源和目的地檔案路徑，以及任何其他您想要包含支援自訂的資訊。 在專案檔中，屬性必須定義內[PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx)項目。 MSBuild 屬性是由索引鍵-值配對所組成。 內**PropertyGroup**項目，項目名稱會定義屬性索引鍵和項目的內容定義的屬性值。 例如，您可以在其中定義具名的屬性**ServerName**並**ConnectionString**儲存靜態的伺服器名稱和連接字串。


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


若要擷取的屬性值，您可以使用格式<strong>$(</strong><em>PropertyName</em><strong>)</strong><em>。</em>例如，若要擷取的值<strong>ServerName</strong>屬性，您會輸入：


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> 您會看到如何的範例，以及何時使用本主題稍後的屬性值。


在專案檔中內嵌為靜態屬性的資訊不一定理想的方式，來管理建置程序。 在許多案例中，您會想要從其他來源取得資訊，或讓使用者提供的資訊，從命令提示字元。 MSBuild 可讓您指定做為命令列參數的任何屬性值。 例如，使用者無法提供值給**ServerName**當他或她 MSBuild.exe 從命令列執行。


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> 如需有關引數和參數可以搭配 MSBuild.exe 使用的詳細資訊，請參閱 < [MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。


您可以使用相同的屬性語法以取得環境變數和內建的專案屬性的值。 針對您所定義的許多常用的屬性，您可以在您的專案檔中使用它們，包含相關的參數名稱。 例如，若要擷取目前的專案平台&#x2014;，例如**x86**或**AnyCpu**&#x2014;您可以包含 **$ （platform)** 中的屬性參考您的專案檔。 如需詳細資訊，請參閱 <<c0> [ 建置命令和屬性的巨集](https://msdn.microsoft.com/library/c02as0cs.aspx)，[通用的 MSBuild 專案屬性](https://msdn.microsoft.com/library/bb629394.aspx)，並[保留的屬性](https://msdn.microsoft.com/library/ms164309.aspx)。

屬性通常用於搭配*條件*。 大部分的 MSBuild 項目支援**條件**屬性，可讓您指定的 MSBuild 應該評估之項目的準則。 例如，請考慮此屬性定義：


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


當 MSBuild 處理此屬性定義時，它會先檢查以查看是否 **$(OutputRoot)** 屬性的值為止。 如果屬性值為空白&#x2014;亦即，使用者未提供值給此屬性&#x2014;條件評估為 **，則為 true**屬性值設定為與 **...\Publish\Out**。如果使用者已經提供值，這個屬性，條件評估為**false**並不會使用靜態屬性值。

如需有關您可以在其中指定條件的不同方式的詳細資訊，請參閱[MSBuild 條件](https://msdn.microsoft.com/library/7szfhaft.aspx)。

### <a name="items-and-item-groups"></a>項目和項目群組

其中一個重要角色的專案檔是定義建置程序的輸入。 通常，這些輸入是檔案&#x2014;程式碼檔案、 組態檔、 指令檔和任何其他您需要處理，或做為複製的檔案在建置程序的一部分。 在 MSBuild 專案結構描述中，以表示這些輸入[項目](https://msdn.microsoft.com/library/ms164283.aspx)項目。 在專案檔中，必須定義項目內[ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx)項目。 就如同**屬性**項目，您可以命名**項目**項目您隨心所欲。 不過，您必須指定**Include**來識別的檔案或萬用字元項目所代表的屬性。


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


藉由指定多個**項目**具有相同名稱的項目，您要有效地建立資源的具名的清單。 若要查看此動作的好方法是查看其中一個 Visual Studio 會建立專案檔。 例如， *ContactManager.Mvc.csproj*範例方案中的檔案包含大量的項目群組，每個都有數個相同的已命名**項目**項目。


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


如此一來，專案檔會指示 MSBuild 建構需要相同的方式處理的檔案清單&#x2014;**參考**清單包含組件，必須先成功的組建，準備好**編譯**清單包含必須編譯的程式碼檔案和**內容**清單包含必須複製不變的資源。 我們將探討如何建置程序的參考位置，並使用本主題稍後的這些項目。

也可以包含項目的項目[ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx)子項目。 這些是使用者定義的索引鍵 / 值組，而且基本上代表該項目的特定屬性。 比方說，許多**編譯**專案檔中的項目包含**DependentUpon**子項目。


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> 除了使用者建立的項目中繼資料，所有的項目指派建立各種常見的中繼資料。 如需詳細資訊，請參閱[已知的項目中繼資料](https://msdn.microsoft.com/library/ms164313.aspx)。


您可以建立**ItemGroup**根層級中的項目**專案**項目或在特定**目標**項目。 **ItemGroup**項目也支援**條件**屬性，可讓您調整建置程序，根據條件，例如專案組態或平台所需的輸入。

### <a name="targets-and-tasks"></a>目標和工作

在 MSBuild 結構描述[任務](https://msdn.microsoft.com/library/77f2hx1s.aspx)項目代表的個別組建指令 （或工作）。 MSBuild 包含許多預先定義的工作。 例如: 

- **複製**工作將檔案複製到新位置。
- **Csc**工作叫用 Visual C# 編譯器。
- **Vbc**工作叫用 Visual Basic 編譯器。
- **Exec**工作會執行指定的程式。
- **訊息**工作會將訊息寫入至記錄器。

> [!NOTE]
> 立即可用的工作的完整詳細資訊，請參閱[MSBuild 工作參考](https://msdn.microsoft.com/library/7z253716.aspx)。 如需有關工作，包括如何建立您自己自訂的工作，請參閱[MSBuild 工作](https://msdn.microsoft.com/library/ms171466.aspx)。


工作必須一律包含在[目標](https://msdn.microsoft.com/library/t50z2hka.aspx)項目。 A**目標**項目是一組循序執行的一或多個工作和專案檔案可以包含多個目標。 當您想要執行工作或一組工作時，您就會叫用包含它們的目標。 例如，假設您有一個簡單的專案檔，會記錄訊息。


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


您可以使用連線，叫用命令列中，從目標 **/t**參數來指定目標。


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


或者，您可以在其中加入**DefaultTargets**屬性設定為**專案**項目，來指定您想要叫用的目標。


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


在此情況下，您不需要指定從命令列目標。 您只可以指定專案檔，並叫用 MSBuild **FullPublish**為您的目標。


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


目標和工作可以包含**條件**屬性。 因此，您可以選擇略過整個目標或個別的工作如果符合特定條件。

一般而言，當您建立有用的工作和目標，您必須參考的屬性和您已在專案檔中其他地方定義的項目：

- 若要使用的屬性值，請輸入<strong>$(</strong><em>PropertyName</em><strong>)</strong>，其中<em>PropertyName</em>名稱<strong>屬性</strong>項目或參數的名稱。
- 若要使用的項目，輸入<strong>@(</strong><em>ItemName</em><strong>)</strong>，其中<em>ItemName</em>名稱<strong>項目</strong>項目。

> [!NOTE]
> 請記住，是否您建立多個項目具有相同名稱時，您要建置清單。 相反地，如果您建立多個屬性具有相同名稱時，您所提供的最後一個屬性值會覆寫任何先前的屬性具有相同名稱&#x2014;屬性只能包含單一值。


例如，在*Publish.proj*檔案中的範例解決方案，看看**BuildProjects**目標。


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


在此範例中，您可以觀察下列重點：

- 如果**BuildingInTeamBuild**參數指定，且其值為 **，則為 true**，則這個目標內的工作將會執行。
- 目標包含的單一執行個體[MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx)工作。 此工作可讓您建立其他的 MSBuild 專案。
- **ProjectsToBuild**項目傳遞給工作。 此項目可能代表專案或方案檔，所有已定義的一份**ProjectsToBuild**項目中的項目群組的項目。 在此情況下， **ProjectsToBuild**項目是指單一方案檔。

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- 屬性值傳遞給**MSBuild**工作包含名為的參數**OutputRoot**並**Configuration**。 如果它們不是，這些都會設為參數值，如果提供或靜態屬性值。

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

您也可以看到**MSBuild**工作會叫用名為的目標**建置**。 這是很廣泛地在 Visual Studio 專案檔，並且可用於您在自訂專案檔案中，例如的數個內建目標之一**建置**，**清除**，**重建**，並**發行**。 您將了解更多關於使用目標和工作，以控制建置程序，以及關於**MSBuild**工作特別的是，本主題稍後的。

> [!NOTE]
> 如需有關目標的詳細資訊，請參閱[MSBuild 目標](https://msdn.microsoft.com/library/ms171462.aspx)。


## <a name="splitting-project-files-to-support-multiple-environments"></a>分割的專案檔，以支援多個環境

假設您想要能夠將方案部署到多個環境，例如測試伺服器、 暫存的平台，以及實際執行環境。 設定這些環境之間可能大幅不同&#x2014;只是根據伺服器名稱、 連接字串，並依此類推，但也可能根據認證、 安全性設定，以及許多其他因素。 如果您需要定期執行這項操作，不過它不是編輯專案檔中的多個屬性，每次切換的目標環境很有利。 也不是理想的解決方案需要永無止盡的清單，提供給建置程序的屬性值。

所幸還有另一個方法。 MSBuild 可讓您將您的組建組態分割到多個專案檔。 若要查看其運作方式，在範例解決方案中，請注意，有兩個自訂的專案檔：

- *Publish.proj*，其中包含屬性，項目，以及目標，通用於所有環境。
- *Env Dev.proj*，其中包含開發人員環境特有的屬性。

現在，請注意*Publish.proj*檔案包含[匯入](https://msdn.microsoft.com/library/92x05xfs.aspx)元素，緊接在開頭**專案**標記。


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**匯入**元素用來匯入到目前的 MSBuild 專案檔案的另一個 MSBuild 專案檔的內容。 在此情況下， **TargetEnvPropsFile**參數會提供您想要匯入的專案檔的檔名。 當您執行 MSBuild 時，您便可以提供這個參數的值。


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


這會有效地將兩個檔案的內容合併到單一專案檔案。 您可以使用這個方法，建立包含您的通用的組建組態的專案檔案和包含環境特定屬性的多個增補的專案檔。 如此一來，只要使用不同的參數值執行的命令可讓您將方案部署至不同的環境。

![](understanding-the-project-file/_static/image3.png)

分割您的專案檔案，以這種方式是最好的作法是遵循。 它可讓開發人員，同時避免重複的通用的建置屬性，跨多個專案檔中執行單一命令，將部署到多個環境。

> [!NOTE]
> 如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="conclusion"></a>結論

本主題提供 MSBuild 專案檔的一般簡介，並說明如何建立您自己的自訂專案檔，來控制建置程序。 它也導入的概念將專案檔案分割成通用的組建指示和環境特定建置屬性，讓您輕鬆建置及將專案部署到多個目的地。

下一個主題中，[了解建置程序](understanding-the-build-process.md)，提供更多深入了解如何使用專案檔，以控制組建和部署，以及在逐步引導您透過實際的複雜度層級部署的解決方案。

## <a name="further-reading"></a>進一步閱讀

專案檔和 WPP 更深入的簡介，請參閱[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William bartholomew< /，ISBN: 978-0-7356-4524-0。

> [!div class="step-by-step"]
> [上一頁](setting-up-the-contact-manager-solution.md)
> [下一頁](understanding-the-build-process.md)
