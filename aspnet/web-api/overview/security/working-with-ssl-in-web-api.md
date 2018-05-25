---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: 使用 Web 應用程式開發介面中的 SSL |Microsoft 文件
author: MikeWasson
description: 示範如何使用 SSL 與 ASP.NET Web API，包括使用 SSL 用戶端憑證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a>使用 Web 應用程式開發介面中的 SSL
====================
由[Mike Wasson](https://github.com/MikeWasson)

透過一般 HTTP，不安全幾個常見的驗證配置。 特別是，基本驗證和表單驗證來傳送未加密的認證。 為安全的這些驗證配置*必須*使用 SSL。 此外，SSL 用戶端憑證可以用來驗證用戶端。

## <a name="enabling-ssl-on-the-server"></a>啟用在伺服器上的 SSL

若要設定 SSL，在 IIS 7 或更新版本：

- 建立或取得憑證。 為了測試，您可以建立自我簽署的憑證。
- 加入 HTTPS 繫結。

如需詳細資訊，請參閱[如何 IIS 7 上設定 SSL](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。

本機測試，您可以啟用 IIS Express 從 Visual Studio 中的 SSL。 在 [屬性] 視窗中，設定**啟用 SSL**至**True**。 請注意的**SSL URL**; 用於這個 URL 測試 HTTPS 連線。

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>強制執行此 Web API 控制器中的 SSL

如果您有 HTTPS 和 HTTP 繫結，用戶端仍然可以使用 HTTP 來存取網站。 您可能會允許可透過 HTTP，而其他資源則需要 SSL 的一些資源。 在此情況下，若需要 SSL 的受保護的資源使用動作篩選條件。 下列程式碼將示範會檢查有 SSL 的 Web API 驗證篩選條件：

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

將此篩選條件加入至任何要求使用 SSL 的 Web API 動作：

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL 用戶端憑證

SSL 憑證公開金鑰基礎結構來提供驗證。 伺服器必須提供憑證來驗證伺服器到用戶端。 較不常見的用戶端提供的憑證伺服器，但這是其中一個選項來驗證用戶端。 若要使用 SSL 用戶端憑證，您需要將簽署的憑證分送到您的使用者的方式。 對於許多應用程式類型，這是良好的使用者經驗，但在某些環境中 （例如，企業） 可能可行。

| 優點 | 缺點 |
| --- | --- |
| -憑證認證的使用者名稱/密碼比更強的。 SSL 提供完整的安全通道，驗證、 訊息完整性與加密訊息。 | -您必須取得並管理 PKI 憑證。 -用戶端平台必須支援 SSL 用戶端憑證。 |

若要設定 IIS 以接受用戶端憑證，請開啟 IIS 管理員，然後執行下列步驟：

1. 按一下樹狀檢視中的 [網站] 節點。
2. 按兩下**SSL 設定**在中間窗格中的功能。
3. 在下**用戶端憑證**，選取其中一個選項： 

    - **接受**: IIS 將會接受來自用戶端的憑證，而不是需要。
    - **需要**： 需要用戶端憑證。 （若要啟用此選項，您也必須選取 [需要 SSL]）

您也可以在 ApplicationHost.config 檔案中設定這些選項：

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert**旗標表示 IIS 會接受來自用戶端的憑證，但不需要 （相當於 接受 選項在 IIS 管理員）。 若要要求憑證，設定**SslRequireCert**旗標。 為了測試，您也可以設定這些選項在 IIS Express 中，本機 applicationhost 中。組態檔中，位於 「 Documents\IISExpress\config"。

### <a name="creating-a-client-certificate-for-testing"></a>建立用戶端憑證以進行測試

為了測試用途，您可以使用[MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx)建立用戶端憑證。 首先，建立測試根授權單位：

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert 會提示您輸入私密金鑰的密碼。

接下來，將憑證加入至測試伺服器的 「 信任的根憑證授權單位 存放區，如下：

1. 開啟 MMC。
2. 在下**檔案**，選取**新增/移除嵌入式管理單元**。
3. 選取**電腦帳戶**。
4. 選取**本機電腦**並完成精靈。
5. 在瀏覽 窗格中，展開 信任根憑證授權單位 節點。
6. 在**動作**功能表上，指向**所有工作**，然後按一下 **匯入**啟動 憑證匯入精靈。
7. 瀏覽憑證檔案，TempCA.cer。
8. 按一下**開啟**，然後按一下 **下一步**並完成精靈。 （系統會提示您重新輸入密碼。）

現在建立之第一個憑證所簽署的用戶端憑證：

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Web API 中使用用戶端憑證

在伺服器端，您可以取得用戶端憑證，藉由呼叫[GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx)要求訊息。 方法會傳回 null，如果沒有用戶端憑證。 否則，它會傳回**X509Certificate2**執行個體。 使用此物件，取得憑證，例如簽發者和主旨的相關資訊。 然後您可以使用這項資訊的驗證及/或授權。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
