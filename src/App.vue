<template>
<div class="main-wrapper">
  <div class="selectors-container">
    <div class="col">
      <label for="overlays">Overlays:</label>
      <select class="left-select" id="overlays">
        <option value="abands">Acceleration Bands</option>
        <option value="bb">Bollinger Bands</option>
        <option value="dema">DEMA (Double Exponential Moving Average)</option>
        <option value="ema">EMA (Exponential Moving Average)</option>
        <option value="ikh">Ichimoku Kinko Hyo</option>
        <option value="keltnerchannels">Keltner Channels</option>
        <option value="linearRegression">Linear Regression</option>
        <option value="pivotpoints">Pivot Points</option>
        <option value="pc" selected="selected">Price Channel</option>
        <option value="priceenvelopes">Price Envelopes</option>
        <option value="psar">PSAR (Parabolic SAR)</option>
        <option value="sma">SMA (Simple Moving Average)</option>
        <option value="supertrend">Super Trend</option>
        <option value="tema">TEMA (Triple Exponential Moving Average)</option>
        <option value="vbp">VbP (Volume by Price)</option>
        <option value="vwap">WMA (Weighted Moving Average)</option>
        <option value="wma">VWAP (Volume Weighted Average Price)</option>
        <option value="zigzag">Zig Zag</option>
      </select>
    </div>
    <div class="col">
      <label for="oscillators">Oscillators:</label>
      <select class="right-select" id="oscillators">
        <option value="apo">Absolute price indicator</option>
        <option value="ad">A/D (Accumulation/Distribution)</option>
        <option value="aroon">Aroon</option>
        <option value="aroonoscillator">Aroon oscillator</option>
        <option value="atr">ATR (Average True Range)</option>
        <option value="ao">Awesome oscillator</option>
        <option value="cci">CCI (Commodity Channel Index)</option>
        <option value="chaikin">Chaikin</option>
        <option value="cmf">CMF (Chaikin Money Flow)</option>
        <option value="disparityindex">Disparity Index</option>
        <option value="cmo">CMO (Chande Momentum Oscillator)</option>
        <option value="dmi">DMI (Directional Movement Index)</option>
        <option value="dpo">Detrended price</option>
        <option value="linearRegressionAngle">Linear Regression Angle</option>
        <option value="linearRegressionIntercept">Linear Regression Intercept</option>
        <option value="linearRegressionSlope">Linear Regression Slope</option>
        <option value="klinger">Klinger Oscillator</option>
        <option value="macd" selected="selected">MACD (Moving Average Convergence Divergence)</option>
        <option value="mfi">MFI (Money Flow Index)</option>
        <option value="momentum">Momentum</option>
        <option value="natr">NATR (Normalized Average True Range)</option>
        <option value="obv">OBV (On-Balance Volume)</option>
        <option value="ppo">Percentage Price oscillator</option>
        <option value="roc">RoC (Rate of Change)</option>
        <option value="rsi">RSI (Relative Strength Index)</option>
        <option value="slowstochastic">Slow Stochastic</option>
        <option value="stochastic">Stochastic</option>
        <option value="trix">TRIX</option>
        <option value="williamsr">Williams %R</option>
      </select>
    </div>
  </div>
  <div>
  <highcharts :constructor-type="'stockChart'" :options="chartOptions"></highcharts>
  </div>
</div>
</template>

<script>
import {Chart} from 'highcharts-vue'
import stockInit from 'highcharts/modules/stock'
import Highcharts from 'highcharts'

stockInit(Highcharts)

export default {
  components: {
     highcharts: Chart
  },
  async  mounted(){
   const response = await fetch('https://demo-live-data.highcharts.com/aapl-ohlcv.json');
      const json = await response.json();
      //  let volume = []
       let dataLength = json.length;
    for (var i = 0; i < dataLength; i += 1) {
        this.ohcl.push([
            json[i][0], // the date
            json[i][1], // open
            json[i][2], // high
            json[i][3], // low
            json[i][4] // close
        ]);

        this.volume.push([
            json[i][0], // the date
            json[i][5] // the volume
        ]);
  }
  this.chartOptions.series[0]['data'] = this.ohcl
  this.chartOptions.series[1]['data'] = this.volume
  console.log(this.ohcl)
  },
  data() {
    return {
      ohcl: [],
      volume: [],
      chartOptions: {
         chart: {
          type: 'candlestick',
           height: '100%'
        },
        title: {
          text: 'Candlestick and Volume Chart'
        },
        xAxis: {
          type: 'datetime'
        },
        yAxis: [{
          title: {
            text: 'Price'
          }
        }, {
          opposite: true,
          title: {
            text: 'Volume'
          }
        }],
        series: [{
          name: 'Price',
          // data: this.ohcl,
          type: 'candlestick'
        }, {
          name: 'Volume',
          // data: this.volume,
          yAxis: 1,
          type: 'column'
        }]
      }
    }
  }
}
</script>

<style>
.selectors-container {
    background: #f2f2f2;
    margin-bottom: 1rem;
    font-size: 0;
}

.selectors-container .col {
    font-size: 1.2rem;
    width: calc(50% - 1em);
    padding: 0.5em;
    display: inline-block;
}

.selectors-container select {
    width: 100%;
    font-size: 16px; /* prevent page zoom in iOS */
}

@media (max-width: 768px) {
    .selectors-container .col {
        display: block;
        width: calc(100% - 1em);
    }
}
</style>