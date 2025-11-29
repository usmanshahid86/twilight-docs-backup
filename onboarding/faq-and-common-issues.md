# FAQ & Common Issues

This page covers common questions and troubleshooting tips to help you get started on the Twilight Testnet.\
If you encounter an issue not listed here, please reach out through the official support channel or file an issue on [GitHub](https://github.com/twilight-project/testnets).

***

### 1. Wallet Connection Issues

#### Keplr popup does not appear

If the connection window doesn’t show when you click **Connect Wallet**:

* Disable any active popup blockers for `frontend.twilight.rest`.
* Make sure Keplr is unlocked before attempting to connect.
* Refresh the page and try again.

#### Twilight Testnet not showing in Keplr

If you don’t see _Twilight Testnet_ listed under your networks:

* Reconnect your wallet and approve the **“Add Chain”** prompt again.
* In Keplr, open **Manage Chains → Add/Remove Chain** and search for _Twilight Testnet_.
* Ensure it is toggled on (checked).

#### Wrong account connected

If the wallet address displayed on the site doesn’t match your intended Keplr account:

* Open Keplr, switch to the correct account, and reconnect.
* Avoid connecting multiple Keplr profiles simultaneously in the same browser session.

***

### 2. Faucet & Funding Issues

#### Faucet says “insufficient funds”

* The faucet has request rate limits. Wait **10–15 minutes** before trying again.
* If the faucet is temporarily offline, check the [status page](https://frontend.twilight.rest/status) or monitor announcements.

#### BTC address registration failed

* Ensure you approved the registration transaction in Keplr.
* Avoid refreshing the page while the transaction is being processed.
* Retry registration from the faucet page if it didn’t complete.

#### Didn’t receive NYKS or SATS tokens

* Faucet requests sometimes queue during high usage. Refresh the page and check your wallet after a few minutes.
* If balances still don’t appear, reconnect your wallet and verify on the [Wallet page](https://frontend.twilight.rest/wallet).
* Confirm that the **Twilight Testnet** network is selected in Keplr.

***

### 3. Wallet Display & Balance Issues

#### Balances not updating

* Refresh the **Wallet** page or reconnect Keplr.
* Network latency may delay balance updates for up to 30 seconds.
* If the issue persists, clear cache and relaunch your browser.

#### Incorrect SATS balance after transaction

* Confirm the transaction hash using the **Nyks Explorer** once live.
* If confirmed but not visible, wait a few blocks for indexer sync.
* For persistent mismatches, open a ticket with transaction details.

***

### 4. General Best Practices

* Keep a small buffer of **NYKS** tokens for gas fees to avoid transaction failures.
* Never share your Keplr seed phrase — losing it means permanent loss of wallet access.
* Avoid running multiple wallets or browser extensions that modify RPC endpoints.
* If all else fails, reconnect using an incognito window to rule out cached state issues.

***

### 5. Still Stuck?

If you continue experiencing issues:

* Open an issue on the [Twilight Testnet GitHub repository](https://github.com/twilight-project/testnets/issues).
* Provide:
  * Your wallet address
  * Description of the issue
  * Screenshot or error message
  * Approximate timestamp of the failed transaction

Our team will review your report and respond as soon as possible.

***
