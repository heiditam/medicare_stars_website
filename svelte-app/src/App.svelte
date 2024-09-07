<script>
	import { onMount } from 'svelte';
  
	let provider_info_html = '';
	let fines_boxplot = '';
	let infections_citations_boxplot = '';
	let qm_state_graph = '';
	let qm_map_all = '';
	let staffing_map = '';
	let health_insection_map = '';
  
	const fetchDataFrame = async () => {
		const response = await fetch('/provider_info.html');
		if (response.ok) {
		  provider_info_html = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	const fetchBoxPlot = async () => {
		const response = await fetch('/fines_boxplot.html');
		if (response.ok) {
		  fines_boxplot = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	const fetchBoxPlot2 = async () => {
		const response = await fetch('/infections_citations.html');
		if (response.ok) {
		  infections_citations_boxplot = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	const fetchMap = async () => {
		const response = await fetch('/qm_state_graph.html');
		if (response.ok) {
		  qm_state_graph = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	const fetchMap1 = async () => {
		const response = await fetch('/qm_map_all.html');
		if (response.ok) {
		  qm_map_all = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	const fetchMap2 = async () => {
		const response = await fetch('/staffing_rating_map.html');
		if (response.ok) {
		  staffing_map = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	const fetchMap3 = async () => {
		const response = await fetch('/health_inspection_rating_map.html');
		if (response.ok) {
		  health_insection_map = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};


	onMount(async() =>{
		await fetchDataFrame();
		await fetchBoxPlot();
		await fetchBoxPlot2();
		await fetchMap();
		await fetchMap1();
		await fetchMap2();
		await fetchMap3();
	});

  </script>

<main>
	<svg width='100%' height='250'>
		<rect width="100%" height="100%" style="fill:lightblue; stroke:navy; stroke-width:4px;" />
		<text x= 30% y = 75% fill = 'navy' font-family='Latos' font-size= 75px>Medicare Star Insights</text>
		<text x= 37% y = 90% fill = 'darkblue' font-family='Latos' font-size= 20px font-style='italic'>A Healthcare Machine Learning Project by Heidi Tam</text>
	</svg>
	<h1>Data Analysis</h1>
	<div class = 'blue_box'>
		<h2>Introduction</h2>
		<p>Every year, the Centers for Medicare & Medicaid Services (CMS) publishes a rating of Medicare Advantage (Part C) and prescription drugs (Part D). Medicare Advantage
		(Part C) is a type of Medicare health plan that are given ratings based on members' overall satisfaction with the health plan, customer service, plan performance, chronic
		conditions, and staying healthy. Part D helps cover the cost of prescription drugs, and is given a rating based on member experience with the drug plan, customer service,
		plan performance, and drug safety & pricing.</p>
		<p>We are interested in how a comprehensive analysis of Medicare star ratings can reveal strategic insights for healthcare insurance companies to enhance operational
		performance, drive growth, and have an advantage against its competitors. In this project, data was meticulously cleaned using Python's pandas library. An exploratory data
		analysis (EDA) visualizes the data through boxplots, maps, and a correlation analysis. Permutation and hypothesis tests were also conducted to draw statistical inferences,
		and a pipeline model was built to forecast the overall star ratings for healthcare providers based on key factors. Finally, actionable insights were drawn to create opportunities
		healthcare insurance companies to build strategic actions to optimally benefit their company and the customer.</p>
	</div>
	<div class = 'blue_box'>
		<h2>Methodology</h2>
		<p>The data (14834 rows, 103 columns), was collected from CMS and provides information on the data used in the 5 star rating system.</p>
		<p> Below are some important columns from the dataset:</p>
		<pre><code>
			| Column                                             | Description                                                                |
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
		<p> Below is the head (first 5 rows) of the dataframe. Scroll left and right to view the other columns of the dataset.</p>
		<div class='dataframe'> 
			{@html provider_info_html}
		</div>
	</div>

	<div class='blue_box'>
		<h2>Data Cleaning</h2>
		Data is messy! Here's how I cleaned it:
		<ul>
			<li>To deal with missingness, I created a function called removeNA, which removed all NaN values in the columns
				'Number of Citations from Infection Control Inspections', 'Health Inspection Rating', 
                'Average Number of Residents per Day', 'Overall Rating', 'QM Rating', and 'Staffing Rating'
			</li>
			<li>
				I imputed the missing data with the mean of the category for its respective state after dropping all NaNs. If a state <i>only</i> 
				contained NaNs for a particular category, I imputed the missing data with the mean of the category across <i>all</i> states.
			</li>
			<li>
				The 'Special Focus Status' column only indicated whether a provided belonged to a special focus facility program or was a 
				candidate for one. Providers that are labelled 'SFF' are required to go to a special focus facility program because they have
				serious quality issues and need improvements. However, if a provider was not under surveillance for special attention, it would
				have an 'NaN' in the 'Special Focus Status' column. To make our data more readable, I replaced all the 'NaN' with 'Not Special Focus'.
			</li>
			<li>
				The 'Abuse Icon' column consists of data labelled either 'Y' or 'N'. To make our data more expressive and useful for 
				statistical analysis, I relabelled into binary values and converted every 'Y' to 1 and 'N' to 0. 
			</li>
		</ul>
	</div>
	<div class='blue_box'>
		<h2>Exploratory Data Analysis (EDA)</h2>
		<iframe src='/fines_boxplot.html' width="100%" height="500px" title = 'Boxplot of Total Amount of Fines in Dollars'></iframe>
		<iframe src='infections_citations.html' width='100%' height='500px' title='Boxplot of Number of Citations From Infection Control Measures'></iframe>
		<div style='display:flex; justify-content:space-between;'>
			<iframe src='qm_state_graph.html' width='50%' height='500px' title='Map of Average QM Rating by State'></iframe>
			<iframe src='staffing_rating_map.html' width='50%' height='500px' title='Map of Average Staffing Rating by State'></iframe>
		</div>
		<div style='display:flex; justify-content:flex-start;'>
			<iframe src='health_inspection_rating.html' width='50%' height='500px' title='Map of Average Health Inspection Rating by State'></iframe>
			<p class = 'caption'> 
				<b> Analysis of Choropleth Maps</b>
			</p>
		</div>
		<iframe src='qm_map_all.html' width='100%' height='500px' title='Map of Average QM Rating by Provider Site'></iframe>
	</div>
	
</main>

<style>

	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
		padding: 0;
	}

	h1{
		color: #4682B4;
		text-transform: uppercase;
		font-family: 'Lato', sans-serif;
		font-size: 2em;
		font-weight: bold;
		text-align: left;
		padding-left: 45%;
		-webkit-text-stroke: 1px black;
	}

	.blue_box{
		width: 100%;
		box-sizing: border-box;
		background-color: lightcyan;
		border: 4px solid navy;
		border-bottom: 2px;
		font-family: 'Lato', sans-serif;
		font-size: 1em;
		padding: 20px 50px 10px 50px;
		text-align: left;
	}

	.blue_box h2{
		color: darkblue;
		font-size: 2em;
		margin-bottom: 10px;
		font-family: 'Lato', sans-serif;
	}

	.dataframe{
		max-height: 500px;
		overflow:auto;
		margin-top: 20px;
		background-color: white;
		padding: 10px;
		border: 1px solid #ccc;
	}

	.caption{
		background-color: #708090;
		max-width: 50%;
		color: white;
		text-align: left;
		padding-left: 20px;
		font-family: 'Lato', sans-serif;
	}


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>