---
title: Migration Services
layout: page
categories: ["Cloud","Migration"]
---

## Migration Services Overview

Vanilla offers full, professional migration services from nearly any platform imaginable. If you provide the data, we'll figure out how to make it work in Vanilla.

### Estimating your migration

1. You provide the platform name, version number (if applicable), and number of users & posts for your current community platform.
1. Vanilla will provide an SFTP drop.
1. You provide a test dump of your data to the SFTP. This is typically a database dump, but may include additional files like avatars or attachments depending on your platform. **Please provide a full dump for an accurate estimate**. Vanilla will generally accept an NDA if one is required.
2. You provide the email address for the user that will become the owner account.
3. You provide any working username & password combination for testing.
2. Vanilla will evaluate the data and provide a cost estimate and, if requested, a downtime estimate (how long your forum will need to be offline during the transition).
3. Vanilla will provide feedback on concerns, limitations, or other needs.

### Test migration

1. Vanilla will import your test dump to a Vanilla forum.
2. You review the test import and provide feedback on data integrity.
3. Vanilla makes any corrections necessary.
4. You provide a list of URL patterns that will need to be 301 redirected (discussions, profiles, categories, etc). We will have a pre-existing list for most common platforms, but be sure to mention any customizations or special URLs you need us to handle.
5. Vanilla implements the 301 redirects and you test them.
6. A final migration is scheduled.

### Final migration

1. You place your current forum into read-only mode.
2. You create a fresh data dump, upload it to Vanilla's SFTP, and notify your contact(s) at Vanilla.
3. Vanilla confirms it received your final dump and the final import begins.
4. Vanilla notifies you when the import is complete.
5. You verify the import is correct.
6. You switch over DNS, SSO, and/or any other final switchovers. Consult with your Vanilla contact if you are unsure.
7. If your forum was previously on a domain not being redirected to your new forum, be sure to 301 redirect the folder (with the entire URL intact) to your new forum.
8. You're done. Sweet!