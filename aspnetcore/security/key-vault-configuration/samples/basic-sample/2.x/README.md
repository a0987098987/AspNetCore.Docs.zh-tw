# <a name="key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>金鑰保存庫的組態提供者範例應用程式 (ASP.NET Core 2.x)

此範例說明使用 Azure 金鑰保存庫的組態提供者適用於 ASP.NET Core 2.x。 如需 ASP.NET Core 1.x 範例中，請參閱[金鑰保存庫的組態提供者範例應用程式 (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)。

如需有關範例的運作方式的詳細資訊，請參閱[Azure 金鑰保存庫的組態提供者](xref:security/key-vault-configuration)主題。

## <a name="using-the-sample"></a>使用範例
1. 建立金鑰保存庫，並設定下列中的指導方針的應用程式的 Azure Active Directory (Azure AD)[開始使用 Azure 金鑰保存庫](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
   * 將密碼加入金鑰保存庫使用[AzureRM 金鑰保存庫 PowerShell 模組](/powershell/module/azurerm.keyvault)可從[PowerShell 資源庫](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure 金鑰保存庫 REST API](/rest/api/keyvault/)，或[Azure 入口網站](https://portal.azure.com/)。 密碼會建立為*手動*或*憑證*機密資料。 *憑證*密碼使用的應用程式和服務憑證，但不是支援的組態提供者。 您應該使用*手動*選項建立的組態提供者使用的名稱 / 值組密碼。
     * 簡單密碼，會建立為名稱 / 值組。 Azure 金鑰保存庫密碼名稱會限制為英數字元及虛線。
     * 使用階層式的值 （組態區段） `--` （兩個破折號） 做為範例中的分隔符號。 通常用來分隔區段中的，從子機碼中的冒號[ASP.NET Core 組態](xref:fundamentals/configuration/index)，秘密名稱中不允許。 因此，使用兩個破折號和機密資料載入至應用程式的組態時，已還原為冒號。
     * 建立兩個*手動*具有下列名稱 / 值組的密碼。 第一個密碼是簡單名稱和值，並以第二個密碼建立祕密值區段和秘密名稱中的子機碼：
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * 向 Azure Active Directory 中註冊範例應用程式。
   * 授權應用程式存取金鑰保存庫。 當您使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet，以授權應用程式存取金鑰保存庫中，提供`List`和`Get`密碼與存取`-PermissionsToSecrets list,get`。

2. 更新應用程式的*appsettings.json*檔案使用的數值`Vault`， `ClientId`，和`ClientSecret`。
3. 執行範例應用程式，會取得從其組態值`IConfigurationRoot`具有相同名稱與密碼的名稱。
   * 非階層式的值： 的值`SecretName`取得`config["SecretName"]`。
   * 階層值 （區段）： 使用`:`（冒號） 標記法或`GetSection`擴充方法。 您可以使用任一種方法來取得組態值：
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`
