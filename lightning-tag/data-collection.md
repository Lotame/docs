# Data Collection Guide

## Passing Data on Tag Load

Lightning Tag will automatically execute any data collection rules that you have configured in cooperation with your Lotame account representative. In addition, you can explicitly pass in data attributed to the user by supplying it in the `data` object of your tag input.

```javascript
// Lotame Config
var lotameTagInput = {
  data: {
    behaviorIds: [CSV_OF_BEHAVIOR_IDS],
    behaviors: {
      int: ['behaviorName', 'behaviorName2'],
      act: ['behaviorName']
    },
    ruleBuilder: {
      key1: ['value 1a', 'value 1b']
    },
    thirdParty: {
      namespace: 'NAMESPACE',
      value: 'TPID_VALUE'
    },
    sha256email: 'SHA256_VALUE'
  },
  config: {
    clientId: <lotameClientId>,
    audienceLocalStorage: true, // written to 'lotame_<lotameClientId>_auds' key
    onProfileReady: audienceReadyCallback
  }
};

// Lotame initialization
! function(input) {
  input = input || {};
  var config = input.config || {};
  var namespace = window['lotame_' + config.clientId] = {};
  namespace.config = config;
  namespace.data = input.data || {};
} (lotameTagInput);
```

The `data` object's parameters are fully described in [Lightning Tag Data Collection](lightning-tag/detailed-reference?id=data-object).

## Passing Data outside of Tag Load

You can use the Lightning Tag `collect()` method to send data from custom events that cannot be handled through standard Lotame data collection rules at any point after the Lightning Tag has loaded.

```javascript
window.lotame_<lotameClientId>.collect({
  behaviorIds: [CSV_OF_BEHAVIOR_IDS],
  behaviors: {
    int: ['behaviorName', 'behaviorName2'],
    act: ['behaviorName']
  },
  ruleBuilder: {
    key1: ['value 1a', 'value 1b']
  },
  thirdParty: {
    namespace: 'NAMESPACE',
    value: 'TPID_VALUE'
  },
  sha256email: 'SHA256_VALUE'
});
```

This option takes the same `{data}` object as described in [Lightning Tag Data Collection](lightning-tag/detailed-reference?id=data-object).

## Triggering New Page Load Events

Lightning Tag provides a `page()` method that you can pass data in on just like the `collect()` method described above. Unlike the `collect()` method, the `page()` method records a page view in Lotame and initiates targeting again which will update audiences in your audience callback or local storage as well as fire any additional sync pixels, export beacons, or automated data collection rules for the page. 

An example use-case is a single page app where the URL remains the same, no new page load events occur, but the main content of the window is replaced. `page()` allows your site to note the new pageView and pass data in that corresponds to the new page so targeting can be rerun based on this new information.

```javascript
window.lotame_<lotameClientId>.page({
  behaviorIds: [CSV_OF_BEHAVIOR_IDS],
  behaviors: {
    int: ['behaviorName', 'behaviorName2'],
    act: ['behaviorName']
  },
  ruleBuilder: {
    key1: ['value 1a', 'value 1b']
  },
  thirdParty: {
    namespace: 'NAMESPACE',
    value: 'TPID_VALUE'
  },
  sha256email: 'SHA256_VALUE'
});
```
This option takes the same `{data}` object as described in [Lightning Tag Data Collection](lightning-tag/detailed-reference?id=data-object).
