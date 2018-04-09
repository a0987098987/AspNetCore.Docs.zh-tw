---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: 驗證使用者使用表單驗證 (C#) |Microsoft 文件
author: microsoft
description: 了解如何使用 [Authorize] 屬性以密碼保護的 MVC 應用程式中的特定頁面。 您了解如何使用網站管理太...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: e1def84bbf48847339e89b239b026d053640b935
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-c"></a>驗證使用者使用表單驗證 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何使用 [Authorize] 屬性以密碼保護的 MVC 應用程式中的特定頁面。 您了解如何使用網站管理工具來建立及管理使用者和角色。 您也會了解如何設定使用者帳戶和角色的資訊儲存在何處。


本教學課程的目標是要說明如何使用表單驗證以密碼保護在 ASP.NET MVC 應用程式中的檢視。 您了解如何使用網站管理工具來建立使用者和角色。 您也會學習如何叫用控制器的動作時，防止未經授權的使用者。 最後，您會了解如何設定使用者名稱和密碼的儲存位置。

#### <a name="using-the-web-site-administration-tool"></a>使用網站管理工具

我們執行任何其他動作之前，我們應該先建立某些使用者和角色。 若要建立新的使用者和角色最簡單的方式是利用 Visual Studio 2008 網站管理工具。 您可以選取功能表選項來啟動這個工具**專案時，ASP.NET 組態**。 或者，您可以叫用會出現在 [方案總管] 視窗頂端的世界 hammer （有些嚇人） 圖示，即可啟動網站管理工具 （請參閱圖 1）。

**圖 1 – 啟動網站管理工具**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

在網站管理工具中，您可以選取 [安全性] 索引標籤建立新的使用者和角色。按一下**建立使用者**連結，以建立名為 Stephen 的新使用者 （請參閱圖 2）。 作者： Stephen 使用者提供您想要的任何密碼 (例如，*密碼*)。

**圖 2 – 建立新的使用者**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

您可以建立新的角色啟用角色，定義一個或多個角色。 按一下 啟用角色**啟用角色**連結。 接下來，建立名為的角色*管理員*按一下**建立或管理角色**連結 （請參閱圖 3）。

**圖 3 – 建立新的角色**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

最後，建立名為 Sally 的新使用者並 Sally 關聯的系統管理員角色，按一下 [建立使用者] 連結，然後建立 Sally 時，請選取系統管理員 （請參閱圖 4）。

**圖 4-新增使用者至角色**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

當完成後時，您應該有兩個名為 Stephen 和 Sally 的新使用者。 您也應該擁有名為系統管理員的新角色。 Sally 是系統管理員角色的成員，且 Stephen。

#### <a name="requiring-authorization"></a>需要授權

您可以要求使用者在使用者叫用 [Authorize] 屬性加入至動作的控制器動作之前，進行驗證。 您可以將 [Authorize] 屬性套用至個別的控制器動作，或您可以將此屬性套用至整個控制器類別。

例如，列表 1 中的控制站會公開名為 CompanySecrets() 的動作。 因為此動作會以 [Authorize] 屬性裝飾，除非使用者已驗證無法呼叫此動作。

**列出 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

如果您藉由在您的瀏覽器的網址列中輸入 URL /Home/CompanySecrets 叫用 CompanySecrets() 動作，而且您不是已驗證的使用者，則您會重新導向至登入檢視自動 （請參閱圖 5）。

**圖 5-登入檢視**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

輸入您的使用者名稱和密碼，您可以使用登入檢視。 如果您不是已註冊的使用者，則您可以按一下**註冊**連結以瀏覽至登錄檢視 （請參閱圖 6）。 您可以使用 [暫存器] 檢視來建立新的使用者帳戶。

**圖 6 – 暫存器檢視**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

已成功登入之後，您可以看到 CompanySecrets 檢視 （請參閱圖 7）。 根據預設，您將繼續登入，直到您關閉瀏覽器視窗。

**圖 7 – CompanySecrets 檢視**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>授權的使用者名稱或使用者角色

您可以使用 [Authorize] 屬性來限制控制器動作，以一組特定的使用者或一組特定的使用者角色存取。 例如，修改的主控制器清單 2 中包含兩個名為 StephenSecrets() 和 AdministratorSecrets() 的新動作。

**列出 2 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

只有具有使用者名稱 Stephen 使用者可以叫用 StephenSecrets() 動作。 所有其他使用者取得重新導向至登入檢視。 使用者屬性可以接受以逗號分隔清單的使用者帳戶名稱。

只有系統管理員角色中的使用者可以叫用 AdministratorSecrets() 動作。 例如，因為 Sally 是 Administrators 群組的成員，她可以叫用 AdministratorSecrets() 動作。 所有其他使用者取得重新導向至登入檢視。 角色屬性可以接受以逗號分隔清單的角色名稱。

#### <a name="configuring-authentication"></a>設定驗證

此時，您可能會想知道所儲存的使用者帳戶和角色資訊。 根據預設，資訊會儲存在 MVC 應用程式的應用程式中 qr 具名的 ASPNETDB.mdf (RANU) SQL Express 的資料庫\_Data 資料夾。 這個資料庫是由 ASP.NET 架構時自動產生您開始使用成員資格。

若要查看 ASPNETDB.mdf 資料庫，在 [方案總管] 視窗中的，您首先要選取功能表選項專案中，顯示所有檔案。

開發應用程式時，使用預設 SQL Express 資料庫是正常的。 最可能的原因不過，您不想要用於生產應用程式的預設 ASPNETDB.mdf 的資料庫。 在此情況下，您可以變更下列兩個步驟，以儲存使用者帳戶資訊：

1. 將應用程式服務資料庫物件加入至您的生產資料庫-變更您的應用程式的連接字串以指向您的生產資料庫

第一個步驟會將所有必要的資料庫物件 （資料表和預存程序） 為您的生產資料庫。 若要將這些物件加入至新的資料庫最簡單的方式是利用 ASP.NET SQL Server 安裝精靈 （請參閱圖 8）。 您可以啟動此工具，從 Microsoft Visual Studio 2008 程式群組開啟 Visual Studio 2008 命令提示字元，並從命令提示字元執行下列命令：

aspnet\_regsql

**圖 8 – ASP.NET SQL Server 安裝精靈**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

ASP.NET SQL Server 安裝精靈可讓您選取網路上的 SQL Server 資料庫，然後安裝所有 ASP.NET 應用程式服務所需的資料庫物件。 資料庫伺服器不需要放在本機電腦上。

> [!NOTE] 
> 
> 如果您不想要使用 ASP.NET SQL Server 安裝精靈，您可以新增應用程式服務的資料庫物件的下列資料夾中找到 SQL 指令碼：
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727


建立必要的資料庫物件之後，您需要修改您的 MVC 應用程式所使用的資料庫連接。 修改 ApplicationServices 連接字串，在您 web 組態檔 (web.config) 中的，讓它指向實際執行資料庫。 例如，範例 3 中修改過的連接會指向名為的 MyProductionDB （原始 ApplicationServices 連接字串已經被標記為註解） 的資料庫。

**列出 3 – Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>設定資料庫權限

如果您使用整合式安全性來連接到您的資料庫將會需要將正確的 Windows 使用者帳戶做為登入加入至您的資料庫。 正確的帳戶取決於您使用 ASP.NET 程式開發伺服器或 Internet Information Services 為網頁伺服器。 正確的使用者帳戶也會取決於您的作業系統。

如果您使用 ASP.NET 程式開發伺服器 （供 Visual Studio 的預設 web 伺服器） 應用程式執行您的 Windows 使用者帳戶的內容中。 在此情況下，您需要加入您的 Windows 使用者帳戶做為資料庫伺服器登入。

或者，如果您使用 Internet Information Services 然後您需要將 ASPNET 帳戶或 NT 授權單位/網路服務帳戶新增為資料庫伺服器登入。 如果您使用 Windows XP，請將 ASPNET 帳戶做為登入加入至您的資料庫。 如果您使用較新作業系統，例如 Windows Vista 或 Windows Server 2008，請將 NT 授權單位/網路服務帳戶新增為資料庫登入。

可以使用 Microsoft SQL Server Management Studio 將新的使用者帳戶加入至您的資料庫 （請參閱圖 9）。

**圖 9 – 建立新的 Microsoft SQL Server 登入**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

建立必要的登入之後，您需要登入對應到具有正確的資料庫角色的資料庫使用者。 按兩下登入，並選取 [使用者對應] 索引標籤。選取一或多個應用程式服務資料庫角色。 比方說，若要驗證的使用者，您必須啟用 aspnet\_成員資格\_BasicAccess 資料庫角色。 若要建立新的使用者，您必須啟用 aspnet\_成員資格\_FullAccess 資料庫角色 （請參閱圖 10）。

**圖 10 – 新增應用程式服務資料庫角色**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>總結

在本教學課程中，您學會如何建立 ASP.NET MVC 應用程式時，使用表單驗證。 首先，您學到如何利用網站管理工具建立新的使用者和角色。 接下來，您學會如何使用 [Authorize] 屬性，以避免未經授權的使用者叫用控制器的動作。 最後，您學會如何設定您的 MVC 應用程式，將使用者和角色資訊儲存在實際執行的資料庫。

> [!div class="step-by-step"]
> [下一步](authenticating-users-with-windows-authentication-cs.md)
