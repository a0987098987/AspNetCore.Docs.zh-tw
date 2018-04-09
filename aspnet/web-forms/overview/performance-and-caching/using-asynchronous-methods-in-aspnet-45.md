---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: 使用非同步方法中 ASP.NET 4.5 |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置非同步的 ASP.NET Web Form 應用程式使用 Visual Studio Express 2012 for Web，是一套免費的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 839cfc39188a91b6674465b8ff8fe51804033295
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>ASP.NET 4.5 中使用非同步方法
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置非同步的 ASP.NET Web Form 應用程式使用的基本概念[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)，這是 Microsoft Visual Studio 的免費版本。 您也可以使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)。 在本教學課程包含下列各節。
> 
> - [執行緒集區處理要求的方式](#HowRequestsProcessedByTP)
> - [選擇同步或非同步方法](#ChoosingSyncVasync)
> - [範例應用程式](#SampleApp)
> - [Gizmos 同步頁面](#GizmosSynch)
> - [建立非同步 Gizmos 頁面](#CreatingAsynchGizmos)
> - [以平行方式執行多項作業](#Parallel)
> - [使用取消語彙基元](#CancelToken)
> - [高並行/高延遲的 Web 服務呼叫的伺服器組態](#ServerConfig)
> 
> 在本教學課程會提供完整的範例  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) 在[GitHub](https://github.com/)站台。


組合中的 ASP.NET 4.5 Web Pages [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)可讓您註冊非同步方法的傳回型別的物件[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 .NET Framework 4 引進稱為 「 非同步程式設計概念[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)和 ASP.NET 4.5 支援[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 工作由**工作**類型和相關的類型中[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)命名空間。 .NET Framework 4.5 為基礎的這個非同步支援[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，讓使用[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)比前一個較複雜的物件非同步方法。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字是用以表示某段程式碼應該以非同步方式等候一段程式碼的語法縮寫。 [非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字表示提示可讓您將方法標記為以工作為基礎的非同步方法。 組合**await**，**非同步**，而**工作**物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。 呼叫非同步方法的新模型*工作架構非同步模式*(**點選**)。 本教學課程假設您已熟悉非同步程式設計使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間。

如需有關使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱下列參考。

- [.NET 中的技術白皮書： 非同步](https://go.microsoft.com/fwlink/?LinkId=204844)
- [非同步/等候常見問題集](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio 非同步程式設計](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  執行緒集區處理要求的方式

在 web 伺服器上，.NET Framework 會維護用於 ASP.NET 要求提供服務的執行緒集區。 當要求到達時，集區的執行緒會分派處理該要求。 以同步方式處理要求時，處理要求的執行緒忙碌時要求正在處理，且執行緒無法服務其他要求。   
  
這可能不是問題，因為執行緒集區可以由大到足以應付許多的忙碌執行緒。 不過，執行緒集區中的執行緒數目是限制 （.NET 4.5 的最大的預設值是 5000）。 在大型應用程式具有高的並行處理長時間執行要求的所有可用的執行緒可能忙碌中。 這種狀況稱為執行緒資源用盡。 當達到這種狀況時，web 伺服器要求加入佇列。 如果要求佇列額滿，網頁伺服器會拒絕要求的 HTTP 503 狀態 （伺服器忙碌中）。 CLR 執行緒集區會限制對新的執行緒資料隱碼攻擊。 如果並行暴增 （也就是您的網站突然可以取得大量的要求），而且所有可用的要求執行緒忙碌因為後端呼叫，所以含高延遲，限制的執行緒資料隱碼速率可以讓應用程式非常不回應。 此外，每個新執行緒加入執行緒集區具有 （例如 1 MB 的堆疊記憶體) 的額外負荷。 Web 應用程式使用服務高延遲呼叫同步方法，執行緒集區成長至.NET 4.5 預設最大值為 5，000 執行緒會取用大約 5 GB 更多的記憶體比應用程式可以相同的服務要求使用非同步方法，而且只有 50 的執行緒。 在您執行非同步工作時，您不一定正在使用的執行緒。 例如，當您進行非同步的 web 服務要求時，ASP.NET 將不會使用任何執行緒之間**非同步**方法呼叫和**await**。 使用含高延遲為要求提供服務的執行緒集區可能會導致大量的記憶體耗用量和伺服器硬體使用率不佳。

## <a name="processing-asynchronous-requests"></a>處理非同步要求

在 web 應用程式，請參閱大量的並行要求，在啟動時，或具有暴增負載 （其中增加並行突然），讓 web 服務呼叫的非同步會增加您的應用程式的回應能力。 非同步要求所需的時間來處理與同步要求相同的數量。 比方說，如果要求會提出 web 服務呼叫，要求兩秒來完成，要求會採用兩秒是否以同步還是非同步方式執行。 不過，在非同步呼叫，執行緒不會封鎖回應其他要求，同時等候完成的第一個要求。 因此，非同步要求可以避免要求佇列和執行緒集區成長時叫用長時間執行作業的許多並行要求。

## <a id="ChoosingSyncVasync"></a>  選擇同步或非同步方法

本節列出何時使用同步或非同步方法的指導方針。 這些只是一般指引。檢查每個應用程式判斷採用非同步方法是否協助提高效能。

一般情況下，使用同步方法，在下列情況：

- 作業會簡單或對於執行短期。
- 簡單是比效率重要。
- 這些作業有主要是 CPU 作業，而非作業牽涉到大量磁碟或網路額外負荷。 使用受限於 CPU 之作業的非同步方法並沒有好處會增加負擔。

  一般情況下，使用非同步方法，在下列情況：

- 您正在呼叫可以透過非同步方法，來取用的服務，而且您要使用.NET 4.5 或更高版本。
- 作業會受限於網路或我 I/O 繫結而不是 cpu-bound。
- 平行處理原則是更重要的是比簡化程式碼。
- 您想要提供一套機制，可讓使用者取消長時間執行要求。
- 當優點切換執行緒出加權內容切換的成本。 一般情況下，您應該方法設為非同步同步方法會封鎖在 ASP.NET 要求執行緒時不執行任何工作。 藉由呼叫非同步，ASP.NET 要求執行緒不會封鎖不任何工作，同時等候完成的 web 服務要求。
- 測試顯示出封鎖作業是效能瓶頸的網站和 IIS 可以服務多個要求，使用這些封鎖呼叫的非同步方法。

  可下載的範例示範如何有效地使用非同步方法。 提供的範例被設計來提供簡單的示範，ASP.NET 4.5 中非同步程式設計。 此範例不是在 ASP.NET 中的非同步程式設計參考架構。 範例程式會呼叫[ASP.NET Web API](../../../web-api/index.md)方法會接著呼叫[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模擬長時間執行的 web 服務呼叫。 大部分的實際執行應用程式不會顯示使用非同步方法可帶來明顯好處。   
  
少數應用程式需要的所有方法都是非同步。 通常，將一些同步方法轉換為非同步方法會提供最佳效率增加所需的工作量。

## <a id="SampleApp"></a>  範例應用程式

您可以下載範例應用程式從[ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET)上[GitHub](https://github.com/)站台。 儲存機制包含三個專案：

- *WebAppAsync*： 使用 Web API 的 ASP.NET Web Form 專案**WebAPIpwg**服務。 大部分的程式碼本教學課程是從這個專案。
- *WebAPIpgw*： 實作 ASP.NET MVC 4 Web API 專案`Products, Gizmos and Widgets`控制站。 它提供的資料*WebAppAsync*專案和*Mvc4Async*專案。
- *Mvc4Async*: ASP.NET MVC 4 專案包含另一個教學課程中使用之程式碼。 它會呼叫 Web API **WebAPIpwg**服務。

## <a id="GizmosSynch"></a>  Gizmos 同步頁面

 下列程式碼會示範`Page_Load`用來顯示一份 gizmos 的同步方法。 （這個發行項，gizmo 是虛構的機械裝置）。 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

下列程式碼會示範`GetGizmos`gizmo 服務的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos`方法會將 URI 傳遞到 ASP.NET Web API HTTP 服務會傳回一份 gizmos 資料。 *WebAPIpgw*專案包含 Web 應用程式開發介面的實作`gizmos, widget`和`product`控制站。  
下圖顯示範例專案的 gizmos 頁。

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  建立非同步 Gizmos 頁面

此範例會使用新[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)和[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字 （適用於.NET 4.5 和 Visual Studio 2012），讓編譯器會負責維護所需的複雜的轉換非同步程式設計。 編譯器可讓您撰寫程式碼會使用 C# 的同步的控制流程建構，編譯器會自動套用必要使用回呼，以避免封鎖執行緒的轉換。

非同步的 ASP.NET 網頁必須包含[頁面](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞與`Async`屬性設定為"true"。 下列程式碼會示範[頁面](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞與`Async`屬性設定為"true"，如*GizmosAsync.aspx*頁面。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

下列程式碼會示範`Gizmos`同步`Page_Load`方法和`GizmosAsync`非同步頁面。 如果您的瀏覽器支援[HTML 5&lt;標示&gt;元素](http://www.w3.org/wiki/HTML/Elements/mark)，您會看到中的變更`GizmosAsync`以黃色反白顯示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

非同步版本：

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 下列變更套用至允許`GizmosAsync`頁面是非同步。

- [頁面](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞必須有`Async`屬性設定為"true"。
- `RegisterAsyncTask`方法用來註冊包含程式碼會以非同步方式執行的非同步工作。
- 新`GetGizmosSvcAsync`方法標示為[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，它會告訴編譯器產生的組件主體的回呼，並自動建立`Task`傳回。
- &quot;非同步&quot;已附加至非同步方法名稱。 將"Async"附加就不需要但慣例撰寫的非同步方法時。
- 新的傳回型別`GetGizmosSvcAsync`方法`Task`。 傳回型別`Task`代表進行中的工作，並提供與控制代碼，以等候非同步作業完成的方法的呼叫者。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至 web 服務呼叫。
- 非同步的 web 服務 API 呼叫 (`GetGizmosAsync`)。

內部`GetGizmosSvcAsync`方法主體另一個非同步方法，`GetGizmosAsync`呼叫。 `GetGizmosAsync` 立即傳回`Task<List<Gizmo>>`，最終將會完成時有可用資料。 您不想要執行任何其他動作，直到您 gizmo 資料，因為程式碼會等候工作 (使用**await**關鍵字)。 您可以使用**await**以註解的方法中的關鍵字**非同步**關鍵字。

**Await**關鍵字不會封鎖執行緒，直到工作完成為止。 它註冊方法的其餘部分，做為回呼，以於工作，並立即傳回。 當最後完成等候的工作時，它將會叫用回撥，並因此繼續中斷的位置之方法權限執行。 如需有關使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱[非同步參考](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

下列程式碼會示範`GetGizmos`和`GetGizmosAsync`方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 非同步的變更會類似於對**GizmosAsync**上方。 

- 方法簽章已標註具有[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，傳回型別已變更為`Task<List<Gizmo>>`，和*非同步*已附加至的方法名稱。
- 非同步[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)使用類別來取代同步[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)類別。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx)非同步方法。

下圖顯示非同步 gizmo 檢視。

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Gizmos 資料的瀏覽器呈現等同於同步呼叫所建立的檢視。 唯一的差別是非同步的版本可能會更好的效能載量下。

## <a name="registerasynctask-notes"></a>RegisterAsyncTask 附註

方法與接到`RegisterAsyncTask`之後，立即將執行[PreRender](https://msdn.microsoft.com/library/ms178472.aspx)。 您也可以使用 async void 網頁事件直接管理，如下列程式碼所示：

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Async void 事件的缺點是開發人員也不再有事件時執行的完整控制權。 例如，如果這兩個.aspx 和。定義主要`Page_Load`事件以及一個或兩者都非同步的執行順序無法保證。 非事件處理常式的相同 indeterminiate 順序 (例如`async void Button_Click`) 會套用。 大部分的開發人員應該可接受的但也需要完整控制權的執行順序的人員才應該使用應用程式開發介面，例如`RegisterAsyncTask`，取用傳回的 Task 物件的方法。

## <a id="Parallel"></a>  以平行方式執行多項作業

動作必須執行數個獨立作業時，非同步方法會有更大的優點，透過同步方法。 在範例中提供，同步頁面*PWG.aspx*（適用於產品、 Widget 和 Gizmos） 會顯示三個 web 服務呼叫，以取得一份產品、 widget 和 gizmos 的結果。 [ASP.NET Web API](../../../web-api/index.md)專案提供這些服務會使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模擬延遲或慢速網路呼叫。 當延遲設定為 500 毫秒，非同步*PWGasync.aspx*頁面會稍微超過 500 毫秒時同步完成`PWG`版本高於 1500 毫秒。 同步*PWG.aspx*頁面會顯示下列程式碼。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

非同步`PWGasync`後置程式碼如下所示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

下圖顯示檢視所傳回的非同步*PWGasync.aspx*頁面。

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  使用取消語彙基元

非同步方法傳回`Task`會取消，則會接受的[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)參數如果有提供與`AsyncTimeout`屬性[頁面](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞。 下列程式碼會示範*GizmosCancelAsync.aspx*逾時值為在第二個頁面。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

下列程式碼會示範*GizmosCancelAsync.aspx.cs*檔案。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

在提供範例應用程式的情況下，選取*GizmosCancelAsync*連結呼叫*GizmosCancelAsync.aspx*頁面上，並示範非同步呼叫的取消 （藉由逾時）。 由於延遲時間是隨機的範圍內，您可能需要重新整理頁面兩次，以取得逾時錯誤訊息。

## <a id="ServerConfig"></a>  高並行/高延遲的 Web 服務呼叫的伺服器組態

若要實現非同步的 web 應用程式的優點，您可能需要進行一些變更為預設伺服器設定。 請記住下列事項時設定和壓力測試非同步的 web 應用程式中。

- Windows 7、 Windows Vista、 Windows 8 和 Windows 用戶端作業系統有最多 10 個並行要求。 您必須是 Windows 伺服器作業系統以查看在高負載之下的非同步方法的優點。
- 向 IIS 註冊.NET 4.5，從提升權限的命令提示字元使用下列命令：  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
  請參閱[ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 您可能需要增加[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)佇列限制從 1000 到 5000 的預設值。 如果設定為太低，您可能會看到[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)拒絕 HTTP 503 狀態的要求。 若要變更 HTTP.sys 佇列限制：

    - 開啟 IIS 管理員，並瀏覽至 [應用程式集區] 窗格。
    - 目標應用程式集區上按一下滑鼠右鍵，然後選取**進階設定**。  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - 在**進階設定**對話方塊中，變更*佇列長度*從 1000 到 5000。  
        ![佇列長度](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  請注意，在上述映像中的.NET framework 會列為 v4.0，即使應用程式集區使用.NET 4.5。 若要了解此差異，請參閱下列各項：

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- 如果您的應用程式正在使用 web 服務或 System.NET 為與後端透過 HTTP 通訊您可能需要增加[connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)項目。 ASP.NET 應用程式，這受限於自動設定功能，為 12 倍的 Cpu 數目。 這表示，在四處理器，您可以擁有最多 12 \* 4 = 48 IP 結束點的並行連線。 因為這繫結至[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)，最簡單的方式增加`maxconnection`在 ASP.NET 應用程式是設定[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)以程式設計方式在從`Application_Start`方法中的*global.asax*檔案。 請參閱下載範例的範例。
- 在.NET 4.5，預設值為 5000 [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)情況應該沒問題。

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
