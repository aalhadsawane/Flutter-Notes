### SizedBox Widget
-A box with a specified size. (specified in pixels).
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
    return GestureDetector(
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
###### GestureDetector widget
This widget detects gestures and has enum of gestures like onTap, onLongPress, onDoubleTap etc. Set a setState function to them to see the change upon gesture. This widget is also used in the above example of animated container.
