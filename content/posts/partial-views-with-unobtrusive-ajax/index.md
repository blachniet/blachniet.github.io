---
title: "Partial Views with Unobtrusive AJAX"
date: "2011-08-10"
categories:
- "site-updates"
tags:
- "ajax"
- "asp-net"
- "csharp"
- "mvc-3"
- "razor"
aliases:
- "/blog/partial-views-with-unobtrusive-ajax/"
blachnietWordPressExport: true
---

In my recent exploration of web development in ASP.NET, I found what I assume to be a fairly common need to have part of a view/page update without the entire page updating. In my particular case, I wanted to have a page that listed items but also provided a form that allowed you to add an item. When an item was added, the list of items would be updated without having to regenerate the entire page. I eventually accomplished this through the unobtrusive AJAX provided in the MVC and jQuery frameworks. Below you can see the Razor page that was used to solve my problem. In this page, we include the _jquery.unobtrusive-ajax.min.js_ in order to create a form using the **AjaxHelper**. The AJAX form indicates that it needs to call the action function _Index\_AddItem_, and then update the _productList_ when the form is submitted. At the bottom, in the _productList_ div, we render the partial view, _ProductListControl_, with the list of products. (**ProductIndexViewModel** contains a **Product** and an **IEnumerable.** See Rachel Appel's post for more information on ViewModels in MVC here: [http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp.net-mvc-applications](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp.net-mvc-applications))

```
@* Views\Product\Index.cshtml *@
@model BoringStore.ViewModels.ProductIndexViewModel
@{
    ViewBag.Title = "Index";
}
<h2>Index</h2>

<script src="@Url.Content("~/Scripts/jquery.unobtrusive-ajax.min.js")" type="text/javascript"></script>
@using (Ajax.BeginForm("Index_AddItem", new AjaxOptions { UpdateTargetId = "productList" }))
{
    <div>
        @Html.LabelFor(model => model.NewProduct.Name)
        @Html.EditorFor(model => model.NewProduct.Name)
    </div>
    <div>
        @Html.LabelFor(model => model.NewProduct.Price)
        @Html.EditorFor(model => model.NewProduct.Price)
    </div>
    <div>
        <input type="submit" value="Add Product" />
    </div>
}

<div id='productList'>
    @{ Html.RenderPartial("ProductListControl", Model.Products); }
</div>
```

The _ProductListControl.cshtml_ is a very simple partial view that displays all the product names and prices in a table:

```
@* Views\Product\ProductListControl.cshtml *@
@model IEnumerable<BoringStore.Models.Product>
<table>
    <!-- Render the table headers. -->
    <tr>
        <th>Name</th>
        <th>Price</th>
    </tr>
    <!-- Render the name and price of each product. -->
    @foreach (var item in Model)
    {
        <tr>
            <td>@Html.DisplayFor(model => item.Name)</td>
            <td>@Html.DisplayFor(model => item.Price)</td>
        </tr>
    }
</table>  
```

The controller has a GET action for the Index and a POST action for adding the item. The _Index\_AddItem_ function is what the AJAX form calls when it is submitted. This function returns a partial view, allowing us to avoid regenerating the entire page.

```csharp
// Controllers\ProductController.cs 
namespace BoringStore.Controllers 
{ 
    public class ProductController : Controller 
    { 
        public ActionResult Index() 
        { 
            BoringStoreContext db = new BoringStoreContext(); 
            ProductIndexViewModel viewModel = new ProductIndexViewModel 
            { 
                NewProduct = new Product(), 
                Products = db.Products 
            }; 
            return View(viewModel); 
        } 

        [HttpPost] 
        public ActionResult Index_AddItem(ProductIndexViewModel viewModel) 
        { 
            BoringStoreContext db = new BoringStoreContext(); 
            db.Products.Add(viewModel.NewProduct); 
            db.SaveChanges(); 
            return PartialView("ProductListControl", db.Products); 
        } 
    } 
}
```

**Summary:** The unobtrusive AJAX provided by MVC and jQuery looks like it may be useful in many areas in my future web development. I hope this post helps get some people to this solution faster than I got to it. If you see any errors in the this post, or have suggestions for alternative ways of accomplishing the same goal, please feel free to share.
