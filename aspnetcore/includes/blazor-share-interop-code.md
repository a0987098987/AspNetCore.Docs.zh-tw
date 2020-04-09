## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="4b82a-101">在類別庫中共用互通程式碼</span><span class="sxs-lookup"><span data-stu-id="4b82a-101">Share interop code in a class library</span></span>

<span data-ttu-id="4b82a-102">JS 互通代碼可以包含在類庫中,這允許您在 NuGet 包中共用代碼。</span><span class="sxs-lookup"><span data-stu-id="4b82a-102">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="4b82a-103">類庫處理在生成的程式集中嵌入 JavaScript 資源。</span><span class="sxs-lookup"><span data-stu-id="4b82a-103">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="4b82a-104">JavaScript 檔案放置在*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="4b82a-104">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="4b82a-105">工具負責在構建庫時嵌入資源。</span><span class="sxs-lookup"><span data-stu-id="4b82a-105">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="4b82a-106">構建的 NuGet 套件在應用的專案檔中引用的方式與引用任何 NuGet 套件的方式相同。</span><span class="sxs-lookup"><span data-stu-id="4b82a-106">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="4b82a-107">還原包後,應用代碼可以調用到 JAVAScript 中,就像它是 C#一樣。</span><span class="sxs-lookup"><span data-stu-id="4b82a-107">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="4b82a-108">如需詳細資訊，請參閱 <xref:blazor/class-libraries>。</span><span class="sxs-lookup"><span data-stu-id="4b82a-108">For more information, see <xref:blazor/class-libraries>.</span></span>
