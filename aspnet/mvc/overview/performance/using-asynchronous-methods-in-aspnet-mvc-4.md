---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: "ASP.NET MVC 4 中使用非同步方法 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您建置使用 Visual Studio Express 2012 for Web，也就是可用 ve 非同步 ASP.NET MVC Web 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: b4280c6ab1b6d8d2ceaa7cef14fce94ab8c6df53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>使用 ASP.NET MVC 4 中的非同步方法
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置非同步的 ASP.NET MVC Web 應用程式使用的基本概念[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)，這是 Microsoft Visual Studio 的免費版本。 您也可以使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)。

> Github 上的本教學課程提供完整的範例是[https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx)中組合的類別[.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)可讓您撰寫非同步動作方法的傳回型別的物件[工作&lt;ActionResult&gt;](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 引進稱為 「 非同步程式設計概念[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)並支援 ASP.NET MVC 4[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 工作由**工作**類型和相關的類型中[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)命名空間。 .NET Framework 4.5 為基礎的這個非同步支援[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，讓使用[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)比前一個較複雜的物件非同步方法。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字是用以表示某段程式碼應該以非同步方式等候一段程式碼的語法縮寫。 [非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字表示提示可讓您將方法標記為以工作為基礎的非同步方法。 組合**await**，**非同步**，而**工作**物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。 呼叫非同步方法的新模型*工作架構非同步模式*(**點選**)。 本教學課程假設您已熟悉非同步程式設計使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間。

如需有關使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱下列參考。

- [.NET 中的技術白皮書： 非同步](https://go.microsoft.com/fwlink/?LinkId=204844)
- [非同步/等候常見問題集](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio 非同步程式設計](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>執行緒集區處理要求的方式

在 web 伺服器上，.NET Framework 會維護用於 ASP.NET 要求提供服務的執行緒集區。 當要求到達時，集區的執行緒會分派處理該要求。 以同步方式處理要求時，處理要求的執行緒忙碌時要求正在處理，且執行緒無法服務其他要求。   
  
這可能不是問題，因為執行緒集區可以由大到足以應付許多的忙碌執行緒。 不過，執行緒集區中的執行緒數目是限制 （.NET 4.5 的最大的預設值是 5000）。 在大型應用程式具有高的並行處理長時間執行要求的所有可用的執行緒可能忙碌中。 這種狀況稱為執行緒資源用盡。 當達到這種狀況時，web 伺服器要求加入佇列。 如果要求佇列額滿，網頁伺服器會拒絕要求的 HTTP 503 狀態 （伺服器忙碌中）。 CLR 執行緒集區會限制對新的執行緒資料隱碼攻擊。 如果並行暴增 （也就是您的網站突然可以取得大量的要求），而且所有可用的要求執行緒忙碌因為後端呼叫，所以含高延遲，限制的執行緒資料隱碼速率可以讓應用程式非常不回應。 此外，每個新執行緒加入執行緒集區具有 （例如 1 MB 的堆疊記憶體) 的額外負荷。 Web 應用程式使用服務高延遲呼叫同步方法，執行緒集區成長至.NET 4.5 預設最大值為 5，000 執行緒會取用大約 5 GB 更多的記憶體比應用程式可以相同的服務要求使用非同步方法，而且只有 50 的執行緒。 在您執行非同步工作時，您不一定正在使用的執行緒。 例如，當您進行非同步的 web 服務要求時，ASP.NET 將不會使用任何執行緒之間**非同步**方法呼叫和**await**。 使用含高延遲為要求提供服務的執行緒集區可能會導致大量的記憶體耗用量和伺服器硬體使用率不佳。

## <a name="processing-asynchronous-requests"></a>處理非同步要求

看見大量的並行要求，在啟動時，或具有暴增負載 （其中增加並行突然） 的 web 應用程式，讓這些 web 服務呼叫非同步將會增加您的應用程式的回應能力。 非同步要求所需的時間來處理與同步要求相同的數量。 比方說，如果要求會提出 web 服務呼叫，要求兩秒來完成，要求會採用兩秒是否以同步還是非同步方式執行。 不過，在非同步呼叫，執行緒不會封鎖回應其他要求，同時等候完成的第一個要求。 因此，非同步要求可以避免要求佇列和執行緒集區成長時叫用長時間執行作業的許多並行要求。

## <a id="ChoosingSyncVasync"></a>選擇同步或非同步動作方法

本節列出何時使用同步或非同步動作方法的指導方針。 這些只是一般指引。檢查每個應用程式判斷採用非同步方法是否協助提高效能。

一般情況下，使用同步方法，在下列情況：

- 作業會簡單或對於執行短期。
- 簡單是比效率重要。
- 這些作業有主要是 CPU 作業，而非作業牽涉到大量磁碟或網路額外負荷。 受限於 CPU 之作業上使用非同步動作方法並沒有好處會增加負擔。

 一般情況下，使用非同步方法，在下列情況：

- 您正在呼叫可以透過非同步方法，來取用的服務，而且您要使用.NET 4.5 或更高版本。
- 作業會受限於網路或我 I/O 繫結而不是 cpu-bound。
- 平行處理原則是更重要的是比簡化程式碼。
- 您想要提供一套機制，可讓使用者取消長時間執行要求。
- 當優點切換執行緒出加權內容切換的成本。 一般情況下，您應該方法設為非同步如果同步方法會在 ASP.NET 要求執行緒上時不執行任何工作。 藉由呼叫非同步，ASP.NET 要求執行緒不停止不任何工作，同時等候完成的 web 服務要求。
- 測試顯示出封鎖作業是效能瓶頸的網站和 IIS 可以服務多個要求，使用這些封鎖呼叫的非同步方法。

 可下載的範例示範如何有效地使用非同步動作方法。 提供的範例被設計來提供簡單的示範，使用.NET 4.5 的 ASP.NET MVC 4 中非同步程式設計。 此範例不是在 ASP.NET MVC 中的非同步程式設計參考架構。 範例程式會呼叫[ASP.NET Web API](../../../web-api/index.md)方法會接著呼叫[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模擬長時間執行的 web 服務呼叫。 大部分的實際執行應用程式不會顯示使用非同步動作方法可帶來明顯好處。   
  
少數應用程式需要的所有動作方法都是非同步。 通常，將一些同步動作方法轉換為非同步方法會提供最佳效率增加所需的工作量。

## <a id="SampleApp"></a>範例應用程式

您可以下載範例應用程式從[https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET)上[GitHub](https://github.com/)站台。 儲存機制包含三個專案：

- *Mvc4Async*： 包含在本教學課程使用的程式碼的 ASP.NET MVC 4 專案。 它會呼叫 Web API **WebAPIpgw**服務。
- *WebAPIpgw*： 實作 ASP.NET MVC 4 Web API 專案`Products, Gizmos and Widgets`控制站。 它提供的資料*WebAppAsync*專案和*Mvc4Async*專案。
- *WebAppAsync*： 另一個教學課程中使用的 ASP.NET Web Form 專案。

## <a id="GizmosSynch"></a>Gizmos 同步動作方法

 下列程式碼會示範`Gizmos`同步動作方法，用來顯示一份 gizmos。 （這個發行項，gizmo 是虛構的機械裝置）。 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

下列程式碼會示範`GetGizmos`gizmo 服務的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

`GizmoService GetGizmos`方法會將 URI 傳遞到 ASP.NET Web API HTTP 服務會傳回一份 gizmos 資料。 *WebAPIpgw*專案包含 Web 應用程式開發介面的實作`gizmos, widget`和`product`控制站。  
下圖顯示範例專案 gizmos 檢視。

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>建立非同步 Gizmos 動作方法

此範例會使用新[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)和[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字 （適用於.NET 4.5 和 Visual Studio 2012），讓編譯器會負責維護所需的複雜的轉換非同步程式設計。 編譯器可讓您撰寫程式碼會使用 C# 的同步的控制流程建構，編譯器會自動套用必要使用回呼，以避免封鎖執行緒的轉換。

下列程式碼會示範`Gizmos`同步的方法和`GizmosAsync`非同步方法。 如果您的瀏覽器支援[HTML 5`<mark>`元素](http://www.w3.org/wiki/HTML/Elements/mark)，您會看到中的變更`GizmosAsync`以黃色反白顯示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 下列變更套用至允許`GizmosAsync`非同步。

- 方法會標示[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，它會告訴編譯器產生的組件主體的回呼，並自動建立`Task<ActionResult>`傳回。
- &quot;非同步&quot;已附加至的方法名稱。 將"Async"附加就不需要但慣例撰寫的非同步方法時。
- 傳回型別已經從`ActionResult`至`Task<ActionResult>`。 傳回型別`Task<ActionResult>`代表進行中的工作，並提供與控制代碼，以等候非同步作業完成的方法的呼叫者。 在此情況下，呼叫端是 web 服務。 `Task<ActionResult>`代表進行中工作的結果`ActionResult.`
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至 web 服務呼叫。
- 非同步的 web 服務 API 呼叫 (`GetGizmosAsync`)。

內部`GetGizmosAsync`方法主體另一個非同步方法，`GetGizmosAsync`呼叫。 `GetGizmosAsync`立即傳回`Task<List<Gizmo>>`，最終將會完成時有可用資料。 您不想要執行任何其他動作，直到您 gizmo 資料，因為程式碼會等候工作 (使用**await**關鍵字)。 您可以使用**await**以註解的方法中的關鍵字**非同步**關鍵字。

**Await**關鍵字不會封鎖執行緒，直到工作完成為止。 它註冊方法的其餘部分，做為回呼，以於工作，並立即傳回。 當最後完成等候的工作時，它將會叫用回撥，並因此繼續中斷的位置之方法權限執行。 如需有關使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱[非同步參考](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

下列程式碼會示範`GetGizmos`和`GetGizmosAsync`方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 非同步的變更會類似於對**GizmosAsync**上方。 

- 方法簽章已標註具有[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，傳回型別已變更為`Task<List<Gizmo>>`，和*非同步*已附加至的方法名稱。
- 非同步[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)而不是使用類別[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)類別。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)非同步方法。

下圖顯示非同步 gizmo 檢視。

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

Gizmos 資料的瀏覽器呈現等同於同步呼叫所建立的檢視。 唯一的差別是非同步的版本可能會更好的效能載量下。

## <a id="Parallel"></a>以平行方式執行多項作業

動作必須執行數個獨立作業時，非同步動作方法會有更大的優點，透過同步方法。 在範例中提供，同步方法`PWG`（適用於產品、 Widget 和 Gizmos） 會顯示三個 web 服務呼叫，以取得一份產品、 widget 和 gizmos 的結果。 [ASP.NET Web API](../../../web-api/index.md)專案提供這些服務會使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模擬延遲或慢速網路呼叫。 當延遲設定為 500 毫秒，非同步`PWGasync`方法會採用完成時同步稍微超過 500 毫秒`PWG`版本高於 1500 毫秒。 同步`PWG`方法以下列程式碼所示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

非同步`PWGasync`方法以下列程式碼所示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

下圖顯示從傳回檢視**PWGasync**方法。

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>使用取消語彙基元

傳回非同步動作方法`Task<ActionResult>`會取消，則會接受的[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)參數如果有提供與[AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx)屬性。 下列程式碼會示範`GizmosCancelAsync`與 150 毫秒的逾時值的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

下列程式碼將示範 GetGizmosAsync 多載會採用[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)參數。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

在提供範例應用程式的情況下，選取*取消語彙基元示範*連結呼叫`GizmosCancelAsync`方法，並且示範非同步呼叫的取消。

## <a id="ServerConfig"></a>高並行/高延遲的 Web 服務呼叫的伺服器組態

若要實現非同步的 web 應用程式的優點，您可能需要進行一些變更為預設伺服器設定。 請記住下列事項時設定和壓力測試非同步的 web 應用程式中。

- Windows 7、 Windows Vista 和所有 Windows 用戶端的作業系統有最多 10 個並行要求。 您必須是 Windows 伺服器作業系統以查看在高負載之下的非同步方法的優點。
- 向 IIS 註冊.NET 4.5，從提升權限的命令提示字元：  
 %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
 請參閱[ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 您可能需要增加[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)佇列限制從 1000 到 5000 的預設值。 如果設定為太低，您可能會看到[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)拒絕 HTTP 503 狀態的要求。 若要變更 HTTP.sys 佇列限制：

    - 開啟 IIS 管理員，並瀏覽至 [應用程式集區] 窗格。
    - 目標應用程式集區上按一下滑鼠右鍵，然後選取**進階設定**。  
        ![advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - 在**進階設定**對話方塊中，變更*佇列長度*從 1000 到 5000。  
        ![佇列長度](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
 請注意，在上述映像中的.NET framework 會列為 v4.0，即使應用程式集區使用.NET 4.5。 若要了解此差異，請參閱下列各項：

    - [.NET 版本控制和多目標為.NET 4.5 是就地升級至.NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [如何設定 IIS 應用程式或應用程式集區使用 ASP.NET 3.5，而不是 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [.NET framework 版本和相依性](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- 如果您的應用程式正在使用 web 服務或 System.NET 為與後端透過 HTTP 通訊您可能需要增加[connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)項目。 ASP.NET 應用程式，這受限於自動設定功能，為 12 倍的 Cpu 數目。 這表示，在四處理器，您可以擁有最多 12 \* 4 = 48 IP 結束點的並行連線。 因為這繫結至[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)，最簡單的方式增加`maxconnection`在 ASP.NET 應用程式是設定[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)以程式設計方式在從`Application_Start`方法中的*global.asax*檔案。 請參閱下載範例的範例。
- 在.NET 4.5，預設值為 5000 [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)情況應該沒問題。
