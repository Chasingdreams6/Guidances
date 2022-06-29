## flutter 哲学
1. Everything is Widget.

## flutter渲染机制
ref: https://juejin.cn/post/6844903777565147150

Widget是不可变的，只是一个配置信息，可以多次出现，真正的实体是element-tree.  
BuildContext对象实际上就是Element对象，BuildContext 接口用于阻止对 Element 对象的直接操作。
在build(context)时，首先创建一个element,再将widget对应的配置信息导入，build实际是widget的方法，需要一个element的函数，就是BuildContext.
#### StatelessWidget
首先它会调用StatelessWidget的 createElement 方法，并根据这个widget生成StatelesseElement对象。
将这个StatelesseElement对象挂载到element树上。
StatelesseElement对象调用widget的build方法，并将element自身作为BuildContext传入。
#### StatefulWidget
首先同样也是调用StatefulWidget的 createElement方法，并根据这个widget生成StatefulElement对象，并保留widget引用。
将这个StatefulElement挂载到Element树上。
根据widget的 createState 方法创建State。
StatefulElement对象调用state的build方法，并将element自身作为BuildContext传入。
#### navigator必须要在一个widget中使用（有father node）
当我们在 build 函数中使用Navigator.of(context)的时候，这个context实际上是通过 MyApp 这个widget创建出来的Element对象，而of方法向上寻找祖先节点的时候（MyApp的祖先节点）并不存在MaterialApp，也就没有它所提供的Navigator。
所以当我们把Scaffold部分拆成另外一个widget的时候，我们在FirstPage的build函数中，获得了FirstPage的BuildContext，然后向上寻找发现了MaterialApp，并找到它提供的Navigator，于是就可以愉快进行页面跳转了。

## 语法
1. 以下划线（_）开头的成员或类是私有的。
2. 创建一个含状态的组件需要两个类，外面的类是statefulWidget，含有一个createState的函数，里面的类是一个State<T>模板，T是外面statefulWidget的名字，
  具体的逻辑写在State<T>中，可以使用stful语法糖自动创建一个含状态组件。
3. https://flutter.cn/docs/development/ui/interactive 由父组件管理状态，给子组件一个回调函数。

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

#### Hint
在使用expanded的时候，如果固定了多个expanded的flex比例，且其中某些的长度是固定的，就有可能出现bottom overflow的问题。因为某些的长度固定，按照比例算出其他组件的长度
，总的长度就可能会超。

#### scalable widget ? 

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
     
## test
1. 一般前端的test中不允许发http请求，因为网络请求的结果可能不稳定，需要用mock拦截http请求，将所有的http请求置换为200OK. flutter中的一个库是https://pub.dev/packages/network_image_mock
2. find.test('str')，即使是一行的渲染，有可能是两个不同的组件，也许不能直接find
3. find.byKey(key)可以按key寻找组件，需要在组件内分配key


## key
key是flutter内部组件彼此之间的识别码，key分为local key和global key.

local key分为value key, object key和unique key.

value key比较值是否相同，object key比较对象地址是否相同，unique key不可能相同.

gobal key 用于不同组件之间，内部可以蕴含一个状态组件.


