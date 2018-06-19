---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: 啟用 Windows 驗證中 Katana |Microsoft 文件
author: MikeWasson
description: 本文將說明如何啟用 Katana 中的 Windows 驗證。 它涵蓋了兩個案例： 使用 IIS 來裝載 Katana，以及使用 HttpListener 自我裝載 Kat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033966"
---
<a name="enabling-windows-authentication-in-katana"></a>Katana 中啟用 Windows 驗證
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 本文將說明如何啟用 Katana 中的 Windows 驗證。 它涵蓋了兩個案例： 使用 IIS 來裝載 Katana，以及使用 HttpListener 自我裝載 Katana 自訂程序中。 感謝您 Barry Dorrans、 David Matson 和 Chris Ross 檢閱這份文件。


Katana 是 Microsoft 實作[OWIN](http://owin.org/)，開啟 Web 介面 for.NET。 您可以閱讀簡介 OWIN 和 Katana[這裡](an-overview-of-project-katana.md)。 OWIN 架構分為數個層級：

- 主機： 管理 OWIN 管線執行所在的程序。
- 伺服器： 會開啟網路通訊端，並接聽要求。
- 中介軟體： 處理 HTTP 要求和回應。

Katana 目前提供兩個伺服器，這兩者都支援 Windows 整合式驗證：

- **Microsoft.Owin.Host.SystemWeb**. 使用 IIS 的 ASP.NET 管線。
- **Microsoft.Owin.Host.HttpListener**. 使用[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)。 此伺服器目前是預設選項自我裝載時 Katana。

> [!NOTE]
> Katana 目前未提供 OWIN 中介軟體的 Windows 驗證，因為這項功能已在伺服器可用。


## <a name="windows-authentication-in-iis"></a>在 IIS 中的 Windows 驗證

使用 Microsoft.Owin.Host.SystemWeb，您可以只啟用 IIS 中的 Windows 驗證。

讓我們開始建立新的 ASP.NET 應用程式，使用 「 ASP.NET 空 Web 應用程式 」 專案範本。

![](enabling-windows-authentication-in-katana/_static/image1.png)

接下來，新增 NuGet 封裝。 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

現在加入類別，名為`Startup`為下列程式碼：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

這是您只需要建立 owin，在 IIS 上執行"Hello world"應用程式。 按 F5，進行應用程式偵錯。 您應該會看到"Hello World ！" 在瀏覽器視窗中。

![](enabling-windows-authentication-in-katana/_static/image2.png)

接下來，我們會啟用 IIS Express 中的 Windows 驗證。 從**檢視**功能表上，選取**屬性**。 按一下 [方案總管]，檢視專案屬性中的專案名稱上。

在**屬性**視窗中，將**匿名驗證**至**已停用**並設定**Windows 驗證**至**啟用**。

![](enabling-windows-authentication-in-katana/_static/image3.png)

當您從 Visual Studio 執行應用程式時，則 IIS Express 會要求使用者的 Windows 認證。 您可以看到此使用[Fiddler](http://fiddler2.com/home)或另一個 HTTP 偵錯工具。 以下是範例 HTTP 回應：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Www-authenticate 標頭，在此回應指出伺服器支援[交涉](http://www.ietf.org/rfc/rfc4559.txt)使用 Kerberos 或 NTLM 通訊協定。

稍後，當您部署至伺服器應用程式，請遵循[步驟](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)該伺服器上啟用 IIS 中的 Windows 驗證。

## <a name="windows-authentication-in-httplistener"></a>HttpListener 中的 Windows 驗證

如果您使用來自我裝載 Katana Microsoft.Owin.Host.HttpListener，您可以直接上啟用 Windows 驗證**HttpListener**執行個體。

首先，建立新的主控台應用程式。 接下來，新增 NuGet 封裝。 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

現在加入類別，名為`Startup`為下列程式碼：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

這個類別會實作"Hello world"的範例相同，但它也會做為驗證配置設定 Windows 驗證。

內部`Main`函式中，啟動 OWIN 管線：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

您可以使用 Fiddler，以確認應用程式使用 Windows 驗證來傳送要求：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>相關主題

[Katana 專案概觀](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[了解 OWIN MVC 5 中的表單驗證](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
