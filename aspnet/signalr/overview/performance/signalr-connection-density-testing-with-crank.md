---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: SignalR 連線密度測試與區軸 |Microsoft Docs
author: tfitzmac
description: SignalR 連線密度測試與區軸
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: c4ca8e368c8b722cc81ae7367140fd22f6fc7fd1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367822"
---
<a name="signalr-connection-density-testing-with-crank"></a>SignalR 連線密度測試與區軸
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用曲柄工具來測試多個模擬用戶端應用程式。


一旦您的應用程式執行 （可能是 Azure web 角色，IIS，或自我裝載使用 Owin） 及其裝載環境中，您可以測試使用曲柄工具連線密度高程度的應用程式的回應。 Internet Information Services (IIS) 伺服器、 Owin 主機或 Azure web 角色，可以是裝載環境。 (注意： 效能計數器並不適用於 Azure App Service Web Apps，因此您無法從連線密度測試取得效能資料。)

連線密度是指可以在伺服器建立的同時 TCP 連線數目。 每個 TCP 連線時，會產生其本身的額外負荷，並開啟大量的閒置的連線將最終會建立記憶體瓶頸。

[SignalR 程式碼基底](https://github.com/signalr/signalr)包含負載測試的工具，稱為**Crank**。 即使最新版本可在[Dev 分支](https://github.com/SignalR/signalr/tree/dev)GitHub 上。 您可以下載的 Zip 封存 SignalR 的 Dev 分支的程式碼基底[此處](https://github.com/SignalR/SignalR/archive/dev.zip)。

曲柄可能用來完全 saturate 伺服器的記憶體，以便能夠計算可能伺服器硬體上的閒置連線的總數。 或者，您也可以使用曲柄對負載測試一定數量的記憶體不足的壓力，伺服器的緩慢增加連線，直到到達特定的計數或特定的記憶體臨界值。

測試時，請務必使用遠端用戶端，以避免任何競爭資源 （亦即，TCP 連線和記憶體）。 監視用戶端，以確保它們未達到任何可能防止伺服器到達其完整的容量 （記憶體或 CPU） 的瓶頸。 您可能需要增加用戶端數目，以便可以完全載入伺服器。

### <a name="running-a-connection-density-test"></a>執行連線密度測試

本章節描述的 SignalR 應用程式上執行連線密度測試所需的步驟。

1. 下載並建置[SignalR 的 Dev 分支程式碼基底](https://github.com/SignalR/SignalR/archive/dev.zip)。 在命令提示字元中，瀏覽至&lt;專案目錄&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug。
2. 應用程式部署至其預定的裝載環境。 請記下您的應用程式使用; 端點例如，在建立的應用程式[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，則端點`http://<yourhost>:8080/signalr`。
3. 安裝[SignalR 效能計數器](signalr-performance.md#perfcounters)伺服器上。 如果您的應用程式在 Azure 上執行，請參閱[Azure Web 角色中使用 SignalR 效能計數器](using-signalr-performance-counters-in-an-azure-web-role.md)。

一旦您已下載和建置程式碼基底，並安裝在主機上的效能計數器，即使命令列工具位於`src\Microsoft.AspNet.SignalR.Crank\bin\Debug`資料夾。

可用的選項，即使工具包括：

- **/?**： 顯示說明畫面。 可用的選項也會顯示如果**Url**省略參數。
- **/ Url**: SignalR 連線的 URL。 這是必要參數。 SignalR 應用程式中使用預設的對應，路徑的結尾是"/ signalr"。
- **/ 傳輸**： 使用傳輸的名稱。 預設值是`auto`，這將會選取最適合可用的通訊協定。 選項包括`WebSockets`， `ServerSentEvents`，並`LongPolling`(`ForeverFrame`不可行的曲柄，因為.NET 用戶端而不是使用 Internet Explorer)。 如需有關如何 SignalR 選取傳輸的詳細資訊，請參閱[傳輸和後援](../getting-started/introduction-to-signalr.md#transports)。
- **/ BatchSize**： 在每個批次中加入的用戶端數目。 預設值為 50。
- **/ ConnectInterval**: 間隔 （毫秒） 之間加入連接。 預設值為 500。
- **/ 連線**： 用於負載測試應用程式的連線數目。 預設值是 100,000。
- **/ ConnectTimeout**: 逾時中止測試之前的秒數。 預設值為 300。
- **MinServerMBytes**： 達到的最小伺服器 mb。 預設值為 500。
- **SendBytes**： 裝載傳送至伺服器，以位元組為單位的大小。 預設值為 0。
- **SendInterval**： 以毫秒為單位，到伺服器的訊息之間的延遲。 預設值為 500。
- **SendTimeout**： 以毫秒為單位的訊息至伺服器的逾時。 預設值為 300。
- **ControllerUrl**： 其中一部用戶端將裝載控制站中樞的 Url。 預設值是 null （無中樞控制站）。 即使工作階段啟動; 時，控制器中樞已啟動沒有進一步中樞控制站與區軸之間的連絡方式為。
- **NumClients**： 模擬用戶端連線到應用程式的數目。 的預設值是 1。
- **Logfile**： 測試回合記錄檔的檔名。 預設值為 `crank.csv`。
- **SampleInterval**： 以毫秒為單位的效能計數器樣本之間的時間。 預設值為 1000。
- **SignalRInstance**： 伺服器上的效能計數器的執行個體名稱。 預設值是使用用戶端連線狀態。

### <a name="example"></a>範例

下列命令會測試網站，稱為`pfsignalr`在 Azure 上裝載使用名為"ControllerHub 」 中樞的連接埠 8080 上的應用程式，使用 100 個連接。

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
