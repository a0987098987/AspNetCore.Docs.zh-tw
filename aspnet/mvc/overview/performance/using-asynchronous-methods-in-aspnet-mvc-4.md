---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: ASP.NET MVC 4 中使用非同步方法 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置非同步 ASP.NET MVC Web 應用程式使用 Visual Studio Express 2012 for Web，也就是免費的 ve 的基本概念...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 757b15c34f6fa0078d0bca0dfb38d553bb73809d
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577700"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>使用 ASP.NET MVC 4 中的非同步方法
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 本教學課程將教導您建置非同步的 ASP.NET MVC Web 應用程式使用的基本概念[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)，這是免費的 Microsoft Visual Studio 版本。 您也可以使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)。
> 
> 在本教學課程，在 github 上提供完整的範例  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4[控制器](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx)類別中組合[.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)可讓您撰寫非同步動作方法會傳回型別的物件[工作&lt;ActionResult&gt;](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). .NET Framework 4 引進稱為 「 非同步程式設計概念[任務](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)，並支援 ASP.NET MVC 4[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 工作由**任務**型別和中的相關型別[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)命名空間。 與這個非同步支援是根據.NET Framework 4.5 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)便於使用的關鍵字[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)比以前更不複雜的物件非同步方法。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字是速記語法來表示，某段程式碼應該以非同步方式等候一段程式碼。 [非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字各表示可用來將方法標記為以工作為基礎的非同步方法的提示。 組合**await**，**非同步**，而**工作**物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。 新的模型之非同步方法會呼叫*工作式非同步模式*(**點選**)。 本教學課程會假設您熟悉使用非同步程式設計[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間。

如需有關 using [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱下列參考。

- [.NET 中的技術白皮書： 非同步](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await 常見問題集](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio 非同步程式設計](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  執行緒集區要求的處理方式

在 web 伺服器上，.NET Framework 會維護用來處理 ASP.NET 要求的執行緒集區。 當要求抵達時，集區的執行緒會分派至處理該要求。 以同步方式處理要求時，處理要求的執行緒忙碌時要求正在處理，以及執行緒無法服務其他要求。   
  
這可能不是問題，因為執行緒集區可以由大到足以容納許多的忙碌執行緒數。 不過，執行緒集區中的執行緒數目有所限制 （針對.NET 4.5 的最大的預設值為 5000）。 在大型應用程式具有高並行存取的長時間執行要求，所有可用的執行緒可能忙碌中。 這種情況就是所謂的執行緒資源用盡。 當達到這種情況時，網頁伺服器排入佇列的要求。 如果要求佇列已滿，web 伺服器會拒絕要求，並顯示 HTTP 503 狀態 （伺服器忙碌中）。 CLR 執行緒集區會限制對新執行緒插入式攻擊。 如果並行處理暴增 （也就是您的網站突然可以取得大量的要求） 以及所有可用要求執行緒都忙碌中因為後端呼叫，所以具有高延遲、 有限的執行緒插入率可以讓應用程式回應非常差。 此外，執行緒集區中新增每個新執行緒會有額外負荷 （例如 1 MB 的堆疊記憶體)。 使用對服務的高延遲呼叫同步方法所在的執行緒集區成長至.NET 4.5 預設最多 5 的 web 應用程式，000 執行緒會取用大約是 5 GB 以上的記憶體可以應用程式相同的服務要求使用非同步方法，而且只有 50 的執行緒。 當您要執行非同步工作時，您不一定會使用執行緒。 比方說，當您進行非同步 web 服務要求時，ASP.NET 不會使用任何執行緒之間**非同步**方法呼叫而**await**。 使用服務要求執行緒集區具有高延遲可能會導致大量的記憶體耗用量和伺服器硬體的使用率不佳。

## <a name="processing-asynchronous-requests"></a>處理非同步要求

在 web app 中看到大量的並行要求，在啟動時，或有暴增的負載 （其中增加並行突然），進行非同步 web 服務呼叫，將會增加應用程式的回應能力。 非同步要求，會採用相同的處理與同步要求的時間量。 如果發出呼叫的 web 服務要求時需要兩秒來完成，此要求會採用兩秒是否以同步方式或以非同步方式執行。 不過在非同步呼叫，不會封鎖執行緒等候完成的第一個要求回應其他要求。 因此，非同步要求可以避免要求佇列和執行緒集區成長時有許多並行要求，以叫用長時間執行的作業。

## <a id="ChoosingSyncVasync"></a>  選擇同步或非同步動作方法

本節列出何時使用同步或非同步動作方法的指導方針。 這些是僅為建議方針;檢查每個應用程式判斷採用非同步方法是否有助於提升效能。

一般情況下，使用同步方法，在下列情況：

- 作業都是簡單或短期。
- 簡單是比效率更重要的。
- 作業是主要 CPU 而不是包含大量磁碟或網路額外負荷作業的作業。 受限於 CPU 作業上使用非同步動作方法不提供任何好處，並會增加負擔。

  一般情況下，使用非同步方法，在下列情況：

- 您會呼叫可以透過非同步方法，來取用的服務，且您使用.NET 4.5 或更新版本。
- 作業會受限於網路或 I/o-bound 而不是 CPU 繫結。
- 平行處理原則是比簡化程式碼更重要的。
- 您想要提供一個機制，讓使用者取消長時間執行的要求。
- 當切換執行緒的優點超過內容切換的成本。 一般情況下，您應該方法設為非同步如果同步的方法在 ASP.NET 要求執行緒上等候時不執行任何工作。 藉由呼叫非同步，ASP.NET 要求執行緒不會停止其等候完成的 web 服務要求時不任何工作。
- 測試顯示出封鎖作業是效能瓶頸的站台和 IIS 可以服務多個要求，使用非同步方法對這些封鎖呼叫。

  可下載的範例示範如何有效地使用非同步動作方法。 所提供的範例被設計成提供簡單的示範，在 ASP.NET MVC 4 中使用.NET 4.5 的非同步程式設計。 此範例的目的不是要在 ASP.NET MVC 中的非同步程式設計的參考架構。 範例程式會呼叫[ASP.NET Web API](../../../web-api/index.md)方法會接著呼叫[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)以模擬長時間執行的 web 服務呼叫。 大部分的生產應用程式不會顯示使用非同步動作方法可帶來明顯好處。   
  
幾個應用程式需要的所有動作方法都是非同步。 通常，將一些同步動作方法轉換為非同步方法會提供最佳效率增加所需的工作量。

## <a id="SampleApp"></a>  範例應用程式

您可以下載範例應用程式，從[ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET)上[GitHub](https://github.com/)站台。 儲存機制是由三個專案所組成：

- *Mvc4Async*： 包含在本教學課程使用的程式碼的 ASP.NET MVC 4 專案。 它會呼叫 Web API **WebAPIpgw**服務。
- *WebAPIpgw*： 實作的 ASP.NET MVC 4 Web API 專案`Products, Gizmos and Widgets`控制站。 它提供的資料*WebAppAsync*專案並*Mvc4Async*專案。
- *WebAppAsync*： 另一個教學課程中使用的 ASP.NET Web Form 專案。

## <a id="GizmosSynch"></a>  Gizmo 同步動作方法

 下列程式碼示範`Gizmos`同步動作方法，用來顯示一份 gizmo。 （在本文中，gizmo 是虛構的機械裝置）。 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

下列程式碼示範`GetGizmos`gizmo 服務的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

`GizmoService GetGizmos`方法會將 URI 傳遞至 ASP.NET Web API HTTP 服務會傳回一份 gizmo 資料。 *WebAPIpgw*專案中包含的 Web API 實作`gizmos, widget`和`product`控制站。  
下圖顯示範例專案的 gizmo 檢視。

![Gizmo](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  建立非同步的 Gizmo 動作方法

此範例會使用新[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)及[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字 （適用於.NET 4.5 和 Visual Studio 2012），讓編譯器會負責維護複雜的轉換所需非同步程式設計。 編譯器可讓您撰寫程式碼可讓您使用 C# 的同步的控制流程建構，編譯器會自動套用的轉換需要使用回呼，以避免封鎖執行緒。

下列程式碼示範`Gizmos`同步方法和`GizmosAsync`非同步方法。 如果您的瀏覽器支援[HTML 5`<mark>`項目](http://www.w3.org/wiki/HTML/Elements/mark)，您會看到在變更`GizmosAsync`以黃色反白顯示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 已套用下列變更，以允許`GizmosAsync`必須是非同步的。

- 方法會標示[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，它會告訴編譯器產生的內文部分的回呼，並自動建立`Task<ActionResult>`所傳回。
- &quot;非同步&quot;已附加至的方法名稱。 附加"Async"不是必要，但是撰寫非同步方法時，是的慣例。
- 傳回的型別已經從`ActionResult`至`Task<ActionResult>`。 傳回型別`Task<ActionResult>`代表進行中的工作，並提供方法的呼叫端的控制代碼，以等候非同步作業的完成。 在此情況下，呼叫端是 web 服務。 `Task<ActionResult>` 代表進行中工作的結果 `ActionResult.`
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至 web 服務呼叫。
- 非同步 web 服務 API 呼叫 (`GetGizmosAsync`)。

內`GetGizmosAsync`方法主體另一個非同步方法，`GetGizmosAsync`呼叫。 `GetGizmosAsync` 立即傳回`Task<List<Gizmo>>`，最終將會完成資料可用時。 因為您不想要執行任何動作，在您的 gizmo 資料之前，程式碼會等候工作 (使用**await**關鍵字)。 您可以使用**await**附註的方法中的關鍵字**非同步**關鍵字。

**Await**關鍵字不會封鎖執行緒，直到工作完成為止。 它代表註冊方法的其餘工作中，將回呼，並立即傳回。 等候的工作最終完成時，它會叫用該回呼，並因此繼續執行停止的地方方法權限。 如需有關使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱[非同步參考](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

下列程式碼示範`GetGizmos`和`GetGizmosAsync`方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 非同步的變更會類似於對**GizmosAsync**上方。 

- 方法簽章已標註[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，傳回的型別已變更為`Task<List<Gizmo>>`，以及*非同步*已附加至的方法名稱。
- 非同步[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)而不是使用類別[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)類別。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)非同步方法。

下圖顯示非同步 gizmo 檢視。

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

瀏覽器資料的呈現方式 gizmo 等同於同步呼叫所建立的檢視。 唯一的差別是非同步的版本可能會更好的效能在沈重的負載。

## <a id="Parallel"></a>  以平行方式執行多個作業

動作必須執行數個獨立作業時，非同步動作方法會有重要的優點，透過同步方法。 在範例中提供，同步方法`PWG`（適用於產品、 Widget 和 Gizmo） 會顯示三個 web 服務呼叫，以取得的產品、 widget 和 gizmo 清單的結果。 [ASP.NET Web API](../../../web-api/index.md)專案，以提供這些服務會使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模擬延遲或較慢的網路呼叫。 當延遲設定為 500 毫秒，非同步`PWGasync`方法需要稍微超過 500 毫秒就能完成時同步`PWG`版本高於 1500 毫秒。 同步`PWG`方法以下列程式碼所示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

非同步`PWGasync`方法以下列程式碼所示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

下圖顯示傳回之檢視**PWGasync**方法。

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  使用取消語彙基元

非同步動作方法傳回`Task<ActionResult>`是可取消，是他們採取[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)時使用提供的其中一個參數[AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx)屬性。 下列程式碼示範`GizmosCancelAsync`150 毫秒的逾時值的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

下列程式碼顯示 GetGizmosAsync 多載會採用[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)參數。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

提供範例應用程式的情況下，選取*取消語彙基元 Demo*連結呼叫`GizmosCancelAsync`方法，並示範取消非同步呼叫。

## <a id="ServerConfig"></a>  高並行/高延遲的 Web 服務呼叫的伺服器組態

若要了解非同步的 web 應用程式的優點，您可能需要為預設伺服器組態進行一些變更。 請記住下列設定時的考量和壓力測試您的非同步 web 應用程式中。

- Windows 7、 Windows Vista 及所有 Windows 用戶端的作業系統有最多 10 個並行要求。 您將需要 Windows Server 作業系統，以查看在高負載之下的非同步方法的優點。
- 向 IIS 註冊.NET 4.5，從提升權限的命令提示字元：  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis-i  
  請參閱[ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 您可能需要增加[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)佇列的限制，從 1000 到 5000 的預設值。 如果設定太低，您可能會看到[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)拒絕要求的 HTTP 503 狀態。 若要變更 HTTP.sys 佇列限制：

    - 開啟 IIS 管理員，並瀏覽至 [應用程式集區] 窗格。
    - 目標應用程式集區上按一下滑鼠右鍵，然後選取**進階設定**。  
        ![advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - 在 **進階設定**  對話方塊中，變更*佇列長度*從 1000 到 5000。  
        ![佇列長度](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  請注意，在上述映像中的.NET framework 會列為 v4.0，即使應用程式集區使用.NET 4.5。 若要了解這項差異，請參閱下列各項：

    - [.NET 版本控制和多目標為.NET 4.5 是.NET 4.0 的就地升級](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [如何設定 IIS 應用程式或應用程式集區使用 ASP.NET 3.5，而不是 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [.NET Framework 版本和相依性](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- 如果您的應用程式使用 web 服務或 System.NET 與後端透過 HTTP 進行通訊您可能需要增加[Connectionmanagement>/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)項目。 ASP.NET 應用程式，這會受限於 12 倍的 Cpu 數目的自動設定功能。 這表示在四處理器上可以有最多 12 \* 4 = 48 IP 端點的並行連線。 因為這會繫結至[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)，最簡單的方式來增加`maxconnection`在 ASP.NET 應用程式是設定[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)以程式設計方式在從`Application_Start`方法中的*global.asax*檔案。 如需範例下載此範例，請參閱。
- 在.NET 4.5 中，預設值為 5000 [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)應該沒問題。
