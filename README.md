# Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-Permit-Trading-Program
This project provides an analysis of California's fossil-fueled power plants and their response to different emission policy and elasticity scenarios. It employs behavioral simulations, under cap-&-trade and command-&-control policies, which help understand the emission reduction behavior of individual power plants across census tracts—including those hosting Distadvantaged Communities (DACs)—in California.

# Methodology and Data

The data used in the analysis comes from multiple different sources. Plant-level data is sourced from EPA's Emissions & Generation Resource Integrated Database. Census tract information and ACS demographic data for California's census tracts have been sourced from census burea web interface. Finally, DAC scores—used in the tableau dashboard—are sourced from Cal Enviro SB 535 database. 

Firstly, plant level data—which includes plant specific information, such as geolocation, annual emissions (tons), and heat rates—is filtered to California's census tract level. Only fossil-fueld power plants, running on Gas, Oil, and Coal, are included for emission reduction scenarios. Secondly, this plant-level data is merged with census tracts and demographic information for exploratory data analysis. Thirdly, market demand and supply functions for emission reduction are formulated to simulate how individual power plants would respond to different policy scenarios under permit trading and command & control policies. Fourthly, the response of power plants is mapped on California's census tract level to see how individual power plants—differentiated through their annual emissions, percentage reduction in emission levels, and tract-based location—would respond to a given scenarion within a specific community (with its own DAC score). Finally, the power plant behavior, and its potential impact on DACs through emission reduction, is displayed on an [interactive Tableau dashboard](https://public.tableau.com/app/profile/haris.khan6829/viz/SimulatingPermitTradingforCalisPowerPlantsUnderDifferentPolicyElasticityScenarios/Dashboard1?publish=yes) for public use.

QGIS, geopandas, Tableau, and Python's folium module are used for mapping purposes throughout this analysis.

# Policy and Elasticity Scenarios

* Elasticity Scenarios: We run the simulations for three elasticity scenarios:

    1. Bigger polluters (those emitting more than 250k tons of Co2) have a higher elasticity of demand, and are therefore more responsive to the permit market demand curve, than smaller ones. This scenario also assumes that bigger polluters, with higher elasticities, have lower Marginal Abatement Cost curves (MACs). Under this assumption, a lower MAC translates into a higher elasticity because it is relatively less costly for the bigger polluter to reduce its own emissions. 

    2. Bigger polluters have a lower elasticity of demand, and are therefore less responsive to changes in the permit market demand curve, than smaller ones. This scenario assumes that bigger polluters, with lower elasticities, have higher Marginal Abatement Cost curves (MACs).

    3. All carbon-emitting power plants have the same elasticities of demand, and are thus equally responsive to price changes in the permit market. All polluters have the same MACs. 

*We use constant elasticity of demand function for the individual powerplants— which is then aggregated to find the market demand—as a stylized representation of individual permit trading behaviors under different elasticities*


* Policy Scenarios: The simulation is run for eight market-level reduction targets ranging from 20% to 90% emission reduction in the state. Users can choose the market reduction target to see how much individual power plants would abate emissions in response to the policy intervention given their specific elasticities/MACs. 


# Python Scripts

1. [Filtering egrid.py](https://github.com/Harisazeem/Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-s-Permit-Trading-Program/blob/main/Filtering%20egrid.py) filters national level data on plants and generators by fossil-fueled power plants in California. 

2. [Mapping plant-level data.py](https://github.com/Harisazeem/Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-s-Permit-Trading-Program/blob/main/Mapping%20plant-level%20data.py) adds information on unused capacity and total possible annual generation for each power plant in California. It then reads and filters census files. It tests the spread of the power plant point location and imports demographic information for California's census tracts using an API call. 

3. [Merges Plots.py filters](https://github.com/Harisazeem/Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-s-Permit-Trading-Program/blob/main/Merges%20%26%20Plots.py), spatially joins, and groups data on power plants by census tracts and their demographic information. It then visualized some of the information for exploratory data analysis. Data visualized here includes top 10 census tracts with highest number of power plants, highest emission levels, highest annual generation potential, and a correlation matrix for power plant and demographic information. The script finally creates several geopackage files containing the merged data.

4. [Other Visualization.py](https://github.com/Harisazeem/Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-s-Permit-Trading-Program/blob/main/Other%20Visualizations.py) creates more visualizations for exploratory data analysis. These include overlayed histograms for net generation frequency, nameplate frequence, and distribtion of Co2 equivalent emissions in tons. 

5. [Mkt Demand Supply.py](https://github.com/Harisazeem/Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-s-Permit-Trading-Program/blob/main/Mkt%20Demand%20%26%20Supply.py) creates and calibrates individual and market demand functions for carbon trading by each power plant. This caliberation is based on baseline information on total annual carbon emissions in 2022 and the carbon price in California's permit trading system. The market demand and market supply functions are then used to calculate new market price based on different emission targets using the opt.newton optimization method. Two final functions (called magnum_opus and magnum_opus2 ) are created, which take intital plant-level emission, price, and emission-reduction target as inputs, to find new market price and individual demand for permits under different policy scenarios. The function is used to measure emission reduction by each individual plant using three hypothetical scenarios: A Command and Control policy for different policy scenarios for state-level emission reduction where all power plants have the same elasticity of demand for permits, a permit-trading scenario where bigger polluters (those emitting more than 250,000 tons of Co2) have higher elasticities than the rest, and a permit-trading scenario where bigger polluters (those emitting more than 250,000 tons of Co2) have lower elasticities than the the smaller power plants. Highe

6. [Tableau Power Plant Impact](https://public.tableau.com/app/profile/haris.khan6829/viz/SimulatingPermitTradingforCalisPowerPlantsUnderDifferentPolicyElasticityScenarios/Dashboard1?publish=yes) is an interactive tableau dashboard which allows users to evaluate the emission reduction behavior of individual power plants, in specific census tracts with their respective DAC scores, in response to different policy and elasticity scenarios. It also shows average percentage emission reduction by census tracts which have either large power plant(s) (big polluters), or small power plant(s), or a combination of both. The dashboard also gives a distribution of Large and Small power plants by DAC scores across the state. 

Alternatively, [Mapping Emission Reduction](https://github.com/Harisazeem/Simulating-Power-Plant-Emission-Reduction-Behavior-Under-California-s-Permit-Trading-Program/blob/main/Mapping%20Emission%20Reduction%20Scenarios%20Under%20Different%20Policies.py) Under Different Policies.py creates interactive maps for the aforementioned scenarios. Each map (html files which need to be downloaded to be viewed) shows individual plant-level response in specific census tracts to different policy scenarios.

# Visualizations
Note: This section includes few of the important visualizations in the analysis.

![PPs in California](Plant%20Mapping%20in%20California%20(Census%20Tract%20Level).png)
Fig. 1

This map shows all the fossil-fueled power plants in California by census tract. The pie chart for each power plant represents its unused capacity while the size of the charts shows the total generation potential. Blue color on the chart signifies unused capacity.

![PPs on tracts with income](Power%20Plants%20by%20Tracts%20with%20PPs%20and%20Income.png)
Fig. 2

This map shows just the census tracts with power plants in California. The tracts are heatmapped to represent income of the people living in the tract. 


![Plant nameplate capacity (MW)](res_Plant%20nameplate%20capacity%20(MW).png)
Fig. 3

This shows the spread of Plant nameplate capacity (MW) for all power plants in California using a historgram and a kernel density function.

![Census Tract Correlation Matrix](Census%20Tract%20Correlation%20Matrix.png) 
  Fig. 4

This is a correlation matrix for plant level data and census tract demographic information. The matrix could not locate a clear correlation between tract-level plant information and demographics of disadvantaged populations. A DAC score specific analysis can be conducted in the dashboard directly. 

![Dashboard Snapshot](Dashboard%20Snapshot.png)
Fig 5. A snapshot of an interactive dashboard showing plant-level emission information pre and post permit-trading scenario. Different emission policy targets and elasticity scenarios for big polluters/large powerplants can be selected from the drop-down filters. Moreover, the figures on the left show percentage emission reduction by type of census tract (depending on whether it houses small power plant(s), large power plant(s), or a combination of both) and count of power plants by DAC score. The visualizations can be filtered by exact census tract and DAC percentile too.


# Noteworth Findings:

* Bigger polluters reduce more than smaller polluters if they have a higher elasticity of demand as per the assumption that they have lower MACs. In the short term, this is often a realistic scenario since smaller, more efficient power plants are already abating more and bigger, dirtier plants are at the initial stages of their abatement curves with more emissions to abate at lower costs. Moreover, bigger polluters perform better, in terms of reductions, under permit trading than command and control policy. However, the difference in their emission reduction under these two elasticity or MAC scenarios is not significant. Moreover, bigger polluters reduce less if they have lower elasticities of demand. And this difference is significant as seen from the dashboard. The location of each plant and emission-level information can be seen on the aforementioned interactive maps.

* On average, census tracts with large(r) or a combination of both small and large power plant(s) see slightly higher percentage emission reductions if the larger power plants have higher elasticities under a permit trading scenario vs command-and-control policy. 

* Under all policy scenarions, census tracts with smaller power plant(s) experience higher percentage reductions if these power plants have higher elasticities (or bigger power plants have lower elasticities). 

* It is important to note, however, that the percentage emission reduction by smaller power plants for each policy scenario fluctuates significantly more in response to elasticity than emission reduction by larger power plants. Consequently, as seen by the bars on the dashboard, and contrary to some of the environmental justice (EJ) concerns, controlling the emissions of smaller power plants in California should be a more important policy consideration. Their elasticities (or Marginal Abatement Cost curves) play a bigger role in overall emission reductions than the behavior of larger polluters. The difference in emissions reduction of larger power plant with higher and lower elasticities or MACs is not significant to begin with. 

* The last finding becomes even more crucial from an EJ perspective given there are more small power plants at each DAC score than large power plants. However, no strong correlation could be found between overall DAC scores and total emissions. 

Relevant stakeholders (individuals, firms, and policymakers) can easily use the dashboard to see how their specific census tracts would respond in terms of percentage emission reductions based on different policy and elasticity scenarios. 


