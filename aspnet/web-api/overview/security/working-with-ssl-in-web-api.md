---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: 使用 Web API 中的 SSL |Microsoft Docs
author: MikeWasson
description: 示範如何使用 SSL 與 ASP.NET Web API，包括使用 SSL 用戶端憑證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4e14607f11fcd376b4ceca4bf7862259abe015e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396626"
---
<a name="working-with-ssl-in-web-api"></a>使用 Web API 中的 SSL
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

在一般的 HTTP 是不安全的數個常見的驗證配置。 特別是，基本驗證和表單驗證來傳送未加密的認證。 為安全，這些驗證結構描述*必須*使用 SSL。 此外，SSL 用戶端憑證可用來驗證用戶端。

## <a name="enabling-ssl-on-the-server"></a>啟用在伺服器上的 SSL

若要設定 SSL，在 IIS 7 或更新版本：

- 建立或取得憑證。 進行測試，您可以建立自我簽署的憑證。
- 新增 HTTPS 繫結。

如需詳細資訊，請參閱 < [IIS 7 上設定 SSL 如何](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)。

本機測試，您可以讓 IIS Express 從 Visual Studio 中的 SSL。 在 [屬性] 視窗中，設定**啟用 SSL**要 **，則為 True**。 請記下的值**SSL URL**; 用於這個 URL 測試 HTTPS 連線。

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>強制執行 Web API 控制器中的 SSL

如果您有 HTTPS 和 HTTP 繫結，用戶端仍然可以使用 HTTP 來存取網站。 您可能會允許一些資源，可透過 HTTP，而其他資源需要 SSL。 在此情況下，使用動作篩選條件來要求使用 SSL，受保護的資源。 下列程式碼顯示檢查 SSL 的 Web API 驗證篩選條件：

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

此篩選器加入任何需要 SSL 的 Web API 動作：

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL 用戶端憑證

SSL 會使用公開金鑰基礎結構憑證，以提供驗證。 伺服器必須提供憑證來驗證伺服器到用戶端。 較不常見的用戶端提供憑證給伺服器，但這是其中一個選項來驗證用戶端。 若要使用 SSL 用戶端憑證，您需要將簽署的憑證分送到您的使用者的方式。 對於許多應用程式類型，這不是良好的使用者經驗，但在某些環境中 （例如，企業） 可能可行。

| 優點 | 缺點 |
| --- | --- |
| -憑證認證是使用者名稱/密碼比更強。 SSL 提供完整的安全通道，驗證、 訊息完整性與加密訊息。 | -您必須取得並管理 PKI 憑證。 -用戶端平台必須支援 SSL 用戶端憑證。 |

若要設定 IIS 以接受用戶端憑證，請開啟 IIS 管理員，並執行下列步驟：

1. 按一下樹狀檢視中的 [網站] 節點。
2. 按兩下**SSL 設定**在中間窗格中的功能。
3. 底下**用戶端憑證**，選取其中一個選項： 

    - **接受**: IIS 會接受來自用戶端的憑證，而不需要。
    - **需要**： 需要用戶端憑證。 （若要啟用此選項，您也必須選取 [需要 SSL]）

您也可以在 ApplicationHost.config 檔案中設定這些選項：

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert**旗標表示 IIS 將會接受來自用戶端的憑證，但不需要 （相當於 「 Accept 」 選項在 [IIS 管理員] 中）。 若要要求憑證，設定**SslRequireCert**旗標。 進行測試，您也可以設定這些選項在 IIS Express 中，在本機的 applicationhost。組態檔中，位於 「 Documents\IISExpress\config"。

### <a name="creating-a-client-certificate-for-testing"></a>建立用戶端憑證進行測試

基於測試目的，您可以使用[MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx)建立用戶端憑證。 首先，建立測試根授權單位：

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert 會提示您輸入私密金鑰的密碼。

接下來，將憑證新增至測試伺服器的 「 受信任的根憑證授權單位 存放區，，如下所示：

1. 開啟 MMC。
2. 底下**檔案**，選取**新增/移除嵌入式管理單元**。
3. 選取 **電腦帳戶**。
4. 選取 **本機電腦**並完成精靈。
5. 在 導覽 窗格中，展開 「 受信任的根憑證授權單位 節點。
6. 在上**動作**功能表上，指向**所有工作**，然後按一下 **匯入**以啟動 憑證匯入精靈。
7. 瀏覽至憑證檔案，TempCA.cer。
8. 按一下 [**開放**，然後按一下**下一步]** 並完成精靈。 （將會提示您重新輸入密碼。）

現在建立第一個憑證簽署的用戶端憑證：

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>使用 Web API 中的用戶端憑證

在伺服器端，您可以藉由呼叫取得的用戶端憑證[GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx)要求訊息。 方法會傳回 null，如果沒有任何用戶端憑證。 否則，它會傳回**X509Certificate2**執行個體。 使用此物件來取得憑證，例如簽發者和主旨的資訊。 然後您可以使用這項資訊的驗證和/或授權。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
