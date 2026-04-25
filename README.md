# Econ148project3
# Sectoral Variation in Google Trends as a Predictor of UI Claims

Final project for Econ 148, Spring 2026.

## Research Question
Does the predictive value of unemployment-related Google search data for 
state-level initial UI claims vary systematically with state labor-market 
structure (tech, energy/manufacturing, tourism)?

## Approach

**Hypothesis** Layoffs are more visible in some state economies than 
others. When a tech company in California cuts staff, it usually shows 
up in WARN filings, news coverage, and social media before workers file 
claims, so people often search online first. In Florida, where a lot of 
separations come from seasonal hospitality work, layoffs are more 
routine and expected, and workers may file without searching beforehand. 
My hypothesis is that Google search activity predicts UI claims more 
strongly in California than in Florida, with Texas somewhere between them.

**Data** I use weekly initial claims data from FRED for the three states, 
picked because they represent different labor-market structures: tech in 
California, energy and manufacturing in Texas, tourism and hospitality 
in Florida. For Google Trends I use six search terms applied identically 
across all three states ("unemployment benefits," "file for unemployment," 
"how to file unemployment", "laid off", "severance", "jobs near me"), 
pulled with pytrends and cached to CSV so the analysis is reproducible 
without re-hitting the API. I left out state-specific terms like EDD 
or TWC on purpose, since differences in how familiar those acronyms could 
influence their search rates.


**Models** The econometric baseline is OLS with lagged claims (1, 2, 4, 
and 8 weeks) and month-of-year dummies, which gives interpretable 
coefficients on the Trends features and a regression table I can 
compare across states. Because the feature panel is wide relative to 
the weekly sample once Trends lags are added, I also run a 
Lasso-regularized version with the same features. Whether Lasso 
retains the Trends coefficients in each state is itself part of the 
answer.

The ML comparison is LightGBM on the same feature panel, so the only 
difference between the linear models and the ML model is the 
functional form.

**What would confirm or refute the hypothesis** If the gain from Trends 
is roughly equal across CA, TX, and FL, the hypothesis is wrong and 
search visibility doesn't vary as much by sector as I'm assuming. If 
the gains line up in the order I expect (CA > TX > FL), that's 
evidence for it. The OLS coefficient magnitudes and significance, the 
Lasso feature-selection pattern, and the LightGBM out-of-sample 
improvement should all tell roughly the same story if the hypothesis 
is right.

**COVID.** This is the obvious complication. Including 2020 means the 
model sees a period where search activity and claims both spiked 
massively, which is informative but could end up driving the entire 
result. I plan to report two specifications: one with a regime dummy 
from March 2020 onward, and one that drops the acute pandemic period 
entirely (2015–2019 plus 2022–2025), and check whether the conclusion 
holds in both.

## Author
Nicolas Goumas - ngoumas@berkeley.edu
