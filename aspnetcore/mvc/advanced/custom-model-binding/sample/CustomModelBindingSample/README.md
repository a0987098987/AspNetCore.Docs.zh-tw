# <a name="custom-model-binding-demo"></a>自訂模型繫結示範

您可以執行應用程式，並將 base64 編碼字串張貼 (POST) 到 ImageController 端點 (/api/image/)，以測試 `ByteArrayModelBinder`。 您應該在要求主體中指定檔案和檔案名稱屬性作為表單資料 (使用 Postman 或類似工具）。 您可以使用[此範例字串](Base64String.txt)。 結果將以您指定的檔案名稱儲存在 wwwroot/images/upload 資料夾中。

若要測試自訂繫結範例，請嘗試下列端點： /api/authors/1 /api/authors/2 (NOT FO「」) /api/boundauthors/1 /api/boundauthors/2 (NOT FOUND) /api/boundauthors/get/1 /api/boundauthors/get/2 (NO CONTENT) - 這個動作不會檢查是否有 null，並傳回「找不到」
