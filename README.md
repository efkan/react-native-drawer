## React Native Drawer
<img width="220px" align="right" src="https://raw.githubusercontent.com/rt2zz/react-native-drawer/master/examples/rn-drawer.gif" />

**2.0** is in progress if if you are in the early stages of your project it is probably worth switching now. To access 2.0 run `npm install react-native-drawer@next`.

React native drawer, configurable to achieve material design style, slack style, parallax, and more. Works in both iOS and Android.

[![npm version](https://img.shields.io/npm/v/react-native-drawer.svg?style=flat-square)](https://www.npmjs.com/package/react-native-drawer)
[![npm downloads](https://img.shields.io/npm/dm/react-native-drawer.svg?style=flat-square)](https://www.npmjs.com/package/react-native-drawer)

**Android**: Android support has been added in v1.3.0!

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
- [Props](#props)
- [Demo](#demo)
- [Credits](#credits)

### Installation
`npm install react-native-drawer`

### Usage
```javascript
import Drawer from 'react-native-drawer'

class Application extends Component {  
  closeControlPanel = () => {
    this.refs.drawer.close()
  };
  openControlPanel = () => {
    this.refs.drawer.open()
  };
  render () {
    return (
      <Drawer
        ref="drawer"
        content={<ControlPanel />}
        >
        <MainView />
      </Drawer>
    )
  }
})
```

### Examples
```js
//Parallax Effect (slack style)
<Drawer
  type="static"
  content={<ControlPanel />}
  openDrawerOffset={100}
  styles={{main: {shadowColor: "#000000", shadowOpacity: 0.4, shadowRadius: 3}}}
  tweenHandler={Drawer.tweenPresets.parallax}
  >
    <Main />
</Drawer>

//Material Design Style Drawer
<Drawer
  type="overlay"
  content={<ControlPanel />}
  tapToClose={true}
  openDrawerOffset={0.2} // 20% gap on the right side of drawer
  panCloseMask={0.2}
  closedDrawerOffset={-3}
  styles={{
    drawer: {shadowColor: '#000000', shadowOpacity: 0.8, shadowRadius: 3},
    main: {paddingLeft: 3}
  }}
  tweenHandler={(ratio) => ({
    main: { opacity:(2-ratio)/2 }
  })}
  >
    <Main />
</Drawer>
```

Also a [react-native-router-flux](https://github.com/aksonov/react-native-router-flux) implemented example is  **[here](https://github.com/efkan/rndrawer-implemented-rnrouter)**.

### Props
This module supports a wide range of drawer styles, and hence has *a lot* of props.
##### Important
- `content` (React.Component) `null` - Menu component
- `type` (String: displace:overlay:static) `displace`- Type of drawer.
- `openDrawerOffset` (Number) `0` - Can either be a integer (pixel value) or decimal (ratio of screen width). Defines the right hand margin when the drawer is open.
- `closedDrawerOffset` (Number) `0` - Same as openDrawerOffset, except defines left hand margin when drawer is closed.
- `disabled` (Boolean) `false` - If true the drawer can not be opened and will not respond to pans.

##### Animation / Tween
- `tweenHandler` (Function) `null` - Takes in the pan ratio (decimal 0 to 1) that represents the tween percent. Returns and object of native props to be set on the constituent views { drawer: {/*native props*/}, main: {/*native props*/} }
- `tweenDuration` (Integer) `250` - The duration of the open/close animation.

##### Event Handlers
- `onOpen` (Function) - Will be called immediately after the drawer has entered the open state.
- `onClose` (Function) - Will be called immediately after the drawer has entered the closed state.

##### Gestures
- `captureGestures` (oneOf(true, false, 'open', 'closed')) `open` - If true, will capture all gestures inside of the pan mask. If 'open' will only capture when drawer is open.
- `acceptDoubleTap` (Boolean) `false` - Toggle drawer when double tap occurs within pan mask?
- `acceptTap` (Boolean) `false` - Toggle drawer when any tap occurs within pan mask?
- `acceptPan` (Boolean) `true` - Allow for drawer pan (on touch drag). Set to false to effectively disable the drawer while still allowing programmatic control.
- `tapToClose` (Boolean) `false` - Same as acceptTap, except only for close.
- `negotiatePan` (Boolean) `false` - If true, attempts to handle only horizontal swipes, making it play well with a child `ScrollView`.

##### Additional Configurations
- `panThreshold` (Number) `.25` - Ratio of screen width that must be travelled to trigger a drawer open/close.
- `panOpenMask` (Number) `null` - Ratio of screen width that is valid for the start of a pan open action. If null -> defaults to `max(.05, closedDrawerOffset)`.
- `panCloseMask` (Number) `null` - Ratio of screen width that is valid for the start of a pan close action. If null -> defaults to `max(.05, openDrawerOffset)`.
- `initializeOpen` (Boolean) `false` - Initialize with drawer open?
- `side` (String left|right) `left` - which side the drawer should be on.

##### Experimental & Deprecated Props
**Subject to change or deletion**
- `onOpenStart` (Function) callback fired at the start of an open animation
- `onCloseStart` (Function) callback fired at the start of a close animation
- `tweenEasing` (String) `linear` - A easing type supported by [tween-functions](https://www.npmjs.com/package/tween-functions)

### Tween Handler
You can achieve pretty much any animation you want using the tween handler with the transformMatrix property. E.G.
```js
tweenHandler={(ratio) => {
  var r0 = -ratio/6
  var r1 = 1-ratio/6
  var t = [
             r1,  r0,  0,  0,
             -r0, r1,  0,  0,
             0,   0,   1,  0,
             0,   0,   0,  1,
          ]
  return {
    main: {
      style: {
        transformMatrix: t,
        opacity: 1 - ratio/2,
      },
    }
  }
}}
```
Will result in a skewed fade out animation.

**warning:** Frame rate, and perceived smoothness will vary based on the complexity and speed of the animation. It will likely require some tweaking and compromise to get the animation just right.

### Opening & Closing the Drawer Programmatically
Two options:

1. Using the Drawer Ref:

    ```js
    onPress={() => {this.drawer.open()}}
    ```

2. Using Context

    ```js
    contextTypes = {drawer: React.PropTypes.object}
    // later...
   this.context.drawer.open()
   ```

### Demo
* `git clone https://github.com/rt2zz/react-native-drawer.git`
* `cd react-native-drawer/examples/RNDrawerDemo && npm install`
* **iOS**
	* Open ``./examples/RNDrawerDemo/RNDrawerDemo.xcodeproject` in xcode
	* `command+r` (in xcode)
* **Android**
	* Run android simulator / plug in your android device
	* Run `react-native run-android` in terminal

### Credits
Component was adapted from and inspired by
[@khanghoang](https://github.com/khanghoang)'s [RNSideMenu](https://github.com/khanghoang/RNSideMenu)
*AND*
[@kureevalexey](https://twitter.com/kureevalexey)'s [react-native-side-menu](https://github.com/Kureev/react-native-side-menu)
