# FAQs

## Why don't I see audiences being sent to my ad-service solution?

Here are some things to check. These will all involve opening up developer tools in a browser and looking at the network tab.

1. Do you have the onProfileReady callback handler set up to pass the data received from Lotame to your ad-serving solution?
1. Look for a call to the Lotame servers that looks like https://bcp.crwdcntrl.net/6/data
1. Look for calls to your ad-serving solution and ensure that these calls come after the the above calls.
1. If you are using Google, ensure that the call to Lotame above finishes before enableServices is called.
1. If you still need assistance, please reach out to Lotame by emailing support@lotame.com.

## How do I make sure that the newest Lotame audiences are included in my ad calls?

Make sure that the onProfileReady() event is fired before rendering your ads. The following script shows how to wait for up to a certain amount of time (500ms in the example) to allow Lotame's audience processing to complete before making the ad requests on the page. In the unlikely case that Lotame's new audiences are not returned in that time, any audiences stored in local storage from a previous call will be used.

```html
<head>
  <link rel="dns-prefetch" href="https://tags.crwdcntrl.net">
  <link rel="dns-prefetch" href="https://bcp.crwdcntrl.net">  

  <script>
  
    var targetAndDisplayAds = function(commaSeparatedAudienceString) {
      console.log('Replace with implementation that calls googletag or other ad-rendering capabilities');
    }

    // Adjust this timing as needed
    var maxAudienceWaitMillis = 500;
  
    // Set up fallback logic if timeout is reached
    var audienceReadyTimeoutId = window.setTimeout(function() {
      var localStorageAudiences = '';

      try {
        localStorageAudiences = window.localStorage.getItem('lotame_<lotameClientId>_auds') || '';
      } catch(e) {
      } finally {
        targetAndDisplayAds(localStorageAudiences);
      }
    }, maxAudienceWaitMillis);

    var audienceReadyCallback = function(profile) { 
      // Get audience string & Call your company's Ad Display function
      var lotameAudiences = profile.getAudienceString(',') || '';

      // Cancel the timeout
      window.clearTimeout(audienceReadyTimeoutId);

      targetAndDisplayAds(lotameAudiences);
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
