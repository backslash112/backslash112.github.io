---
layout: post
title: "How to Structure a React Native Project"
date: 2016-01-04 14:32:48
description: Run and modified your first React Native app
img: react-native.png
tags: [ios]
---

### iOS Setup

1. Install Node.js (4.0 or newer): <br />
```
nvm install node && nvm alias default node
```

2. Initialize Project:<br />
Navigate to the folder where you would like to develop your React Native application:<br />
```
react-native init YourProjectName
```

3. Run the app:<br />
	```
	cd YourProjectName
	```
	<br />
	Open ```YourProjectName.xcodeproj``` and hit run in Xcode.


4. Make some change:<br />
 Open ```index.ios.js``` in your text editor of choice and edit some lines.<br />
 Hit ⌘+R in your iOS simulator to reload the app and see your change!

Congratulations! You've successfully run and modified your first React Native app.
![Welcome to React Native]({{ site.url }}/assets/welcomeToReactNative.png){:width="340px"}

[react-native-getting-started]: http://facebook.github.io/react-native/docs/getting-started.html#content
[jekyll]:      http://jekyllrb.com
