---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: 在 Visual Studio 2005 中的改良 |Microsoft Docs
author: microsoft
description: Visual Studio 2005 提供一長串改進和增強功能，Web 專案的 Web 應用程式開發人員。
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 0a42699381fd326891898e01b4e98662e9ce22bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811366"
---
<a name="improvements-in-visual-studio-2005"></a>在 Visual Studio 2005 的增強功能
====================
by [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 提供一長串改進和增強功能，Web 專案的 Web 應用程式開發人員。


Visual Studio 2005 提供一長串改進和增強功能，Web 專案的 Web 應用程式開發人員。 強大的 Visual Studio.NET 2002年和 2003年是，沒有多抱怨 Web 專案已處理的方式。 Visual Studio 2005 加入大量的新功能，若要解決這些抱怨。 對於喜歡 Visual Studio.NET 2003年處理的 Web 應用程式編譯的方式，請參閱[Web 應用程式專案](https://go.microsoft.com/fwlink/?LinkId=57870)。

在這個模組中，我們將介紹中建立 Web 專案、 管理和開發的增強功能。 在更新版本的模組中，我們將介紹增強功能，建置 Web 專案，並將其部署。

## <a name="frontpage-server-extensions"></a>FrontPage Server Extensions

Visual Studio.NET 2002年和 2003年所需的方塊上才能建立或建立 Web 專案的 FrontPage Server Extensions。 開發人員有兩種不同的存取模式 （FrontPage Server Extensions 或檔案存取模式） 之間做選擇，同時用來執行工作，例如設定 IIS 等等的應用程式根目錄的 FrontPage Server Extensions。

Visual Studio 2005 會依賴 FrontPage Server Extensions 的本機專案中移除。 Visual Studio 2005 現在會存取 IIS metabase 直接而不是使用 FrontPage Server Extensions。 Visual Studio 2005 也會新增為 FTP 以達到遠端專案存取權，而不需要 FrontPage Server Extensions 的支援。

開發人員想要在其專案中使用 FrontPage Server Extensions 而言，此選項為仍然可用。 不過，根據從 ASP.NET 開發人員社群的強式回饋，它不需要。

> [!NOTE]
> 仍然需要遠端專案建立、 開啟等 FrontPage Server Extensions。


## <a name="aspnet-development-server"></a>ASP.NET 程式開發伺服器

Visual Studio 2005 隨附新的 Web 伺服器，稱為 ASP.NET 程式開發伺服器。 （此網頁伺服器先前稱為 Cassini）。

有許多的 ASP.NET 程式開發伺服器的數個優點。

- 現在，則為非系統管理員能夠開發和偵錯對 Web 伺服器。
- ASP.NET Development Server 動態虛擬目錄中對應至任何位置以彈性的專案位置的檔案系統。
- 在 Windows XP Professional 上已經在使用 IIS 的使用者現在可以建立新的 Web 應用程式將不會影響其預設網站上的 IIS 中的檔案或資料夾結構。

任何特殊設定，不才能充分善用 ASP.NET 程式開發伺服器。 當偵錯或瀏覽檔案系統裝載的 Web 專案時，Visual Studio 2005 會自動啟動 ASP.NET 程式開發伺服器的執行個體隨機的連接埠，服務要求。

在 ASP.NET Development Server，稍後在本單元中，將會說明的詳細資訊。

## <a name="improved-file-management"></a>改善的檔案管理

在 Visual Studio 2002 和 2003年中，(.vbproj vb.net) 和 C# 的.csproj 專案檔會儲存在 Web 應用程式中的所有檔案資訊。 方案總管 中顯示為基礎的專案檔中的檔案資訊。 因為這個緣故，[方案總管] 會通常在其中外部編輯器所使用的情況下顯示不正確的資訊。 Visual Studio 2002 和 2003年會經常覆寫檔案的變更，或顯示檔案的最新版本。

Visual Studio 2005 就立即與專案檔。 相反地，它會讀取檔案和資料夾資訊直接從磁碟，導致您的專案中的檔案正確顯示。 因為 Visual Studio 2002 和 2003年中的 [參考] 資料夾不代表 Web 應用程式中的實際資料夾，Visual Studio 2005 也會移除 [參考] 資料夾從 [方案總管]。 若要存取 Visual Studio 2005 的專案中的參考，您應該使用專案的屬性頁。

## <a name="creating-web-projects"></a>建立 Web 專案

Web 開發人員在 Visual Studio 2005 中，有許多的新選項可用於建立專案。 Web sites 現在可以建立任何位置在檔案系統，然後進行偵錯或瀏覽 使用新的 ASP.NET 程式開發伺服器。 開發人員也可以建立新的網站使用 FTP。

按一下這裡可觀看視訊逐步解說在 Visual Studio 2005 中建立 Web 專案。


![](improvements-in-visual-studio-2005/_static/image1.png)


[開啟全螢幕影片](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>檔案系統的專案

如您所見的視訊逐步解說中，您可以選擇在本機電腦上或遠端位置透過檔案共用上的檔案系統上建立網站。 檔案系統所建立的網站的瀏覽和偵錯使用 ASP.NET 程式開發伺服器。

> [!NOTE]
> ASP.NET 程式開發伺服器可能會造成一些混淆的客戶。 如果 IISs 目錄結構 (亦即 c: / inetpub/wwwroot) 中的檔案系統上建立 Web 專案，則將仍瀏覽的網站透過從 Visual Studio 2005 內啟動時，才會進行 ASP.NET 程式開發伺服器。 因此，任何 IIS 設定 （也就是驗證方法） 不適用。


預設的 web 專案也會移除許多的額外負荷，藉由只包含 Default.aspx 頁面、 default.cs 檔案和應用程式/資料 （_d） 資料夾。 如有需要會加入 web.config 和特殊資料夾 （也就是應用程式/（_c））。 您的 web 專案只包含的檔案和您所需要的資料夾。

### <a name="http-projects"></a>HTTP 專案

HTTP 專案可以是本機 IIS 網站上或遠端網站上所建立的專案。 預設專案位置是`http://localhost`。 如果您按一下 [瀏覽] 按鈕時，有兩個 HTTP 選項： 本機 IIS 和遠端站台。 這兩個選項中的主要差異是在 選擇位置對話方塊和如何將檔案複製到 Web 伺服器，會顯示網站資訊的方法。

本機 IIS 選項會讀取在本機電腦上的 metabase 中的站台資訊，並使用檔案系統複製檔案。 遠端站台選項使用 FrontPage Server Extensions 和站台資訊會複製檔案，使用 HTTP 和 FrontPage Server Extensions RPC 呼叫。

> [!NOTE]
> Vs###/_tmp.htm 檔案，並在 get/_aspx/_ver.aspx 不再會用來判斷版本資訊中。


預設 HTTP 選項是 本機 IIS。 此選項會讀取 IIS Metabase 來判斷哪一個站台可用並在其中建立內容的位置。 您可以選取樹狀檢視中選取不同的資料夾或虛擬目錄。 您可以也建立新的虛擬目錄、 標示為應用程式的資料夾，以及從這個對話方塊中刪除現有的虛擬目錄。


![選擇位置對話方塊](improvements-in-visual-studio-2005/_static/image1.gif)

**圖 1**: 選擇位置對話方塊


不同於在舊版的 Visual Studio 中，如果您勾選**使用安全通訊端層**核取方塊和 SSL 憑證不符合您瀏覽的 URL，您會看到安全性警示對話方塊，詢問您，是否您想要繼續進行。 如果憑證不相符的一個，請使用 Visual Studio.NET 2003年，建立專案會失敗。


![安全性警示的相關 SSL 憑證](improvements-in-visual-studio-2005/_static/image2.gif)

**圖 2**: SSL 憑證有關的安全性警示


### <a name="note-on-host-headers"></a>請注意，在 主機標頭

如果您要繫結至特定的 IP 站台上建立 Web 應用程式，您必須確定已設定的主機標頭。 否則，Visual Studio 會建立在站台`http://localhost`，但網站瀏覽或從 IDE 內進行偵錯時無法正確解析 IP 位址。

如果您選取 [遠端站台] 選項時，對話方塊就會變成可讓您輸入新的網站的目的地 URL。 此 URL 必須是已啟用 FrontPage Server Extensions 的伺服器上。 如果您想要搭配您使用 FrontPage Server Extensions 的本機 Web 伺服器，您可以使用 [遠端站台] 選項，並指定本機 URL。


![在遠端伺服器上建立網站](improvements-in-visual-studio-2005/_static/image1.jpg)

**圖 3**： 遠端伺服器上建立網站


如果不符合 SSL 憑證，請在透過 SSL，遠端站台上建立應用程式，確認對話方塊時稍有不同於使用本機 IIS 選項時，顯示對話方塊。


![遠端站台的安全性警示](improvements-in-visual-studio-2005/_static/image3.gif)

**圖 4**： 遠端站台的安全性警示


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 將介紹建立透過 FTP 網站的選項。 當您使用此選項時，IDE 就會在本機建立的使用者暫存資料夾中的檔案，並接著使用 FTP 將檔案移到 FTP 位置。

> [!NOTE]
> 暫存資料夾位置是 c: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;


使用 FTP 選項時，系統會向您選擇位置對話方塊。 您輸入所需的 FTP 連線資訊，此對話方塊，如下所示。


![選擇 ftp 位置對話方塊](improvements-in-visual-studio-2005/_static/image2.jpg)

**圖 5**: 選擇 ftp 位置對話方塊


## <a name="lab-setup-ftp-site-and-create-a-project"></a>實驗室： 設定 FTP 站台，並建立專案

下列步驟可用來設定 FTP 站台，讓使用者具有只是它們可以透過 FTP 上傳至的位置。

### <a name="install-the-ftp-service"></a>安裝 FTP 服務

1. 開啟 新增或移除程式 中，選取 新增/移除 Windows 元件
2. 選取 Internet Information Services （Windows 2003 上的應用程式伺服器），然後按一下**詳細資料**。
3. 請檢查**檔案傳輸通訊協定 (FTP) 服務**然後按一下**確定**。
4. 按一下 [**下一步]** 安裝 FTP 服務。

### <a name="create-a-new-folder-for-content"></a>建立新的資料夾內容

1. 在 Windows 檔案總管中，會建立新資料夾，稱為**User1**內 c: / inetpub wwwroot。

#### <a name="configure-folders-and-permissions-on-folders"></a>在資料夾上設定資料夾和權限。

1. 從 系統管理工具，以開啟 Internet Information Services 嵌入式管理單元。 您現在會有電腦名稱節點下的 FTP 站台資料夾。
2. 依序展開**FTP 站台**。
3. 以滑鼠右鍵按一下**預設的 FTP 站台**，選取**新增**，然後**虛擬目錄**，然後按一下**下一步**。
4. 請輸入**User1**的虛擬目錄名稱，然後按一下**下一步**。
5. 請輸入**c: / inetpub/wwwroot/User1**路徑，然後按一下 [**下一步]**。
6. 按一下 **下一步**，然後**完成**以完成精靈。
7. 以滑鼠右鍵按一下**User1**在預設的 FTP 站台，然後選取的虛擬目錄**屬性**。
8. 檢查**撰寫**核取方塊，按一下 **確定**以關閉對話方塊。
9. 以滑鼠右鍵按一下**預設的 FTP 站台**，然後選取**屬性**。
10. 在 **安全性帳戶**索引標籤上，取消核取**允許匿名連線**。
11. 按一下 **是**詢問您是否要繼續  對話方塊中。
12. 按一下 **確定**以關閉對話方塊。
13. 依序展開**Default Web Site**下方**網站**節點。
14. 以滑鼠右鍵按一下**User1**目錄，然後選取**屬性**
15. 在 **應用程式設定**區段中，按一下**建立**將標示為應用程式的資料夾。
16. 按一下 **確定**以關閉對話方塊。
17. 關閉 Internet Information Services 嵌入式管理單元。

### <a name="create-web-project"></a>建立 web 專案

1. 開啟 Visual Studio 2005。
2. 從**檔案**功能表上，選取**新的網站**。
3. 在 **位置**下拉式清單中，選取**FTP**。
4. 按一下 [瀏覽] 。
5. 請輸入**localhost**中**Server**文字方塊中。
6. 請輸入**User1** [目錄] 文字方塊中。
7. 按一下 **開啟**。 FTP 位置將會進入新網站 對話方塊。
8. 按一下 [確定 **Deploying Office Solutions**]。
9. 取消核取**匿名登入**在 [FTP 登入] 對話方塊中，輸入您的認證，然後按一下**確定**。
10. 什麼是專案的 URL？ （專案的 URL 會顯示在 [方案總管]）。
11. 從**建置**功能表上，選取**建置網站**或是**建置方案**。
12. 以滑鼠右鍵按一下方案總管 中的 Default.aspx，然後選取**瀏覽器中的檢視**。
13. 在必須有網站 URL] 對話方塊中，輸入`http://localhost/user1`URL，然後按一下 [ **[確定]**。

> [!NOTE]
> 如果您收到錯誤，指出無法載入類型 /_Default，請確定您的網站並不是較早的版本上執行 ASP.NET 2.0。 您可以從 Internet Information Services 中的 [ASP.NET] 索引標籤來這麼做。


## <a name="opening-web-projects"></a>開啟 Web 專案

開啟 Web 專案也是類似於建立專案。 下列各節說明留意出針對在 IDE 中工作時的區域。 它也涵蓋了有關使用 HTTP 和 FTP Web 專案。

若要開啟 Web 專案，請從 [檔案] 功能表中選取開啟網站。 先前討論的相同選擇位置對話方塊會提示您，並且您有相同的四個選項可供您： 檔案系統、 本機 IIS、 FTP 和遠端站台。

<a id="_Toc116100245"></a>

## <a name="file-system"></a>檔案系統

如同先前在此模組，Visual Studio 不會再使用專案檔。 因此，如果您選擇從檔案系統開啟網站，您實際上需要選擇想要的話，任何資料夾，即使您選擇的資料夾不是作為一開始在 Visual Studio 中的 Web 專案。 比方說，您可以選擇開啟為網站的 [我的文件] 資料夾，Visual Studio 會很高興地操作加以開啟並顯示您的檔案，如下所示。


![我的網站以開啟的文件](improvements-in-visual-studio-2005/_static/image3.jpg)

**圖 6**:*我的文件*網站形式開啟


因為 Visual Studio 只會建立其他檔案和資料夾在必要時，沒有其他檔案或資料夾會新增至您所開啟的位置中。 此架構的副作用是，它會防止您在檔案系統上的巢狀的網站。 例如，請考慮下列目錄結構。

在 c: / MyWebSite web 專案

在 c: / MyWebSite/巢狀的另一個 web 專案

當您開啟的網站，在 c: / MyWebSite 時，巢狀資料夾會顯示為該應用程式的子資料夾中。

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

開啟時透過 HTTP 的網站，從 IIS metabase (IIS)，或使用 FrontPage Server Extensions （遠端站台。） 讀取設定如果有巢狀的 web 應用程式，這些會以其識別為應用程式的圖示也顯示。 如果您已熟悉使用 FrontPage 中的 web 應用程式，Visual Studio 2005 中的行為會類似。

即使 Visual Studio 會顯示在 IDE 中目前開啟的應用程式底下巢狀的應用程式的圖示，它將不允許您展開以查看其內容。 您可以不過，按兩下加以開啟。 當您這樣做時，您會看到對話方塊，提示您開啟的 web 應用程式 （並取代目前開啟的方案），或加入至目前方案的 Web 應用程式。


![按兩下巢狀的應用程式圖示會出現此對話方塊](improvements-in-visual-studio-2005/_static/image4.jpg)

**圖 7**： 按兩下巢狀的應用程式圖示會出現此對話方塊


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP 站台

當您開啟透過 FTP 站台時，檔案會全部在本機複製到您的暫存資料夾。 本機儲存體位置的完整路徑會顯示在專案 [屬性] 窗格，並使用下列格式建立。

C: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;

在使用 FTP 時，Visual Studio 必須指定專案的基底 URL，以便您可以瀏覽它，如下所示。 如果您未指定基底 URL，Visual Studio 會要求您提供此第一次您嘗試瀏覽的網站中的頁面。


![指定 FTP 站台的基底 URL](improvements-in-visual-studio-2005/_static/image5.jpg)

**圖 8**: FTP 站台的指定基底 URL


## <a name="improvements-in-compilation"></a>在編譯中的增強功能

使用 Visual Studio 2005 中的 Web 應用程式是比舊版顯然速度更快。 這是因為任何小型的組件中編譯架構中的變更。

在 Visual Studio 2002 和 2003年，Web 應用程式已編譯成一個 /bin 資料夾中的主要組件。 在 Visual Studio 2005 中，已新增的應用程式/（_c） 資料夾。 類別和其他非 UI 程式碼會新增至應用程式/（_c） 資料夾中。 當 Visual Studio 建置專案時，應用程式/（_c） 資料夾中的所有檔案會都編譯成單一的 App/_Code.dll 檔案。 這項變更的結果是速度也比在舊版中後續的組建。

> [!NOTE]
> MSBuild 命令列公用程式也可用來建置 ASP.NET Web 應用程式。 模組 9 中，將會說明該工具。


另一個編譯增強功能是新的組建 頁面選項，在 建置 功能表上。 這項功能可讓開發人員重建只有目前頁面 （連同，課程中，和相依性），因此可以更快速地編譯變更。 C# 未提供的更新 IntelliSense 等進行背景編譯，因為它們將受益於非常這項功能因為這樣可讓 intellisense 快速更新的只重建單一頁面。

專案組建屬性可讓您設定組建的執行啟動頁面之前，就會發生型別。 開發人員可以選擇只建立目前的頁面，讓 Visual Studio 可以更快速地偵錯應用程式，程式碼變更後啟動。


![組建頁面啟動動作](improvements-in-visual-studio-2005/_static/image6.jpg)

**圖 9**： 組建頁面啟動動作


Visual Studio 和 ASP.NET 架構另一個絕佳的增強功能是編輯的區域中，並繼續。 在 Visual Studio 2005 中，開發人員可以開始偵錯的專案，並在專案上進行程式碼變更，而不中斷連結偵錯工具。 事實上，您實際上可以啟動偵錯專案時，加入新的類別、 將程式碼新增至該類別、 將程式碼新增至您建立該類別的新執行個體的頁面和執行的類別，而不中斷連結偵錯工具的所有方法。 執行新的程式碼會解譯為常值，只要重新整理瀏覽器就行了 ！

按一下這裡以查看編輯的影片逐步解說，並繼續在 Visual Studio 2005 的功能。


![](improvements-in-visual-studio-2005/_static/image2.png)


[開啟全螢幕影片](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


強大的編輯後繼續 [功能]，請在 ASP.NET 2.0 和 Visual Studio 2005 是因為 ASP.NET 應用程式的架構變更。 在 ASP.NET 1.x 中，建立在 Visual Studio 2002/2003 中的應用程式編譯成主要組件儲存在 /bin 資料夾中。 所有類別，頁面等的應用程式編譯成該 DLL。 然後在執行階段，ASP.NET 會編譯所有的控制項、 標記和頁面內的 ASP.NET 程式碼，並將這些 Dll 複製到 ASP.NET 暫存資料夾。

使用 ASP.NET 2.0 中，在執行階段的兩個編譯模型說明上方 （適用於 Visual Studio 的其中一個），另一個用於 ASP.NET 的 Visual Studio 2005 中已合併至另一個常見的編譯模型。 這表示在執行階段現在在開發階段，而不是攔截所有編譯問題。 它也可讓設計工具和功能，例如使用者控制項和主版頁面的 IntelliSense 支援。

若要查看使用者控制項的設計工具支援的影片逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image3.png)


[開啟全螢幕影片](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> 從頁面上，移除使用者控制項時@Register指示詞會保留在標記，而且應該手動移除，以避免發生剖析器錯誤，如果從網站中刪除使用者控制項。


在 Visual Studio 編譯模型中的另一項改進是發行網站的功能。 [發佈] 功能會先行編譯網站，因為開發人員可以享有不必編譯任何項目依需求新增的效能。 它也會預先編譯應用程式/（_c） 資料夾中的所有原始程式碼的 dll，讓沒有原始程式碼，就必須部署。


![[發行網站] 對話方塊](improvements-in-visual-studio-2005/_static/image7.jpg)

**[圖 10**: 發行網站] 對話方塊


> [!NOTE]
> Aspnet/_compile.exe 公用程式也可用來預先編譯的 ASP.NET Web 應用程式。 模組 9 中，將會說明該工具。


當您發佈網站時，先行編譯的檔案會儲存在 Temporary ASP.NET Files 資料夾，如下所示。 檔案與 *.compiled*副檔名是定義特定 dll 的相依性的 XML 檔案。 編譯任何 Webform 或使用者控制項為開頭的隨機 Dll*應用程式 /_Web /_*。

如果您離開*讓這個先行編譯的站台成為可更新*選取核取方塊，Webforms 和使用者控制項內的標記將不會預先編譯成 DLL，讓您可以在部署之後進行變更。 如果您不希望鎖定的標記，以便不允許變更已部署的內容，請取消核取此方塊。

*使用固定命名和單一頁面的組件*核取方塊可讓您停用批次編譯，讓每個頁面會編譯成固定式名稱的組件。 保留未核取此方塊，可讓您利用批次編譯。

*啟用強式命名上先行編譯的組件*核取方塊可讓您將強式名稱您先行編譯的組件。

> [!NOTE]
> 在 ASP.NET 1.x 中，強式名稱組件必須安裝到全域組件快取 (GAC) 中。 在 ASP.NET 2.0 中，您不需要強式名稱組件安裝到 GAC。


![ASP.NET 應用程式預先編譯的檔案](improvements-in-visual-studio-2005/_static/image8.jpg)

**圖 11**: ASP.NET 應用程式預先編譯的檔案


> [!NOTE]
> 在上述的應用程式，發生任何 web.config 檔。 如果有，它會呼叫*PrecompiledApp.config*之後發行的 Web 網站處理程序。


## <a name="improvements-in-deployment"></a>部署的改進功能

為 Visual Studio 2002 與 2003 年 Visual Studio 2005 提供複製專案功能。 不過，此功能已經過增強 Visual Studio 2005 中，且現在稱為複製網站。

複製網站 對話方塊會分割成左框架內，右框架。 左框架內稱為來源網站和右邊框架會呼叫遠端網站。 有些開發人員可能會混淆的一點是，在右側框架中顯示的網站不一定是遠端站台。 這可能是本機檔案系統或 IIS 的本機執行個體上的網站。 此外，在左框架所顯示的網站也不一定與來源網站 對話方塊可讓您從遠端網站上發行*至*來源網站。

如果您將專案複製到遠端網站，該網站必須在其上安裝的 FrontPage Server Extensions。 如果沒有，您必須使用 FTP 連接。 相反地，如果您將專案複製到本機 IIS 執行個體，FrontPage Server Extensions 並不需要。

> [!NOTE]
> 如果您嘗試在本機 IIS 執行個體上建立新的網站和 FrontPage 2002 Server Extensions 安裝，您會取得錯誤訊息，告知您的網站不支援建立 SharePoint 伺服器上。 在此情況下，您可以選擇 FrontPage 2000 Server Extensions 安裝或移除 FrontPage Server Extensions。


如 「 複製網站 」 功能的影片逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image4.png)


[開啟全螢幕影片](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>在偵錯改善

有四個主要的改善項目在 Visual Studio 2005 中偵錯。

- 根據預設，可能會以非系統管理員在本機偵錯。
- Compilation 項目的偵錯屬性現在預設為 false。
- 遠端偵錯設定和組態是比以前更容易。
- 您現在可以偵錯透過 FTP 位置開啟網站。

## <a name="debugging-as-a-non-administrator"></a>非系統管理員身分偵錯

ASP.NET Development Server 新增允許非系統管理員輕鬆地偵錯豎立劃時代的 ASP.NET 應用程式。 當本機檔案系統上執行的 ASP.NET 應用程式進行偵錯時，Visual Studio 會啟動 ASP.NET 程式開發伺服器，登入的使用者內容下。 該使用者接著可以偵錯該應用程式，而不需要任何額外的設定。

## <a name="debug-is-false-by-default"></a>偵錯的 False 預設

在 ASP.NET 1.x*偵錯*屬性中*編譯*web.config 檔案的項目已設定為 *，則為 true*預設。 已建議一律，開發人員會將此屬性設定為*false*之前應用程式部署到生產環境中，但是因為大多數的開發人員不完全瞭解的離開偵錯屬性設定為結果為 true，就只是保持為-是。

最嚴重的問題有偵錯屬性設為 true 時，它會停用 ASP.NETs 批次編譯模型。 因此，每個頁面會編譯成不同的 DLL。 如果 Web 應用程式包含數千個分頁 （不前所未聞的任何方法），這表示該應用程式會建立數個數千個的小型 Dll。 這些 Dll 大小較小的它們並不是載入記憶體中的任何特定位置。 因此，它們會造成系統記憶體的分散程度，而且 OutOfMemoryException 相符項目可以參與。

在 ASP.NET 2.0 中，偵錯屬性依預設將設定為 false。 因為您已經看過，當開發人員偵錯 ASP.NET 應用程式在 Visual Studio 2005 中，系統會提示他們啟用偵錯中加入的 web.config 檔案。 這樣會產生原本在 ASP.NET 中的相同缺點 1.x，但現在開發人員會清楚地警告屬性應該重設為 false，再移至生產環境應用程式。

## <a name="remote-debugging-setup-and-configuration"></a>遠端偵錯設定和組態

在 Visual Studio 2002/2003，遠端偵錯依賴機器偵錯管理員 (mdm.exe) 和 vs7jit.exe 程序。 因此，疑難排解遠端偵錯的問題通常是黑色方塊的客戶，和通常不是更美好的 PSS。

Visual Studio 2005 中移除的依賴的 mdm.exe 和 vs7jit.exe 程序。 相反地，它現在會使用遠端偵錯監視服務 (msvsmon.exe)。

遠端偵錯 Visual Studio 2005 中的需求都非常簡單。 您需要之前先偵錯在遠端伺服器上執行 msvsmon.exe。 您可以從 Visual Studio 光碟片來安裝遠端偵錯監視，或您只要執行 msvsmon.exe 從共用而不需要完全在 Web 伺服器上安裝任何項目。

當您執行 msvsmon.exe 時，很可能會抱怨連接埠遭到封鎖遠端偵錯。 幸運的是，您可以輕鬆地解除封鎖的連接埠從右側 [警告] 對話方塊中，如下所示。


![Windows 防火牆會封鎖遠端偵錯的通知](improvements-in-visual-studio-2005/_static/image9.jpg)

**圖 12**： 通知 Windows 防火牆會封鎖遠端偵錯


一旦您已解除封鎖的偵錯所需的連接埠，您會看到 遠端偵錯監視，如下所示。 從這個介面中，您可以監視連線，並變更輕鬆地偵錯權限。


![遠端偵錯監視](improvements-in-visual-studio-2005/_static/image10.jpg)

**圖 13**： 遠端偵錯監視


它也可進行遠端偵錯 Web 應用程式開啟透過 FTP。 步驟都與先前涵蓋相同。 不過，您必須指定基底 URL 來瀏覽 FTP 專案稍早在本單元中所述。

## <a name="lab-2"></a>實驗室 2

## <a name="remote-debugging-with-visual-studio-2005"></a>使用 Visual Studio 2005 的遠端偵錯

這個實驗室將引導您完成使用 Visual Studio 2005 的遠端偵錯。

如本實驗室中的影片逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image5.png)


[開啟全螢幕影片](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


這個實驗室中必須要有一個執行的 Visual Studio 2005 與其他執行 IIS 5 或更高的兩部機器。

1. 開啟 Visual Studio 2005，並在遠端伺服器上建立新的網站。

> [!NOTE]
> 在遠端的 IIS 執行個體，或透過 FTP，您可以建立網站。


1. 從遠端的 Web 伺服器中，找出 msvsmon.exe 使用 UNC 路徑的開發電腦上並執行它。  
 Msvsmon.exe 的預設位置是 //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/遠端偵錯工具/x86。
2. 如果系統提示您解除封鎖通訊埠進行遠端偵錯時，這麼做。
3. 從開發電腦中，開啟程式碼後置 Default.aspx，和頁面/（_l） 方法中設定中斷點。
4. 開始偵錯從開發電腦。

如預期般運作，您應該叫用中斷點。

## <a name="aspnet-development-server"></a>ASP.NET 程式開發伺服器

將已經討論過，為 Visual Studio 2005 隨附稱為 ASP.NET 程式開發伺服器的 Web 伺服器。 （ASP.NET Development Server 有時稱為 Cassini。）此網頁伺服器是一個方便的方法，來瀏覽和偵錯的檔案系統上執行的 Web 應用程式。

ASP.NET 程式開發伺服器是受限制的 Web 伺服器。 它不會允許遠端連接，它不允許從啟動 Web 伺服器的使用者以外的任何使用者的任何要求。 它也沒有提供 ASP 頁面的功能。 只有 ASP.NET 資源和 HTML 資源 （包括影像、 CSS 檔案等） 會提供服務。

ASP.NET Development Server 可以透過命令列啟動執行 WebDev.WebServer.exe 檔案位於 c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. 下列對話方塊會顯示可用的參數。


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**圖 14**


> [!NOTE]
> 透過命令列明確啟動時，不支援 ASP.NET 程式開發伺服器。
