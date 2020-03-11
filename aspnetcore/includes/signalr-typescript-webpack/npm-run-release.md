```console
npm run release
```

<span data-ttu-id="0574a-101">此命令會產生要在執行應用程式時提供服務的用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="0574a-101">This command generates the client-side assets to be served when running the app.</span></span> <span data-ttu-id="0574a-102">這些資產位於 *wwwroot* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="0574a-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="0574a-103">Webpack 已完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="0574a-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="0574a-104">清除 *wwwroot* 目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="0574a-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="0574a-105">在稱為*轉譯*的進程中，將 TypeScript 轉換為 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="0574a-105">Converted the TypeScript to JavaScript in a process known as *transpilation*.</span></span>
* <span data-ttu-id="0574a-106">已損壞產生的 JavaScript，以在稱為*縮制*的程式中縮減檔案大小。</span><span class="sxs-lookup"><span data-stu-id="0574a-106">Mangled the generated JavaScript to reduce file size in a process known as *minification*.</span></span>
* <span data-ttu-id="0574a-107">將處理的 JavaScript、CSS 與 HTML 檔案從 *src* 複製到 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="0574a-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="0574a-108">將下列元素插入到 *wwwroot/index.html* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0574a-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
  * <span data-ttu-id="0574a-109">`<link>` 標記，參考 *wwwroot/main.\<hash\>.css* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0574a-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="0574a-110">此標記緊接在結尾 `</head>` 標記之前。</span><span class="sxs-lookup"><span data-stu-id="0574a-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
  * <span data-ttu-id="0574a-111">`<script>` 標記，參考縮減的 *wwwroot/main.\<hash\>.js* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0574a-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="0574a-112">此標記緊接在結尾 `</body>` 標記之前。</span><span class="sxs-lookup"><span data-stu-id="0574a-112">This tag is placed immediately before the closing `</body>` tag.</span></span>