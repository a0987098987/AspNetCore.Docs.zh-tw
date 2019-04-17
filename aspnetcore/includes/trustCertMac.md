---
ms.openlocfilehash: 2ec079606cb48670dbc3852482fd8d401e7db44b
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472309"
---
* <span data-ttu-id="e14cf-101">藉由執行下列命令來信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="e14cf-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="e14cf-102">上述命令會顯示以下輸出：</span><span class="sxs-lookup"><span data-stu-id="e14cf-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="e14cf-103">出現提示時，請輸入系統管理員使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="e14cf-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="e14cf-104">該憑證現在將會被安裝且受到信任。</span><span class="sxs-lookup"><span data-stu-id="e14cf-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="e14cf-105">如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。</span><span class="sxs-lookup"><span data-stu-id="e14cf-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>