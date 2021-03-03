# Frequently Used Widgets In Flutter

### SizedBox Widget
A box with a specified size. (specified in pixels).
Even if the SizedBox widget has no children it will take up whitespace with given dimensions.
```
SizedBox(
  width: 200.0,
  height: 300.0,
  child: const Card(child: Text('Hello World!')),
)
```
### ConstrainedBox Widget
Lets you specify maximum or minimum width or height of its child widget.
```
ConstrainedBox(
  constraints: BoxConstraints(
      maxWidth : 200,
  ),
  child : Text(
      'Delicious Candy',
      textAlign : TextAlign.center,
  )
)
```
Below, snippet makes the child widget (a Card with some Text) fill the parent, by applying BoxConstraints.expand constraints. The same behavior can be obtained using the new SizedBox.expand widget.
```
ConstrainedBox(
  constraints: const BoxConstraints.expand(),
  child: const Card(child: Text('Hello World!')),
)
```
```
SizedBox.expand(
    child : const Card(child: Text('Hello World!')),
)
```
### FittedBox
Scales and positions its child within itself according to fit. Very important tool for layouting.
```FittedBox(
    fit: BoxFit.contain,
    child: MyDashPic(),
)
```
BoxFit has contain , fill, fitWidth, fillHeight, cover, none (image won't resize but instead discard the part of child which goes outside the box)
A detailed look at the BoxFit enum is [here](https://api.flutter.dev/flutter/painting/BoxFit-class.html)
### MainAxis
It is a property of rows and columns.
Main axis is horizontal for row and vertical for column.

###### MainAxisSize
The mainAxisSize property determines how much space a Row and Column can occupy on their main axes. It  has two possible values:
1)```MainAxisSize.max``` Row and Column occupy all of the space on their main axes.
2)```MainAxisSize.min``` Row and Column only occupy enough space on their main axes for their children.

###### MainAxisAlignment
When mainAxisSize is set to MainAxisSize.max, Row and Column might lay out their children with extra space. The mainAxisAlignment property determines how Row and Column can position their children in that extra space. mainAxisAlignment's enum:
1)MainAxisAlignment.start
2)MainAxisAlignment.end
3)MainAxisAlignment.center
4)MainAxisAlignment.spaceBetween (Divides the extra space evenly between children)
5)MainAxisAlignment.spaceEvenly  (Divides the extra space evenly between children and before and after the children.)
6)MainAxisAlignment.spaceAround  (Similar to MainAxisAlignment.spaceEvenly, but reduces half of the space before the first child and after the last child to half of the width between the children.)
### Padding Widget
There is actually no difference betwween adding this widget or adding a Container and setting its padding property to something but for clean code use this.
```
Padding(
    padding: EdgeInsets.all(16.0),
    child: Text('Hello World!'),
  )
```
### AnimatedPadding
Animated version of Padding which automatically transitions the indentation over a given duration whenever the given inset (a variable) changes.
```
//this is in a stateful widget
double padVal = 0; //always set this as double
_updatePadding(double val) => setState(() => padVal = value);
```
/*Note that only the code below is build at the very start and is rebuild when state is changed, so _updatePadding will change padVal and then set state which will rebuild the widgets given below, and then we will see a smooth animation.*/
```
Widget build(BuildContext context){

    return AnimatedPadding(
        padding: EdgeInsghts.all(padVal),
        duration: const Duration(seconds : 1),
        curve: Curves.easeInOut, //curve = transition
        child: Container(color: Colors.blue),
    )
}
```
### AnimatedContainer
Works just like AnimatedPadding but instead of just animating just the padding, this widget animates and changes transform and other properties of the child over a duration. Instead of having just one variable (padVal), we can now have many variables like (color: \_color) or (alignment: \_alignment) or booleans like (selected) which can change various properties of the child widget.
```
class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  bool selected = false;

  @override
  Widget build(BuildContext context) {
    return InkWell(
      onTap: () {
        setState(() {
          selected = !selected;
        });
      },
      child: Center(
        child: AnimatedContainer(
          width: selected ? 200.0 : 100.0,
          height: selected ? 100.0 : 200.0,
          color: selected ? Colors.red : Colors.blue,
          alignment:
              selected ? Alignment.center : AlignmentDirectional.topCenter,
          duration: Duration(seconds: 2),
          curve: Curves.fastOutSlowIn,
          child: FlutterLogo(size: 75),
        ),
      ),
    );
  }
}
```
```width: selected ? 200.0 : 100.0``` is short hand for if selected = true, width: 200.0 else width: 100.0
Also you can see the result of the AnimatedContainer code block [here](https://api.flutter.dev/flutter/widgets/AnimatedContainer-class.html#widgets.AnimatedContainer.1)
###### InkWell widget
This widget detects gestures and has enum of gestures like onTap, onLongPress, onDoubleTap etc. Set a setState function to them to see the change upon gesture. This widget is also used in the above example of animated container. This widget also adds a ripple effect to its child (i.e. we get a splashColor on child)If there is no child, the InkWell widget simply functions on it's parent. For more gestures like dragging you can use ```GestureDetector``` widget.

### BoxDecoration
The BoxDecoration class provides a variety of ways to draw a box.
The box has a border, a body, may cast a boxShadow and if it's a rectangle, may have borderRadius property. 
The body of the box is painted in layers. The bottom-most layer is the color, which fills the box. Above that is the gradient, which also fills the box. 
Finally there is the image, the precise alignment of which is controlled by the ```DecorationImage```  widget (which has the "image" property).
BoxDecoration is a widget in decoration property of a Container or a DecoratedBox. Both are essentially same but DecorationBox is efficient when using primarily for decoration and is considered clean code.
```
DecoratedBox(
  decoration: BoxDecoration(
    color: const Color(0xff7c94b6),
    image: const DecorationImage(
      image: NetworkImage('https://flutter.github.io/assets-for-api-docs/assets/widgets/owl-2.jpg'),
      fit: BoxFit.cover,
    ),
    border: Border.all(
      color: Colors.black,
      width: 8,
    ),
    borderRadius: BorderRadius.circular(12),
  ),
)
```

### ListView
A scrollable list of widgets arranged linearly. It has a very important property- ```scrollDirection```.
```
ListView(
  padding: const EdgeInsets.all(8),
  scrollDirection: Axis.horizontal,
  children: <Widget>[BlueBox(), BlueBox(), BlueBox()],
)
```
### ListView.builder
This constructor is appropriate for list views with a large (or infinite) number of children because the builder is called only for those children that are scrolled onto the screen and are actually visible.This process is also called "lazily rendering widgets".
###### ListView.seperated
This widget gives more acces to divider between the list items when number of list items is known.
### PageView and PageController widget
You can use a PageController to control which page is visible in the view. These widgets control how to swipe onto the next screens. These 2 widgets are best explained [here](https://api.flutter.dev/flutter/widgets/PageView-class.html).
### Stack
The widget for creating overlapping children widgets. Stack takes a list of widgets (children) and stacks them ground up (meaning that the first widget in children will be at bottom in the stack). Each child can be either ```Positioned``` or non-positioned. Positioned children are those wrapped in a Positioned widget that has at least one non-null property. This means, the children of Positioned widget (which will be a child of Stack) will be positioned. Others, which are not the children of a positioned widget or directly a positioned widget will be a non-positioned widget. 
The stack sizes itself to contain all the non-positioned children, which are positioned according to alignment property of Stack.
Stack has a property ```fit``` which is defaulted to ```fit: StackFit.loose```, you can also make the stack expand to fill its parent by ```fit: StackFit.expand```. It also has a property named ```overflow``` which can be set to either allow overflow by ```overflow: Overflow.visible``` or hide the the overflow by ```overflow: Overflow.clip```.
Note that positioned widget's position is relative to stack's edges.
```
Stack(
  children: <Widget>[
    fit: StackFit.loose, //This is default fit
    alignment: AlignmentDirectional.topStart //This is default alignment
    overflow: Overflow.clip,
    Positional(
      bottom: 50, //50 pixels from bottom
      right: 50, //50 pixels from right
      width: 60,
      height: 60,
      color: Colors.red,
    ),
    Container( //non-positioned and its position and fit will be controlled by stack
      width: 100,
      height: 100,
      color: Colors.green,
    ),
    Container( //non-positioned and its position and fit will be controlled by stack
      width: 90,
      height: 90,
      color: Colors.blue,
    ),
  ],
)
```