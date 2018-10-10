---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: 啟用在 Katana 中的 Windows 驗證 |Microsoft Docs
author: MikeWasson
description: 本文說明如何啟用 Windows 驗證，在 Katana 中。 它涵蓋了兩種案例： 使用 IIS 來裝載 Katana，並使用 HttpListener 自我裝載 Kat...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8afa2c9dfbe03a9874513f7d083adf7608f4218f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910456"
---
<a name="enabling-windows-authentication-in-katana"></a>在 Katana 中啟用 Windows 驗證
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 本文說明如何啟用 Windows 驗證，在 Katana 中。 它涵蓋了兩種案例： 使用 IIS 來裝載 Katana，並使用 HttpListener 自我裝載的自訂處理序中的 Katana。 感謝您 Barry Dorrans、 David Matson 和 Chris Ross 閱本篇文章。


Katana 是 Microsoft 的實作[OWIN](http://owin.org/)，Open Web Interface for.NET。 您可以閱讀簡介 OWIN 和 Katana[此處](an-overview-of-project-katana.md)。 OWIN 架構有數個層級：

- 主機： 管理程序中執行 OWIN 管線。
- 伺服器： 會開啟網路通訊端，並接聽要求。
- 中介軟體： 處理 HTTP 要求和回應。

Katana 目前提供兩部伺服器，這兩者都支援 Windows 整合式驗證：

- **Microsoft.Owin.Host.SystemWeb**。 使用 IIS 與 ASP.NET 管線。
- **Microsoft.Owin.Host.HttpListener**。 會使用[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)。 此伺服器目前是預設選項自我裝載時 Katana。

> [!NOTE]
> Katana 目前不提供 OWIN 中介軟體進行 Windows 驗證，因為這項功能已在伺服器中可用。

## <a name="windows-authentication-in-iis"></a>在 IIS 中的 Windows 驗證

使用 Microsoft.Owin.Host.SystemWeb，您可以只啟用 IIS 中的 Windows 驗證。

現在就開始建立新的 ASP.NET 應用程式，使用 「 ASP.NET 空白 Web 應用程式 」 專案範本。

![](enabling-windows-authentication-in-katana/_static/image1.png)

接下來，新增 NuGet 套件。 從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

現在加入類別，名為`Startup`為下列程式碼：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

這是您只需要建立 OWIN，在 IIS 上執行"Hello world"應用程式。 按 F5，進行應用程式偵錯。 您應該會看到"Hello World ！" 在瀏覽器視窗中。

![](enabling-windows-authentication-in-katana/_static/image2.png)

接下來，我們將啟用 IIS Express 中的 Windows 驗證。 從**檢視**功能表上，選取**屬性**。 按一下 [方案總管]，檢視專案屬性中的專案名稱上。

中**屬性**視窗中，將**匿名驗證**來**已停用**並將**Windows 驗證**到**啟用**。

![](enabling-windows-authentication-in-katana/_static/image3.png)

當您從 Visual Studio 執行應用程式時，則 IIS Express 會要求使用者的 Windows 認證。 您可以使用來查看這[Fiddler](http://fiddler2.com/home)或另一個 HTTP 偵錯工具。 以下是範例 HTTP 回應：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Www-authenticate 標頭，此回應指出伺服器支援[交涉](http://www.ietf.org/rfc/rfc4559.txt)通訊協定，它會使用 Kerberos 或 NTLM。

稍後，當您部署至伺服器應用程式，請遵循[這些步驟](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)該伺服器上啟用 IIS 中的 Windows 驗證。

## <a name="windows-authentication-in-httplistener"></a>HttpListener 中的 Windows 驗證

如果您使用 Microsoft.Owin.Host.HttpListener 自我裝載 Katana，您可以直接上啟用 Windows 驗證**HttpListener**執行個體。

首先，建立新的主控台應用程式。 接下來，新增 NuGet 套件。 從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

現在加入類別，名為`Startup`為下列程式碼：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

這個類別會實作"Hello world"的範例相同，但它也會做為驗證配置設定 Windows 驗證。

內`Main`函式中，啟動 OWIN 管線：

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

您可以使用 Fiddler 以確認應用程式會使用 Windows 驗證來傳送要求：

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>相關主題

[Katana 專案概觀](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[了解 OWIN MVC 5 中的表單驗證](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
