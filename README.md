# ngMobileDialog
A responsive dialog projected as mobile-first, in mobile use fullscreen like iOS modals, and 768+ screens use centralized dialogs

Demo

   [http://plnkr.co/dz3vZyrgBi6jSjbjmIqi](https://plnkr.co/dz3vZyrgBi6jSjbjmIqi/)



Install

    npm install ng-mobile-dialog
    
Or

    bower install ngMobileDialog
    
Include minified version
```html
	<script src="bower_components/ngMobileDialog/dialog.js"></script>
```

To debug include max version
```html
	<script src="bower_components/ngMobileDialog/dialog.max.js"></script>
``` 
Use
```javascript
    angular.module("app", ["ngMobileDialog"], function ($dialogProvider) {
    	//Default is false
    	$dialogProvider.multiple = true;
    });
```
Markup
```html
    <button ng-click="openDialog()">Open Dialog</button>
```
Javascript

main-controller.js
```javascript
    
angular.module("app").controller("MainController", function ($scope, $dialog) {
    $scope.openDialog = function() {
        var resolverOne = function () {
            return "MainController"
        };
        $dialog.create({templateUrl: "dialog.html", controller: "DialogController", resolve: {resolveOne: resolverOne}}, function (dialog) {
            dialog.open().then(function (someValue) {
                //Will show: This can be whatever from dialog like an object or string
                alert(someValue);
            });
        });
    };
    
     $scope.openDialogTwo = function() {
        var resolverOne = function () {
            return "MainController"
        };
        
        var template = '<div role="dialog" tabindex="-1" class="dialog" aria-labelledby="dialog-title">'+
                        '  <div role="document" class="modal-dialog">'+
                        '     <div class="modal-content">'+
                        '<div class="modal-header">'+
                        '<button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>'+
                        '  <h1 id="dialog-title">this is a dialog from {{parent}}<!-- MainController will be show--></h1>'+
                        '</div>'+
                        '<div class="modal-body">'+
                        '  Modal Body'+
                        '</div>'+
                        '<div class="modal-footer">'+
                        '<button ng-click="close()">Close Dialog</button><br><br>'+
                        '<strong>Without parameters can resolve and close directly</strong>'+
                        '<button ng-click="resolve()">Close Dialog without parameters</button>'+
                        '</div>'+
                        '</div>'+
                        '</div>'+
                      '</div>';
        
        $dialog.create({template: template, controller: "DialogController", resolve: {resolveOne: resolverOne}}, function (dialog) {
            dialog.open().then(function (someValue) {
                //Will show: This can be whatever from dialog like an object or string
                alert(someValue);
            });
        });
    };
});
```
dialog-controller.js
```javascript
	angular.module("app").controller("DialogController", function ($scope, resolveOne) {
		$scope.parent = resolveOne;//MainController
		
		$scope.close = function () {
			$scope.resolve("This can be whatever from dialog like an object or string");
		};
	});
```

dialog.html
```html
<div role="dialog" tabindex="-1" class="dialog" aria-labelledby="dialog-title">
    <div role="document" class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>
            <h1 id="dialog-title">this is a dialog from {{parent}}<!-- MainController will be show--></h1>
          </div>

          <div class="modal-body">
            Modal Body
          </div>

          <div class="modal-footer">
            <button ng-click="close()">Close Dialog</button><br><br>
            <strong>Without parameters can resolve and close directly</strong>
            <button ng-click="resolve()">Close Dialog without parameters</button>
          </div>
        </div>
    </div>
</div>
```
Options

| Name      | Type   | Example                                                | Required            | Description                                                              |
|-------------|--------|--------------------------------------------------------|---------------------|--------------------------------------------------------------------------|
| controller     | String | "DialogController" | true               | The name of defined controller for dialog |
| controllerAs     | String | "vm" | false               | A abbreviation to controller name such as vm, it allows you to use vm.property at your view |
| scope     | Object | {parameter: 'myParameter'} | false               | A scope to be used in template |
| template    | String | "defined template string(from [Require.js Text](https://github.com/requirejs/text) for example)"                                      | If not template url | Template string                                                          |
| templateUrl | String | "src/app/dialog.html"                                  | If not template     | Template url for load                                                    |
| resolve     | Object | resolve: {resolveOne: function () { return "example"}} | false               | A object with keys of functions with values that will be passed to dialog controller |
| escKey     | Boolean | true(Default: false) | false               | A boolean that defines if modal will close with ESC press |

