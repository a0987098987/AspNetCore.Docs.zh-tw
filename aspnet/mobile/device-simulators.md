---
uid: mobile/device-simulators
title: 模擬熱門的行動裝置進行測試 |Microsoft Docs
author: rick-anderson
description: 您可以依照下列連結下載熱門的行動裝置和瀏覽器模擬器
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348373"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>模擬熱門的行動裝置進行測試

> 您可以依照下列連結下載熱門的行動裝置和瀏覽器模擬器。

| 裝置或瀏覽器 | 模擬器 / 模擬器 |
| --- | --- |
| BrowserStack 裝載瀏覽器虛擬化 ![BrowserStack 裝載瀏覽器虛擬化](device-simulators/_static/image1.png) | [BrowserStack 裝載瀏覽器虛擬化](http://browserstack.com)任何平台上的任何瀏覽器中測試您的本機或實際執行環境。 您可以在裝載虛擬機器中，您的電腦與 BrowserStack 網路之間建立 ipsec 通道。 請務必取得[BrowserStack Visual Studio 擴充功能](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack)更順暢的體驗。 |
| iPhone / iPod / iPad | [Electric 所研發的 Mobile Studio](http://www.electricplum.com/studio.aspx) iPhone 與 iPad 模擬器針對 Windows，以及回應式設計工具。 |
| Android | [Android Studio](https://developer.android.com/studio/)或[Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Opera Mobile 傳統模擬器](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 開發人員工具組](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en)請注意，為了讓電話網路存取，您也需要納入 VPC 網路介面卡[Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en)。 若要將 IE 的電話上到您的 Visual Studio 程式開發伺服器，請參閱[Kiran Patil 部落格文章](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/)。 |

> [!NOTE]
> 如果您想要在實際的裝置 （這是唯一的選項，進行完整測試 iPhone 或 iPad，因為沒有 Windows 沒有真正的模擬器） 上檢視您的應用程式必須裝載於 IIS 或 IIS Express 應用程式。 您輕鬆地無法使用為此，Visual Studio 的內建的 web 伺服器，因為它不會回應要求從其他電腦。