* `-F|--no-formatting`

  <span data-ttu-id="ef8ce-101">其目前狀態會隱藏 HTTP 回應格式的旗標。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="ef8ce-102">設定 HTTP 要求標頭。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-102">Sets an HTTP request header.</span></span> <span data-ttu-id="ef8ce-103">支援下列兩種值格式：</span><span class="sxs-lookup"><span data-stu-id="ef8ce-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="ef8ce-104">指定要寫入整個 HTTP 回應 (包括標頭和本文) 的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="ef8ce-105">例如： `--response "C:\response.txt"` 。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="ef8ce-106">如果檔案不存在，就會建立檔案。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="ef8ce-107">指定 HTTP 回應本文應寫入的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="ef8ce-108">例如： `--response:body "C:\response.json"` 。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="ef8ce-109">如果檔案不存在，就會建立檔案。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="ef8ce-110">指定 HTTP 回應標頭應寫入的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="ef8ce-111">例如： `--response:headers "C:\response.txt"` 。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="ef8ce-112">如果檔案不存在，就會建立檔案。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="ef8ce-113">其目前狀態會啟用 HTTP 回應串流的旗標。</span><span class="sxs-lookup"><span data-stu-id="ef8ce-113">A flag whose presence enables streaming of the HTTP response.</span></span>
