---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665746"
---
# <a name="error-handling-sample-application"></a>錯誤處理範例應用程式

此範例應用程式示範[在 ASP.NET Core 中處理錯誤](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling)主題中所述的案例。

## <a name="configure-a-custom-exception-handling-page"></a>設定自訂的例外狀況處理頁面

當應用程式不在開發環境中執行時，例外狀況處理中介軟體會：

* 攔截例外狀況。
* 記錄例外狀況。
* 在位於所提供路徑的替代管線中重新執行要求。

## <a name="configure-custom-exception-handling-code"></a>設定自訂例外狀況處理程式碼

為端點提供服務以處理具有自訂例外狀況處理頁面之錯誤的方式是提供 lambda 給 `UseExceptionHandler`。 搭配 `UseExceptionHandler` 使用 lambda 可讓您在傳回回應之前存取錯誤。

範例應用程式示範 `Startup.Configure` 中的自訂例外狀況處理程式碼。 依照 *Startup.cs* 檔案頂端的指示執行 (`LambdaErrorHandler`)。 在應用程式啟動之後，使用 [索引] 頁面上的 [擲回例外狀況] 連結觸發例外狀況。
