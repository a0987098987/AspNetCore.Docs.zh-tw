> [!WARNING]
> <span data-ttu-id="cccce-101">基於安全性考量，您必須選擇將 `GET` 要求資料繫結到頁面模型屬性。</span><span class="sxs-lookup"><span data-stu-id="cccce-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="cccce-102">請先驗證使用者輸入再將其對應至屬性。</span><span class="sxs-lookup"><span data-stu-id="cccce-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="cccce-103">在處理依賴於`GET`查詢字串或路由值的方案時,選擇綁定非常有用。</span><span class="sxs-lookup"><span data-stu-id="cccce-103">Opting into `GET` binding is useful when addressing scenarios that rely on query string or route values.</span></span>
>
> <span data-ttu-id="cccce-104">要對要求`GET`結合屬性,請將[`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute)屬性的`SupportsGet`屬性`true`設定為 :</span><span class="sxs-lookup"><span data-stu-id="cccce-104">To bind a property on `GET` requests, set the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute's `SupportsGet` property to `true`:</span></span>
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> <span data-ttu-id="cccce-105">有關詳細資訊,請參閱[ASP.NET核心社區站立:綁定 GET 討論 (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s)。</span><span class="sxs-lookup"><span data-stu-id="cccce-105">For more information, see [ASP.NET Core Community Standup: Bind on GET discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span></span>
