# Time-Series
[English below]
## Laiko eilutės
Patalpintas Laiko eilutės (angl. *Time-Series*) kurso projektas, įgyvendintas panaudojant `R Markdown`. Faile `laiko_eiluciu_uzd.html` pateiktas projekto užduoties aprašymas, o `laiko_eiluciu_spr.html` ir `laiko_eiluciu_spr.Rmd` pateiktas užduoties sprendimas `.html` ir `.Rmd` formatu.

### Uždavinių motyvacija
2020 m. akcijų rinkos kritimas prasidėjo 2020 m. vasario 20 d. dėl investuotojų nerimo dėl COVID-19 epidemijos pasekmių. Šie investuotojų lūkesčiai aiškiai atsispindi vertybinių popierių biržose prekiaujamų akcijų uždarymo kainose.

Laikoma, kad akcijų rinkos kritimas tęsėsi iki 2020 m. balandžio 7 d., o po to rinka pradėjo atsigauti. Tačiau galima matyti, kad pastarieji keli metai kai kurioms įmonėms nebuvo sėkmingesni (žiūrint pagal jų akcijų vertę).

Tad iškyla svarbus klausimas: **ar tokie neapibrėžtumo laikotarpiai pasikartos ateityje?**

### Projekto įgyvendinimas
Projekte įgyvendinta:
- Logaritmuotų grąžų vidurkio, standartinio nuokrypio, asimetrijos ir eksceso koeficientų apskaičiavimas;
- Statistinių testų atlikimas;
- Laiko eilutės analizė (ACF, PACF grafikai, Ljung-box testas);
- Modelio sudarymas (vidurkio lygtis) - ARMA modelis ir modelio liekanų analizė;
- Modelio sudarymas vidurkio modelio liekanoms(volatilumo/dispersijos lygtis);
- Volatilumo lygties standartizuotų liekanų analizė;
- Jungtinio modelio sudarymas ir liekanų analizė;
- Volatilumo prognozės (pagal testavimo imties ilgį) naudojantis sudarytu modeliu - ar prognozuojama didesnė, ar mažesnė rizika? Ar volatilumo prognozė suderinama su tuo, kad faktinė grąža pasižymėjo didesniu (arba mažesniu) neapibrėžtumu testavimo imtyje?

-------------
## Time-Series
This is a project for a *Time-Series* course using `R Markdown`. The file `laiko_eiluciu_uzd.html` contains the description of the project task, while `laiko_eiluciu_spr.html` and `laiko_eiluciu_spr.Rmd` contain the solution to the task in `.html` and `.Rmd` formats.

### Motivation
The 2020 stock market crash started on 20 February 2020 due to investor concerns about the consequences of the COVID-19 epidemic. These investor expectations are clearly reflected in the closing prices of shares traded on stock exchanges.

The stock market decline is considered to have lasted until 7 April 2020, after which the market started to recover. However, it can be seen that the last few years have not been more successful for some companies (in terms of their share value).

This raises an important question: **will such periods of uncertainty recur in the future?**

### Project implementation
The project involves:
- Calculation of the mean, standard deviation, asymmetry and excess coefficients of the logarithmic returns;
- Performing statistical tests;
- Time series analysis (partial autocorrelation function (PACF) and the autocorrelation function (ACF), Ljung-box test);
- Building a model (mean equation) - ARMA model and analysis of model residuals;
- Model construction for the residuals of the mean model (volatility/dispersion equation);
- Analysis of the standardised residuals of the volatility equation;
- Joint model building and residue analysis;
- Volatility predictions (based on the length of the test sample) using the constructed model - does it predict more or less risk? Is the volatility forecast compatible with the fact that the actual return had higher (or lower) uncertainty in the test sample?

## Data
To download the data, run the code below:
```r
getData <- function(s_code){
  set.seed(s_code)
  symbols_stocks <- c("NVDA", "INTC", "AMD", "META", "AAPL", "MSFT",
                   "IBM", "NFLX", "GOOGL", "AMZN", "EBAY", "BABA",
                   "DIS", "TSLA", "KO", "PEP", "ADBE",
                   "AAL", "HPQ", "MCD", "NDAQ") 
  my_symbols <- c(sample(symbols_stocks, 1))
  print(paste0("Simboliai: ", sub("\\^", "", my_symbols))) 
  #
  library(quantmod)
  for(var_names in my_symbols){
    # Get the symbol:
    getSymbols(var_names,  auto.assign = TRUE,
               src  = "yahoo", 
               from = "2015-01-01", 
               to   = "2023-06-25")
    # Automatically remove missing data from the variable:
    assign(gsub("\\^", "", var_names), na.omit(get(gsub("\\^", "", var_names))))
  }
  return(get(var_names))
}
DT <- getData(X)
```
To save data, use:
```r
saveRDS(DT, "./laiko_eiluciu_duom.RDS")
```
The data is stored in the file `laiko_eiluciu_duom.RDS`

*Consider that:*

- Time is consistent (i.e. ignore non-working days and periods with empty (`NA`) values),
- ignore exchange rate effects,
- $P_t^{(F)}$ - the closing price of the company's shares at time t

Using the closing prices of the company's shares and stock index symbols, calculate the simple returns. Express the simple returns in logarithmic terms and analyse the logarithmic returns further.

Divide the time series into training (80%) and testing (20%) samples. Build the models for the training sample and predict for the testing sample.
