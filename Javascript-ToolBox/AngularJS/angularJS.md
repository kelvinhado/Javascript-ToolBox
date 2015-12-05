#### Angular JS

What's good with Angular ?

 it allows you to write expressive html through custom directives so people can read the html and understand

*index.html*
```html
<!DOCTYPE html>
<!-- to notice this html section that it will be a part of our angular app-->
<html ng-app="gemStore">
  <head>
    <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
    <script type="text/javascript" src="angular.min.js"></script>
    <!--call our app.js -->
    <script type="text/javascript" src="app.js"></script>
  </head>
  <body>
    <!-- to insert dynamic data-->
    <h1>{{ "hello you "}}</h1>
  </body>
</html>
```

*app.js*
```javascript
// gemStore : name of the app, [ ] : dependencies
var app = angular.module("gemStore", []);
```

### Controllers
*app.js*
```js
(function(){
  var gem = { name: 'Azurite', price: 2.95 };
  var app = angular.module('gemStore', []);

  app.controller("StoreController", function() {
    this.product = gem;
    });

 })();
```
*index.html*
```html
<!DOCTYPE html>
<html ng-app="gemStore">
  <head>
    <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
    <script type="text/javascript" src="angular.min.js"></script>
    <script type="text/javascript" src="app.js"></script>
  </head>
  <body ng-controller="StoreController as store">
    <div class="product row">
      <h3>
        {{ store.product.name }}
        <em class="pull-right">{{ store.product.price }}</em>
      </h3>
    </div>
  </body>
</html>
```

### Display a list of products, use show, hide, repeat directives

*app.js*
```js
(function() {
  var app = angular.module('gemStore', []);

  app.controller('StoreController', function(){
    this.products = gems;
  });

  var gems = [
    { name: 'Azurite', price: 2.95 },
    { name: 'Bloodstone', price: 5.95 },
    { name: 'Zircon', price: 3.95 }
  ];
})();
```
*index.html*
```html
<!DOCTYPE html>
<html ng-app="gemStore">
  <head>
    <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
    <script type="text/javascript" src="angular.min.js"></script>
    <script type="text/javascript" src="app.js"></script>
  </head>
  <body class="container" ng-controller="StoreController as store">
    <div class="product row" ng-repeat="product in store.products">
      <h3>
        {{product.name}}
        <em class="pull-right">${{product.price}}</em>
      </h3>
    </div>
  </body>
</html>
```

### Directives known

* **ng-app** : attach the Application Module to the page
* **ng-controller** : attach a Controller function to the page
* **ng-show/ng-hide** : display a section based on an Expression
* **ng-repeat** : repeat a section for each item in an Array
* **ng-init** : initialize a value for a var
* **ng-click** : trigger click from the user
* **ng-class** : to change class attributes in the html (ex active state)
* **ng-submit** : allow us to call a function when a form is submitted
* **ng-include** : include an external file

### ng-src Angular Sources
*index.html*
```html
<div class="list-group-item" ng-repeat="product in store.products">
  <img ng-src="{{product.images[0]}}" />
</div>
```

### filters
we can use filters to format the text we display, some examples :
* **date** :
```js
{{'133242354534534' | date:'MM/dd/yyy @ h:mma'}}
```
* **uppercase & lowercase** :
```js
{{'my text' | uppercase }}
```
* **limitTo** :
```js
{{'keep the first the 8 caracteres' | limitTo:8 }}
<li ng-repeat="product in store.product | limitTo:3"> //display the first 3 products
```
* **orderBy** :
```js
<li ng-repeat="product in store.product | orderBy:'-price'">
```

### Controllers - 2way data binding
example: tab controller for tabs in html file

1) create app controller
```js
app.controller('TabController', function(){
        //initialize the tab
    this.tab = 1;
    this.setTab = function(setTab){
      this.tab = setTab;
     };
    this.isSet = function(value) {
      return this.tab === value;
    };
```
2) attach the TabController to the <balise> using ng-controller with an alias
```html
<section class="tab" ng-controller="TabController as panel">
```
3) Trigger the setTab with ng-click
```html
<a ng-click="panel.setTab(1)">Description</a>
```
4) Use ng-show directive to show the appropriate content using the isSet methods from the controller
```html
<div ng-show="panel.isSet(1)"> <!--div to show when the first tab is clicked -->
```
5) Add the active class (ng-class)
```html
<li ng-class="{active : panel.isSet(1)}">
```

### Form preview binding
1) into your form, add **ng-model** to each input you wanna binding
```html
<input ng-model="review.author" type="email" />
```
2) just use the same tag to access the value that is automatically updated
```
{{review.author}}
```

### Submit a form
1) controller
```js
app.controller('ReviewController', function() {
   this.review = {};
   this.addReview = function(product){
     product.reviews.push(this.review);
     this.review = {};  //then reset the review to clear in html
    };   
 });
```
2) add controller to the form with alias reviewCtrl
3) add the call to the addReview function (define in the controller) using ng-submit inside the form tag
```html
<form name="reviewForm" ng-controller="ReviewController as reviewCtrl"  ng-submit="reviewCtrl.addReview(product)">
```
4) don't forget to prefix all using the alias in the ng-model..

### Form Validation !
1) Turn off default HTML validation. add "novalidate" in form tag
2) add "required" in required input tags
3) to preven the submit if not valid, add a name to your form tag and modify the ng-submit like below :
```html
<form name="reviewForm" ng-controller="ReviewController as reviewCtrl" ng-submit="reviewForm.$valid && reviewCtrl.addReview(product)" novalidate>
```

### Form Styling
To guide the user, we can add some style to the form using CSS.

There are several step during the completion of an input :
  * ng-invalid : input empty of not good
  * ng-dirty : input not empty
  * ng-valid : input valid
So in the CSS we add style like below :
```css
.ng-invalid.ng-dirty {
 		border-color: red;
}
.ng-valid.ng-dirty {
 		border-color: green;
}
```

### Custom directives
We can move repetitive code into other html file and then call them using  ng-include (added to a tag)
```html
<div ng-show="tab.isSet(1)" ng-include="'product-description.html'">
</div>
```

or we can use custom directives that have to be define in the app.js
There are two kind of custom directives, Element or Attributes directives

#### 1) Element directive

```html
<product-description></product-description>
```
note that product-description will call the directive named productDescription. We are also using two tags instead of <.../> because some brothers doesn't like that on.

```js
app.directive('productDescription', function() {
  return {
    restrict: 'E',
    templateUrl: 'product-description.html'
   };
});
```

#### 2) Attribute directive
Same thing with the exception that we will use *restrict:'A'* instead. and add the name of the directive *product-description* into the div tag.

#### How to deal with controller with directives ?
If a section that you want to export in an other html file, contain controllers you will have to adapt the directive

**create the productTab directive (for example)**
```js
app.directive('productTabs', function() {
  return {
    restrict: 'E',
    templateUrl: 'product-tabs.html',
    controller: function(){
            this.tab = 1;
            this.isSet = function(checkTab) {
              return this.tab === checkTab;
            };

            this.setTab = function(setTab) {
              this.tab = setTab;
            };
       },
     controllerAs:'tab'  //so the directive knows what all the references to tab in product-tabs.html are
    };
});
```
we do not have ng-controller anymore (see below) and the html will know what is tab
```html
<section>
          <ul class="nav nav-pills">
            <li ng-class="{ active:tab.isSet(1) }">
              <a href ng-click="tab.setTab(1)">Description</a>
            </li>
            <li ng-class="{ active:tab.isSet(2) }">
              <a href ng-click="tab.setTab(2)">Specs</a>
            </li>
            <li ng-class="{ active:tab.isSet(3) }">
              <a href ng-click="tab.setTab(3)">Reviews</a>
            </li>
          </ul>

          <!--  Description Tab's Content  -->
          <div ng-show="tab.isSet(1)" ng-include="'product-description.html'">
          </div>

          <!--  Spec Tab's Content  -->
          <div product-specs ng-show="tab.isSet(2)"></div>

          <!--  Review Tab's Content  -->
          <product-reviews ng-show="tab.isSet(3)"></product-reviews>

</section>
```
we simply use it like it
```html
<product-tabs></product-tabs>
```

### refactor the code in the app.js

if we want to put all directives that deal with products in an other file :

1) new *products.js*
```js
(function(){
  var app = angular.module('store-products', [ ]);

  // ...
  // all product custom directives here
  // ...

})();
```
2) add dependencies in the main app.js
```js
(function() {
  var app = angular.module('gemStore', ['store-products']);
)();
```
3) Link in the new js file in the html file
```html
<script type="text/javascript" src="products.js"></script>
```

### Services,
