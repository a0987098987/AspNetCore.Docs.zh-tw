# <a name="custom-model-binding-demo"></a>自訂模型繫結示範

執行應用程式，並將 base64 編碼的字串張貼到`ImageController`端點 (`/api/image/`)，以測試 `ByteArrayModelBinder`。 請在要求主體中指定檔案與檔案名稱屬性作為表單資料 (使用 [Postman](https://www.getpostman.com/) 或類似的工具)。 您可以使用[此範例字串](Base64String.txt)。 結果會以您指定的檔案名稱儲存在 *wwwroot/images/upload* 資料夾中。

若要測試自訂繫結的範例，請嘗試下列端點：

* /api/authors/1
* /api/authors/2 (找不到)
* /api/boundauthors/1
* /api/boundauthors/2 (找不到)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (NO CONTENT) &ndash; 此動作不會檢查 null，並會傳回 *404 找不到*。
