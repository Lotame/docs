# User Consent Guide

## Passing Consent Signals Into Lotame

For clients that utilize Lotame's Consent Management Product, consent signals can be passed directly into Lightning Tag. Use this feature to inform Lotame of each user's consent preference and how they should be handled within the DMP.

```javascript
// Validate Lotame Lightning Tag script is ready
if (window.lotame_<lotameClientId>.onTagReady() ) {

  // Callback that runs when setConsent() completes that
  //  allows webpage processing if needed at this event
  function consentCb(returnData) {};

  // Set these as true or false as per the signals from your User Consent process
  var customerConsents = {
    analytics: true,
    crossdevice: true,
    datasharing: true,
    targeting: true
  };
  window.lotame_<lotameClientId>.setConsent(consentCb, <lotameClientId>, customerConsents);
}
```

The full details of the `returnData` object passed to the callback can be found in the [Lightning Tag Field Reference Guide](lightning-tag/detailed-reference.md).

## Strict User Consent Use-Case

Certain Lotame clients do not want Lotame Lightning Tag to run until the customer has gone through a consent management flow. The Lotame Platform provides the ability to enable a strict consent flag at the account level that prevents data from being collected until Lotame is informed of user consent agreement.

When this flag is enabled, the Lightning Tag has specific configuration to successfully enable collection and targeting on your webpage after your consent flow is complete. The `autoRun: false` flag is set on the Lightning Tag config to prevent the tag from running automatically. Once your consent flow is completed, you set calls the Lightning Tag `setConsent` method to enable the data collection and then the `page()` method to collect data and receive audiences (assuming the customer approved in the flow).

This below example javascript code shows the following happy-path flow:

1. Webpage loads, Lotame tag loads but does not run because of `autoRun: false` config.
1. Customer approves tracking via Client's consent management process.
1. Webpage calls Lotame `setConsent()` method.
1. Webpage activates Lotame data collection and a page-view by calling the `page()` method.
1. Webpage receives Lotame audiences in the `onProfileReady()` callback.
1. Webpage calls Ad-Serving solution passing Lotame audiences.

```javascript
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">  

  <script>

    var targetAndDisplayAds = function(commaSeparatedAudienceString) {
      console.log('Replace with implementation that calls googletag or other ad-rendering capabilities');
    }
  
    // Callback when targeting audience is ready
    var audienceReadyCallback = function(profile) {

      // Get audience string & Call your company's Ad Display function
      var lotameAudiences = profile.getAudienceString(',') || '';
      targetAndDisplayAds(lotameAudiences);
    };
  
    // Lotame Config
    var lotameTagInput = {
      data: {},
      config: {
        clientId: <lotameClientId>,
        onProfileReady: audienceReadyCallback,
        autoRun: false
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

  <script>

    // Validate Lotame Lightning Tag script is ready
    if (window.lotame_<lotameClientId>.onTagReady() ) {

      // Callback that runs when setConsent() completes
      function consentCb(returnData) {
        var dataCollection = {
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
        };

        // Call the page() method to fire Data Collection
        // When complete, this will fire the audienceReadyCallback defined in the
        //   Lotame config above to pass audiences to your site's ad-serving solution
        window.lotame_<lotameClientId>.page(dataCollection);
      };

      // Set these as true or false as per the signals from your User Consent process
      var customerConsents = {
        analytics: true,
        crossdevice: true,
        datasharing: true,
        targeting: true
      };
      window.lotame_<lotameClientId>.setConsent(consentCb, <lotameClientId>, customerConsents);

    }
  </script>
```

To see the full feature set that the Lotame Lightning Tag offers, visit the [Lightning Tag Field Reference Guide](lightning-tag/detailed-reference.md). Also check out the [Lightning Tag FAQ](lightning-tag/faq.md) for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.
