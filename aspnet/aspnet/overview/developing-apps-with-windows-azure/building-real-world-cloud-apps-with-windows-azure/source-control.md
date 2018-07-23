---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: 原始檔控制 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: aspnetcontent
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 045dc654057802be4ad96f5ecc3ed6c3d7a1ccb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823309"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>原始檔控制 （使用 Azure 建置真實世界的雲端應用程式）
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。 它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。 電子書的相關資訊，請參閱[第 1 章](introduction.md)。


原始檔控制是不可或缺的所有雲端開發專案，不只是小組的環境。 您不會將編輯原始程式碼，或甚至不需要復原函式和自動備份和原始檔控制的 Word 文件可讓您在專案層級，他們可以在其中儲存更多的時間，當發生錯誤的這些函式。 雲端原始檔控制服務，您再也不必擔心複雜的設定，與您可以使用最多 5 位使用者免費的 Visual Studio Online 原始檔控制。

本章的第一個部分會說明三個索引鍵的最佳作法，要牢記在心：

- [自動化指令碼視為原始程式碼](#scripts)和版本它們與您的應用程式程式碼。
- [在 密碼永遠不檢查](#secrets)（敏感性資料，例如認證） 到原始程式碼存放庫。
- [設定來源分支](#devops)啟用 DevOps 工作流程。

本章的其餘部分提供 Visual Studio、 Azure 和 Visual Studio Online 中的這些模式的實作一些範例：

- [在 Visual Studio 中的原始檔控制中加入指令碼](#vsscripts)
- [在 Azure 中儲存敏感性資料](#appsettings)
- [在 Visual Studio 和 Visual Studio Online 中使用 Git](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>自動化指令碼視為原始程式碼

當您使用雲端專案上您要經常變更項目，且想要能夠快速地回應您的客戶所回報的問題。 快速回應牽涉到使用自動化指令碼中所述[自動執行的所有項目](automate-everything.md)一章。 所有您用來建立您的環境中，指令碼部署給它，小數位數等等，必須與您的應用程式來源程式碼保持同步。

若要讓指令碼與程式碼保持同步，請將它們儲存在您的原始檔控制系統中。 然後如果您需要回復變更，或快速檢修，不同於開發的程式碼的實際執行程式碼中，您不需要浪費時間嘗試追蹤的設定已變更，或哪些小組成員擁有您所需要的版本的複本。 您可以保證，您需要的指令碼會在與同步程式碼基底，您需要它們，也可以保證所有小組成員會都使用相同的指令碼。 無論您需要將自動化測試和部署至生產環境或新的功能開發了 hot fix，也會有正確的指令碼需要更新的程式碼。

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>未簽入祕密

它是適當安全的地方，如密碼等機密資料的太多人通常存取原始程式碼存放庫。 如果指令碼依賴密碼等機密資料時，會參數化為這些設定，讓它們不會儲存在原始程式碼中，並儲存您的祕密的任一處。

比方說，Azure 可讓您下載包含的檔案會發行設定，才能自動建立發行設定檔。 這些檔案包括使用者名稱和密碼已獲授權來管理您的 Azure 服務。 如果您使用這個方法用來建立發行設定檔，以及如果您檢查這些檔案加入原始檔控制中，存取您的儲存機制的任何人都可以看到這些使用者名稱和密碼。 您可以安全地將密碼儲存在發行設定檔本身，因為它會加密，且位於 *.pubxml.user* ，依預設不會納入原始檔控制的檔案。

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>結構以促進 DevOps 工作流程的來源分支

如何在您的存放庫中實作分支會影響您開發新功能和在生產環境中修正問題的能力。 以下是許多中型大小的小組使用的模式：

![來源分支結構](source-control/_static/image1.png)

主要的分支永遠符合在生產環境中的程式碼。 下方主要分支會對應至不同的階段開發生命週期中。 「 開發 」 分支是您用來實作新功能。 對小型的小組您可能只有 master 和開發，但我們通常建議人員具備開發和主機之間的預備分支。 針對最終整合測試更新移動到生產環境之前，您可以使用預備環境。

適用於大型的小組可能有不同的分支，每個新功能;較小小組您可能必須每個人都以 「 開發 」 分支簽入。

如果您有的分支，每項功能，當 Feature A 已經準備好您合併其原始程式碼變更增加至開發分支，然後向下到其他的功能分支。 此合併處理序的原始程式碼可能很耗時的而且若要避免這項工作，同時仍保留功能不同，某些小組實作稱為 「 替代*[功能切換](http://en.wikipedia.org/wiki/Feature_toggle)* （也稱為*功能旗標*)。 這表示所有的所有功能的程式碼是在相同的分支中，但您啟用或停用程式碼中使用參數的每個功能。 例如，假設 Feature A 是它修正應用程式工作，新的欄位和 Feature B 新增快取的功能。 這兩項功能的程式碼可以是 「 開發 」 分支，但應用程式將只顯示新的欄位時變數設為 true，而且它只會使用快取設定不同的變數時為 true。 如果功能的結果尚未準備好升級，但 Feature B 已就緒，您可以將所有關閉功能的切換到生產環境程式碼的升級，Feature B 開啟。 然後，您可以完成功能的並將它升級之後，所有與任何來源的程式碼合併。

不論是否使用分支或切換功能，分支結構，就像這樣可讓您靈活且可重複的方式中的流程從開發到生產環境程式碼。

此結構也可讓您快速地回應客戶的意見反應。 如果您需要快速修正生產環境，您也可以執行，有效率地敏捷的方式。 您可以建立從 master 或預備分支，以及何時就緒向上合併至 master 和向下到開發和功能分支。

![Hotfix 分支](source-control/_static/image2.png)

而其為區隔生產和開發分支的分支結構，這類不實際執行的問題可以將放入您不必升級新功能的程式碼，以及您的生產環境修正的位置。 新功能的程式碼可能無法完全經過測試，並可供生產環境，您可能必須進行許多變更尚未準備好支援的工作。 或者，您可能會延遲您的修正，以測試變更，並讓它們準備部署。

接下來，您會看到如何在 Visual Studio、 Azure 和 Visual Studio Online 中實作這三個模式的範例。 這些是範例，而不是詳細的逐步作法-要-執行-it 指示;如需詳細指示，提供所有必要的內容，請參閱[資源](#resources)本章的最後一節。

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>在 Visual Studio 中的原始檔控制中加入指令碼

您可以新增至原始檔控制在 Visual Studio 中的指令碼，將其納入 Visual Studio 方案資料夾 （假設您的專案在原始檔控制中）。 若要這麼做的其中一個方法如下。

在您的方案資料夾中建立指令碼的資料夾 (相同的資料夾具有您 *.sln*檔案)。

![自動化資料夾](source-control/_static/image3.png)

將指令碼檔案複製到資料夾。

![自動化資料夾內容](source-control/_static/image4.png)

在 Visual Studio 中，加入專案中的方案資料夾。

![新的方案資料夾功能表選取項目](source-control/_static/image5.png)

並將指令碼檔案新增至方案資料夾。

![新增現有項目 功能表選取項目](source-control/_static/image6.png)

![[加入現有項目] 對話方塊](source-control/_static/image7.png)

指令碼檔案現在會包含在專案和原始檔控制正在追蹤其版本變更，以及對應原始程式碼變更。

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>在 Azure 中儲存敏感性資料

如果您執行您的應用程式在 Azure 網站時，避免將認證儲存在原始檔控制中的一種方法是將它們儲存在 Azure 中。

比方說，它修正應用程式會儲存在其 Web.config 檔案兩個連接字串，必須在生產環境並可讓您的 Azure 儲存體帳戶存取金鑰的密碼。

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

如果您將實際的生產環境值，這些設定，在您*Web.config*檔案，或如果您將它們放在*Web.Release.config*檔案來設定在部署期間，將 Web.config 轉換它們會儲存於原始程式碼儲存機制。 如果您輸入的資料庫連接字串至實際執行環境發行設定檔，密碼會在您 *.pubxml*檔案。 (您無法排除 *.pubxml*檔案從原始檔控制，但接著您會失去共用的其他部署設定。)

Azure 可讓您的替代方式**appSettings**和連接字串的區段*Web.config*檔案。 以下是相關的部分**組態**網站在 Azure 管理入口網站中的索引標籤：

![appSettings 和入口網站中的 connectionStrings](source-control/_static/image8.png)

當您將專案部署到此網站和應用程式執行時，您已在 Azure 中儲存任何值會覆寫任何值位於 Web.config 檔案。

您可以使用管理入口網站 」 或 「 指令碼，在 Azure 中設定這些值。 環境建立自動化指令碼所示[自動執行的所有項目](automate-everything.md)一章會建立 Azure SQL Database、 取得的儲存體和 SQL Database 的連接字串，並將這些機密資訊儲存在您的網站的設定。

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

請注意，指令碼會進行參數化，讓實際的值不保存至來源存放庫。

當您在本機執行您的開發環境中時，應用程式讀取您的本機 Web.config 檔案和您的連線字串中的 LocalDB 的 SQL Server 資料庫指向*應用程式\_資料*web 專案的資料夾。 當您在 Azure 中執行應用程式和應用程式會嘗試從 Web.config 檔案中讀取這些值時，取得上一步並使用是網站上，沒有什麼實際上是在 Web.config 檔案中所儲存的值。

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>在 Visual Studio 和 Visual Studio Online 中使用 Git

您可以使用任何原始檔控制環境來實作 DevOps 分支結構，使用稍早所呈現。 分散式團隊[分散式版本控制系統](http://en.wikipedia.org/wiki/Distributed_revision_control)(DVCS) 可能適合; 對於其他團隊[集中式系統](http://en.wikipedia.org/wiki/Revision_control)可能比較適合。

[Git](http://git-scm.com/)是 DVCS 有變得非常受歡迎。 當您使用 Git 原始檔控制時，您將在本機電腦上有及其所有記錄的存放庫的完整複本。 許多人喜歡，因為可以更輕鬆繼續工作時您未連接到網路，您可以繼續執行認可和回復，建立並切換分支，並繼續下一步。 即使您連線到網路，很容易且更快速地建立分支及切換分支時的所有項目是在本機。 您也可以執行本機認可及回復，而不會影響其他開發人員。 並可批次認可之後，再將它們傳送到伺服器。

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO)，先前稱為 Team Foundation Service 提供 Git 和[Team Foundation 版本控制](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx)(TFVC; 集中式原始檔控制)。 在 Microsoft Azure 的群組中有些小組使用集中式的原始檔控制、 分散式的有些使用和某些使用混合 （集中式針對某些專案及發佈適用於其他專案）。 VSO 服務為最多 5 位使用者免費。 您可以註冊免費的計劃[此處](https://go.microsoft.com/fwlink/?LinkId=307137)。

Visual Studio 2013 包含內建的第一級[Git 支援](https://msdn.microsoft.com/library/hh850437.aspx); 以下是快速的運作方式的示範。

使用 Visual Studio 2013 中開啟專案，以滑鼠右鍵按一下中的解決方案**方案總管] 中**，然後選擇 [**將方案加入至原始檔控制**。

![將方案加入原始檔控制](source-control/_static/image9.png)

Visual Studio 會詢問您想要使用 TFVC （集中式的版本控制） 或 Git。

![選擇原始檔控制](source-control/_static/image10.png)

當您選取 Git，並按一下**確定**，Visual Studio 在您的方案資料夾中建立新的本機 Git 存放庫。 新的存放庫中未指定任何檔案您必須將其加入至儲存機制執行 Git 認可。 在方案上按一下滑鼠右鍵**方案總管**，然後按一下**認可**。

![認可](source-control/_static/image11.png)

Visual Studio 會自動設置的所有專案檔案進行認可，而且列在**Team Explorer**中**包含的變更**窗格。 (如果有一些您不想要包含在認可中，您可以選取它們，以滑鼠右鍵按一下，然後按一下 **排除**。)

![Team Explorer](source-control/_static/image12.png)

輸入認可註解，然後按一下**認可**，和 Visual Studio 執行認可，並且顯示認可識別碼。

![Team Explorer 的變更](source-control/_static/image13.png)

現在如果您變更一些程式碼，使其不同的儲存機制中，您可以輕鬆地檢視差異。 以滑鼠右鍵按一下檔案，您已經變更，選取**Unmodified 與比較**，並取得未認可的變更會顯示比較顯示。

![與未修改的比較](source-control/_static/image14.png)

![差異顯示變更](source-control/_static/image15.png)

您可以輕鬆地看到您正在進行，並將它們簽入哪些變更。

假設您需要進行分支 – 您可以在 Visual Studio 中執行。 在  **Team Explorer**，按一下**新的分支**。

![Team Explorer 中的新分支](source-control/_static/image16.png)

輸入分支名稱，按一下**Create Branch**，以及您選取**簽出分支**，Visual Studio 會自動簽出新的分支。

![Team Explorer 中的新分支](source-control/_static/image17.png)

您現在可以對檔案進行變更，並簽入至這個分支。 此外，您可以輕鬆地切換分支和 Visual Studio 會自動同步處理到者為準的分支的檔案已簽出。在此範例中顯示標題中的網頁 *\_Layout.cshtml*已變更為 「 Hot Fix 1 」 在 HotFix1 分支。

![Hotfix1 分支](source-control/_static/image18.png)

如果您切換回主要分支的內容 *\_Layout.cshtml*檔案自動還原它們的主要分支中。

![主要分支](source-control/_static/image19.png)

此方式快速地建立分支和分支之間來回翻轉的簡單範例。 這項功能可讓使用分支結構的高度敏捷式軟體開發工作流程和自動化指令碼所示[自動執行的所有項目](automate-everything.md)一章。 例如，您可以在 「 開發 」 分支中工作，建立 hotfix 分支從主、 切換至新的分支、 讓您的變更和認可，並再切換回 「 開發 」 分支並繼續您先前進行的作業。

您已看這裡是您在 Visual Studio 中以本機 Git 儲存機制的運作方式。 在小組環境中您通常也發送變更到通用儲存機制。 Visual Studio 工具也可讓您以指向遠端 Git 存放庫。 您可以使用 GitHub.com 基於這個目的，或者您可以使用[在 Visual Studio Online 的 Git](https://msdn.microsoft.com/library/hh850437.aspx)與所有其他 Visual Studio Online 功能，例如工作項目和 bug 追蹤整合在一起。

這不是唯一的方法，您可以實作敏捷式軟體開發分支策略，當然。 您可以啟用相同的敏捷式軟體開發工作流程，以使用集中式原始檔控制儲存機制。

## <a name="summary"></a>總結

測量您如何快速進行變更，並取得即時安全且可預測的方式為基礎的原始檔控制系統成功。 如果您發現自己害怕得進行變更，因為您只需要一天，或兩個在其上的手動測試，您可能會自問您不必執行 process-wise 或 test-wise，以便您可以進行該變更，以分鐘為單位，或在無法再最差超過一小時。 這麼做的其中一個策略是實作持續整合與持續傳遞，在中，我們將討論[下一步 一章](continuous-integration-and-continuous-delivery.md)。

<a id="resources"></a>
## <a name="resources"></a>資源

[Visual Studio Online](https://www.visualstudio.com/)入口網站提供文件和支援服務，以及您可以註冊帳戶。 如果您有 Visual Studio 2012，並想要使用 Git，請參閱[Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)。

如需 TFVC （集中式的版本控制） 和 Git （分散式的版本控制） 的詳細資訊，請參閱下列資源：

- [應該使用哪個版本控制系統： TFVC 或 Git？](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN 文件，包含彙總 TFVC 和 Git 之間差異的資料表。
- [嗯，我喜歡 Team Foundation Server 和我喜歡 Git，但是何者較佳？](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Git 和 TFVC 的比較。

如需分支策略的詳細資訊，請參閱下列資源：

- [建置與 Team Foundation Server 2012 的發行管線](https://msdn.microsoft.com/library/dn449957.aspx)。 Microsoft Patterns and Practices 文件。 請參閱第 6 章，如需分支策略的討論。 提倡功能與切換功能分支，透過使用功能分支時，如果是代表很短期 （小時或天至多）。
- [版本控制指南](https://aka.ms/vsarsolutions)。 ALM Ranger 指引分支策略。 在 [下載] 索引標籤上，請參閱分支 Strategies.pdf。
- [使用功能切換進行軟體開發](https://msdn.microsoft.com/magazine/dn683796.aspx)。 MSDN Magazine 文章。
- [功能切換](http://martinfowler.com/bliki/FeatureToggle.html)。 簡介功能切換/Martin Fowler 部落格上的功能旗標。
- [功能切換 vs 功能分支](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)。 關於功能切換，Dylan smith 的另一個部落格文章。

如需如何處理不會保留在原始檔控制存放庫中的機密資訊的詳細資訊，請參閱下列資源：

- [將密碼和其他機密資料部署到 ASP.NET 和 Azure App Service 最佳作法](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
- [Azure 網站： 如何應用程式字串與連接字串的運作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)。 說明 Azure 功能，以覆寫`appSettings`並`connectionStrings`中的資料*Web.config*檔案。
- [自訂組態和應用程式設定 Azure 網站中-Stefan schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/)。

保留出原始檔控制的機密資訊的其他方法的相關資訊，請參閱[ASP.NET MVC： 保留原始檔控制的私用設定](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。

> [!div class="step-by-step"]
> [上一頁](automate-everything.md)
> [下一頁](continuous-integration-and-continuous-delivery.md)
