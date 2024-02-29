# wiki-swift-xcuitest
Repository for iOS mobile testing technical reference using XCUITest

## XCTest vs XCUITest

### XCTest

XCTest is the unit testing framework by Apple.

#### Advantages
- XCTest is built into XCode, the default IDE (Integrated Development Environment) for Mac systems. So, you don't need to do complicated setups that other frameworks, like Appium, require.
- We can also write XCTest on an iPad because XCode is now available on iPad and isn't dependent on Mac.
- XCTest is the fastest testing framework for iOS apps. The time to run the test on the iOS simulator is the fastest compared to doing it with other frameworks.
- The developers creating these test cases are quite familiar with these languages because they've created the app in the same language.

#### Disadvantages
- XCTest can't be used to test cross-mobile apps written in React Native. 
- XCTest is focused on unit testing of iOS apps.

### XCUITest

Apple's default automated testing framework - based on XCTest.

#### Advantages
- XCUITest is built into XCode, just like XCTest. So, you don't need to do complicated setups that similar automation mobile testing frameworks, like Appium, require.
- Because the setup in Appium is so complicated, we even have an additional tool, Appium doctor, to check if all setup is done properly.
- XCUITest is the fastest automation testing framework for iOS apps.
- XCode can easily record interactions with the simulator.

#### Disadvantages
- XCUITest can't be used to test cross-mobile apps written in React Native.
- (Questionable) XCUITest doesn't run very well on real connected devices.

## Finding Elements

```java
app.alerts.element
app.buttons.element
app.collectionViews.element
app.images.element
app.maps.element
app.navigationBars.element
app.pickers.element
app.progressIndicators.element
app.scrollViews.element
app.segmentedControls.element
app.staticTexts.element
app.switches.element
app.tabBars.element
app.tables.element
app.textFields.element
app.textViews.element
app.webViews.element
```

**Note:** the **staticTexts** refers to labels on iOS.

It is recommended to use accessibilityIdentifier in elements to ease test automation:

```java
helpLabel.accessibilityIdentifier = "helpLabelName"
```

Then, you can simply search by its **staticTexts**:

```java
app.staticTexts["helpLabelName"]
```

### Advanced Queries

While the commands above are the main ways to find elements in application interface, there are other advanced queries to find specific elements: 

```java
// the only button
app.buttons.element

// the button titled "Help"
app.buttons["Help"]

// all buttons inside a specific scroll view (direct subviews only)
app.scrollViews["Main"].children(matching: .button)

// all buttons anywhere inside a specific scroll view (subviews, sub-subviews, sub-sub-subviews, etc)
app.scrollViews["Main"].descendants(matching: .button)    

// find the first and fourth buttons
app.buttons.element(boundBy: 0)
app.buttons.element(boundBy: 3)

// find the first button
app.buttons.firstMatch
```

Here you can find some of the use cases why the advanced queries are preferred: 

- When using **element** tests can fail in case there are multiple objects found
- **firstMatch** will always return the first item matched despite more elements found
- **children(matching:)**: find only direct subviews (more efficient)
- **descendant(matching:)**: find subviews and subviews of subviews

## Interacting with Elements

```java
tap()                                   // standard tap for buttons / active text fields
doubleTap()                             // as name suggests
twoFingerTap()                          // two fingers tap on element
tap(withNumberOfTaps:numberOfTouches)   // tap and touch count
press(forDuration:)                     // long press over seconds duration
swipeLeft()
swipeRight()
swipeUp()
swipeDown()
pinch(withScale:velocity:)              // pinches and zoom (good for maps) 
rotate(_: withVelocity)                 // rotates around element 
```

### Typing Text

```java
app.textFields.element.tap()
app.keys["t"].tap()
app.keys["e"].tap()
app.keys["s"].tap()
app.keys["t"].tap()
```
or: 

```java
app.textFields.element.typeText("test") 
```

## Assertions

```java
XCTAssertEqual(app.buttons.element.title, "Buy")
XCTAssertEqual(app.staticTexts.element.label, "test")
```
Checking if element **exists**:

```java
XCTAssertTrue(app.alerts["Warning"].exists)
```
It is recommended to wait a little while for real devices:

```java
XCTAssertTrue(app.alerts["Warning"].waitForExistence(timeout: 1))
```

## Attachments

```java
let screenshot = app.screenshot()
let attachment = XCTAttachment(screenshot: screenshot)
attachment.name = "Beautiful Screenshot"
attachment.lifetime = .keepAlways
add(attachment)
```
