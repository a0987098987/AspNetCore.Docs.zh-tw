# <a name="custom-model-binding-demo"></a>自訂模型繫結示範

您可以測試`ByteArrayModelBinder`以執行應用程式，並張貼 ImageController 端點的 base64 編碼字串 (/ api/影像 /)。 您應該在要求主體為表單資料中指定檔案和檔案名稱 proparties （使用郵差或類似工具）。 您可以使用[此範例字串](Base64String.txt)。 結果會儲存在您指定的檔名 [上傳 wwwroot/映像] 資料夾。

若要測試的自訂繫結的範例，請嘗試下列端點： /api/authors/1 /api/authors/2 （找不到） /api/boundauthors/1 /api/boundauthors/2 （找不到） /api/boundauthors/get/1 /api/boundauthors/get/2 （沒有內容）-這個動作並不會檢查為 null，傳回找不到
