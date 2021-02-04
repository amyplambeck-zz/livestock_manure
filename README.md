# Livestock Emissions Inventory: Manure Management in the Twin Cities (MN)
This project calculates approximate methane emissions (measured in metric tons of CO2 equivalent) from manure management in the Twin Cities 7 county region at the county scale. Its purpose is to contribute to the Metropolitan Council Research division's growing Greenhouse Gas Inventory. Cattle and other mammalian livestock are a significant source of methane, which has the potential to exacerbate global climate change. This inventory is being put together in order to provide public data on these greenhouse gases -- where they are coming from, and how much. 

## Code
All code is done in R.

## Method

CH4 Emissions (in CO2 equivalent) = Volatile Solids (VS) * Bo * Methane Conversion Factor (MCF) * 0.662 x (1/1000) x Global warming Potential (GWP)

GWP used for this analysis is 25. 

Volatile solids necessary for CH4 emissions estimates derived from

VS (Most livestock) = Animal Population x Typical Animal Mass (TAM) /1000 x VS Rate x 365.25
VS (Most cattle) = Animal Population x VS

The methodology is taken from the 2013 ICLEI protocol, specifically from Appendix G: Agricultural Livestock  Emission Activities and Sources (U.S. Community Protocol for Accounting and Reporting of Greenhouse Gas Emissions), summarized below: 

- *Step 1: Obtain data on animal populations*
- *Step 2: Calculate the amount of volatile solids (VS) produced, with different methods for cattle and non-cattle livestock*
- *Step 3: Estimate methane emissions for each type of manure management system*
- *Step 4: Convert to metric tons CO2e*
- *Step 5: Sum all methane emissions from different livestock*

Emissions factors are the multiplier associated with each head of livestock (each animal). Emissions factors for each waste management system have been sourced from the Inventory of U.S. Greenhouse Gas Emissions and Sinks: 1990-2018 from the EPA.
*(Specifically, Annexes to the Inventory of U.S. GHG Emissions and Sinks, Annex 3.11,  Table A-185 and 186, https://www.epa.gov/sites/production/files/2020-04/documents/us-ghg-inventory-2020-annexes.pdf)*

Farm level data on manure management were provided by the Minnesota Pollution Control Agency (MPCA). 

# Walkthrough 
The .rmd is annotated, but additional information may be helpful. 

## 1: Data (Sorting)

- 1.1: Importing data from Excel sheets
- 1.2: Creating indicators for different types of cattle to prepare for cattle specific methodology later on
- 1.3: Indicators for swine, which only use one manure management system as a general rule
- 1.4: Indicators for poultry, which only use one manure management system as a general rule but have poultry-specific emissions factors
- 1.5: Combining totals for small and large animals to generalize animal populations (EPA conversion factors don't specify size, and typical animal mass accounts for size differences in the sums
- 1.6: Writing fields for the different types (feedlot and not feedlot) of cattle and their subgroups, using indicators created in 1.2
- 1.7: Sorting data overall into poultry, liquid, solid, or both liquid and solid (meaning ambiguous at this stage-- sorted out in part 2) 

## 2: Data (Joining)

- 2.1: Summarizing animal population counts by county
- 2.2: Bringing animal specific conversion factors into county data
- 2.3: There are two components to this step. The first is sorting out the animals that do incorporate weight in their VS calculations (most non-cattle) and the rest that don't (most cattle). The second is tagging them with the daily or annual VS rate that they'll need, since weight calculated VS is on a daily level. 
- 2.4: Revising the ambiguous liquidsolid set by selecting swine out and into the liquid dataset, only after all other joins have been made. 

## 3: Calculations

- 3.1: Calculating annual VS for both categories of animals (differentiated by vsday and vsyear measurement methods) using ICLEI estimation formulas. 
- 3.2: Simple step storing waste manangement emissions factors as values.
- 3.3: Finally estimating methane emissions measured in CO2 equivalents for all single storage methods (not liquidsolid). All values are summarized by county and include only county name, total VS, and emissions. Not the complete estimate as it excludes the ambiguous data. 
- 3.3 (1/2): Step that allows for additional, albeit not necessary, information to be stored instead of the three simple variables from the previous step. Can keep the waste management systems and/or animals distinct. Also excludes the ambiguous data. 
- 3.4: Final step includes the ambiguous data to create an estimated emissions total using the average emissions factor between pasture, solid, and liquid. Also includes the low estimate and the high estimate to create a range of potential estimated values. 
- 3.5: Viewing final table

# Credits
Thanks to Mauricio Leon and Kristen Peterson of Met Council and Sara Isebrand and Angela Hawkins of of MPCA for their contributions. 

