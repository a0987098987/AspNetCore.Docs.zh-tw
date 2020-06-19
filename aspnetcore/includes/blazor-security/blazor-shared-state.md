Blazor 伺服器應用程式存留在伺服器記憶體中。 這表示在相同的進程內有多個應用程式託管。 針對每個應用程式會話，Blazor 會使用自己的 DI 容器範圍來啟動一個線路。 這表示範圍服務在每個 Blazor 會話中都是唯一的。

> [!WARNING]
> 我們不建議使用單一服務的相同伺服器共用狀態上的應用程式，因為這可能會造成安全性弱點，例如線上路之間洩漏使用者狀態。

您可以在 Blazor apps 中使用具狀態的單一服務（如果已特別為它設計）。 例如，您可以將記憶體快取當做 singleton 使用，因為它需要金鑰來存取指定的專案，前提是使用者無法控制所使用的快取索引鍵。

**此外，基於安全性理由，您不能 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 在 Blazor apps 中使用。** Blazor 應用程式會在 ASP.NET Core 管線的內容之外執行，而且 <xref:Microsoft.AspNetCore.Http.HttpContext> 不保證可在中使用 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> ，也不保證會保留啟動 Blazor 應用程式的內容。

將要求狀態傳遞至 Blazor 應用程式的建議方式是在應用程式的初始轉譯中，透過根元件的參數來進行：

* 定義一個類別，其中包含您要傳遞至 Blazor 應用程式的所有資料。
* 使用當時可用的來填入 Razor 頁面中的資料 <xref:Microsoft.AspNetCore.Http.HttpContext> 。
* 將資料傳遞至 Blazor 應用程式，做為根元件（應用程式）的參數。
* 在根元件中定義參數，以保存要傳遞至應用程式的資料。
* 在應用程式中使用使用者特定的資料;或者，將該資料複製到內的範圍服務， <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 使其可跨應用程式使用。

如需詳細資訊和範例程式碼，請參閱 <xref:blazor/security/server/additional-scenarios#pass-tokens-to-a-blazor-server-app>。
