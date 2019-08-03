![Austin Coding Academy](http://en.gravatar.com/userimage/107370100/a08594145564536138dfaaf072c7b241.png?size=200)

# React Native Workshop

## What is React Native

[React Native](https://facebook.github.io/react-native/) is a framework that
lets you use your current knowledge of building web apps with
[React.js](https://reactjs.org/) to build full-fledged native iOS and Android
apps that can be downloaded from their respective app stores. If you are comfortable
with React.js, you will feel right at home with React Native.

## What is Expo

React Native is cool, but there are A LOT of moving parts with mobile development.
Things like device simulators, debugging, building, debugging and publishing can
really keep you from even writing your first line of code.
[Expo]((https://expo.io/)) is a set of tools that streamlines all these things
for you in really simple CLI commands. It can be compared to a "Create React App"-esque
platform where as long as you stay inside their little box and don't poke around too
much, they will handle everything for free.

We'll need to:

1. Create an account on [Expo](https://expo.io/signup)
2. Download the [iOS/Android Client App](https://expo.io/tools#client)
3. Install the [Expo CLI](https://expo.io/tools#cli): `npm install expo-cli --global`
  * On MacOSX, you may need to install [Watchman](https://docs.expo.io/versions/v34.0.0/introduction/installation/#watchman)

## Devices and Device Simulators

The [iOS/Android Client App](https://expo.io/tools#client) takes care of most of
the work on loading on your phone.

To load Android simulators, you'll need to install
[Android Studio 3](https://developer.android.com/studio)

If you are on a Mac, you can load iPhone simulators by installing
[Xcode](https://apps.apple.com/us/app/xcode/id497799835)

## Let's create an App

Getting started is too easy. You can generate the boilerplate needed by running
`expo init react-native-workshop`. We will create a blank app so we don't end up
with too many generated files.

```bash
$ expo init react-native-workshop
? Choose a template: (Use arrow keys)
  ----- Managed workflow -----
❯ blank                 a minimal app as clean as an empty canvas
  blank (TypeScript)    same as blank but with TypeScript configuration
  tabs                  several example screens and tabs using react-navigation
  ----- Bare workflow -----
  minimal               bare and minimal, just the essentials to get you started
  minimal (TypeScript)  same as minimal but with TypeScript configuration
```

We can then enter a name for our app just by typing

```bash
? Please enter a few initial configuration values.
  Read more: https://docs.expo.io/versions/latest/workflow/configuration/ › 100% completed
 {
   "expo": {
     "name": "React Native Workshop",
     "slug": "react-native-workshop"
   }
 }
```

And let it install.

Then we can `cd react-native-app` and boot 'er up! `npm start`

It'll load a debugger page in your web browser. From there you can click on the
"Run on Android/iOS" buttons to start simulators, or you can connect with your
Expo Client phone apps as long as they are on the same network (most of the time)

## Navigation

Navigation can be difficult to set up on React Native Apps. Most libraries that
are out of the box only have a piece of the puzzle, and documentation is sparse
on how to wire them together. Here we will go through one way of setting that up.

### Three Parts of Navigation

Routing on phone apps usually has three features:

1. The actual links that bring you to another screen
   * [`react-router-native`](https://reacttraining.com/react-router/native/guides/quick-start)
2. A Side Bar that opens and closes with navigation links
   * [`react-native-side-menu`](https://github.com/react-native-community/react-native-side-menu)
3. A back button that let's you go back to the previous screens
   * [`react-router-navigation`](https://github.com/winoteam/react-router-navigation)

So let's get those installed

`npm install --save react-router-native react-native-side-menu react-router-navigation`

### Three Screens

Let's create three "screen" components so we can practice navigating between them

`components/Home.js`

```js
import React from 'react';
import { Text, View } from 'react-native';

export default () => {
  return (
    <View style={{ height: '100%', width: '100%', justifyContent: 'center', alignItems: 'center' }}>
      <Text>I am Home!</Text>
    </View>
  );
}
```

`components/PageOne.js`

```js
import React from 'react';
import { Text, View } from 'react-native';

export default () => {
  return (
    <View style={{ height: '100%', width: '100%', justifyContent: 'center', alignItems: 'center' }}>
      <Text>I am on Page One!</Text>
    </View>
  );
}

```

`components/PageTwo.js`

```js
import React from 'react';
import { Text, View } from 'react-native';

export default () => {
  return (
    <View style={{ height: '100%', width: '100%', justifyContent: 'center', alignItems: 'center' }}>
      <Text>I am on Page Two!</Text>
    </View>
  );
}
```

Now in our `App.js` we can wire them together:

```js
import React, { Component } from 'react';
import { Text, View } from 'react-native';
import { NativeRouter, Link } from "react-router-native";
import { Navigation, Card } from 'react-router-navigation'
import SideMenu from 'react-native-side-menu';
import { FontAwesome } from '@expo/vector-icons';
import PageOne from './components/PageOne';
import PageTwo from './components/PageTwo';
import Home from './components/Home';

export default class App extends Component {
  state = {
    isOpen: false
  }

  render = () => {
    return (
      <NativeRouter>
        <SideMenu
          menu={
            <View style={{ marginTop: 75 }}>
              <Link to="/" onPress={() => this.setState({ isOpen: false })}>
                <Text>Home</Text>
              </Link>
              <Link to="/PageOne" onPress={() => this.setState({ isOpen: false })}>
                <Text>PageOne</Text>
              </Link>
              <Link to="/PageTwo" onPress={() => this.setState({ isOpen: false })}>
                <Text>PageTwo</Text>
              </Link>
            </View>
          }
          isOpen={this.state.isOpen}
          onChange={isOpen => this.setState({ isOpen })}
          menuPosition="right"
        >
          <Navigation
            title='React Native'
            renderRightButton={() =>
              <FontAwesome
                name='bars'
                size={24}
                onPress={() => this.setState({ isOpen: !this.state.isOpen })}
              />
            }
          >
            <Card exact path="/" component={Home} title='Home' />
            <Card path="/PageOne" component={PageOne} title='Page One' />
            <Card path="/PageTwo" component={PageTwo} title='Page Two' />
          </Navigation>
        </SideMenu>
      </NativeRouter>
    );
  }
}
```

## React Native Component Frameworks

If you are familiar with CSS frameworks like Bootstrap or Material Design, you'll
appreciate that React Native also has some options so you are not left to write
all the styles for every button, etc. The most popular are:

* [NativeBase](https://nativebase.io/)
* [React Native Elements](https://react-native-training.github.io/react-native-elements/)

These provide out-of-the-box components to get you creating great looking apps quickly.

## Other Popular React Native Libraries

* [React Native Calendars](https://github.com/wix/react-native-calendars)
* [React Native Snap Carousel](https://github.com/archriss/react-native-snap-carousel)
* [React Native Gifted Chat](https://github.com/FaridSafi/react-native-gifted-chat)

## How to find other React Native Libraries

The best place to find React Native compatible libraries is [Native Directory](https://www.native.directory/)

## Frequently Asked Questions

* Can I use HTML tags?
  * No. The `<View>` and `<Text>` tags replace the `<div>` and `<span>` tags respectively. Everything has to be component tags.
* Will my favorite React.js libraries work?
  * Probably not. You'll have to check their documentation or Native Directory for equivalents
* What about Redux?
  * Yes! Redux works the same way!
* Better Devtools
  * You can get Redux and React devtools working with [React Native Debugger](https://github.com/jhen0409/react-native-debugger)
* I'm more of a [Vue](https://vuejs.org/) guy/girl myself. Am I out of luck?
  * There is [Vue Native](https://vue-native.io/)!
