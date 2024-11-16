# FinReport: Explainable Stock Earnings Forecasting via News Factor Analyzing Model

## Abstract

mine factors and analyze news for decision making!

an automatic system, for ordinary investors to collect information, analyze it, and generate reports after summarizing.

based on financial news announcements and a multi-factors model to ensure the professionalism of the report.

three modules: 

- news factorization module, understanding news information and combine it with stock factors
- return forecasting module, aim to analysis the impact of news on market sentiment
- risk assessment module, to control investment risk

## Introduction

two crucial factors that influence investors' decision-making:

- stock data: refers to numerical data that characterizes stocks in time series. 
- news data: unstructured text that contains complex time-sensitive information 

previous studies disregard the fundamental differences between the continuity and density of stocks and the discontinuity and sparseness of news.

the impact of news on the stock market is subject to "chronological deviation" meaning that news events may produce varying reactions at different timestamps. 

新闻对股市的影响受“时间偏差”的影响，相同的新闻时间在不同时间点可能引发不同的市场反应

three modules: 

- news factorization module, combines semantic role labeling and semantic dependency parsing graph to comprehend news
- return forecasting module, utilized the Fama-French 5-factor model to analyze the sentiment impact of the news
- risk assessment module, employs the EGARCH model to build a VaR risk assessment based on the historical fluctuation information.

以上的信息都会交给大模型来形成一篇report

*如何提供？*

ROI and Sharpe Ratio

## Related Work

### Quantitative Finance

derivatives pricing (衍生品价值) and risk analysis

### News Semantic Understanding

Semantic Role labeling: 语义角色标注，用于分析句子的语义结构，识别句子中的谓词及其相关的语义角色

semantic dependency parsing graph: 使用图的形式来表示句子的语义结构，他解释了句子中歌歌词之间的与语义依赖关系

## Methodology

stock data: includes opening price, closing price, highest price, lowest price and trading volume, with related news.

### New Factorization Module

consider both the overall semantic information and the roles information within the news sentence.

For semantic information, use a pre-trained textual encoder

for roles information, extracted through SRL or SDPG

