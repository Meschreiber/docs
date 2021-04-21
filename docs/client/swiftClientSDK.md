---
title: iOS Swift SDK
---

Statsig client side SDK for iOS applications. This SDK is [open source and hosted on github](https://github.com/statsig-io/ios-sdk).

## Getting started {#getting-started}

Statsig is a singleton class which you can initialize with Statsig.start() function:

```swift
Statsig.start(sdkKey: "my_client_sdk_key", user: StatsigUser(userID: "my_user_id"))
```

You can also optionally use a completion block to wait for it to finish initializing:

```swift
Statsig.start(sdkKey: "my_client_sdk_key", user: StatsigUser(userID: "my_user_id")) { errorMessage in

  // Statsig client is ready;

  // You can also check errorMessage for any debugging information.

}
```

To check the value of a feature gate for the current user, use the checkGate() function. Note that if the gate_name provided does not exist,
or if the device is offline, we will return false as the default value.

```swift
let showNewDesign = Statsig.checkGate("show_new_design")
```

To retrieve a Dynamic Config for the current user, use the getConfig() function:

```swift
let localizationConfig = Statsig.getConfig("localization_config")
```

which will return a DynamicConfig object that you can then call getValue() on to retrieve specific values within the Dynamic Config. The
defaultValue will be returned when the user if offline or the key does not exist.

```swift
let buttonText = localizationConfig.getValue(forKey: "button_text", defaultValue: "Check out")
```

Sometimes the logged in user might switch to a different user, or you just received more information about the user and wish to update them,
you can call the updateUser() function to notify Statsig so it can retrieve the correct values for the updated user:

```swift
let newUser = StatsigUser(userID: "new_user_id", email: "newUser@gmail.com", country: "US")

Statsig.updateUser(newUser)
```

You can also use the same optional completion block to be notified when Statsig is done fetching values for the new user, just like in start().

## StatsigUser {#statsiguser}

The StatsigUser class is what we use to help you with targeting. You can provide _userID_, _email_, _ip_, _country_, and even _custom_, which
is a dictionary of String values for your own choices of targeting criteria. _userID_ is highly recommended, and we will try to use device ID
to identify the same user in the absence of a _userID_. You are also encouraged to provide as much _custom_ info as you know about the
user, all of which can be used by you in our console for feature gating and Dynamic Config's targeting.

## Logging custom events {#logging-custom-events}

The logEvent() API can be used to log custom events for your application, which will be shown in your Statsig dashboard and used for
metrics calculation for A/B testing:

```swift
Statsig.logEvent(withName: "purchase", value: 2.99, metadata: ["item_name": "remove_ads"])
```

## Shut down {#shut-down}

When your application is shutting down, call shutdown() so we can make sure any event logs are being sent for logging properly and all resources are released.

```swift
Statsig.shutdown()
```
