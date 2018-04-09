---
title: 整合測試中 ASP.NET Core
author: ardalis
description: 如何使用 ASP.NET Core 整合測試，以確保應用程式的元件可以正確運作。
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 3c618b2bd5919f6536601631eb4d21359a6bc03a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="integration-tests-in-aspnet-core"></a>整合測試中 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/)

整合測試，可確保應用程式的元件正確運作時組合在一起。 ASP.NET Core 支援使用單元測試架構和內建測試 Web 主機來進行整合測試，此 Web 主機可以用來處理要求而不會有網路額外負荷。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>整合測試的簡介

整合測試會驗證，應用程式的不同部分正確一同運作。 不同於[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)，整合測試通常牽涉到的應用程式基礎結構考量，例如資料庫、 檔案系統、 網路資源或 web 要求和回應。 單元測試使用 fakes 或模擬物件來取代這些考量，但整合測試的目的是要確認系統可以與這些系統所正常運作。

整合測試，因為它們執行較大的程式碼區段，而且它們是依賴基礎結構項目，通常的數量級低於單元測試。 因此，最好限制多少的整合測試您所撰寫，尤其是您可以使用單元測試中測試相同的行為。

> [!NOTE]
> 如果某些行為可使用單元測試或整合測試進行測試，會想要單元測試，因為它將會幾乎一律會比較快。 您可能會有數十份或數百個單元測試，以許多不同的輸入，但只要少數幾個整合測試涵蓋範圍的最重要的案例。

測試您自己的方法內的邏輯通常是網域的單元測試。 測試您的應用程式架構，例如與 ASP.NET Core 或資料庫中的運作方式是整合測試的位置開始執行。 它並不需要太多的整合測試，以確認您能夠寫入資料庫中的資料列和讀回。 您不需要測試每個可能的排列的資料存取程式碼，您只需要測試足以提供您的應用程式正常運作的信心。

## <a name="integration-testing-aspnet-core"></a>整合測試的 ASP.NET Core

若要取得設定執行的整合測試，您將需要建立測試專案，請將參考加入 ASP.NET Core web 專案，並安裝的測試執行器。 此程序所述[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)文件，以及執行測試和建議命名您的測試和測試類別的詳細指示。

> [!NOTE]
> 將您的單元測試和整合測試使用不同的專案。 這有助於確保您不小心引入基礎結構考量單元測試，並可讓您輕鬆地選擇要執行的測試設定。

### <a name="the-test-host"></a>測試主機

ASP.NET Core 包含可加入至整合測試專案的測試主機來裝載 ASP.NET Core 應用程式使用，而不需要實際的 web 主機要求的服務測試。 提供的範例包括整合測試專案已設定使用[xUnit](https://xunit.github.io)和測試的主機。 它會使用`Microsoft.AspNetCore.TestHost`NuGet 封裝。

一次`Microsoft.AspNetCore.TestHost`封裝包含在專案中，您便可以建立及設定`TestServer`在您的測試。 下列測試顯示如何以確認站台的根提出的要求會傳回"Hello World ！" 和應該成功地對執行預設 Visual Studio 所建立的 ASP.NET Core 空白網站範本。

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

這項測試會使用排列 Act Assert 模式。 排列步驟會在建構函式，建立的執行個體中完成`TestServer`。 設定`WebHostBuilder`將用來建立`TestHost`; 在此範例中，`Configure`來自待測系統 (SUT) 的方法`Startup`類別會傳遞至`WebHostBuilder`。 這個方法會用來設定的要求管線`TestServer`相同至 SUT 伺服器會設定方式。

在測試動作部分，請發出要求來`TestServer`"/"路徑，和回應的執行個體重新讀入字串。 與預期的"Hello World ！"的字串，這個字串進行比較。 如果兩者相符，測試就會成功。否則，它會失敗。

現在您可以加入一些其他的整合測試，以確認質數檢查功能，適用於透過 web 應用程式：

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

請注意，您不會真的想要測試正確性的質數檢查程式，而是，但是這些測試與 web 應用程式正在執行您的預期。 您已有單元測試涵蓋範圍，可讓您在中的信心`PrimeService`，您可以在這裡看到如下：

![測試總管](integration-testing/_static/test-explorer.png)

您可以進一步了解中的單元測試[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)發行項。


### <a name="integration-testing-mvcrazor"></a>測試 Mvc/Razor 的整合

含有 Razor 檢視的測試專案需要`<PreserveCompilationContext>`設為 true *.csproj*檔案：


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

專案遺漏這個項目會產生類似下面的錯誤：
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>若要使用的中介軟體中重構

重構是變更以改善其設計不會變更其行為的應用程式的程式碼的程序。 在理想情況下應該有一套傳遞測試，因為這些協助確保系統行為保持不變之前和之後所做的變更時。 查看所在的 web 應用程式中實作檢查邏輯質數方式`Configure`方法，您看到：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

此程式碼可以運作，但很遠低於您希望如何實作 ASP.NET Core 應用程式中，即使一樣簡單因為這是這種功能。 假設有什麼`Configure`方法看起來會像視每次您加入另一個 URL 端點加入這麼多的程式碼 ！

要考慮的其中一個選項將加入[MVC](xref:mvc/overview)應用程式，以及建立控制站來處理的基本檢查。 不過，假設您目前不需要任何其他 MVC 功能，位元 overkill。

您可以不過，利用 ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)，這將協助我們封裝質數檢查它自己的類別中的邏輯，並達成更好[的重要性分離](http://deviq.com/separation-of-concerns/)中`Configure`方法。

您想要允許讓中介軟體類別必須是做為參數，指定用於中介軟體路徑`RequestDelegate`和`PrimeCheckerOptions`其建構函式中的執行個體。 如果要求路徑不符合此中介軟體的是設定為預期，您只需呼叫鏈結中的下一個中介軟體，並且進行進一步動作。 中的實作程式碼的其餘部分`Configure`現在處於`Invoke`方法。

> [!NOTE]
> 由於中介軟體取決於`PrimeService`服務，您也正在要求的建構函式與此服務執行個體。 架構會提供此服務透過[相依性插入](xref:fundamentals/dependency-injection)，假設它已設定，例如在`ConfigureServices`。

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

其路徑會比對時，此中介軟體都可做為要求委派鏈結中的端點，因為沒有要呼叫`_next.Invoke`當此中介軟體會處理要求。

與此中介軟體，而且某些很有幫助擴充方法建立，以簡化設定它，重構的`Configure`方法看起來像這樣：

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

這項重構之後, 您確信，web 應用程式仍能運作，因為所有要通過整合測試。

> [!NOTE]
> 您最好將變更認可到原始檔控制之後您完成重構和您的測試都成功。 如果您正在練習測試導向開發[認可加入您紅-綠-重構 」 循環，請考慮](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development)。

## <a name="resources"></a>資源

* [單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [中介軟體](xref:fundamentals/middleware/index)
* [測試控制器](xref:mvc/controllers/testing)
