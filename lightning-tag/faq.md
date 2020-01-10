# FAQs

## Why don't I see audiences being sent to my ad-serving solution?

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
    var targetAndDisplayAds = function(audienceArray) {
      console.log('Replace with implementation that calls googletag or other ad-rendering capabilities');
    }

    ! function() {
      var lotameClientId = '<lotameClientId>';
      var audLocalStorageKey = 'lotame_' + lotameClientId + '_auds';

      // Adjust this timing as needed
      var maxAudienceWaitMillis = 500;
    
      // Set up fallback logic if timeout is reached
      var audienceReadyTimeoutId = window.setTimeout(function() {
        var localStorageAudiences = [];

        try {
          var tempAudiences = window.localStorage.getItem(audLocalStorageKey) || '';

          if (tempAudiences) {
            localStorageAudiences = tempAudiences.split(',');
          }
        } catch(e) {
        } finally {
          targetAndDisplayAds(localStorageAudiences);
        }
      }, maxAudienceWaitMillis);

      var audienceReadyCallback = function(profile) { 
        // Get audience string & Call your company's Ad Display function
        var lotameAudiences = profile.getAudiences() || [];

        // Cancel the timeout
        window.clearTimeout(audienceReadyTimeoutId);

        targetAndDisplayAds(lotameAudiences);
      };
    
      // Lotame Config
      var lotameTagInput = {
        data: {},
        config: {
          clientId: Number(lotameClientId),
          audienceLocalStorage: audLocalStorageKey,
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

If you still need assistance, please reach out to Lotame by emailing support@lotame.com.

## How can I access Lotame's profile id in my browser?

1. In the onProfileReady callback, you can retrieve the latest profile id for the browser by using the `getProfileId()` function as described at [Audience Callback](lightning-tag/detailed-reference?id=audience-callback).

1. The Lotame profile id is stored in local storage when possible under the key `_cc_id` and can be retrieved by the following code:

```html
var localStoragePid = '';
try {
  localStoragePid = window.localStorage.getItem('_cc_id') || '';
} catch(e) {
} 
```

## How can I limit the number of audiences returned?

Our servers will always return the full set of audiences that your user qualifies for. However, you can limit the number of audiences you use through the following methods:

1. In the `onProfileReady` callback, the profile object has both `getAudiences()` and `getAudiencesString()` functions which accept a limit parameter as follows
   ```html
    var audienceReadyCallback = function (profile) {
      var limit = 1000;

      // Get audiences as an array
      var lotameAudiences = profile.getAudiences(limit) || [];
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
   ```
2. When retrieving from local storage, you can use the following snippet to limit the number returned
   ```html
    var localStorageAudiencesRaw = localStorage.getItem('lotame_<lotameClientId>_auds') || '';
    var localStorageAudiences = localStorageAudiencesRaw.split(',');
    var maxItems = 1000;
    var targetingList = localStorageAudiences.slice(0, maxItems);
   ```

## How can I prioritize which audiences are returned first?

Provide a list of audiences you would like to return first to your Lotame representative, and they will prioritize them accordingly in the platform.