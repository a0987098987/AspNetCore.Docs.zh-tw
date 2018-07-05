---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN 啟動類別偵測 |Microsoft Docs
author: Praburaj
description: 本教學課程會示範如何設定載入的 OWIN 啟動類別。 如需有關 OWIN 的詳細資訊，請參閱 < 概觀的 Katana 專案。 本教學課程是...
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0a4b87192296054bf6aef6c9406c64f19677a061
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828289"
---
<a name="owin-startup-class-detection"></a>OWIN 啟動類別偵測
====================
藉由[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何設定載入的 OWIN 啟動類別。 如需有關 OWIN 的詳細資訊，請參閱[概觀的 Katana 專案](an-overview-of-project-katana.md)。 本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。
> 
> ## <a name="prerequisites"></a>必要條件
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN 啟動類別偵測

 每個 OWIN 應用程式有啟動類別，您可在其中指定應用程式管線的元件。 有許多種，您可以連接您的啟動類別的執行階段、 根據裝載模型選擇 （OwinHost、 IIS 和 IIS Express）。 本教學課程中所示的 startup 類別可以用於每個裝載的應用程式。 您可以連接的啟動類別裝載執行階段使用其中一種方法：  

1. **命名慣例**: Katana 會尋找名為類別`Startup`比對組件名稱或全域命名空間的命名空間中。
2. **OwinStartup 屬性**： 這是大部分開發人員將移至指定的啟動類別的方法。 下列屬性會設定為啟動類別`TestStartup`類別中`StartupDemo`命名空間。 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   `OwinStartup`屬性會覆寫的命名慣例。 您也可以使用這個屬性指定好記的名稱，不過，使用易記的名稱需要您同時使用`appSetting`組態檔中的項目。
3. **在組態檔中的 appSetting 元素**:`appSetting`項目會覆寫`OwinStartup`屬性和命名慣例。 您可以有多個啟動類別 (每個使用`OwinStartup`屬性)，並設定啟動類別將會載入組態檔使用類似下列的標記中：  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   也可以使用下列機碼，明確指定的啟動類別和組件： 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   下列 XML 組態檔中的指定的好記的啟動類別名稱`ProductionConfiguration`。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   必須使用上述的標記取代為下列`OwinStartup`屬性指定好記的名稱，並造成`ProductionStartup2`類別來執行。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. 若要停用新增 OWIN 啟動探索`appSetting owin:AutomaticAppStartup`值為`"false"`web.config 檔案中。

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>建立 ASP.NET Web 應用程式使用 OWIN 啟動

1. 建立空的 Asp.Net web 應用程式並將它命名**StartupDemo**。 -安裝`Microsoft.Owin.Host.SystemWeb`使用 NuGet 套件管理員。 從**工具**功能表上，選取**程式庫套件管理員**，然後**Package Manager Console**。 輸入下列命令：  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. 新增 OWIN 啟動類別。 Visual Studio 2013 中以滑鼠右鍵按一下專案，然後選取**加入類別**。-在**加入新項目**對話方塊中，輸入*OWIN*在搜尋欄位中，並將 Startup.cs 中，名稱然後按一下**新增**。  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   您想要新增的下次*Owin 啟動類別*，則會在可從**新增**功能表。  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   或者，您可以以滑鼠右鍵按一下專案，並選取**新增**，然後選取**新項目**，然後選取**Owin 啟動類別**。  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- 取代產生的程式碼*Startup.cs*以下列檔案：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  `app.Use` Lambda 運算式用來註冊指定的中介軟體元件至 OWIN 管線。 在此情況下我們設定了連入要求的連入要求回應之前的記錄。 `next`參數是委派 ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [工作](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) 管線中的下一個元件。 `app.Run` Lambda 運算式的內送要求管線，並提供回應的機制。
     > [!NOTE]
     > 在上述程式碼中，我們有註解`OwinStartup`屬性，我們依賴執行名為類別的慣例`Startup`。-按***F5***執行應用程式。 按 重新整理數次。  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  注意： 本教學課程中的映像中顯示的數字不會符合您看到的數字。 毫秒數的字串用來顯示新的回應，當您重新整理頁面。  
  您可以看到中的追蹤資訊**輸出**視窗。  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>新增更多的啟動類別

在本節中，我們將新增另一個啟動類別。 您可以將多個 OWIN 啟動類別新增至您的應用程式。 例如，您可能要建立開發、 測試和生產環境的啟動類別。

1. 建立新的 OWIN 啟動類別並將它命名`ProductionStartup`。
2. 使用下列程式碼取代產生的程式碼：

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. 按下控制項 F5 執行應用程式。 `OwinStartup`屬性會指定在執行生產環境的啟動類別。  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. 建立另一個的 OWIN 啟動類別並將它命名`TestStartup`。
5. 使用下列程式碼取代產生的程式碼：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   `OwinStartup`上述的屬性多載會指定`TestingConfiguration`作為*易記*Startup 類別的名稱。
6. 開啟*web.config*檔案，並新增 OWIN 應用程式啟動金鑰，指定啟動類別的易記名稱：

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. 按下控制項 F5 執行應用程式。 應用程式設定項目會優先於，與測試組態執行。  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. 移除*易記*名稱從`OwinStartup`屬性中`TestStartup`類別。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. 取代 OWIN 應用程式的啟動金鑰，在*web.config*以下列檔案：

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. 還原`OwinStartup`Visual Studio 所產生的預設屬性程式碼的每個類別中的屬性：  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    每個以下的 OWIN 應用程式的啟動金鑰會導致執行的生產環境類別。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    最後一個的啟動金鑰指定啟動組態方法。 下列的 OWIN 應用程式啟動金鑰可讓您變更組態類別的名稱`MyConfiguration`。

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>使用 Owinhost.exe

1. 以下列標記取代 Web.config 檔案：  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Wins 的最後一個金鑰，因此在此情況下`TestStartup`指定。
2. 安裝 Owinhost 從 PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. 瀏覽至應用程式資料夾 (包含的資料夾*Web.config*檔案)，並在命令提示字元和型別： 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   [命令] 視窗會顯示： 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. 啟動 URL 的瀏覽器`http://localhost:5000/`。  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost 採用上面所列的啟動慣例。
5. 在 [命令] 視窗中，按 Enter 以結束 OwinHost。
6. 在 `ProductionStartup`類別中，新增下列 OwinStartup 屬性指定的易記名稱*ProductionConfiguration*。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. 在命令提示字元並輸入： 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   載入實際執行啟動類別。  
    ![](owin-startup-class-detection/_static/image9.png)  
   我們的應用程式有多個啟動類別，並在此範例中我們已延後載入執行階段之前的啟動類別。
8. 測試下列執行階段啟動選項：

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
