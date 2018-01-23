<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="112f7-101">Scaffold 電影模型</span><span class="sxs-lookup"><span data-stu-id="112f7-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="112f7-102">從命令列執行下列命令 (在包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的專案目錄中)：</span><span class="sxs-lookup"><span data-stu-id="112f7-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="112f7-103">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="112f7-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="112f7-104">開啟專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 的命令殼層。</span><span class="sxs-lookup"><span data-stu-id="112f7-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
