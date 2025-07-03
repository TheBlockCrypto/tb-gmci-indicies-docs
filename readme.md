# TradingView Datafeed API

This API powers the TradingView charts for our indices. It implements the TradingView [Universal Datafeed](https://www.tradingview.com/charting-library-docs/latest/connecting_data/connecting_data.md) (UDF) format, enabling seamless integration with the TradingView Charting Library.

## Authentication

All requests to the Datafeed API require authentication via a Bearer token. Obtain your API key from your account manager, then include it in the `Authorization` header:

```
Authorization: Bearer <API_KEY>
```

## Base URL

```
https://www.theblock.co/api
```

## Endpoints

### Get Historical Bars

Fetch historical price bars for a given index.

```
GET /indices/price-history/:id/:from/:to/:resolution
```

#### Path Parameters
- `id` (integer): The index ID.
- `from` (integer): Start of the time range (UNIX timestamp in seconds).
- `to` (integer): End of the time range (UNIX timestamp in seconds).
- `resolution` (string): Timeframe for bars: `1min`, `1h`, or `1d`.

#### Request Headers
```
Authorization: Bearer <API_KEY>
Accept: application/json
```

#### Response
- **200 OK**
```json
{
  "indexPriceHistory": {
    "hasEarlierData": true,
    "indexPriceHistory": [
      {
        "time": 1686787200000,
        "open": 100.5,
        "high": 102.0,
        "low": 99.5,
        "close": 101.2
      }
      // ...
    ]
  }
}
```
- **Error**
```json
{
  "error": "Error message here"
}
```

## Integration Example

**JavaScript (TradingView UDFCompatibleDatafeed)**
```javascript
import Datafeeds from 'https://charting-library.tradingview-widget.com/datafeeds/udf/dist/bundle.js';

new TradingView.widget({
  symbol: 'INDEX_SYMBOL',
  container_id: 'tradingview',
  library_path: '/charting_library/',
  datafeed: new Datafeeds.UDFCompatibleDatafeed('https://www.theblock.co/api', {
    headers: { 'Authorization': 'Bearer YOUR_API_KEY' }
  }),
  autosize: true,
  interval: '60',
  theme: 'light',
});
```

**cURL**
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://www.theblock.co/api/indices/price-history/277090/1724803200/1725580800/1d
```

---

For questions or support, reach out to our developer support team. Happy charting!
