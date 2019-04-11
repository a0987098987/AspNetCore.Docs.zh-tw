---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472335"
---
*  <span data-ttu-id="f6a7e-101">藉由執行下列命令來信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="f6a7e-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

    <span data-ttu-id="f6a7e-102">上述命令會顯示以下對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f6a7e-102">The preceding command displays the following dialog:</span></span>

    ![安全性警告對話方塊](~/getting-started/_static/cert.png)

*    <span data-ttu-id="f6a7e-104">若您同意信任開發憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f6a7e-104">Select **Yes** if you agree to trust the development certificate.</span></span>

     <span data-ttu-id="f6a7e-105">如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。</span><span class="sxs-lookup"><span data-stu-id="f6a7e-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>