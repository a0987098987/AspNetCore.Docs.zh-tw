---
title: ASP.NET Core 負載/壓力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用程式的負載測試和壓力測試的幾項值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 1fd77a767fb53b9276081dd712e13108094a0382
ms.sourcegitcommit: cb6015f737b6a93127016ab0f21b58e34b624ff3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/04/2020
ms.locfileid: "77004288"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core 負載/壓力測試

負載測試和壓力測試非常重要，可確保 web 應用程式的效能和可擴充性。 它們的目標會不同，即使它們經常共用類似的測試也一樣。

**負載測試**&ndash; 測試應用程式是否可以在特定案例中處理指定的使用者負載，同時仍然滿足回應目標。 應用程式會在正常情況下執行。

**壓力測試**在極端情況下執行時，&ndash; 測試應用程式穩定性，通常很長一段時間。 測試會在應用程式上放置高使用者負載、尖峰或逐漸增加的負載，或限制應用程式的運算資源。

壓力測試會判斷壓力不足的應用程式是否可以從失敗中復原，並正常地回到預期的行為。 在壓力之下，應用程式不會在正常情況下執行。

Visual Studio 2019 是具有負載測試功能 Visual Studio 的最後一個版本。 對於未來需要負載測試工具的客戶，我們建議替代工具，例如 Apache JMeter、Akamai CloudTest 和 BlazeMeter。 如需詳細資訊，請參閱[Visual Studio 2019 版本](/visualstudio/releases/2019/release-notes-v16.0#test-tools)資訊。

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio 可讓使用者建立、開發和調試 web 效能和負載測試。 有一個選項可用來在網頁瀏覽器中錄製動作，以建立測試。

如需如何使用 Visual Studio 2017 建立、設定及執行負載測試專案的詳細資訊，請參閱[快速入門：建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。

負載測試可以設定為在內部部署執行，或使用 Azure DevOps 在雲端中執行。

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

