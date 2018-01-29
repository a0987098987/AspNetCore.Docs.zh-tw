<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Scaffold 電影模型

* 從命令列執行下列命令 (在包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的專案目錄中)：

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

如果您收到錯誤：
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

開啟專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 的命令殼層。

如果您收到錯誤：
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

結束 Visual Studio 並再次執行命令。
