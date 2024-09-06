<script>
	import { onMount } from 'svelte';
  
	let provider_info_html = '';
  
	const fetchDataFrame = async () => {
		const response = await fetch('/provider_info.html');
		if (response.ok) {
		  provider_info_html = await response.text(); 
		} else {
		  console.error("Failed to fetch provider_info.html");
		}
	};

	onMount(fetchDataFrame);
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
		<p> Below is the head (first 5 rows) of the dataframe:</p>
		<div class='dataframe'> 
			{@html provider_info_html}
		</div>
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


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>