English | [简体中文](./README_zh-CN.md)

# animated_interpolation

[![pub package](https://img.shields.io/pub/v/animated_interpolation.svg)](https://pub.dartlang.org/packages/animated_interpolation)

<div align=start>
<img src="https://github.com/flutter-studio/animated_interpolation/blob/master/untitled.gif" width = "280" alt="图片名称" align=center />
<img src="https://github.com/flutter-studio/animated_interpolation/blob/master/untitled2.gif" width = "280" alt="图片名称" align=center />
  </div>

<br/>
<br/>

A flutter interpolation plugin inspired by the React Native interpolation animation


## Usage
To use this plugin, add `animated_interpolation` as a [dependency in your pubspec.yaml file](https://flutter.io/platform-plugins/).

### InterpolationTween


#### constructor

| Parameter  |Required | Default   | Description |
| :------------ |:--------:|:---------------:| :-----|
| inputRange | YES | NULL  | Set the range of input,eg: ```[0,0.2,0.5,0.8,1]``` |
| outputRange |YES| NULL | Set the range of input eg: ```[10,100,105,200,300]``` |
| curve |NO| ```_Linear._()``` |  Set the input/output animation curve |
| extrapolate |NO| NULL | it will extrapolate the curve beyond the ranges given, but you can also have it clamp the output value |
| extrapolateLeft |NO| ExtrapolateType.extend  | It will extrapolate the curve beyond the given left range |
| extrapolateRight |NO| ExtrapolateType.extend  | It will extrapolate the curve beyond the given right range |

### SmartAnimatedWidget

#### constructor

| Parameter  |Required | Default   | Description |
| :------------ |:--------:|:---------------:| :-----|
| from | No | NULL  | It's similar to the ```from``` for keyframes in css3,eg: ```AnimatedConfig(opacity: 0)``` |
| to |No| NULL | It's similar to the ```to``` for keyframes in css3 ,eg:```AnimatedConfig(opacity:1)```|
| configMap |NO| NULL |  Similar to keyframes in css3,eg:```{0:AnimatedConfig(opacity:0,translateX: 200),0.2:AnimatedConfig(opacity:1,translateX:100),1:AnimatedConfig(opacity:1,translateX:0)}``` |
| curve |NO| ```_Linear._()``` | Set the input/output animation curve |
| duration |NO| Duration(seconds: 1)  | Animation execution time |
| autoPlay |NO| false  | Whether to automatically animate |

#### method

| method  | Description |
| :------------ |:--------:|
| animate | Execute the appropriate animation |


## Example

``` dart
import 'package:flutter/material.dart';
import 'package:animated_interpolation/animated_interpolation.dart';
class AnimatedLogo1 extends AnimatedWidget {
  // The Tweens are static because they don't change.
  static final _opacityTween = new InterpolationTween<double>(inputRange: [0,0.2,1], outputRange: [0,0.5,1]);
  static final _sizeTween = new InterpolationTween(inputRange: [0,0.2,1], outputRange: [0,250,300]);

  AnimatedLogo1({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return new Center(
      child: new Opacity(
        opacity: _opacityTween.evaluate(animation),
        child: new Container(
          margin: new EdgeInsets.symmetric(vertical: 10.0),
          height: _sizeTween.evaluate(animation),
          width: _sizeTween.evaluate(animation),
          child: new FlutterLogo(),
        ),
      ),
    );
  }
}

class LogoApp4 extends StatefulWidget {
  _LogoAppState createState() => new _LogoAppState();
}

class _LogoAppState extends State<LogoApp4> with TickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = new AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = new CurvedAnimation(parent: controller, curve: Curves.easeIn);


    animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        controller.reverse();
      } else if (status == AnimationStatus.dismissed) {
        controller.forward();
      }
    });

    controller.forward();
  }

  Widget build(BuildContext context) {
    return new AnimatedLogo1(animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}
```
