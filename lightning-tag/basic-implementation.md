# Base Lightning Tag Implementation 

!> This document provides information on how to implement Lightning Tag in its basic form. Throughout this page, there will be links to documentation detailing additional features and use-cases that can be deployed

Setup of the Lotame Lightning Tag javascript on your website involves an input object that gets passed to an initialization function that prepares a window namespace on your page for the javascript tag itself. 

Lotame recommends loading the Lightning Tag directly on your page, in the <head> section. While not always possible, Lotame recommends against loading through a tag manager such as Google Tag Manager (GTM)

The basic setup looks like the below:

```javascript 
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
```

The basic version of Lightning Tag above will automatically execute any data collection rules that you have configured in cooperation with your Lotame account representative. This version of the tag is right for you if:

1. 1st party data collection is the only desired use-case and you will not be passing data directly into the tag, instead relying on data collection rules configured within the DMP in cooperation with your Lotame account representative. 
1. You do not need audiences to be returned to the page for client-side activation (example passing audiences into your [Google Ad Manager](lightning-tag/implementation-google-ad-manager.md) calls). 

Additional functionality that can be added to Lightning Tag:
Data Collection - sending 1st party data directly into the tag
Profile Extraction - retrieving the profile id and list of audiences 
Passing audiences and/or profile id into Google Ad Manager calls (or other platforms) - https://lotame.github.io/docs/#/lightning-tag/implementation-google-ad-manager
Consent API - passing consent signals into the DMP
To see the full feature set that the Lotame Lightning Tag offers, including passing in your own data fields for rule matching purposes, visit the Detailed Reference Guide. Also check out the Lightning Tag FAQ for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.
.
