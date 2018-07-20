---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: 以程式設計方式設定 ObjectDataSource 的參數值 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將探討將方法加入我們的 DAL 和 BLL，會接受單一輸入的參數並傳回資料。 此範例會設定此參數...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 561b197aae925eb432a3e93d37b347a081f64a2b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827346"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a><span data-ttu-id="655e6-104">以程式設計方式設定 ObjectDataSource 的參數值 (C#)</span><span class="sxs-lookup"><span data-stu-id="655e6-104">Programmatically Setting the ObjectDataSource's Parameter Values (C#)</span></span>
====================
<span data-ttu-id="655e6-105">藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="655e6-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="655e6-106">[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe)或[下載 PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="655e6-106">[Download Sample App](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) or [Download PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)</span></span>

> <span data-ttu-id="655e6-107">在本教學課程中，我們將探討將方法加入我們的 DAL 和 BLL，會接受單一輸入的參數並傳回資料。</span><span class="sxs-lookup"><span data-stu-id="655e6-107">In this tutorial we'll look at adding a method to our DAL and BLL that accepts a single input parameter and returns data.</span></span> <span data-ttu-id="655e6-108">此範例會以程式設計方式設定此參數。</span><span class="sxs-lookup"><span data-stu-id="655e6-108">The example will set this parameter programmatically.</span></span>


## <a name="introduction"></a><span data-ttu-id="655e6-109">簡介</span><span class="sxs-lookup"><span data-stu-id="655e6-109">Introduction</span></span>

<span data-ttu-id="655e6-110">如我們在中所見[先前的教學課程](declarative-parameters-cs.md)，有多種選項可供以宣告方式將參數值傳遞至 ObjectDataSource 的方法。</span><span class="sxs-lookup"><span data-stu-id="655e6-110">As we saw in the [previous tutorial](declarative-parameters-cs.md), a number of options are available for declaratively passing parameter values to the ObjectDataSource's methods.</span></span> <span data-ttu-id="655e6-111">如果參數值為硬式編碼，來自 Web 控制項的頁面上，或在任何其他資料來源是可讀取的來源`Parameter`物件，例如，值可以結合到輸入參數，而不需要撰寫一行程式碼。</span><span class="sxs-lookup"><span data-stu-id="655e6-111">If the parameter value is hard-coded, comes from a Web control on the page, or is in any other source that is readable by a data source `Parameter` object, for example, that value can be bound to the input parameter without writing a line of code.</span></span>

<span data-ttu-id="655e6-112">可能有的時間，不過，當參數值來自尚未納入考量的其中一個內建的資料來源所一些來源`Parameter`物件。</span><span class="sxs-lookup"><span data-stu-id="655e6-112">There may be times, however, when the parameter value comes from some source not already accounted for by one of the built-in data source `Parameter` objects.</span></span> <span data-ttu-id="655e6-113">如果我們的網站支援的使用者帳戶我們可能會想要設定參數，根據目前登入訪客的使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="655e6-113">If our site supported user accounts we might want to set the parameter based on the currently logged in visitor's User ID.</span></span> <span data-ttu-id="655e6-114">或者，我們可能需要自訂參數值之前將它傳送至 ObjectDataSource 的基礎物件的方法。</span><span class="sxs-lookup"><span data-stu-id="655e6-114">Or we may need to customize the parameter value before sending it along to the ObjectDataSource's underlying object's method.</span></span>

<span data-ttu-id="655e6-115">每當 ObjectDataSource`Select`方法會叫用 ObjectDataSource 第一次引發其[選取事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)。</span><span class="sxs-lookup"><span data-stu-id="655e6-115">Whenever the ObjectDataSource's `Select` method is invoked the ObjectDataSource first raises its [Selecting event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx).</span></span> <span data-ttu-id="655e6-116">ObjectDataSource 的基礎物件的方法則會叫用。</span><span class="sxs-lookup"><span data-stu-id="655e6-116">The ObjectDataSource's underlying object's method is then invoked.</span></span> <span data-ttu-id="655e6-117">一旦您已完成的 ObjectDataSource[選取事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)引發 （[圖 1] 說明這一系列的事件）。</span><span class="sxs-lookup"><span data-stu-id="655e6-117">Once that completes the ObjectDataSource's [Selected event](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) fires (Figure 1 illustrates this sequence of events).</span></span> <span data-ttu-id="655e6-118">傳入 ObjectDataSource 的基礎物件的方法的參數值可以設定或自訂事件處理常式中`Selecting`事件。</span><span class="sxs-lookup"><span data-stu-id="655e6-118">The parameter values passed into the ObjectDataSource's underlying object's method can be set or customized in an event handler for the `Selecting` event.</span></span>


<span data-ttu-id="655e6-119">[![ObjectDataSource 的選取和選取的事件引發之前和之後其基礎物件的方法會叫用](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-119">[![The ObjectDataSource's Selected and Selecting Events Fire Before and After Its Underlying Object's Method is Invoked](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)</span></span>

<span data-ttu-id="655e6-120">**圖 1**: ObjectDataSource`Selected`並`Selecting`事件引發之前和之後其基礎物件的方法叫用 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-120">**Figure 1**: The ObjectDataSource's `Selected` and `Selecting` Events Fire Before and After Its Underlying Object's Method is Invoked ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))</span></span>


<span data-ttu-id="655e6-121">在本教學課程將探討將方法加入我們的 DAL 和接受單一輸入的參數的 BLL `Month`，型別的`int`，並傳回`EmployeesDataTable`填入員工具有其招聘年度中指定的物件`Month`.</span><span class="sxs-lookup"><span data-stu-id="655e6-121">In this tutorial we'll look at adding a method to our DAL and BLL that accepts a single input parameter `Month`, of type `int` and returns an `EmployeesDataTable` object populated with those employees that have their hiring anniversary in the specified `Month`.</span></span> <span data-ttu-id="655e6-122">我們的範例會設定此參數，以程式設計方式根據目前的月份中，顯示一份 「 員工主管本月。 」</span><span class="sxs-lookup"><span data-stu-id="655e6-122">Our example will set this parameter programmatically based on the current month, showing a list of "Employee Anniversaries This Month."</span></span>

<span data-ttu-id="655e6-123">讓我們開始吧 ！</span><span class="sxs-lookup"><span data-stu-id="655e6-123">Let's get started!</span></span>

## <a name="step-1-adding-a-method-toemployeestableadapter"></a><span data-ttu-id="655e6-124">步驟 1： 將方法加入`EmployeesTableAdapter`</span><span class="sxs-lookup"><span data-stu-id="655e6-124">Step 1: Adding a Method to`EmployeesTableAdapter`</span></span>

<span data-ttu-id="655e6-125">第一個範例中我們需要加入一個方法來擷取這些員工的`HireDate`發生在指定的月份。</span><span class="sxs-lookup"><span data-stu-id="655e6-125">For our first example we need to add a means to retrieve those employees whose `HireDate` occurred in a specified month.</span></span> <span data-ttu-id="655e6-126">若要提供此功能，根據我們必須先建立方法，以在我們架構`EmployeesTableAdapter`子可對應到適當的 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="655e6-126">To provide this functionality in accordance with our architecture we need to first create a method in `EmployeesTableAdapter` that maps to the proper SQL statement.</span></span> <span data-ttu-id="655e6-127">若要這麼做，首先開啟 Northwind 具類型資料集。</span><span class="sxs-lookup"><span data-stu-id="655e6-127">To accomplish this, start by opening the Northwind Typed DataSet.</span></span> <span data-ttu-id="655e6-128">以滑鼠右鍵按一下`EmployeesTableAdapter`加上標籤，然後選擇 加入查詢。</span><span class="sxs-lookup"><span data-stu-id="655e6-128">Right-click on the `EmployeesTableAdapter` label and choose Add Query.</span></span>


<span data-ttu-id="655e6-129">[![將新的查詢加入至 EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-129">[![Add a New Query to the EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)</span></span>

<span data-ttu-id="655e6-130">**圖 2**： 加入新的查詢，以`EmployeesTableAdapter`([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-130">**Figure 2**: Add a New Query to the `EmployeesTableAdapter` ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))</span></span>


<span data-ttu-id="655e6-131">選擇此選項，將會傳回資料列的 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="655e6-131">Choose to add a SQL statement that returns rows.</span></span> <span data-ttu-id="655e6-132">當您來到 指定`SELECT`陳述式畫面的預設值`SELECT`陳述式`EmployeesTableAdapter`會載入。</span><span class="sxs-lookup"><span data-stu-id="655e6-132">When you reach the Specify a `SELECT` Statement screen the default `SELECT` statement for the `EmployeesTableAdapter` will already be loaded.</span></span> <span data-ttu-id="655e6-133">只需在中新增`WHERE`子句： `WHERE DATEPART(m, HireDate) = @Month`。</span><span class="sxs-lookup"><span data-stu-id="655e6-133">Simply add in the `WHERE` clause: `WHERE DATEPART(m, HireDate) = @Month`.</span></span> <span data-ttu-id="655e6-134">[DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)會傳回特定的日期部分的 T-SQL 函式`datetime`類型; 在此案例中，我們會使用`DATEPART`傳回當月`HireDate`資料行。</span><span class="sxs-lookup"><span data-stu-id="655e6-134">[DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) is a T-SQL function that returns a particular date portion of a `datetime` type; in this case we're using `DATEPART` to return the month of the `HireDate` column.</span></span>


<span data-ttu-id="655e6-135">[![傳回只有那些資料列位置的 HireDate Sloupec je 小於或等於@HiredBeforeDate參數](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-135">[![Return Only Those Rows Where the HireDate Column is Less Than or Equal to the @HiredBeforeDate Parameter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)</span></span>

<span data-ttu-id="655e6-136">**圖 3**： 傳回只有那些資料列所在`HireDate`資料行小於或等於`@HiredBeforeDate`參數 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-136">**Figure 3**: Return Only Those Rows Where the `HireDate` Column is Less Than or Equal to the `@HiredBeforeDate` Parameter ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))</span></span>


<span data-ttu-id="655e6-137">最後，變更`FillBy`並`GetDataBy`方法名稱來`FillByHiredDateMonth`和`GetEmployeesByHiredDateMonth`分別。</span><span class="sxs-lookup"><span data-stu-id="655e6-137">Finally, change the `FillBy` and `GetDataBy` method names to `FillByHiredDateMonth` and `GetEmployeesByHiredDateMonth`, respectively.</span></span>


<span data-ttu-id="655e6-138">[![選擇比 FillBy 和 GetDataBy 更適當的方法名稱](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-138">[![Choose More Appropriate Method Names Than FillBy and GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)</span></span>

<span data-ttu-id="655e6-139">**圖 4**： 選擇 更多適當方法名稱比`FillBy`並`GetDataBy`([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-139">**Figure 4**: Choose More Appropriate Method Names Than `FillBy` and `GetDataBy` ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))</span></span>


<span data-ttu-id="655e6-140">按一下 完成 以完成精靈並返回 資料集的設計介面。</span><span class="sxs-lookup"><span data-stu-id="655e6-140">Click Finish to complete the wizard and return to the DataSet's design surface.</span></span> <span data-ttu-id="655e6-141">`EmployeesTableAdapter`現在應該包含一組新的方法，以存取指定的月份中雇用的員工。</span><span class="sxs-lookup"><span data-stu-id="655e6-141">The `EmployeesTableAdapter` should now include a new set of methods for accessing employees hired in a specified month.</span></span>


<span data-ttu-id="655e6-142">[![新的方法會出現在資料集的設計介面](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-142">[![The New Methods Appear in the DataSet's Design Surface](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)</span></span>

<span data-ttu-id="655e6-143">**圖 5**: 新方法會出現在資料集的設計介面中 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-143">**Figure 5**: The New Methods Appear in the DataSet's Design Surface ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))</span></span>


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a><span data-ttu-id="655e6-144">步驟 2： 加入`GetEmployeesByHiredDateMonth(month)`方法商業邏輯層</span><span class="sxs-lookup"><span data-stu-id="655e6-144">Step 2: Adding the`GetEmployeesByHiredDateMonth(month)`Method to the Business Logic Layer</span></span>

<span data-ttu-id="655e6-145">由於我們應用程式架構會使用不同的圖層的商務邏輯和資料存取邏輯，我們必須將方法加入到 DAL 呼叫擷取員工雇用指定日期之前我們 BLL。</span><span class="sxs-lookup"><span data-stu-id="655e6-145">Since our application architecture uses a separate layer for the business logic and data access logic, we need to add a method to our BLL that calls down to the DAL to retrieve employees hired before a specified date.</span></span> <span data-ttu-id="655e6-146">開啟`EmployeesBLL.cs`檔案，並新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="655e6-146">Open the `EmployeesBLL.cs` file and add the following method:</span></span>


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

<span data-ttu-id="655e6-147">如同我們在這個類別中的其他方法`GetEmployeesByHiredDateMonth(month)`DAL 直接呼叫，並傳回結果。</span><span class="sxs-lookup"><span data-stu-id="655e6-147">As with our other methods in this class, `GetEmployeesByHiredDateMonth(month)` simply calls down into the DAL and returns the results.</span></span>

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a><span data-ttu-id="655e6-148">步驟 3： 顯示的員工的招聘年度是本月</span><span class="sxs-lookup"><span data-stu-id="655e6-148">Step 3: Displaying Employees Whose Hiring Anniversary Is This Month</span></span>

<span data-ttu-id="655e6-149">此範例中最後一個步驟是顯示的員工的招聘年度是本月。</span><span class="sxs-lookup"><span data-stu-id="655e6-149">Our final step for this example is to display those employees whose hiring anniversary is this month.</span></span> <span data-ttu-id="655e6-150">新增 GridView，以開始`ProgrammaticParams.aspx`頁面中`BasicReporting`資料夾並加入新的 ObjectDataSource 做為其資料來源。</span><span class="sxs-lookup"><span data-stu-id="655e6-150">Start by adding a GridView to the `ProgrammaticParams.aspx` page in the `BasicReporting` folder and add a new ObjectDataSource as its data source.</span></span> <span data-ttu-id="655e6-151">設定要使用 ObjectDataSource`EmployeesBLL`類別搭配`SelectMethod`設定為`GetEmployeesByHiredDateMonth(month)`。</span><span class="sxs-lookup"><span data-stu-id="655e6-151">Configure the ObjectDataSource to use the `EmployeesBLL` class with the `SelectMethod` set to `GetEmployeesByHiredDateMonth(month)`.</span></span>


<span data-ttu-id="655e6-152">[![使用 EmployeesBLL 類別](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-152">[![Use the EmployeesBLL Class](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)</span></span>

<span data-ttu-id="655e6-153">**圖 6**： 使用`EmployeesBLL`類別 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-153">**Figure 6**: Use the `EmployeesBLL` Class ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))</span></span>


<span data-ttu-id="655e6-154">[![選取從 GetEmployeesByHiredDateMonth(month) 方法](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-154">[![Select From the GetEmployeesByHiredDateMonth(month) method](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)</span></span>

<span data-ttu-id="655e6-155">**圖 7**: Select From`GetEmployeesByHiredDateMonth(month)`方法 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-155">**Figure 7**: Select From the `GetEmployeesByHiredDateMonth(month)` method ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))</span></span>


<span data-ttu-id="655e6-156">最後一個畫面會要求我們提供`month`參數值的來源。</span><span class="sxs-lookup"><span data-stu-id="655e6-156">The final screen asks us to provide the `month` parameter value's source.</span></span> <span data-ttu-id="655e6-157">因為我們會以程式設計方式設定此值，將參數的來源 設定為預設值沒有任何選項，然後按一下 完成。</span><span class="sxs-lookup"><span data-stu-id="655e6-157">Since we'll set this value programmatically, leave the Parameter source set to the default None option and click Finish.</span></span>


<span data-ttu-id="655e6-158">[![保留參數的 [來源] 設定為 None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-158">[![Leave the Parameter Source Set to None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)</span></span>

<span data-ttu-id="655e6-159">**圖 8**： 保留參數來源設定為 None ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-159">**Figure 8**: Leave the Parameter Source Set to None ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))</span></span>


<span data-ttu-id="655e6-160">這會建立`Parameter`物件的 ObjectDataSource`SelectParameters`並沒有指定值的集合。</span><span class="sxs-lookup"><span data-stu-id="655e6-160">This will create a `Parameter` object in the ObjectDataSource's `SelectParameters` collection that does not have a value specified.</span></span>


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

<span data-ttu-id="655e6-161">若要以程式設計方式設定此值，我們需要建立 ObjectDataSource 的事件處理常式`Selecting`事件。</span><span class="sxs-lookup"><span data-stu-id="655e6-161">To set this value programmatically, we need to create an event handler for the ObjectDataSource's `Selecting` event.</span></span> <span data-ttu-id="655e6-162">若要這麼做，請移至 [設計] 檢視，並按兩下 ObjectDataSource。</span><span class="sxs-lookup"><span data-stu-id="655e6-162">To accomplish this, go to the Design view and double-click the ObjectDataSource.</span></span> <span data-ttu-id="655e6-163">或者，選取 ObjectDataSource、 移至 [屬性] 視窗中，並按一下閃電圖示。</span><span class="sxs-lookup"><span data-stu-id="655e6-163">Alternatively, select the ObjectDataSource, go to the Properties window, and click the lightning bolt icon.</span></span> <span data-ttu-id="655e6-164">接下來，請按兩下在文字方塊旁`Selecting`事件或輸入您想要使用的事件處理常式的名稱。</span><span class="sxs-lookup"><span data-stu-id="655e6-164">Next, either double-click in the textbox next to the `Selecting` event or type in the name of the event handler you want to use.</span></span>


![按一下 [屬性] 視窗，列出 Web 控制項的事件中的閃電圖示](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

<span data-ttu-id="655e6-166">**圖 9**： 按一下 屬性 視窗，列出 Web 控制項的事件中的閃電圖示</span><span class="sxs-lookup"><span data-stu-id="655e6-166">**Figure 9**: Click on the Lightning Bolt Icon in the Properties Window to List a Web Control's Events</span></span>


<span data-ttu-id="655e6-167">這兩種方法的 ObjectDataSource 的加入新的事件處理常式`Selecting`頁面的程式碼後置類別的事件。</span><span class="sxs-lookup"><span data-stu-id="655e6-167">Both approaches add a new event handler for the ObjectDataSource's `Selecting` event to the page's code-behind class.</span></span> <span data-ttu-id="655e6-168">這個事件處理常式中，我們可以讀取和寫入使用的參數值`e.InputParameters[parameterName]`，其中*`parameterName`* 的值`Name`屬性中`<asp:Parameter>`標記 (`InputParameters`集合也可以是索引序數，如同在`e.InputParameters[index]`)。</span><span class="sxs-lookup"><span data-stu-id="655e6-168">In this event handler we can read and write to the parameter values using `e.InputParameters[parameterName]`, where *`parameterName`* is the value of the `Name` attribute in the `<asp:Parameter>` tag (the `InputParameters` collection can also be indexed ordinally, as in `e.InputParameters[index]`).</span></span> <span data-ttu-id="655e6-169">若要設定`month`參數，以目前的月份中，將下列內容加入`Selecting`事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="655e6-169">To set the `month` parameter to the current month, add the following to the `Selecting` event handler:</span></span>


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

<span data-ttu-id="655e6-170">當瀏覽此頁面，透過瀏覽器可以看到該只有一位員工的雇用本月 （年 3 月） Laura Callahan，自 1994 年以來已被使用的公司。</span><span class="sxs-lookup"><span data-stu-id="655e6-170">When visiting this page through a browser we can see that only one employee was hired this month (March) Laura Callahan, who's been with the company since 1994.</span></span>


<span data-ttu-id="655e6-171">[![顯示這個月的週年紀念日員工](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="655e6-171">[![Those Employees Whose Anniversaries This Month Are Shown](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)</span></span>

<span data-ttu-id="655e6-172">**圖 10**： 這些員工的週年紀念日這個月所示 ([按一下以檢視完整大小的影像](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))</span><span class="sxs-lookup"><span data-stu-id="655e6-172">**Figure 10**: Those Employees Whose Anniversaries This Month Are Shown ([Click to view full-size image](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))</span></span>


## <a name="summary"></a><span data-ttu-id="655e6-173">總結</span><span class="sxs-lookup"><span data-stu-id="655e6-173">Summary</span></span>

<span data-ttu-id="655e6-174">雖然 ObjectDataSource 的參數值通常會設定以宣告方式，而不需要任何一行程式碼中，很容易就能以程式設計方式設定參數值。</span><span class="sxs-lookup"><span data-stu-id="655e6-174">While the ObjectDataSource's parameters' values can typically be set declaratively, without requiring a line of code, it's easy to set the parameter values programmatically.</span></span> <span data-ttu-id="655e6-175">我們要做的就是建立 ObjectDataSource 的事件處理常式`Selecting`引發的事件，基礎物件的方法是叫用，並手動設定透過一或多個參數的值之前`InputParameters`集合。</span><span class="sxs-lookup"><span data-stu-id="655e6-175">All we need to do is create an event handler for the ObjectDataSource's `Selecting` event, which fires before the underlying object's method is invoked, and manually set the values for one or more parameters via the `InputParameters` collection.</span></span>

<span data-ttu-id="655e6-176">本教學課程結束時的基本報表區段。</span><span class="sxs-lookup"><span data-stu-id="655e6-176">This tutorial concludes the Basic Reporting section.</span></span> <span data-ttu-id="655e6-177">[下一個教學課程](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)一開始會篩選和主版詳細資料的案例 區段中，我們將查看允許的訪問項來篩選資料的技術，並從主要報表鑽研至詳細資料報表。</span><span class="sxs-lookup"><span data-stu-id="655e6-177">The [next tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kicks off the Filtering and Master-Details Scenarios section, in which we'll look at techniques for allowing the visitor to filter data and drill down from a master report into a details report.</span></span>

<span data-ttu-id="655e6-178">快樂地寫程式 ！</span><span class="sxs-lookup"><span data-stu-id="655e6-178">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="655e6-179">關於作者</span><span class="sxs-lookup"><span data-stu-id="655e6-179">About the Author</span></span>

<span data-ttu-id="655e6-180">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。</span><span class="sxs-lookup"><span data-stu-id="655e6-180">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="655e6-181">Scott 會擔任獨立的顧問、 培訓講師和作家。</span><span class="sxs-lookup"><span data-stu-id="655e6-181">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="655e6-182">他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="655e6-182">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="655e6-183">他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="655e6-183">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span> <span data-ttu-id="655e6-184">或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。</span><span class="sxs-lookup"><span data-stu-id="655e6-184">or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="655e6-185">特別感謝</span><span class="sxs-lookup"><span data-stu-id="655e6-185">Special Thanks To</span></span>

<span data-ttu-id="655e6-186">本教學課程系列是由許多實用的檢閱者檢閱。</span><span class="sxs-lookup"><span data-stu-id="655e6-186">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="655e6-187">本教學課程中的潛在客戶檢閱者已 Hilton Giesenow。</span><span class="sxs-lookup"><span data-stu-id="655e6-187">Lead reviewer for this tutorial was Hilton Giesenow.</span></span> <span data-ttu-id="655e6-188">有興趣檢閱我即將推出的 MSDN 文章嗎？</span><span class="sxs-lookup"><span data-stu-id="655e6-188">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="655e6-189">如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="655e6-189">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="655e6-190">[上一頁](declarative-parameters-cs.md)
> [下一頁](displaying-data-with-the-objectdatasource-vb.md)</span><span class="sxs-lookup"><span data-stu-id="655e6-190">[Previous](declarative-parameters-cs.md)
[Next](displaying-data-with-the-objectdatasource-vb.md)</span></span>
