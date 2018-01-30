---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: "在 IIS 中的 OWIN 中介軟體整合管線 |Microsoft 文件"
author: Praburaj
description: "本文將說明如何執行 OWIN 中介軟體元件 (OMCs) 在 IIS 整合式管線中，如何設定管線事件 OMC 上並執行。 您應該..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5f6ed1ae0309e9bdd3ca4ae229195835f20bc729
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>IIS integrated 管線中的 OWIN 中介軟體
====================
由[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)

> 本文將說明如何執行 OWIN 中介軟體元件 (OMCs) 在 IIS 整合式管線中，如何設定管線事件 OMC 上並執行。 您應該檢閱[的專案 Katana 概觀](an-overview-of-project-katana.md)和[OWIN 啟動類別偵測](owin-startup-class-detection.md)閱讀本教學課程之前。 本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Chris Ross、 Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。


雖然[OWIN](an-overview-of-project-katana.md)中介軟體元件 (OMCs) 主要用於在伺服器無從驗證管線中執行，就可以在 IIS integrated 管線以及中執行 OMC (**傳統模式是*不*支援**)。 OMC 對 IIS integrated 管線搭配使用封裝管理員主控台 (PMC) 從安裝下列套件：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

這表示，所有應用程式架構，即使尚無法執行 IIS 和 System.Web，外部可受益於現有的 OWIN 中介軟體元件。 

> [!NOTE]
> 所有`Microsoft.Owin.Security.*`封裝隨附於 Visual Studio 2013 中新的身分識別系統 (例如： Cookie、 Microsoft 帳戶、 Google、 Facebook、 Twitter [Bearer Token](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)，OAuth 授權伺服器，JWT，Azure Active目錄中，與 Active directory 同盟服務） 為 OMCs，編寫，並可在自我裝載和 IIS 裝載的案例。

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>OWIN 中介軟體會在 IIS Integrated 管線中執行的方式

OWIN 主控台應用程式，應用程式管線使用建置[啟動設定](owin-startup-class-detection.md)設定元件會使用加入的順序`IAppBuilder.Use`方法。 也就是說，OWIN 管線中的[Katana](an-overview-of-project-katana.md)執行階段將其註冊使用的順序處理 OMCs `IAppBuilder.Use`。 在 IIS integrated 管線要求管線組成[HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx)例如訂閱一組預先定義的管線事件[BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)， [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)， [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)等等。

如果我們比較的 OMC [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) ASP.NET 世界 OMC 必須註冊到正確的預先定義的管線的事件。 例如，HttpModule`MyModule`會叫用時要求送入[AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)管線階段中：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

為了讓 OMC 參與此相同且以事件為基礎的執行順序[Katana](an-overview-of-project-katana.md)透過執行階段程式碼掃描[啟動設定](owin-startup-class-detection.md)中, 介軟體元件的每個訂閱整合的管線的事件。 例如，下列 OMC 及註冊程式碼可讓您看到的中介軟體元件的預設事件註冊。 (如需詳細指示，建立 OWIN 啟動類別時，請參閱[OWIN 啟動類別偵測](owin-startup-class-detection.md)。)

1. 建立空 web 應用程式專案並將其命名**owin2**。
2. 從封裝管理員主控台 (PMC)，執行下列命令： 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. 新增`OWIN Startup Class`並將其命名`Startup`。 產生的程式碼替換成的下列 （所做的變更會反白顯示）：  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. 按 F5 執行應用程式。

啟動組態來設定具有三個中介軟體元件，前兩個顯示的診斷資訊及回應事件的最後一個 （和也顯示診斷資訊） 的管線。 `PrintCurrentIntegratedPipelineStage`方法會顯示在叫用此中介軟體的整合式的管線事件和訊息。 [輸出] 視窗會顯示下列資訊：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Katana 執行階段對應每個 OWIN 中介軟體元件[PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)根據預設，它會對應到 IIS 管線事件[PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)。

## <a name="stage-markers"></a>階段標記

您可以利用執行管線的特定階段 OMCs`IAppBuilder UseStageMarker()`擴充方法。 若要在特定階段期間執行一組的中介軟體元件，請插入之後的最後一個元件的階段標誌是在註冊期間的集合。 在管線的哪個階段中，您可以執行中介軟體的規則，而且必須在執行順序元件 （稍後在本教學課程中的規則會說明）。 新增`UseStageMarker`方法`Configuration`程式碼如下所示：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

`app.UseStageMarker(PipelineStage.Authenticate)`呼叫會設定 （在此情況下，我們有兩個診斷元件） 的所有先前註冊的中介軟體元件在管線的驗證階段上執行。 在上執行的最後一個中介軟體元件 （這會顯示診斷和回應的要求）`ResolveCache`階段 ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx)事件)。

按 F5 執行應用程式。[輸出] 視窗顯示下列資訊：

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>階段標記規則

Owin 中介軟體元件 (OMC) 可以設定為在下列的 OWIN 管線階段事件時執行：

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. 根據預設，OMCs 執行在最後一個事件 (`PreHandlerExecute`)。 這就是為什麼我們第一個範例程式碼會顯示 「 PreExecuteRequestHandler"。
2. 您可以使用`app.UseStageMarker`中所列的方法，以註冊 OWIN 管線的任何階段之前，執行 OMC`PipelineStage`列舉。
3. OWIN 管線的管線與 IIS 管線經過排序，因此呼叫`app.UseStageMarker`必須按照順序。 您無法將事件處理常式設定事件之前的最後一個事件以向`app.UseStageMarker`。 例如，*之後*呼叫：

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

 呼叫`app.UseStageMarker`傳遞`Authenticate`或`PostAuthenticate`不會接受，而且將會擲回任何例外狀況。 在最新的階段，其預設值是執行 OMCs `PreHandlerExecute`。 階段標記用來使它們執行早。 如果您指定順序階段標記，我們會四捨五入至較早的標記。 換句話說，新增的階段標誌會說 「 不晚於階段 X 執行 」。 在 OWIN 管線中加入後它們最早的階段標誌 OMC 的執行。
4. 若要呼叫的最早階段`app.UseStageMarker`wins。 例如，如果您切換的順序`app.UseStageMarker`呼叫我們先前的範例：

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

 [輸出] 視窗會顯示： 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

 所有執行中 OMCs`AuthenticateRequest`階段，因為最後一個 OMC 向`Authenticate`事件，而`Authenticate`事件位於所有其他事件。
