# Lotame Code Update Addressing the Google Chrome 80 Transition — Technical Requirements

Google has recently announced that starting in Chrome 80, expected to be released on **February 4th, 2020**, they will be enabling by default a feature that will require cookies used in a third-party context to be secure (i.e. `https`).

To prevent interruptions to in-browser data flows, Lotame and its customers must ensure that all calls made to Lotame's URL (`crwdcntrl.net`) use hypertext transfer protocol secure (`https`) prior to the release of Chrome 80.

## Lotame Action Items

1. Lotame is already following Google's new SameSite standard by using `SameSite=None` in web environments that enable 3rd party cookies and `SameSite=Lax` in environments that block 3rd party cookies (where Lotame will use a 1st party cookie). For more information on Google's SameSite Standard, see [Same Site Cookies Explained](https://web.dev/samesite-cookies-explained/).
1. **On January 15th, 2020** we will be updating our JavaScript tags (Standard Tag, Advance Tag, SmartTag, and Lightning Tag) to deploy secure (https) calls in all instances; and updating our https responses to specify cookies as secure.

## Lotame Customer Action Items

As a Lotame customer, if you are firing Lotame http calls directly, you must ensure these are updated to fire securely prior to **January 15th, 2020**. To do so, identify any calls that utilize `http://` and update them to use `https://`

Below, you will find some Lotame calls that you may be deploying directly:

### Lotame's Data Collection Pixel

This call could be used within your ad-server to track campaign performance:

```http://bcp.crwdcntrl.net/5/c=1234/camp_int=advertiser-abcd^campaign-1234^Impression```

This call could also be used to collect data from your webpage/mobile-app, perform ID syncing, or to collect data from a 3rd party partner.

### Lotame's Audience Extraction API

This call is used to retrieve audiences on your users, typically to push audiences into your ad-server ad calls:

```http://ad.crwdcntrl.net/5/c=<client_id>/pe=y/```

### Lotame's Sync Pixel

This call is exclusively used to send your customer IDs into Lotame (ID syncing)

```http://sync.crwdcntrl.net/map/c=<client_id>/tp=<namespace>/tpid=<customer_ids>```

## Important Considerations For Your Previous Deployments

Don’t assume that the only occurrences of the changes from http to https are in HTML or code that compiles as HTML. The references to `crwdcntrl.net` transactions may have been included in your own custom Javascript libraries for performance efficiencies. There may be minified code on a CDN that is used globally by your sites that needs to be updated from source.

If there are examples of un-updated code that are still present in your site as a test you can use the browser inspector and search for `crwdcntrl.net` and see if any of the transactions in the Network tab are coming in over http rather than https.

These updates also affect calls to the Lotame system from native mobile apps in our mobile SDK for Android or iOS. If you rolled your own data collected calls that make URL requests and pass in the IDFA or GAID values from the users device then these paths also need to be updated.

Example

```https://bcp.crwdcntrl.net/5/c=<CLIENT_ID>/e=app/mid=<DEVICE_ID>/dt=<IDFA/GAID/RIDA/TVOS/MSAI/PST4/SMSG/SONY/LGTV/FTCH>/int=<segment_1>/act=<another_segment>```

Supported Devices include:

- iOS (IDFA)
- Google Advertiser ID (GAID)
- Roku (RIDA)
- Apple TV (TVOS)
- Google Chromecast (CHRM)
- XBOX (MSAI)
- Playstation 4 (PST4)
- Samsung TV (SMSG)
- Sony TV (SONY)
- LG TV (LGTV)
- Fetch TV (FTCH)

## Further Reference and Support

If you have any questions, please reach out to your Lotame representative or support@lotame.com.
