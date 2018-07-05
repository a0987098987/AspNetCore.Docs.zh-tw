---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: 使用 ASP.NET 4.5 中的非同步方法 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您建置非同步 ASP.NET Web Forms 應用程式使用 Visual Studio Express 2012 for Web，也就是一個免費的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 3e35365cb2307ed89ee423af8afdf9c4588fcd58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382630"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>使用 ASP.NET 4.5 中的非同步方法
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將教導您建置非同步的 ASP.NET Web Forms 應用程式使用的基本概念[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)，這是免費的 Microsoft Visual Studio 版本。 您也可以使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)。 在本教學課程包含下列各節。
> 
> - [執行緒集區要求的處理方式](#HowRequestsProcessedByTP)
> - [選擇同步或非同步方法](#ChoosingSyncVasync)
> - [範例應用程式](#SampleApp)
> - [Gizmo 同步頁面](#GizmosSynch)
> - [建立非同步的 Gizmo 頁面](#CreatingAsynchGizmos)
> - [以平行方式執行多個作業](#Parallel)
> - [使用取消語彙基元](#CancelToken)
> - [高並行/高延遲的 Web 服務呼叫的伺服器組態](#ServerConfig)
> 
> 在本教學課程提供完整的範例  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) 在  [GitHub](https://github.com/)站台。


在組合中的 ASP.NET 4.5 Web Pages [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)可讓您註冊非同步方法的傳回型別的物件[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 .NET Framework 4 引進稱為 「 非同步程式設計概念[任務](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)，並支援 ASP.NET 4.5[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 工作由**任務**型別和中的相關型別[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)命名空間。 與這個非同步支援是根據.NET Framework 4.5 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)便於使用的關鍵字[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)比以前更不複雜的物件非同步方法。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字是速記語法來表示，某段程式碼應該以非同步方式等候一段程式碼。 [非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字各表示可用來將方法標記為以工作為基礎的非同步方法的提示。 組合**await**，**非同步**，而**工作**物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。 新的模型之非同步方法會呼叫*工作式非同步模式*(**點選**)。 本教學課程會假設您熟悉使用非同步程式設計[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間。

如需有關 using [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱下列參考。

- [.NET 中的技術白皮書： 非同步](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await 常見問題集](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio 非同步程式設計](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  執行緒集區要求的處理方式

在 web 伺服器上，.NET Framework 會維護用來處理 ASP.NET 要求的執行緒集區。 當要求抵達時，集區的執行緒會分派至處理該要求。 以同步方式處理要求時，處理要求的執行緒忙碌時要求正在處理，以及執行緒無法服務其他要求。   
  
這可能不是問題，因為執行緒集區可以由大到足以容納許多的忙碌執行緒數。 不過，執行緒集區中的執行緒數目有所限制 （針對.NET 4.5 的最大的預設值為 5000）。 在大型應用程式具有高並行存取的長時間執行要求，所有可用的執行緒可能忙碌中。 這種情況就是所謂的執行緒資源用盡。 當達到這種情況時，網頁伺服器排入佇列的要求。 如果要求佇列已滿，web 伺服器會拒絕要求，並顯示 HTTP 503 狀態 （伺服器忙碌中）。 CLR 執行緒集區會限制對新執行緒插入式攻擊。 如果並行處理暴增 （也就是您的網站突然可以取得大量的要求） 以及所有可用要求執行緒都忙碌中因為後端呼叫，所以具有高延遲、 有限的執行緒插入率可以讓應用程式回應非常差。 此外，執行緒集區中新增每個新執行緒會有額外負荷 （例如 1 MB 的堆疊記憶體)。 使用對服務的高延遲呼叫同步方法所在的執行緒集區成長至.NET 4.5 預設最多 5 的 web 應用程式，000 執行緒會取用大約是 5 GB 以上的記憶體可以應用程式相同的服務要求使用非同步方法，而且只有 50 的執行緒。 當您要執行非同步工作時，您不一定會使用執行緒。 比方說，當您進行非同步 web 服務要求時，ASP.NET 不會使用任何執行緒之間**非同步**方法呼叫而**await**。 使用服務要求執行緒集區具有高延遲可能會導致大量的記憶體耗用量和伺服器硬體的使用率不佳。

## <a name="processing-asynchronous-requests"></a>處理非同步要求

在 web 應用程式，請參閱大量的並行要求，在啟動時，或有暴增的負載 （其中增加並行突然），讓 web 服務呼叫的非同步會增加您的應用程式的回應能力。 非同步要求，會採用相同的處理與同步要求的時間量。 例如，如果發出呼叫的 web 服務要求時需要兩秒來完成，此要求會採用兩秒是否以同步方式或以非同步方式執行。 不過，非同步呼叫期間，執行緒不會封鎖其等候第一個要求完成時回應其他要求。 因此，非同步要求可以避免要求佇列和執行緒集區成長時有許多並行要求，以叫用長時間執行的作業。

## <a id="ChoosingSyncVasync"></a>  選擇同步或非同步方法

本節列出何時使用同步或非同步方法的指導方針。 這些是僅為建議方針;檢查每個應用程式判斷採用非同步方法是否有助於提升效能。

一般情況下，使用同步方法，在下列情況：

- 作業都是簡單或短期。
- 簡單是比效率更重要的。
- 作業是主要 CPU 而不是包含大量磁碟或網路額外負荷作業的作業。 使用受限於 CPU 作業的非同步方法不提供任何好處，並會增加負擔。

  一般情況下，使用非同步方法，在下列情況：

- 您會呼叫可以透過非同步方法，來取用的服務，且您使用.NET 4.5 或更新版本。
- 作業會受限於網路或 I/o-bound 而不是 CPU 繫結。
- 平行處理原則是比簡化程式碼更重要的。
- 您想要提供一個機制，讓使用者取消長時間執行的要求。
- 當切換出執行緒的優點加權內容切換的成本。 一般情況下，您應該讓方法非同步如果在不執行任何工作時封鎖 ASP.NET 要求執行緒的同步方法。 藉由呼叫非同步，ASP.NET 要求執行緒不會封鎖其等候完成的 web 服務要求時不任何工作。
- 測試顯示出封鎖作業是效能瓶頸的站台和 IIS 可以服務多個要求，使用非同步方法對這些封鎖呼叫。

  可下載的範例示範如何有效地使用非同步方法。 所提供的範例被設計成提供簡單的示範，ASP.NET 4.5 中非同步程式設計。 此範例的目的不是要在 ASP.NET 中的非同步程式設計的參考架構。 範例程式會呼叫[ASP.NET Web API](../../../web-api/index.md)方法會接著呼叫[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)以模擬長時間執行的 web 服務呼叫。 大部分的生產應用程式不會顯示使用非同步方法可帶來明顯好處。   
  
幾個應用程式需要的所有方法都是非同步。 通常，將一些同步方法轉換為非同步方法會提供最佳效率增加所需的工作量。

## <a id="SampleApp"></a>  範例應用程式

您可以下載範例應用程式，從[ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET)上[GitHub](https://github.com/)站台。 儲存機制是由三個專案所組成：

- *WebAppAsync*： 使用 Web API 的 ASP.NET Web Form 專案**WebAPIpwg**服務。 大部分的程式碼針對本教學課程是從這個專案。
- *WebAPIpgw*： 實作的 ASP.NET MVC 4 Web API 專案`Products, Gizmos and Widgets`控制站。 它提供的資料*WebAppAsync*專案並*Mvc4Async*專案。
- *Mvc4Async*： 包含另一個教學課程中使用的程式碼的 ASP.NET MVC 4 專案。 它會呼叫 Web API **WebAPIpwg**服務。

## <a id="GizmosSynch"></a>  Gizmo 同步頁面

 下列程式碼示範`Page_Load`用來顯示一份 gizmo 的同步方法。 （在本文中，gizmo 是虛構的機械裝置）。 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

下列程式碼示範`GetGizmos`gizmo 服務的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos`方法會將 URI 傳遞至 ASP.NET Web API HTTP 服務會傳回一份 gizmo 資料。 *WebAPIpgw*專案中包含的 Web API 實作`gizmos, widget`和`product`控制站。  
下圖顯示範例專案的 gizmo 頁面。

![Gizmo](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  建立非同步的 Gizmo 頁面

此範例會使用新[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)並[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字 （適用於.NET 4.5 和 Visual Studio 2012），讓編譯器會負責維護複雜的轉換所需非同步程式設計。 編譯器可讓您撰寫程式碼可讓您使用 C# 的同步的控制流程建構，編譯器會自動套用的轉換需要使用回呼，以避免封鎖執行緒。

ASP.NET 的非同步頁面必須包含[網頁](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞搭配`Async`屬性設定為"true"。 下列程式碼示範[網頁](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞搭配`Async`屬性設定為"true"，如*GizmosAsync.aspx*頁面。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

下列程式碼示範`Gizmos`同步`Page_Load`方法和`GizmosAsync`非同步頁面。 如果您的瀏覽器支援[HTML 5&lt;標示&gt;項目](http://www.w3.org/wiki/HTML/Elements/mark)，您會看到在變更`GizmosAsync`以黃色反白顯示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

非同步版本：

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 已套用下列變更，以允許`GizmosAsync`頁面是非同步的。

- [網頁](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞必須有`Async`屬性設定為"true"。
- `RegisterAsyncTask`方法用來註冊包含以非同步方式執行的程式碼的非同步工作。
- 新`GetGizmosSvcAsync`方法會標示[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，它會告訴編譯器產生的內文部分的回呼，並自動建立`Task`所傳回。
- &quot;非同步&quot;已附加至非同步方法名稱。 附加"Async"不是必要，但是撰寫非同步方法時，是的慣例。
- 新的傳回型別`GetGizmosSvcAsync`方法是`Task`。 傳回型別`Task`代表進行中的工作，並提供方法的呼叫端的控制代碼，以等候非同步作業的完成。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至 web 服務呼叫。
- 非同步 web 服務 API 呼叫 (`GetGizmosAsync`)。

內`GetGizmosSvcAsync`方法主體另一個非同步方法，`GetGizmosAsync`呼叫。 `GetGizmosAsync` 立即傳回`Task<List<Gizmo>>`，最終將會完成資料可用時。 因為您不想要執行任何動作，在您的 gizmo 資料之前，程式碼會等候工作 (使用**await**關鍵字)。 您可以使用**await**附註的方法中的關鍵字**非同步**關鍵字。

**Await**關鍵字不會封鎖執行緒，直到工作完成為止。 它代表註冊方法的其餘工作中，將回呼，並立即傳回。 等候的工作最終完成時，它會叫用該回呼，並因此繼續執行停止的地方方法權限。 如需有關使用[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)並[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字和[工作](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空間，請參閱[非同步參考](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

下列程式碼示範`GetGizmos`和`GetGizmosAsync`方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 非同步的變更會類似於對**GizmosAsync**上方。 

- 方法簽章已標註[非同步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)關鍵字，傳回的型別已變更為`Task<List<Gizmo>>`，以及*非同步*已附加至的方法名稱。
- 非同步[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)使用類別來取代同步[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)類別。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)關鍵字已套用至[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx)非同步方法。

下圖顯示非同步 gizmo 檢視。

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

瀏覽器資料的呈現方式 gizmo 等同於同步呼叫所建立的檢視。 唯一的差別是非同步的版本可能會更好的效能在沈重的負載。

## <a name="registerasynctask-notes"></a>RegisterAsyncTask 備忘稿

方法與接到`RegisterAsyncTask`之後，立刻會執行[PreRender](https://msdn.microsoft.com/library/ms178472.aspx)。 您也可以使用非同步 void 頁面事件直接，如下列程式碼所示：

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Void 的非同步事件的缺點是，開發人員不再有事件時執行的完整控制權。 例如，如果這兩個.aspx 和。主要定義`Page_Load`事件和一個或這兩者都非同步，執行順序無法保證。 非事件處理常式的相同 indeterminiate 順序 (例如`async void Button_Click`) 會套用。 適用於大部分的開發人員這應該是可接受的但需要的執行順序的完整控制權的人應該只使用 Api，例如`RegisterAsyncTask`，使用方法傳回工作物件。

## <a id="Parallel"></a>  以平行方式執行多個作業

動作必須執行數個獨立作業時，非同步方法就會有重要的優點，透過同步方法。 在範例中提供，[同步] 頁面*PWG.aspx*（適用於產品、 Widget 和 Gizmo） 會顯示三個 web 服務呼叫，以取得的產品、 widget 和 gizmo 清單的結果。 [ASP.NET Web API](../../../web-api/index.md)專案，以提供這些服務會使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模擬延遲或較慢的網路呼叫。 當延遲設定為 500 毫秒，非同步*PWGasync.aspx*頁面需要稍微超過 500 毫秒就能完成時同步`PWG`版本高於 1500 毫秒。 同步*PWG.aspx*頁面會顯示下列程式碼。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

非同步`PWGasync`如下所示的程式碼後置。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

下圖顯示非同步傳回之檢視*PWGasync.aspx*頁面。

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  使用取消語彙基元

傳回的非同步方法`Task`是可取消，是他們所採取[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)時使用提供的其中一個參數`AsyncTimeout`屬性[頁面](https://msdn.microsoft.com/library/ydy4x04a.aspx)指示詞。 下列程式碼示範*GizmosCancelAsync.aspx*逾時值為在第二個頁面。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

下列程式碼示範*GizmosCancelAsync.aspx.cs*檔案。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

提供範例應用程式的情況下，選取*GizmosCancelAsync*連結呼叫*GizmosCancelAsync.aspx*頁面上，並示範非同步呼叫 （藉由逾時） 被取消。 因為延遲時間是隨機的範圍內，您可能需要重新整理頁面很多次，以取得逾時錯誤訊息。

## <a id="ServerConfig"></a>  高並行/高延遲的 Web 服務呼叫的伺服器組態

若要了解非同步的 web 應用程式的優點，您可能需要為預設伺服器組態進行一些變更。 請記住下列設定時的考量和壓力測試您的非同步 web 應用程式中。

- Windows 7、 Windows Vista、 Windows 8 和所有 Windows 用戶端的作業系統有最多 10 個並行要求。 您將需要 Windows Server 作業系統，以查看在高負載之下的非同步方法的優點。
- 向 IIS 註冊.NET 4.5，從提升權限的命令提示字元使用下列命令：  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  請參閱[ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 您可能需要增加[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)佇列的限制，從 1000 到 5000 的預設值。 如果設定太低，您可能會看到[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)拒絕要求的 HTTP 503 狀態。 若要變更 HTTP.sys 佇列限制：

    - 開啟 IIS 管理員，並瀏覽至 [應用程式集區] 窗格。
    - 目標應用程式集區上按一下滑鼠右鍵，然後選取**進階設定**。  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - 在 **進階設定**  對話方塊中，變更*佇列長度*從 1000 到 5000。  
        ![佇列長度](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  請注意，在上述映像中的.NET framework 會列為 v4.0，即使應用程式集區使用.NET 4.5。 若要了解這項差異，請參閱下列各項：

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- 如果您的應用程式使用 web 服務或 System.NET 與後端透過 HTTP 進行通訊您可能需要增加[Connectionmanagement>/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)項目。 ASP.NET 應用程式，這會受限於 12 倍的 Cpu 數目的自動設定功能。 這表示在四處理器上可以有最多 12 \* 4 = 48 IP 端點的並行連線。 因為這會繫結至[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)，最簡單的方式來增加`maxconnection`在 ASP.NET 應用程式是設定[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)以程式設計方式在從`Application_Start`方法中的*global.asax*檔案。 如需範例下載此範例，請參閱。
- 在.NET 4.5 中，預設值為 5000 [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)應該沒問題。

## <a name="contributors"></a>Contributors

- [於 Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
