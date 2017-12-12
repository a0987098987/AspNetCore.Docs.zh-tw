<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="1c14e-101">Scaffold 電影模型</span><span class="sxs-lookup"><span data-stu-id="1c14e-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="1c14e-102">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="1c14e-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1c14e-103">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1c14e-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="1c14e-104">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="1c14e-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="1c14e-105">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="1c14e-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>