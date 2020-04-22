<!-- THIS INCLUDE USED BY MVC AND RP -->
<span data-ttu-id="4300e-101">將下列屬性新增至 `Movie` 類別：</span><span class="sxs-lookup"><span data-stu-id="4300e-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="4300e-102">`Movie` 類別包含：</span><span class="sxs-lookup"><span data-stu-id="4300e-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="4300e-103">`ID` 欄位是資料庫對於主索引鍵的必要欄位。</span><span class="sxs-lookup"><span data-stu-id="4300e-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="4300e-104">`[DataType(DataType.Date)]`[:DataType](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute)屬性指定數據類型(日期)。</span><span class="sxs-lookup"><span data-stu-id="4300e-104">`[DataType(DataType.Date)]`:  The [DataType](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="4300e-105">使用此屬性：</span><span class="sxs-lookup"><span data-stu-id="4300e-105">With this attribute:</span></span>

  * <span data-ttu-id="4300e-106">使用者不需要在日期欄位中輸入時間資訊。</span><span class="sxs-lookup"><span data-stu-id="4300e-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="4300e-107">只會顯示日期，不會顯示時間資訊。</span><span class="sxs-lookup"><span data-stu-id="4300e-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="4300e-108">稍後的教學課程會涵蓋 [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)。</span><span class="sxs-lookup"><span data-stu-id="4300e-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>
