# Lotame Lightning Tag

!> Please note that the Lotame Lightning Tag is currently in closed beta release. If your company would like to join the beta, please reach out to support@lotame.com.

## Overview

The Lotame Lightning Tag is a JavaScript tag and API that facilitates all DMP operations on your webpages. This all-in-one solution performs both data collection and audience extraction, along with other important functions described below.

Lightning Tag executes in two stages, the 1st impression stage and the page collection stage.

The 1st impression stage executes immediately upon `<script>` rendering by the browser and can perform the following:

- **Data Collection**: Collect data explicitly passed the tag input, or available via collection rules that utilize the page URL or any page elements present at the time.
- **Profile Extraction**: Optionally, provide profile metadata, such as the id and matching audiences.
- **Pixel Firing**: Optionally, create an iframe to contain and render any relevant id syncing pixels and export beacons.

The page collection stage executes following the page onload event to evaluate all custom data collection rules that inspect page content.

## Initial Setup

Before you enable the Lotame Lightning Tag on your site, you need to perform some steps in your Lotame platform. These steps can be seen in our [Pre-Implementation Steps guide](lightning-tag/implementation-setup-tasks.md).

## Implementation Guides

### Basic Implementation Guide

For Lotame customers that want to collect 1st party data only, along with other basic functions (deploy ID syncing & export beacons). The [Basic Implementation Guide](lightning-tag/basic-implementation.md) will provide our best practice implementation.

### Google Ad Manager Guide

For Lotame customers that use Google Ad Manager (formerly DoubleClick for Publishers) for their ad-serving, we have an example showing you how to pass your Lotame audience data in our Best Practice Guide for integrating [Lotame Lightning Tag with Google Ad Manager](lightning-tag/implementation-google-ad-manager.md).

### Data Collection Guide

To pass data to your Lotame DMP, review the Lotame [Lightning Tag Data Collection Guide](lightning-tag/data-collection.md).

### User Consent Guide

If your company has user consent requirements, refer to the [Lightning Tag User Consent Guide](lightning-tag/data-collection-user-consent.md)

## Detailed Lightning Tag Reference

The above are basic implementations to get up and running. To use the complete power of Lotame Lightning Tag, review the [Detailed Reference Document](lightning-tag/detailed-reference.md) detailing all possible functionality available within the Lotame Lightning Tag including passing your own data to assist in the matching rules.

## FAQ

[Click here](lightning-tag/faq.md) for answers to common questions about the Lotame Lightning Tag.
