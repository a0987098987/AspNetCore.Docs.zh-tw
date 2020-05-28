# <a name="key-vault-configuration-provider-sample-app"></a>Key Vault 設定提供者範例應用程式

這個範例說明如何使用 Azure Key Vault 設定提供者。

此範例會以 Program.cs 檔案頂端的語句所決定的兩種模式之一來執行 `#define` 。 *Program.cs* 如需指示，請參閱[範例程式碼中的預處理器](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code)指示詞：

* `Certificate`：示範如何使用 Azure Key Vault 的用戶端識別碼和 x.509 憑證，來存取儲存在 Azure Key Vault 中的秘密。 這個版本的範例可以從任何位置執行，部署至 Azure App Service 或任何能夠提供 ASP.NET Core 應用程式的主機。
* `Managed`：示範如何使用 Azure 的[受控服務識別](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)來驗證應用程式，以在應用程式的程式碼或設定中沒有認證的情況下，透過 Azure AD 驗證來 Azure Key Vault。 應用程式不需要 Azure AD 的用戶端識別碼和密碼，就能使用 Azure Key Vault 進行驗證。 此範例必須部署到 Azure App Service，才能探索受控識別 scearnio。

如需詳細資訊，請參閱[Azure Key Vault 設定提供者](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration)。
