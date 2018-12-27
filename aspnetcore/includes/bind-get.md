> [!WARNING]
> 基於安全性考量，您必須選擇將 `GET` 要求資料繫結到頁面模型屬性。 請先驗證使用者輸入再將其對應至屬性。 在解決仰賴查詢字串或路由值的案例時，選擇使用 `GET` 繫結會很有幫助。
>
> 若要在 `GET` 要求上繫結屬性，請將 [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute)屬性的 `SupportsGet` 屬性設定為 `true`：`[BindProperty(SupportsGet = true)]`
