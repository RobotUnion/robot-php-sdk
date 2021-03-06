RobotUnion PHP SDK (experimental)
==================

Toolset to create Robot Apps for [R.A.L.F.](https://ralf.robotunion.net)
based on [Appium php client](https://github.com/appium/php-client) 



# First steps
## Installation
  * Require `RobotUnion/robot-php-sdk` into `composer.json`
  
    ```
    {
      "name": "myrobot/my-task",
      "repositories": [
        {
          "type": "vcs",
          "url": "https://github.com/RobotUnion/robot-php-sdk"
        }
      ],
      "require": {
        "RobotUnion/robot-php-sdk": "^1.0"
      },
      "autoload": {
        "psr-4": {
          "": "src/"
        }
      },
      "autoload-dev": {
        "psr-4": {
          "Tests\\": "tests/"
        }
      }
    }
    ```
    
## Create simple app
  * Create the robotapp manifest `manifest.json`
  
    ```
    {
        "name": "My first robot task",
        "description": "here a brief description",
        "main": "MyRobots\\MyFirstTask"
    }
    ```
    
  * Extend from `RobotUnion/Integration/Task`
  
    ```
    <?php
    // File: my-php-project/src/MyRobots/MyFirstTask.php
    
    namespace MyRobots;
    
    use RobotUnion/Integration/Task;
    
    class MyFirstTask extends Task {
        
        //log START
        $this->logger->debug("START");
        
        //open android settings
        $screen = $this->device->startActivity([
            'appPackage' => 'com.android.settings',
            'appActivity' => '.Settings'
        ]);
        
        //take and log the screenshot
        $screen = $this->device->currentScreenshot();
        $this->logger->debug(base64_encode($screen));
        
        //log END
        $this->logger->debug("END");
    }
    ```
    
## Run Debug
    composer exec rr
    
## Package `out/app.tsk`
    composer exec rpkg

# API Documentation (selenium/appium based)
## Methods added

### Methods in `PHPUnit_Extensions_AppiumTestCase`

* `byIOSUIAutomation`
* `byAndroidUIAutomator`
* `byAccessibilityId`
* `keyEvent`
* `pullFile`
* `pushFile`
* `backgroundApp`
* `isAppInstalled`
* `installApp`
* `removeApp`
* `launchApp`
* `closeApp`
* `endTestCoverage`
* `lock`
* `shake`
* `getDeviceTime`
* `hideKeyboard`
* `initiateTouchAction`
* `initiateMultiAction`
* `scroll`
* `dragAndDrop`
* `swipe`
* `tap`
* `pinch`
* `zoom`
* `startActivity`
* `getSettings`
* `setSettings`

### Methods in `PHPUnit_Extensions_AppiumTestCase_Element`

* `byIOSUIAutomation`
* `byAndroidUIAutomator`
* `byAccessibilityId`
* `setImmediateValue`

### Methods for Touch Actions and Multi Gesture Touch Actions

Appium 1.0 allows for much more complex ways of interacting with your app through Touch Actions and Multi Gesture Touch Actions. The `PHPUnit_Extensions_AppiumTestCase_TouchAction` class allows for the following events:

* `tap`
* `press`
* `longPress`
* `moveTo`
* `wait`
* `release`

All of these except `tap` and `release` can be chained together to create arbitrarily complex actions. Instances of the `PHPUnit_Extensions_AppiumTestCase_TouchAction` class are obtained through the Test Class's `initiateTouchAction` method, and dispatched through the `perform` method.

The Multi Gesture Touch Action API allows for adding an arbitrary number of Touch Actions to be run in parallel on the device. Individual actions created as above are added to the multi action object (an instance of `PHPUnit_Extensions_AppiumTestCase_MultiAction` obtained from the Test Class's `initiateMultiAction` method) through the `add` method, and the whole thing is dispatched using `perform`.