# Data Collection

## Passing Data on Page Load

Lightning Tag will automatically execute any data collection rules that you have configured in cooperation with your Lotame account representative. In addition, you can explicitly pass in data attributed to the user by supplying it in the data object of your tag input.

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

[comment]: # (Markdown tables are not fun especially trying to embed code in them)

Name | Description | Example
---- | ----------- | -------
behaviorIds	| Existing behavior ID's from your DMP account that are owned by the same client supplied in the tag |data: { <br/>&nbsp;&nbsp;behaviorIds: [1,2,3], <br/> },
behaviors | New or existing behaviors as type/value pairs, where the type is a supported type in the DMP, respectively <br/> 'int' = interest <br/> 'med' = media <br/> 'act' = action  <br/> 'seg' = custom segment |data: { <br/> &nbsp;&nbsp;behaviors: { <br/> &nbsp;&nbsp;&nbsp;&nbsp;int: ['site section: news', 'traffic: mysite.com'], <br/> &nbsp;&nbsp;&nbsp;&nbsp;med: ['article category : politics'] <br/> &nbsp;&nbsp;},<br/> },
ruleBuilder	| Custom keys and values to be used for the Rule Builder tool within the DMP | data: {<br/>&nbsp;&nbsp;ruleBuilder: {<br/>&nbsp;&nbsp;&nbsp;&nbsp;article_tags: ['food', 'in the news'],<br/>&nbsp;&nbsp;&nbsp;&nbsp;article_title: ['Todays Headline'],<br/>&nbsp;&nbsp;&nbsp;&nbsp;article_author: ['Bob Roberts'],<br/>&nbsp;&nbsp;},<br/>},
thirdParty | An identifier to associate with the current browser, typically to enable server side data transfer. Your Lotame representative will provide the namespace value as necessary for your implementation. | data: {<br/>&nbsp;&nbsp;thirdParty: {<br/>&nbsp;&nbsp;&nbsp;&nbsp;namespace: 'FAKE',<br/>&nbsp;&nbsp;&nbsp;&nbsp;value: '123456789101112131415'<br/>&nbsp;&nbsp;},<br/>},
sha256email	| The current users email address, first lower-cased, trimmed of whitespace, then hashed using SHA256	|data: {<br/>&nbsp;&nbsp;sha256email: 'lowercase_no_whitespace_sha256_hashed_email'<br/>},

## Passing Data outside of Page Load

### Page Interactions

You can use the Lightning Tag `collect()` method to send data from custom events that cannot be handled through standard Lotame data collection rules at any point after the Lightning Tag has loaded.

This option takes the same `{data}` object as describe at the top of the page.

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

### New Page Load Events

Lightning Tag provides a `page()` method that you can pass data on just like the `collect()` method described above. Unlike the `collect()` method, the `page()` method records a pageView in Lotame and initiates targeting again which will update audiences in your audience Callback or LocalStorage. An example use-case would be an single page app where the URL remains the same, no page load event occurs, but the main content of the window is replaced. `page()` allows your site to note the new pageView and pass data in that corresponds to the new page so targeting can be rerun based on this new information.

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
