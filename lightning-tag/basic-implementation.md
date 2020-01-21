# Base Lightning Tag Implementation 

!> This document provides information on how to implement Lightning Tag in its basic form. Throughout this page, there will be links to documentation detailing additional features and use-cases that can be deployed

The base version of Lightning Tag will only:

1. Execute data collection rules that you have configured in cooperation with your Lotame account representative
1. Fire subscribed sync pixels

This version of the tag is right for you if:

1. 1st party data collection is the only desired use-case and you will not be passing data directly into the tag; instead relying on data collection rules configured within the DMP in cooperation with your Lotame account representative. 
   * [How to pass data directly into the tag](lightning-tag/data-collection.md)
1. You do not need audiences to be returned to the page for client-side activation (example passing audiences into your [Google Ad Manager](lightning-tag/implementation-google-ad-manager.md) calls). 
   * [How to receive information on the profile](lightning-tag/detailed-reference?id=config-object.md)


The base verison of Lightning Tag:

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

Note:

* Setup of the Lotame Lightning Tag javascript on your website involves an input object that gets passed to an initialization function that prepares a window namespace on your page for the javascript tag itself. 
* Lotame recommends loading the Lightning Tag directly on your page, in the <head> section. While not always possible, Lotame recommends against loading through a tag manager such as Google Tag Manager (GTM)


Additional functionality that can be added to Lightning Tag:

1. [Data Collection](lightning-tag/data-collection.md) - sending 1st party data directly into the tag
1. [Profile Extraction](lightning-tag/detailed-reference?id=config-object.md) - retrieving the profile id and list of audiences 
    * [Passing audiences and/or profile id into Google Ad Manager calls (or other platforms)](lightning-tag/implementation-google-ad-manager.md) 
1. [Consent API](lightning-tag/user-consent.md) - passing consent signals into the DMP

!> To see the full feature set that the Lotame Lightning Tag offers, including passing in your own data fields for rule matching purposes, visit the [Detailed Reference Guide](lightning-tag/detailed-reference.md). Also check out the [Lightning Tag FAQ](lightning-tag/faq.md) for answers to common questions. If you need any other assistance, please reach out to Lotame by emailing support@lotame.com.

