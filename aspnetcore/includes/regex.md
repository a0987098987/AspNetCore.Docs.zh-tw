> [!WARNING]
> <span data-ttu-id="6a863-101">使用來處理<xref:System.Text.RegularExpressions>不受信任的輸入時,傳遞超時。</span><span class="sxs-lookup"><span data-stu-id="6a863-101">When using <xref:System.Text.RegularExpressions> to process untrusted input, pass a timeout.</span></span> <span data-ttu-id="6a863-102">惡意使用者可以提供導致`RegularExpressions`[拒絕服務攻擊](https://www.us-cert.gov/ncas/tips/ST04-015)的輸入。</span><span class="sxs-lookup"><span data-stu-id="6a863-102">A malicious user can provide input to `RegularExpressions` causing a [Denial-of-Service attack](https://www.us-cert.gov/ncas/tips/ST04-015).</span></span> <span data-ttu-id="6a863-103">ASP.NET使用`RegularExpressions`傳遞超時的核心框架 API。</span><span class="sxs-lookup"><span data-stu-id="6a863-103">ASP.NET Core framework APIs that use `RegularExpressions` pass a timeout.</span></span>