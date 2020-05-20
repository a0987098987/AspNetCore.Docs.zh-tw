> [!NOTE]
> <span data-ttu-id="4f5f2-101">如果 Azure 入口網站提供應用程式的範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回未處理的例外狀況，請嘗試使用不包含配置和主機的範圍 uri。</span><span class="sxs-lookup"><span data-stu-id="4f5f2-101">If the Azure portal provides the scope URI for the app and the app throws an unhandled exception when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="4f5f2-102">例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：</span><span class="sxs-lookup"><span data-stu-id="4f5f2-102">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{TENANT}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="4f5f2-103">請嘗試提供不具配置和主機的範圍 URI：</span><span class="sxs-lookup"><span data-stu-id="4f5f2-103">Try supplying the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```
