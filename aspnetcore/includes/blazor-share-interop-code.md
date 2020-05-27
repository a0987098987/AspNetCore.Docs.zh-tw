## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="dcc3c-101">共用類別庫中的 interop 程式碼</span><span class="sxs-lookup"><span data-stu-id="dcc3c-101">Share interop code in a class library</span></span>

<span data-ttu-id="dcc3c-102">JS interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-102">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="dcc3c-103">類別庫會處理在建立的元件中內嵌 JavaScript 資源。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-103">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="dcc3c-104">JavaScript 檔案會放在*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-104">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="dcc3c-105">工具會在建立程式庫時，負責內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-105">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="dcc3c-106">在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-106">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="dcc3c-107">還原套件之後，應用程式程式碼可以呼叫 JavaScript，就好像它是 c # 一樣。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-107">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="dcc3c-108">如需詳細資訊，請參閱<xref:blazor/class-libraries>。</span><span class="sxs-lookup"><span data-stu-id="dcc3c-108">For more information, see <xref:blazor/class-libraries>.</span></span>
