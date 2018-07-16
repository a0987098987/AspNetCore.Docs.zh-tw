```console
npm run release
```

<span data-ttu-id="fde19-101">此命令會產生執行應用程式時要提供的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="fde19-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="fde19-102">這些資產位於 *wwwroot* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="fde19-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="fde19-103">Webpack 已完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="fde19-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="fde19-104">清除 *wwwroot* 目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="fde19-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="fde19-105">將 TypeScript 轉換為 JavaScript&mdash;稱為「轉譯」的程序。</span><span class="sxs-lookup"><span data-stu-id="fde19-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="fde19-106">損壞產生的 JavaScript 以減少檔案大小&mdash;稱為「縮製」的程序。</span><span class="sxs-lookup"><span data-stu-id="fde19-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="fde19-107">將處理的 JavaScript、CSS 與 HTML 檔案從 *src* 複製到 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="fde19-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="fde19-108">將下列元素插入到 *wwwroot/index.html* 檔案：</span><span class="sxs-lookup"><span data-stu-id="fde19-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
    * <span data-ttu-id="fde19-109">`<link>` 標記，參考 *wwwroot/main.\<hash\>.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fde19-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="fde19-110">此標記緊接在結尾 `</head>` 標記之前。</span><span class="sxs-lookup"><span data-stu-id="fde19-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
    * <span data-ttu-id="fde19-111">`<script>` 標記，參考縮減的 *wwwroot/main.\<hash\>.js* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fde19-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="fde19-112">此標記緊接在結尾 `</body>` 標記之前。</span><span class="sxs-lookup"><span data-stu-id="fde19-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
