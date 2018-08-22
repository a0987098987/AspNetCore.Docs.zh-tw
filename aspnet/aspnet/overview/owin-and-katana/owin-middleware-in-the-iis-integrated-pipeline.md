---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: 在 IIS 中的 OWIN 中介軟體整合式管線 |Microsoft Docs
author: Praburaj
description: 這篇文章會示範如何執行 OWIN 中介軟體元件 (OMCs) 在 IIS 整合式管線中，以及如何設定管線事件 OMC 上執行。 您應該...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 56bd145688e1ab0a70710cc80cb8f5fa10ee8968
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834248"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>IIS 整合式管線中的 OWIN 中介軟體
====================
藉由[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 這篇文章會示範如何執行 OWIN 中介軟體元件 (OMCs) 在 IIS 整合式管線中，以及如何設定管線事件 OMC 上執行。 您應該先檢閱[的專案 Katana 概觀](an-overview-of-project-katana.md)並[OWIN 啟動類別偵測](owin-startup-class-detection.md)之前先閱讀本教學課程。 本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Chris Ross、 Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。


雖然[OWIN](an-overview-of-project-katana.md)中介軟體元件 (OMCs) 主要用於在伺服器無從驗證管線中執行，就可以在 IIS 整合式管線以及執行 OMC (**傳統模式是*不*支援**)。 OMC 可以成為以搭配 IIS 整合式管線從套件管理員主控台 (PMC) 中安裝下列套件：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

這表示，所有的應用程式架構，即使尚無法執行 IIS 和 System.Web，外部可受益於現有的 OWIN 中介軟體元件。 

> [!NOTE]
> 所有`Microsoft.Owin.Security.*`套件隨附於 Visual Studio 2013 中新的身分識別系統 (例如： Cookie、 Microsoft 帳戶、 Google、 Facebook、 Twitter[持有人權杖](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)，OAuth 授權伺服器，JWT，Azure Active目錄和 Active directory 同盟服務） 為 OMCs，撰寫，而且可用在自我裝載和 IIS 裝載案例中。

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>OWIN 中介軟體會在 IIS 整合式管線中執行的方式

OWIN 主控台應用程式，建置使用應用程式管線[啟動組態](owin-startup-class-detection.md)由使用加入元件的順序設定`IAppBuilder.Use`方法。 也就是在 OWIN 管線[Katana](an-overview-of-project-katana.md)執行階段將會註冊它們使用的順序處理 OMCs `IAppBuilder.Use`。 在 IIS 整合式管線要求管線組成[HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx)這類訂閱一組預先定義的管線事件[BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)， [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)， [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)等等。

如果我們比較的 OMC [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx)在 ASP.NET 世界中，必須註冊 OMC 正確的預先定義的管線事件。 比方說，HttpModule`MyModule`傳入的要求時便會叫用[AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)管線中的階段：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

為了讓 OMC 參與此相同、 以事件為基礎的執行順序時， [Katana](an-overview-of-project-katana.md)執行階段程式碼掃描[啟動設定](owin-startup-class-detection.md)，每個中介軟體元件，以訂閱整合式的管線的事件。 例如，下列 OMC 和註冊程式碼可讓您查看中介軟體元件的預設事件註冊。 (如需詳細指示，建立 OWIN 啟動類別，請參閱 < [OWIN 啟動類別偵測](owin-startup-class-detection.md)。)

1. 建立空的 web 應用程式專案並將它命名**owin2**。
2. 從套件管理員主控台 (PMC)，執行下列命令： 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. 新增`OWIN Startup Class`並將它命名`Startup`。 產生的程式碼取代為的下列 （變更已醒目提示）：  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. 按 f5 鍵執行應用程式。

啟動設定會設定具有三個中介軟體元件，前兩個顯示的診斷資訊和回應事件的最後一個 （以及也顯示診斷資訊） 的管線。 `PrintCurrentIntegratedPipelineStage`方法會顯示此中介軟體會在叫用的整合式的管線事件和訊息。 輸出視窗會顯示下列資訊：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana 執行階段對應每個 OWIN 中介軟體元件[PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)根據預設，對應到 IIS 管線事件[PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)。

## <a name="stage-markers"></a>階段標記

您可以藉由使用執行管線的特定階段 OMCs`IAppBuilder UseStageMarker()`擴充方法。 若要執行的中介軟體元件的一組特定的階段期間，插入後的最後一個元件的階段標誌是在註冊期間的集合。 在管線的階段中，您可以執行的中介軟體的規則，而且必須在執行順序元件 （稍後在本教學課程中說明的規則）。 新增`UseStageMarker`方法，以`Configuration`程式碼，如下所示：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)`呼叫所設定 （在此案例中，我們的兩個診斷元件） 的所有先前已註冊的中介軟體元件，可在管線的 [驗證] 階段上執行。 （這會顯示 診斷並回應要求） 的最後一個中介軟體元件會在上執行`ResolveCache`階段 ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx)事件)。

按 f5 鍵執行應用程式。[輸出] 視窗顯示下列資訊：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>階段標記規則

Owin 中介軟體元件 (OMC) 可以設定為在下列的 OWIN 管線階段事件時執行：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. 根據預設，在最後一個事件都會執行 OMCs (`PreHandlerExecute`)。 這就是為什麼我們第一次的範例程式碼顯示"PreExecuteRequestHandler 」。
2. 您可以使用`app.UseStageMarker`中所列的方法，以註冊 OWIN 管線的每一個階段更早版本，執行 OMC`PipelineStage`列舉。
3. OWIN 管線和 IIS 管線經過排序，因此呼叫`app.UseStageMarker`必須按照順序排列。 您無法將事件處理常式設定在前面的最後一個事件，以註冊事件`app.UseStageMarker`。 例如，*之後*呼叫：

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   若要呼叫`app.UseStageMarker`傳遞`Authenticate`或`PostAuthenticate`將無法接受，而且會擲回任何例外狀況。 在最新的階段，其預設值是執行 OMCs `PreHandlerExecute`。 階段標記用來使它們之前執行。 如果您指定順序的階段標記，我們會四捨五入到較早的標記。 換句話說，新增的階段標誌會說 「 不晚於階段 X 執行 」。 在 OWIN 管線中加入他們最早的階段標誌 OMC 的執行。
4. 若要呼叫的最早階段`app.UseStageMarker`wins。 例如，如果您切換的順序`app.UseStageMarker`呼叫從先前的範例：

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   [輸出] 視窗會顯示： 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   所有執行中 OMCs`AuthenticateRequest`階段中，因為最後一個 OMC 向`Authenticate`事件，和`Authenticate`事件之前的所有其他事件。
