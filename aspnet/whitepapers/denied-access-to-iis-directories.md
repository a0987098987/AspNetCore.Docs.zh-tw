---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET 拒絕存取 IIS 目錄 |Microsoft Docs
author: rick-anderson
description: 本白皮書會說明如果您 ASP.NET 應用程式要求會傳回錯誤，也就是 「 拒絕存取 DirectoryName 目錄必須做事情。 無法為 s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830583"
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET 拒絕存取 IIS 目錄
====================
> 本白皮書會說明如果您的 ASP.NET 應用程式的要求會傳回錯誤，必須做的事情 「 存取被拒*DirectoryName*目錄。 無法啟動監視目錄變更 」。
> 
> 適用於 ASP.NET 1.0 及 ASP.NET 1.1。


ASP.NET V1 RTM 現已使用無特殊權限的 windows 帳戶-註冊為 「 ASPNET 」 帳戶，在本機電腦上。

一些鎖定系統，此帳戶可能並非預設讀取的網站內容目錄、 應用程式根目錄的目錄或網站根目錄的安全性存取權。 在此情況下您會在從給定的 web 應用程式要求頁面時，收到下列錯誤：

![](denied-access-to-iis-directories/_static/image1.jpg)

若要修正此問題，您必須變更適當的目錄中的安全性權限。

具體來說，ASP.NET 要求讀取、 執行和列出網站根目錄的 ASPNET 帳戶的存取權 (例如： c:\inetpub\wwwroot 或任何您可能已在 IIS 中設定的替代網站目錄)，content 目錄及應用程式根目錄若要監視組態檔變更。 應用程式根目錄對應至 IIS 系統管理工具 (inetmgr) 中的應用程式虛擬目錄相關聯的資料夾路徑。

例如，請考慮下列應用程式階層的 wwwroot 資料夾下方。

`C:\inetpub\wwwroot\myapp\default.aspx`

針對此範例中，將 ASPNET 帳戶需要以上定義的 myapp 和的 wwwroot 目錄內容的讀取權限。 在根資料夾的單一繼承的 ACL 也會選擇性地用於這兩個目錄如果它們巢狀。

若要新增至目錄的權限，執行下列步驟：

- 使用 Windows 檔案總管，瀏覽至目錄
- 以滑鼠右鍵按一下 [目錄] 資料夾，然後選擇 [屬性]
- 瀏覽至 [安全性] 索引標籤，在 [屬性] 對話方塊
- 按一下 [新增] 按鈕並輸入電腦名稱，後面接著 ASPNET 帳戶名稱。 例如，在電腦上名為"webdev 」，您會輸入 webdev\ASPNET，並點擊 [確定]。
- 確定 ASPNET 帳戶具有 「 讀取&amp;Execute 」、 「 列出資料夾內容 」 和 「 讀取 」 核取方塊已核取。
- 按 [確定] 以關閉對話方塊，然後儲存變更。

![](denied-access-to-iis-directories/_static/image2.jpg)

如有需要，可以使用 Windows 中的指令碼或所附的 「 cacls.exe"工具自動化這些變更。 如需有關將 ASPNET 帳戶的詳細資訊，請參閱[常見問題集文件](https://go.microsoft.com/fwlink/?LinkId=5828)。

如果指定的 web 應用程式依存於寫入，或修改特定資料夾或檔案的權限，這可以授與相同的程序，並檢查 「 寫入 」 及/或 「 修改 」 的核取方塊。

在機器上，可以讓每個人或使用者群組讀取權限，這些目錄 （這是預設組態），您會遇到任何問題，並不需要上述的步驟。
