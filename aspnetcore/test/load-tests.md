---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 描述幾個值得注意的工具和負載測試和壓力測試 ASP.NET Core 應用程式的方法。
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 0a53405cba19435a74b398ba42a05456c50bdc72
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099478"
---
# <a name="load-and-stress-testing-aspnet-core"></a>負載和壓力測試的 ASP.NET Core

負載測試和壓力測試很重要，可確保 web 應用程式是高效能且可調整。 其目標是不同甚至是他們通常會共用類似的測試。

**負載測試**:測試是否應用程式可以處理的特定案例的使用者指定的負載，同時仍然可滿足回應目標。 在正常情況下執行的應用程式。

**壓力測試**:測試應用程式穩定性下執行，就極端的情況，而且通常很長一段時間時：

* 高使用者負載 – 尖峰或逐漸增加。
* 有限的運算資源。  

壓力的情況下，可以應用程式從失敗中復原並依正常程序傳回預期的行為？ 在正常情況下執行的應用程式。

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio 可讓使用者建立、 開發及偵錯 web 效能和負載測試。 會提供透過網頁瀏覽器中錄製動作建立測試的選項。

[快速入門：建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)示範如何建立、 設定和執行負載測試使用 Visual Studio 2017 的專案。

如需詳細資訊，請參閱[其他資源](#add)。

在內部部署中執行，或使用 Azure DevOps 的雲端中執行，您可以設定負載測試。

## <a name="azure-devops"></a>Azure 的 DevOps

可以使用啟動負載測試回合[Azure DevOps 測試計劃](/azure/devops/test/load-test/index?view=vsts)服務。

![](./load-tests/_static/azure-devops-load-test.png)

服務支援下列類型的測試格式：

- Visual Studio 測試，– Visual Studio 中建立的 web 測試。
- HTTP 封存式測試 – 封存內擷取的 HTTP 流量是在測試期間重新執行。
- [URL 為基礎的測試](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts)– 可讓您指定負載測試中，要求類型、 標頭和查詢字串的 Url。 執行設定的參數，例如持續時間，您可以設定負載模式、 使用者、 等等的數目。
- [Apache JMeter](https://jmeter.apache.org/)測試。

## <a name="azure-portal"></a>Azure 入口網站

[Azure 入口網站可讓設定及執行負載測試的 Web 應用程式，](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts)直接從 Azure 入口網站中的 App Service 中的 [效能] 索引標籤。

![](./load-tests/_static/azure-appservice-perf-test.png)

測試可以是具有指定的 URL 或 Visual Studio Web 測試檔案，其中可以測試多個 Url 的手動測試。

![](./load-tests/_static/azure-appservice-perf-test-config.png)

在測試結束時，會產生報表以顯示應用程式的效能特性。 範例統計資料包括：

- 平均回應時間
- 最大輸送量： 每秒要求數
- 失敗百分比

## <a name="third-party-tools"></a>第三方工具

下列清單包含使用各種不同的功能集的第三方 web 效能工具：

- [Apache JMeter](https://jmeter.apache.org/) :負載測試工具的完整功能的套件。 執行緒繫結： 需要每位使用者的一個執行緒。
- [ab-Apache HTTP 伺服器效能評定工具](https://httpd.apache.org/docs/2.4/programs/ab.html)
- [Gatling](https://gatling.io/) :使用 GUI 和測試的燒錄的桌面工具。 效能比 JMeter 的更好。
- [Locust.io](https://locust.io/) :不受限制的執行緒。

<a name="add"></a>
## <a name="additional-resources"></a>其他資源

[載入測試部落格系列](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)由 Charlie sterling 主持。 帶有但仍有相關之大部分主題。