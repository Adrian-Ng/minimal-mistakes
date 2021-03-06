---
title: "Historical Simulation"
permalink: /java/var/historical/
excerpt: "Estimating Value at Risk via Historical Simulation"
toc: true
mathjax: true
classes: wide
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/headers/historical.jpg
  caption: "Skaftafellsjökull"  
  actions:
    - label: "Download"
      url: "https://github.com/Adrian-Ng/ValueAtRisk"  
---

Here we make no probabilistic assumptions. 
Our view is that what happened in the past is a guide to what will happen in the future.
Tomorrow's price change will be sampled from the distribution of price changes in our historical data.

First we must value our portfolio using today's stock prices $$S_{today}$$.
Next, we must calculate a vector of [percentage changes](https://adrian.ng/java/var/intro/#percentagechange).

For each of these percentage changes $$\Delta S_i$$, we compute the difference between the current value of our portfolio and the future value our portfolio to build a sample of changes $$\Delta\Pi$$.

We then sort $$\Delta\Pi$$ in order of largest to smallest, positive to negative.
If, for example, our confidence level is $$c=99\%$$, our estimate of VaR occurs at the cut-off point in $$\Delta\Pi$$ at $$99\%$$.
$$99\%$$ of 1000 samples is 990, so we take $$\Delta\Pi_{990}\sqrt{\Delta t}$$, where $$\Delta t$$ is the time horizon.


## Algorithm

1. Value today's portfolio from $$S_{today}$$
2. for each asset:
   * Calculate daily returns $$\Delta S_i$$ from historical data
   * Apply all $$\Delta S_i$$ to $$S_{today}$$
3. Value for $$\Pi^{tomorrow}$$
4. $$\Delta\Pi = \Pi^{tomorrow} - \Pi^{today}$$
5. Sort $$\Delta\Pi$$ ascending
6. $$\mathit{VaR} \leftarrow \Delta\Pi_{99\%}\sqrt{\Delta t}$$

## Implementation



## Sampling tomorrow's portfolio prices

```java
try {
    for (String sym : strSymbols) {
        Stock stock = stockHashMap.get(sym);
        int stockDeltas = hashStockDeltas.get(sym);
        double currentPrice = stock.getQuote().getPreviousClose().doubleValue();
        double[] percentageChanges = PercentageChange.getArray(stock.getHistory());
        for(int i = 0; i < percentageChanges.length; i++)
            tomorrowPortfolio[i] += (percentageChanges[i] + 1) * currentPrice * stockDeltas;
    }
} catch (Exception e) {
    e.printStackTrace();
}
```

## Estimating VaR

```java
Collections.sort(tomorrowPortfolio);
double index = (1 - Confidence) * size;
double VaR = (currentPortfolio - tomorrowPortfolio.get((int) index)) * TimeHorizon;
return VaR;
```


## Output

```
	Historical Simulation
		VaR: 3383.974036
```