---
layout: post
title: "WinJS: Binding datacontext to a page"
date: 2013-09-15 14:03
comments: false
categories:
- WinJS
- Windows Store apps
- HTML5
- Javascript
- MVVM

---
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce mollis ante et nisi consequat, nec vulputate ante tincidunt. Cras euismod suscipit luctus. Donec sed nisi a erat malesuada malesuada ultricies vitae magna.

#### HTML:
{% codeblock lang:html %}
<!DOCTYPE html><html>    <head>        <meta charset="utf-8" />        <title>Products</title>        <link href="//Microsoft.WinJS.1.0/css/ui-dark.css" rel="stylesheet" />        <script src="//Microsoft.WinJS.1.0/js/base.js"></script>        <script src="//Microsoft.WinJS.1.0/js/ui.js"></script>        <link href="/css/default.css" rel="stylesheet" />        <link href="/pages/products/products.css" rel="stylesheet" />        <script src="/pages/products/products.js"></script>    </head>    <body>
        <div id="productTemplate" data-win-control="WinJS.Binding.Template">
            <img src="#" data-win-bind="src: image; alt: name" />            <div data-win-bind="innerText: name"></div>            <div data-win-bind="innerText: description"></div>    	</div>        <div data-win-control="WinJS.UI.ListView"
             data-win-options="{ itemTemplate:#productTemplate }"
             data-win-bind="winControl.itemDataSource:products">
        </div>
    </body>
</html>
{% endcodeblock %}

Quisque adipiscing odio interdum velit molestie, ullamcorper ornare lectus rutrum. Vestibulum in semper metus, ut tristique nisl. Ut laoreet molestie facilisis. Aenean condimentum in nibh eu malesuada.

#### Javascript:
{% codeblock lang:javascript %}
(function () {
    "use strict";
    ui.Pages.define("/pages/products/products.html", {
    	ready: function (element, options) {
    		var data = new DataService().getProducts();
    		var list = new WinJS.Binding.List(data);
    		var dataContext =
    			WinJS.Binding.as({ products: list.dataSource });
    		WinJS.Binding.processAll(element, dataContext);
    	},
    });
})();  
{% endcodeblock %}

### Using MVVM
Etiam pellentesque libero ut lorem suscipit gravida. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum laoreet mollis massa, nec fringilla purus congue quis. Donec et tortor adipiscing, viverra mauris at, aliquet massa.

#### HTML
{% codeblock lang:html %}
<div
    data-win-control="WinJS.UI.ListView"
    data-win-bind="winControl.itemDataSource:products;
                   winControl.itemTemplate:productTemplate">
</div>
{% endcodeblock %}

#### Javascript
Integer consequat, diam nec ultricies tristique, arcu dui laoreet lorem, id eleifend tellus lacus ac dolor.

ProductsViewModel.js
{% codeblock lang:javascript %}
(function () {
    var viewModel = WinJS.Class.define(
    	function() {
    		this.latestProducts = new DataService().getProducts();
    		//TODO: mark public members as supported-for-processing
    	},
    	{ 
    		latestProducts: null,
    		productTemplate: function(itemPromise) {
    			return itemPromise.then(function (item) {
    				var template = document.getElementById("productTemplate");
    				var container = document.createElement("div");
    				template.winControl.render(item.data, container);
    				return container;
    			})
    		} 
    	});
    	
    	WinJS.Namespace.define("App.ViewModels", {
    		ProductsViewModel: viewModel
    	});
})()
{% endcodeblock %}
    	
products.js

{% codeblock lang:javascript %}
(function () {
    "use strict";
    ui.Pages.define("/pages/products/products.html", {
    	ready: function (element, options) {
    	    WinJS.Binding.processAll(element, new App.ViewModels.ProductsViewModel());
    	},
    });
})();  
{% endcodeblock %}

the end :)