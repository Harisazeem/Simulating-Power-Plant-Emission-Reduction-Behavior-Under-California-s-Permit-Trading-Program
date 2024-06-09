# Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-Permit-Trading-Program
This project provides an analysis of California's fossil-fueled power plants and their response to different emission policy and elasticity scenarios. It employs behavioral simulations, under cap-&-trade and command-&-control policies, which help understand the emission reduction behavior of individual power plants across census tracts—including those hosting Distadvantaged Communities (DACs)—in California.

# Methodology and Data

The data used in the analysis comes from multiple different sources. Plant-level data is sourced from EPA's Emissions & Generation Resource Integrated Database. Census tract information and ACS demographic data for California's census tracts have been sourced from census burea web interface. Finally, DAC scores—used in the tableau dashboard—are sourced from Cal Enviro SB 535 database. 

Firstly, plant level data—which includes plant specific information, such as geolocation, annual emissions (tons), and heat rates—is filtered to California's census tract level. Only fossil-fueld power plants, running on Gas, Oil, and Coal, are included for emission reduction scenarios. Secondly, this plant-level data is merged with census tracts and demographic information for exploratory data analysis. Thirdly, market demand and supply functions for emission reduction are formulated to simulate how individual power plants would respond to different policy scenarios under permit trading and command & control policies. Fourthly, the response of power plants is mapped on California's census tract level to see how individual power plants—differentiated through their annual emissions, percentage reduction in emission levels, and tract-based location—would respond to a given scenarion within a specific community (with its own DAC score). Finally, the power plant behavior, and its potential impact on DACs through emission reduction, is displayed on an interactive Tableau dashboard for public use.

QGIS, geopandas, Tableau, and Python's folium module are used for mapping purposes throughout this analysis.

# Policy and Elasticity Scenarios

* Elasticity Scenarios: We run the simulations for three elasticity scenarios:

    1. Bigger polluters (those emitting more than 250k tons of Co2) have a higher elasticity of demand, and are therefore more responsive to the permit market demand curve, than smaller ones. This scenario also assumes that bigger polluters, with higher elasticities, have lower Marginal Abatement Cost curves (MACs). Under this assumption, a lower MAC translates into a higher elasticity because it is relatively less costly for the bigger polluter to reduce its own emissions. 

    2. Bigger polluters have a lower elasticity of demand, and are therefore more responsive to changes in the permit market demand curve, than smaller ones. This scenario assumes that bigger polluters, with lower elasticities, have higher Marginal Abatement Cost curves (MACs).

    3. All carbon-emitting power plants have the same elasticities of demand, and are thus equally responsive to price changes in the permit market. All polluters have the same MACs. 

*We use constant elasticity of demand function for the individual powerplants— which is then aggregated to find the market demand—as a stylized representation of individual permit trading behaviors under different elasticities*


* Policy Scenarios: The simulation is run for eight market-level reduction targets ranging from 20% to 90% emission reduction in the state. Users can choose the market reduction target to see how much individual power plants would abate emissions in response to the policy intervention given their specific elasticities/MACs. 


# Python Scripts

1. [Filtering egrid.py]() filters national level data on plants and generators by fossil-fueled power plants in California. 

2. [Mapping plant-level data.py]() adds information on unused capacity and total possible annual generation for each power plant in California. It then reads and filters census files. It tests the spread of the power plant point location and imports demographic information for California's census tracts using an API call. 

3. [Merges Plots.py filters](), spatially joins, and groups data on power plants by census tracts and their demographic information. It then visualized some of the information for exploratory data analysis. Data visualized here includes top 10 census tracts with highest number of power plants, highest emission levels, highest annual generation potential, and a correlation matrix for power plant and demographic information. The script finally creates several geopackage files containing the merged data.

4. [Other Visualization.py]() creates more visualizations for exploratory data analysis. These include overlayed histograms for net generation frequency, nameplate frequence, and distribtion of Co2 equivalent emissions in tons. 

5. [Mkt Demand Supply.py]() creates and calibrates individual and market demand functions for carbon trading by each power plant. This caliberation is based on baseline information on total annual carbon emissions in 2022 and the carbon price in California's permit trading system. The market demand and market supply functions are then used to calculate new market price based on different emission targets using the opt.newton optimization method. A final function (called magnum_opus) is created, which takes intital plant-level emission, price, and emission-reduction target, to find new market price and individual demand for permits under different policy scenarios. The function is used to measure emission reduction by each individual plant using three hypothetical scenarios: A Command and Control policy for 20% state-level emission reduction where all power plants have the same elasticity of demand for permits, a permit-trading scenario for 20% state-level reduction where bigger polluters (those emitting more than 250,000 tons of Co2) have higher elasticities than rest, and a permit-trading scenario for 20% reduction where bigger polluters (those emitting more than 250,000 tons of Co2) have lower elasticities than the rest. 

6. [Tableau Power Plant Impact]() 

[Mapping Emission Reduction]() Under Different Policies.py creates interactive maps for the aforementioned scenarios. Each map (html format) shows individual plant-level response in specific census tracts to different policy scenarios.
