---
uid: whitepapers/aspnet-and-iis6
title: 執行 IIS 6.0 與 ASP.NET 1.1 |Microsoft 文件
author: rick-anderson
description: 雖然 Windows Server 2003 包括 IIS 6.0 與 ASP.NET 1.1，預設會停用這些元件。 本白皮書說明如何啟用 IIS 6.0...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530107"
---
<a name="running-aspnet-11-with-iis-60"></a>執行 IIS 6.0 與 ASP.NET 1.1
====================
> 雖然 Windows Server 2003 包括 IIS 6.0 與 ASP.NET 1.1，預設會停用這些元件。 本白皮書說明如何啟用 IIS 6.0 與 ASP.NET 1.1，並建議數個組態設定從 IIS 和 ASP.NET 中取得最佳效能。
> 
> 適用於 ASP.NET 1.1 版和 IIS 6.0。


ASP.NET 1.1 隨附於 Windows Server 2003，也會包含最新版的 Internet Information Server (IIS) 6.0 版的版本。 IIS 6.0 與 ASP.NET 1.1 專為緊密整合，ASP.NET 現在預設為新的 IIS 6.0 工作者處理序模型。

## <a name="aspnet-11-is-not-installed-by-default"></a>根據預設不會安裝 ASP.NET 1.1

不同於舊版的 Microsoft 的伺服器作業系統，Internet Information Server (IIS) 是預設不啟用。也不是 ASP.NET 1.1。 有兩個選項可啟用 IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>啟用 IIS，選項 #1-設定您的伺服器精靈

Windows Server 2003 隨附新 ' 設定您的伺服器精靈 可協助您正確設定您的伺服器中的所需的模式。

若要啟動精靈-請注意，若要執行精靈，您必須以系統管理員的身分登入，請移至： 啟動 |程式 |系統管理工具] 並選取 [設定您的伺服器 '。

您應該會看到 ' 設定您的伺服器精靈 開啟螢幕一次選取：

![](aspnet-and-iis6/_static/image1.jpg)

按一下 下一步&gt;':

![](aspnet-and-iis6/_static/image2.jpg)

按一下 下一步&gt;'

![](aspnet-and-iis6/_static/image3.jpg)

在這個畫面上，您將需要選取 ' 的應用程式伺服器 （IIS，ASP.NET） 來設定選項。

按一下 下一步&gt;'。

![](aspnet-and-iis6/_static/image4.jpg)

選取後將伺服器設定為應用程式伺服器，這個畫面將會顯示提示應該安裝哪些其他功能。 預設會選取這兩個選項。 若要自動啟用 ASP.NET，您必須選取 ' 啟用 ASP。NET'。

按一下 下一步&gt;'。

![](aspnet-and-iis6/_static/image5.jpg)

此畫面顯示選項會安裝。

按一下 下一步&gt;'。

![](aspnet-and-iis6/_static/image6.jpg)

正在安裝您選取的選項時，您會看到這個畫面。 這是正常看到其他對話方塊會出現方塊，因為正在安裝服務。 您可能此外會提示您在 Windows 2003 Server 安裝光碟的位置。

按一下 下一步&gt;' 時完成。

![](aspnet-and-iis6/_static/image7.jpg)

按一下 [完成]-Windows Server 2003 現在已設定為支援 IIS 6.0 與 ASP.NET 1.1。

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>啟用 IIS，選項 #2-手動設定 IIS 和 ASP.NET

如果您不想使用 ' 設定您的伺服器精靈 可以選擇性地從控制台安裝 IIS 6.0 與 ASP.NET 1.1，使用 新增或移除程式。

第一次開啟 控制台：

![](aspnet-and-iis6/_static/image8.jpg)

接下來，按一下 新增/移除 Windows 元件 ' 它將會開啟 「 Windows 元件精靈 」:

![](aspnet-and-iis6/_static/image9.jpg)

反白顯示及檢查 「 應用程式伺服器 」，然後按一下 [詳細資料]？ 按鈕：

![](aspnet-and-iis6/_static/image10.jpg)

若要安裝 ASP.NET，請檢查 ' ASP。NET'。

按一下 [確定] 以返回 [Windows 元件精靈]。 按一下 下一步&gt;' 從 Windows 元件精靈，開始安裝：

![](aspnet-and-iis6/_static/image11.jpg)

這是正常看到其他對話方塊會出現方塊，因為正在安裝服務。 您可能此外會提示您在 Windows 2003 Server 安裝光碟的位置。

安裝完成時，您會看到 Windows 元件精靈的最後一個畫面：

![](aspnet-and-iis6/_static/image12.jpg)

現在設定 IIS 6.0 與 ASP.NET 1.1 及可用。

## <a name="recommended-settings"></a>建議的設定

IIS 6.0 中執行 ASP.NET 1.1 時有數個建議使用它們以取得最佳效能，從 ASP.NET 的組態設定：

- 設定背景工作處理序記憶體限制
- 設定背景工作處理序回收

### <a name="configuring-worker-process-memory-limits"></a>設定背景工作處理序記憶體限制

預設 IIS 6.0 不會設定限制的允許 IIS 使用的記憶體數量。 ASP。網路的快取功能會依賴記憶體的限制，所以從記憶體中快取可以主動移除未使用的項目。

建議您設定的記憶體回收的 IIS 6.0 的功能。 若要設定這個開啟的 Internet Information Services 管理員 (啟動 |程式 |系統管理工具 |網際網路資訊服務）。 一旦開啟，請展開 '應用程式集區' 資料夾：

每個應用程式集區：

![](aspnet-and-iis6/_static/image13.jpg)

1. 以滑鼠右鍵按一下應用程式集區，例如：'DefaultAppPool'，並選取 [內容]:

![](aspnet-and-iis6/_static/image14.jpg)

2. 接下來，讓記憶體回收，請按一下 ' 使用的記憶體上限 （以 mb 為單位）:'。 值不能超過伺服器上的實體 （不是虛擬的） 的記憶體數量，很好的近似值是 60%的實體記憶體，也就是具有 512 MB 的實體記憶體選取 310 的伺服器。 也建議使用 2 GB 的位址空間時，最多不超過 800 MB。 如果伺服器的記憶體位址空間為 3 GB，可以是 1，800 MB 最高工作者處理序的最大記憶體限制：

![](aspnet-and-iis6/_static/image15.jpg)

按一下 [套用] 和 [確定] 以結束 [屬性] 對話方塊。 所有可用的應用程式集區重複此步驟。

### <a name="configuring-worker-recycling"></a>設定回收背景工作

預設會設定 IIS 6.0，以回收其工作者處理序每 29 小時。 這是一個位元積極執行 ASP.NET 應用程式，並建議，停用自動背景工作處理序回收。

若要停用自動背景工作處理序回收，請先開啟 Internet Information Services 管理員 (啟動 |程式 |系統管理工具 |網際網路資訊服務）。 一旦開啟，請展開 '應用程式集區' 資料夾：

![](aspnet-and-iis6/_static/image16.jpg)

每個應用程式集區：

1. 以滑鼠右鍵按一下應用程式集區，例如：'DefaultAppPool'，並選取 [內容]:

![](aspnet-and-iis6/_static/image17.jpg)

2. 取消選取 ' 回收工作者處理序 （以分鐘為單位）:':

![](aspnet-and-iis6/_static/image18.jpg)

按一下 [套用] 和 [確定] 以結束 [屬性] 對話方塊。 所有可用的應用程式集區重複此步驟。

## <a name="granting-write-access-to-the-file-system"></a>授與檔案系統的寫入權限

如果您的應用程式需要在檔案系統的寫入權限，而且您正在使用 NTFS 您必須修改存取控制清單 (ACL) 上的資料夾或檔案授與 ASP.NET 存取權。

比方說，若要授與 ASP.NET c:\inetpub\wwwroot 的寫入權限第一次開啟檔案總管並瀏覽至目錄：

![](aspnet-and-iis6/_static/image19.jpg)

接下來，以滑鼠右鍵按一下目錄，例如 'wwwroot' 並選取 [屬性]。 [屬性] 對話方塊開啟之後，選取 [安全性] 索引標籤：

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ 目錄是特殊的目錄中的特殊 IIS 6.0 群組 ' IIS\_WPG' 已經授與讀取&amp;執行、 列出資料夾內容、 及讀取權限。 不過，若要授與寫入權限，您需要寫入中按一下 [允許] 核取方塊：

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 現在會有此資料夾的寫入權限。 若要授與寫入權限，在其他資料夾，請遵循下列步驟-請注意，您可能需要加入 IIS\_WPG 群組，如果不存在。

> [!CAUTION]
> 授與寫入權限 IIS\_WPG 可讓任何可寫入這個目錄的 ASP.NET 應用程式。

## <a name="supporting-integrated-authentication-with-sql-server"></a>支援與 SQL Server 的整合式的驗證

整合式的驗證可讓 SQL Server，以運用 Windows NT 驗證以驗證 SQL Server 登入帳戶。 這可讓使用者略過標準的 SQL Server 登入程序。 使用此方法時，網路使用者可以存取 SQL Server 資料庫，而不需要提供不同的登入識別碼或密碼，因為 SQL Server 中取得使用者和密碼資訊從 Windows NT 網路安全性的程序。

選擇 ASP.NET 應用程式的整合式的驗證是不錯的選擇，因為沒有認證永遠會儲存在您的應用程式的連接字串。 而是用來連接到 SQL 連接字串將如下所示：

`"server=localhost; database=Northwind;Trusted_Connection=true"`

這個連接字串會告知 SQL Server 使用嘗試存取 SQL Server 的應用程式的 Windows 認證。 在 ASP.NET/IIS 6 的情況下，這會是在 IIS 中的帳戶\_WPG 群組。

若要啟用 SQL Server 與 ASP.NET 之間的整合式的驗證，您必須先確認 SQL Server 針對整合式的驗證或混合模式驗證-設定與判斷 DBA 檢查。 如果 SQL Server 是在這兩種模式的其中一個，您可以使用整合式的驗證。

開啟 SQL Server Enterprise Manager (開始 |程式 |Microsoft SQL Server |Enterprise Manager)，選取適當的伺服器，並展開 [安全性] 資料夾：

![](aspnet-and-iis6/_static/image22.jpg)

如果 ' BUILTINT\IIS\_WPG' 未列出群組，以滑鼠右鍵按一下 登入並選取 新的登入:

![](aspnet-and-iis6/_static/image23.jpg)

在 '名稱:'，或者輸入的文字方塊中' [伺服器/網域名稱] \IIS\_WPG' 或按一下省略符號按鈕，以開啟 Windows NT 使用者/群組選擇器：

![](aspnet-and-iis6/_static/image24.jpg)

選取 在目前電腦的 IIS\_WPG 群組，然後按一下 新增 和 確定 以關閉選擇器。

然後，您需要也是預設的資料庫以及存取資料庫的權限設定。 若要設定的預設資料庫選擇從下拉式清單中，如下列 Northwind 選取：

![](aspnet-and-iis6/_static/image25.jpg)

接下來，按一下 [資料庫存取權] 索引標籤：

![](aspnet-and-iis6/_static/image26.jpg)

按一下您想要允許存取的每個資料庫的允許核取方塊。 您也必須選擇資料庫角色檢查 db\_擁有者可確保您的登入具有所有必要權限管理，並使用選取的資料庫。

按一下 [確定] 結束 [屬性] 對話方塊。 ASP.NET 應用程式現在已設定為支援整合式的 SQL Server 驗證。

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>不在 IIS 6.0 原生模式下執行 ASP.NET 1.0

IIS 5 相容性模式中才支援 ASP.NET 1.0 IIS 6.0 上。

若要設定 IIS 5.0 相容性模式中執行 ASP.NET 1.0，開啟網際網路服務管理員，以滑鼠右鍵按一下 [網站] 並選取 [屬性]:

![](aspnet-and-iis6/_static/image27.jpg)

切換至 [服務] 索引標籤，然後檢查嗎？在 IIS 5.0 隔離模式中執行 WWW 服務？

![](aspnet-and-iis6/_static/image28.jpg)
