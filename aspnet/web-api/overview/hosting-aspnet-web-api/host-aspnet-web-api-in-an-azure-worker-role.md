---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: 裝載 ASP.NET Web API 2 中的 Azure 背景工作角色 |Microsoft Docs
author: MikeWasson
description: 本教學課程會示範如何裝載在 Azure 背景工作角色中的 ASP.NET Web API 使用 OWIN 自我裝載 Web API 架構。 Open Web Interface for.NET (OWIN) de...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 40cb1a4514beaf81e7ed75bbd3e478f2ba146fe5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910742"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>裝載 ASP.NET Web API 2 中的 Azure 背景工作角色
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 本教學課程會示範如何裝載在 Azure 背景工作角色中的 ASP.NET Web API 使用 OWIN 自我裝載 Web API 架構。
>
> [Open Web Interface for.NET](http://owin.org/) (OWIN) 定義.NET web 伺服器和 web 應用程式之間的抽象概念。 OWIN 可分隔的伺服器，讓 OWIN 很適合用於自我裝載的 web 應用程式，在您自己的程序，IIS 外部的 web 應用程式 – 比方說，在 Azure 背景工作角色。
>
> 在本教學課程中，您將使用 Microsoft.Owin.Host.HttpListener 封裝，以提供 HTTP 伺服器，用於自我裝載 OWIN 應用程式。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web API 2
> - [Azure SDK for.NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>建立 Microsoft Azure 專案

啟動 Visual Studio 系統管理員權限。 偵錯在本機的應用程式使用 Azure 計算模擬器需要系統管理員權限。

在上**檔案**功能表上，按一下**新增**，然後按一下**專案**。 從**已安裝的範本**，在 Visual C# 中，按一下**雲端**，然後按一下  **Windows Azure 雲端服務**。 將專案命名為"AzureApp"，然後按一下**確定**。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

在 [**新的 Windows Azure 雲端服務**] 對話方塊中，按兩下**背景工作角色**。 保留預設名稱 ("WorkerRole1")。 此步驟會將背景工作角色加入至方案。 按一下 [確定 **Deploying Office Solutions**]。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

建立 Visual Studio 方案包含兩個專案：

- &quot;AzureApp&quot;定義的角色和設定 Azure 應用程式。
- &quot;WorkerRole1&quot;包含背景工作角色的程式碼。

一般情況下，Azure 應用程式都可以包含多個角色，雖然本教學課程使用單一角色。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>新增 Web API 和 OWIN 套件

從**工具**功能表上，按一下**NuGet 套件管理員**，然後按一下**Package Manager Console**。

在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>新增 HTTP 端點

在 [方案總管] 中，展開 AzureApp 專案。 展開 角色 節點，以滑鼠右鍵按一下 WorkerRole1，然後選取**屬性**。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

按一下 **端點**，然後按一下**新增端點**。

在 **通訊協定**下拉式清單中，選取 「 http 」。 在 **公用連接埠**並**私人連接埠**，輸入 80。 這些連接埠號碼可以不同。 公用連接埠是用戶端使用的項目時，它們將要求傳送至角色。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>設定 Web API 的自我裝載

在 方案總管 中，以滑鼠右鍵按一下 WorkerRole1 專案，然後選取**新增** / **類別**若要加入新的類別。 將類別命名為 `Startup` 。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

您可以將所有未定案程式碼，此檔案中取代為下列：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>新增 Web API 控制器

接下來，新增 Web API 控制器類別。 以滑鼠右鍵按一下 WorkerRole1 專案，然後選取**新增** / **類別**。 將類別命名為 TestController。 您可以將所有未定案程式碼，此檔案中取代為下列：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

為了簡單起見，此控制站只會定義兩個 GET 方法會傳回純文字。

## <a name="start-the-owin-host"></a>啟動 OWIN 主機

開啟 WorkerRole.cs 檔案。 這個類別會定義啟動及停止背景工作角色時所執行的程式碼。

新增下列 using 陳述式：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

新增**IDisposable**成員，才能`WorkerRole`類別：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

在 `OnStart`方法，加入下列的程式碼，以啟動主機：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start**方法會啟動 OWIN 主機。 名稱`Startup`類別是型別參數的方法。 依照慣例，主機會呼叫`Configure`這個類別的方法。

覆寫`OnStop`處置*\_應用程式*執行個體：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

以下是 WorkerRole.cs 的完整程式碼：

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

建置方案，並按下 F5 以在 Azure 計算模擬器在本機執行應用程式。 根據您的防火牆設定，您可能需要允許通過防火牆的模擬器。

> [!NOTE]
> 如果您收到類似下列的例外狀況，請參閱[此部落格文章](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)的因應措施。 「 無法載入檔案或組件 ' Microsoft.Owin，版本 = 2.0.2.0，Culture = neutral，PublicKeyToken = 31bf3856ad364e35' 或其中一個相依性。 找到的組件資訊清單定義不符合組件參考。 (來自 HRESULT 的例外狀況： 0x80131040) 」


計算模擬器會將本機的 IP 位址指派給端點。 您可以藉由檢視計算模擬器 UI 中找到的 IP 位址。 在工作列通知區域中的 [模擬器] 圖示上按一下滑鼠右鍵，然後選取**顯示計算模擬器 UI**。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

尋找 [服務部署，部署 [識別碼]，服務詳細資料] 下的 IP 位址。 開啟網頁瀏覽器並瀏覽至 http://<em>地址</em>/測試/1，其中<em>地址</em>是由計算模擬器; 指派的 IP 位址，例如`http://127.0.0.1:80/test/1`。 您應該會看到來自 Web API 控制器的回應：

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>部署到 Azure

此步驟中，您必須有 Azure 帳戶。 如果您還沒有做，您可以建立免費的試用帳戶，只需要幾分鐘的時間。 如需詳細資訊，請參閱 < [Microsoft Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)。

在 [方案總管] 中，以滑鼠右鍵按一下 AzureApp 專案。 選取 [發行]。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

如果您未登入您的 Azure 帳戶，按一下**登入**。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

您已登入後，選擇訂用帳戶，然後按一下**下一步**。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

輸入雲端服務的名稱，然後選擇的區域。 按一下 [建立] 。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

按一下 [發行] 。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

[Azure 活動記錄] 視窗會顯示部署進度。 部署應用程式時，瀏覽至 http://appname.cloudapp.net/test/1 。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>其他資源

- [Katana 專案概觀](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [在 GitHub 上的 Katana 專案](https://github.com/aspnet/AspNetKatana)
