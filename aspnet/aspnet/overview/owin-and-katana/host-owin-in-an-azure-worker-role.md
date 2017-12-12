---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: "裝載 Azure 背景工作角色中的 OWIN |Microsoft 文件"
author: MikeWasson
description: "本教學課程會示範如何在自我裝載 OWIN Microsoft Azure 背景工作角色中。 開啟 Web 介面的.NET (OWIN) 所定義的.NET web 伺服器之間的抽象概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 647514ae5a92b9d729179327fb97bd8005b0a4b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="host-owin-in-an-azure-worker-role"></a>主機 OWIN 的 Azure 背景工作角色
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 本教學課程會示範如何在自我裝載 OWIN Microsoft Azure 背景工作角色中。
> 
> [開啟適用於.NET 的 Web 介面](http://owin.org/)(OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 以減少從 web 應用程式伺服器，使得 OWIN 適合自我裝載的 web 應用程式，在您自己的處理序，在 IIS 外部 – 例如，在 Azure 工作者角色。
> 
> 在此教學課程中，您將學習如何在自我裝載在 Microsoft Azure 背景工作角色內 OWIN 應用程式。 若要了解有關背景工作角色的詳細資訊，請參閱[Azure 執行模型](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK for.NET 2.3](https://azure.microsoft.com/en-us/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>建立 Microsoft Azure 專案

啟動 Visual Studio 系統管理員權限。 若要偵錯應用程式在本機，使用 Azure 計算模擬器需要系統管理員權限。

在**檔案**功能表上，按一下 **新增**，然後按一下 **專案**。 從**已安裝的範本**，在 Visual C# 中，按一下**雲端**，然後按一下  **Windows Azure 雲端服務**。 將專案命名為"AzureApp 」，然後按一下**確定**。

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

在**新的 Windows Azure 雲端服務** 對話方塊中，按兩下**背景工作角色**。 保留預設名稱 ("WorkerRole1")。 此步驟會將背景工作角色加入至方案。 按一下 [確定]。

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

建立 Visual Studio 方案包含兩個專案：

- &quot;AzureApp&quot;定義的角色和 Azure 應用程式的設定。
- &quot;WorkerRole1&quot;包含背景工作角色的程式碼。

一般情況下，Azure 應用程式都可以包含多個角色，但本教學課程使用的單一角色。

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>新增 OWIN 自我裝載套件

從**工具**功能表上，按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**。

在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>加入的 HTTP 端點

在 [方案總管] 中，展開 AzureApp 專案。 展開 角色 節點，以滑鼠右鍵按一下 WorkerRole1，並選取**屬性**。

![](host-owin-in-an-azure-worker-role/_static/image6.png)

按一下**端點**，然後按一下 **加入端點**。

在**通訊協定**下拉式清單中，選取 「 http 」。 在**公用連接埠**和**私用連接埠**，輸入 80。 這些連接埠號碼可能會不同。 公用連接埠是哪些用戶端時使用它們將要求傳送至角色。

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>建立 OWIN 啟動類別

在 方案總管 中，以滑鼠右鍵按一下 WorkerRole1 專案並選取**新增** / **類別**若要加入新的類別。 將類別命名為 `Startup` 。

以下列內容取代所有未定案程式碼：

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage`擴充方法會將簡單的 HTML 頁面加入至您的應用程式，以確認站台運作。

## <a name="start-the-owin-host"></a>啟動 OWIN 主機

開啟 WorkerRole.cs 檔案。 這個類別會定義啟動及停止背景工作角色時執行的程式碼。

加入下列 using 陳述式：

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

新增**IDisposable**成員`WorkerRole`類別：

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

在`OnStart`方法，加入下列的程式碼，以啟動主機時：

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start**方法會啟動 OWIN 主機。 名稱`Startup`類別是方法的型別參數。 依照慣例，主機會呼叫`Configure`這個類別的方法。

覆寫`OnStop`來處置*\_應用程式*執行個體：

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

以下是 WorkerRole.cs 的完整程式碼：

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

建置方案，並按下 F5，在 Azure 計算模擬器在本機執行應用程式。 根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。

計算模擬器會將本機 IP 位址指派給端點。 您可以藉由檢視計算模擬器 UI 中找到的 IP 位址。 在工作列通知區域中的模擬器圖示上按一下滑鼠右鍵，然後選取**顯示計算模擬器 UI**。

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

尋找 IP 位址在服務部署、 部署 [識別碼]，服務詳細資料。 開啟網頁瀏覽器並瀏覽至 http://*位址*，其中*位址*是由計算模擬器; 指派的 IP 位址，例如`http://127.0.0.1:80`。 您應該會看到 OWIN 歡迎頁面：

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>部署至 Azure

此步驟中，您必須有 Azure 帳戶。 如果您還沒有其中一個，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱[Microsoft Azure 免費試用](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F)。

在 [方案總管] 中，以滑鼠右鍵按一下 AzureApp 專案。 選取 [發行]。

![](host-owin-in-an-azure-worker-role/_static/image12.png)

如果您未登入您的 Azure 帳戶，按一下**登入**。

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

您已登入後，選擇訂用帳戶，並按一下**下一步**。

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

輸入雲端服務的名稱，然後選擇一個區域。 按一下 [建立] 。

![](host-owin-in-an-azure-worker-role/_static/image17.png)

按一下 [發行] 。

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Azure 活動記錄 視窗會顯示部署的進度。 部署應用程式時，瀏覽至`http://appname.cloudapp.net/`，其中*appname*是您的雲端服務的名稱。

## <a name="additional-resources"></a>其他資源

- [專案 Katana 的概觀](an-overview-of-project-katana.md)
- [Katana GitHub 上的專案](https://github.com/aspnet/AspNetKatana/)
