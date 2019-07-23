
## <a name="add-validation-rules-to-the-movie-model"></a>將驗證規則新增至電影模型

開啟 *Movie.cs* 檔案。 DataAnnotations 命名空間提供一組內建的驗證屬性 (attribute)，其以宣告方式套用至類別或屬性 (property)。 DataAnnotations 也包含格式化屬性 (如 `DataType`)，可協助進行格式化，但不提供任何驗證。

更新 `Movie` 類別，以充分利用內建的 `Required`、`StringLength`、`RegularExpression` 和 `Range` 驗證屬性。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

驗證屬性會指定您想要對套用目標模型屬性強制執行的行為：

* `Required` 和 `MinimumLength` 屬性 (attribute) 指出屬性 (property) 必須是值；但無法防止使用者輸入空格以滿足此驗證。
* `RegularExpression` 屬性則用來限制可輸入的字元。 在上述程式碼中，"Genre"：

  * 必須指使用字母。
  * 第一個字母必須是大寫。 不允許使用空格、數字和特殊字元。

* `RegularExpression` "Rating"：

  * 第一個字元必須為大寫字母。
  * 允許後續空格中的特殊字元和數位。 "PG-13" 對分級而言有效，但不適用於 "Genre"。

* `Range` 屬性會將值限制在指定的範圍內。
* `StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。
* 實值型別 (如`decimal`、`int`、`float`、`DateTime`) 原本就是必要項目，而且不需要 `[Required]` 屬性。

擁有 ASP.NET Core 自動強制執行的驗證規則有助於讓您的應用程式更穩固。 它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。
