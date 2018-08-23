---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: 疑難排解封裝程序 |Microsoft Docs
author: jrjlee
description: 本主題描述如何使用 M 中的 EnablePackageProcessLoggingAndAssert 屬性可以收集在封裝程序的詳細的資訊...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22be1ccc5a1ec52d143bedffd79264918c1a45e1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833451"
---
<a name="troubleshooting-the-packaging-process"></a>疑難排解封裝程序
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何使用可以收集在封裝程序的詳細的資訊**EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 中的屬性。
> 
> 當您設定**EnablePackageProcessLoggingAndAssert**屬性設 **，則為 true**，MSBuild 將會：
> 
> - 加入組建記錄檔中的封裝程序的其他資訊。
> - 記錄錯誤某些狀況下，比方說，如果在封裝清單中找到重複的檔案。
> - 建立的記錄目錄中*ProjectName*\_封裝資料夾，並使用它來記錄您正在封裝之檔案的相關的資訊。
> 
> 如果無法在封裝程序，或您的 web 部署套件不包含您預期的檔案，您可以使用此資訊來疑難排解程序和 pinpoint 即將錯誤項目。
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert**如果您要建置您的專案使用，僅適用於屬性**偵錯**組態。 在 其他設定，會忽略此屬性。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>了解 EnablePackageProcessLoggingAndAssert 屬性

[建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)描述 Web 發行管線 (WPP) 如何提供一組的 MSBuild 目標擴充功能的 MSBuild，讓它能夠與 Internet Information Services (IIS) Web 整合部署工具 (Web Deploy)。 當您封裝的 web 應用程式專案時，叫用 WPP 目標。

許多這些 WPP 目標包含記錄的其他資訊的條件式邏輯時**EnablePackageProcessLoggingAndAssert**屬性設定為 **，則為 true**。 例如，如果您檢閱**封裝**目標，您可以看到它會建立額外的記錄檔目錄，並將一份檔案寫入至文字檔案，如果**EnablePackageProcessLoggingAndAssert**等於 **，則為 true**。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> 中所定義的 WPP 目標*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。 您可以開啟這個檔案，並檢閱 Visual Studio 2010 或任何 XML 編輯器中的目標。 請小心不要修改檔案的內容。


## <a name="enabling-the-additional-logging"></a>啟用額外的記錄功能

您可以提供的值**EnablePackageProcessLoggingAndAssert**以各種方式，取決於您如何建置您的專案屬性。

如果您要建置您的專案，從命令列，您可以提供的值**EnablePackageProcessLoggingAndAssert**做為命令列引數的屬性：


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


如果您使用自訂的專案檔來建置專案，您可以包含**EnablePackageProcessLoggingAndAssert**中的值**屬性**屬性**MSBuild**工作：


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


如果您使用 Team Foundation Server (TFS) 組建定義來建置您的專案，您可以提供的值**EnablePackageProcessLoggingAndAssert**中的屬性**MSBuild 引數**資料列：![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> 如需有關建立和設定組建定義的詳細資訊，請參閱 <<c0> [ 建立組建定義，支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。


或者，如果您想要在每次建置中包含的套件，您可以修改專案檔來設定 web 應用程式專案**EnablePackageProcessLoggingAndAssert**屬性設 **，則為 true**。 您應該將屬性加入至第一個**PropertyGroup**您的.csproj 或.vbproj 檔案內的項目。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>檢閱記錄檔

當您建置並封裝使用的 web 應用程式專案**EnablePackageProcessLoggingAndAssert**設為 **，則為 true**，MSBuild 會建立名為登入的其他資料夾*ProjectName*\_套件資料夾。 記錄檔資料夾包含各種不同的檔案：

![](troubleshooting-the-packaging-process/_static/image2.png)

您會看到的檔案清單會根據您的專案和建置流程中的項目而有所不同。 不過，這些檔案通常用來記錄的封裝，在此程序的各個階段 WPP 所收集檔案清單：

- *PreExcludePipelineCollectFilesPhaseFileList.txt*檔會列出封裝移除指定排除的任何檔案之前，MSBuild 會收集的檔案。
- *AfterExcludeFilesFilesList.txt*之後指定排除的任何檔案會遭到移除，檔案會包含已修改的檔案清單。

    > [!NOTE]
    > 如需有關如何在封裝程序中排除檔案和資料夾的詳細資訊，請參閱 <<c0> [ 排除的檔案和資料夾從部署](excluding-files-and-folders-from-deployment.md)。
- *AfterTransformWebConfig.txt*檔案會列出封裝之後，任何針對收集的檔案*Web.config*已執行轉換。 在此清單中，任何組態特有*Web.config*轉換檔案，例如*Web.Debug.config*並*Web.Release.config*，會排除的檔案清單封裝。 轉換單一*Web.config*包含在它們的位置。
- *PostAutoParameterizationWebConfigConnectionStrings.txt*檔案包含的檔案清單中的連接字串之後*Web.config*已參數化的檔案。 這是程序，可讓您部署封裝時將連接字串取代為您的目標環境的正確設定。
- *Prepackage.txt*檔案包含最終的建置前清單要包含在套件的檔案。

> [!NOTE]
> 其他記錄檔的名稱通常會對應至 WPP 目標。 您可以檢閱這些目標，藉由檢查*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。


如果您的 web 封裝的內容不是您所預期，檢閱這些檔案可以是實用的方式，來識別在何種點中的程序項目時發生錯誤。

## <a name="conclusion"></a>結論

本主題描述如何使用**EnablePackageProcessLoggingAndAssert** MSBuild 疑難排解封裝程序中的屬性。 它說明不同的方式，您可以在其中提供要建置處理序的屬性值，並描述當您將屬性設定為記錄的其他資訊 **，則為 true**。

## <a name="further-reading"></a>進一步閱讀

如需有關如何使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 如需有關 WPP 及如何管理封裝的程序的詳細資訊，請參閱[建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。 如需如何從 web 部署套件中排除特定檔案和資料夾的指引，請參閱[排除的檔案和資料夾從部署](excluding-files-and-folders-from-deployment.md)。

> [!div class="step-by-step"]
> [上一步](running-windows-powershell-scripts-from-msbuild-project-files.md)
