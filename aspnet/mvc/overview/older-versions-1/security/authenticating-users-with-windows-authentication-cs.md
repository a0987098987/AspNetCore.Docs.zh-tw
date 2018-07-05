---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: 驗證使用者使用 Windows 驗證 (C#) |Microsoft Docs
author: microsoft
description: 了解如何在 MVC 應用程式的內容中使用 Windows 驗證。 您了解如何啟用 Windows 驗證，在您的應用程式 web 共同...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 0261f3b989d721e6e9aae8fa5766e02c8bca7b63
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397799"
---
<a name="authenticating-users-with-windows-authentication-c"></a>使用 Windows 驗證 (C#) 驗證使用者
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何在 MVC 應用程式的內容中使用 Windows 驗證。 您將了解如何啟用您的應用程式 web 組態檔中的 Windows 驗證以及如何設定 IIS 驗證。 最後，您會了解如何使用 [Authorize] 屬性來限制存取權給特定的 Windows 使用者或群組的控制器動作。


本教學課程的目標是要說明，如何利用內建於網際網路資訊服務的安全性功能，以藉由密碼來保護 MVC 應用程式中的檢視。 了解如何只被允許的特定 Windows 使用者或特定的 Windows 群組的成員叫用控制器動作。

當您建立公司內部的網站 （內部網路網站）並且想讓使用者能夠使用標準的 Windows 使用者名稱及密碼存取網站時，使用 Windows 驗證就很合理。 如果您要建置對外網站 （網際網路網站），請考慮改用表單驗證。

#### <a name="enabling-windows-authentication"></a>啟用 Windows 驗證

當您建立新的 ASP.NET MVC 應用程式時，預設是不啟用 Windows 驗證的。 表單驗證是MVC 應用程式預設啟用的驗證類型。 您必須修改 MVC 應用程式的 web 組態檔 (web.config)，以啟用 Windows 驗證。 尋找&lt;驗證&gt;區段，然後修改它以使用 Windows，而不是表單驗證，就像這樣：

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

當您啟用 Windows 驗證時，您的網頁伺服器會負責驗證使用者。 通常，當在建立及部署 ASP.NET MVC 應用程式時，會使用兩種不同的網頁伺服器。

首先，在開發 MVC 應用程式時，您會使用 Visual Studio 隨附的 ASP.NET 程式開發 Web 伺服器。 根據預設，ASP.NET 開發 Web 伺服器會以目前的 Windows 帳戶 （不論您登入 Windows 的帳戶） 執行所有頁面。

ASP.NET 開發 Web 伺服器也支援 NTLM 驗證。 您可以滑鼠右鍵按一下方案總管] 視窗中的專案名稱，然後選取 [屬性，以啟用 NTLM 驗證。 接下來，選取 [Web] 頁籤，並勾選 NTLM 核取方塊 （請參閱圖 1）。 

**圖 1 – 啟用 NTLM 驗證的 ASP.NET 程式開發 Web 伺服器**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

另一種，正式環境的 web 應用程式中，使用 IIS 做為網頁伺服器。 IIS 支援的各種驗證類型有：

- 基本驗證-定義 HTTP 1.0 通訊協定的一部分。 使用者名稱和密碼以純文字 (Base64 編碼) 透過網際網路傳送。 -摘要式驗證 – 傳送密碼，而不是密碼本身，在網際網路上的雜湊。 -整合式的 Windows (NTLM) 驗證 – 最佳的型別，要使用 windows 的內部網路環境中使用的驗證。 -憑證驗證 – 啟用驗證使用用戶端憑證。 此憑證對應至 Windows 使用者帳戶。

> [!NOTE] 
> 
> 如需這些不同類型的驗證更詳細概觀，請參閱[ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx)。


若要啟用特定類型的驗證，您可以使用 Internet Information Services 管理員。 請注意，所有類型的驗證不都是在每個作業系統的情況下可用。 此外，如果您使用 IIS 7.0 使用 Windows Vista，您必須啟用不同類型的 Windows 驗證，才會出現在 Internet Information Services 管理員。 開啟**控制台中、 程式、 程式和功能，開啟或關閉開啟的 Windows 功能**，然後展開 Internet Information Services 節點 （請參閱 圖 2）。

**圖 2-啟用 Windows IIS 功能**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

使用 Internet Information Services，您可以啟用或停用的不同驗證類型。 例如，圖 3 說明停用匿名驗證並啟用整合式 Windows (NTLM) 驗證時使用 IIS 7.0。

**圖 3 – 啟用整合式的 Windows 驗證**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>授權 Windows 使用者和群組

啟用 Windows 驗證之後，您可以使用 [Authorize] 屬性來控制存取控制器或控制器動作。 這個屬性可以套用至整個的 MVC 控制器或特定的控制器動作。

例如，列表 1 中的主控制器會公開名為 index （）、 CompanySecrets() 和 StephenSecrets() 的三個動作。 任何人都可以叫用的 index （） 動作。 不過，只有 Windows 本機管理員群組的成員可以叫用 CompanySecrets() 動作。 最後，只有 Windows 網域使用者名為 Stephen （位於 Redmond 網域中） 可以叫用 StephenSecrets() 動作。

**列表 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> 因為 Windows 使用者帳戶控制 (UAC)，使用 Windows Vista 或 Windows Server 2008 時，本機 Administrators 群組行為會與不同於其他群組。 [Authorize] 屬性將不會正確辨識本機 Administrators 群組的成員，除非您修改您電腦的 UAC 設定。


當您嘗試叫用控制器動作，而不正確的權限的情況完全取決於啟用的驗證類型。 根據預設，使用 ASP.NET 程式開發伺服器時，您只會收到空白頁面。 頁面會提供與**401 未授權**HTTP 回應狀態。

如果相反地，您使用 IIS 已停用匿名驗證與基本驗證已啟用，則您一直收到登入對話方塊提示您要求受保護的頁面每次 （請參閱 圖 4）。

**[圖 4 – 基本驗證登入] 對話方塊**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>總結

本教學課程說明如何使用 Windows 驗證的 ASP.NET MVC 應用程式內容中。 您已了解如何啟用您的應用程式 web 組態檔中的 Windows 驗證以及如何設定 IIS 驗證。 最後，您已了解如何使用 [Authorize] 屬性來限制存取權給特定的 Windows 使用者或群組的控制器動作。

> [!div class="step-by-step"]
> [上一頁](authenticating-users-with-forms-authentication-cs.md)
> [下一頁](preventing-javascript-injection-attacks-cs.md)
