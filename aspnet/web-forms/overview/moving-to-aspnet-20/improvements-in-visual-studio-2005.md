---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: 在 Visual Studio 2005 中的增強功能 |Microsoft 文件
author: microsoft
description: Visual Studio 2005 提供 Web 應用程式開發人員一長串的增強功能和增強功能，Web 專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2018
ms.locfileid: "28987986"
---
<a name="improvements-in-visual-studio-2005"></a>在 Visual Studio 2005 中的增強功能
====================
by [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 提供 Web 應用程式開發人員一長串的增強功能和增強功能，Web 專案。


Visual Studio 2005 提供 Web 應用程式開發人員一長串的增強功能和增強功能，Web 專案。 強大的 Visual Studio.NET 2002年和 2003年是，許多抱怨中沒有已處理的 Web 專案的方式。 Visual Studio 2005 定址這些抱怨將大量的新功能。 對於喜歡 Visual Studio.NET 2003 中處理的 Web 應用程式編譯方式，請參閱[Web 應用程式專案](https://go.microsoft.com/fwlink/?LinkId=57870)。

這個模組中也涵蓋在 Web 專案建立、 管理和開發的增強功能。 在更新版本的模組中，也涵蓋建置 Web 專案，以及將其部署中的增強功能。

## <a name="frontpage-server-extensions"></a>FrontPage Server Extensions

Visual Studio.NET 2002年和 2003年所需的方塊上才能建立或建立 Web 專案的 FrontPage Server Extensions。 開發人員有兩種不同的存取模式 （FrontPage Server Extensions 或檔案存取模式） 之間的選擇，同時用來執行工作，例如 IIS 和其他內容中設定應用程式根目錄的 FrontPage Server Extensions。

Visual Studio 2005 中移除對本機專案的 FrontPage Server Extensions 依賴。 Visual Studio 2005 現在會存取 IIS metabase 直接而不是使用 FrontPage Server Extensions。 Visual Studio 2005 也會加入 FTP 會允許遠端專案存取而不需要 FrontPage Server Extensions 的支援。

這些的開發人員想要在其專案中使用 FrontPage Server Extensions，此選項仍可使用。 不過，根據從 ASP.NET 開發人員社群的強式回饋，它不需要。

> [!NOTE]
> 仍然需要遠端專案建立、 開啟等 FrontPage Server Extensions。


## <a name="aspnet-development-server"></a>ASP.NET 程式開發伺服器

Visual Studio 2005 隨附新的 Web 伺服器呼叫 ASP.NET 程式開發伺服器。 （此 Web 伺服器先前稱為 Cassini）。

有數個優點的 ASP.NET 程式開發伺服器。

- 它現在可能會非系統管理員來開發及偵錯 Web 伺服器。
- ASP.NET 程式開發伺服器動態會對應至任何位置虛擬目錄以彈性的專案位置的檔案系統中。
- 在 Windows XP Professional 上已經在使用 IIS 的使用者現在可以建立新的 Web 應用程式將不會影響的其預設的網站在 IIS 中的檔案或資料夾結構。

沒有特殊的設定，才能利用 ASP.NET 程式開發伺服器。 當偵錯或瀏覽檔案系統裝載的 Web 專案時，Visual Studio 2005 將自動服務此要求的隨機連接埠上啟動 ASP.NET 程式開發伺服器執行個體。

稍後在此模組中的 ASP.NET 程式開發伺服器上，將涵蓋的詳細資訊。

## <a name="improved-file-management"></a>改善的檔案管理

在 Visual Studio 2002 和 2003年中，專案檔 (如 VB.NET.vbproj) 和 C#.csproj 儲存 Web 應用程式中的所有檔案的資訊。 方案總管 中顯示為基礎的專案檔中的檔案資訊。 因為這個緣故，[方案總管] 會經常外部編輯器所使用的情況下顯示不正確的資訊。 Visual Studio 2002 和 2003年會通常覆寫檔案的變更，或顯示檔案的最新版本。

Visual Studio 2005 會立即執行專案檔。 相反地，它會直接從磁碟，導致您的專案中的檔案正確顯示讀取檔案和資料夾資訊。 因為 Visual Studio 2002 和 2003年中的 [參考] 資料夾不代表 Web 應用程式中的實際資料夾，Visual Studio 2005 也會移除 [參考] 資料夾從 [方案總管]。 若要存取 Visual Studio 2005 中的專案參考，您應該使用屬性頁專案。

## <a name="creating-web-projects"></a>建立 Web 專案

Web 開發人員有許多的新選項可用於建立專案在 Visual Studio 2005。 Web sites 現在可以建立任意位置在檔案系統，然後進行偵錯或瀏覽 使用新的 ASP.NET 程式開發伺服器。 開發人員也可以建立新的網站使用 FTP。

若要檢視 Visual Studio 2005 中建立 Web 專案的視訊逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image1.png)


[開啟全螢幕視訊](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>檔案系統的專案

如您所見視訊逐步解說中，您可以選擇在本機電腦上或遠端位置透過檔案共用上，在檔案系統上建立網站。 檔案系統所建立的網站的瀏覽和偵錯使用 ASP.NET 程式開發伺服器。

> [!NOTE]
> ASP.NET 程式開發伺服器可能會導致一些混淆的客戶。 如果 IISs 目錄結構 (亦即 c: / inetpub/wwwroot) 中的檔案系統上建立 Web 專案，將仍瀏覽的網站透過從 Visual Studio 2005 內啟動時，才會進行 ASP.NET 程式開發伺服器。 因此，任何 IIS 設定 （也就是驗證方法） 不適用。


預設的 web 專案也會移除許多由的額外負荷僅包含 Default.aspx 頁面、 default.cs 檔案和應用程式/（_d） 資料夾。 有需要，會加入 web.config 和特殊資料夾 （也就是應用程式/（_c））。 您的 web 專案只會包含的檔案和您需要的資料夾。

### <a name="http-projects"></a>HTTP 專案

HTTP 專案可以是本機 IIS 網站上或遠端網站上所建立的專案。 預設專案位置`http://localhost`。 如果您按一下瀏覽按鈕時，有兩個 HTTP options： 本機 IIS 和遠端站台。 這兩個選項中的主要差異是和中選擇位置對話方塊如何將檔案複製到 Web 伺服器，會顯示網站資訊的方法。

本機 IIS 選項讀取站台資訊從本機電腦上 metabase，並使用檔案系統複製檔案。 遠端站台選項使用 FrontPage Server Extensions 和站台資訊檔案會複製使用 HTTP 和 FrontPage Server 延伸 RPC 呼叫。

> [!NOTE]
> 無法再使用 vs###/_tmp.htm 檔和 get/_aspx/_ver.aspx 來判斷版本資訊。


預設 HTTP 選項是本機 IIS。 此選項會讀取 IIS Metabase，判斷哪些站台可用並建立內容所在的位置。 您可以選取樹狀檢視中選取不同的資料夾或虛擬目錄。 您可以也建立新的虛擬目錄、 標記資料夾做為應用程式，以及從這個對話方塊中刪除現有的虛擬目錄。


![選擇位置對話方塊](improvements-in-visual-studio-2005/_static/image1.gif)

**圖 1**: 選擇位置對話方塊


不同於在舊版的 Visual Studio 中，如果您核取**使用安全通訊端層**核取方塊，SSL 憑證不符合您在瀏覽的 URL，您會看到安全性警告對話方塊，以詢問是否會要繼續進行。 請使用 Visual Studio.NET 2003 中，如果沒有任何憑證比對一個，建立專案會失敗。


![安全性警示有關 SSL 憑證](improvements-in-visual-studio-2005/_static/image2.gif)

**圖 2**： 有關 SSL 憑證的安全性警示


### <a name="note-on-host-headers"></a>請注意，在 主機標頭

如果您要在繫結至特定 IP 的網站上建立 Web 應用程式，您必須確定已設定的主機標頭。 否則，Visual Studio 會建立站台`http://localhost`，但 IP 位址將不會解析正確地瀏覽或在 IDE 中偵錯從站台時。

如果您選取的遠端站台選項時，對話方塊就會變成可讓您輸入新網站的目的地 URL。 此 URL 必須是已啟用 FrontPage Server Extensions 的伺服器上。 如果您想要搭配使用 FrontPage Server Extensions 您本機 Web 伺服器，您可以使用遠端站台選項，並指定本機 URL。


![遠端伺服器上建立網站](improvements-in-visual-studio-2005/_static/image1.jpg)

**圖 3**： 遠端伺服器上建立網站


如果不符合 SSL 憑證，請建立應用程式透過 SSL，遠端站台上，確認對話方塊時稍有不同於使用本機 IIS 選項時，顯示對話方塊。


![遠端站台安全性警示](improvements-in-visual-studio-2005/_static/image3.gif)

**圖 4**： 遠端站台安全性警示


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 導入了建立透過 FTP 網站的選項。 當您使用此選項時，IDE 會在本機使用者的暫存資料夾中建立的檔案，並再使用 FTP 將檔案移到 FTP 位置。

> [!NOTE]
> 暫存資料夾位置是 c: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;


使用 FTP 選項時，您會看到選擇位置對話方塊。 您輸入此對話方塊，如下所示的所需的 FTP 連線資訊。


![選擇 ftp 位置對話方塊](improvements-in-visual-studio-2005/_static/image2.jpg)

**圖 5**: 選擇 ftp 位置對話方塊


## <a name="lab-setup-ftp-site-and-create-a-project"></a>實驗室： 設定 FTP 站台，並建立專案

下列步驟設定 FTP 站台，讓使用者有它們只可以透過 FTP 上傳至的位置。

### <a name="install-the-ftp-service"></a>安裝 FTP 服務

1. 開啟 新增或移除程式，選取 新增/移除 Windows 元件
2. 選取 Internet Information Services （Windows 2003 上的應用程式伺服器），然後按一下**詳細資料**。
3. 請檢查**檔案傳輸通訊協定 (FTP) 服務**按一下**確定**。
4. 按一下**下一步**安裝 FTP 服務。

### <a name="create-a-new-folder-for-content"></a>建立新的資料夾內容

1. 在 Windows 檔案總管，建立新資料夾，稱為**User1** c: / inetpub/wwwroot 內。

#### <a name="configure-folders-and-permissions-on-folders"></a>在資料夾上設定資料夾和權限。

1. 從系統管理工具，以開啟 [網際網路資訊服務] 嵌入式管理單元。 您現在就必須在 [電腦名稱] 節點下的 FTP 站台資料夾。
2. 展開**FTP 站台**。
3. 以滑鼠右鍵按一下**預設 FTP 站台**，選取**新增**，然後**虛擬目錄**，然後按一下 **下一步**。
4. 輸入**User1**虛擬目錄名稱，然後按一下**下一步**。
5. 輸入**c: / inetpub/wwwroot/User1**路徑並按一下**下一步**。
6. 按一下**下一步**然後**完成**以完成精靈。
7. 以滑鼠右鍵按一下**User1**下預設 FTP 網站，並選取的虛擬目錄**屬性**。
8. 檢查**寫入**核取方塊，按一下 **確定**以關閉對話方塊。
9. 以滑鼠右鍵按一下**預設 FTP 站台**選取**屬性**。
10. 在**安全性帳戶**索引標籤上，取消核取**允許匿名連線**。
11. 按一下**是**在詢問您是否要繼續 對話方塊中。
12. 按一下**確定**以關閉對話方塊。
13. 展開**Default Web Site**下**網站**節點。
14. 以滑鼠右鍵按一下**User1**目錄中，而且選取**屬性**
15. 在**應用程式設定**區段中，按一下**建立**將標示為應用程式的資料夾。
16. 按一下**確定**以關閉對話方塊。
17. 關閉 [網際網路資訊服務] 嵌入式管理單元。

### <a name="create-web-project"></a>建立 web 專案

1. 開啟 Visual Studio 2005。
2. 從**檔案**功能表上，選取**新網站**。
3. 在**位置**下拉式清單中，選取**FTP**。
4. 按一下 [瀏覽] 。
5. 輸入**localhost**中**伺服器**文字方塊。
6. 輸入**User1**目錄 文字方塊中。
7. 按一下**開啟**。 FTP 位置將會輸入到新的網站 對話方塊。
8. 按一下 [確定 **Deploying Office Solutions**]。
9. 取消核取**匿名登入**在 FTP 登入] 對話方塊中，輸入您的認證，然後按一下 [**確定**。
10. 什麼是專案的 URL？ （專案的 URL 將會顯示在 [方案總管]）。
11. 從**建置**功能表上，選取**建置網站**或**建置方案**。
12. 以滑鼠右鍵按一下方案總管 中的 Default.aspx，然後選取**瀏覽器中的檢視**。
13. 在必須有網站 URL 對話方塊中，輸入`http://localhost/user1`URL 並按一下**確定**。

> [!NOTE]
> 如果您收到錯誤指出無法載入型別 /_Default，請確定您的網站和不較早的版本上執行 ASP.NET 2.0。 您可以從 Internet Information Services 中的 [ASP.NET] 索引標籤執行。


## <a name="opening-web-projects"></a>開啟 Web 專案

開啟 Web 專案是類似於建立專案。 下列各節說明留意出針對在 IDE 中的工作時的區域。 其中也涵蓋搭配使用 HTTP 和 FTP 的 Web 專案的工作。

若要開啟 Web 專案，請從 [檔案] 功能表中選取開啟網站。 與先前涵蓋相同選擇位置對話方塊會提示您，並且您有相同的四個選項可供您： 檔案系統、 本機 IIS、 FTP 和遠端站台。

<a id="_Toc116100245"></a>

## <a name="file-system"></a>檔案系統

如同先前在此模組中，Visual Studio 無法再使用專案檔。 因此，如果您選擇從檔案系統開啟網站，您實際上具有選擇您想，任何資料夾，即使您選擇的資料夾不建立為 Web 專案一開始在 Visual Studio 中的選項。 例如，您可以選擇開啟為網站的 [我的文件] 資料夾，Visual Studio 將愉快地保存他們開啟它並顯示您的檔案，如下所示。


![我的網站以開啟的文件](improvements-in-visual-studio-2005/_static/image3.jpg)

**圖 6**:*我的文件*開啟為網站


因為 Visual Studio 只會建立其他檔案和資料夾在必要時，任何其他檔案或資料夾會加入至您所開啟的位置。 此架構的副作用是，它會防止您從巢狀方式置於檔案系統上的網站。 例如，請考慮下列目錄結構。

在 c: / MyWebSite web 專案

在 c: / MyWebSite/巢狀的另一個 web 專案

當您開啟位於 c: / MyWebSite 網站時，巢狀資料夾會顯示為該應用程式的子資料夾。

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

開啟時透過 HTTP 的網站，從 IIS metabase (本機 IIS) 或使用 FrontPage Server Extensions （遠端站台。） 讀取設定如果沒有巢狀的 web 應用程式，這些會以其識別為應用程式的圖示也顯示。 如果您很熟悉使用 frontpage web 應用程式，Visual Studio 2005 中的行為是類似。

即使 Visual Studio 會顯示一個圖示巢狀下方時，目前會在 IDE 中開啟應用程式的應用程式，它將不允許您展開以檢視其內容。 您可以不過，連按兩下加以開啟。 當您這樣做時，您會看到一個對話方塊，提示您開啟 web 應用程式 （並取代目前開啟的方案），或新增至目前方案的 Web 應用程式。


![巢狀的應用程式圖示上按兩下顯示的這個對話方塊](improvements-in-visual-studio-2005/_static/image4.jpg)

**圖 7**： 巢狀的應用程式圖示上按兩下顯示的這個對話方塊


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP 站台

當您開啟透過 FTP 站台時，檔案會所有在本機複製到暫存資料夾。 本機儲存體位置的完整路徑會顯示專案的 [屬性] 窗格中，而且會使用下列格式建立。

C: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;

當使用 FTP，Visual Studio 必須指定專案的基底 URL，使您可以瀏覽它如下所示。 如果您沒有指定基底 URL，Visual Studio 會要求您為其第一次您嘗試瀏覽的網站中的頁面。


![指定 FTP 站台的基底 URL](improvements-in-visual-studio-2005/_static/image5.jpg)

**圖 8**: FTP 站台的指定基底 URL


## <a name="improvements-in-compilation"></a>在編譯中的增強功能

使用 Visual Studio 2005 中的 Web 應用程式的速度明顯比之前的版本。 這是因為沒有小部分編譯架構中的變更。

在 Visual Studio 2002 和 2003年，Web 應用程式編譯成一個主要組件位於 /bin 資料夾中。 在 Visual Studio 2005 中已加入應用程式/（_c） 資料夾。 類別和其他非 UI 程式碼會加入至應用程式/（_c） 資料夾中。 當 Visual Studio 建置專案時，應用程式/（_c） 資料夾中的所有檔案會都編譯成單一的 App/_Code.dll 檔案。 這項變更的結果是後續建置會比舊版更快。

> [!NOTE]
> MSBuild 命令列公用程式也可用來建置 ASP.NET Web 應用程式。 該工具將涵蓋單元 9。


另一編譯增強功能是在 建置 功能表上新的組建 頁面選項。 這項功能可讓開發人員重建只有目前頁面 （連同，過程，和相依性），讓變更可以更快速地編譯。 C# 不提供背景編譯為了更新 IntelliSense 等，因為它們會受益於喜歡這項功能因為這樣可讓 intellisense 快速更新只重建單一頁面。

專案建置屬性可讓您設定的執行啟動頁面之前發生的組建類型。 開發人員可以選擇只建置目前的頁面，讓 Visual Studio 可以更快速地偵錯應用程式，程式碼變更之後開始。


![組建頁面啟動動作](improvements-in-visual-studio-2005/_static/image6.jpg)

**圖 9**： 組建頁面啟動動作


到 Visual Studio 和 ASP.NET 架構的另一個絕佳增強功能是在編輯的區域，並繼續。 在 Visual Studio 2005 中，開發人員就可以開始偵錯專案和專案進行程式碼變更，而不偵錯工具中斷連結。 事實上，您可以依其字面開始偵錯專案時，加入新的類別、 將程式碼加入至該類別、 將程式碼加入至您建立該類別的新執行個體的頁面和執行的類別，而不需要卸離偵錯工具的方法。 執行新的程式碼是依其字面，只要重新整理瀏覽器 ！

按一下這裡查看視訊逐步解說的編輯，並繼續在 Visual Studio 2005 的功能。


![](improvements-in-visual-studio-2005/_static/image2.png)


[開啟全螢幕視訊](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


健全的編輯後繼續在 ASP.NET 2.0 的功能，Visual Studio 2005 是由於 ASP.NET 應用程式發生架構變更。 在 ASP.NET 中 1.x，在 Visual Studio 2002/2003 中建立的應用程式編譯成 /bin 資料夾中儲存的主要組件。 所有的類別，頁面等應用程式編譯成該 DLL。 然後在執行階段，ASP.NET 會編譯的所有控制項、 標記和 ASP.NET 頁面內的程式碼，並將這些 Dll 複製到 ASP.NET 暫存資料夾。

使用 ASP.NET 2.0 中，兩個編譯模型概述上方 （一個 Visual Studio），另一個用於 ASP.NET 在執行階段的 Visual Studio 2005 中合併成一個一般編譯模型。 這表示在執行階段現在而不是在開發階段攔截所有的編譯問題。 它也可讓設計工具和功能，例如使用者控制項和主版頁面的 IntelliSense 支援。

若要查看使用者控制項的設計工具支援的視訊逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image3.png)


[開啟全螢幕視訊](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> 從頁面中，移除使用者控制項時@Register指示詞會保留在標記，而且應該手動移除，以避免發生剖析器錯誤，如果網站已刪除的使用者控制項。


在 Visual Studio 編譯模型中的另一個改善的幅度發行網站功能。 發行功能會先行編譯網站，因為開發人員可以享有不必編譯隨選的任何項目新增的效能。 它也會先行編譯的應用程式/（_c） 資料夾中所有原始程式碼成 DLL，讓沒有原始程式碼已部署。


![[發行網站] 對話方塊](improvements-in-visual-studio-2005/_static/image7.jpg)

**圖 10**： 發行網站 對話方塊


> [!NOTE]
> Aspnet/_compile.exe 公用程式也可用來預先編譯的 ASP.NET Web 應用程式。 該工具將涵蓋單元 9。


當您發行網站，先行編譯的檔案儲存在暫存 ASP.NET 檔案資料夾如下所示。 具有檔案 *.compiled*檔案延伸模組會定義特定 dll 的相依性的 XML 檔案。 任何 Webform 或使用者控制項編譯成開頭的隨機 Dll*應用程式 /_Web /_*。

如果您離開*讓這個先行編譯的網站成為可更新*勾選核取方塊，內部 Webforms 和使用者控制項的標記不會先行編譯成 DLL，可讓您在部署後進行變更。 如果您想要鎖定的標記，以便對已部署的內容不允許，取消核取此方塊。

*使用固定命名和單一網頁組件*核取方塊可讓您停用批次編譯，讓每一頁會編譯固定命名的組件。 保持未核取此方塊可讓您充分利用批次編譯。

*啟用強式命名的先行編譯的組件*核取方塊可讓您將強式名稱您先行編譯的組件。

> [!NOTE]
> 在 ASP.NET 中 1.x，強式名稱組件必須安裝到全域組件快取 (GAC) 中。 在 ASP.NET 2.0 中，您不需要強式名稱組件安裝到 GAC。


![ASP.NET 應用程式的先行編譯的檔案](improvements-in-visual-studio-2005/_static/image8.jpg)

**圖 11**: ASP.NET 應用程式的先行編譯的檔案


> [!NOTE]
> 在上述的應用程式，沒有 web.config 檔。 如果有，它會呼叫*PrecompiledApp.config*之後發行的 Web 網站處理程序。


## <a name="improvements-in-deployment"></a>在部署中的增強功能

做為 Visual Studio 2002 和 2003 中，Visual Studio 2005 提供複製專案功能。 不過，此功能已經過增強 Visual Studio 2005 中，且現在稱為複製網站。

複製網站對話方塊分割成左框架內，右框架。 左框架內呼叫來源網站和右框架稱為遠端網站。 可能會混淆有些開發人員的一件事是，右框架中顯示的網站不一定與遠端站台。 這可能是在本機檔案系統或 IIS 的本機執行個體上的站台。 此外，在左框架所顯示的網站不一定與來源網站對話方塊可讓您從遠端網站上發行*至*來源網站。

如果您將專案複製到遠端網站，該網站必須在其上安裝的 FrontPage Server Extensions。 如果不存在，您必須使用 FTP 連接。 相反地，如果您將專案複製到本機 IIS 執行個體，FrontPage Server Extensions 並不需要。

> [!NOTE]
> 如果您嘗試在本機 IIS 執行個體上建立新的網站，且 FrontPage 2002 Server Extensions 已安裝，您會取得錯誤訊息，告知您，建立網站不支援在 SharePoint 伺服器上。 在此情況下，您可以選擇安裝 FrontPage 2000 伺服器擴充功能或移除 FrontPage Server Extensions。


複製網站功能的視訊逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image4.png)


[開啟全螢幕視訊](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>在偵錯的增強功能

沒有偵錯在 Visual Studio 2005 中的四個重要改良。

- 根據預設可能會以非系統管理員在本機偵錯。
- Compilation 項目的偵錯屬性現在預設為 false。
- 遠端偵錯設定和組態是比以前更容易。
- 您現在可以偵錯透過 FTP 位置開啟網站。

## <a name="debugging-as-a-non-administrator"></a>非系統管理員身分偵錯

新增 ASP.NET 程式開發伺服器可讓非系統管理員輕鬆地偵錯現成的 ASP.NET 應用程式。 當本機檔案系統上執行的 ASP.NET 應用程式進行偵錯時，Visual Studio 會啟動 ASP.NET 程式開發伺服器，登入使用者的身分。 該使用者可以再偵錯該應用程式，而不需要任何額外的設定。

## <a name="debug-is-false-by-default"></a>偵錯的 False 預設

在 ASP.NET 中 1.x*偵錯*屬性*編譯*web.config 檔案的項目已設定為*true*預設。 它一律已建議開發人員，將此屬性設*false*之前應用程式部署到生產環境中，但因為大部分的開發人員不完全了解的偵錯屬性設定為結果為 true，而只是中斷它當做-是。

最嚴重的問題與具有偵錯屬性設為 true 時，它會停用 ASP.NETs 批次編譯模型。 因此，每一頁會編譯到個別的 DLL。 如果 Web 應用程式包含的千分位 （未當然的透過任何方式） 的頁面，這表示可幾千個小的數個 Dll 將會建立由該應用程式。 雖然這些 Dll 較小的大小，則不載入記憶體中的任何特定位置。 因此，它們會造成系統記憶體中的片段，以及提供意見給 OutOfMemoryException 項目。

在 ASP.NET 2.0 中，偵錯屬性依預設將設定為 false。 當您已經看，開發人員時進行偵錯 ASP.NET 應用程式在 Visual Studio 2005 中，系統會提示他們加入已啟用偵錯的 web.config 檔案。 這樣做會出現在 ASP.NET 中的相同缺點 1.x，但現在開發人員清楚警告屬性應該重設為 false，再移至生產環境應用程式。

## <a name="remote-debugging-setup-and-configuration"></a>遠端偵錯設定和組態

在 Visual Studio 2002/2003 中，遠端偵錯依賴機器偵錯 Manager (mdm.exe) 及 vs7jit.exe 程序。 因此，疑難排解遠端偵錯問題通常是客戶黑色方塊，它通常不是比較好的產品支援服務。

Visual Studio 2005 中移除的依賴 mdm.exe 和 vs7jit.exe 程序。 相反地，它現在會使用遠端偵錯監視服務 (msvsmon.exe)。

遠端偵錯在 Visual Studio 2005 中的需求是相當簡單。 您需要在之前偵錯在遠端伺服器上執行 msvsmon.exe。 您可以從 Visual Studio 光碟片安裝遠端偵錯監視，或只要執行 msvsmon.exe 從共用而不需要完全在 Web 伺服器上安裝任何項目。

當您執行 msvsmon.exe 時，很可能會投訴封鎖遠端偵錯的連接埠。 幸運的是，您可以輕鬆地解除封鎖從 [警告] 對話方塊中的右連接埠如下所示。


![Windows 防火牆會封鎖遠端偵錯的通知](improvements-in-visual-studio-2005/_static/image9.jpg)

**圖 12**： 通知 Windows 防火牆會封鎖遠端偵錯


一旦您已解除封鎖偵錯所需的連接埠，您會看到遠端的偵錯監視，如下所示。 從這個介面，您可以監視連線，並變更輕鬆地偵錯權限。


![遠端偵錯監視](improvements-in-visual-studio-2005/_static/image10.jpg)

**圖 13**： 遠端偵錯監視


它也可進行遠端偵錯 Web 應用程式開啟透過 FTP。 會以先前未包含相同的步驟。 不過，您必須指定基底 URL 的瀏覽 FTP 專案稍早在此模組中所述。

## <a name="lab-2"></a>Lab 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Visual Studio 2005 的遠端偵錯

這個實驗室將引導您使用 Visual Studio 2005 的遠端偵錯。

如本實驗室的視訊逐步解說，請按一下這裡。


![](improvements-in-visual-studio-2005/_static/image5.png)


[開啟全螢幕視訊](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


這個實驗室中必須要有一個執行的 Visual Studio 2005 和其他執行 IIS 5 或更高的兩部電腦。

1. 開啟 Visual Studio 2005，並在遠端伺服器上建立新的網站。

> [!NOTE]
> 遠端的 IIS 執行個體上或透過 FTP，您可以建立網站。


1. 從遠端的 Web 伺服器，將 msvsmon.exe 在開發機器上使用 UNC 路徑，並加以執行。  
 Msvsmon.exe 的預設位置是 //server/c$/Program 檔案/Microsoft Visual Studio 8/Common7/IDE/遠端偵錯工具/x86。
2. 如果系統提示您解除封鎖通訊埠進行遠端偵錯時，這樣做。
3. 從開發電腦中，開啟程式碼後置 Default.aspx，然後頁面/（_l） 方法中設定中斷點。
4. 開始偵錯從開發電腦。

您應該如預期般，叫用中斷點。

## <a name="aspnet-development-server"></a>ASP.NET 程式開發伺服器

為 weve 已經討論過，Visual Studio 2005 隨附呼叫 ASP.NET 程式開發伺服器的 Web 伺服器。 （ASP.NET 程式開發伺服器有時稱為 Cassini。）此 Web 伺服器是方便瀏覽和偵錯 Web 應用程式在檔案系統上執行。

ASP.NET 程式開發伺服器是受限制的 Web 伺服器。 不允許遠端連接，所以不允許啟動 Web 伺服器的使用者以外的任何使用者從任何要求。 它也沒有為 ASP 網頁的功能。 只有 ASP.NET 資源和 HTML 資源 （包括影像、 CSS 檔案等） 都有受到處理。

可以透過命令列啟動 ASP.NET 程式開發伺服器，執行 WebDev.WebServer.exe 檔案位於 c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. 下列對話方塊會顯示可用的參數。


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**圖 14**


> [!NOTE]
> ASP.NET 程式開發伺服器不支援透過命令列明確啟動時。
