# FAQs

## Lightning Tag is implemented, but no audiences are sent to the ad-serving solution

Here are some things to check. These will all involve opening up developer tools in a browser  and looking at the network tab.

1. Do you have the onProfileReady callback handler set up to pass the data received from Lotame to your ad-serving solution?
1. Look for a call to the Lotame servers that looks like https://bcp.crwdcntrl.net/6/data
1. Look for calls to your ad-serving solution and ensure that these calls come after the the above calls.
1. If you are using Google, ensure that the call to Lotame above finishes before enableServices is called.
1. If you still need assistance, please reach out to Lotame by emailing support@lotame.com.

## Wait for the audience data to get freshest possible, but not too long as to delay the ad display

This scenario is slightly different than the Best Practice implementation. In this scenario, instead of calling myCompanyAdDisplay(lotameAudiences) immediately using possibly stale localStorage audiences, this script waits 500ms to allow Lotame’s processing to complete yet not wait so long we delay ad display. Generally Lotame’s processing will be complete within this time and the new audiences will be available in localStorage.

```html
<head>
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">  

  <script>
  
    // Adjust this timing as needed
    var maxAudienceWaitMillis = 500;
  
    // Get from LocalStorage & Call Ad display if timeout happens
    var audienceReadyTimeoutId = window.setTimeout(function() {      
      if (window.localStorage && window.localStorage.getItem) {
        var localStorageAudiences = window.localStorage.getItem('lotame_<lotameClientId>_auds') || "";

        // use audiences however your company needs.
        targetAndDisplayAds(audiences);
      }
  
    }, maxAudienceWaitMillis);

     var audienceReadyCallback = function(profile) { 

       // Get audience string & Call your company's Ad Display function
       var lotameAudiences = profile.getAudienceString(',') || "";
       targetAndDisplayAds(lotameAudiences);

       // Cancel the timout
       window.clearTimeout(audienceReadyTimeoutId);
    };
  
    // Lotame Config
    var lotameTagInput = {
      data: {},
      config: {
        clientId: <lotameClientId>,
        onProfileReady: audienceReadyCallback,
        audienceLocalStorage: true // written to 'lotame_<lotameClientId>_auds' key
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
</head>```

If you still need assistance, please reach out to Lotame by emailing support@lotame.com.
