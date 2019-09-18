* <span data-ttu-id="019bb-101">藉由執行下列命令來信任 HTTPS 開發憑證：</span><span class="sxs-lookup"><span data-stu-id="019bb-101">Trust the HTTPS development certificate by running the following command:</span></span>

  ```dotnetcli
  dotnet dev-certs https --trust
  ```
  
  <span data-ttu-id="019bb-102">上述命令無法在 Linux 上使用。</span><span class="sxs-lookup"><span data-stu-id="019bb-102">The preceding command doesn't work on Linux.</span></span> <span data-ttu-id="019bb-103">如需信任憑證的資訊，請參閱您的 Linux 發行版本文件。</span><span class="sxs-lookup"><span data-stu-id="019bb-103">See your Linux distribution's documentation for trusting a certificate.</span></span>

  <span data-ttu-id="019bb-104">上述命令會顯示以下對話方塊：</span><span class="sxs-lookup"><span data-stu-id="019bb-104">The preceding command displays the following dialog:</span></span>

  ![安全性警告對話方塊](~/getting-started/_static/cert.png)

* <span data-ttu-id="019bb-106">若您同意信任開發憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="019bb-106">Select **Yes** if you agree to trust the development certificate.</span></span>

  <span data-ttu-id="019bb-107">如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。</span><span class="sxs-lookup"><span data-stu-id="019bb-107">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>
  
