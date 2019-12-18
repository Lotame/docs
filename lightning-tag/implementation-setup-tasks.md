# Pre-Implementation Setup

Before deploying the Lotame Lightning Tag on your site, you must establish the following settings within the DMP:

1. **Domain Whitelist** - The list of domains on which Lightning Tag is allowed to collect data and utilize third-party cookies, first-party cookies, and localStorage.
1. **Profile Extraction: Audience Owner Client** - The client id for which to provide audiences.
1. **Profile Extraction: Audience Format** - Whether Lightning Tag will return audiences as ids or targeting codes.

## Domain Whitelisting

For Lightning Tag to operate on your site, the domain must first be whitelisted. Domains can be whitelisted individually, or in bulk, directly within your DMP account by navigating to ‘Client Admin: Manage Settings’ → ‘Domains.’.

?> Ensure the domain is whitelisted to the correct client account. You may have access to multiple linked child accounts under your main parent account. The domain should be added to the client ID that is being applied to the tag on your web pages.

## Profile Extraction: Audience Owner Client

By default, Lightning Tag returns audiences owned by the client ID passed to the tag. In many cases, audiences are created and managed within a separate parent account. If this is how you are set up within the DMP, your Lotame account manager must adjust the settings to return audiences from this linked parent account.

Please work with your Lotame account manager or support@lotame.com to ensure the correct settings are established.

## Profile Extraction: Audience Format

For the audiences returned in Profile Extraction, Lotame can either return the audience ID or ‘targeting code’ (a custom value assigned to each audience).

- Audience Id: `[12345,26654]`
- Targeting Code: `["jazzmusic","female"]`

Please work with your Lotame account manager or support@lotame.com to ensure the desired setting is established.
