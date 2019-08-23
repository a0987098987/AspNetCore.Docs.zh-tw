---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用程式的負載測試和壓力測試的幾項值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975238"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core 負載/壓力測試

負載測試和壓力測試非常重要, 可確保 web 應用程式的效能和可擴充性。 它們的目標會不同, 即使它們經常共用類似的測試也一樣。

**負載測試**&ndash;測試應用程式是否可以在特定案例中處理指定的使用者負載, 同時仍然滿足回應目標。 應用程式會在正常情況下執行。

**壓力測試**&ndash;在極端情況下執行時, 測試應用程式的穩定性通常長時間。 測試會在應用程式上放置高使用者負載、尖峰或逐漸增加的負載, 或限制應用程式的運算資源。

壓力測試會判斷壓力不足的應用程式是否可以從失敗中復原, 並正常地回到預期的行為。 在壓力之下, 應用程式不會在正常情況下執行。

Visual Studio 2019 是具有負載測試功能 Visual Studio 的最後一個版本。 對於未來需要負載測試工具的客戶, 我們建議替代工具, 例如 Apache JMeter、Akamai CloudTest 和 BlazeMeter。 如需詳細資訊, 請參閱[Visual Studio 2019 版本](/visualstudio/releases/2019/release-notes-v16.0#test-tools)資訊。

Azure DevOps 中的負載測試服務將于2020結束。 如需詳細資訊, 請參閱[雲端負載測試服務生命週期結束](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/)。

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio 可讓使用者建立、開發和調試 web 效能和負載測試。 有一個選項可用來在網頁瀏覽器中錄製動作, 以建立測試。

如需如何使用 Visual Studio 2017 建立、設定及執行負載測試專案的詳細資訊, 請參閱[快速入門:建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。

負載測試可以設定為在內部部署執行, 或使用 Azure DevOps 在雲端中執行。

## <a name="azure-devops"></a>Azure DevOps

您可以使用[Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts)服務來啟動負載測試回合。

![Azure DevOps 負載測試登陸頁面](./load-tests/_static/azure-devops-load-test.png)

此服務支援下列測試格式:

* Visual Studio &ndash;在 Visual Studio 中建立的 Web 測試。
* 在測試期間, 將會重新執行 HTTP 封存在封存內捕獲的HTTP流量。&ndash;
* 以[URL 為基礎](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)&ndash;允許指定 url 來載入測試、要求類型、標頭和查詢字串。 您可以設定執行設定參數, 例如持續時間、負載模式和使用者數目。
* [Apache JMeter](https://jmeter.apache.org/)。

## <a name="azure-portal"></a>Azure 入口網站

Azure 入口網站允許直接從 Azure 入口網站中 App Service 的 [**效能**] 索引標籤[, 設定和執行 web 應用程式的負載測試](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)。

![Azure 入口網站中的 Azure App Service](./load-tests/_static/azure-appservice-perf-test.png)

測試可以是具有指定 URL 或 Visual Studio Web 測試檔案的手動測試, 可以測試多個 Url。

![Azure 入口網站上的 [新增效能測試] 頁面](./load-tests/_static/azure-appservice-perf-test-config.png)

在測試結束時, 產生的報表會顯示應用程式的效能特性。 範例統計資料包括:

* 平均回應時間
* 輸送量上限: 每秒的要求數
* 失敗百分比

## <a name="third-party-tools"></a>協力廠商工具

下列清單包含具有各種功能集的協力廠商 web 效能工具:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locust](https://locust.io/)
* [West 風 WebSurge](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
