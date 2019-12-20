# Google Ad Manager Guide

## Overall Notes

This document has the best practice for integrating the Lotame Lightning Tag with Google Ad Manager (formerly known as DoubleClick for Publishers). The example below allows you to receive the Lotame audience matches for a user and pass those audiences to Google Ad Manager for targeting.

NOTE: If you are using an ad-server other than Google, simply replace any lines referencing googletag with references to your specific implementation.

In order to ensure the fastest audience targeting response, copy and paste the following into the `<head>` section of your page. Make sure that the onProfileReady callback returns before the `google.enableServices()` call is made to ensure that your Lotame audiences are included in the call to Google. 

Lotame recommends loading this directly on your page in the `<head>` section to best ensure that targeting calls will complete in time to successfully include Lotame's audiences in your ad calls. We recommend against loading through a tag manager such as Google Tag Manager (GTM). 

!> If you must use a tag manager, use any priority or sequencing features available to have the Lotame Lightning Tag fire as early as possible. Also, include the below prefetch calls in the `<head>` section of your page and not in the tag management code as this will speed up the loading of the Lightning Tag script to best ensure Lotame audiences are available to your ad calls.

```html
<link rel="dns-prefetch" href="https://tags.crwdcntrl.net">            
<link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">
```

!> By default we send all audiences for your user to Google. If you would like to limit that number, see [Limit Audiences](lightning-tag/faq?id=how-can-i-limit-the-number-of-audiences-returned).

To see the full feature set that the Lotame Lightning Tag offers, including passing in your own data fields for rule matching purposes, visit the [Detailed Reference Guide](lightning-tag/detailed-reference.md). Also check out the [Lightning Tag FAQ](lightning-tag/faq.md) for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.

## Push Audiences to Google

Before using this snippet, please replace the instances of `<lotameClientId>` with your Lotame account's client ID.

```html
<head>
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">
  
  <script>
    ! function() {
      window.googletag = window.googletag || {};
      window.googletag.cmd = window.googletag.cmd || []; 

      // Immediately get audiences from local storage and get them loaded
      try {
        var localStorageAudiences = window.localStorage.getItem('lotame_<lotameClientId>_auds') || '';
        googletag.cmd.push(function() {
          window.googletag.pubads().setTargeting('lotame', localStorageAudiences);
        });  
      } catch(e) {
      } 

      // Callback when targeting audience is ready to push latest audience data
      var audienceReadyCallback = function (profile) {

        // Get audiences in a comma-separated string which conforms to Google Ads input format
        var lotameAudiences = profile.getAudienceString(',') || '';
    
        // Set the new target audiences for call to Google
        googletag.cmd.push(function() {
          window.googletag.pubads().setTargeting('lotame', lotameAudiences);
        });  
      };
    
      // Lotame Config
      var lotameTagInput = {
        data: {},
        config: {
          clientId: <lotameClientId>,
          audienceLocalStorage: true, // written to 'lotame_<lotameClientId>_auds' key
          onProfileReady: audienceReadyCallback
        }
      };

      // Lotame initialization
      var lotameConfig = lotameTagInput.config || {};
      var namespace = window['lotame_' + lotameConfig.clientId] = {};
      namespace.config = lotameConfig;
      namespace.data = lotameTagInput.data || {};
    }();
  </script>
  
  <script async src="https://tags.crwdcntrl.net/lt/c/<lotameClientId>/lt.min.js"></script>
</head>
```


## Push Audiences and Profile Id to Google

Before using this snippet, please replace the instances of `<lotameClientId>` with your Lotame account's client ID.
This example uses `lotame` as the Google audience targeting key and `lpid` as the key for the lotame profile id sent to Google.

```html
<head>
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">
  
  <script>
    ! function() {
      window.googletag = window.googletag || {};
      window.googletag.cmd = window.googletag.cmd || []; 

      // Immediately get audiences from local storage and get them loaded
      try {
        var localStorageAudiences = window.localStorage.getItem('lotame_<lotameClientId>_auds') || '';
        googletag.cmd.push(function() {
          window.googletag.pubads().setTargeting('lotame', localStorageAudiences);
        });

        var localStoragePid = window.localStorage.getItem('_cc_id') || '';
        if (localStoragePid) {
          googletag.cmd.push(function() {
              window.googletag.pubads().setTargeting('lpid', localStoragePid);
          });
        }
      } catch(e) {
      } 

      // Callback when targeting audience is ready to push latest audience data
      var audienceReadyCallback = function (profile) {

        // Get audiences in a comma-separated string which conforms to Google Ads input format
        var lotameAudiences = profile.getAudienceString(',') || '';
    
        // Set the new target audiences for call to Google
        googletag.cmd.push(function() {
          window.googletag.pubads().setTargeting('lotame', lotameAudiences);
        });  

        var lotamePid = profile.getProfileId() || '';
        googletag.cmd.push(function() {
          window.googletag.pubads().setTargeting('lpid', lotamePid);
        });
      };
    
      // Lotame Config
      var lotameTagInput = {
        data: {},
        config: {
          clientId: <lotameClientId>,
          audienceLocalStorage: true, // written to 'lotame_<lotameClientId>_auds' key
          onProfileReady: audienceReadyCallback
        }
      };

      // Lotame initialization
      var lotameConfig = lotameTagInput.config || {};
      var namespace = window['lotame_' + lotameConfig.clientId] = {};
      namespace.config = lotameConfig;
      namespace.data = lotameTagInput.data || {};
    } ();
  </script>
  
  <script async src="https://tags.crwdcntrl.net/lt/c/<lotameClientId>/lt.min.js"></script>
</head>
```
