# Implementation Guide

This page has the Best Practice for implementing the Lotame Lightning Tag on to your site. The example below allows you to receive the Lotame audience matches for a customer and pass those audiences to your ad server’s targeting function. This includes customers that have been on your site before along with a customer having their first page view on your site.

In order to ensure the fastest audience targeting response, copy and paste the following into the `<head>` section of your page.

Before using this snippet, please replace the instances of `<lotameClientId>` with your Lotame account's client ID. Also change the `myCompanyAdDisplay()` function to whatever calls your company’s ad serving solution.

```html
<head>
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">  

  <script>

    var myCompanyAdDisplay = function(audiences) {

      // use audiences however your company needs.
      targetAndDisplayAds(audiences);
    };
  
    // Immediately get audiences from local storage and get them loaded
    if (window.localStorage && window.localStorage.getItem) {

      var localStorageAudiences = window.localStorage.getItem('lotame_<lotameClientId>_auds') || "";
      myCompanyAdDisplay(localStorageAudiences);
    }

    // Callback when targeting audience is ready
    var audienceReadyCallback = function(profile) {

      // Get audience string & Call your company's Ad Display function
      var lotameAudiences = profile.getAudienceString(',') || "";
      myCompanyAdDisplay(lotameAudiences);
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

To see the full feature set that the Lotame Lightning Tag offers, including passing in your own data fields for rule matching purposes, visit the [Lightning Tag Field Reference Guide](lightning-tag/detailed-reference.md). Also check out the [Lightning Tag FAQ](lightning-tag/faq.md) for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.
