官方的代码 

```
<ion-side-menus>
  <!-- Left menu -->
  <ion-side-menu side="left">
  </ion-side-menu>

  <ion-side-menu-content>
  <!-- Main content, usually <ion-nav-view> -->
  </ion-side-menu-content>

  <!-- Right menu -->
  <ion-side-menu side="right">
  </ion-side-menu>

</ion-side-menus>
function ContentController($scope, $ionicSideMenuDelegate) {
  $scope.toggleLeft = function() {
    $ionicSideMenuDelegate.toggleLeft();
  };
}
```

手动触发:


```
$scope.$on("$ionicView.beforeEnter", function(){
})
```

不能解决问题

```
stateId:"first",
stateName:"first",
stateParams:undefined
```

手动设置：

```
  $scope.$on("$ionicView.beforeEnter", function(){
    init();
    var currentView = $ionicHistory.currentView();
    currentView.stateId = "second";
    currentView.stateName = "second";
    $ionicHistory.currentView(currentView);
  });
```

----

https://github.com/ionic-team/ionic/issues/2802


