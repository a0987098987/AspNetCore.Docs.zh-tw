# <a name="update-the-generated-pages"></a><span data-ttu-id="eedbd-101">更新產生的頁面</span><span class="sxs-lookup"><span data-stu-id="eedbd-101">Update the generated pages</span></span>

<span data-ttu-id="eedbd-102">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eedbd-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eedbd-103">剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。</span><span class="sxs-lookup"><span data-stu-id="eedbd-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="eedbd-104">我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是 **Release Date** (兩個分開的字)。</span><span class="sxs-lookup"><span data-stu-id="eedbd-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="eedbd-106">更新產生的程式碼</span><span class="sxs-lookup"><span data-stu-id="eedbd-106">Update the generated code</span></span>

<span data-ttu-id="eedbd-107">開啟 *Models/Movie.cs* 檔案，然後新增下列程式碼中顯示的醒目提示行：</span><span class="sxs-lookup"><span data-stu-id="eedbd-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](code/Models/Movie.cs?highlight=2,11-12)]
