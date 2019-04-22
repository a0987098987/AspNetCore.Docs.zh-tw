---
ms.openlocfilehash: 260f774fdba4d16a4fcb00ac1c699acf4d1bf5b5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615370"
---
* 藉由執行下列命令來信任 HTTPS 開發憑證：

  ```console
  dotnet dev-certs https --trust
  ```

  上述命令會顯示以下對話方塊：

  ![安全性警告對話方塊](~/getting-started/_static/cert.png)

* 若您同意信任開發憑證，請選取 [是]。

  如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。
  
