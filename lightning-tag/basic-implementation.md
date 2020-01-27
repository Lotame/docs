# Basic Lightning Tag Implementation 

!> This document provides information on how to implement Lightning Tag in its basic form. Throughout this page, there will be links to documentation detailing additional features and use-cases that can be deployed.

The basic version of Lightning Tag will:

1. Execute data collection rules that you have configured in cooperation with your Lotame account representative.
2. Deploy subscribed sync pixels.
3. Deploy export beacons.

```javascript 
<head>
  <link rel="preconnect" href="https://tags.crwdcntrl.net">
  <link rel="preconnect" href="https://bcp.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">            
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">
  
  <script>
    ! function() {
      var lotameClientId = '<lotameClientId>';
      var lotameTagInput = {
          data: {},
          config: {
            clientId: Number(lotameClientId)
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
* Before using this snippet, please replace the instances of `<lotameClientId>`with your Lotame client ID.
* Lotame recommends loading the Lightning Tag directly on your page, in the <head> section. Lotame recommends against loading through a tag manager such as Google Tag Manager (GTM) if at all possible.


Additional functionality that can be added to Lightning Tag:

1. [Data Collection](lightning-tag/data-collection.md) - sending 1st party data directly into the tag.
2. [Profile Extraction](lightning-tag/detailed-reference?id=config-object.md) - retrieving the profile id and list of audiences.
    * [Passing audiences and/or profile id into Google Ad Manager calls (or other platforms)](lightning-tag/implementation-google-ad-manager.md) 
3. [Consent API](lightning-tag/user-consent.md) - passing consent signals into the DMP.

!> To see the full feature set that the Lotame Lightning Tag offers, including passing in your own data fields for rule matching purposes, visit the [Detailed Reference Guide](lightning-tag/detailed-reference.md). Also check out the [Lightning Tag FAQ](lightning-tag/faq.md) for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.
