# <a name="gdpr-sample"></a>GDPR 範例

* 在*appsettings*中，將 `CheckNotConsentNeeded` 設定為 `false` 以要求同意;否則，請將設定為 true 或省略。 使用設定為 `false` 的 `CheckNotConsentNeeded` 來測試應用程式，並將設定為 [`true`]。
* 建立基本和非必要的 cookie，並將每一種 `CheckConsentNeeded` 和同意的變化都授與。
* 註冊使用者。
* 刪除 cookie。
