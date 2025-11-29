# Relayer API

The Relayer API provides programmatic access to the Twilight Exchange, including market data, account operations, and real-time streams.\
It consists of three main components:

* Public API — for general market data (no authentication required).
* Private API — for authenticated trading and account operations.
* WebSocket Streams — for live price and market updates.

This guide outlines how to connect, query, and subscribe using each of these interfaces.\
For full endpoint specifications and live code examples, visit the [Twilight API Reference](https://docs.twilight.rest/).

***

## Public API

The Public API provides access to market data without requiring authentication.\
These endpoints are ideal for fetching order books, tickers, trades, and other publicly available exchange data.

### Base URL

```
https://api.twilight.rest/api/v1/
```

### Common Endpoints

| Endpoint                    | Description                                   |
| --------------------------- | --------------------------------------------- |
| `/markets`                  | List available trading pairs.                 |
| `/ticker?symbol=BTCUSDT`    | Latest ticker information for a trading pair. |
| `/orderbook?symbol=BTCUSDT` | Retrieve the current order book snapshot.     |
| `/trades?symbol=BTCUSDT`    | Recent trades for a given symbol.             |

### Example Request (Shell)

{% code title="curl" %}
```bash
curl https://api.twilight.rest/api/v1/ticker?symbol=BTCUSDT
```
{% endcode %}

### Example Response

{% code title="response.json" %}
```json
{
  "symbol": "BTCUSDT",
  "price": "67543.25",
  "volume": "182.34",
  "timestamp": 1729912034000
}
```
{% endcode %}

Use the public API for lightweight integrations such as dashboards, analytics, or bots that don’t require authentication.

***

## Private API

The Private API allows users to execute trades, manage orders, and query their account balances.\
Authentication is required using your Twilight API key and secret.

### Authentication

All private requests must be signed using your API key. Include headers:

```
X-API-KEY: <your-key>
X-API-SIGNATURE: <sha256-signature>
X-API-TIMESTAMP: <unix-timestamp>
```

### Common Endpoints

| Endpoint     | Description                          |
| ------------ | ------------------------------------ |
| `/order`     | Place a new order (limit or market). |
| `/cancel`    | Cancel an existing order.            |
| `/balance`   | Retrieve wallet balances.            |
| `/positions` | View open or closed positions.       |

### Example Request (POST /order)

{% code title="place-order.sh" %}
```bash
curl -X POST https://api.twilight.rest/api/v1/order \
  -H "Content-Type: application/json" \
  -H "X-API-KEY: your-key" \
  -H "X-API-SIGNATURE: your-signature" \
  -d '{
        "symbol": "BTCUSDT",
        "side": "buy",
        "type": "limit",
        "price": "65000",
        "size": "0.1"
      }'
```
{% endcode %}

### Example Response

{% code title="response.json" %}
```json
{
  "orderId": "1234567890",
  "status": "placed",
  "symbol": "BTCUSDT",
  "price": "65000",
  "size": "0.1"
}
```
{% endcode %}

For details on signing, authentication flow, and error codes, see the [Private API Reference](https://docs.twilight.rest/_private).

***

## WebSocket Streams

For real-time updates, Twilight provides WebSocket streams for both public market data and authenticated account notifications.

### WebSocket URL

```
wss://stream.twilight.rest/ws/
```

### Available Streams

| Stream       | Description                                        |
| ------------ | -------------------------------------------------- |
| `@ticker`    | Live ticker updates for all markets.               |
| `@trades`    | Real-time trade events.                            |
| `@orderbook` | Incremental order book updates.                    |
| `@account`   | Account balance and order updates (authenticated). |

### Example Connection

{% code title="ws-example.js" %}
```javascript
const ws = new WebSocket("wss://stream.twilight.rest/ws/ticker@BTCUSDT");

ws.onmessage = (msg) => {
  console.log(JSON.parse(msg.data));
};
```
{% endcode %}

### Example Response

{% code title="ws-response.json" %}
```json
{
  "stream": "ticker@BTCUSDT",
  "data": {
    "price": "67550.80",
    "volume": "5.23",
    "timestamp": 1729912035000
  }
}
```
{% endcode %}

WebSocket streams are the preferred way to monitor markets and active positions with minimal latency.

***

{% hint style="info" %}
Best Practices

* Use REST endpoints for snapshots and WebSockets for live updates.
* Implement rate-limiting and reconnect logic in your client.
* Validate all signatures and timestamps on Private API requests.
* Keep your API keys secure — never expose them in front-end code.
* For heavy-load trading bots, consider local caching of order books.
{% endhint %}

***

## Additional Resources

<details>

<summary>Links</summary>

* [Public API Reference](https://docs.twilight.rest/_public)
* [Private API Reference](https://docs.twilight.rest/_private)
* [WebSocket Documentation](https://docs.twilight.rest/_websocket)
* [SDK Integration](/broken/pages/786efcc79382cd8f6d26475311cea9ea143e19bf)

</details>

_Last updated: November 2025_
