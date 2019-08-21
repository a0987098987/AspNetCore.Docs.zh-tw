* 藉由執行下列命令來信任 HTTPS 開發憑證：

  ```console
  dotnet dev-certs https --trust
  ```
  
  上述命令無法在 Linux 上使用。 如需信任憑證的資訊，請參閱您的 Linux 發行版本文件。

  上述命令會顯示以下對話方塊：

  ![安全性警告對話方塊](~/getting-started/_static/cert.png)

* 若您同意信任開發憑證，請選取 [是]  。

  如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。
  
