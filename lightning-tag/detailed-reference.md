# Detailed Reference Guide

!> Under Construction.

## Platform Setup

Before implementing the Lotame Lightning Tag on your site, there is setup that needs to be completed inside the Lotame Platform as described in the [Pre-Implementation Setup Tasks](lightning-tag/implementation-setup-tasks.md)

## Lightning Tag Setup

Setup of the Lotame Lightning Tag javascript on your website involves an input object that gets passed to an initialization function that prepares a window namespace on your page for the javascript tag itself. The basic setup looks like the below:

```javascript
<script>
     var lotameTagInput = {
         data: {},
         config: {
             clientId: CLIENT_ID
         }
     };

     ! function(input) {
         input = input || {};
         var config = input.config || {};
         namespace = window['lotame_' + config.clientId] = {};
         namespace.config = config;
         namespace.data = input.data || {};
     } (lotameTagInput);
 </script>
 ```

The details of the input `data` and `config` options are below.

### Config Object

The `config` object in the Lotame Lightning Tag input has the following possible parameters:

Parameter Name | Description | Type | Required? | Default
-------------- | ----------- | ---- | :-------: | :-----:
clientId | Your Lotame Client ID  | `int` | YES | N/A
onProfileReady | Callback function called once Lotame targeting returns. Discussed further below in the Callback section of this page |`function (profile)` | NO | `{}`
audienceLocalStorage | Determines whether to write audiences to the browser's `localStorage` and what the name of the key is. Discussed further below in the Local Storage section | `boolean` _**-OR-**_ `string` | NO | `false`
onTagReady | Callback function called once Lotame Lightning Tag object is loaded and ready to be used. | `function (namespace)` | NO | `{}`
autoRun | Determines whether Lotame Lightning Tag runs on load. If set to false, your website needs to explicitly call the `page()` method listed on this page to send data and retrieve targeting audiences. | `boolean` | NO | `true`

#### Audience Callback

As noted above in the `config` object created at script setup, there is a callback method named `onProfileReady` that is called after targeting completes. This work happens asynchronously, so the callback is fired when the targeting work is completed.

When fired, the callback is passed a `profile` object that has the following functionality.

Method Name | Return Type | Parameters | Description
----------- | ----------- | ---------- | -----------
`getProfileId()` | String | n/a | The id of the profile associated with the browser
`getAudienceString( [delimiter=','], [limit])` | String | 1. `delimiter` - optional, string, defaults to comma. <br/> 2. `limit` - optional, integer, defaults to no limit | Delimited string of targeting codes or audience ids, based on your established tag settings. The default, i.e. no arguments, is comma delimited with no limit. Example: `"aud1,aud2"` or `"1,2"`
`getAudiences([limit])` | Array | `limit` - optional, integer, defaults to no limit | Array of either targeting codes as strings or audience ids as integers, based on your established tag settings. Example: `["aud1","aud2"]` or `[123,789]`

#### Local Storage Usage

As noted above in the `config` object created at script setup, there is a parameter named `audienceLocalStorage`. If passed, when targeting is completed, the audiences are written out to the browser's localStorage.

Value | Result
:---: | ------
`false` | This is the default, and audiences are not written to browser localStorage.
`true` |  Lightning Tag writes audiences to localStorage under the key `lotame_CLIENT_ID_auds` (where CLIENT_ID is your Lotame clientId)
`'string'` | Passing a string tells Lightning Tag to write the audiences to localStorage under the key of the string you pass. (i.e. `audienceLocalStorage: 'myLotameAudiences'`)

Audiences are written out as a string of comma-delimited targeting codes or audience IDs, based on your established tag settings. Example: `"aud1,aud2"` or `"123,789"`

#### Tag Ready Callback

The `onTagReady` callback fires when the Lotame Lightning Tag interface is ready to be used. Which includes below described methods such as `collect` and `page`.

```javascript
onTagReady: function(namespace) {
  // Lightning Tag script is ready to be used
},
```

?> This event does not confirm audiences are available, that is the `onProfileReady` callback described above in this document.

#### Auto Run Flag

The `autoRun` boolean is defaulted `true` and runs the Lotame data collection and targeting retrieval is run immediately after the script is loaded. By default it is `true` so those functions run automatically after the script is loaded. If set to `false` control is left to the client's website to fire data collection and targeting using the `collect()` and `page()` methods described lower on this document.

A use-case for setting this to `false` is if you want to get the customer's consent before enabling the data collection and targeting to run.

### Data Object

The `data` object is used to pass first party-data to the Lotame DMP for use in targeting or other features of the Lotame platform. The object is optionally passed at setup time in the tag input function. It can also be passed post-page load in the `collect({data})` or in the `page({data})` methods that is described further down this page.

The `data` object's parameters are fully described in [Lightning Tag Data Collection](lightning-tag/data-collection.md).

## Lightning Tag Methods

!> To ensure any of the below methods are available, ensure the `onTagReady` callback has returned else the below methods are not available yet.

### collect()

The `collect()` method is available to pass data to your Lotame DMP the Lotame javascript is loaded. An example use-case is tracking events such as user interaction with a video player on your site.

```javascript
window.lotame_<lotameClientId>.collect({data});
```

The `data` object parameters are described in the [Data Collection page](lightning-tag/data-collection.md).

Calls to this method are queued up. Every 1 second (and also at the `page.unload()` event) the Lightning Tag flushes the queue and sends the batch of data objects to the DMP. This functionality allows your website to make calls to this method and not need to wait on a response.

### page()

The `page()` method allows you to re-run your targeting call without reloading the Lotame Lightning Tag script. A use-case is a single-page site which as a user scrolls, new articles pop up. Your site can now pass in new `{data}` elements that describe this new article the user is now viewing and then retrieve updated audiences based on that new information.

```javascript
window.lotame_<lotameClientId>.page({data});
```

?> Please note that `{data}` is an optional parameter to the `page()` method.

The following tasks occur when `page()` is called:

1. The data queue described above in the `collect()` method flushes, including any new data passed on this `page()` call.
1. Your Optimus rules execute, pulling in new values based on the page contents.
1. A new page view registers in your Lotame DMP.
1. Targeting executes, and audiences return on either the callback method or localStorage (or both) depending on how you configured audience extraction on your site.

### getAudiences()

`getAudiences()` is a helper method. It is a shortcut that retrieves the targeted audience list from `localStorage`.

```javascript
window.lotame_<lotameClientId>.getAudiences();
```

Audiences are written out as a string of comma-delimited targeting codes or audience IDs, based on your established tag settings. Example: `"aud1,aud2"` or `"123,789"`

?> The `audienceLocalStorage` parameter must be set in the Lotame Lightning Tag `config` object. If it is not, the response to `getAudiences` will be an empty string and an error message written to `console` log.

### setConsent()

The Lotame Ligntning Tag provides a method `setConsent` method to submit consent signals to the Lotame Consent API for enforcement within the Lotame DMP. Once you've collected the appropriate consent from your customer within your privacy user experience, submit the appropriate signals as follows:

```javascript
function consentCb(returnData) {
  // Now that consent is loaded in your Lotame DMP, run data collection and targeting 
  var dataCollection = {
    // data fields here
  };
  window.lotame_<lotameClientId>.page(dataCollection);
}

var customerConsents = {
  analytics: true,
  crossdevice: true,
  datasharing: true,
  targeting: true
};

window.lotame_<lotameClientId>.setConsent(consentCb, <lotameClientId>, customerConsents);
```

The `setConsent` method has 4 possible return values in the object passed to the callback. 

Return State | Return Object Format | Description
------------ | -------------------- | -----------
Success |{<br/>&nbsp;&nbsp;"consent": [{<br/>&nbsp;&nbsp;&nbsp;&nbsp;"clientid":CONSENT_CLIENT_ID,<br/>&nbsp;&nbsp;&nbsp;&nbsp;"lastupdate": UTC_SECONDS,<br/>&nbsp;&nbsp;&nbsp;&nbsp;"types": [{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "analytics",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}, {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "targeting",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}, {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "datasharing",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}, {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "crossdevice",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}]<br/>&nbsp;&nbsp;}]<br/>} | The Lotame DMP recorded the consent signals as indicated and will enforce them going forward. The Lotame Consent API will only store and return here signals that you’ve explicitly provided via a set call (i.e. the types array could be empty).
Browser opted out of Lotame cookies | {"error": 200} | The Lotame DMP is unable to store or enforce consent.
Could not write cookie | {"error": 201} | The Lotame DMP is unable to store or enforce consent.
Consent Collection not enabled | {"error": 202} | The client id is not configured to store or enforce consent.

### getConsent()

`getConsent` is almost exactly the same as `setConsent` with the difference being, you do not pass in any consents to set. The return object is in the same format and passed to the callback just like `setConsent`.

```javascript
function consentCb(returnData) {
  // Now that consent is loaded in your Lotame DMP, run data collection and targeting 
  var dataCollection = {
    // data fields here
  };
  window.lotame_<lotameClientId>.page(dataCollection);
}

window.lotame_<lotameClientId>.setConsent(consentCb, <lotameClientId>);
```
