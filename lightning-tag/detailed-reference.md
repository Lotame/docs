# Detailed Reference Guide

?> Coming Soon

## Platform Setup

Before implementing the Lotame Lightning Tag on your site, there is setup needed to be completed inside the Lotame Platform as described in the [Pre-Implementation Setup Tasks](lightning-tag/implementation-setup-tasks.md)

## Javascript Setup

Setup of the Lotame Lightning Tag javascript on your website involves Passing a input object to a function which loads the window namespace on your page which is where the javascript tag loads itself into an executes. The basic setup looks like the below:

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

### Config object

The `config` object in the Lotame Lightning Tag input has the below possible parameters:

Parameter Name | Description | Type | Required? | Default
-------------- | ----------- | ---- | :-------: | :-----:
clientId | Your Lotame Client ID  | `int` | YES | N/A
onProfileReady | Callback function called once Lotame targeting returns. Discussed further below in the Callback section of this page | `function(profile)` | NO | `{}`
audienceLocalStorage | Determines whether to write audiences to the browser's `localStorage` and what the name of the key is. Discussed further below in the Local Storage section | `boolean` _**-OR-**_ `string` | NO | `false`

### Data Object

The `data` object is used in to pass first party-data to the Lotame DMP for use in targeting or other features of the Lotame platform. The object can be passed at setup time in the tag input function. It can also be passed post-page load in the `collect({data})` method that is described further down this page.

The `data` object's parameters are fully described in its own page called [Lightning Tag Data Collection](lightning-tag/data-collection.md).

## Retrieving Audiences

The Lotame Lightning Tag allows you to retrieve audiences for the customer on your site. There is a few ways to do this and they are described below.

### Callback

As noted above in the `config` object created at script setup, there is a callback method named `onProfileReady` that can be set to be called at the completion of the targeting. This work happens asynchronously so the callback is fired at the point the targeting work is completed.

When fired, the callback is passed a `profile` object that has the following functionality.

Method Name | Return Type | Parameters | Description
----------- | ----------- | ---------- | -----------
`getProfileId()` | String | n/a | The id of the profile associated with the browser
`getAudienceString( [delimiter=','], [limit])` | String | 1. `delimiter` - optional, defaults to comma. <br/> 2. `limit` - optional, defaults to no limit | Delimited string of targeting codes or audience ids, based on your established tag settings. The default, i.e. no arguments, is comma delimited with no limit. Example: `"aud1,aud2"` or `"1,2"`
`getAudiences([limit])` | Array | `limit` - optional, defaults to no limit | Array of either targeting codes as strings or audience ids as integers, based on your established tag settings. Example: `["aud1","aud2"]` or `[123,789]`

### Local Storage

As noted above in the `config` object created at script setup, there is a callback method named `audienceLocalStorage` parameter. If passed, then when targeting is completed, the audiences will be written out to the browser's localStorage.

Value | Result
:---: | ------
`false` | This is the default and will not write audiences to browser localStorage.
`true` |  Lightning Tag will write audiences to localStorage under the key `lotame_CLIENT_ID_auds` (where CLIENT_ID is your Lotame clientId)
`'string'` | Passing a string will tell Lightning Tag to write the audiences to localStorage under the key of the string you pass. (i.e. `audienceLocalStorage: 'myLotameAudiences'`)

Audiences are written out as a string of comma-delimited targeting codes or audience ids, based on your established tag settings. Example: `"aud1,aud2"` or `"123,789"`

### getAudiences()

?> Per Mark "which is a helper to retrieve audiences out of local storage". Need more details as I also see this method on the profile object returned by the callback.

## Other Methods

### Collect()

?> TBD, need to describe the "why" on this to describe it.

```javascript
window.lotame_<lotameClientId>.collectData({data});
```

The `data` object parameters are described in full on the [Data Collection page](lightning-tag/data-collection.md).

### Page()

?> TBD, need more clarity on exactly what this does and when it should be called.
