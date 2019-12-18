# Pre-Implementation Setup

Before deploying the Lotame Lightning Tag on your site, you must establish the following settings within the DMP:

1. **Domain Whitelist** - the list of domains on which Lightning Tag is allowed to collect data and utilize third-party cookies, first-party cookies, and localStorage.
1. **Profile Extraction: Audience Owner Client** - The client id for which to provide audiences.
1. **Profile Extraction: Audience Format** - Decide whether Lightning Tag will provide audiences as ids or targeting codes.

## Domain Whitelisting

For Lightning Tag to operate on your site, the domain must first be whitelisted. Domains can be added individually, or in bulk, directly into the DMP by navigating to ‘Client Admin: Manage Settings’ → ‘Domains.’

?> Ensure the domain is whitelisted to the correct client account. You may have access to multiple linked child accounts under your main parent account. The domain should be added to the client ID that is being applied to the tag on your web pages.

## Profile Extraction: Audience Owner Client

By default, Lightning Tag returns audiences owned by the client ID passed to the tag. In many client use-cases, audiences are created and managed within a separate parent account. If this is your use-case, your Lotame account manager must adjust the settings to return audiences from this linked parent account.

Please work with your Lotame account manager or support@lotame.com to ensure the correct settings are established.

## Profile Extraction: Audience Format

For the audiences included in the callback, Lotame can either return the audience ID or ‘targeting code’ (a custom value assigned to each audience).

- Audience Id: `{"pid":"9132c153ba0233f7462881b2843e0932","tc":["12345","26654"]}`
- Targeting Code: `{"pid":"9132c153ba0233f7462881b2843e0932","tc":["jazzmusic","female"]}`

Please work with your Lotame account manager or support@lotame.com to ensure the desired setting is established.
