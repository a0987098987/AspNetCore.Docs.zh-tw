讀取相關設定值的慣用方法是使用[選項模式](xref:fundamentals/configuration/options)。 例如，若要讀取下列設定值：

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

建立下列 `PositionOptions` 類別：

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

選項類別：

* 必須是非抽象，且具有公用無參數的函式。
* 類型的所有公用讀寫屬性都會系結。
* 欄位***未***系結。 在上述程式碼中， `Position` 不會系結。 `Position`使用屬性，因此將類別系結 `"Position"` 至設定提供者時，不需要在應用程式中硬式編碼字串。

下列程式碼：

* 呼叫[ConfigurationBinder](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*)將類別系結 `PositionOptions` 至 `Position` 區段。
* 顯示設定 `Position` 資料。

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

在上述程式碼中，根據預設，會讀取應用程式啟動後對 JSON 設定檔進行的變更。

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*)系結並傳回指定的型別。 `ConfigurationBinder.Get<T>`可能比使用更方便 `ConfigurationBinder.Bind` 。 下列程式碼示範如何搭配使用 `ConfigurationBinder.Get<T>` 與 `PositionOptions` 類別：

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

在上述程式碼中，根據預設，會讀取應用程式啟動後對 JSON 設定檔進行的變更。

使用 [***選項] 模式***時，另一種方法是系結 `Position` 區段，並將它加入至相依性[插入服務容器](xref:fundamentals/dependency-injection)。 在下列程式碼中， `PositionOptions` 會新增至服務容器， <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> 並系結至設定：

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

使用上述程式碼，下列程式碼會讀取位置選項：

[!code-csharp[](~/fundamentals/configuration/index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

在上述程式碼中，***不***會讀取在應用程式啟動後對 JSON 設定檔進行的變更。 若要在應用程式啟動後讀取變更，請使用[IOptionsSnapshot](xref:fundamentals/configuration/options#ios)。
