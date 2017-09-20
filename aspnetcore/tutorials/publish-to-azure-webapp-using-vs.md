---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service

由 [Rick Anderson](https://twitter.com/RickAndMSFT)、[Cesar Blum Silveira](https://github.com/cesarbs) 及 [Rachel Appel](https://twitter.com/rachelappel) 共同編纂

## <a name="set-up-the-development-environment"></a>設定開發環境

* 安裝最新的 [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/)。 如果還沒有 Visual Studio，SDK 會安裝它。

* 驗證您的 [Azure 帳戶](https://portal.azure.com/)。 您可以[建立免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)或[啟用 Visual Studio 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。

## <a name="create-a-web-app"></a>建立 Web 應用程式

在 Visual Studio 起始畫面中，選取 [檔案] > [新增] > [專案...]

![檔案功能表](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

完成 [新增專案] 對話方塊：

* 在左窗格中，選取 [.NET Core]。

* 在中央窗格中，選取 [ASP.NET Core Web 應用程式]。

* 選取 [確定]。

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

在 [新增 ASP.NET Core Web 應用程式] 對話方塊中：

* 選取 [Web 應用程式]。

* 選取 [變更驗證]。

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

[變更驗證] 對話方塊會隨即出現。 

* 選取 [個別使用者帳戶]。

* 選取 [確定] 返回 [新增 ASP.NET Core Web 應用程式]，然後再次選取 [確定]。

![[新增 ASP.NET Core Web 驗證] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio 會建立解決方案。

## <a name="run-the-app-locally"></a>在本機執行應用程式

* 選擇 [偵錯]，然後 [啟動但不偵錯] 以在本機執行應用程式。

* 按一下 [關於] 與 [連絡資訊] 連結，以驗證 Web 應用程式能夠運作。

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

* 選取 [註冊] 並註冊新的使用者。 您可以使用虛構的電子郵件地址。 提交時，頁面會顯示下列錯誤：

    「內部伺服器錯誤: 處理要求時資料庫作業失敗。SQL 例外狀況：無法開啟資料庫。為應用程式資料庫內容套用現有的移轉可能會解決此問題。」

* 選取 [套用移轉]，並在頁面更新後重新整理頁面。

![內部伺服器錯誤：處理要求時資料庫作業失敗。 SQL 例外狀況：無法開啟資料庫。 為應用程式資料庫內容套用現有的移轉可能會解決此問題。](publish-to-azure-webapp-using-vs/_static/mig.png)

應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。

![在 Microsoft Edge 中開啟的 Web 應用程式。 [註冊] 連結會取代為文字「email@domain.com，您好！」](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

關閉網頁，回到 Visual Studio 並在 [偵錯] 功能表選取 [停止偵錯]。

在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

在 [發行] 對話方塊中，選取 [Microsoft Azure App Service]，然後按一下 [發行]。

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

* 為應用程式提供唯一名稱。 

* 選取訂用帳戶。

* 對資源群組選取 [新增...]，並輸入新資源群組的名稱。

* 對應用程式服務方案選取 [新增...]，並選取您附近的位置。 您可以保留預設產生的名稱。

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* 選取 [服務] 索引標籤來建立新的資料庫。

* 選取綠色的 **+** 圖示來建立新的 SQL 資料庫

![新增 SQL 資料庫](publish-to-azure-webapp-using-vs/_static/sql.png)

* 在 [設定 SQL 資料庫] 對話方塊上選取 [新增...] 來建立新的資料庫。

![新增 SQL 資料庫與伺服器](publish-to-azure-webapp-using-vs/_static/conf.png)

[設定 SQL Server] 對話方塊會隨即出現。

* 輸入系統管理員使用者名稱與密碼，然後選取 [確定]。 請勿忘記您在此步驟中建立的使用者名稱和密碼。 您可以保留預設的**伺服器名稱**。 

* 輸入資料庫與連接字串的名稱。

> [!NOTE]
> "admin" 不允許作為系統管理員使用者名稱。

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* 選取 [確定]。

Visual Studio 會回到 [建立 App Service] 對話方塊。

* 在 [建立 App Service] 對話方塊上選取 [建立]。

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* 在 [發行] 對話方塊中按一下 [設定] 連結。

![[發行] 對話方塊：連線面板](publish-to-azure-webapp-using-vs/_static/pubc.png)

在 [發行] 對話方塊的 [設定] 頁面上：

  * 展開 [資料庫] 並選取 [在執行階段使用此連接字串]。

  * 展開 [Entity Framework 移轉] 並選取 [在發行時套用此移轉]。

* 選取 [儲存]。 Visual Studio 會回到 [發行] 對話方塊。 

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

按一下 [發行] 。 Visual Studio 會將您的應用程式發行至 Azure，並在您的瀏覽器中啟動雲端應用程式。

### <a name="test-your-app-in-azure"></a>在 Azure 中測試應用程式

* 測試 [關於] 和 [連絡人] 連結

* 註冊新的使用者

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>更新應用程式

* 編輯 *Pages/About.cshtml* Razor 頁面，並變更其內容。 例如，您可以修改段落使其說出 "Hello ASP.NET Core!"：

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* 以滑鼠右鍵按一下專案，然後再次選取 [發行...]。

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* 發行應用程式後，請驗證您進行的變更在 Azure 上有效。

![驗證工作是否完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>清除

當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。

* 選取 [資源群組]，然後選取您建立的資源群組。

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* 在 [資源群組] 頁面中，選取 [刪除]。

![Azure 入口網站：[資源群組] 頁面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* 輸入資源群組的名稱，並選取 [刪除]。 您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除。

### <a name="next-steps"></a>後續步驟

* [ASP.NET Core MVC 與 Visual Studio 使用者入門](first-mvc-app/start-mvc.md)

* [ASP.NET Core 簡介](../index.md)

* [基礎概念](../fundamentals/index.md)
