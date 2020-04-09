```console
npm run release
```

此命令生成運行應用時要提供的用戶端資產。 這些資產位於 *wwwroot* 資料夾中。

Webpack 已完成下列工作：

* 清除 *wwwroot* 目錄的內容。
* 在稱為 *「傳播*」的進程中將 TypeScript 轉換為 JavaScript。
* 在稱為 *「小化*」的進程中,對生成的 JavaScript 進行調整以減小檔大小。
* 將處理的 JavaScript、CSS 與 HTML 檔案從 *src* 複製到 *wwwroot* 目錄。
* 將下列元素插入到 *wwwroot/index.html* 檔案：
  * `<link>` 標記，參考 *wwwroot/main.\<hash\>.css* 檔案。 此標記緊接在結尾 `</head>` 標記之前。
  * `<script>` 標記，參考縮減的 *wwwroot/main.\<hash\>.js* 檔案。 此標記緊接在結尾 `</body>` 標記之前。