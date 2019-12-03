# Implementation Guide with Google Ad Manager

This document has the Best Practice for integrating the Lotame Lightning Tag with Google Ad Manager (formerly known as DoubleClick for Publishers). The example below allows you to receive the Lotame audience matches for a customer and pass those audiences to Google Ad Manager for targeting.

In order to ensure the fastest audience targeting response, copy and paste the following into the `<head>` section of your page. Deploy this Javascript after the `googletag` object is set up in the `<head>` section earlier on the page, but before the `google.enableServices()` call to ensure that your Lotame audiences are included in the passing of fields to Google. 

Before using this snippet, please replace the instances of `<lotameClientId>` with your Lotame account's client ID.

```html
<head>
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">
  
  <script>
    window.googletag = window.googletag || {};
    window.googletag.cmd = window.googletag.cmd || []; 

    // Immediately get audiences from local storage and get them loaded
    if (window.localStorage && window.localStorage.getItem) {
      var localStorageAudiences = window.localStorage.getItem('lotame_<lotameClientId>_auds') || "";
      googletag.cmd.push(function() {
        window.googletag.pubads().setTargeting('lotame', localStorageAudiences);
      });  
    }

    // Callback when targeting audience is ready to push latest audience data
    var audienceReadyCallback = function (profile) {

      // Get audiences in a comma separated string which conforms to Google Ads input format
      var lotameAudiences = profile.getAudienceString(',') || "";
  
      // Set the target audiences at Google
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
    ! function(input) {
      input = input || {};
      var config = input.config || {};
      var namespace = window['lotame_' + config.clientId] = {};
      namespace.config = config;
      namespace.data = input.data || {};
    } (lotameTagInput);
  </script>
  
  <script async src="https://tags.crwdcntrl.net/lt/c/<lotameClientId>/lt.min.js"></script>
</head>
```

Lotame recommends loading this directly on your page in the `<head>` section to best ensure that targeting calls will complete in time to successfully include Lotame's audiences in your ad calls. We recommend against loading through a tag manager such as Google Tag Manager (GTM). 

!> If you must use a tag manager, use any priority or sequencing features available to have the Lotame Lightning Tag fire as early as possible. Also, include the below prefetch calls in the `<head>` section of your page and not in the tag management code as this will speed up the loading of the Lightning Tag script to best ensure Lotame audiences are available to your ad calls.

```html
<link rel="dns-prefetch" href="https://tags.crwdcntrl.net">            
<link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">
```

To see the full feature set that the Lotame Lightning Tag offers, including passing in your own data fields for rule matching purposes, visit the Lightning Tag Field Reference Guide. Also check out the Lightning Tag FAQ for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.
