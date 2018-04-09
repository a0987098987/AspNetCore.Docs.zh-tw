---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN 啟動類別偵測 |Microsoft 文件
author: Praburaj
description: 本教學課程會示範如何設定哪些 OWIN 啟動已載入類別。 如需有關 OWIN 的詳細資訊，請參閱的專案 Katana 概觀。 本教學課程是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 33d2745b24387419e5614c62c2d46948427b242a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="owin-startup-class-detection"></a>OWIN 啟動類別偵測
====================
由[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何設定哪些 OWIN 啟動已載入類別。 如需有關 OWIN 的詳細資訊，請參閱[的專案 Katana 概觀](an-overview-of-project-katana.md)。 本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。
> 
> ## <a name="prerequisites"></a>必要條件
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN 啟動類別偵測

 每個 OWIN 應用程式已啟動類別讓您指定的應用程式管線元件。 有不同的方式，您可以使用執行階段連接啟動類別，根據裝載模型選擇 （OwinHost、 IIS 和 IIS Express）。 在每個主控的應用程式可啟動類別顯示在本教學課程。 您可以連接啟動類別裝載執行階段使用下列其中一種方法：  

1. **命名慣例**: Katana 會尋找名為類別`Startup`相符的組件名稱或全域命名空間的命名空間中。
2. **OwinStartup 屬性**： 這是大部分的開發人員會指定啟動類別的方法。 下列屬性會設定為啟動類別`TestStartup`類別`StartupDemo`命名空間。 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   `OwinStartup`屬性會覆寫的命名慣例。 您也可以使用這個屬性指定易記名稱，不過，使用易記名稱必須也使用`appSetting`組態檔中的項目。
3. **在組態檔中的 appsetting owin： 項目**:`appSetting`元素會覆寫`OwinStartup`屬性和命名慣例。 您可以有多個啟動類別 (每一個使用`OwinStartup`屬性) 並設定啟動類別將會載入組態檔使用類似下列的標記中：  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   也可以使用下列機碼，明確指定啟動類別和組件： 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   下列 XML 組態檔中的指定的好記的啟動類別名稱`ProductionConfiguration`。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   必須使用上述的標記，以下列`OwinStartup`屬性會指定好記的名稱，並造成`ProductionStartup2`類別來執行。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. 若要停用 OWIN 啟動探索新增`appSetting owin:AutomaticAppStartup`值是`"false"`web.config 檔案中。

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>建立 ASP.NET Web 應用程式使用 OWIN 啟動

1. 建立空的 Asp.Net web 應用程式並將其命名**StartupDemo**。 -安裝`Microsoft.Owin.Host.SystemWeb`透過 NuGet 套件管理員。 從**工具**功能表上，選取**程式庫套件管理員**，然後**Package Manager Console**。 輸入下列命令：  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. 新增 OWIN 啟動類別。 Visual Studio 2013 中以滑鼠右鍵按一下專案，然後選取**加入類別**。-在**加入新項目**對話方塊方塊中，輸入*OWIN*搜尋欄位中，以及變更要 Startup.cs，名稱然後按一下 **新增**。  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   下次您想要新增*Owin 啟動 「 類別*，其可從**新增**功能表。  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   或者，您可以以滑鼠右鍵按一下專案，並選取**新增**，然後選取**新項目**，然後選取**Owin 啟動 「 類別**。  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- 取代產生的程式碼*Startup.cs*以下列檔案：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  `app.Use` Lambda 運算式用來註冊指定的中介軟體元件至 OWIN 管線。 在此情況下我們正在設定的連入要求的連入要求回應之前的記錄。 `next`參數是委派 ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [工作](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) 管線中的下一個元件。 `app.Run` Lambda 運算式內送要求管線連結，並提供回應機制。
     > [!NOTE]
     > 上述程式碼我們已標記為註解`OwinStartup`屬性和我們依賴執行名為類別的慣例`Startup`。-按***F5***執行應用程式。 點擊 重新整理數次。  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  注意： 在本教學課程的影像中顯示的數字不會符合您看到。 毫秒字串用來顯示新的回應，當您重新整理頁面。  
  您可以查看中的追蹤資訊**輸出**視窗。  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>新增多個啟動類別

這一節中，我們會將新增另一個啟動類別。 您可以將多個 OWIN 啟動類別加入您的應用程式。 例如，您可以建立啟動的開發、 測試及實際執行的類別。

1. 建立新的 OWIN 啟動 「 類別並將其命名`ProductionStartup`。
2. 使用下列程式碼取代產生的程式碼：

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. 按 f5 執行應用程式的控制鍵。 `OwinStartup`屬性會指定生產啟動類別執行。  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. 建立另一個 OWIN 啟動 「 類別並將其命名`TestStartup`。
5. 使用下列程式碼取代產生的程式碼：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   `OwinStartup`上述屬性多載會指定`TestingConfiguration`為*易記*啟動類別的名稱。
6. 開啟*web.config*檔案，然後加入 OWIN 應用程式啟動金鑰會指定啟動類別的易記名稱：

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. 按 f5 執行應用程式的控制鍵。 應用程式設定項目會優先者列，以及測試組態執行。  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. 移除*易記*名稱從`OwinStartup`屬性`TestStartup`類別。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. 取代 OWIN 應用程式的啟動金鑰中*web.config*以下列檔案：

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. 還原`OwinStartup`屬性中由 Visual Studio 產生的預設屬性程式碼的每個類別：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    每個 OWIN 應用程式的啟動金鑰下方將會造成執行的生產環境類別。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    最後一個啟動金鑰指定啟動組態方法。 下列的 OWIN 應用程式啟動金鑰可讓您變更的設定類別的名稱`MyConfiguration`。

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>使用 Owinhost.exe

1. 以下列標記取代 Web.config 檔案：  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Wins 的最後一個索引鍵，因此在此情況下`TestStartup`指定。
2. 從 PMC 安裝 Owinhost: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. 瀏覽至應用程式資料夾 (資料夾包含*Web.config*檔案)，並在命令提示字元和型別： 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   [命令] 視窗會顯示： 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. URL 啟動瀏覽器`http://localhost:5000/`。  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost 接受一個以上所列的啟動慣例。
5. 在 [命令] 視窗中，按下 Enter 以結束 OwinHost。
6. 在`ProductionStartup`類別中，加入下列的 OwinStartup 屬性指定的易記名稱*ProductionConfiguration*。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. 在命令提示字元和型別中： 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   載入實際執行啟動類別。  
    ![](owin-startup-class-detection/_static/image9.png)  
   我們的應用程式有多個啟動類別，並在此範例中我們已延後載入執行階段之前啟動類別。
8. 測試下列執行階段啟動選項：

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
