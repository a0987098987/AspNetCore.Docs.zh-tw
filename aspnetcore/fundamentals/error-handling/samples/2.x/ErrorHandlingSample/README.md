---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665746"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="a057b-101">錯誤處理範例應用程式</span><span class="sxs-lookup"><span data-stu-id="a057b-101">Error Handling Sample Application</span></span>

<span data-ttu-id="a057b-102">此範例應用程式示範[在 ASP.NET Core 中處理錯誤](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling)主題中所述的案例。</span><span class="sxs-lookup"><span data-stu-id="a057b-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="a057b-103">設定自訂的例外狀況處理頁面</span><span class="sxs-lookup"><span data-stu-id="a057b-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="a057b-104">當應用程式不在開發環境中執行時，例外狀況處理中介軟體會：</span><span class="sxs-lookup"><span data-stu-id="a057b-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="a057b-105">攔截例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a057b-105">Catches exceptions.</span></span>
* <span data-ttu-id="a057b-106">記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a057b-106">Logs exceptions.</span></span>
* <span data-ttu-id="a057b-107">在位於所提供路徑的替代管線中重新執行要求。</span><span class="sxs-lookup"><span data-stu-id="a057b-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="a057b-108">設定自訂例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="a057b-108">Configure custom exception handling code</span></span>

<span data-ttu-id="a057b-109">為端點提供服務以處理具有自訂例外狀況處理頁面之錯誤的方式是提供 lambda 給 `UseExceptionHandler`。</span><span class="sxs-lookup"><span data-stu-id="a057b-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="a057b-110">搭配 `UseExceptionHandler` 使用 lambda 可讓您在傳回回應之前存取錯誤。</span><span class="sxs-lookup"><span data-stu-id="a057b-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="a057b-111">範例應用程式示範 `Startup.Configure` 中的自訂例外狀況處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="a057b-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="a057b-112">依照 *Startup.cs* 檔案頂端的指示執行 (`LambdaErrorHandler`)。</span><span class="sxs-lookup"><span data-stu-id="a057b-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="a057b-113">在應用程式啟動之後，使用 [索引] 頁面上的 [擲回例外狀況] 連結觸發例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a057b-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
