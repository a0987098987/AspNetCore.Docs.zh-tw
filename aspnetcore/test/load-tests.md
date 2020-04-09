---
title: ASP.NET核心負載/應力測試
author: Jeremy-Meng
description: 瞭解 ASP.NET Core 應用的負載測試和壓力測試的幾個值得注意的工具和方法。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 1fd77a767fb53b9276081dd712e13108094a0382
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664686"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET核心負載/應力測試

負載測試和壓力測試對於確保 Web 應用具有可執行性和可擴充性非常重要。 他們的目標是不同的,即使他們經常分享類似的測試。

**負載測試**&ndash;測試應用是否可以處理特定方案指定的使用者負載,同時仍滿足回應目標。 該應用程式在正常情況下運行。

**壓力測試**&ndash;測試在極端條件下運行時測試應用的穩定性,通常長時間運行。 測試將高使用者負載(峰值或逐漸增加負載)放在應用上,或者限制應用的計算資源。

壓力測試確定受壓力的應用是否可以從故障中恢復,並優雅地返回到預期行為。 在壓力下,應用程式在正常情況下無法運行。

Visual Studio 2019 是具有負載測試功能的 Visual Studio 的最後一個版本。 對於將來需要負載測試工具的客戶,我們建議使用備用工具,如 Apache JMeter、Akamai 雲端測試和 BlazeMeter。 有關詳細資訊,請參閱 Visual [Studio 2019 發行說明](/visualstudio/releases/2019/release-notes-v16.0#test-tools)。

## <a name="visual-studio-tools"></a>Visual Studio 工具

Visual Studio 允許用戶創建、開發和調試 Web 性能和負載測試。 可透過在 Web 瀏覽器中記錄操作來創建測試的選項。

有關如何使用 Visual Studio 2017 建立、設定和執行負載測試專案的資訊,請參閱[快速入門:建立負載測試專案](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017)。

負載測試可以配置為在內部運行或使用 Azure DevOps 在雲端中執行。

## <a name="third-party-tools"></a>第三方工具

以下清單包含具有各種功能集的第三方 Web 效能工具:

* [Apache JMeter](https://jmeter.apache.org/)
* [阿帕奇Bench(ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [加特林](https://gatling.io/)
* [k6](https://k6.io)
* [蝗蟲](https://locust.io/)
* [西風網浪](https://websurge.west-wind.com/)
* [網林](https://github.com/hallatore/Netling)
* [韋加塔](https://github.com/tsenart/vegeta)

