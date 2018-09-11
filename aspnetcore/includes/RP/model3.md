<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>新增 Scaffold 工具並執行初始移轉

將以下各行加到 *RazorPagesMovie.csproj* 檔案中的結尾 `</Project>` 標記之前：

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```  
從命令列中，執行下列 .NET Core CLI 命令：

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`DotNetCliToolReference` 元素與 `add package` 命令會安裝執行 Scaffolding 引擎所需的工具。

`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。 結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。 `InitialCreate` 引數用來命名移轉。 您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。 如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。

`ef database update` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。
