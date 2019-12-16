# User Consent Guide

## Passing Consent Signals Into Lotame

For clients that utilize Lotame's Consent Management Product, consent signals can be passed directly into Lightning Tag. Use this feature to inform Lotame of each user's consent preference and how they are handled within the DMP.

```javascript
// Callback that runs when setConsent() completes that
// allows webpage-specific processing if needed at this event
function consentCb(returnData) {};

// Set these as true or false as per the signals from your User Consent process
var customerConsents = {
  analytics: true,
  crossdevice: true,
  datasharing: true,
  targeting: true
};
window.lotame_<lotameClientId>.setConsent(consentCb, <lotameConsentClientid>, customerConsents);
```

A successful call to `setConsent` passes the following object to the callback method:

```javascript
{
  "consent": [{
    "clientid":CONSENT_CLIENT_ID,
    "lastupdate": UTC_SECONDS,
    "types": [{
      "name": "analytics",
      "consent": true
    }, {
      "name": "targeting",
      "consent": true
    }, {
      "name": "datasharing",
      "consent": true
    }, {
      "name": "crossdevice",
      "consent": true
    }]
  }]
}
```

## Retrieving Consent Signals

To retrieve consent signals previously sent to Lotame, use the `getConsent()` method as shown below. The object passed to the callback method is the same object format as above in `setConsent`.

```javascript
// Callback that runs when setConsent() completes that
// allows webpage-specific processing if needed at this event
function consentCb(returnData) {};

// Set these as true or false as per the signals from your User Consent process
window.lotame_<lotameClientId>.getConsent(consentCb, <lotameConsentClientid>);
```

The full details of these two methods, along with all other functionality of the Lotame Lightning Tag, can be found in the [Lightning Tag Field Reference Guide](lightning-tag/detailed-reference.md).

