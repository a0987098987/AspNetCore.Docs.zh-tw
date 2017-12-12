---
uid: whitepapers/denied-access-to-iis-directories
title: "ASP.NET 拒絕存取 IIS 目錄 |Microsoft 文件"
author: rick-anderson
description: "本白皮書描述所需的動作如果 ASP.NET 應用程式的要求會傳回錯誤，也就是 「 拒絕存取 DirectoryName 目錄。 無法為 s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET 拒絕 IIS 目錄的存取
====================
> 本白皮書說明若您的 ASP.NET 應用程式的要求會傳回錯誤，您須執行的作業 「 拒絕存取*DirectoryName*目錄。 無法啟動監視目錄 chaanges。 」
> 
> 適用於 ASP.NET 1.0 和 1.1 的 ASP.NET。


ASP.NET V1 RTM 現在執行時使用較小權限的 windows 帳戶-註冊為本機電腦上的"ASPNET 」 帳戶。

在某些系統已鎖定，此帳戶可能並非預設讀取的網站內容目錄、 應用程式的根目錄，或網站根目錄的安全性存取權。 在此情況下您會在從給定的 web 應用程式要求頁面時，收到下列錯誤：

![](denied-access-to-iis-directories/_static/image1.jpg)

若要修正此問題，您必須變更適當的目錄的安全性權限。

具體而言，ASP.NET 需要讀取、 執行和清單的網站根目錄的 ASPNET 帳戶存取權 (例如： c:\inetpub\wwwroot 或可能會在 IIS 中設定任何其他站台目錄)，內容的目錄和應用程式根目錄若要監視的組態檔變更。 應用程式根目錄對應至 IIS 系統管理工具 (inetmgr) 中的應用程式虛擬目錄相關聯的資料夾路徑。

例如，請考慮下列應用程式階層下的 wwwroot 資料夾。

`C:\inetpub\wwwroot\myapp\default.aspx`

例如，ASPNET 帳戶需要以上定義的 myapp 和 wwwroot 目錄內容的讀取權限。 根資料夾上的單一繼承的 ACL 也會選擇性地用於兩個目錄如果它們巢狀。

若要加入目錄的權限，執行下列步驟：

- 使用 Windows 檔案總管，瀏覽至目錄
- 目錄資料夾上按一下滑鼠右鍵，然後選擇 [屬性]
- 瀏覽至 [屬性] 對話方塊的 [安全性] 索引標籤
- 按一下 [新增] 按鈕並輸入電腦名稱，後面接著 ASPNET 帳戶名稱。 比方說，在電腦上名為"webdev 」，您會輸入 webdev\ASPNET，然後點擊 [確定]。
- 請確認 ASPNET 帳戶具有 「 讀取&amp;Execute"，「 列出資料夾內容 」 和 「 讀取 」 核取的核取方塊。
- 點擊 [確定] 以關閉對話方塊並儲存變更。

![](denied-access-to-iis-directories/_static/image2.jpg)

如有需要，可以使用指令碼或隨附的 「 cacls.exe 」 工具與 Windows 自動化這些變更。 如需有關 ASPNET 帳戶的詳細資訊，請參閱[常見問題集文件](https://go.microsoft.com/fwlink/?LinkId=5828)。

如果給定的 web 應用程式依賴具有寫入或修改特定資料夾或檔案的權限，這可以授與相同的程序，並檢查 「 寫入 」 和/或 「 修改 」 核取方塊。

在電腦上，可以讓每個人或使用者群組讀取權限，這些目錄 （這是預設組態），會發生任何問題，並不需要上述步驟。
