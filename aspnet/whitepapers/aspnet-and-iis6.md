---
uid: whitepapers/aspnet-and-iis6
title: 執行 ASP.NET 1.1 與 IIS 6.0 |Microsoft Docs
author: rick-anderson
description: 雖然 Windows Server 2003 包含 IIS 6.0 與 ASP.NET 1.1 中，預設會停用這些元件。 本白皮書說明如何啟用 IIS 6.0...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 338059104f46a6c5517212db6e1a54c12677e133
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839290"
---
<a name="running-aspnet-11-with-iis-60"></a>執行 ASP.NET 1.1 與 IIS 6.0
====================
> 雖然 Windows Server 2003 包含 IIS 6.0 與 ASP.NET 1.1 中，預設會停用這些元件。 本白皮書說明如何啟用 IIS 6.0 與 ASP.NET 1.1 中，並建議從 IIS 和 ASP.NET 取得最佳效能的數個組態設定。
> 
> 適用於 ASP.NET 1.1 與 IIS 6.0。


ASP.NET 1.1 隨附於 Windows Server 2003，也會包含最新的 Internet Information Server (IIS) 6.0 版的版本。 IIS 6.0 與 ASP.NET 1.1 專為緊密整合，現在，ASP.NET 會預設為新的 IIS 6.0 背景工作處理序模型。

## <a name="aspnet-11-is-not-installed-by-default"></a>依預設未安裝 ASP.NET 1.1

不同於舊版的 Microsoft 伺服器作業系統，Internet Information Server (IIS) 不會啟用預設值;也不是 ASP.NET 1.1。 有兩個選項可啟用 IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>啟用 IIS，選項 #1-設定您的伺服器精靈

Windows Server 2003 隨附的新 「 設定您伺服器精靈 」 可協助您正確設定伺服器，所需的模式。

若要啟動精靈-請注意，若要執行精靈，您必須以系統管理員的身分登入，請移至︰ 啟動 |程式 |系統管理工具和選取 ' 設定您的伺服器 '。

一次選取的是您應該會看到 「 設定您伺服器精靈 」 開啟的畫面：

![](aspnet-and-iis6/_static/image1.jpg)

按一下 [下一步] &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

按一下 [下一步] &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

在此畫面上，您將需要選取 ' 的應用程式伺服器 （IIS、 ASP.NET） 設定選項。

按一下 [下一步] &gt;'。

![](aspnet-and-iis6/_static/image4.jpg)

選取之後若要將伺服器設定為應用程式伺服器，此畫面會顯示提示應該安裝哪些額外的功能。 預設會選取這兩個選項。 若要自動啟用 ASP.NET，您必須選取 ' 啟用 ASP。NET'。

按一下 [下一步] &gt;'。

![](aspnet-and-iis6/_static/image5.jpg)

此畫面會顯示已安裝的選項。

按一下 [下一步] &gt;'。

![](aspnet-and-iis6/_static/image6.jpg)

正在安裝您所選取的選項時，您會看到這個畫面。 這是正常看到方塊出現，因為服務正在安裝其他對話方塊。 您可能此外會提示您在 Windows 2003 Server 安裝光碟的位置。

按一下 [下一步] &gt;' 完成時。

![](aspnet-and-iis6/_static/image7.jpg)

按一下 [完成]-Windows Server 2003 現在已設定為支援 IIS 6.0 與 ASP.NET 1.1。

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>啟用 IIS，選項 #2-手動設定 IIS 和 ASP.NET

如果您不想要使用 「 設定您伺服器精靈 」 可以選擇性地從控制台安裝 IIS 6.0 與 ASP.NET 1.1 中使用 [新增或移除程式]。

第一次開啟 控制台：

![](aspnet-and-iis6/_static/image8.jpg)

接下來，按一下 新增/移除 Windows 元件 ' 以開啟 「 Windows 元件精靈 」:

![](aspnet-and-iis6/_static/image9.jpg)

反白顯示並檢查 「 應用程式伺服器 」，然後按一下 [詳細資料]？ 按鈕：

![](aspnet-and-iis6/_static/image10.jpg)

若要安裝 ASP.NET，請檢查 ' ASP。NET'。

按一下 [確定] 以返回 [Windows 元件精靈]。 按一下 [下一步] &gt;' 開始安裝 [Windows 元件精靈] 從：

![](aspnet-and-iis6/_static/image11.jpg)

這是正常看到方塊出現，因為服務正在安裝其他對話方塊。 您可能此外會提示您在 Windows 2003 Server 安裝光碟的位置。

安裝完成時，您會看到 [Windows 元件精靈] 的最後一個畫面：

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 和 ASP.NET 1.1 版現在已設定且可用。

## <a name="recommended-settings"></a>建議的設定

執行 ASP.NET 1.1 與 IIS 6.0 時有數個建議使用它們以獲得最佳效能，從 ASP.NET 的組態設定：

- 設定背景工作角色處理序記憶體限制
- 設定背景工作處理序回收

### <a name="configuring-worker-process-memory-limits"></a>設定背景工作角色處理序記憶體限制

IIS 6.0 預設不會設定允許 IIS 使用的記憶體數量限制。 ASP。NET 的快取功能會依賴記憶體的限制，因此從記憶體中快取可以主動移除未使用的項目。

建議您設定記憶體回收的 IIS 6.0 的功能。 若要設定這個開啟的 Internet Information Services 管理員 (啟動 |程式 |系統管理工具 |Internet Information Services)。 一旦開啟，請展開 ' 應用程式集區 資料夾：

每個應用程式集區：

![](aspnet-and-iis6/_static/image13.jpg)

1. 以滑鼠右鍵按一下應用程式集區例如'DefaultAppPool'，並選取 [屬性]:

![](aspnet-and-iis6/_static/image14.jpg)

2. 接下來，啟用 記憶體回收，請按一下 ' 使用的記憶體上限 （以 mb 為單位）:'。 值不能超過伺服器上的實體 （不是虛擬的） 的記憶體數量的適當近似值是 60%的實體記憶體，也就是具有 512 MB 的實體記憶體選取 310 的伺服器。 也建議使用 2 GB 的位址空間時，最多不超過 800 MB。 如果伺服器的記憶體位址空間會是 3 GB，背景工作處理序的最大記憶體限制最多可為 1，800 MB:

![](aspnet-and-iis6/_static/image15.jpg)

按一下 [套用] 和 [確定] 以結束 [屬性] 對話方塊。 所有可用的應用程式集區重複此步驟。

### <a name="configuring-worker-recycling"></a>設定背景工作角色回收

預設會每隔 29 小時回收其背景工作處理序設定 IIS 6.0。 這是更積極執行 ASP.NET 的應用程式，建議您，停用自動的背景工作處理序回收。

若要停用自動的背景工作處理序回收，請先開啟 Internet Information Services 管理員 (啟動 |程式 |系統管理工具 |Internet Information Services)。 一旦開啟，請展開 ' 應用程式集區 資料夾：

![](aspnet-and-iis6/_static/image16.jpg)

每個應用程式集區：

1. 以滑鼠右鍵按一下應用程式集區例如'DefaultAppPool'，並選取 [屬性]:

![](aspnet-and-iis6/_static/image17.jpg)

2. 取消選取 ' 回收工作者處理序 （以分鐘為單位）:':

![](aspnet-and-iis6/_static/image18.jpg)

按一下 [套用] 和 [確定] 以結束 [屬性] 對話方塊。 所有可用的應用程式集區重複此步驟。

## <a name="granting-write-access-to-the-file-system"></a>在檔案系統的寫入存取權授與

如果您的應用程式需要寫入檔案系統權限，且您使用 NTFS 您必須修改存取控制清單 (ACL) 的資料夾或檔案上授與 ASP.NET 的存取權。

比方說，若要授與 ASP.NET c:\inetpub\wwwroot 的寫入權限第一次開啟檔案總管並瀏覽至目錄：

![](aspnet-and-iis6/_static/image19.jpg)

接下來，以滑鼠右鍵按一下目錄，例如 'wwwroot' 並選取 [屬性]。 [屬性] 對話方塊開啟之後，請選取 [安全性] 索引標籤：

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ 目錄是特殊的目錄中，特殊的 IIS 6.0 群組 ' IIS\_WPG' 已經授與讀取&amp;執行]、 [列出資料夾內容] 和 [讀取權限。 不過，若要授與寫入權限，您需要針對寫入，按一下 [允許] 核取方塊：

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 現在會有此資料夾的寫入權限。 若要授與寫入權限，在其他資料夾，請遵循下列步驟-請注意，您可能需要新增 IIS\_WPG 群組不存在時。

> [!CAUTION]
> 授與寫入權限至 IIS\_WPG 可讓任何可寫入此目錄的 ASP.NET 應用程式。

## <a name="supporting-integrated-authentication-with-sql-server"></a>支援使用 SQL Server 的整合式的驗證

整合式的驗證來運用 Windows NT 驗證的 SQL Server 驗證 SQL Server 登入帳戶。 這可讓使用者略過標準的 SQL Server 登入程序。 使用此方法時，網路使用者可以存取 SQL Server 資料庫，而不需要提供不同的登入識別碼或密碼，因為 SQL Server 會從 Windows NT 網路的安全性程序取得的使用者和密碼資訊。

選擇 ASP.NET 應用程式的整合式的驗證是不錯的選擇，因為無認證曾經儲存在您的應用程式的連接字串。 而是用來連接到 SQL 連接字串將如下所示：

`"server=localhost; database=Northwind;Trusted_Connection=true"`

此連接字串會告訴 SQL Server 將嘗試存取 SQL Server 的應用程式的 Windows 認證。 在 ASP.NET/IIS 6 的情況下，這會是在 IIS 中的帳戶\_WPG 群組。

若要啟用 SQL Server 與 ASP.NET 之間的整合式的驗證，您必須先確認 SQL Server 已設定整合式的驗證或混合模式驗證-請洽詢您的 DBA，若要判斷這個。 如果這兩種模式之一，是 SQL Server，您可以使用整合式的驗證。

開啟 SQL Server Enterprise Manager (開始 |程式 |Microsoft SQL Server |Enterprise Manager)，選取適當的伺服器，並展開 [安全性] 資料夾：

![](aspnet-and-iis6/_static/image22.jpg)

如果 ' BUILTINT\IIS\_WPG' 未列出群組，以滑鼠右鍵按一下 登入並選取 新的登入:

![](aspnet-and-iis6/_static/image23.jpg)

在 '名稱:' 文字方塊中輸入' [伺服器/網域名稱] \IIS\_WPG' 或按一下省略符號按鈕，以開啟 Windows NT 使用者/群組選擇器：

![](aspnet-and-iis6/_static/image24.jpg)

選取 在目前電腦的 IIS\_WPG 群組，然後按一下 新增 和 確定 以關閉選擇器。

接著，您必須也設定 預設的資料庫和資料庫的存取權限。 若要設定的預設資料庫選擇從下拉式清單，例如以下的 Northwind 已選取：

![](aspnet-and-iis6/_static/image25.jpg)

接下來，按一下 [資料庫存取權] 索引標籤：

![](aspnet-and-iis6/_static/image26.jpg)

按一下您想要允許存取的每個資料庫的允許核取方塊。 您也必須選擇資料庫角色中，檢查 db\_擁有者可確保您的登入具有所有必要權限管理，並使用選取的資料庫。

按一下 [確定] 以結束 [屬性] 對話方塊。 您的 ASP.NET 應用程式現在已設定為支援整合式的 SQL Server 驗證。

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>不在 IIS 6.0 原生模式中執行 ASP.NET 1.0

在 IIS 6.0 上的 ASP.NET 1.0 僅支援 IIS 5 相容性模式中。

若要設定 IIS 5.0 相容性模式中執行的 ASP.NET 1.0，請開啟網際網路服務管理員和以滑鼠右鍵按一下 網站並選取 屬性:

![](aspnet-and-iis6/_static/image27.jpg)

切換至 [服務] 索引標籤，然後檢查嗎？在 IIS 5.0 隔離模式中執行 WWW 服務？:

![](aspnet-and-iis6/_static/image28.jpg)
