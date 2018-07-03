---
uid: whitepapers/ms03-32-issue
title: 套用 IE 的安全性更新之後修正 「 無法使用伺服器應用程式 」 錯誤 |Microsoft Docs
author: rick-anderson
description: 本文件說明 MS03 32 安全性更新與修正的問題會影響 Wi-fi 上執行的 ASP.NET 1.0 應用程式的 Internet explorer 的修補程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 186ed7ea7ad9b317506fb3951a974682b44b27a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402466"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>套用 IE 的安全性更新之後修正 「 無法使用伺服器應用程式 」 錯誤
====================
> 本文件說明 MS03 32 安全性更新與修正的問題會影響執行 Windows XP Professional 上的 ASP.NET 1.0 應用程式的 Internet explorer 的修補程式。
> 
> 適用於 ASP.NET 1.0 和 Windows XP Professional。


Microsoft Internet Explorer 的安全性修補程式 MS03-32 的安全性更新與在 Windows XP 上執行的 ASP.NET 1.0 識別發生問題。 可以安裝此修補程式，以手動方式或從 Windows Update 網站取得最新的重大更新。

此問題的徵兆是，Windows XP 電腦上安裝修補程式之後, 在本機的 IIS 5.1 web 伺服器上執行的 ASP.NET 應用程式的所有要求會都導致錯誤訊息，指出 「 伺服器應用程式無法使用 」。 遠端 web 伺服器的要求，則不會受到影響。

此問題只會影響在 Windows XP 上執行 ASP.NET 1.0 中的安裝。 它不會影響執行 Windows 2000 或 Windows Server 2003 的電腦。 它也不會影響安裝的 ASP.NET 1.1 中執行 Windows XP 的電腦。

請注意，本期**不是**使用 ASP.NET 的安全性問題。 它**則否**開啟，或允許針對 ASP.NET 應用程式或伺服器的任何惡意攻擊。 相反地，它是純粹功能所造成的 bug 的修補程式本身。

我們正致力在本期的永久解決方案。 在此同時，您可以執行下列批次檔，為問題的因應措施。 批次檔會執行下列動作：

1. 停止 IIS 和 ASP.NET 狀態服務
2. 刪除並重新建立已知的暫時密碼的 ASPNET 帳戶
3. 使用 Windows`runas`命令，以啟動建立 ASPNET 使用者設定檔的可執行檔
4. 重新登錄 ASP.NET。 這會建立新的隨機密碼的帳戶，並套用預設 ASP.NET 存取控制設定，
5. 重新啟動 IIS 服務

批次檔包含硬式編碼的暫時密碼的 「<strong>1pass@word</strong>」 將會提示您輸入 runas 命令批次檔執行時。 Runas 命令完成後，ASPNET 帳戶密碼會重新建立強式隨機值。 請注意，如果硬式編碼密碼不符合密碼複雜性需求，您的環境中，批次檔可能會失敗。 如果是這樣，您可以為適用於您環境的另一個值來進行變更。

*> [!IMPORTANT]* 如果您已新增自訂存取控制設定] 或 [ASPNET 帳戶的資料庫帳戶權限，他們必須完成這個批次檔之後，重新建立。 這是因為當帳戶重新建立，它會取得新的安全性識別碼 (SID)。

*> [!IMPORTANT]* 如果您執行 ASP.NET 工作者處理序 ASPNET 帳戶以外的自訂帳戶，那麼您不應該執行此批次檔。 相反地，您應該以互動方式登入，或使用 runas 命令，且該帳戶會建立該帳戶的使用者設定檔。

批次檔包含在下面的自我解壓縮的封存。 若要使用它：

1. 您必須以系統管理員權限執行身分帳戶
2. [下載並開啟自動解壓縮的可執行檔](ms03-32-issue/_static/fixup1.exe)
3. 將內容解壓縮至 c:\
4. 選取 [執行] 從 [開始] 功能表，然後輸入 `cmd.exe`
5. 在 開啟 命令 視窗中，輸入`c:\fixup.cmd`。
6. 出現提示時，請輸入<strong>1pass@word</strong>做為密碼。
7. 如果您有先前自訂的存取控制設定或 ASPNET 帳戶的資料庫帳戶權限，您必須立即重新套用這些設定。

在您的不便，這導致許多此。 可供使用，我們將刊登的其他資訊。

下面的矩陣圖詳細說明平台和版本，此問題所致。

| .NET Framework | 平台 | 受影響 |
| --- | --- | --- |
| 1.0 版 | Windows 2000 Professional | 否 |
| 1.0 版 | Windows 2000 Server | 否 |
| 1.0 版 | Windows XP Professional | [是] |
| 1.0 版 | Windows Server 2003 | 否 |
| 1.0 版 | Windows XP 回家 Cassini | 否 |
| 1.1 版 | Windows 2000 Professional | 否 |
| 1.1 版 | Windows 2000 Server | 否 |
| 1.1 版 | Windows XP Professional | 否 |
| 1.1 版 | Windows Server 2003 | 否 |
| 1.1 版 | Windows XP 回家 Cassini | 否 |

謝謝 ！   
 ASP.NET 團隊
