---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: 從 MSBuild 專案檔中執行 Windows PowerShell 指令碼 |Microsoft Docs
author: jrjlee
description: 本主題描述如何建置和部署程序的一部分執行的 Windows PowerShell 指令碼。 您可以在本機執行指令碼 (換句話說，在 b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 018a962c3bac774a770b83b2fd1f44f72b6f5b09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824691"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>從 MSBuild 專案檔中執行 Windows PowerShell 指令碼
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何建置和部署程序的一部分執行的 Windows PowerShell 指令碼。 您可以在本機的指令碼 （亦即，組建伺服器上執行），或遠端電腦上，例如在目的地 web 伺服器或資料庫伺服器上。
> 
> 有許多原因為何您可能想要執行部署後的 Windows PowerShell 指令碼。 例如，您可能要：
> 
> - 新增自訂的事件來源登錄。
> - 產生上傳的檔案系統目錄。
> - 清除組建目錄。
> - 寫入自訂記錄檔中的項目。
> - 傳送電子郵件邀請新佈建的 web 應用程式的使用者。
> - 建立使用者帳戶具有適當的權限。
> - 設定 SQL Server 執行個體之間的複寫。
> 
> 本主題將說明如何從 Microsoft Build Engine (MSBuild) 專案檔中的自訂目標本機和遠端執行 Windows PowerShell 指令碼。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="task-overview"></a>工作概觀

若要執行的 Windows PowerShell 指令碼自動化或單一步驟的部署程序的一部分，您必須完成下列高階工作：

- 您的方案和原始檔控制，請新增 Windows PowerShell 指令碼。
- 建立會叫用您的 Windows PowerShell 指令碼的命令。
- 逸出任何保留的 XML 字元，在您的命令。
- 建立自訂 MSBuild 專案檔中的目標，並使用**Exec**工作來執行您的命令。

本主題將示範如何執行這些程序。 工作與本主題中的逐步解說假設您已經熟悉 MSBuild 目標和屬性，並了解如何使用自訂的 MSBuild 專案檔，以建置和部署程序。 如需詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

## <a name="creating-and-adding-windows-powershell-scripts"></a>建立並加入 Windows PowerShell 指令碼

本主題中的工作會使用名為的 Windows PowerShell 指令碼範例**LogDeploy.ps1** ，說明如何從 MSBuild 執行指令碼。 **LogDeploy.ps1**指令碼包含單一列項目寫入記錄檔的簡單函式：


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


**LogDeploy.ps1**指令碼接受兩個參數。 第一個參數表示您要新增項目時，記錄檔的完整路徑和第二個參數表示您想要記錄的記錄檔中的部署目的地。 當您執行指令碼時，它便會加入一條線，記錄檔，格式如下：


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


若要讓**LogDeploy.ps1**指令碼可使用 MSBuild，您需要：

- 加入原始檔控制中的指令碼。
- 新增至您的方案，Visual Studio 2010 中的指令碼。

您不需要部署具有您方案的內容，不論您是否計劃組建伺服器上或遠端電腦上執行指令碼的指令碼。 其中一個選項是將指令碼新增至方案資料夾。 在連絡人管理員範例中，因為您想要使用 Windows PowerShell 指令碼部署程序的一部分，所以合理將指令碼新增至發佈方案資料夾。

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

方案資料夾的內容會複製到組建伺服器作為來源資料。 不過，它們會形成任何專案輸出的任何部分。

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>在組建伺服器上執行 Windows PowerShell 指令碼

在某些情況下，您可能想要建置您的專案的電腦上執行 Windows PowerShell 指令碼。 比方說，您可能會使用 Windows PowerShell 指令碼來清除組建的資料夾或項目寫入自訂記錄檔。

語法，以從 MSBuild 專案檔中執行的 Windows PowerShell 指令碼是從一般的命令提示字元中執行的 Windows PowerShell 指令碼相同。 您需要可執行 powershell.exe，並使用 **– 命令**交換器以提供您想要執行的 Windows PowerShell 命令。 (在 Windows PowerShell v2 中，您也可以使用 **– 檔案**切換)。 此命令應該採取這種格式：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


例如: 


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


如果您的指令碼的路徑包含空格，您需要加上連字號的單一引號括住的檔案路徑。 您無法使用雙引號括住，，因為您已經使用它們來括住的命令：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


當您叫用此命令從 MSBuild 時，有一些其他考量。 首先，您應該在其中包含 **– NonInteractive**旗標，以確保以無訊息模式執行指令碼。 接下來，您應該在其中包含 **-ExecutionPolicy**與適當的引數值的旗標。 這會指定 Windows PowerShell 會套用到您的指令碼，並可讓您覆寫預設的執行原則，可能會導致您的指令碼執行的執行原則。 您可以選擇從這些引數的值：

- 值為**Unrestricted**可讓 Windows PowerShell 中執行您的指令碼，不論是否簽署指令碼。
- 值為**RemoteSigned**會允許執行未簽署的指令碼在本機電腦上所建立的 Windows PowerShell。 不過，在其他地方所建立的指令碼必須經過簽署。 （在實務上，您很難已在組建伺服器上本機建立的 Windows PowerShell 指令碼）。
- 值為**AllSigned**可讓 Windows PowerShell 中執行簽署指令碼。

是預設執行原則**Restricted**，這可防止 Windows PowerShell 執行任何指令碼檔案。

最後，您必須逸出任何保留的 XML 字元，就會發生在 Windows PowerShell 命令：

- 取代單一引號**&amp;a p o s;**
- 取代以雙引號括住**&amp;q u o t;**
- 取代連字號與**&amp;amp;**

- 當您進行這些變更時，您的命令大致如下：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


在您自訂的 MSBuild 專案檔，您可以建立新的目標，並使用**Exec**工作來執行此命令：


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


在此範例中，請注意：

- 任何變數，例如參數值和 Windows PowerShell 可執行檔的位置會宣告為 MSBuild 屬性。
- 若要讓使用者覆寫這些值，從命令列包含條件。
- **MSDeployComputerName**專案檔中其他位置宣告屬性。

當您執行此目標，您的建置程序的一部分時，Windows PowerShell 會執行您的命令，並寫入您指定的檔案中的記錄項目。

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>在遠端電腦上執行 Windows PowerShell 指令碼

Windows PowerShell 是透過遠端電腦上執行指令碼[Windows 遠端管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。 若要這樣做，您必須使用[Invoke-command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet。 這可讓您執行您的指令碼，對一或多個遠端電腦，而不會將指令碼複製到遠端電腦。 您執行指令碼的本機電腦，會傳回任何結果。

> [!NOTE]
> 在您使用之前**Invoke-command** cmdlet 來執行 Windows PowerShell 指令碼在遠端電腦上，您需要設定 WinRM 接聽程式，以接受遠端的訊息。 您可以執行命令**winrm quickconfig**遠端電腦上。 如需詳細資訊，請參閱 <<c0> [ 安裝的 Windows 遠端管理與設定](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)。


從 Windows PowerShell 視窗中，您會使用此語法來執行**LogDeploy.ps1**遠端電腦上的指令碼：


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> 使用的其他可以使用各種方式**Invoke-command**執行指令碼檔案，但這種方法是最直接當您需要提供參數值，並管理有空格的路徑。


當您執行命令提示字元中時，您需要 Windows PowerShell 可執行檔，並使用 **– 命令**參數，以提供您指示：


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


為之前，您需要提供一些額外的參數和逸出任何保留的 XML 字元，當您執行命令時從 MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


最後，同樣地，您可以使用**Exec**自訂 MSBuild 目標來執行您的命令中的工作：


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


當您執行此目標，您的建置程序的一部分時，Windows PowerShell 就會執行您的指令碼中所指定的電腦上 **– computername**引數。

## <a name="conclusion"></a>結論

本主題說明如何從 MSBuild 專案檔中執行的 Windows PowerShell 指令碼。 您可以使用這種方法，在本機或遠端電腦上執行 Windows PowerShell 指令碼，自動化或單一步驟建置和部署程序的一部分。

## <a name="further-reading"></a>進一步閱讀

如需簽署 Windows PowerShell 指令碼和管理執行原則的指引，請參閱 <<c0> [ 執行的 Windows PowerShell 指令碼](https://technet.microsoft.com/library/ee176949.aspx)。 如需從遠端電腦執行 Windows PowerShell 命令的指引，請參閱 <<c0> [ 執行遠端命令](https://technet.microsoft.com/library/dd819505.aspx)。

如需有關如何使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

> [!div class="step-by-step"]
> [上一頁](taking-web-applications-offline-with-web-deploy.md)
> [下一頁](troubleshooting-the-packaging-process.md)
