---
ms.openlocfilehash: 209b5c41e17897693962954b1e795bdbb41f9384
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012730"
---
# <a name="key-vault-configuration-provider-sample-app"></a><span data-ttu-id="bf9f1-101">金鑰保存庫組態提供者範例應用程式</span><span class="sxs-lookup"><span data-stu-id="bf9f1-101">Key Vault Configuration Provider Sample App</span></span>

<span data-ttu-id="bf9f1-102">此範例說明如何使用 Azure 金鑰保存庫的組態提供者。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-102">This sample illustrates the use of the Azure Key Vault Configuration Provider.</span></span>

<span data-ttu-id="bf9f1-103">這個範例會執行所決定的兩個模式之一`#define`陳述式，在頂端*Program.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-103">The sample runs in one of two modes determined by the `#define` statement at the top of the *Program.cs* file.</span></span> <span data-ttu-id="bf9f1-104">如需相關指示，請參閱 <<c0> [ 範例程式碼中的前置處理器指示詞](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):</span><span class="sxs-lookup"><span data-stu-id="bf9f1-104">For instructions, see [Preprocessor directives in sample code](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):</span></span>

* <span data-ttu-id="bf9f1-105">`Certificate` &ndash; 示範如何使用 Azure Key Vault 中儲存的存取祕密的 Azure 金鑰保存庫用戶端識別碼和 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-105">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="bf9f1-106">從任何位置，部署至 Azure App Service 或任何能夠為 ASP.NET Core 應用程式的主機，可以執行此版本的範例。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-106">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="bf9f1-107">`Managed` &ndash; 示範如何使用 Azure[受控服務識別](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)驗證的應用程式至 Azure Key Vault 與 Azure AD 驗證，而不需要在應用程式的程式碼或組態中的認證。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-107">`Managed` &ndash; Demonstrates how to use Azure's [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials in the app's code or configuration.</span></span> <span data-ttu-id="bf9f1-108">Azure AD 用戶端識別碼和密碼不需要使用 Azure Key Vault 進行驗證的應用程式。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-108">An Azure AD Client ID and Secret aren't required for the app to authenticate with Azure Key Vault.</span></span> <span data-ttu-id="bf9f1-109">此範例必須部署至 Azure App Service，以探索受管理的身分識別 scearnio。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-109">This sample must be deployed to Azure App Service to explore the Managed Identity scearnio.</span></span>

<span data-ttu-id="bf9f1-110">如需詳細資訊，請參閱 < [Azure 金鑰保存庫組態提供者](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)。</span><span class="sxs-lookup"><span data-stu-id="bf9f1-110">For more information, see [Azure Key Vault Configuration Provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).</span></span>
