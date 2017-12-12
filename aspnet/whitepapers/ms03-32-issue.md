---
uid: whitepapers/ms03-32-issue
title: "修正 「 無法使用伺服器應用程式 」 錯誤之後套用安全性更新 IE |Microsoft 文件"
author: rick-anderson
description: "本白皮書說明 MS03 32 安全性更新可以修正這個問題會影響 ASP.NET 1.0 Wi-fi 上執行的應用程式的 Internet explorer 的修補程式..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 8658e387aeb4ea0340080666906b2b89db49a31a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>修正 「 無法使用伺服器應用程式 」 錯誤 IE 的安全性更新
====================
> 本白皮書說明 MS03 32 安全性更新可以修正這個問題會影響 ASP.NET 1.0 應用程式在 Windows XP Professional 上執行的 Internet explorer 的修補程式。
> 
> 適用於 ASP.NET 1.0 和 Windows XP Professional。


Microsoft Internet Explorer 的安全性修補程式 MS03 32 安全性更新與 Windows XP 上執行 ASP.NET 1.0 識別發生問題。 可以安裝這個修補程式，以手動方式或從 Windows Update 網站取得最新重大更新。

此問題的徵兆是，在 Windows XP 電腦上安裝修補程式之後, 本機 IIS 5.1 web 伺服器上執行的 ASP.NET 應用程式的所有要求會都導致錯誤訊息，指出 「 伺服器應用程式無法使用 」。 遠端 web 伺服器的要求，則不會受到影響。

這個問題只會影響 Windows XP 上執行 ASP.NET 1.0 安裝。 它不會影響執行 Windows 2000 或 Windows Server 2003 的電腦。 它也不會影響安裝的 ASP.NET 1.1 與執行 Windows XP 的電腦。

請注意，此問題**不**安全性與 ASP.NET 的錯誤。 它**不**開啟，或允許對 ASP.NET 應用程式或伺服器的任何惡意攻擊。 相反地，它是純功能性 bug 修補程式本身所造成。

我們正在硬永久此問題的解決方案。 在此同時，您可以執行下列批次檔因應措施的問題。 批次檔會執行下列動作：

1. 停止 IIS 和 ASP.NET 狀態服務
2. 刪除並重新建立已知的暫時密碼 ASPNET 帳戶
3. 使用 Windows`runas`命令，以啟動建立 ASPNET 使用者設定檔的可執行檔
4. 重新登錄 ASP.NET。 這會建立新的隨機密碼的帳戶，並套用預設 ASP.NET 存取控制設定，
5. 重新啟動 IIS 服務

批次檔會包含硬式編碼暫時密碼的 「**1pass@word**」 將會提示您輸入 runas 命令批次檔執行時。 Runas 命令完成之後，ASPNET 帳戶密碼會重新建立強式隨機值。 請注意，如果硬式編碼密碼不符合密碼複雜性需求，您的環境中，批次檔可能會失敗。 如果是這樣，您可以變更適用於您環境的另一個值。

*> [!IMPORTANT]*如果您加入自訂存取控制設定或 ASPNET 帳戶的資料庫帳戶權限，他們必須完成這個批次檔後重新建立。 這是因為當帳戶重新建立，它會取得新的安全性識別碼 (SID)。

*> [!IMPORTANT]*如果您執行 ASP.NET 工作者處理序，以自訂帳戶，ASPNET 帳戶以外，那麼您不應該執行此批次檔。 相反地，您應該以互動方式登入，或使用 runas 命令，與該帳戶將會建立該帳戶的使用者設定檔。

批次檔隨附於下面的自我解壓縮的封存。 若要使用它：

1. 您必須以系統管理權限執行身分帳戶
2. [下載並開啟自動解壓縮可執行檔](ms03-32-issue/_static/fixup1.exe)
3. 擷取到 c:\ 的內容
4. 選取 [執行] 從 [開始] 功能表，然後輸入`cmd.exe`
5. 在 開啟 命令 視窗中，輸入`c:\fixup.cmd`。
6. 出現提示時，輸入 **1pass@word** 做為密碼。
7. 如果您有先前自訂的存取控制設定或 ASPNET 帳戶的資料庫帳戶權限，您要立即重新套用這些設定。

在您的不便，這導致許多此。 可用時，我們將會張貼的其他資訊。

詳細資料平台和版本，此問題所致下方的矩陣。

| .NET Framework | 平台 | 受影響 |
| --- | --- | --- |
| 1.0 版 | Windows 2000 Professional | 否 |
| 1.0 版 | Windows 2000 Server | 否 |
| 1.0 版 | Windows XP Professional | 是 |
| 1.0 版 | Windows Server 2003 | 否 |
| 1.0 版 | Windows XP 家用版與 Cassini | 否 |
| 1.1 版 | Windows 2000 Professional | 否 |
| 1.1 版 | Windows 2000 Server | 否 |
| 1.1 版 | Windows XP Professional | 否 |
| 1.1 版 | Windows Server 2003 | 否 |
| 1.1 版 | Windows XP 家用版與 Cassini | 否 |

感謝您，   
 ASP.NET 團隊
