<h1><b>ViewData、ViewBag、TempData、ViewMode</b></h1>

在ASP.NET MVC中，沒有 view stat，沒有code behind，沒有server controls。<br>
Controle傳遞資料到View，可使用ViewData、ViewBag、TempData三個物件。<br>
上述三個物件適合少量的資料傳遞。<br>

<h2><b>ViewData</b></h2>
● ViewData 是 ControllerBase 類的 property<br>
● ViewData 用於 Controller 將資料傳遞給對應的 view (Controller to View)<br>
● ViewData 生命週期只存在於當下的請求，導頁後就被清掉了 (null)<br>
● ViewData 是 Dictionary 類別，繼承 ViewDataDictionary 纇，要注意字典資料轉型跟 null 問題<br>
● Example - ViewData["Key"] = "Value"<br>

Home / Index

> public class HomeController:Controller

> {

>   public ActionResult Index()

>   {

>     ViewData["Company"] = "This is ViewData";

>   }

> }

Index.cshtml的View

> < div >

> < h1 >@ViewData["Company"]< /h1 >

> < /div >

<h2><b>ViewBag</b></h2>
● ViewBag 也是 ControllerBase 類的 property<br>
● ViewData 操作方法須對每個資料使用中括弧，ViewBag 簡化為點運算式<br>
● ViewBag 基本上是 ViewData 的包裝類別，也是用於 Controller 將資料傳遞給對應的View (Controller to View)<br>
● ViewBag 是 C# 4.0的 dynamic 型別，可在執行期再判斷真正型別 (C# reflection)。(效能較差)<br>
● ViewBag 生命週期也只存在於當下的請求，導頁後就被清掉了 (null)<br>
● Example - ViewBag.Key = "Value"<br>

Home / Index

> public class HomeController:Controller

> {

>   public ActionResult Index()

>   {

>     ViewBag.Company = "This is ViewBag";

>   }

> }

Index.cshtml的View

> < div >
>  < h1 >@ViewBag.Company< /h1 >
> < /div >

<h2><b>TempData</b></h2>
● TempData 也是 ControllerBase 類的 property<br>
● TempData 生命週期除了當下請求，導頁後仍可續存 (如action to action, controller to action)，或想像為暫時性的 Session<br>
● TempData也是 Dictionary 類別，繼承 TempDataDictionary 纇，要注意字典資料轉型跟Null問題<br>
● Example - TempData["Order1"] = order，注意 value 若為類別物件建議加上 [Serializable]<br>

Home / Index

> public class HomeController:Controller

> {

>   public ActionResult Index()

>   {

>     TempData["Company"] = "This is ViewBag";

>   }

> }

Index.cshtml的View

> < div >

>   < h1 >@TempData["Company"]< /h1 >

> < /div >

<h2><b>ViewMode</b></h2>
列舉3段簡化後的程式碼 :<br>
LoginViewModel 的宣告<br>
AccountController 針對 Login Action 的處理<br>
Login.csthml 操作 LoginViewModel<br>

AccountViesModels.cs

> public class LoginViewModel

> {

>   [Required]

>   [EmailAddress]

>   public string Email { get; set; }

>   [Required]

>   public string Password { get; set; }

> }

AccountController.cs

> public ActionResult Login(LoginViewModel model, string returnUrl)

> {

>   if (!ModelState.IsValid)

>   {

>     return View(model);

>   }

>   var isValid = IsVaild(model.Email, model.Password, model.RememberMe, out var msg);

>   if (isValid)

>   {

>     return RedirectToLocal(returnUrl);

>   }

>   ModelState.AddModelError("","登入失敗：" + msg);

>   return View(model);

> }

Login.csthml

> @using BlogWebApplication.Models

> @model LoginViewModel #是一個 View 頁面的模型宣告，一個 View 可以有一個 Model，注意是小寫的 model

> @{

>     ViewBag.Title = "Log in";

> }

> < h2 >@ViewBag.Title.< /h2 >

> < section id="loginForm" >

>  @using (Html.BeginForm("Login", "Account", new { ReturnUrl = ViewBag.ReturnUrl }, FormMethod.Post, new { @class = "form-horizontal", role = "form" }))

>  {

>   < h4 >Use a local account to log in.< /h4 >< hr />    

>   <!--@Html.ValidationSummary()可取得 Controller 透過 ModelState.AddModelError() 傳遞來的錯誤訊息, 做互動或除錯很好用-->

>   @Html.ValidationSummary(true, "", new { @class = "text-danger" }) 

>   <div class="form-group">

>    @Html.LabelFor(m => m.Email)

>    <!--可產生 <input id="Name" name="Name" type="text" value="@Model.Email"> 的 HTML 腳本，但這寫法有點囉嗦, 也隱含 Model 為 null 時的錯誤。注意是大寫的 Model-->

>    @Html.TextBox("Email", Model.Email, new { @class = "form-control" })

>    <!--同樣產生 HTML 語法, 但能自動取得 property name, 是不是優雅多了, 推薦這種 For 結尾的 Lambda 用法-->

>    @Html.ValidationMessageFor(m => m.Email, "", new { @class = "text-danger" })

>   </div>

>   <div class="form-group">

>    @Html.LabelFor(m => m.Password)

>    @Html.PasswordFor(m => m.Password)

>    @Html.ValidationMessageFor(m => m.Password, "", new { @class = "text-danger" })

>   </div>

>   <div>

>    <input type="submit" value="Log in" />

>   </div>

>  }

> </section>
