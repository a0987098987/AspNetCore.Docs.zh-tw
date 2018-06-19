---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: 測試區軸與 SignalR 連線密度 |Microsoft 文件
author: tfitzmac
description: 測試區軸與 SignalR 連線密度
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2017
ms.locfileid: "26535327"
---
<a name="signalr-connection-density-testing-with-crank"></a>測試區軸與 SignalR 連線密度
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用區軸工具來測試與多個模擬用戶端應用程式。


一旦您的應用程式 （任一 Azure web 角色，IIS，或自我裝載使用 Owin） 其主控環境中執行，您可以測試使用區軸工具連接密度的高層級的應用程式的回應。 網際網路資訊服務 (IIS) 伺服器、 Owin 主機或 Azure web 角色，可以是裝載環境。 (注意： 效能計數器並不適用於 Azure App Service Web 應用程式，因此無法從連接密度測試取得效能資料。)

連接密度是指同時可以建立在伺服器的 TCP 連線的數目。 每個 TCP 連線會產生其本身的額外負荷，並開啟大量閒置連線的最終將會建立記憶體瓶頸。

[程式碼基底 SignalR](https://github.com/signalr/signalr)包含負載測試的工具，稱為**Crank**。 區軸的最新版本位於[Dev 分支](https://github.com/SignalR/signalr/tree/dev)GitHub 上。 您可以下載的 Zip 封存 SignalR 的 Dev 分支的程式碼基底[這裡](https://github.com/SignalR/SignalR/archive/dev.zip)。

區軸可能用來完全飽和伺服器的記憶體，以便能夠計算總數的伺服器硬體上可能的閒置連接。 或者，您也可以使用區軸對進行負載測試中的伺服器記憶體不足的壓力，數量由根本上連線，直到達到特定的計數或特定的記憶體臨界值。

測試時，務必使用遠端用戶端，以避免任何競爭資源 （亦即，TCP 連線和記憶體）。 監視用戶端，以確保它們不到達可能防止伺服器到達其完整的容量 （記憶體或 CPU） 的任何瓶頸。 您可能需要增加用戶端數目，以便可以完全載入伺服器。

### <a name="running-a-connection-density-test"></a>執行連線密度測試

本章節描述 SignalR 應用程式上執行連接密度測試所需的步驟。

1. 下載並建置[SignalR 的 Dev 分支程式碼基底](https://github.com/SignalR/SignalR/archive/dev.zip)。 在命令提示字元，瀏覽至&lt;專案目錄&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug。
2. 應用程式部署至指定的裝載環境。 請記下之端點的應用程式使用。例如，在應用程式中建立[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，端點`http://<yourhost>:8080/signalr`。
3. 安裝[SignalR 效能計數器](signalr-performance.md#perfcounters)在伺服器上。 如果您的應用程式在 Azure 上執行，請參閱[Azure Web 角色中使用 SignalR 效能計數器](using-signalr-performance-counters-in-an-azure-web-role.md)。

一旦您下載及建置程式碼基底，並在主機上安裝效能計數器，區軸命令列工具位於`src\Microsoft.AspNet.SignalR.Crank\bin\Debug`資料夾。

區軸工具可用的選項包括：

- **/?**： 顯示說明畫面。 可用的選項也會顯示如果**Url**省略參數。
- **/ Url**: SignalR 連線的 URL。 這是必要參數。 將 SignalR 應用程式中使用預設的對應，在結束路徑 」 / signalr"。
- **/ 傳輸**： 所用之傳輸的名稱。 預設值是`auto`，這將會選取最適合可用通訊協定。 選項包括`WebSockets`， `ServerSentEvents`，和`LongPolling`(`ForeverFrame`不的選項區軸，因為.NET 用戶端而不使用 Internet Explorer)。 如需有關 SignalR 選取傳輸方式的詳細資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。
- **/ BatchSize**： 加入每個批次中的用戶端數目。 預設值為 50。
- **/ ConnectInterval**： 以毫秒為單位之間加入連接的間隔。 預設值為 500。
- **/ Connections**： 用於負載測試應用程式的連接數目。 預設值為 100,000。
- **/ ConnectTimeout**: 逾時中止測試之前的秒數。 預設值為 300。
- **MinServerMBytes**： 連線到最小的伺服器 （mb)。 預設值為 500。
- **SendBytes**： 裝載傳送至伺服器，以位元組為單位的大小。 預設值為 0。
- **SendInterval**： 以毫秒為單位之伺服器的訊息之間的延遲。 預設值為 500。
- **SendTimeout**： 以毫秒為單位的訊息，伺服器逾時。 預設值為 300。
- **ControllerUrl**： 其中一個用戶端將裝載控制站集線器的 Url。 預設為 null （沒有中樞控制站）。 中樞控制站會在啟動區軸工作階段啟動。沒有進一步進行區軸中樞控制站之間的連絡人。
- **NumClients**： 模擬用戶端連線到應用程式數目。 預設為其中一個。
- **記錄檔**： 測試回合記錄檔的檔名。 預設為 `crank.csv`。
- **SampleInterval**： 以毫秒為單位的效能計數器樣本之間的時間。 預設值為 1000。
- **SignalRInstance**： 伺服器上的效能計數器的執行個體名稱。 預設值是使用用戶端連線狀態。

### <a name="example"></a>範例

下列命令會測試網站，呼叫`pfsignalr`在 Azure 上裝載以名為"ControllerHub"集線器連接埠 8080 上的應用程式，使用 100 個連接。

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
