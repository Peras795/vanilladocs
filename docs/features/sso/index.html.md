---
title: Single Sign-On
layout: docs
categories: ["Features","Single Sign-On"]
---

## Single Sign-On Overview

Vanilla's web-based single sign on (SSO) has 2 parts:

1. Your SSO endpoint (based on one of our example libraries) on _your_ site.
2. Vanilla's jsConnect plugin.

jsConnect pings your endpoint whenever it wants to know if a user is logged in on your site. It does this using the user's current session, so you can use your normal "is logged in" detection to determine this - nothing fancy here.

### Your endpoint

Your endpoint needs to say 1 of 3 things when it's called:

1. The current user is a guest (not logged in).
2. User is logged in here, but this wasn't a secure request, so here's his name & photo only.
3. User is logged in AND this was securely requested, so here's all the requested data (including email and a unique ID from your system, which are required).

Our example libaries help you structure the output so that jsConnect can read it correctly. There is separate [technical documentation for setting it up](http://blog.vanillaforums.com/jsconnect-technical-documentation/).


### How jsConnect maps users

After calling your endpoint and getting a "signed in" reply, jsConnect looks up the user. If they've _already_ used SSO, we've permanently mapped their unique ID to our UserID, so we sign them into that account. If they _haven't_ used SSO before, 1 of 3 things happens:

1. If the email for the user has never been used on the forum, it makes them a new account using the data passed and signs them into it.
2. If the email is in use and `AutoConnect` is enabled, we will immediately sign them in (and permanently store the mapping).
3. If the email is in use and `AutoConnect` is NOT enabled, we will prompt the user for their forum password to confirm their identity. This is the default setting because it is safer to assume you haven't verified their email address.

Vanilla Cloud customers should request Support enable `AutoConnect` if they desire it. That is an important component of a seamless experience.

### Tightening the integration

To get very tight SSO integration, you will also want to follow these steps. Always carefully test your _basic_ SSO authentication **before** tightening your integration.

1. Change your registration method to 'Connect' to block non-SSO users from registering.
2. Set your sign-in, sign-out, and registration URLs under jsConnect's settings in your Dashboard.
3. Check "Make this connection your default signin method." 
4. When linking or redirecting signed-in users to your forum, use the `/sso` endpoint on the forum. This triggers jsConnect's user lookup **on the connection with "default sign in method" selected** without the user needing to click. Optionally, you can provide a `Target` parameter with a relative path to specify where they should ultimately land on the forum. Example: `http://forum.yoursite.com/sso?Target=/categories`. This is the final critical step in a fully seamless experience.
5. For seamless SSO on an embedded forum, see our [embedded SSO solution](http://blog.vanillaforums.com/jsconnect-technical-documentation-for-embedded-sso/).

### Testing the integration

Use the "Test URL" link under your jsConnect settings to see if your endpoint is returning a good response. 

Try using your browser's Incognite/Private window mode for testing. This allows you to have a separate "test" session while you stay logged in as the administrator in your main session to make changes quickly.


### Common questions

**What do we do if SSO breaks and we're locked out?**

You can log back in using the "hidden" URL `/entry/password` to sign-in with an existing forum account. This page is never redirected for SSO.

**Can't we just add users over the API?**

We _strongly recommend against_ adding users over the API. Using jsConnect and allowing it to add users as they sign-in is the most robust and reliable approach that will cause you the fewest issues.

**Can we use multiple SSO connections?**

Absolutely. However, only one can be the default, which is what will trigger when `/sso` is used.

**How do we have seamless sign-in for users who go directly to the forum?**

If you've followed all the steps above, the only way a user can be not signed into the forum while signed into your main site is if they sign into your site separately and then manually visit the forum by typing in the address manually or clicking a browser bookmark. In this scenario, they would need to click a prompt to finish forum sign-in.

**How is sign-out handled?**

We bounce them to your sign-in page after they sign-out on the forum. To do the opposite, redirect users thru our sign-out page, again passing a `Target` parameter so they finish where you want them to.

**Does this work for native applications, non-web scenarios, or third-party products?** 

No. jsConnect is purely a web-based SSO workflow for your existing login system. We have separate products for popular third-party platforms and non-web scenarios.

**Can we reverse-SSO to Vanilla via API to see if a user is logged in there?**

Not at this time.

**Can we use jsConnect without providing an email address?**

No. You absolutely must send an email, which is the only method for mapping users. If you are importing forum users without email addresses and need a way to map them over SSO, we recommend using dummy email addresses that follow a formula like `{uniqueID}`@yoursite.com. 

**Our system has overlapping names but Vanilla needs unique ones. How can we handle this?**

We strongly recommend assigning them unique usernames in your system, then passing that unique username to jsConnect. Keep in mind that, for features like mentions and autocomplete to work, usernames must be unique and adhere to Vanilla's username restrictions. Passing a duplicate `Name` field for a new user over jsConnect will cause the connection to fail.

**Can we set roles over jsConnect?**

Yes, see our [technical documentation](http://blog.vanillaforums.com/jsconnect-technical-documentation/) for more information.

**The settings & endpoint response look correct now, but it's still not working or redirecting properly.**

Try clearing your cookies and browser cache and restarting your browser. Use Incognito/Private mode whenever possible. Also, beware of Firefox caching redirects; when in doubt, double-check a redirect using another browser.

Repeatedly attempting SSO with many sessions and accounts with changing settings can cause unexpected results. This isn't a problem users would ever run into day-to-day.
