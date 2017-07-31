# react-native-fabric-twitterkit
React Native Fabric Twitter-kit support for iOS and Android

Use [react-native-fabric](https://github.com/corymsmith/react-native-fabric) for Answers and Crashlytics

# Installation:

```
npm install react-native-fabric-twitterkit --save
rnpm link react-native-fabric-twitterkit
```

## iOS

Follow the official Fabric iOS instructions on [Fabric.io](https://docs.fabric.io/apple/twitter/installation.html)

## Android

Follow "Set Up Kit" from official Fabric Android docs at [Fabric.io](https://docs.fabric.io/android/twitter/compose-tweets.html)

Navigate to your `MainActivity.java` somewhere in `MyApp/android/app/src/main/java/...../MainActivity.java`

```diff
+ import com.tkporter.fabrictwitterkit.FabricTwitterKitPackage;

...

public class MainActivity extends ReactActivity {

	.....

	@Override
	public void onActivityResult(int requestCode, int resultCode, Intent data) {
+ 		FabricTwitterKitPackage.getInstance().onActivityResult(this, requestCode, resultCode, data);
	}

	...

}
```

Go to your `MyAppApplication.java` inside the same folder as `MainActivity.java`

```diff
+ import com.tkporter.fabrictwitterkit.FabricTwitterKitPackage;

...

public final MyApp extends ....... {

	...

	@Override List<ReactPackage> getPackages() {
		return Arrays.<ReactPackage>asList(
			...
+			FabricTwitterKitPackage.getInstance(),
			...
		);
	}

	...

}
```

# Usage

This package has iOS and Android functionality, so you can use the same call for each platform.

There are lots of functions, and not a lot of README writing time. Check out `FabricTwitterKit/FabricTwitterKit.m` and `Android/src/main/java/com/tkporter/fabrictwitterkit/FabricTwitterKitModule.java` for the other supported functions! :)

ComposeTweet example:

```JavaScript
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  Button,
  View
} from 'react-native';
import FabricTwitterKit from 'react-native-fabric-twitterkit';

export default class TwitterExample extends Component {

  _composeTweet() {
  	FabricTwitterKit.composeTweet({
  		text: 'react-native-fabric-twitterkit is awesome!'
  		// link: 'https://www.twitter.com',
  		// image: 'https://g.twimg.com/dev/img/marketing/twitter-for-websites/header-logo.png'
  	}, (data, cancelled, error) => {
  		console.log('data: ' , data);
  		console.log('cancelled: ' , cancelled);
  		console.log('error: ' , error);
  	});

  }
  _loginViaTweet(){
    FabricTwitterKit.login((data, user, error) => {
      console.log('data: ' , data);
      console.log('user: ' , user);
      console.log('error: ' , error);
  	})
  }
  _fetchTweet(){
    FabricTwitterKit.fetchTweet({
  		id: '',
  		trim_user: 'true',
  		include_my_retweet: ''
  	}, (tweetData, err) => {
  		console.log('tweetData: ' , tweetData);
  		console.log('err: ' , err);
  	});
  }
  _Api(){
    FabricTwitterKit.Api({
  		endpoint: 'search/tweets.json',
  		q: encodeURI('#Trump')
      // endpoint: 'friends/list.json',
      // count: 12,
      // 'endpoint': 'followers/list.json',
      // screen_name:'realdonaldtrump'
  	}, (tweetData, err) => {
  		console.log('tweetData: ' , tweetData);
  		console.log('err: ' , err);
  	});
  }
  _fetchProfile(){
    FabricTwitterKit.fetchProfile((data, cancelled, error) => {
      console.log('data: ' , data);
      console.log('cancelled: ' , cancelled);
      console.log('error: ' , error);
  	});
  }
  _logOut(){
    FabricTwitterKit.logOut()
  }

  render() {
    return (
      <View style={styles.container}>
          <Button title="Compose Tweet" style={styles.instructions} onPress={this._composeTweet}></Button>
          <Button title="Login via Tweet" style={styles.instructions} onPress={this._loginViaTweet}></Button>
          <Button title="Fetch Tweet" style={styles.instructions} onPress={this._fetchTweet}></Button>
          <Button title="Fetch Profile" style={styles.instructions} onPress={this._fetchProfile}></Button>
          <Button title="Fetch Api" style={styles.instructions} onPress={this._Api}></Button>
          <Button title=" logOut of Twitter" style={styles.instructions} onPress={this._logOut}></Button>
        </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
        flex: 1,
    // alignSelf: 'center',
    // color: '#333333',
    // marginBottom: 5,
  },
});

AppRegistry.registerComponent('TwitterExample', () => TwitterExample);


```
