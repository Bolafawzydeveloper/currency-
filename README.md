
This is the source of the Crypto currency trading bot running on: https://wolfbot.org
## Features
* **Trading**: buying + selling, portfolio management (sync balances with exchanges)
* **Margin Trading**: leveraged trading including short selling and futures trading
* **Arbitrage**: profit from price differences between 2 exchanges (done "on the books" with balances on both exchanges, no withdrawals from exchanges required)
* **Lending**: lend your coins on the lending market of supported crypto exchanges for the highest possible interest rates
* **Backtesting**: test your trade strategies in simulation on historical data
* **Web Plugins**: [Access social media data](https://wolfbot.org/social-sentiment/) from Twitter, Reddit, Telegram Channels, RSS Feeds,... to trade based on news and real world events (not part of open source version yet)


### Key trading advantages
* **Indicators**: Over 200 technical indicators (MACD, RSI, EMA,..), candlestick pattern recognition (Doji, Tri-Star,...) of 20+ most common patterns, Fibonacci Retracements (upwards & downwards), automatic trendline (support & resistance) detection
* **Strategy events**: All strategies emit buy/sell events that can be forwarded to other strategies using different candle sizes before trade execution. This gives you the ability to easily configure your bot to “zoom in” on candlestick chart data (for example from 12h candles trendline to 1h MACD to 10min RSI).
* **Realtime**: Trades come in realtime via websocket connection from the exchange. Strategies can process (and react) on every single trade. Furthermore all indicators can be updated within the current (latest) candle as new data comes in.
* **Backtesting**: Advanced automatic parameter optimization using:
    * the Cartesian product to try all permutations of given config parameters or
    * a genetic algorithm to find the most profitable config parameters within given parameter ranges 


Screenshots of the Trading UI:

[Trading Chart](https://wolfbot.org/wp-content/uploads/2018/06/trading-1024x605.png)

[Live Strategy data](https://wolfbot.org/wp-content/uploads/2018/06/trading_strat-1024x564.png)

[Strategy configuration](https://wolfbot.org/wp-content/uploads/2018/06/config_strat-1024x606.png)

A more detailed list of all features: https://wolfbot.org/features/

The list of supported exchanges: https://wolfbot.org/features/exchanges/

Additionally WolfBot supports over 130 exchanges using [CCXT Library](https://github.com/ccxt/ccxt) (no WebSockets, no margin trading).

The full strategy documentation: https://wolfbot.org/strategy-docs/

## Getting Started

### Requirements
```
NodeJS >= 12 && <= 14
MongoDB >= 4.0
TypeScript >= 3.5
yarn >= 1.9.4 (npm should work too, but no support given if you run int
### Installation
### Start trading


After running TypeScript (automatically in your IDE or run the `tsc` command in the project root dir) you will see a file:
```
build/app.js
```
`tsc` will show you some errors (due to shared code with missing types between server and client side, I will refactor this later). Just ignore these errors and make sure `noEmitOnError` is not set (the default) and that you have a `build/` dir in the project root as well as for all packages under `node_modules/@ekliptor` which contain a `tsconfig.json` file. Use the `build` directory as the working directory and run:
```
node app.js --debug --config=Noop --trader=RealTimeTrader --noUpdate --noBrowser
```
The `config` parameter must be a JSON file from the `config` directory. For a list of all parameters look at the top of the `app.ts` file.

### Docker support
To you can install WolfBot with all its dependencies using Docker:
```
docker-compose up
```


### Writing your own trading strategies
There is documentation available here: https://forum.wolfbot.org/forums/strategy-development.17/

Look into the `/src/Strategies` folder for more examples.

[Incoming trades flow diagram](https://raw.githubusercontent.com/Ekliptor/WolfBot/master/tests/docs/diagram-incoming.png)
[Outgoing trades flow diagram](https://raw.githubusercontent.com/Ekliptor/WolfBot/master/tests/docs/diagram-outgoing.png)



### Modifying the UI
In the project root directory, run:
```
yarn watch
```

You should also run the bot with the `--uiDev` flag so that changes to HTML template files are reloaded from disk.

The UI uses a single persistent WebSocket connection. UI related code is in the following directories:
```
├ project root
├─── public <- all code shipped to the browser
├────── js <- all TypeScript code to be compiled with WebPack
├─── src
├────── WebSocket <- all server-side logic to push updates to the browser
├─── views <- all HTML templates
```


## REST and WebSocket JSON APIs
If you want to connect WolfBot to your trading terminal, please take a look at the [API docs](https://github.com/Ekliptor/WolfBot/blob/master/tests/docs/WolfBot%20API.pdf).

This let's you:
* create new cloud bot instances and earn commission for every referral (not applicable to open source version)
* import the trades book with all trades WolfBot made via REST API call
* subscribe to live trades via WebSocket 
