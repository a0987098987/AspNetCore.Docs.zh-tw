---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: "了解專案檔 |Microsoft 文件"
author: jrjlee
description: "Microsoft Build Engine (MSBuild) 專案檔之間的組建和部署程序的核心。 本主題一開始的 MSBuild 概觀..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="understanding-the-project-file"></a>了解專案檔
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) 專案檔之間的組建和部署程序的核心。 本主題一開始 MSBuild 和專案檔的概觀。 它會描述當您使用專案檔和它的運作方式透過使用專案檔將真實世界應用程式部署的範例將會遇到的重要元件。
> 
> 您將學習：
> 
> - 如何 MSBuild 會使用 MSBuild 專案檔建置專案。
> - 如何與網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 這類的部署技術整合 MSBuild。
> - 如何了解專案檔案的重要元件。
> - 如何使用專案檔建置並部署複雜的應用程式。


## <a name="msbuild-and-the-project-file"></a>MSBuild 和專案檔

當您建立並建置 Visual Studio 中的方案時，Visual Studio 會使用 MSBuild 建置您的方案中每個專案。 每個 Visual Studio 專案包含 MSBuild 專案檔，副檔名為檔案，以反映專案 & #x 2014年的類型; 例如，C# 專案 (.csproj)、 Visual Basic.NET 專案 (.vbproj) 或資料庫專案 (.dbproj)。 若要建置專案時，MSBuild 必須處理與專案相關的專案檔。 專案檔是 XML 文件，其中包含所有資訊和指示 MSBuild 能夠建置您的專案，例如平台需求、 版本設定資訊、 web 伺服器或資料庫伺服器設定，包括內容和必須執行的工作。

MSBuild 專案檔根據[MSBuild XML 結構描述](https://msdn.microsoft.com/library/5dy88c2e.aspx)，如此一來，在建置程序是完全開啟和透明和。 此外，您不需要安裝 Visual Studio 時才能使用 MSBuild 引擎 & #x 2014年; MSBuild.exe 可執行檔是.NET framework 的一部分，而且您可以從命令提示字元中執行它。 身為開發人員，您就可以建立您自己的 MSBuild 專案檔，使用 MSBuild XML 結構描述來強制執行精密且更細緻控制如何建立與部署您的專案。 這些自訂專案檔案在 Visual Studio 會自動產生的專案檔案完全相同的方式。

> [!NOTE]
> 您也可以與 Team Build 服務 Team Foundation Server (TFS) 中使用 MSBuild 專案檔。 例如，您可以使用持續整合 (CI) 案例中的專案檔來自動化部署至測試環境，新的程式碼簽入時。 如需詳細資訊，請參閱[用於自動化 Web 部署設定 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。


### <a name="project-file-naming-conventions"></a>專案檔案命名慣例

當您建立您自己的專案檔時，您可以使用任何您喜歡的副檔名。 不過，若要簡化您的方案來了解其他人，您應該使用這些常用的慣例：

- 當您建立的專案檔建置專案時，請使用.proj 延伸模組。
- 當您建立可重複使用的專案檔案匯入到其他專案檔案，請使用.targets 延伸模組。 副檔名為.targets 檔案通常沒有建置任何項目本身，它們只會包含您可以匯入.proj 檔案的指示。

### <a name="integration-with-deployment-technologies"></a>與部署技術的整合

如果您已使用 web 應用程式專案，在 Visual Studio 2010 中，例如 ASP.NET web 應用程式和 ASP.NET MVC web 應用程式，就會知道這些專案包含封裝和部署 web 應用程式目標環境的內建支援。 **屬性**這些專案包含頁**封裝/發行 Web**和**封裝/發行 SQL**索引標籤可讓您設定如何元件的程式封裝和部署應用程式。 這會顯示**封裝/發行 Web**  索引標籤：

![](understanding-the-project-file/_static/image1.png)

這些功能的基礎技術，即做為 Web 發行管線 (WPP)。 WPP 基本上是將 MSBuild 和[Web Deploy](https://go.microsoft.com/?linkid=9805122)在一起以提供 web 應用程式的完整建置、 封裝和部署程序。

好消息是，您可以利用 WPP 提供當您建立 web 專案的自訂專案檔案的整合點。 您可以在專案檔，可讓您建置專案、 建立 web 部署套件，以及在遠端伺服器上安裝這些套件，透過單一專案檔和 MSBuild 的單一呼叫中包含部署指示。 您也可以呼叫任何其他可執行檔做為建置流程的一部分。 例如，您可以執行 VSDBCMD.exe 命令列工具來部署結構描述檔案的資料庫。 在本主題的過程中，您會看到如何使用這些功能，以符合您的企業部署案例的需求的利用。

> [!NOTE]
> 如需 web 應用程式部署程序的運作方式的詳細資訊，請參閱[ASP.NET Web 應用程式專案部署概觀](https://msdn.microsoft.com/library/dd394698.aspx)。


## <a name="the-anatomy-of-a-project-file"></a>專案檔的結構

您可以在建置程序，更詳細地查看之前，它是值得花幾分鐘的時間讓自己熟悉如何使用 MSBuild 專案檔的基本結構。 本節提供當您檢視、 編輯或建立的專案檔時，就會發生常見元素的概觀。 特別是，您將學習：

- 如何使用*屬性*管理建置程序的變數。
- 如何使用*項目*以識別要在建置程序的輸入，例如 程式碼檔案。
- 如何使用*目標*和*工作*來執行指示 MSBuild，使用*屬性*和*項目*中其他位置定義專案檔。

這會顯示在 MSBuild 專案檔中索引鍵的項目之間的關聯性：

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>專案項目

[專案](https://msdn.microsoft.com/library/bcxfsh87.aspx)元素是每個專案檔的根項目。 除了識別專案檔的 XML 結構描述**專案**元素可以包含屬性，指定在建置程序的進入點。 例如，在[連絡人管理員範例方案](the-contact-manager-solution.md)、 *Publish.proj*檔案會指定開始組建時，應該藉由呼叫名為目標**FullPublish**。


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>屬性和條件

專案檔通常需要提供大量項資訊才能成功建立及部署您的專案。 這項資訊可能包括伺服器名稱、 連接字串、 認證、 組建組態、 來源和目的地檔案路徑和任何其他您想要包含為支援自訂的資訊。 在專案檔中，屬性必須在定義內[PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx)項目。 MSBuild 屬性是由索引鍵-值配對所組成。 內**PropertyGroup**項目，項目名稱會定義屬性索引鍵和項目的內容定義的屬性值。 例如，您可以定義名為屬性**ServerName**和**ConnectionString**來儲存靜態伺服器名稱和連接字串。


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


若要擷取屬性值，您可以使用格式 **$(***PropertyName***) * * *。*例如，若要擷取的值**ServerName**屬性，您會輸入：


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> 您會看到的範例，以及何時使用本主題稍後的屬性值。


在專案檔中內嵌為靜態屬性的資訊不是理想的方法來管理建置程序。 在許多案例中，您會想要從其他來源取得資訊或讓使用者能夠提供命令提示字元中的資訊。 MSBuild 可讓您指定為命令列參數的任何屬性值。 例如，使用者無法提供值給**ServerName**他或她執行時 MSBuild.exe 從命令列。


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> 如需有關引數和參數，您可以使用 MSBuild.exe 的詳細資訊，請參閱[MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。


您可以使用相同的屬性語法，以取得環境變數和內建的專案屬性的值。 針對您所定義的許多常用的屬性，您可以使用這些專案檔中包括相關的參數名稱。 例如，若要擷取目前的專案平台 & #x 2014; 例如， **x86**或**AnyCpu**（& s) #x 2014; 可以包含**$(Platform)**中的屬性參考您的專案檔。 如需詳細資訊，請參閱[建置命令和屬性的巨集](https://msdn.microsoft.com/library/c02as0cs.aspx)，[一般 MSBuild 專案屬性](https://msdn.microsoft.com/library/bb629394.aspx)，和[保留的屬性](https://msdn.microsoft.com/library/ms164309.aspx)。

屬性通常用於搭配*條件*。 大部分的 MSBuild 項目支援**條件**屬性，可讓您指定的準則的 MSBuild 應評估項目。 例如，請考慮此屬性定義：


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


當 MSBuild 處理此屬性定義時，它會先檢查以查看是否**$(OutputRoot)**就可使用屬性值。 如果屬性值為空白 & #x 2014; 也就是說，使用者未提供值的這個屬性 & #x 2014年; 條件評估為**true**和屬性值設定為**...\Publish\Out**。如果使用者已經提供值，這個屬性，條件評估為**false**並不會使用靜態屬性值。

如需您可在指定條件的不同方式的詳細資訊，請參閱[MSBuild 條件](https://msdn.microsoft.com/library/7szfhaft.aspx)。

### <a name="items-and-item-groups"></a>項目和項目群組

其中一個重要的角色專案檔是定義建置流程的輸入。 一般來說，這些輸入是檔案 & #x 2014年; 程式碼檔案、 組態檔、 指令檔和任何其他您需要處理，或做為複製的檔案組件在建置程序。 在 MSBuild 專案結構描述，以表示這些輸入[項目](https://msdn.microsoft.com/library/ms164283.aspx)項目。 在專案檔中，項目必須在定義內[ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx)項目。 就像**屬性**項目，您可以命名**項目**元素不過嗎。 不過，您必須指定**Include**屬性識別的檔案或項目所表示的萬用字元。


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


藉由指定多個**項目**具有相同名稱的項目，您要有效地建立具名的資源的清單。 若要查看此動作中的好方法是來看看其中一個 Visual Studio 建立的專案檔案內。 例如， *ContactManager.Mvc.csproj*範例方案中的檔案包含大量項目群組，每個都有數個相同的已命名**項目**項目。


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


如此一來，專案檔會指示 MSBuild 建構來處理相同的方法 & #x 2014; 需要的檔案清單**參考**清單含有組件必須先準備就緒，成功的組建， **編譯**清單包含必須編譯的程式碼檔案和**內容**清單包含必須複製的資源不變。 我們會探討如何建置程序參考，但在本主題稍後會使用這些項目。

也可以包含項目[ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx)子項目。 這些是使用者定義的索引鍵-值組，基本上表示該項目特有的屬性。 例如，許多**編譯**專案檔中的項目包含**DependentUpon**子項目。


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> 除了使用者建立的項目中繼資料，所有的項目指派上建立各種常見的中繼資料。 如需詳細資訊，請參閱[已知的項目中繼資料](https://msdn.microsoft.com/library/ms164313.aspx)。


您可以建立**ItemGroup**根層級中的項目**專案**項目或在特定**目標**項目。 **ItemGroup**項目也支援**條件**屬性，可讓您自訂的建置程序，根據平台專案組態等條件來輸入。

### <a name="targets-and-tasks"></a>目標和工作

MSBuild 結構描述中[工作](https://msdn.microsoft.com/library/77f2hx1s.aspx)元素代表個別的組建指令 （或工作）。 MSBuild 會包含許多預先定義的工作。 例如: 

- **複製**工作將檔案複製到新位置。
- **Csc**工作叫用 Visual C# 編譯器。
- **Vbc**工作叫用 Visual Basic 編譯器。
- **Exec**工作會執行指定的程式。
- **訊息**工作會將訊息寫入記錄器。

> [!NOTE]
> 完整的現成可用的工作的詳細資訊，請參閱[MSBuild 工作參考](https://msdn.microsoft.com/library/7z253716.aspx)。 如需有關工作，包括如何建立您自己自訂的工作，請參閱[MSBuild 工作](https://msdn.microsoft.com/library/ms171466.aspx)。


工作必須一律包含在[目標](https://msdn.microsoft.com/library/t50z2hka.aspx)項目。 A**目標**項目是一組一個或多個工作，會循序執行，且專案檔包含多個目標。 您想要執行的工作或一組工作，當您叫用包含它們的目標。 例如，假設您有簡單的專案檔，記錄的訊息。


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


您可以使用叫用命令列中，從目標**/t**切換到指定的目標。


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


或者，您可以新增**DefaultTargets**屬性**專案**項目，來指定您想要叫用的目標。


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


在此情況下，您不需要指定命令列從目標。 您可以僅指定專案檔和 MSBuild 將會叫用**FullPublish**為您的目標。


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


目標和工作可以包含**條件**屬性。 因此，您可以選擇略過整個目標或個別的工作必須符合特定條件。

一般而言，當您建立有用的工作和目標，您將需要的屬性和項目已在其他位置中定義的專案檔，請參閱：

- 若要使用的屬性值，輸入**$(***PropertyName***)**，其中*PropertyName*名稱**屬性**項目或名稱參數。
- 若要使用的項目，請輸入**@(***ItemName***)**，其中*ItemName*名稱**項目**項目。

> [!NOTE]
> 請記住您建立多個項目具有相同名稱時，是否您要建置清單。 相反地，如果您建立多個屬性具有相同名稱時，您提供的最後一個屬性值將會覆寫任何先前的屬性相同的名稱 & #x 2014年; 屬性只能包含單一值。


例如，在*Publish.proj*檔案範例方案中，看看**BuildProjects**目標。


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


在此範例中，您可以觀察這些關鍵點：

- 如果**BuildingInTeamBuild**參數指定，且值為**true**，這個目標內的工作將會執行。
- 目標中包含的單一執行個體[MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx)工作。 這項工作可讓您建立其他的 MSBuild 專案。
- **ProjectsToBuild**項目傳遞給工作。 此項目可能代表的專案或方案檔，它是由所有定義清單**ProjectsToBuild**項目項目群組中的項目。 在此情況下， **ProjectsToBuild**項目會參考單一方案檔案。

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- 屬性值傳遞至**MSBuild**工作包含名為參數**OutputRoot**和**組態**。 如果它們不是，這些都會設為參數值，如果提供或靜態屬性值。

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

您也可以看到**MSBuild**工作叫用名為目標**建置**。 這是其中一個廣泛使用在 Visual Studio 專案檔，您可以在自訂專案檔案中，使用類似的數個內建目標**建置**，**清除**，**重建**，和**發行**。 您將進一步了解使用目標和工作，以控制建置流程中，以及有關**MSBuild**工作特別的是，本主題稍後的。

> [!NOTE]
> 如需目標的詳細資訊，請參閱[MSBuild 目標](https://msdn.microsoft.com/library/ms171462.aspx)。


## <a name="splitting-project-files-to-support-multiple-environments"></a>分割的專案檔，以支援多個環境

假設您想要能夠將方案部署到多個環境，例如測試伺服器、 暫存的平台和實際執行環境。 組態可能因本質上這些環境 & #x 2014年; 不只根據伺服器名稱、 連接字串等等，但也可能根據認證、 安全性設定，以及許多其他因素。 如果您需要定期執行這項操作，並不真的有利編輯專案檔中的多個屬性，每次切換的目標環境。 也不是理想的解決方案需要的屬性值，以提供給建置處理序無止盡的清單。

所幸還有另一個方法。 MSBuild 可讓您跨多個專案檔案分割您的組建組態。 若要查看其運作方式，在範例方案，請注意，有兩個自訂專案檔案：

- *Publish.proj*，其中包含內容，項目，與目標的通用於所有環境。
- *Env Dev.proj*，其中包含開發人員環境特有的屬性。

現在請注意， *Publish.proj*檔案包含[匯入](https://msdn.microsoft.com/library/92x05xfs.aspx)項目，下方的 開啟**專案**標記。


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**匯入**元素用來匯入到目前的 MSBuild 專案檔的另一個的 MSBuild 專案檔的內容。 在此情況下， **TargetEnvPropsFile**參數可讓您想要匯入專案檔的檔名。 當您執行 MSBuild 時，您可以為此參數提供值。


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


兩個檔案的內容有效地合併至單一專案檔案中。 使用這個方法，您可以建立一個專案檔包含通用的組建組態和多個增補專案檔包含特定環境的屬性。 如此一來，只需執行命令，以不同的參數值可讓您將方案部署至不同的環境。

![](understanding-the-project-file/_static/image3.png)

請遵循最佳作法是分割您的專案檔案，以這種方式。 它可讓開發人員執行單一命令，同時避免重複通用建置屬性跨多個專案檔以部署至多個環境。

> [!NOTE]
> 如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。


## <a name="conclusion"></a>結論

本主題介紹一般 MSBuild 專案檔，並說明如何建立自訂專案檔，以控制建置流程。 它也導入的概念將專案檔案分割成通用組建指示和特定環境的建置屬性，讓您輕鬆建置和部署專案至多個目的地。

下一個主題[瞭解建置程序](understanding-the-build-process.md)，提供更多深入了解如何使用專案檔，以控制建置和部署帶您逐步部署解決方案與實際的層級的複雜性。

## <a name="further-reading"></a>進一步閱讀

專案檔和 WPP 的更深入簡介，請參閱[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。

>[!div class="step-by-step"]
[上一頁](setting-up-the-contact-manager-solution.md)
[下一頁](understanding-the-build-process.md)
