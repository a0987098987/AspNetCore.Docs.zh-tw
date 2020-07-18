<a name="ddav"></a>
### <a name="disable-default-account-verification"></a>停用預設帳戶驗證

使用預設範本時，系統會將使用者重新導向至， `Account.RegisterConfirmation` 他們可以在其中選取連結以確認帳戶。 預設 `Account.RegisterConfirmation` ***僅***用於測試，應在生產應用程式中停用自動帳戶驗證。

若要在註冊時要求已確認的帳戶並防止立即登入，請 `DisplayConfirmAccountLink = false` 在 */Areas/Identity/Pages/Account/RegisterConfirmation.cshtml.cs*中設定：

[!code-csharp[](~/security/authentication/identity/sample/WebApp3/Areas/Identity/Pages/Account/RegisterConfirmation.cshtml.cs?name=snippet&highlight=34)]