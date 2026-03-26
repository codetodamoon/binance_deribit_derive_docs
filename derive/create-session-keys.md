# Manage Session Keys

For more information on why you need session keys, refer to the "API Reference" > "Session Keys".

Managing session keys via UX can easily be done by going to the ''Session Keys'' section on the [Developers page](https://app.derive.xyz/developers).

<Image align="center" border={false} src="https://files.readme.io/60705dc4c0ff2f18f2b7e65374a499fe5455029d1c2295227bc7d0be4d555dff-app.derive.xyz_developers_1.png" />

Session Keys in the UX are broken down into two types. Under the hood, both have the same access rights, however each is stored in a separate manner:

* Developer Session Keys: private keys of these session keys are never stored in in the client nor server. Common use case for this is to register the session key via UX and then use the private key to sign requests in your trading scripts.
* Device Session Keys: created when a user deposits and trades via the UX. To enable instant deposits and trades, a random session private key is generated, registered and encrypted in the browser's local storage. You can manually revoke device session keys below.

You may revoke both developer and device session keys at any time.