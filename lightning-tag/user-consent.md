# User Consent Guide

## Passing Consent Signals Into Lotame

For clients that utilize Lotame's Consent Management Product, consent signals can be passed directly into Lightning Tag. Use this feature to inform Lotame of each user's consent preference and how they are handled within the DMP.

```javascript
// Callback that runs when setConsent() completes
function setConsentCb(returnData) {};

// Set these as true or false as per the signals from your User Consent process
var customerConsents = {
  analytics: true,
  crossdevice: true,
  datasharing: true,
  targeting: true
};
window.lotame_<lotameClientId>.cmd.push(function() {
  window.lotame_<lotameClientId>.setConsent(setConsentCb, <lotameConsentClientId>, customerConsents);
});
```

## Retrieving Consent Signals

To retrieve consent signals previously sent to Lotame, use the `getConsent()` method as shown below. The object passed to the callback method is the same object format as above in `setConsent`.

```javascript
// Callback that runs when setConsent() completes
function getConsentCb(returnData) {};

// Call getConsent
window.lotame_<lotameClientId>.cmd.push(function() {
  window.lotame_<lotameClientId>.getConsent(getConsentCb, <lotameConsentClientId>);
});
```

The full details of these two methods, along with all other functionality of the Lotame Lightning Tag, can be found in the [Lightning Tag Field Reference Guide](lightning-tag/detailed-reference.md).

## Callback Data
The following table describes the possible responses that could be contained in the `returnData` object provided to your callbacks:

Return State | Return Object Format | Description
------------ | -------------------- | -----------
Success |{<br/>&nbsp;&nbsp;"consent": [{<br/>&nbsp;&nbsp;&nbsp;&nbsp;"clientid":CONSENT_CLIENT_ID,<br/>&nbsp;&nbsp;&nbsp;&nbsp;"lastupdate": UTC_SECONDS,<br/>&nbsp;&nbsp;&nbsp;&nbsp;"types": [{<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "analytics",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}, {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "targeting",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}, {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "datasharing",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}, {<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "crossdevice",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"consent": true<br/>&nbsp;&nbsp;&nbsp;&nbsp;}]<br/>&nbsp;&nbsp;}]<br/>} | The Lotame DMP recorded the consent signals as indicated and will enforce them going forward. The Lotame Consent API will only store and return here signals that you’ve explicitly provided via a set call (i.e. the types array could be empty).
Browser opted out of Lotame cookies | {"error": 200} | The Lotame DMP is unable to store or enforce consent.
Could not write cookie | {"error": 201} | The Lotame DMP is unable to store or enforce consent.
Consent Collection not enabled | {"error": 202} | The client id is not configured to store or enforce consent.