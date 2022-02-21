# Predict West Nile Virus
 Darren, Hong Aik, Mun Yee, Sean, Yu Kiat
 
 Problem Statement
 ---
The leading cause of mosquito-borne disease in the United States is the West Nile Virus (WNV). It is most commonly spread to humans through the bite of infected mosquitos. Around 1 in 5 people who get infected with the virus develop a fever, and other symptoms. Around 1 in 150 people develop more serious symptoms which could be fatal. ([CDC](https://www.cdc.gov/westnile/index.html))

In 2002, the first human cases of West Nile virus were reported in Chicago. By 2004 the City of Chicago and the Chicago Department of Public Health (CDPH) had established a comprehensive surveillance and control program that is still in effect today. Every week from late spring through the fall, mosquitos in traps across the city are tested for the virus. The results of these tests influence when and where the city will spray airborne pesticides to control adult mosquito populations. ([Kaggle](https://www.kaggle.com/c/predict-west-nile-virus/))

We are given the weather, location, testing, and spraying data from the CDPH, which we will use to predict the presence of the West Nile Virus. Following te predictions, we will decide on where and when to deploy pesticides in the city, to maximise cost performance.
 
 Data
 ---
The data is from a [Kaggle Competition](https://www.kaggle.com/c/predict-west-nile-virus/)
| File        | Description                                                                                         |
|-------------|-----------------------------------------------------------------------------------------------------|
| train.csv   | Training set of the main dataset. The training set consists of data from 2007, 2009, 2011, and 2013 |
| test.csv    | Test set of the main data set. The test set consists of data from 2008, 2010, 2012, and 2014.       |
| spray.csv   | GIS data of spraying efforts in 2011 and 2013                                                       |
| weather.csv | Weather data from 2007 to 2014. Column descriptions in noaa_weather_qclcd_documentation.pdf.        |

 
 Model Scores
 ---
| Model                         | Train ROC AUC score | Test ROC AUC score |
|-------------------------------|---------------------|--------------------|
| MLP Classifier                | 0.897               | 0.875              |
| Logistic Regression           | 0.815               | 0.794              |
| Random Forest                 | 0.892               | 0.867              |
| AdaBoost Classifier (untuned) | 0.898               | 0.884              |
| AdaBoost Classifier (tuned)   | 0.916               | 0.884              |

 Cost Benefit Analysis
 ---
 The CDPH utilises mosquito adulticides, specifically Zenivex (Zenivex E20 costs $0.67 per acre ([Zenivex Price Brochure](../main/assets/2015-Zenivex-Pricing-Brochure.pdf))), to reduce the occurrence of the WNV. The spray is implemented at dusk and ends approximately 1 hour past midnight ([source](https://www.chicago.gov/city/en/depts/cdph/provdrs/healthy_communities/news/2020/august/city-to-spray-insecticide-thursday-to-kill-mosquitoes0.html)). 

With the model, we can conduct targeted spraying, and spray only at the trap locations that the model has predicted to have WNV. As the Culex mosquitoes are known to fly up to 1.33 km (1373 acres) ([source](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3278816/)), it would cost \$919.91 to spray at each trap to cover 1373 acres. Spraying all traps (92) that were predicted WNV-present would cost \$89,231.27. Since each spray lasts about 30 days ([source](https://www.callnorthwest.com/2019/05/how-long-does-a-mosquito-treatment-last/)), and we will only need to spray during the peak months, i.e. June-October as predicted by the model, it will cost \$446,156.35 annually.

The benefits of conducting the spray include: lowering the number of mosquitoes in the city, which would also reduce the occurrence of other diseases, such as Zika and Malaria and lowering the amount of medical and hospitalisation fees paid by the citizens of Chicago.

Should spraying not be conducted, while the city would save \$446,156 annually, the cost of making such a decision is substantially greater. If a person is infected by the WNV, they may develop serious symptoms which could result in hospitalisation and treatment, which would in turn result in lost productivity. It is estimated that the WNV would cost about \$60,000,000 a year in medical fees and lost productivity ([source](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3945683/)).

 Conclusion
 ---
 Based on our EDA and modeling, we uncovered the following pointers:
- The probability of detecting WNV in mosquitos is correlated to the number of mosquitos present. In other words, if there are more mosquitos, the risk of WNV transmission is higher.
- WNV-positive mosquitos are more prevalent in Jul - Sept; this corroborates evidence found online.
- The increase in prevalence during these months also correlate with higher temperatures and humidity.
- WNV density - the frequency which a trap tests positive - is the strongest indicator of whether WNV is present, followed by weather conditions

Due to the limitations of the dataset, there are many factors which were not taken into account:
- Time lag - all data across years were aggregated together when in fact WNV transmissions may not be uniform across years.
- There could be other areas not under surveillance and hence not predicted upon - we might have missed hot spots to conduct sprays in.
- Amount of training data was limited for certain months and locations.

 Recommendations
 ---
We recommend the City of Chicago to carry out sprays in this fashion:
- All trap locations which are predicted to be positive.
- Conducting sprays on a monthly basis from Jun - Oct, the period where WNV is at its peak.
- Spray frequency could even be increased in areas predicted to be "hot spots".
- Since temperature and humidity are important indicators of WNV transmission, officials should monitor the weather round the year and look out for seasons of high temperature and humidity and conduct sprays accordingly, not only in months of Jun-Oct. This is particularly pertinent given that the climate is changing and WNV peak seasons may shift to other months.
