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

**Advanced Details**

Config options to follow to be inline with our current implementation.

```javascript
 //@ts-ignore
      new window.TradingView.widget({
        symbol: this.tradingViewSymbolWithFallback,
        autosize: true,
        theme: this.theme,

        ...(this.isLocal
          ? {
              container: 'tradingview',
              library_path: '/charting_library/',
              datafeed: new IndexDatafeed(
                this.tradingViewSymbolWithFallback,
                this.id,
                this.$config.API_URL,
              ),
              disabled_features: [
                'header_symbol_search',
                'header_settings',
                'header_quick_search',
                'header_compare',
                'go_to_date',
                'header_indicators',
                'create_volume_indicator_by_default',
                'edit_buttons_in_legend',
                'show_hide_button_in_legend',
                'format_button_in_legend',
                'delete_button_in_legend',
                'use_localstorage_for_settings',
                'left_toolbar',
              ],
              custom_css_url: '/charting_library/style.css',
              ...(!this.isLightTheme && {
                overrides: {
                  'paneProperties.background': '#2e2e2e',
                  'paneProperties.backgroundType': 'solid',
                  'paneProperties.horzGridProperties.color': '#333333',
                  'paneProperties.vertGridProperties.color': '#333333',
                  'scalesProperties.axisHighlightColor': 'fefefe',
                  'scalesProperties.lineColor': '#333333',
                  'scalesProperties.textColor': '#fefefe',
                },
                loading_screen: { backgroundColor: '#222' },
              }),
            }
          : {
              gridColor: this.gridColor,
              backgroundColor: this.backgroundColor,
              allow_symbol_change: false,
              interval: '60',
              timezone: 'Etc/UTC',
              style: '3',
              locale: 'en',
              enable_publishing: false,
              hide_volume: true,
              hide_legend: true,
              withdateranges: true,
              container_id: 'tradingview',
            }),
      })
```

**cURL**

#### daily

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://www.theblock.co/api/indices/price-history/277090/1724803200/1725580800/1d
```

#### hourly

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://www.theblock.co/api/indices/price-history/277090/1750493916/1751573916/1h
```


#### minutely

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://www.theblock.co/api/indices/price-history/277090/1751555970/1751556030/1min
```

---

For questions or support, reach out to our developer support team. Happy charting!
