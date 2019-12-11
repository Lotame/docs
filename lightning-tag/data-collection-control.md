# Controlling Data Collection Timing

Some Lotame clients do not want Lotame's data collection and targeting to run automatically. A use-case is checking for customer consent to tracking to comply with GPDR. In this use-case, the client does not want Lotame Lightning Tag to run until the customer has gone through consent management flow.

This page has example javascript code to show the following flow:

1. Webpage loads.
1. Lotame Lightning Tag initializes but is configured to not run automatically by disabling autoRun.
1. Customer approves tracking via Client's consent management process.
1. Webpage activates Lotame data collection.
1. Webpage receives Lotame audiences.
1. Webpage calls Ad-Serving solution passing Lotame audiences.

```javascript
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">  

  <script>

    var myCompanyAdDisplay = function(audiences) {

      // use audiences however your company needs.
      targetAndDisplayAds(audiences);
    };
  
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
    // Your website's Consent Management implementation is called
    //  AND user accepted the agreement leads to this code that activates Lotame Lightning Tag

    // Validate Lotame Lightning Tag script is ready
    if (window.lotame_<lotameClientId>.onTagReady() ) {

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
        // When complete, this will call audienceReadyCallback defined in Lotame config above
        //   which passes audiences to your site's ad-serving solution
        window.lotame_<lotameClientId>.page(dataCollection);
      };

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
