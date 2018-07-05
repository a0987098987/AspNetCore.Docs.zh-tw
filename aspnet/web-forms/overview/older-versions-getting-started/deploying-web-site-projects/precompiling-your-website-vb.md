---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: 先行編譯您的網站 (VB) |Microsoft Docs
author: rick-anderson
description: Visual Studio 為 ASP.NET 開發人員提供兩種專案類型： Web 應用程式專案 (Wap) 和網站專案 (WSPs)。 其中一個主要差異 betwe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: ab13f162af8dab679010c63d8c0005b525473a34
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371112"
---
<a name="precompiling-your-website-vb"></a>先行編譯您的網站 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio 為 ASP.NET 開發人員提供兩種專案類型： Web 應用程式專案 (Wap) 和網站專案 (WSPs)。 其中的兩個專案類型之間的主要差異是 Wap 必須明確地編譯在部署之前，而 WSP 中的程式碼可以在 web 伺服器上自動編譯的程式碼。 不過，就可以在部署之前的 WSP 的先行編譯。 本教學課程將探討先行編譯的優點，並示範如何先行編譯網站從 Visual Studio 內，並從命令列。


## <a name="introduction"></a>簡介

Visual Studio 為 ASP.NET 開發人員提供兩種不同的專案類型： Web 應用程式專案 (WAP) 和網站專案 (WSP)。 其中一個這些專案類型之間的主要差異是 Wap 需要*明確編譯*而使用 WSPs*自動編譯*，根據預設。 Wap，與您 web 應用程式的程式碼編譯成單一組件，這是在網站的`Bin`資料夾。 部署需要複製標記的內容 ( `.aspx.ascx`，並`.master`檔案) 在專案中的組件以及`Bin`資料夾; 程式碼後置類別檔案本身不需要部署。 相反地，您可以將 [標記] 頁面和其對應的程式碼後置類別複製到生產環境部署 WSPs。 在 web 伺服器上隨編譯的程式碼後置類別。

> [!NOTE]
> 回頭參考中的 「 明確編譯與自動編譯 」 一節[*判斷哪些檔案需要部署*教學課程](determining-what-files-need-to-be-deployed-vb.md)專案之間差異的詳細背景模型、 明確和自動編譯和編譯模型如何影響部署。


自動編譯選項很容易使用。 沒有明確的編譯步驟，並只將此檔案已修改的需要來部署，而明確編譯需要部署已變更的標記頁面和剛編譯組件。 不過，自動部署具有兩個缺點：

- 由於頁面必須自動編譯，當第一次瀏覽，因此可能會簡短但明顯的延遲 ASP.NET 網頁要求在部署後第一次時。
- 自動編譯需要這兩種宣告式標記和來源的程式碼會出現在 web 伺服器上。 如果您計劃銷售給客戶會將它安裝在他們的 web 伺服器的 web 應用程式，這可能會不討喜的選項。

如果是上述缺點兩個大詞工具，您可以切換至 WAP 模型或*先行編譯*在部署之前 WSP。 本教學課程會檢查最適合用來裝載網站的先行編譯選項，並逐步解說的先行編譯的程序和先行編譯網站的部署。

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>ASP.NET 程式碼產生和編譯的概觀

我們查看可用的先行編譯選項之前，讓我們先討論的程式碼產生和編譯的 ASP.NET 網頁要求第一次建立或上次更新時，就會發生。 如您所知，ASP.NET 網頁由兩個部分組成： 中的宣告式標記`.aspx`檔，以及來源的程式碼部分，通常是在個別的程式碼後置類別檔案 (`.aspx.vb`)。 在要求 ASP.NET 網頁時，執行階段所執行的步驟取決於應用程式的編譯模型。

與 Wap，頁面的原始程式碼必須明確地編譯成單一的組件，再部署。 在部署期間，這個組件和各種不同的標記頁面會複製到實際執行環境。 當要求抵達到 ASP.NET 網頁的 web 伺服器時，執行階段建立網頁的程式碼後置類別的執行個體，並叫用其`ProcessRequest`方法，它會啟動網頁生命週期，並最終會產生頁面的內容，傳回給要求者。 因為程式碼後置類別的已編譯的組件，在部署之前，執行階段可以使用 ASP.NET 頁面的程式碼後置類別。

WSPs 和自動編譯，並沒有明確的編譯步驟是在部署之前。 相反地，部署牽涉到將宣告式和來源的程式碼內容複製到生產環境。 當要求抵達時以 ASP.NET 網頁的網頁伺服器第一次因為已建立或上次更新頁面上，執行階段必須先將程式碼後置類別編譯成組件。 此編譯的組件會儲存在資料夾`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`，雖然此資料夾的位置可以透過自訂`<pages>`中的項目`Web.config`。 因為此組件儲存至磁碟，不需要重新編譯的後續要求相同的頁面。

> [!NOTE]
> 如您所預期，沒有稍有延遲，當要求中使用自動編譯時間，因為需要一點時間來編譯網頁的程式碼並儲存產生的組件伺服器的站台的頁面第一次 （或第一次，因為它已變更）磁碟。


簡單地說，使用明確的編譯您必須編譯網站的原始程式碼，再進行部署，不需要執行該步驟中儲存的執行階段。 自動編譯與執行階段會處理頁面第一次開啟的編譯頁面的原始程式碼，但有稍微初始化成本，因為它已建立或上次更新日期。

但如果是 ASP.NET 網頁的宣告式部分 (`.aspx`檔案)？ 很明顯地是之間的關聯性`.aspx`檔案和其程式碼後置類別，做為宣告式標記中定義的 Web 控制項中的程式碼會在程式碼中存取。 很明顯，中的內容`.aspx`都會大大影響檔案產生頁面所呈現的標記。 如何在將執行階段運作與文字、 HTML、 和中的 Web 控制項語法定義`.aspx`檔案來產生要求的網頁的呈現的內容嗎？

我不想要取得太延宕低階實作詳細資料，Wap 和 WSPs 而有所不同，但概括地說，執行階段會自動產生類別檔案，其中包含各種 Web 控制項做為受保護的成員和方法。 這個產生的檔案會實作成*部分類別*至對應的程式碼後置類別。 ([部分類別](http://www.dotnet-guide.com/partialclasses.html)允許內容的單一類別分散到多個檔案。)因此，在兩個位置，定義程式碼後置類別： 在`.aspx.vb`您所建立，並在這個自動產生的類別中執行階段建立的檔案。 這個自動產生的類別會儲存在`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`資料夾。

重要撤走如下，ASP.NET 網頁的執行階段所要呈現的這兩個其宣告式和來源的程式碼部分必須編譯成組件。 Wap，原始程式碼明確地編譯的組件，在部署之前，但宣告式標記仍會轉換成程式碼和 web 伺服器上的執行階段編譯。 使用自動編譯 WSPs，原始碼和宣告式標記必須要編譯之 web 伺服器。

您可用於根據 WSP 模型中使用明確的編譯。 您可以明確編譯來源的程式碼部分，像 WAP 模型。 除此之外，您也可以編譯宣告式標記。

## <a name="precompilation-options"></a>先行編譯選項

.NET Framework 隨附[ASP.NET 編譯工具 (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) ，可讓您編譯使用 WSP 模型來建置 ASP.NET 應用程式的原始程式碼 （和甚至內容）。 此工具隨附在.NET Framework 2.0 版，且位於`%WINDIR%\Microsoft.NET\Framework\v2.0.50727`資料夾; 可以從命令列使用或啟動從 Visual Studio 內透過 [建置] 功能表的 [發行網站] 選項。

編譯工具提供兩種編譯的一般形式： 就地先行編譯和先行編譯的部署。 在您執行就地先行編譯`aspnet_compiler.exe`從命令列工具，並指定您的電腦上的虛擬目錄或所在的網站實體路徑的路徑。 然後編譯工具編譯的每個 ASP.NET 頁面，在專案中，儲存在已編譯的版本`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`一樣如果頁面有每個已瀏覽第一次從瀏覽器的資料夾。 就地先行編譯可加快對新部署網站上的 ASP.NET 頁面，因為它可減輕不需要執行這個步驟的執行階段的第一個要求。 不過，就地先行編譯不適用於大部分的託管的網站因為它需要，您都能執行程式從 web 伺服器的命令列。 在共用主控環境中不允許此層級的存取。

> [!NOTE]
> 如需有關就地先行編譯的詳細資訊，請參閱[How To： 先行編譯 ASP.NET Web Sites](https://msdn.microsoft.com/library/ms227972.aspx)並[先行編譯 ASP.NET 2.0 中的](http://www.odetocode.com/Articles/417.aspx)。


而不是編譯中網站的頁面`Temporary ASP.NET Files`資料夾中，針對部署先行編譯編譯的頁面，您所選擇的和的格式，可以部署到生產環境中的目錄。

有兩種部署，我們會探究在本教學課程的先行編譯： 使用可更新的使用者介面，先行編譯和先行編譯不可更新的使用者介面。 先行編譯的可更新的使用者介面會保留中的宣告式標記`.aspx`， `.ascx`，和`.master`檔案，藉此可讓開發人員檢視，並如有需要，修改實際執行伺服器上的宣告式標記。 不可更新的使用者介面的先行編譯會產生`.aspx`void 的任何內容，並移除的頁面`.ascx`和`.master`檔案，藉此隱藏的宣告式標記及禁止從變更的開發人員生產環境。

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>先行編譯的可更新的使用者介面部署

若要查看作用中的範例，是了解部署的先行編譯的最佳方式。 讓我們先行編譯書籍評論 WSP，使用可更新的使用者介面進行部署。 可以叫用 ASP.NET 編譯工具，從 Visual Studio 的 [建置] 功能表或從命令列。 本節將探討從使用工具在 Visual Studio;從命令列執行編譯器工具查看 「 先行編譯的命令列 」 一節。

Visual Studio 中開啟活頁簿檢閱 WSP，移至 [建置] 功能表中，然後選取 [發行網站] 功能表選項。 這會啟動 發行網站 對話方塊中 (請參閱**圖 1**)，您可以在其中指定目標位置、 是否不是可更新先行編譯的網站使用者介面和工具的其他編譯器選項。 目標位置可以是遠端網頁伺服器或 FTP 伺服器，但現在選擇 您的電腦硬碟上的資料夾。 因為我們想要先行編譯的可更新的使用者介面與站台，將核取 [允許此先行編譯的網站成為可更新] 的核取方塊，然後按一下 [確定]。

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**圖 1**: ASP.NET 編譯工具將先行編譯網站，以指定的目標位置  
 ([按一下以檢視完整大小的影像](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> 無法在 Visual Web Developer 中使用 [建置] 功能表中的 [發行網站] 選項。 如果您使用 Visual Web Developer，您將需要使用 ASP.NET 編譯工具，「 先行編譯的命令列 」 一節中所含的命令列版本。


之後先行編譯的網站，瀏覽至您在 [發行網站] 對話方塊中輸入的目標位置。 請花一點時間比較此資料夾的內容，您的網站的內容。 **圖 2**顯示書籍評論網站資料夾。 請注意，它會包含這兩`.aspx`和`.aspx.cs`檔案。 另請注意，`Bin`目錄會包含只有一個檔案， `Elmah.dll`，這我們在中加入[前述教學課程](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**圖 2**： 包含專案目錄`.aspx`並`.aspx.cs`檔案;`Bin`資料夾只包含 `Elmah.dll`  
 ([按一下以檢視完整大小的影像](precompiling-your-website-vb/_static/image6.png))

**圖 3**顯示其內容所建立的 ASP.NET 編譯工具的目標位置資料夾。 這個資料夾不包含任何程式碼後置檔案。 此外，此資料夾`Bin`目錄會包含數個組件和兩個`.compiled`檔案除了`Elmah.dll`組件。

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**圖 3**： 的目標位置的資料夾包含了部署的檔案  
 ([按一下以檢視完整大小的影像](precompiling-your-website-vb/_static/image9.png))

不同於在 Wap 的明確編譯，先行編譯的部署程序不會建立整個站台的一個組件。 相反地，它會批次處理一起數個頁面到每個組件。 它也會編譯`Global.asax`成它自己組件，以及在任何類別檔案 （如果有的話）`App_Code`資料夾。 檔案支援 ASP.NET 的宣告式標記的 web 網頁、 使用者控制項和主版頁面 (`.aspx`， `.ascx`，和`.master`檔案，分別) 做為複製-為目標位置的目錄。 同樣地，`Web.config`檔案會直接透過複製以及任何的靜態檔案，例如影像、 CSS 類別和 PDF 檔案。 如需更正式的編譯工具如何處理各種說明檔案類型，請參閱[檔案處理期間 ASP.NET 先行編譯](https://msdn.microsoft.com/library/e22s60h9.aspx)。

> [!NOTE]
> 您可以指示編譯工具來建立每個 ASP.NET 網頁、 使用者控制項或主版頁面的一個組件從 [發行網站] 對話方塊中的 [使用固定命名和單一頁面的組件] 核取方塊。 編譯成自己的組件的每個 ASP.NET 頁面可讓部署更精密地控制。 例如，如果您更新單一 ASP.NET 網頁，而且需要部署這項變更，您需要只部署該頁面`.aspx`檔案和相關聯的組件到生產環境。 請參閱[How To： 使用 ASP.NET 編譯工具產生固定名稱](https://msdn.microsoft.com/library/ms228040.aspx)如需詳細資訊。


目標位置的目錄也包含檔案，並不是一部分的先行編譯的 web 專案，也就是`PrecompiledApp.config`。 此檔案可讓應用程式已先行編譯 ASP.NET 執行階段，而不論它已先行編譯，具有可更新或中午可更新的 UI。

最後，請花一點時間開啟其中一個`.aspx`使用 Visual Studio 或您選擇的文字編輯器的目標位置中的檔案。 先行編譯的可更新的使用者介面的部署，當目標位置的目錄中的 ASP.NET 網頁會包含網站中的對應檔案完全相同的標記。

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>先行編譯為不可更新的使用者介面的部署

ASP.NET 編譯器工具也可用來先行編譯網站部署使用不可更新 UI。 先行編譯網站來使用不可更新 UI 的運作方式很像先行編譯可更新的 ui，主要的差別在於，ASP.NET 網頁、 使用者控制項和目標目錄中的主版頁面會移除其標記。 先行編譯網站部署使用不可更新 UI，請從 建置 功能表中，選擇 發行網站 選項，但取消核取 允許此先行編譯的網站成為可更新 選項 (請參閱**圖 4**)。

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**圖 4**： 取消核取 允許此先行編譯的網站成為可更新 選項來先行編譯使用不可更新 UI  
 ([按一下以檢視完整大小的影像](precompiling-your-website-vb/_static/image12.png))

**圖 5**顯示之後的非可更新的使用者介面使用先行編譯的目標位置的資料夾。

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**圖 5**： 不可更新的 UI 具有部署的目標位置資料夾  
 ([按一下以檢視完整大小的影像](precompiling-your-website-vb/_static/image15.png))

比較**圖 3**要**圖 5**。 雖然看起來完全相同的兩個資料夾，請注意不可更新的 UI 資料夾缺少主版頁面中， `Site.master`。 當**圖 5**包含不同的 ASP.NET 頁面中，如果您檢視了已移除其宣告式標記並取代預留位置文字，您會看到這些檔案的內容: 「 這是所產生的標記檔案先行編譯的工具，而且不應該刪除 ！ 」

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**圖 5**： 宣告式標記已從 ASP.NET 網頁

`Bin`中的資料夾**數字 3**並**5**更大幅不同。 組件，除了`Bin`中的資料夾**圖 5**包含`.compiled`每個 ASP.NET 網頁、 使用者控制項和主版頁面檔案。

先行編譯的站台與非更新的 UI 是適用於的情況下您不想要修改的個人或公司，他們會安裝或管理在生產環境中的網站的 ASP.NET 頁面的內容。 如果您要建置您銷售產品給客戶，他們自己的 web 伺服器上安裝 ASP.NET web 應用程式，您可能想要確保，而不會修改外觀及操作您的網站直接編輯`.aspx`將它們寄送的頁面。 先行編譯您的網站使用不可更新 UI，您寄送預留位置`.aspx`網頁設為安裝的一部分，藉此防止您的客戶，檢查或修改其內容。

### <a name="precompiling-from-the-command-line"></a>從命令列先行編譯

在幕後，Visual Studio 的 [發行網站] 對話方塊中叫用 ASP.NET 編譯工具 (`aspnet_compiler.exe`) 為先行編譯網站。 或者，您可以叫用此工具從命令列。 事實上，如果您使用 Visual Web Developer 然後您必須從命令列中，執行編譯器工具如 Visual Web Developer 的 建置 功能表中沒有發行網站 選項。

若要使用命令列編譯器工具，啟動 命令列來卸除，並瀏覽至 framework 目錄中， `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`。 接下來，輸入下列陳述式到命令列：

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

上述命令會啟動 ASP.NET 編譯器工具 (`aspnet_compiler.exe`) 並透過`-p`切換，會指示它先行編譯網站根目錄*實體\_路徑\_至\_應用程式*;這個值會類似`C:\MySites\BookReviews`，以及應該以引號來分隔。

`-v`參數會指定虛擬目錄的站台。 如果您的網站會註冊為 IIS metabase 中的預設網站，則您可以省略`-p`參數，並只指定應用程式的虛擬目錄。 如果您使用`-p`切換，值繼續`-v`參數表示的網站的根目錄，並用來解析應用程式根目錄的參考。 比方說，如果您指定的值`-v /MySite`然後應用程式中參考`~/path/file`會解析為`~/MySite/path/file`。 因為書籍評論網站的根目錄，在我的 web 主機服務公司在我使用交換器`-v /`。

`-f`參數，如果有的話，會指示要覆寫 [編譯] 工具*目標\_位置\_資料夾*目錄已經存在。 如果您省略`-f`交換器和目標位置的資料夾已經存在，[編譯] 工具將會結束，發生錯誤: 「 錯誤 ASPRUNTIME： 不是空的目標目錄。 請手動刪除或選擇不同的目標。 」

`-u`參數，如果有的話，會通知來建立可更新的使用者介面工具。 省略此參數，為先行編譯的站台具有不可更新的使用者介面。

最後，*目標\_位置\_資料夾*是實體的路徑到目標位置的目錄; 這個值會類似`C:\MySites\Output\BookReviews`，以及應該以引號來分隔。

## <a name="deploying-the-precompiled-website"></a>先行編譯的網站部署

現在我們已了解如何使用 ASP.NET 編譯工具，為先行編譯網站，使用可更新且不可更新的使用者介面選項。 不過，我們的範例中到目前為止已先行編譯網站的本機資料夾，而不適用於生產環境。 好消息是，部署先行編譯的網站就可以輕而易舉的而且可以透過 Visual Studio 或透過完成一些檔案複製機制，例如從獨立的 FTP 用戶端。

發行網站 對話方塊中 (在第一次顯示**圖 1**) 具有目標位置選項，指出先行編譯的網站檔案複製到其中。 此位置可以是遠端網頁伺服器或 FTP 伺服器。 此文字方塊中輸入遠端伺服器時，會先行編譯，並將網站部署到在一個步驟中指定的伺服器。 或者，您可以先行編譯網站的本機資料夾，然後手動將該資料夾的內容複製到實際執行環境，透過 FTP 或其他方法。

透過 Visual Studio 的 [發行網站] 對話方塊會自動部署的先行編譯的網站是很有幫助的簡單網站有任何組態之間的差異開發和生產環境。 不過，如中所述[*常見組態差異之間開發和生產環境*教學課程](common-configuration-differences-between-development-and-production-vb.md)並不是罕見狀況存在這類差異。 比方說，書籍評論的 web 應用程式會在開發環境與生產環境中使用不同的資料庫。 當 Visual Studio 會將網站發佈到遠端伺服器會盲目地複製在開發環境中的設定檔資訊。

針對開發和生產環境的組態差異站台可能是輸出的最佳先行編譯網站的本機目錄，複製生產環境特定組態檔，然後複製內容的先行編譯，生產環境。

如複習一下檔案複製到生產環境的開發環境，請參閱[*部署您的網站使用 FTP 用戶端*](deploying-your-site-using-an-ftp-client-vb.md)並[ *部署您的網站使用的 Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md)教學課程。

## <a name="summary"></a>總結

ASP.NET 支援兩種編譯模式： 自動和明確。 先前的教學課程所述，Web 應用程式專案 (Wap) 會使用明確的編譯，而網站專案 (WSPs) 預設會使用自動編譯。 不過，就可以明確地使用 ASP.NET 編譯工具編譯在部署之前的 WSP。

本教學課程著重於部署支援的編譯工具的先行編譯。 時先行編譯的部署，編譯工具建立的目標位置的資料夾，會編譯指定的 web 應用程式的原始程式碼，並複製這些編譯組件和內容檔案到目標位置的資料夾。 若要建立可更新或非可更新的使用者介面，可以設定編譯工具。 先行編譯不可更新的使用者介面選項，會移除在內容檔案中的宣告式標記。 簡單的說，先行編譯可讓您部署您的網站專案為基礎的應用程式，但不包含任何原始程式碼檔和以宣告式標記移除，如有需要。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 網站先行編譯](https://msdn.microsoft.com/library/ms228015.aspx)
- [程式碼後置和 ASP.NET 2.0 中的編譯](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [在 ASP.NET 中的先行編譯](http://www.odetocode.com/Articles/417.aspx)
- [在 ASP.NET 中的先行編譯的站台選項](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [上一頁](logging-error-details-with-elmah-vb.md)
> [下一頁](users-and-roles-on-the-production-website-vb.md)
