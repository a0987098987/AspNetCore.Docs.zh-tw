---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: "設定連絡人管理員解決方案 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何下載及設定連絡人管理員解決方案開發人員工作站上本機執行。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 85468949ee61504d6076a191b70a96e8018c67aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="setting-up-the-contact-manager-solution"></a>設定連絡人管理員解決方案
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何下載及設定連絡人管理員解決方案開發人員工作站上本機執行。


## <a name="system-requirements"></a>系統需求

若要在本機執行，請連絡管理員方案並執行本教學課程中所述的其他工作，您必須在開發人員工作站上安裝此軟體：

- Visual Studio 2010 Service Pack 1、 Premium 或 Ultimate Edition
- Internet Information Services (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS Web Deployment Tool (Web Deploy) 2.1 或更新版本
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

除了 Visual Studio 2010，您可以下載並安裝最新版本的所有的這些產品和元件透過[Web Platform Installer](https://go.microsoft.com/?linkid=9805118)。

## <a name="download-and-extract-the-solution"></a>下載並解壓縮方案

您可以從 MSDN Code Gallery 下載連絡人管理員應用程式範例[這裡](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)。

## <a name="configure-and-run-the-solution"></a>設定並執行方案

若要設定和在本機電腦上執行連絡人管理員解決方案，您需要執行下列高階步驟：

1. 如果您還沒有已經，建立本機的 ASP.NET 應用程式服務資料庫已啟用的成員資格和角色管理功能。
2. 編輯連接字串中的*web.config*檔案以指向本機 SQL Server Express 執行個體。
3. 從 Visual Studio 2010 中執行方案。

本章節的其餘部分提供如何完成這些工作的詳細指引。

**若要建立應用程式服務資料庫**

1. 開啟 Visual Studio 2010 命令提示字元。 若要這樣做，請在**啟動**功能表上，指向**所有程式**，按一下**Microsoft Visual Studio 2010**，按一下**Visual Studio Tools**，然後按一下按一下**Visual Studio 命令提示字元 (2010)**。
2. 在命令提示字元中，輸入下列命令，並再按 Enter 鍵：

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. 使用**– C**參數來指定您的資料庫伺服器的連接字串。
    2. 使用**–**參數來指定應用程式服務的功能，您想要加入至資料庫。 在此情況下， **m**指出您想要加入成員資格提供者的支援和**r**指出您想要加入角色管理員的支援。
    3. 使用**– d**參數來指定您的應用程式服務資料庫的名稱。 如果您省略這個參數，此公用程式會建立資料庫的預設名稱與**aspnetdb**。
3. 當成功建立資料庫時，命令提示字元會顯示確認訊息。

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> 如需有關 aspnet\_regsql 公用程式，請參閱[ASP.NET SQL Server 註冊工具 (Aspnet\_regsql.exe)](https://msdn.microsoft.com/en-us/library/ms229862(v=vs.100).aspx)。


下一個步驟是確定連絡管理員方案中的連接字串指向 SQL Server Express 的本機執行個體。

**若要更新連接字串**

1. Visual Studio 2010 中開啟連絡人管理員方案。
2. 在**方案總管] 中**視窗中，展開 [ **ContactManager.Mvc**專案，然後再連按兩下**Web.config**節點。

    > [!NOTE]
    > ContactManager.Mvc 專案包含兩個*web.config*檔案。 您必須編輯專案層級檔案。

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. 在**connectionStrings**項目，請確認連接字串名稱為**ApplicationServices**指向本機 ASP.NET 應用程式服務資料庫。

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. 在**方案總管] 中**視窗中，展開 [ **ContactManager.Service**專案，然後再連按兩下**Web.config**節點。

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. 在**connectionStrings**名為連接字串中的項目， **ContactManagerContext**，確認**資料來源**屬性設定為本機的 SQL 執行個體Server Express。 您不需要變更連接字串中的其他任何項目。

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. 儲存所有開啟的檔案。

您現在應該可以在本機電腦上執行，請連絡管理員解決方案。

> [!NOTE]
> 如果您遵循這些步驟不需要先建立應用程式服務資料庫時，ASP.NET 會在您嘗試建立的使用者第一次建立資料庫。 不過，手動建立資料庫可讓您更多控制您想要支援的應用程式服務功能集。


**若要執行連絡人管理員方案**

1. 在 Visual Studio 2010 中，按下 F5。
2. 啟動 Internet Explorer，並要求，請連絡管理員 ASP.NET MVC 3 應用程式的 URL。 根據預設，應用程式會顯示**所有連絡人**頁面。

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. 加入幾個連絡人，然後確認 應用程式可以正常運作。

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. 瀏覽至`http://localhost:50114/Account/Register`（如果您裝載不同的通訊埠上的應用程式，請調整 URL）。 新增使用者名稱、 電子郵件地址和密碼，並確認您能夠順利註冊帳戶。

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. 瀏覽至`http://localhost:50114/Account/LogOn`（如果您裝載不同的通訊埠上的應用程式，請調整 URL）。 請確認您能夠使用您剛才建立的帳戶登入。

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. 關閉 Internet Explorer 停止偵錯。

## <a name="conclusion"></a>結論

此時，請連絡管理員解決方案應該完全設定在本機電腦上執行。 當您使用透過其他主題，在本教學課程時，您可以使用方案做為參考。

下一個主題[了解專案檔](understanding-the-project-file.md)，說明如何使用自訂 Microsoft Build Engine (MSBuild) 專案檔，請連絡管理員方案內控制部署程序。

>[!div class="step-by-step"]
[上一頁](the-contact-manager-solution.md)
[下一頁](understanding-the-project-file.md)
