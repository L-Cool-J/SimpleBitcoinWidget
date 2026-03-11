# Simple Crypto Widget

Clean, simple cryptocurrency widget for Android.

<a href='https://play.google.com/store/apps/details?id=com.brentpanther.bitcoinwidget&pcampaignid=MKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1'><img alt='Get it on Google Play' src='https://play.google.com/intl/en_us/badges/images/generic/en_badge_web_generic.png' height="80pt"/></a>

<a href="http://fdroid.org/packages/com.brentpanther.bitcoinwidget/">
    <img src="https://f-droid.org/badge/get-it-on.png"
         alt="Get it on F-Droid" height="80">
</a>

## Price API Architecture

Prices are fetched by the `Exchange` enum in `Exchange.kt`. Each exchange implements a `getValue(coin, currency, priceType)` method that returns the current price as a string. The `priceType` can be `SPOT`, `BID`, or `ASK`.

### Direct API Calls

Most exchanges call their own public REST APIs directly via `ExchangeHelper`. For example:

- **Binance** → `https://api.binance.com/api/v3/ticker/...`
- **Coinbase** → `https://api.coinbase.com/v2/prices/...`
- **CoinGecko** → `https://api.coingecko.com/api/v3/simple/price?...`
- **Kraken** → `https://api.kraken.com/0/public/Ticker?...`

Each exchange entry can be checked programmatically:

```kotlin
exchange.isViaRelay   // false for direct API calls
exchange.relayService // null for direct API calls
```

### Relay / Proxy Calls

Some exchanges do not expose a public price API directly. Their prices are fetched through a relay (aggregator) service. You can detect these at runtime using the `isViaRelay` and `relayService` properties on each `Exchange` entry.

#### CriptoYa relay (`https://criptoya.com/api/`)

| Exchange enum | CriptoYa slug |
|---|---|
| `BINANCE_P2P` | `binancep2p` |
| `BITSO_ALPHA` | `bitsoalpha` |
| `FIWIND` | `fiwind` |
| `LEMONCASH` | `lemoncashp2p` |
| `SATOSHI_TANGO` | `satoshitango` |

#### BlinkTrade relay (`https://api.blinktrade.com/api/v1/`)

| Exchange enum |
|---|
| `CHILEBIT` |
| `VBTC` |

### HTTP Layer

All HTTP calls (both direct and relay) go through `ExchangeHelper`, which uses **OkHttp** under the hood and handles:
- JSON parsing via `kotlinx.serialization`
- HTTP 429 rate-limit responses (throws `RateLimitedException`)

## License

This software has an MIT License. All forks of this codebase have my authorization to be released on Google Play and other app stores.

## Donations

Donation can be made by Bitcoin to 194QqhXirp21F5fJVaqviMeHkzZzkZKyi8
