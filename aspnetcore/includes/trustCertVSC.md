---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472335"
---
*  藉由執行下列命令來信任 HTTPS 開發憑證：

    ```console
    dotnet dev-certs https --trust
    ```

    上述命令會顯示以下對話方塊：

    ![安全性警告對話方塊](~/getting-started/_static/cert.png)

*    若您同意信任開發憑證，請選取 [是]。

     如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。