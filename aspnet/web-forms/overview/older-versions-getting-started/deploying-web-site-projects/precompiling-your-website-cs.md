---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: "先行編譯您的網站 (C#) |Microsoft 文件"
author: rick-anderson
description: "Visual Studio 可 ASP.NET 開發人員提供兩種專案類型： Web 應用程式專案 (WAPs) 及網站專案 (WSPs)。 其中一個主要差異 betwe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: f31f470b4d2b6736b98c0b7d88ea7a53ad1438b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="precompiling-your-website-c"></a>先行編譯您的網站 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio 可 ASP.NET 開發人員提供兩種專案類型： Web 應用程式專案 (WAPs) 及網站專案 (WSPs)。 其中的兩個專案類型之間的主要差異是 WAPs 必須明確地部署之前，先編譯而 WSP 中的程式碼可以在 web 伺服器上自動編譯的程式碼。 不過，很可能在部署之前的 WSP 先行編譯。 本教學課程探索先行編譯的優點，並示範如何先行編譯網站從 Visual Studio 內和從命令列。


## <a name="introduction"></a>簡介

Visual Studio 提供的 ASP.NET 開發人員兩種不同的專案類型： Web 應用程式專案 (WAP) 和網站專案 (WSP)。 其中一個這些專案類型之間的主要差異是 WAPs 需要*明確編譯*而 WSPs 使用*自動編譯*，根據預設。 WAPs，與您的 web 應用程式程式碼編譯成單一的組件，這是在網站的`Bin`資料夾。 部署需要複製標記內容 ( `.aspx.ascx`，和`.master`檔案) 專案中的組件中`Bin`資料夾; 程式碼後置類別檔本身不需要部署。 相反地，您可以部署 WSPs 藉由複製到生產環境的標記頁面和其相對應的程式碼後置類別。 程式碼後置類別會在 web 伺服器上視需要進行編譯。

> [!NOTE]
> 中的 「 明確編譯與自動編譯 」 一節會回頭[*決定需要的檔案部署*教學課程](determining-what-files-need-to-be-deployed-cs.md)專案之間差異的詳細背景模型、 明確和自動編譯和編譯模型如何影響部署。


自動編譯選項用法很簡單。 並沒有明確編譯步驟是，只有檔案已修改需要供部署，而明確編譯需要部署已變更的標記頁面和只編譯的組件。 不過，自動部署具有兩個缺點：

- 當第一次瀏覽必須自動編譯網頁，因為可以有短暫但明顯的延遲在部署後第一次要求的 ASP.NET 網頁時。
- 自動編譯需要這兩種宣告式標記和原始程式碼的程式碼會出現在 web 伺服器上。 如果您計劃銷售客戶將其 web 伺服器安裝 web 應用程式，這可以是不適當的選項。

如果是這兩個以上的缺點是處理詞工具，您可以切換至 WAP 模型或*先行編譯*部署之前，先 WSP。 本教學課程會檢查先行編譯選項最適合用來託管網站，並逐步引導的先行編譯處理序和先行編譯網站的部署。

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>ASP.NET 程式碼產生和編譯的概觀

我們看看可用的先行編譯選項之前，我們先討論的程式碼產生和編譯的 ASP.NET 網頁要求的第一次建立或上次更新之後，就會發生。 您知道，ASP.NET 網頁中則包含兩個部分： 在宣告式標記`.aspx`檔，以及原始碼部份，通常是在個別的程式碼後置類別檔 (`.aspx.cs`)。 要求的 ASP.NET 網頁時，執行階段所執行的步驟取決於應用程式的編譯模型。

與 WAPs，網頁的原始碼必須明確編譯成單一的組件，才能部署。 在部署期間，這個組件和各種不同的標記頁面會複製到實際執行環境。 當要求到達 ASP.NET 網頁的網頁伺服器時，執行階段建立網頁的程式碼後置類別的執行個體，並叫用其`ProcessRequest`方法，這個方法會啟動頁面生命週期，且最終會產生頁面的內容，傳回給要求者。 因為程式碼後置類別的已編譯的組件，在部署之前，執行階段可以使用 ASP.NET 網頁的程式碼後置類別。

WSPs 和自動編譯，並沒有明確編譯步驟是在部署之前。 相反地，如果部署涉及宣告式和來源的程式碼內容複製到實際執行環境。 要求到達時以 ASP.NET 網頁的網頁伺服器第一次已建立或上次更新頁面之後，執行階段必須先將程式碼後置類別編譯為組件。 這個編譯的組件會儲存在資料夾`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`，不過您可以透過自訂此資料夾的位置`<compilation tempDirectory="" />`元素`<system.web>`，通常在`Web.config`。 因為此組件儲存至磁碟，它不需要重新編譯的後續要求相同的頁面。

> [!NOTE]
> 如您所預期，時，會稍微延遲要求的站台，會編譯網頁的程式碼並儲存產生的組件，以伺服器時間，會使用自動編譯頁面第一次 （或第一次，因為它已被變更）磁碟。


簡單地說，使用明確編譯您必須編譯網站的原始程式碼，再進行部署，儲存的執行階段不需要執行該步驟。 使用自動編譯執行階段會處理頁面第一次的編譯網頁的原始碼，但有些微的初始化成本，因為它已建立或上次更新日期。

但 ASP.NET 網頁的宣告式一部分的情況為何 (`.aspx`檔案)？ 明顯是之間的關聯性`.aspx`檔案和它們的程式碼後置類別，做為宣告式標記中定義的 Web 控制項中的程式碼都可以存取程式碼中。 它也是很明顯，中的內容`.aspx`都會大大影響檔案產生頁面所呈現的標記。 如何在將執行階段運作以文字、 HTML、 和中的 Web 控制項語法定義`.aspx`檔案來產生要求的網頁的呈現的內容嗎？

我不想取得太 sidetracked 的低層級的實作詳細資料，WAPs 和 WSPs 之間有所差異，但簡言之執行階段自動產生類別檔案，其中包含不同的 Web 控制項做為受保護的成員和方法。 這個產生的檔案會實作為*部分類別*對應的程式碼後置類別。 ([部分類別](http://www.dotnet-guide.com/partialclasses.html)分散到多個檔案可供單一類別的內容。)因此，將程式碼後置類別定義在兩個地方： 在`.aspx.cs`您建立和此自動產生的類別中執行階段建立的檔案。 這個自動產生的類別會儲存在`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`資料夾。

重要拿掉以下是執行階段所要呈現 ASP.NET 網頁，這兩種及其宣告式和來源的程式碼部分必須編譯成組件。 WAPs，原始程式碼明確地編譯的組件，在部署之前，但宣告式標記仍會轉換成程式碼和 web 伺服器上的執行階段編譯。 使用自動編譯 WSPs，原始程式碼和宣告式標記需要 web 伺服器，來編譯。

您可用於根據 WSP 模型使用明確編譯。 您可以明確地編譯原始碼部份中，像 WAP 模型。 而且，您也可以編譯的宣告式標記。

## <a name="precompilation-options"></a>先行編譯選項

.NET Framework 隨附[ASP.NET 編譯工具 (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) ，可讓您編譯的 ASP.NET 應用程式使用 WSP 模型所建置的原始程式碼 （和甚至內容）。 此工具隨附於.NET Framework 2.0 版，且其位於`%WINDIR%\Microsoft.NET\Framework\v2.0.50727`資料夾; 可以從命令列或從 Visual Studio 中啟動透過 [建置] 功能表的 [發行網站] 選項。

編譯工具提供兩種一般的編譯形式： 就地先行編譯和部署的先行編譯。 在您執行就地先行編譯`aspnet_compiler.exe`從命令列工具，並指定您的電腦上的虛擬目錄或網站所在的實體路徑的路徑。 編譯工具然後編譯專案中，儲存在已編譯的版本中的每個 ASP.NET 網頁`%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`一樣如果頁面有每個已瀏覽第一次從瀏覽器的資料夾。 就地先行編譯可加速對您的網站上的新部署 ASP.NET 網頁，因為它可減輕就不需要執行此步驟的執行階段的第一個要求。 不過，就地先行編譯不適用於大部分的託管網站因為它需要，您都能執行程式，從 web 伺服器的命令列。 在共用裝載環境中不允許此存取層級。

> [!NOTE]
> 如需有關就地先行編譯的詳細資訊，請參閱[How To： 先行編譯的 ASP.NET Web Sites](https://msdn.microsoft.com/library/ms227972.aspx)和[先行編譯 ASP.NET 2.0 中的](http://www.odetocode.com/Articles/417.aspx)。


而不是編譯至網站中的網頁`Temporary ASP.NET Files`資料夾中，針對部署先行編譯會編譯要目錄您所選擇的格式，可以部署到生產環境中的頁面。

有兩種部署，我們在此教學課程中，瀏覽的先行編譯： 使用可更新的使用者介面，先行編譯和先行編譯不可更新的使用者介面。 先行編譯的可更新的使用者介面會保留中的宣告式標記`.aspx`， `.ascx`，和`.master`檔案，因此允許進行檢視，並如有需要，修改實際執行伺服器上的宣告式標記為開發人員。 不可更新的使用者介面的先行編譯會產生`.aspx`頁面的任何內容，並移除 void`.ascx`和`.master`檔案，藉以隱藏的宣告式標記，並禁止變更它從開發人員生產環境。

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>先行編譯的可更新的使用者介面的部署

若要了解部署的先行編譯，最好是在動作中的範例，請參閱。 讓我們先行編譯書籍檢閱用於根據 WSP 部署使用可更新的使用者介面。 ASP.NET 編譯工具可以從 Visual Studio 的 [建置] 功能表，或從命令列叫用。 本節會檢查使用工具在 Visual Studio;從命令列執行編譯器工具會查看 「 先行編譯從命令列 」 一節。

Visual Studio 中開啟活頁簿檢閱 WSP、 移至 [建置] 功能表中，然後選取 [發行網站] 功能表選項。 這會啟動 [發行網站] 對話方塊 (請參閱**圖 1**)，您可以在其中指定目標位置、 先行編譯的網站的使用者介面是否可更新的與其他編譯器工具選項。 目標位置可以是遠端網頁伺服器或 FTP 伺服器，但現在，請選擇您的電腦硬碟上的資料夾。 由於我們想要先行編譯的可更新的使用者介面與站台，將核取 [允許此先行編譯的網站成為可更新] 的核取方塊，然後按一下 [確定]。

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**圖 1**: ASP.NET Compilation Tool 會先行編譯網站，以指定的目標位置  
 ([按一下以檢視完整大小的影像](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> 無法在 Visual Web Developer 中使用 [建置] 功能表中的 [發行網站] 選項。 如果您使用 Visual Web Developer，您必須使用命令列版本的 ASP.NET 編譯工具，「 先行編譯從命令列 」 一節。


之後先行編譯網站，瀏覽至您在 [發行網站] 對話方塊中輸入的目標位置。 請花一點時間來比較您的網站內容與此資料夾的內容。 **圖 2**顯示書籍檢閱網站資料夾。 請注意，它同時包含`.aspx`和`.aspx.cs`檔案。 另請注意，`Bin`目錄包含只有一個檔案， `Elmah.dll`，我們中加入[前述教學課程](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**圖 2**： 專案目錄中包含`.aspx`和`.aspx.cs`檔案;`Bin`資料夾只包含`Elmah.dll`  
 ([按一下以檢視完整大小的影像](precompiling-your-website-cs/_static/image6.png))

**圖 3**顯示其內容所建立的 ASP.NET 編譯工具的目標位置資料夾。 這個資料夾不包含任何程式碼後置檔案。 此外，此資料夾`Bin`目錄都會包括數個組件和兩個`.compiled`檔案除了`Elmah.dll`組件。

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**圖 3**： 的目標位置的資料夾包含檔案，部署  
 ([按一下以檢視完整大小的影像](precompiling-your-website-cs/_static/image9.png))

不同於在 WAPs 明確編譯，先行編譯的部署程序不會建立一個組件的整個站台。 相反地，它會批次處理一起數個頁面到每個組件。 它也會編譯`Global.asax`到它自己組件，以及在任何類別檔案 （如果有的話）`App_Code`資料夾。 保存適用於 ASP.NET 的宣告式標記檔案 web 網頁、 使用者控制項和主版頁面 (`.aspx`， `.ascx`，和`.master`分別檔案) 複製到做為-為目標位置的目錄。 同樣地，`Web.config`檔案會直接透過複製以及任何靜態檔案，例如影像、 CSS 類別和 PDF 檔案。 如需更正式的編譯工具如何處理各種說明檔案類型，請參閱[檔案處理期間 ASP.NET 先行編譯](https://msdn.microsoft.com/library/e22s60h9.aspx)。

> [!NOTE]
> 您可以指示編譯工具來建立 ASP.NET 頁面、 使用者控制項或主版頁面的每一個組件藉由檢查從發行網站對話方塊中的 「 使用固定命名和單一網頁組件 」 核取方塊。 每個 ASP.NET 網頁，編譯成自己的組件可讓部署更多更細緻的控制。 例如，如果您更新單一 ASP.NET 網頁，並部署該變更時需要您需要只部署該頁面`.aspx`檔案和相關聯的組件到生產環境。 請參閱[How To： 使用 ASP.NET 編譯工具產生固定名稱](https://msdn.microsoft.com/library/ms228040.aspx)如需詳細資訊。


目標位置的目錄也會包含不是一部分的先行編譯的 web 專案，也就是一個檔案`PrecompiledApp.config`。 這個檔案會通知應用程式已先行編譯的 ASP.NET 執行階段和是否先行具有可更新或中午可更新的 UI。

最後，請花一點時間開啟其中一個`.aspx`目標位置，使用 Visual Studio 或您選擇的文字編輯器中的檔案。 先行編譯的可更新的使用者介面的部署，當 ASP.NET 網頁的目標位置的目錄中包含之網站中的對應檔案完全相同的標記。

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>針對部署的非可更新的使用者介面與先行編譯

ASP.NET 編譯器工具也可用來部署的站台與非更新的 UI 先行編譯。 先行編譯不可更新的 UI 與站台的作用很像先行編譯使用更新的 UI，主要差異在於 ASP.NET 網頁、 使用者控制項和主版頁面的目標目錄中會移除其標記。 先行編譯網站，以取得部署具有非可更新的 UI，從 [建置] 功能表中，選擇 [發行網站] 選項，但是取消核取 [允許此先行編譯的網站成為可更新] 選項 (請參閱**圖 4**)。

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**圖 4**： 取消核取 [允許此先行編譯的網站成為可更新] 選項來先行編譯與非更新的 UI  
 ([按一下以檢視完整大小的影像](precompiling-your-website-cs/_static/image12.png))

**圖 5**顯示之後不可更新的使用者介面使用先行編譯的目標位置的資料夾。

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**圖 5**： 不可更新的 UI 與部署的目標位置資料夾  
 ([按一下以檢視完整大小的影像](precompiling-your-website-cs/_static/image15.png))

比較**圖 3**至**圖 5**。 雖然看起來相同的兩個資料夾，請注意不可更新的 UI 資料夾缺少主版頁面`Site.master`。 當**圖 5**包含各種不同的 ASP.NET 網頁中，如果您檢視了已移除其宣告式標記並取代預留位置文字，您會看到這些檔案的內容:"This is 所產生的標記檔案先行編譯工具，而且不會刪除 ！ 」

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**圖 5**： 宣告式標記已從 ASP.NET 網頁

`Bin`資料夾中的**數字 3**和**5**太多大的不同。 組件，除了`Bin`資料夾中的**圖 5**包含`.compiled`每個 ASP.NET 網頁、 使用者控制項和主版頁面檔案。

先行編譯的站台與非更新的 UI 中很有用，您不想要修改的個人或公司安裝或管理在生產環境網站的 ASP.NET 網頁的內容。 如果您建立您賣給客戶，他們自己的 web 伺服器上安裝 ASP.NET web 應用程式，您可能想要確保，而不會修改您的站台的外觀及操作直接編輯`.aspx`將它們寄送的頁面。 先行編譯您的網站與非更新的 UI，出貨預留位置`.aspx`安裝的一部分，因此可以防止您的客戶，檢查或修改其內容頁。

### <a name="precompiling-from-the-command-line"></a>從命令列先行編譯

在幕後，Visual Studio 的 [發行網站] 對話方塊會叫用 ASP.NET 編譯工具 (`aspnet_compiler.exe`) 先行編譯網站。 或者，您可以叫用此工具從命令列。 事實上，如果您使用 Visual Web Developer 然後您必須執行編譯器工具從命令列，因為 Visual Web Developer 的 [建置] 功能表中不包含發行網站選項。

若要使用編譯器工具，從命令列，啟動 命令列來卸除並巡覽至 framework 目錄中， `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`。 接下來，在命令列中輸入下列陳述式：

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

上述命令會啟動 ASP.NET 編譯器工具 (`aspnet_compiler.exe`) 及透過`-p`參數，指示它先行編譯網站根目錄*實體\_路徑\_至\_應用程式*;這個值會像`C:\MySites\BookReviews`，並應該以引號分隔。

`-v`參數指定的站台的虛擬目錄。 如果您的網站是否註冊為 IIS metabase 中的預設網站，則可以省略`-p`切換，並只需要指定虛擬目錄的應用程式。 如果您使用`-p`切換、 值再繼續`-v`參數表示網站的根目錄，而用來解析應用程式根目錄的參考。 比方說，如果您指定的值`-v /MySite`然後應用程式中參考`~/path/file`會解析為`~/MySite/path/file`。 因為活頁簿檢閱站台位於根目錄在我的 web 裝載公司我已經使用交換器`-v /`。

`-f`參數，如果有的話，會指示編譯工具，以覆寫*目標\_位置\_資料夾*目錄已經存在。 如果您省略`-f`交換器和目標位置的資料夾已經存在，編譯工具將會結束，發生錯誤: 「 錯誤 ASPRUNTIME： 不是空的目標目錄。 請手動將它刪除或選擇不同的目標。 」

`-u`參數，如果有的話，會通知工具，以建立可更新的使用者介面。 省略這個參數來先行編譯的站台與非可更新的使用者介面。

最後，*目標\_位置\_資料夾*是目標位置的目錄; 的實體路徑這個值會像`C:\MySites\Output\BookReviews`，並應該以引號分隔。

## <a name="deploying-the-precompiled-website"></a>部署先行編譯的網站

現在我們已經看到如何使用 ASP.NET 編譯工具先行編譯網站，使用可更新而非可更新的使用者介面選項。 不過，我們的範例為止有先行編譯網站的本機資料夾，而非實際執行環境。 好消息是，部署先行編譯的網站是十分簡單，即可透過 Visual Studio 或其他一些檔案複製機制，例如從獨立的 FTP 用戶端。

[發行網站] 對話方塊 (在第一次顯示**圖 1**) 具有目標位置選項，指出先行編譯的網站檔案複製到其中。 此位置可以是遠端網頁伺服器或 FTP 伺服器。 此文字方塊中輸入遠端伺服器時，會先行編譯，並將網站部署到在一個步驟中指定的伺服器。 或者，您可以先行編譯網站的本機資料夾，然後手動將該資料夾的內容複製到實際執行環境中透過 FTP 或某些其他方法。

需要透過 Visual Studio 的 [發行網站] 對話方塊會自動部署的先行編譯的網站很簡單的網站在有設定之間沒有差異開發和生產環境。 不過，如中所述[*通用組態差異之間開發和生產*教學課程](common-configuration-differences-between-development-and-production-cs.md)並不是罕見狀況存在這類差異。 比方說，書籍檢閱 web 應用程式會在開發環境中比生產環境中使用不同的資料庫。 當 Visual Studio 會將網站發行到它盲目地複製設定檔資訊，在開發環境中的遠端伺服器。

設定開發和生產環境之間的差異站台可能是最佳先行編譯網站的本機目錄，複製的實際執行環境專屬的組態檔，然後將複製的先行編譯輸出內容生產環境。

如將檔案從開發環境複製到實際執行環境的重新整理程式，請參閱[*部署您使用 FTP 用戶端的網站*](deploying-your-site-using-an-ftp-client-cs.md)和[ *部署您的網站使用 Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md)教學課程。

## <a name="summary"></a>總結

ASP.NET 支援兩種編譯模式： 自動與明確。 如先前的教學課程所述，Web 應用程式專案 (WAPs) 會使用明確編譯，而預設的網站專案 (WSPs) 使用自動編譯、。 不過，它可能會明確編譯的 WSP 部署之前，先使用 ASP.NET 編譯工具。

本教學課程著重於部署支援的編譯工具先行編譯。 當先行編譯的部署，編譯工具建立的目標位置的資料夾、 編譯指定的 web 應用程式的原始程式碼，並將這些複製到目標位置資料夾編譯的組件和內容的檔案。 編譯工具可以設定為建立可更新或非可更新的使用者介面。 不可更新的使用者介面選項使用先行編譯，會移除在內容檔案中的宣告式標記。 簡而言之，先行編譯可讓您部署您的網站專案為基礎的應用程式，但不包含任何原始程式碼檔和以宣告式標記中移除，如有需要。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 網站先行編譯](https://msdn.microsoft.com/library/ms228015.aspx)
- [程式碼後置和 ASP.NET 2.0 中的編譯](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [在 ASP.NET 中的先行編譯](http://www.odetocode.com/Articles/417.aspx)
- [在 ASP.NET 中的 先行編譯的網站選項](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[上一頁](logging-error-details-with-elmah-cs.md)
[下一頁](users-and-roles-on-the-production-website-cs.md)
