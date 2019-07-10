---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 了解幾個值得注意的工具和負載測試和壓力測試 ASP.NET Core 應用程式的方法。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 3c21da6c799bc3080a1a16cb62ae4535b8890a1b
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724485"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core 負載/壓力測試

負載測試和壓力測試很重要，可確保 web 應用程式是高效能且可調整。 其目標是不同的即使它們通常會共用類似的測試。

**負載測試**&ndash;測試是否應用程式可以處理的特定案例的使用者指定的負載，同時仍然可滿足回應目標。 在正常情況下執行的應用程式。

**壓力測試**&ndash;極端的條件，通常很長一段時間下執行時，測試應用程式穩定性。 測試高使用者負載尖峰或逐漸增加的負載，對應用程式，或者限制應用程式的運算資源。

壓力測試會判斷是否在壓力下的應用程式可以從失敗中復原，並依正常程序傳回給預期的行為。 壓力的情況下，不會在正常情況下執行應用程式。

Visual Studio 2019 是最後一個版本的 Visual Studio 負載測試功能。 對於需要負載測試工具在未來的客戶，我們建議替代工具，例如 Apache JMeter、 Akamai CloudTest 和 BlazeMeter。 如需詳細資訊，請參閱 < [Visual Studio 2019 版本資訊](/visualstudio/releases/2019/release-notes#test-tools)。

Azure DevOps 中測試服務的負載即將結束在 2020年。 如需詳細資訊，請參閱 <<c0> [ 雲端式負載測試服務生命週期結束的](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)。

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio 可讓使用者建立、 開發及偵錯 web 效能和負載測試。 會提供透過網頁瀏覽器中錄製動作建立測試的選項。

如需如何建立、 設定和執行負載測試使用 Visual Studio 2017 的專案資訊，請參閱[快速入門：建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。 如需詳細資訊，請參閱 <<c0> [ 額外的資源](#additional-resources)一節。

若要使用 Azure DevOps 的雲端中執行內部部署或執行，您可以設定負載測試。

## <a name="azure-devops"></a>Azure DevOps

可以使用啟動負載測試回合[Azure DevOps 測試計劃](/azure/devops/test/load-test/index?view=vsts)服務。

![Azure 的 DevOps 負載測試登陸頁面](./load-tests/_static/azure-devops-load-test.png)

服務支援下列測試格式：

* Visual Studio &ndash; Visual Studio 中建立的 Web 測試。
* HTTP 封存&ndash;封存內的擷取 HTTP 流量會重新執行測試期間。
* [URL 型](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)&ndash;可讓您指定負載測試中，要求類型、 標頭和查詢字串的 Url。 執行設定的參數，例如持續時間，負載模式和使用者人數可以設定。
* [Apache JMeter](https://jmeter.apache.org/)。

## <a name="azure-portal"></a>Azure 入口網站

[設定及執行負載測試的 web 應用程式，可讓 azure 入口網站](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)直接從**效能**在 Azure 入口網站中的應用程式服務 索引標籤。

![在 Azure 入口網站中的 azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

測試可以是具有指定的 URL 或 Visual Studio Web 測試檔案，其中可以測試多個 Url 的手動測試。

![在 Azure 入口網站上新的效能測試頁面](./load-tests/_static/azure-appservice-perf-test-config.png)

在測試結束時，產生的報表會顯示應用程式的效能特性。 範例統計資料包括：

* 平均回應時間
* 最大輸送量： 每秒要求數
* 失敗百分比

## <a name="third-party-tools"></a>第三方工具

下列清單包含使用各種不同的功能集的第三方 web 效能工具：

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locust](https://locust.io/)
* [West Wind WebSurge](http://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)

## <a name="additional-resources"></a>其他資源

* [載入測試部落格系列](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
