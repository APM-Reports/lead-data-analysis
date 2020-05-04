## Buried Lead
Story: LINKTK<br>
Reporter: Lauren Rosenthal, <lrosenthal@apmreports.org><br>
Data Reporter: Will Craft, <wcraft@apmreports.org>

Any questions about the data should be directed to Will Craft.

## What's Here?
APM Reports analyzed thousands of individual lead tests and hundreds of system-wide lead levels from two sources to investigate the impact of relying on first-liter sampling in lead regulations. We gathered state-wide sampling data in Michigan and sampling data from 650 homes in Chicago, IL and found that lead levels are sometimes two to three times higher when water from deeper in the plumbing is tested.

The raw data is in `data/source` and the cleaned data is in `data/processed`. The notebooks used for analysis are in `notebooks`. You can view the Chicago analysis [here](https://nbviewer.jupyter.org/github/APM-Reports/lead-data-analysis/blob/master/notebooks/chicago_data_analysis.ipynb) and the Michigan analysis [here](https://nbviewer.jupyter.org/github/APM-Reports/lead-data-analysis/blob/master/notebooks/MI_LCR_testing_data.ipynb)

## Analysis

In Michigan, a change to state lead regulations required utilities with lead service lines to start collecting two samples, the first and fifth liter, and use the higher of the two when calculating their 90th-percentile lead level.  APM Reports acquired statewide water system 90th-percentile values for 2010-2019 and individual lead samples from 2016-2019 through an open records request.  In our analysis, we focused on utilities that included the fifth liter sample in 2019 to isolate how the change in sampling protocol affected their reported lead levels.

Our investigation found that the average 90th-percentile lead levels were two to three times higher in 2019, when utilities began including the fifth liter, than in any of the previous nine years. This difference was also apparent when looking at individual lead samples. On average, individual fifth-liter samples had two and half times the amount of lead as first liter samples.

In 2016, the Chicago Water Department started an extensive lead testing program as a part of a water quality study, testing the home of any customer that requested it. In the past four years, thousands of customers have requested the city test their water. Homes with elevated lead levels in their first test then received more extensive testing. In 650 homes, the city tested each of the first 10 liters from the tap. We calculated the 90th-percentile value of each liter and found that the highest lead levels were found in the 8th-10th liters.

The data from Chicago also allowed us to test how different lead testing protocols can give different results, without the underlying conditions changing. APM Reports calculated different 90th-percentile values using the current Lead and Copper Rule, Michigan’s Lead and Copper Rule (highest of the first and fifth liter samples), and by calculating the 90th-percentile lead level by taking the highest of each home’s 10 samples. Using the current LCR, the data’s lead levels are 16 ppb, while under the MI LCR, the 90th-percentile value would be 29.5 ppb. Using the highest of all 10 samples at each home tested, the lead levels would be 36.9 ppb.

APM Reports found that first-liter sampling often misses the highest levels of lead. If the utility is consistently missing the highest lead levels, they are operating with limited data and will not be effective at reducing lead at the tap. A lack of accurate lead testing data starts a cascading effect of policies and decisions based on misleading information. Utilities are testing water with the lowest lead levels, which leads to corrosion control that is not optimized to reduce high lead levels. Utilities then reduce the amount of testing they do as they spend time under the action level, which leaves lead lines in the ground for years.

## Data Dictionaries
These are the variables and definitions for the spreadsheets in `data/processed`. All variables are original to the dataset unless otherwise noted.

### Michigan
Data for Michigan was acquired through an open records request filed with the Department of Environment, Great Lakes, and Energy. There are two datasets for Michigan: 90th-percentile lead levels for each water system, covering 2010-2019, and the individual samples gathered per water system, covering 2016-2019.

###### 90th-Percentile Values
File: `data/processed/mi_system_90th_percentiles_2010-2019.csv`
* `id`
  * Water System ID.
* `system_name`
  * Water System name.
* `county`
  * Water System county.
* `system_population`
  * Population served by the water system.
* `last_monitoring_period_end`
  * Date of the last monitoring period for the water system. Water systems have different periods they have to sample by, based on previous lead levels.
* `90th_percentile_ppm`
	 * 90th-percentile lead level, measured in parts per million.
* `90th_percentile`
  * 90th-percentile lead level, measured in parts per billion.
* `monitoring_period_year`
  * The year of the last monitoring period, calculated from `last_monitoring_period_end`. This is an added column.
* `fifth_liter_present`
  * `1` if the system included a 5th liter in their 2019 samples. `0` otherwise. This is an added column based on another dataset, detailed in the analysis notebook for Michigan.

###### Individual Samples
File: `data/processed/mi_system_samples_2016-2019.csv`

* `id`
  * Water system ID.
* `system_name`
  * Water system name.
* `sitecode`
  * sitecode TK
* `lab_id`
  * ID of lab that tested the sample.
* `for_compliance`
  * Was the sample for compliance sampling.
* `collect_date`
  * Date of sample collection.
* `address`
  * Address of sample collection.
* `first_or_fifth`
  * Was the sample first or fifth liter. Blanks mean first liter. The `fifth_liter` column is a standardized version of this column.
* `rule`
  * What chemical was being measured.
* `lead_level`
  * The lead level of the sample. The `lead_level_ppb` is a version of this column, standardized to be parts per billion.
* `unit`
  * The unit of the sampled lead level.
* `accepted_rejected`
  * Whether the sample accepted or rejected. Values are either an `A` for accepted or an `R` for rejected. According to Michigan, "Rejected... does not mean rejected because there was a problem with the sample. Sample marked “rejected” so database would excluded it from the 90th (percentile calculation) because it was not the highest result from the site. Should be accompanied by an “NH” in the next column indicating it was Not Highest result from site"
* `reject_reason`
  * Column used to indicate that sample was not the highest result from sample site.
* `lead_level_ppb`
  * Standardized lead level, measured in parts per billion. This is an added column.
* `fifth_liter`
  * Standardized boolean representing whether the sample is the 1st or 5th liter. This is an added column.  

### Chicago
Data for Chicago was downloaded from [this website](http://chicagowaterquality.org/home#results).<br>
File: `data/processed/chicago_sampling.csv`
* `date_sampled`
  * The date the sample was gathered.
* `address`
  * Partially anonymized address from which samples were gathered.
* `1st_draw` - `10th_draw`
  * Lead concentration of the first liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `2nd_draw`
  * Lead concentration of the second liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `3rd_draw`
  * Lead concentration of the third liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `4th_draw`
  * Lead concentration of the fourth liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `5th_draw`
  * Lead concentration of the fifth liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `6th_draw`
  * Lead concentration of the sixth liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `7th_draw`
  * Lead concentration of the seventh liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `8th_draw`
  * Lead concentration of the eighth liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `9th_draw`
  * Lead concentration of the ninth liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `10th_draw`
  * Lead concentration of the tenth liter, measured in parts per billion. All lead concentrations less than 1 were represented as `<1`, which we cast as `0`.
* `year`
  * The year the sample was gathered. This is an added column, calculated from `date_sampled`.
* `month`
	 * The month the sample was gathered. This is an added column, calculated from `date_sampled`.
* `1st_5th_highest`
  * The value of the 1st or 5th liter, whichever was higher. This is an added column.
* `highest_sample`
  * The value of the 1st-10th liter, whichever was highest. This is an added column.
