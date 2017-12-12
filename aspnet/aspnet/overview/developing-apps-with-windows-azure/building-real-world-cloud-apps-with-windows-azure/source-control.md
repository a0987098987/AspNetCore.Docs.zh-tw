---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: "原始檔控制 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: f244e6bd1cd8abd23b64d07ccafcef5c4db1029b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>原始檔控制 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


原始檔控制是不可或缺的所有雲端開發專案，不只是小組的環境。 您不認為編輯原始碼，或甚至不需要復原函式和自動備份和原始檔控制的 Word 文件可讓您在專案層級，它們可以在其中儲存當發生錯誤的更多時間的這些函式。 雲端原始檔控制服務，您再也不必擔心複雜的安裝，且您可以使用最多 5 位使用者的免費 Visual Studio Online 的原始檔控制。

本章節的第一個部分說明三個索引鍵的最佳作法，請記住以下幾點：

- [自動化指令碼視為原始程式碼](#scripts)和版本應用程式程式碼連同它們。
- [在密碼永遠不檢查](#secrets)（敏感性資料，例如認證） 到原始程式碼儲存機制。
- [設定來源分支](#devops)啟用 DevOps 工作流程。

本章的其餘部分提供這些模式，在 Visual Studio、 Azure 和 Visual Studio Online 中的某些範例實作：

- [將指令碼新增至 Visual Studio 中的原始檔控制](#vsscripts)
- [在 Azure 中儲存機密資料](#appsettings)
- [在 Visual Studio 和 Visual Studio Online 中使用 Git](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>自動化指令碼視為原始程式碼

當您使用雲端專案正在經常變更項目，而且您想要能夠快速回應您的客戶所報告的問題。 快速回應牽涉到使用自動化的指令碼中所述[一切自動化](automate-everything.md)章節。 所有您用來建立您的環境，指令碼部署到它，它等等，必須要與您的應用程式的原始程式碼同步的小數位數。

為了讓指令碼和程式碼的未同步，請將它們儲存在您的原始檔控制系統中。 然後如果您需要回復變更，或是快速修正對實際執行程式碼開發的程式碼的不同，您不需要浪費時間再追蹤變更的設定或小組成員有您需要的版本的複本。 您要確保您所需要的指令碼，您需要的時候，並確定所有小組成員都正在使用相同的指令碼的程式碼基底同步的。 然後您是否需要測試及部署至生產或開發新功能的問題修正的程序自動化，您就有正確的指令碼需要更新的程式碼。

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>在密碼中則不會檢查

通常，太多的人員是適當安全的地方之機密資料，例如密碼存取原始程式碼儲存機制。 如果指令碼的機密資料，例如密碼、 參數化這些設定，使它們不儲存在原始程式碼中，並儲存您的機密其他地方。

例如，Azure 可讓您下載檔案，其中包含發行設定才能自動建立的發行設定檔。 這些檔案包含使用者名稱和密碼已獲授權可以管理您的 Azure 服務。 如果您使用這個方法來建立發行設定檔，以及您簽入原始檔控制這些檔案，如果您的儲存機制存取任何人都可以看到這些使用者名稱和密碼。 您可以安全地將密碼儲存在本身的發行設定檔，因為它已經加密，且*。 pubxml.user* ，依預設不會包含在原始檔控制檔案。

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>結構來源分支，以便 DevOps 工作流程

如何在您的儲存機制中實作分支會影響您開發新功能和修正問題，在生產環境中的能力。 以下是媒體的大量大小小組使用的模式：

![來源分支結構](source-control/_static/image1.png)

「 主要 」 分支永遠符合在生產環境中的程式碼。 下方主要分支會對應至不同的階段開發生命週期中。 「 開發 」 分支是您用來實作新功能。 對於小型小組您只必須 master 和開發，但我們通常建議的人有暫存開發與主要分支。 您可以使用暫存的最終的整合測試，才能更新會移至生產環境。

對於大型的小組可能有不同的分支，每一個新的功能。針對較小的小組可能每個人簽入 「 開發 」 分支。

功能 A 已經準備好您合併時，如果分公司，以針對每個功能，其原始程式碼變更註冊在開發分支和其他功能分支向下。 這個合併程序的原始程式碼可能相當耗時，並以避免該工作，同時仍保留個別功能，有些小組實作替代呼叫*[功能切換](http://en.wikipedia.org/wiki/Feature_toggle)* （也稱為*功能旗標*)。 這表示所有程式的所有功能的程式碼都在相同的分支，但您啟用或停用程式碼中使用參數的每個功能。 例如，假設功能 A 不修正它的應用程式的工作，新的欄位，並功能 B 新增快取功能。 這兩項功能的程式碼可以在 「 開發 」 分支，但應用程式將只顯示新的欄位時的變數會設為 true，而且它只會使用快取設定不同的變數時為 true。 如果功能 A 尚未準備好升級，但功能 B 準備，您可以升級所有的程式碼到生產環境與關閉功能的交換器，功能 B 上切換。 您可以完成功能 A，並將其升階更新版本中，所有與任何來源的程式碼合併。

不論您使用分支或切換功能，分支結構，這樣可讓您敏捷式軟體開發和可重複的方式傳送您的程式碼，從開發到實際執行環境。

此結構也可讓您快速回應客戶的意見反應。 如果您需要快速修正對生產環境，您也可以執行，有效率地敏捷式軟體開發的方式。 您可以建立分支 master 或預備環境，以及何時備妥可向上合併到主要和向下開發與功能的分支。

![Hotfix 分支](source-control/_static/image2.png)

不會使用其隔離生產環境和開發分支的分支結構，像這樣實際執行的問題可能讓您不必升級新功能的程式碼，以及您的生產修正的位置。 新功能的程式碼可能不是全部經過測試且可供生產環境，您可能必須執行許多工作退出 未就緒，無法變更。 或者，您可能您的修正，以便測試變更，並讓它們開始準備好要部署的延遲時間。

接下來，您會看到有關如何在 Visual Studio、 Azure 和 Visual Studio Online 中實作下列三種模式的範例。 以下是範例而非詳細 how-要執行的 it 逐步解說。提供所有必要內容的詳細指示，請參閱[資源](#resources)章節的結尾 」 一節。

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>將指令碼新增至 Visual Studio 中的原始檔控制

您可以包含在 Visual Studio 方案資料夾 （假設您的專案在原始檔控制中），Visual Studio 中的原始檔控制加入指令碼。 以下是一個的方法。

在您的方案資料夾中建立指令碼的資料夾 (具有的相同資料夾您*.sln*檔案)。

![自動化資料夾](source-control/_static/image3.png)

將指令碼檔案複製到資料夾。

![自動化資料夾內容](source-control/_static/image4.png)

在 Visual Studio 中，加入專案中的方案資料夾。

![新的方案資料夾功能表選取項目](source-control/_static/image5.png)

並將指令碼檔案加入至方案資料夾。

![加入現有項目功能表選取項目](source-control/_static/image6.png)

![[加入現有項目] 對話方塊](source-control/_static/image7.png)

指令碼檔案目前包含在您的專案和原始檔控制會追蹤其版本變更，以及對應的來源的程式碼變更。

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>在 Azure 中儲存機密資料

如果您在 Azure 網站中執行您的應用程式，避免將認證儲存在原始檔控制中的一種方式是將它們儲存在 Azure 中。

例如，它修正應用程式會儲存其 Web.config 檔案兩個連接字串中，會將實際執行和可讓您的 Azure 儲存體帳戶存取的索引鍵中的密碼。

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

如果您將在這些設定的實際值您*Web.config*檔案，或如果您將它們放在*Web.Release.config*檔案來設定要在部署期間，將其插入的 Web.config 轉換它們會儲存在來源儲存機制中。 如果您輸入的資料庫連接字串至實際執行環境發行設定檔，密碼將會在您*.pubxml*檔案。 (您無法排除*.pubxml*檔案從原始檔控制，但接著您會遺失共用的其他部署設定的優點。)

Azure 可讓您的替代選項**appSettings**和連接字串的區段*Web.config*檔案。 以下是相關的部分**組態**網站在 Azure 管理入口網站中的索引標籤：

![appSettings 和 connectionStrings 在入口網站](source-control/_static/image8.png)

當您將專案部署到此網站和應用程式執行時，覆寫任何您已儲存在 Azure 中的值位於 Web.config 檔案中任何值。

您可以使用管理入口網站或指令碼，在 Azure 中設定這些值。 環境建立自動化指令碼中看到[一切自動化](automate-everything.md)章建立 Azure SQL Database、 取得儲存體和 SQL Database 的連接字串，並將這些密碼儲存在您的網站的設定。

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

請注意指令碼會使原始程式碼儲存機制不保存實際值參數化。

當您在本機執行您的開發環境中時，應用程式讀取本機 Web.config 檔案和您的連接字串指向 LocalDB 的 SQL Server 資料庫中*應用程式\_資料*web 專案的資料夾。 當您在 Azure 中執行應用程式，而且應用程式會嘗試從 Web.config 檔案中讀取這些值時，取得上一步並使用是網站沒有什麼是實際 Web.config 檔案中儲存的值。

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>在 Visual Studio 和 Visual Studio Online 中使用 Git

您可以使用任何原始檔控制環境來實作先前呈現的 DevOps 分支結構。 分散式團隊的[分散式版本控制系統](http://en.wikipedia.org/wiki/Distributed_revision_control)(DVCS) 可能最適合的其他小組;[集中式系統](http://en.wikipedia.org/wiki/Revision_control)可能效果更好。

[Git](http://git-scm.com/)是 DVCS 變得非常受歡迎。 當您使用 Git 原始檔控制時，您在本機電腦上有其記錄的所有儲存機制的完整複本。 許多人喜歡，因為這樣比較簡單繼續工作時您未連接到網路-您可以繼續執行認可及回復，建立和切換分支，以及進行其他操作。 即使您連線到網路，很輕鬆且快速建立分支，並切換分支，當所有項目是本機。 您也可以執行而不會影響其他開發人員在本機認可及回復。 您可以批次認可，然後才傳送到伺服器。

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO)，先前稱為 Team Foundation Service，提供兩個 Git 和[Team Foundation 版本控制](https://msdn.microsoft.com/en-us/library/ms181237(v=vs.120).aspx)(TFVC; 集中式原始檔控制)。 在 Microsoft Azure 的群組中有些小組使用集中式的原始檔控制，散佈，有些使用和某些使用混合 （集中式針對某些專案與發佈適用於其他專案）。 最多 5 位使用者可以免費 VSO 服務。 您可以註冊免費方案[這裡](https://go.microsoft.com/fwlink/?LinkId=307137)。

Visual Studio 2013 包含內建的第一級[Git 支援](https://msdn.microsoft.com/en-us/library/hh850437.aspx); 以下是快速的運作方式的示範。

使用 Visual Studio 2013 中開啟專案，以滑鼠右鍵按一下方案中的**方案總管] 中**，然後選擇 [**將方案加入至原始檔控制**。

![將方案加入至原始檔控制](source-control/_static/image9.png)

Visual Studio 會詢問您想要使用 TFVC （集中式的版本控制） 或 Git。

![選擇原始檔控制](source-control/_static/image10.png)

當您選取 Git 並按一下**確定**，Visual Studio 在您的方案資料夾中建立新的本機 Git 儲存機制。 新的儲存機制尚未任何檔案;您必須將它們加入至儲存機制進行 Git 認可。 以滑鼠右鍵按一下方案中的**方案總管] 中**，然後按一下 [**認可**。

![認可](source-control/_static/image11.png)

Visual Studio 會自動認可的專案檔案的所有階段，而且列在**Team Explorer**中**包含的變更**窗格。 (如果有一些您不想要包含在認可中，您無法選取它們，按一下滑鼠右鍵，然後按一下**排除**。)

![Team Explorer](source-control/_static/image12.png)

輸入認可註解，然後按一下**認可**，Visual Studio 會執行認可，並顯示的認可 id。

![Team Explorer 的變更](source-control/_static/image13.png)

現在如果您變更一些程式碼，使其從儲存機制中的不同，您可以輕鬆地檢視差異。 以滑鼠右鍵按一下檔案，您已經變更，選取**相比較的 Unmodified**，且您比較顯示，其中顯示您未認可的變更。

![比較與未修改](source-control/_static/image14.png)

![差異顯示變更](source-control/_static/image15.png)

您可以輕鬆地看到您做了哪些變更簽入。

假設您需要進行分支 – 您可以在 Visual Studio 中執行。 在**Team Explorer**，按一下 **新分支**。

![Team Explorer 的新分支](source-control/_static/image16.png)

輸入的分支名稱中，按一下**建立分支**，而且如果您選取**簽出分支**，Visual Studio 會自動簽出新的分支。

![Team Explorer 的新分支](source-control/_static/image17.png)

您現在可以對檔案進行變更，並簽入至這個分支。 此外，您可以輕鬆切換分支與 Visual Studio 之間自動同步處理者為準的分支，您的檔案已簽出。在此範例中的網頁標題中 *\_Layout.cshtml*已變更為 「 修正 1 」 在 HotFix1 分支。

![Hotfix1 分支](source-control/_static/image18.png)

如果您切換回主要分支，內容 *\_Layout.cshtml*檔案會自動還原為仍在主要分支。

![主要分支](source-control/_static/image19.png)

這個簡單的範例如何快速建立分支及分支之間來回翻轉。 此功能可讓高敏捷式軟體開發工作流程使用的分支結構和自動化指令碼所示[一切自動化](automate-everything.md)章節。 例如，您可以使用 「 開發 」 分支，建立主要問題修正分支、 切換至新分支、 讓您的變更和認可，然後切換到 「 開發 」 分支並繼續您所執行的動作。

您已在此看到為您搭配 Visual Studio 中的本機 Git 儲存機制的運作方式。 在小組環境中您通常也將變更推送到通用儲存機制。 Visual Studio 工具也可讓您以指向遠端 Git 儲存機制。 您可以使用 GitHub.com 基於這個目的，或者您可以使用[在 Visual Studio Online 中的 Git](https://msdn.microsoft.com/en-us/library/hh850437.aspx)與所有其他 Visual Studio Online 功能，例如工作項目和 bug 追蹤整合。

這不是您可以實作敏捷式軟體開發的分支策略，當然的唯一方式。 您可以啟用相同的敏捷式軟體開發工作流程，以使用集中式的原始檔控制儲存機制。

## <a name="summary"></a>總結

測量成功的原始檔控制系統速度有多快進行變更，並取得即時安全和可預測的方式為基礎。 如果您發現自己恐怖了進行變更，因為您只需要一天，或兩個的手動測試它，您可能會想知道您必須執行 process-wise 或 test-wise，讓您可以進行該變更，以分鐘為單位，或在不會再最差超過一小時。 其中一個策略，是實作連續整合及連續傳遞，我們會在[下一章](continuous-integration-and-continuous-delivery.md)。

<a id="resources"></a>
## <a name="resources"></a>資源

[Visual Studio Online](https://www.visualstudio.com/)入口網站提供文件和支援服務，您可以註冊帳戶。 如果您有 Visual Studio 2012，並想要使用 Git，請參閱[Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)。

如需 （集中式的版本控制） TFVC 和 Git （分散式的版本控制） 的詳細資訊，請參閱下列資源：

- [應該使用的版本控制系統： TFVC 或 Git？](https://msdn.microsoft.com/en-us/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN 文件，包含彙總的差異 TFVC 和 Git 的資料表。
- [嗯，我喜歡 Team Foundation Server 和我喜歡 Git，但是何者較佳嗎？](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Git 和 TFVC 的比較。

如需分支策略的詳細資訊，請參閱下列資源：

- [建立與 Team Foundation Server 2012 的發行管線](https://msdn.microsoft.com/en-us/library/dn449957.aspx)。 Microsoft Patterns and Practices 文件。 請參閱第 6 章分支策略的討論。 如果使用功能的分支，倡導人士等保持其存留較短 （小時或天最多） 與切換功能分支透過代言人功能。
- [版本控制指南](https://aka.ms/vsarsolutions)。 ALM Ranger 所引導分支策略。 在 [下載] 索引標籤上，請參閱分支 Strategies.pdf。
- [軟體開發功能切換](https://msdn.microsoft.com/en-us/magazine/dn683796.aspx)。 MSDN Magazine 文件。
- [功能切換](http://martinfowler.com/bliki/FeatureToggle.html)。 簡介功能切換/Martin Fowler 部落格上的功能旗標。
- [功能切換 vs 功能分支](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)。 關於功能切換，Dylan smith 的另一個部落格文章。

如需如何處理不會保留在原始檔控制儲存機制中的機密資訊的詳細資訊，請參閱下列資源：

- [ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳做法](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。
- [Azure Web Sites： 如何應用程式字串與連接字串的運作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)。 說明 Azure 功能，會覆寫`appSettings`和`connectionStrings`中的資料*Web.config*檔案。
- [自訂組態和應用程式設定 Azure 網站中-與 Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/)。

從原始檔控制保留機密資訊的其他方法的相關資訊，請參閱[ASP.NET MVC： 出原始檔控制中保留私用設定](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。

>[!div class="step-by-step"]
[上一頁](automate-everything.md)
[下一頁](continuous-integration-and-continuous-delivery.md)
