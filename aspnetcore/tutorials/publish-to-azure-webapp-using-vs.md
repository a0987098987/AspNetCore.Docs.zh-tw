---
title: "使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Cesar Blum Silveira](https://github.com/cesarbs)

## <a name="set-up-the-development-environment"></a>設定開發環境

* 安裝最新的 [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)。 如果還沒有 Visual Studio，SDK 會安裝它。

> [!NOTE]
> 如果您的電腦沒有許多相依性，SDK 安裝可能需要超過 30 分鐘的時間。

* 安裝 [.NET Core + Visual Studio 工具](http://go.microsoft.com/fwlink/?LinkID=798306)

* 驗證您的 [Azure 帳戶](https://portal.azure.com/)。 您可以[建立免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/)或[啟用 Visual Studio 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。

## <a name="create-a-web-app"></a>建立 Web 應用程式

在 Visual Studio 起始頁中，點選 [新增專案...]。

![起始頁](publish-to-azure-webapp-using-vs/_static/new_project.png)

或者，您可以使用功能表來建立新的專案。 點選 [檔案] > [新增] > [專案...]。

![檔案功能表](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

完成 [新增專案] 對話方塊：

* 在左窗格中，點選 [Web]

* 在中央窗格中，點選 [ASP.NET Core Web 應用程式 (.NET Core)]

* 點選 [確定]

![[新增專案] 對話](publish-to-azure-webapp-using-vs/_static/new_prj.png)

在 [新增 ASP.NET Core Web 應用程式 (.NET Core)] 對話方塊中：

* 點選 [Web 應用程式]

* 確認 [驗證] 已設定為 [個別使用者帳戶]

* 確認**未**核取 [雲端中的主機]

* 點選 [確定]

![[新增 ASP.NET Core Web 應用程式 (.NET Core)] 對話方塊](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a>在本機測試應用程式

* 按 **Ctrl-F5** 在本機執行應用程式

* 點選 [關於] 和 [連絡人] 連結。 根據裝置大小，您可能需要點選瀏覽圖示來顯示連結

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

* 點選 [註冊]，並註冊新的使用者。 您可以使用虛構的電子郵件地址。 在提交時，您會收到下列錯誤：

![內部伺服器錯誤：處理要求時資料庫作業失敗。 SQL 例外狀況：無法開啟資料庫。 為應用程式資料庫內容套用現有的移轉可能會解決此問題。](publish-to-azure-webapp-using-vs/_static/mig.png)

您可以使用兩種不同的方式解決這個問題：

* 點選 [套用移轉]，一旦頁面更新，請重新整理頁面；或

* 從專案目錄中的命令提示字元執行下列命令：

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

應用程式會顯示用來註冊新使用者的電子郵件和 [登出] 連結。

![在 Microsoft Edge 中開啟的 Web 應用程式。 [註冊] 連結會取代為文字「abc@example.com，您好！」](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

確認部署的已發佈應用程式未在執行中。 當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。 因為無法複製鎖定的檔案，所以無法執行部署。

在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]。

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

在 [發行] 對話方塊中，點選 [Microsoft Azure App Service]。

![[發行] 對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

點選 [新增...] 來建立新的資源群組。 建立新的資源群組可讓您更輕鬆地刪除您在本教學課程中建立的所有 Azure 資源。

![[應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

建立新的資源群組和應用程式服務方案：

* 針對資源群組點選 [新增...]，然後輸入新資源群組的名稱。

* 針對應用程式服務方案點選 [新增...]，然後選取您附近的位置。 您可以保留預設產生的名稱

* 點選 [探索其他 Azure 服務] 以建立新的資料庫

![[新增資源群組] 對話方塊：裝載面板](publish-to-azure-webapp-using-vs/_static/cas.png)

* 點選綠色的  **+** 圖示以建立新的 SQL Database

![[新增資源群組] 對話方塊：服務面板](publish-to-azure-webapp-using-vs/_static/sql.png)

* 在 [設定 SQL Database] 對話方塊上點選 [新增...] 來建立新的資料庫伺服器。

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf.png)

* 輸入系統管理員使用者名稱和密碼，然後點選 [確定]。 請勿忘記您在此步驟中建立的使用者名稱和密碼。 您可以保留預設的**伺服器名稱**

![[設定 SQL Server] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> "admin" 不允許作為系統管理員使用者名稱。

* 在 [設定 SQL Database] 對話方塊中選點 [確定]

![[設定 SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* 在 [建立應用程式服務] 對話方塊上點選 [建立]

![[建立應用程式服務] 對話方塊](publish-to-azure-webapp-using-vs/_static/create_as.png)

* 在 [發行] 對話方塊中點選 [下一步]

![[發行] 對話方塊：連線面板](publish-to-azure-webapp-using-vs/_static/pubc.png)

* 在 [發行] 對話方塊的 [設定] 階段：

  * 展開 [資料庫] 並核取 [在執行階段使用此連線字串]

  * 展開 [Entity Framework 移轉] 並核取 [在發行時套用此移轉]

* 點選 [發行] 並等候 Visual Studio 完成發行應用程式

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pubs.png)

Visual Studio 會將您的應用程式發行至 Azure，並在您的瀏覽器中啟動雲端應用程式。

### <a name="test-your-app-in-azure"></a>在 Azure 中測試應用程式

* 測試 [關於] 和 [連絡人] 連結

* 註冊新的使用者

![在 Microsoft Edge 中於 Azure App Service 上開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a>更新應用程式

* 編輯 `Views/Home/About.cshtml` Razor 檢視檔案，然後變更其內容。 例如: 

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* 以滑鼠右鍵按一下專案，然後再次點選 [發行...]

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

* 發行應用程式之後，請確認您所做的變更可在 Azure 上使用

### <a name="clean-up"></a>清除

當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。

* 選取 [資源群組]，然後點選您建立的資源群組

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* 在 [資源群組] 刀鋒視窗中，點選 [刪除]

![Azure 入口網站：[資源群組] 刀鋒視窗](publish-to-azure-webapp-using-vs/_static/rgd.png)

* 輸入資源群組的名稱，然後點選 [刪除]。 您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除

### <a name="next-steps"></a>後續步驟

* [ASP.NET Core MVC 與 Visual Studio 使用者入門](first-mvc-app/start-mvc.md)

* [ASP.NET Core 簡介](../index.md)

* [基礎概念](../fundamentals/index.md)
