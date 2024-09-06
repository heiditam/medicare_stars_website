<script>
	// type stuff here
</script>

<main>
	<svg width='100%' height='250'>
		<rect width="100%" height="100%" style="fill:lightblue; stroke:navy; stroke-width:4px;" />
		<text x= 30% y = 75% fill = 'navy' font-family='Latos' font-size= 75px>Medicare Star Insights</text>
		<text x= 37% y = 90% fill = 'darkblue' font-family='Latos' font-size= 20px font-style='italic'>A Healthcare Machine Learning Project by Heidi Tam</text>
	</svg>
	<h2>Data Analysis</h2>
	<svg width='100%' height='450'>
		<rect width="100%" height="100%" style="fill:lightcyan; stroke:navy; stroke-width:4px; " />
		<text x= 5% y = 50 fill = 'navy' font-family='Latos' font-size= 35px>Introduction</text>
		<text x= 5% y = 100 fill = 'black' font-family='Latos' font-size= 20px>
			Every year, the Centers for Medicare & Medicaid Services (CMS) publishes a rating of Medicare Advantage (Part C) and
			prescription drugs (Part D). Medicare Advantage 
		</text>
		<text x= 5% y = 125 fill = 'black' font-family='Latos' font-size= 20px>
			(Part C) is a type of Medicare health plan that are given ratings based on members' overall satisfaction with the health
			 plan, customer service, plan performance, chronic
		</text>
		<text x= 5% y = 150 fill = 'black' font-family='Latos' font-size= 20px>
			 conditions, and staying healthy. Part D helps cover the cost of prescription drugs, and is given a rating
			based on member experience with the drug plan, customer service, 
		</text>
		<text x= 5% y = 175 fill = 'black' font-family='Latos' font-size= 20px>
			plan performance, and drug safety & pricing.
		</text>
		<text x= 5% y = 225 fill = 'black' font-family='Latos' font-size= 20px> <!-- +50 px down for paragraph gap-->
			We are interested in a how a comprehensive analysis of Medicare star ratings can reveal strategic insights for 
			healthcare insurance companies to enhance operational
		</text>
		<text x= 5% y = 250 fill = 'black' font-family='Latos' font-size= 20px>
			performance, drive growth, and have an advantage against its competitors. In this project, data was 
			meticulously cleaned using Python's pandas library. An exploratory data
		</text>
		<text x= 5% y = 275 fill = 'black' font-family='Latos' font-size= 20px>
			 analysis (EDA) visualizes the data through boxplots, maps, and a correlation analysis.
			Permutation and hypothesis tests were also conducted to draw statistical inferences, 
		</text>
		<text x= 5% y = 300 fill = 'black' font-family='Latos' font-size= 20px>
			and a pipeline model was built to forecast the overall star ratings for healthcare 
			providers based on key factors. Finally, actionable insights were drawn to create opportunities
		</text>
		<text x= 5% y = 325 fill = 'black' font-family='Latos' font-size= 20px>
			 healthcare insurance companies to build strategic actions to optimally benefit their company and the customer. 
		</text>

		<text x= 5% y = 375 fill = 'black' font-family='Latos' font-size= 20px>
			Our data (14834 rows, 103 columns), was collected from <a href="https://data.cms.gov/provider-data/dataset/4pq5-n9py#data-table">
			<tspan style="fill: blue;">CMS</tspan></a> and provides information on the data used in the 5 star rating system. 
	   </text>

	   <text x= 5% y = 400 fill = 'black' font-family='Latos' font-size= 20px>
		Below are some important columns from the dataset:
   		</text>
		
	</svg>

	<pre><code>                               | columns			                    | Details																	  |
|----------------------------------------------------|----------------------------------------------------------------------------|
| CMS Certification Number (CCN)                     | ID                                                                         |
| State                                              | State the provider is located in                                           |
| Number of Certified Beds                           | The number of federally certified beds                                     |
| Provider Type                                      | Medicare, Medicaid, or both                                                |
| Special Focus Status                               | Part of the special focus program, a candidate for one, or neither         |
| Overall Rating                                     | The overall star rating for a particular provider (1-5)                    |
| Health Inspection Rating                           | Considers whether health standards were met (1-5)                          |
| QM Rating                                          | Quality measure rating (1-5); measures the quality of care provided        |
| Staffing Rating                                    | Staffing rating (1-5); measures the quality of the staff at the providers  |
| # of Citations from Infection Control Inspections  | Number of citations from infection control inspections in the past 3 years |
| Total Amount of Fines in Dollars                   | The total amount of fines incurred in dollars                              |

	</code></pre>
	
</main>

<style>

	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
		padding: 0;
	}

	/* h1 {
		color: navy;
		text-transform: uppercase;
		font-family: 'Lato', sans-serif;
		font-size: 4em;
		font-weight: 100;
		text-align: center;
	} */

	h2{
		color: #4682B4;
		text-transform: uppercase;
		font-family: 'Lato', sans-serif;
		font-size: 2em;
		font-weight: bold;
		text-align: left;
		padding-left: 45%;
		-webkit-text-stroke: 1px black;
	}


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>