# Concentrated Disadvantage: Violent Deaths of Transgender People 
## Author: Revathi Satkuna

## Problem Statement
- The Office of Refugee Resettlement has selected you, through the nonprofit organization you work at, as the management consultant to advise them on part of process of allocating resources to transgender asylum seekers.  As a transgender individual, this is an issue you deeply care about and one you deal with personally.  Your role is to perform a global analysis of safety, understand what parameters can predict influxes in the levels of violence towards transgender people globally so that resources and services can be allocated to those who need it the most.  Moreover, your company has given you the additional task of understanding how the global scope of the issue can influence implementing policy changes to improve the overall quality of life of transgender persons domestically.
## Datasets
### Original Data
* [`1970-2022`](./Data/original_data): Data taken from tdor that detailed deaths recorded meticulously over the past several years ([source](https://github.com/annajayne/tdor-data)
### Processed Data
* [`tdor.csv`](./Data/processed_data/tdor.csv): Post-compiled raw data and pre-imputing for age
* [`train.csv`](./Data/processed_data/train.csv): tdor.csv after split into training set and imputing based on training data
* [`test.csv`](./Data/processed_data/test.csv): tdor.csv after split into testing set and imputing based on training data
* [`nlp_train.csv`](./Data/processed_data/nlp_train.csv): training text data post lemmatizing and tokenizing for NLP analysis
* [`nlp_test.csv`](./Data/processed_data/nlp_test.csv): testing text data post lemmatizing and tokenizing for NLP analysis
### Population Data
* [`brazil_population.csv`](./Data/population_data/brazil_population.csv): 2022 Population of States of Brazil ([source](https://www.ibge.gov.br/en/statistics/social/population/22836-2020-census-censo4.html?=&t=resultados)
* [`mexico_population.csv`](./Data/population_data/mexico_population.csv): 2022 Population of States of Mexico ([source](https://catalog.data.gov/dataset/georeferenced-population-datasets-of-mexico-geo-mex-gis-of-mexican-states-municipalities-a)
* [`usa_population.csv`](./Data/population_data/usa_population.csv): 2022 Population of States of USA ([source](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html)
* [`country_population.csv`](./Data/population_data/country_population.csv): 2022 Population of World by country ([source](https://www.kaggle.com/datasets/whenamancodes/world-population-live-dataset)
### Geographical Data
* [`brazil_geo.json`](./Data/geodata/brazil_geo.json): Brazil state geodata ([source](https://www.kaggle.com/datasets/thiagobodruk/brazil-geojson)
* [`countries.geojson`](./Data/geodata/countries.geojson): country outlines ([source](https://datahub.io/core/geo-countries)
* [`mexico.json`](./Data/geodata/mexico.json): Mexican states geodata ([source](https://github.com/PhantomInsights/mexico-geojson/blob/main/mexico.zip)



## Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|name|object|tdor|name of deceased transgender person|
|age|float64|tdor|age at which said person passed|
|date|datetime|tdor|date the article was posted about their passing|
|state_province|object|tdor|state or province of country|
|country|object|tdor|country|
|latitude|float64|tdor|latitude|
|longitude|float64|tdor|longitude|
|category|object|tdor|categorized death in six broad categories|
|cause_of_death|object|tdor|summarized death in a few words|
|description|object|tdor|text that detailed how person died or was found|
|year|float64|tdor|year|
|city|object|tdor|town city municipality corresponding to longitude and latitude coordinates|
|violence|int64|tdor|binary classifier classifying death as violent vs nonviolent|
|deaths|int64|tdor|column of "1's" created to count instances for analysis|

## Executive Summary
- Data was collected through the trans day of rememberance website-- the site manager was emailed and access to their github was obtained.  A repo containing the data necessary for the project and then proceeded to clean it into a usable form.  
- Two different models were made and cleaned and preprocessed differently. I visualized the data and uncovered insights that I then displayed visually.
- The two types of models were an NLP analysis where a binary classification was used to predict whether a text description depicted a nonviolent or violent death. From the NLP model, features were extracted to see what most correlated with either. The second model made was time series analysis to predict a trajectory for deaths based on trends of the past. 
- The purpose of performing a Natural Language Processing analysis was to identify whether what traits in of a death most likely correlate a violent one versus a nonviolent one.  In seeing the trends that arise,  a better idea of why things happen and what issues need to be addressed can be obtained.
- To prepare cleaned data for Natural Language Processing, a tokenizer was first utilized on the description column.  A tokenizer is used to split paragraphs and sentences into smaller units that can be more easily assigned meaning.  The particular application in this project was for sentiment analysis.   
- Next, the text column was lemmatized, which is a normalization technique that switches any kind of word to its base root mode.  It involves grouping different inflected forms of words into the root form, having the same meaning.  Lemmatization involves using a vocabulary and morphological analysis of words, removing inflectional endings and returning the dictionary form of a word, the lemma.
- The text data was then processed with a text vectorizer, specifically Term Frequency:Inverse document frequency vectorizor.  It transforms the text into a numerical feature.  Then several models were created to predict a binary outcome, of classifying violent vs nonviolent.  
- Out of which,  logistic regression performed the best.  Had accuracy of .930 for the training data and 0.931 for the testing data.   Used BayesSearchCV with different vectorizers to finetune my models.  It performed better than  the baseline accuracy score of 0.87 which is what the dataset would have predicted for the number of violent deaths without any model.
- The models were visualized by displaying the top positive and negative correlations of the features, corresponding to violent vs nonviolent deaths, respectively. From the nonviolent deaths, suicide, procedure, silicone, covid 19 and depression seem to be the most prevalent features. From the violent deaths, they are gun violence and physical assault.
- Time series is a series of data points that are listed in chronological order.  They are used to track changes of certain things over short and long periods.  Looking at changes in regular intervals over time is important when attempting to use the past to forecast the future.  Knowing exactly how something has changed over time can help you make a more educated guess about what might happen to something over the same time interval in the future.  
- Autocorrelation refers to the degree of similarity between a given time series and a lagged version of itself over successive time intervals.  , it is intended to measure the relationship between the present value and any past values you may have access to.  .
- Autocorrelation helps uncover hidden patterns in data and helps select the correct forecasting methods in that we can use it to help identify seasonality and trend in our time series data.  Analyzing the autocorrelation function (ACF) and partial autocorrelation function (PACF) is necessary for selecting the appropriate ARIMA model for time prediction.
- Autocorrrelation plot is wiggling up and down and since all of them are outside the 95% confidence interval (the blue box) -- they are autocorrelated.  
- Peak of the first season and has statistical significance related to the partial autocorrelation and autocorrelation plots.  On the partial autocorrelation plot, there are two points of statistical significance that sticks out of the 95% confidence interval (the blue box).   That might be seasonality at 4 (SARIMAX means squared value of 13.09)  and 10 (means squared value of 13.20) There is a sort of sinusoidal pattern with dampening. In autocorrelation plot, there is a sinusoidal pattern. The lowest value is selected that looks like a repeat of the previous pattern. This is what a season is–a repetition of patterns. 
- ARIMA plots have lower performance because they do not consider seasonality which the data has.  SARIMA is seasonal ARIMA and is used with time series with seasonality–perform better because they consider seasonality…SARIMAX has a means-squared value of 13.09.

## Considerations
- Not a complete data set by any means.
- Missing a lot of data from many parts of the world, hardly any entries for the entire continent of Africa and many parts of Asia.
- If possible, more information on who is killing trans people could be helpful.
- More information about demographic, education level, income, HIV/STD status, whether or not they were a sex worker, health issues, race/ethnic information could have been helpful too, whether or not they were supported by their families would have helped the analysis.  
- More information on living transgender persons could help build a classifier on whether or not a person is likely to be subject to violence.


## Conclusion
- 4 month seasonality period, seeing pattern repeat every 4 months.
- A vast majority of these deaths occur in Central/South America.
- Deaths are increasing every year (could be due to population of people identifying increasing, or increased reports).
- Disproportionate amount of gun violence.

## Recommendation
- Be aware that January and May are the time periods with the most deaths.  This will require a 1-2 month lead time before to allocate more funding to these programs and prioritize applications by trans asylees from the countries in South/Central America that were highlighted earlier.
- Improving quality of life for these  people once they have entered the US, that they will not have to resort to sex work, blackmarket hormones and illegal silicone injections.
- Mental health counseling and access to healthcare, following all WPATH guidelines. 
- Stronger firearm regulations. 

