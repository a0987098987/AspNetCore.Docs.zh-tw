---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用程式的負載測試和壓力測試的幾項值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: test/loadtests
ms.openlocfilehash: 0ec69ad783a4e545ea95ddcb928d03ba6a2e0050
ms.sourcegitcommit: 4437f4c149f1ef6c28796dcfaa2863b4c088169c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85074383"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core 負載/壓力測試

負載測試和壓力測試非常重要，可確保 web 應用程式的效能和可擴充性。 它們的目標會不同，即使它們經常共用類似的測試也一樣。

**負載測試**：測試應用程式是否可以在特定案例中處理指定的使用者負載，同時仍然滿足回應目標。 應用程式會在正常情況下執行。

**壓力測試**：在極端情況下執行時，測試應用程式穩定性通常長時間執行。 測試會在應用程式上放置高使用者負載、尖峰或逐漸增加的負載，或限制應用程式的運算資源。

壓力測試會判斷壓力不足的應用程式是否可以從失敗中復原，並正常地回到預期的行為。 在壓力之下，應用程式不會在正常情況下執行。

Visual Studio 2019 宣佈[取代負載測試的](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)計畫。 對應的 Azure DevOps 雲端式負載測試服務已關閉。

## <a name="third-party-tools"></a>協力廠商工具

下列清單包含具有各種功能集的協力廠商 web 效能工具：

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench （ab）](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [k6](https://k6.io)
* [Locust](https://locust.io/)
* [West 風 WebSurge](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)