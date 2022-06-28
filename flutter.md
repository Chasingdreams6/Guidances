## layout
### 构建响应式layout
响应式layout可以自动适应不同设备大小
ref: https://juejin.cn/post/7026709670274269214
#### MediaQuery
使用MediaQuery可以读取context的高度和宽度，为浮点类型。

#### LayoutBuilder
使用LayoutBuilder可以查看子组件可以用的最大高度和宽度

#### OrientaionBuilder
查看子组件的方向，和MediaQuery提供的方向不同的是，当一个组件的宽度>高度时，即认定为landscape.

#### Expanded and Flexible
在一个等级中，Expanded 和 flexible共享flex的计算方式。通过分配不同的flex（默认为1） 来分配大小。expanded会填充满整个flex分配区域，而flexible只会填充子组件大小的区域。

#### FractionallySizedBox
当你想让你的widget，占据当前屏幕宽度和高度的百分之多少时，使用这个组件，想在Row和Column组件中使用百分比布局时，需要在FractionallySizedBox外包裹一个expanded或flexible
这会使得该Box区域强制扩展到expanded区域的对应百分比的大小。

#### AspectRatio
可以使用AspectRatio小部件将子元素的大小调整为特定的长宽比。首先，它尝试布局约束允许的最大宽度，并通过将给定的高宽比应用于宽度来决定高度。


## navigate
### navigator
ref: https://docs.flutter.dev/cookbook/navigation/hero-animations
navigator类似一个栈，通过push，把一个widget push到栈的顶，就可以显示这个页面。通过pop，可以回到上一个页面。

```dart
Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondRoute()),
            );
Navigator.pop(context);
```
### named navigator
ref: https://docs.flutter.dev/cookbook/navigation/named-routes
通过在material App中将每个页面分配一个route name，在push时就可以直接push这个name，而不是push widget.
这样做是因为如果有很多地方都指向同一个不可变的页面，之前的做法会存在很多相同的widget，现在的做法只会出现一个 widget
```dart
MaterialApp(
  title: 'Named Routes Demo',
  // Start the app with the "/" named route. In this case, the app starts
  // on the FirstScreen widget.
  initialRoute: '/',
  routes: {
    // When navigating to the "/" route, build the FirstScreen widget.
    '/': (context) => const FirstScreen(),
    // When navigating to the "/second" route, build the SecondScreen widget.
    '/second': (context) => const SecondScreen(),
  },
)
// Within the `FirstScreen` widget
onPressed: () {
  // Navigate to the second screen using a named route.
  Navigator.pushNamed(context, '/second');
}
// Within the SecondScreen widget
onPressed: () {
  // Navigate back to the first screen by popping the current route
  // off the stack.
  Navigator.pop(context);
}
```
            
