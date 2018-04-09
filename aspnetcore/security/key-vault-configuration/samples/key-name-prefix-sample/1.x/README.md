# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>金鑰保存庫的組態提供者範例應用程式的前置詞 (ASP.NET Core 1.x)

此範例說明使用 Azure 金鑰保存庫的組態提供者適用於 ASP.NET Core 1.x 使用索引鍵名稱前置詞。 如需 ASP.NET Core 2.x 範例中，請參閱[前置詞的金鑰保存庫的組態提供者範例應用程式 (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)。

> [!NOTE]
> 組態提供者無法使用 ASP.NET Core 1.0。 如果您想要實作的組態提供者，而且應用程式的 ASP.NET Core 1.0 應用程式，請將應用程式升級至 1.1 或更新版本的第一個。

如需有關範例的運作方式的詳細資訊，請參閱[Azure 金鑰保存庫的組態提供者](xref:security/key-vault-configuration)主題。

## <a name="using-the-sample"></a>使用範例
1. 建立金鑰保存庫，並設定下列中的指導方針的應用程式的 Azure Active Directory (Azure AD)[開始使用 Azure 金鑰保存庫](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
   * 將密碼加入金鑰保存庫使用 Azure PowerShell 模組、 Azure 管理 API 或在 Azure 入口網站。 密碼會建立為*手動*或*憑證*機密資料。 *憑證*密碼使用的應用程式和服務憑證，但不是支援的組態提供者。 您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組密碼。
     * 使用階層式的值 （組態區段） `--` （兩個破折號） 做為分隔符號。
     * 範例應用程式中，建立兩個*手動*具有下列名稱 / 值組的密碼：
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * 向 Azure Active Directory 中註冊範例應用程式。
   * 授權應用程式存取金鑰保存庫。 當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，以授權應用程式存取金鑰保存庫中，提供`List`和`Get`密碼與存取`-PermissionsToSecrets list,get`。

2. 更新應用程式的*appsettings.json*檔案使用的數值`Vault`， `ClientId`，和`ClientSecret`。
3. 執行範例應用程式，會取得從其組態值`IConfigurationRoot`具有相同名稱做為帶有前置詞的秘密名稱。 在此範例中，前置詞是應用程式的版本，您提供給`PrefixKeyVaultSecretManager`當您加入的 Azure 金鑰保存庫的組態提供者。 值`AppSecret`取得`config["AppSecret"]`。
4. 變更應用程式中的組件的專案檔版本`5.0.0.0`至`5.1.0.0`，然後再次執行應用程式。 此時，傳回的密碼值是`5.1.0.0_secret_value`。
