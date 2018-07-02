<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="df329-101">Scaffold 電影模型</span><span class="sxs-lookup"><span data-stu-id="df329-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="df329-102">從命令列執行下列命令 (在包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的專案目錄中)：</span><span class="sxs-lookup"><span data-stu-id="df329-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="df329-103">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="df329-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="df329-104">當您處於錯誤的目錄時，就會發生先前的錯誤。</span><span class="sxs-lookup"><span data-stu-id="df329-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="df329-105">開啟專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 的命令殼層，然後執行先前的命令。</span><span class="sxs-lookup"><span data-stu-id="df329-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="df329-106">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="df329-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="df329-107">結束 Visual Studio 並再次執行命令。</span><span class="sxs-lookup"><span data-stu-id="df329-107">Exit Visual Studio and run the command again.</span></span>
