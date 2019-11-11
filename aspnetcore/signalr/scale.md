---
title: ASP.NET Core SignalR 生產環境裝載和調整
author: bradygaster
description: 瞭解如何避免使用 ASP.NET Core SignalR之應用程式的效能和調整問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
no-loc:
- SignalR
ms.openlocfilehash: a215ce489746a7585cf4e72d4f04e0eac588b44c
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905729"
---
# <a name="aspnet-core-opno-locsignalr-hosting-and-scaling"></a>ASP.NET Core SignalR 裝載和調整

[Andrew Stanton-護士](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)和[Tom 作者: dykstra](https://github.com/tdykstra)，

本文說明使用 ASP.NET Core SignalR之高流量應用程式的裝載和調整考慮。

## <a name="sticky-sessions"></a>粘滯話

SignalR 要求特定連接的所有 HTTP 要求都必須由相同的伺服器進程處理。 當 SignalR 在伺服器陣列（多部伺服器）上執行時，必須使用「粘滯會話」。 某些負載平衡器也稱為「粘滯會話」的會話親和性。 Azure App Service 使用[應用程式要求路由](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview)（ARR）來路由傳送要求。 在您的 Azure App Service 中啟用「ARR 親和性」設定，將會啟用「粘滯會話」。 不需要粘滯會話的唯一情況如下：

1. 在單一伺服器上裝載時，在單一進程中。
1. 使用 Azure SignalR 服務時。
1. 當所有用戶端都設定為**只**使用 websocket，**而且**用戶端設定中已啟用[SkipNegotiation 設定](xref:signalr/configuration#configure-additional-options)時。

在所有其他情況下（包括使用 Redis 背板時），則必須針對 [粘滯會話] 設定伺服器環境。

如需設定 SignalRAzure App Service 的指引，請參閱 <xref:signalr/publish-to-azure-web-app>。

## <a name="tcp-connection-resources"></a>TCP 連線資源

網頁伺服器可支援的並行 TCP 連線數目有限。 標準 HTTP 用戶端會使用*暫時*連接。 當用戶端閒置並在稍後重新開啟時，可以關閉這些連線。 另一方面，SignalR 連接是*持續*性的。 即使在用戶端閒置時，SignalR 連線仍會保持開啟狀態。 在服務許多用戶端的高流量應用程式中，這些持續連線可能會導致伺服器達到連線的最大數目。

持續性連接也會耗用一些額外的記憶體，以追蹤每個連接。

藉由 SignalR 大量使用連線相關資源，可能會影響相同伺服器上裝載的其他 web 應用程式。 當 SignalR 開啟並保留最後一個可用的 TCP 連線時，相同伺服器上的其他 web 應用程式也不會有其他可用的連接。

如果伺服器用盡連線，您會看到隨機的通訊端錯誤和連線重設錯誤。 例如:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

若要讓 SignalR 資源使用不會造成其他 web 應用程式中的錯誤，請在與其他 web 應用程式不同的伺服器上執行 SignalR。

若要讓 SignalR 資源使用不會導致 SignalR 應用程式中發生錯誤，請相應放大以限制伺服器必須處理的連線數目。

## <a name="scale-out"></a>向外擴充

使用 SignalR 的應用程式需要追蹤其所有連線，這會造成伺服器陣列的問題。 新增伺服器，並取得其他伺服器不知道的新連接。 例如，下圖中每一部伺服器上的 SignalR 都不知道其他伺服器上的連接。 當其中一部伺服器上的 SignalR 想要將訊息傳送至所有用戶端時，訊息只會移至連線到該伺服器的用戶端。

![調整 [！OP.不含背板的無 LOC （SignalR）]](scale/_static/scale-no-backplane.png)

解決此問題的選項包括[Azure SignalR 服務](#azure-signalr-service)和[Redis 背板](#redis-backplane)。

## <a name="azure-opno-locsignalr-service"></a>Azure SignalR 服務

Azure SignalR 服務是一個 proxy，而不是背板。 每次用戶端起始與伺服器的連線時，會將用戶端重新導向以連接到服務。 該程式如下圖所示：

![建立與 Azure 的連線 [！OP.無 LOC （SignalR）] 服務](scale/_static/azure-signalr-service-one-connection.png)

結果是服務會管理所有用戶端連線，而每個伺服器只需要少量的服務連線，如下圖所示：

![連線到服務的用戶端連線到服務的伺服器](scale/_static/azure-signalr-service-multiple-connections.png)

這種向外延展的方法在 Redis 背板上有數個優點：

* 因為用戶端會在連線時立即重新導向至 Azure SignalR 服務，所以不需要像是「[用戶端親和性](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)」的粘滯話。
* SignalR 應用程式可以根據傳送的訊息數目相應放大，而 Azure SignalR 服務會自動調整以處理任何數目的連線。 例如，可能有數千個用戶端，但如果每秒只傳送幾則訊息，則 SignalR 應用程式不需要向外延展至多部伺服器，只是為了處理連接本身。
* SignalR 應用程式不會使用比 web 應用程式更多的連線資源，而不會 SignalR。

基於這些理由，針對裝載于 Azure 上的所有 ASP.NET Core SignalR 應用程式（包括 App Service、Vm 和容器），我們建議 Azure SignalR 服務。

如需詳細資訊，請參閱[Azure SignalR 服務檔](/azure/azure-signalr/signalr-overview)。

## <a name="redis-backplane"></a>Redis 後擋板

[Redis](https://redis.io/)是記憶體中的索引鍵/值存放區，可支援具有發行/訂閱模型的訊息系統。 SignalR Redis 背板會使用 pub/sub 功能將訊息轉送至其他伺服器。 當用戶端建立連線時，連線資訊會傳遞至背板。 當伺服器想要將訊息傳送至所有用戶端時，它會傳送至背板。 背板會知道所有連線的用戶端，以及它們所在的伺服器。 它會透過其各自的伺服器，將訊息傳送至所有用戶端。 下圖說明此程式：

![Redis 背板，從一部伺服器傳送至所有用戶端的訊息](scale/_static/redis-backplane.png)

對於裝載于您自己的基礎結構上的應用程式，Redis 背板是建議的向外延展方法。 由於您的資料中心與 Azure 資料中心之間的連線延遲，Azure SignalR 服務並不是實際使用內部部署應用程式的選項。

先前所述的 Azure SignalR 服務優點是 Redis 背板的缺點：

* 需要有粘滯話（也稱為[用戶端親和性](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)）。 一旦在伺服器上起始連接，連接就必須停留在該伺服器上。
* SignalR 應用程式必須根據用戶端數目進行相應放大，即使傳送的訊息很少也一樣。
* SignalR 應用程式所使用的連線資源比 web 應用程式更多，而沒有 SignalR。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [Azure SignalR 服務檔](/azure/azure-signalr/signalr-overview)
* [設定 Redis 背板](xref:signalr/redis-backplane)
