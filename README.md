# Crypto Lens System Architecture

## Overview

Crypto Lens is a React Native + Expo mobile application that provides users with real-time cryptocurrency market information, coin details, live charts, market news, and app settings. The app is organized into reusable screens, components, and service layers so the UI remains clean and the data logic stays separated.

## High-Level Structure

The app follows a layered architecture:

- **UI Layer**: Screens and reusable components
- **Service Layer**: API calls and data fetching logic
- **State/Logic Layer**: Screen state, chart range handling, search filtering, and theme handling
- **Navigation Layer**: Screen routing and modal transitions

## File Architecture

Crypto-Lens/
├── App.js
├── app.json
├── eas.json
├── package.json
├��─ assets/
│   ├── favicon.png
│   ├── splash.png
│   ├── logo.png
│   └── icons/
├── src/
│   ├── screens/
│   │   ├── HomeScreen.js
│   │   ├── MarketScreen.js
│   │   ├── MarketNewsScreen.js
│   │   ├── SettingsScreen.js
│   │   └── ArticleScreen.js
│   ├── components/
│   │   ├── CoinCard.js
│   │   ├── CoinChart.js
│   │   ├── NewsCard.js
│   │   ├── SentimentCard.js
│   │   ├── LoadingSkeleton.js
│   │   ├── ErrorCard.js
│   │   └── EmptyState.js
│   ├── services/
│   │   ├── cryptoService.js
│   │   ├── newsService.js
│   │   ├── binanceService.js
│   │   ├── coinGeckoChartService.js
│   │   ├── pairMap.js
│   │   ├── retryFetch.js
│   │   └── cacheService.js
│   ├── navigation/
│   │   └── AppNavigator.js
│   ├── theme/
│   │   ├── colors.js
│   │   ├── spacing.js
│   │   ├── typography.js
│   │   └── index.js
│   └── utils/
│       ├── formatters.js
│       ├── date.js
│       └── helpers.js
└── README.md
### Root Files
- `App.js`
  - Entry point of the application
  - Handles splash flow and app initialization
  - Sets up navigation and theme loading

### Screens
- `HomeScreen.js`
  - Main dashboard
  - Displays watchlist, movers, and news
- `MarketScreen.js`
  - Full coin list
  - Search and filter behavior
  - Coin detail modal logic
- `MarketNewsScreen.js`
  - News feed screen
  - Opens articles
- `SettingsScreen.js`
  - Theme and app settings

### Components
- `CoinCard.js`
  - Coin display card for lists and watchlists
- `CoinChart.js`
  - Chart renderer for historical and live data
- `NewsCard.js`
  - News preview card
- `SentimentCard.js`
  - Market sentiment display card

### Services
- `services/cryptoService.js`
  - Fetches coin list and pricing data
- `services/newsService.js`
  - Fetches crypto news
- `services/binanceService.js`
  - Fetches chart candles and live chart data
- `services/pairMap.js`
  - Maps app coin IDs to Binance trading pairs
- `services/retryFetch.js`
  - Adds retry logic to network calls

## Main Screens

### 1. Splash Screen
**File:** `App.js`

Purpose:
- Display brand logo and app name on startup
- Load initial data and user settings
- Transition to the main app after initialization

### 2. Home Screen
**File:** `HomeScreen.js`

Purpose:
- Show dashboard overview
- Display top coins, trending coins, and recent news
- Provide quick access to Market, Market News, and Settings

### 3. Market Screen
**File:** `MarketScreen.js`

Purpose:
- Display the full list of coins
- Handle search, filtering, and sorting
- Open the coin detail modal when a coin is tapped

### 4. Coin Detail Modal
**File:** `MarketScreen.js` and `CoinChart.js`

Purpose:
- Show coin-specific details such as price, rank, 24h change, market cap, and chart
- Display timeframe buttons such as 24H, 7D, 14D, and 1M
- Show loading and error states for chart data

### 5. Market News Screen
**File:** `MarketNewsScreen.js`

Purpose:
- Display latest crypto-related news
- Open articles in an article viewer or browser

### 6. Settings Screen
**File:** `SettingsScreen.js`

Purpose:
- Manage app preferences
- Toggle theme
- Adjust refresh options
- Clear cache and debug options

## Component Mapping

### CoinCard
**File:** `CoinCard.js`

Used to display:
- Coin name
- Symbol
- Price
- Percentage change
- Mini trend indicator or sparkline

### CoinChart
**File:** `CoinChart.js`

Used to render:
- Historical chart data
- Live or near-live price trend visualization
- Up/down visual indicator based on price movement

### NewsCard
**File:** `NewsCard.js`

Used to display:
- News title
- Source
- Timestamp
- Short article summary

### SentimentCard
**File:** `SentimentCard.js`

Used to display:
- Sentiment score
- Market mood
- Sentiment trend information

## Service Layer

### `services/cryptoService.js`
Handles:
- Coin list
- Price data
- Market statistics

### `services/newsService.js`
Handles:
- News feed fetching
- Article data formatting

### `services/binanceService.js`
Handles:
- Chart data retrieval
- Live candle data
- Binance API integration

### `services/pairMap.js`
Handles:
- Mapping app coin IDs to Binance trading pairs
- Example:
  - `bitcoin` → `BTCUSDT`
  - `ethereum` → `ETHUSDT`

### `services/retryFetch.js`
Handles:
- Retry logic for network requests
- Error recovery for unstable connections

## Data Flow

1. App launches through `App.js`
2. Splash screen loads preferences and initial cached data
3. Home screen displays summary data
4. Market screen fetches coin list and supports search
5. User taps a coin
6. Coin detail modal opens
7. Chart data is fetched from the chart API service
8. CoinChart renders the line or candle chart
9. User can switch chart ranges
10. News items open into article content
11. Settings updates theme and behavior preferences

## Design Goals

The system is designed to be:
- Modular
- Easy to maintain
- Easy to extend with new API sources
- Mobile-friendly and responsive
- Compatible with light and dark themes

## Reflection

Crypto Lens was built to solve the problem of making cryptocurrency information easier to access in one mobile dashboard. Instead of forcing users to open multiple apps or websites, Crypto Lens brings together coin prices, market movement, live chart data, news, and app settings into a single interface. The app is especially useful for users who want a quick overview of the crypto market and a more detailed view of individual coins without leaving the app.

The API used for the chart feature was Binance public market data, while other parts of the app relied on crypto and news services for coin listings and market-related content. The chart integration was one of the most challenging parts of the project because it required matching the app’s coin data with the correct trading pairs, handling range changes, and making sure the chart UI remained responsive when network requests failed or returned limited data. Another difficult part was designing the coin detail flow so that it felt smooth inside a modal while still showing enough information to be useful.

If I had more time, I would improve the app by adding more advanced chart interactions such as zooming, crosshair tooltips, and candle mode. I would also improve offline support by caching more market data locally and making the UI more resilient when the network is unstable. Another improvement would be adding more personalization features such as custom watchlists, alerts, and saved coin comparisons. Overall, the project demonstrates how APIs, reusable UI components, and clear navigation can be combined to create a useful crypto tracking experience.
