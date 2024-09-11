<script>
	import { onMount } from 'svelte';
	import Scrollyteller from './Scrollyteller.svelte';
  
	let provider_info_html = '';
	let fines_boxplot = '';
	let infections_citations_boxplot = '';
	let qm_state_graph = '';
	let qm_map_all = '';
	let staffing_map = '';
	let health_insection_map = '';
	let corr_table = '';
  
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

	const fetchCorrTable = async () => {
		const response = await fetch('/corr_table.html');
		if (response.ok) {
		  corr_table = await response.text(); 
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
		await fetchCorrTable();
	});

  </script>

<main>
	<Scrollyteller />
	<!--TITLE-->
	<svg width='100%' height='250'>
		<rect width="100%" height="100%" style="fill:lightblue; stroke:navy; stroke-width:4px;" />
		<text x= 30% y = 75% fill = 'navy' font-family='Latos' font-size= 75px>Medicare Star Insights</text>
		<text x= 37% y = 90% fill = 'darkblue' font-family='Latos' font-size= 20px font-style='italic'>A Healthcare Machine Learning Project by Heidi Tam</text>
	</svg>
	<!--DATA ANALYSIS-->
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
	<!--METHODOLOGY-->
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
		<div class='df1'> 
			{@html provider_info_html}
		</div>
	</div>
	<!--DATA CLEANING-->
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
	<!--EXPLORATORY DATA ANALYSIS-->
	<div class='blue_box'>
		<h2>Exploratory Data Analysis (EDA)</h2>
		<div style='display:flex; justify-content:flex-start;'>
			<iframe src='/fines_boxplot.html' width="75%" height="500px" title = 'Boxplot of Total Amount of Fines in Dollars'></iframe>
			<ul class='caption' style='max-width: 25%;'>
				<br> <br>
				<li><b>Minimum: </b>$0</li>
				<li><b>1st Quartile: </b>$0</li>
				<li><b>Median: </b>$7,444.375</li>
				<li><b>3rd Quartile: </b>$34,790</li>
				<li><b>Maximum: </b>$1,235,806.05</li>
				<li><b>Mean: </b>$38,144.142</li>
				<br><br>
				<li>Using the 1.5 IQR Rule, <b>12%</b> of the 'Total Amount of Fines in Dollars' data are outliers.</li>
				<li>The graph is heavily <b>right-skewed.</b></li>
				<li>Since the minimum and the 1st quartile are both $0, a substantial portion of the providers have few to none fines.</li>
				<li>The median is much lower than the mean, which means that a few providers were fined exceptionally more than most.</li>
				<li>The majority of the fines are relatively small, but a few outliers dominate the dataset. This could be due to a few providers
					breaking the standards set for health providers.
				</li>
			</ul>
		</div>
		<div style='display:flex; justify-content:flex-start;'>
			<ul class='caption' style='max-width: 25%;'>
				<br> <br>
				<li><b>Minimum: </b>0 citations</li>
				<li><b>1st Quartile: </b>0 citations</li>
				<li><b>Median: </b>0.727 citations</li>
				<li><b>3rd Quartile: </b>1.122 citations</li>
				<li><b>Maximum: </b>43 citations</li>
				<li><b>Mean: </b>1.02 citations</li>
				<br><br>
				<li>Using the 1.5 IQR Rule, <b>about 8.34%</b> of the 'Number of Citations from Infection Control Inspections' data are outliers.</li>
				<li>The graph is heavily <b>right-skewed.</b></li>
				<li>Since the minimum and the 1st quartile are both $0, a substantial portion of the providers have few to none fines. Notice even
					the median and the 3rd quartile of this column are close to only 1 citation. 
				</li>
				<li>The median is <b>significantly</b> lower than the mean, which means that a few providers were cited unusally more than most.</li>
				<li>Most of the providers are doing very well and have virtually no citations. This means most providers obey health standards.
				</li>
			</ul>
			<iframe src='infections_citations.html' width='75%' height='500px' title='Boxplot of Number of Citations From Infection Control Measures'></iframe>
		</div>
		<div style='display:flex; justify-content:space-between;'>
			<iframe src='qm_state_graph.html' width='50%' height='500px' title='Map of Average QM Rating by State'></iframe>
			<iframe src='staffing_rating_map.html' width='50%' height='500px' title='Map of Average Staffing Rating by State'></iframe>
		</div>
		<div style='display:flex; justify-content:flex-start;'>
			<iframe src='health_inspection_rating.html' width='50%' height='500px' title='Map of Average Health Inspection Rating by State'></iframe>
			<ul class = 'caption' style='max-width:50%;'> 
				<b> Analysis of Choropleth Maps</b>
				<br>
				All ratings are based on a scale of 1-5, with 1 being the worst ranking and 5 being the best.
				<br> <br> <b>Quality Measure Ratings </b>(National Average: 3.50/5) <br>
				<li>Western states (eg California, Nevada, Idaho) and some states in the Northeast (eg Delaware, New Jersey) have a pretty
					strong quality measure rating of 4.2 - 4.4.
				</li>
				<li>Several Midwestern states, such as Indiana and Ohio, have an above-average rating, with scores around 4.</li>
				<li>Many <u>Southeastern</u> (eg Alabama, Georgia, North Carolina) and Midwestern (eg Illinois, Missouri) states are rated
					 poor quality</li>
				<li>Two states located in the <u>Southeastern</u>, Louisiana and Mississippi, have extremely poor quality.</li>

				<br> <br> <b>Staffing Ratings </b>(National Average: 2.676/5) <br>
				<li>Many states in the West (eg California, Nevada, Arizona), had subpar staffing quality, with a staffing quality rating
					of 2.6-2.8. </li>
				<li><u>Southern states</u>Texas and Louisiana have significantly below staffing quality. Texas has a staffing quality
				rating average of 1.827/5 and Louisiana has a staffing quality average of 2.153/5.</li>
				<li><u>Midwestern states</u> Missouri, Illinois, Indiana, and more also have indequate staffing quality, generally 
				averaging 2/5 stars.</li>

				<br><br> <b>Health Inspection Ratings</b> (National Average: 2.785/5)
				<li>Most of the U.S. states are fairly close to average, but there is still room for improvement.</li>

			</ul>
		</div>
		<iframe src='qm_map_all.html' width='100%' height='500px' title='Map of Average QM Rating by Provider Site'></iframe>
	</div>
	<div class='blue_box'>
		<h2>Correlation Analysis</h2>
		<div style='display:flex; justify-content:flex-start;'>
			<div class = 'df2'>{@html corr_table}</div>
			<ul class = 'caption' style='max-width:50%;'>
				<li>There is a <u>strong positive correlation</u> between overall rating and health inspection rating.
				As health inspection ratings improve, overall ratings tend to rise as well. The strong relationship between
				these two variables indicates meeting health standards is vital towards achieving an excellent overall rating.</li><br>
				<li>There is a <u>moderately strong positive correlation</u> between overall rating with QM Rating and staffing
				rating. While a relationship exists between these variables, other factors may also play a significant role
				in affecting the overall rating.</li><br>
				<li>There is a <u>weak to moderately strong positive correlation </u> between overall rating and the total amount
				of fines in dollars. Since the health inspection ratings, staffing ratings, and QM ratings are directly factored
				into the calculation of the overall rating, we are interested in analyzing how the total number of fines a provider 
				has impacts its overall rating.</li>
			</ul>
		</div>
		<p class='tests'>
			<b>Question of Interest:</b> Is there a significant difference in overall ratings between providers with varying amounts of fines? <br>
			<b>H<sub>0</sub>:</b> &mu;<sub>1</sub> = &mu;<sub>2</sub> = &mu;<sub>3</sub> = &mu;<sub>4</sub> = &mu;<sub>5</sub>, where 
			&mu;<sub>i</sub> is the mean number of fines for each category rating. The mean number of fines is the same for each rating.<br>
			<b>H<sub>1</sub>:</b> Some &mu;<sub>i</sub> are different. There is a difference in the mean number of fines for at least
			one of the rating categories. <br>
			We will assess the significance of the relationship between more than three categories so we will conduct an <b>ANOVA test</b>.<br>
			After conducting an ANOVA test of 10,000 permutations, we achieved an F-stat of approximately 243.55. 
			The permuted F-values range from 0.002 to 6.774, which is <i>very </i> far off from the observed F-stat.
			Unsurprisingly, our p-value is about 0. <br>
			Because p = 0 &lt; 0.05, we reject the null hypothesis, so the <b>mean total fines is significantly different</b>
			 for each category. A strong correlation exists, but we do not have enough evidence to suggest there is a casual relationship
			  in our data. This conclusion makes sense because we would expect providers with a <i>lower</i> overall quality rating to 
			  have accumulated more fines, since they are more likely to violate health standards and not meet the satisfaction of 
			  beneficiaries (customers).
		</p>
		<img src='fstat.png' alt='Permutation F-Value Distribution' style='display:block; margin:auto;'  >
	</div>
	<!-- MACHINE LEARNING -->
	<h1>MACHINE LEARNING</h1>
	<div class='green_box'>
		<h2>Building a Pipeline Model</h2>
		<h3>Framing the Problem</h3>
		I built an <b>SVC (Support Vector Classification)</b> Model, which can efficiently use support vectors to focus on 
		critical data points and features and ignore the less informative ones for classification. For example, if 'Special Focus
		Status' is a key driver in determining the overall rating, the SVC Model will take this into account. This type of model also 
		pays particular attention to small subsets of the data that may disproportionately affect the overall Medicare star rating.
		This could be useful for targeting facilities with consistently low star ratings, fairly allocating resources amongst providers,
		and adjusting and regulating policies as needed. SVC Models would also be able to capture intricate nonlinear patterns, which may be
		useful because the relationship between the overall ratings and the features may not necessarily be linear. In other words, SVC
		is highly beneficial for using support vectors to classify data with a variety of features: in this case, ratings, categorical
		features, and other numerical features. <br><br>
		<li><b>Response Variable: </b> overall rating (the overall star rating for providers)</li>
			<ul>
				<li>We will be observing how the selected features impact overall rating because we are interested in what factors
					 could cause providers to have higher star ratings.
				</li>
			</ul>
		<li><b>Evaluation Metrics: </b>precision, recall, F1-Score, accuracy</li>
			<ul>
				<li><u>Precision: </u>tells us the percentage of <i>positive predictions</i> that were correctly made (ie Of all the
					predictions predicted to be positive, how many were actually positive?)</li>
				<li><u>Recall: </u>tells us the percentage of<i>actual positive instances </i> that were correctly identified by
				the model. (ie Of all the true positive instances, how many were correctly predicted?)</li>
				<li><u>F1-Score: </u>the harmonic mean of precision and recall</li>
				<li><u>Accuracy: </u>the proportion of correct predictions (positive + negative) out of all predictions</li>
			</ul>
		<pre><code>
			# Separate features (X) and target (y)
			# Drop CMS because it is an identifier
			X = smaller_provider_info.drop(columns=['Overall Rating'])
			y = smaller_provider_info['Overall Rating']
			
			# pipeline preprocessing 
			categorical = ['State', 'Ownership Type', 'Provider Type', 'Special Focus Status', 'Abuse Icon']
			quantitative = ['Number of Certified Beds', 'Average Number of Residents per Day', 'Health Inspection Rating', 'QM Rating',\
							'Staffing Rating', 'Number of Citations from Infection Control Inspections',\
							 'Total Amount of Fines in Dollars']
			
			preprocessor = ColumnTransformer(
				transformers=[
					('cat', OneHotEncoder(), categorical),
					('num', StandardScaler(), quantitative)
				]
			)
			
			# We will use an SVC Model because it can handle complex data and can use special techniques to transform both linear\
			 and nonlinear data. 
			model = Pipeline(
				steps = [
					('preprocessor', preprocessor),
					('classifier', SVC(max_iter=1000, random_state=42))
				]
			)
			
			# train = trains ML algorithm; test = used to evaluate the accuracy of the trained algorithm

			# Save 20% of the data for testing
			# Split the data into training + test sets
			X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
			
			# Train the model
			model.fit(X_train, y_train)
			
			# Make predictions
			y_pred = model.predict(X_test)
			
			print(classification_report(y_test, y_pred))
		</code></pre>
		<h3>Results of the Pipeline Model</h3>
		<div style='display:flex; justify-content:flex-start;'>
			<img src='metrics.png' alt='Metrics of Pipeline Model' width=500; height=auto;>
			<ul class='caption'>
				<li><b>Precision:</b> Our precision scores range from 0.95-0.97. Out of all the positive predictions we made, 95% to 97% were true.</li> <br>
				<li><b>Recall: </b> Our recall scores range from 0.93-0.99. This means out of all the positive instances, our model correctly predicted 93-99% of them.</li> <br>
				<li><b>F1 Score: </b> This is the harmonic mean of precision and recall. Our F1 scores range from 0.95 to 0.98, which is pretty good! We want to be as close to 1 as possible without overfitting.</li> <br>
				<li><b>Support: </b> A similar number of items appeared in each category, which is what we want.</li> <br>
			</ul>
		</div>
	</div>
	<div class='green_box'>
		<h2>Insight & Applications of the Pipeline Model</h2>
		<pre><code>predictions = X.test[y_pred&lt;=2]</code></pre>
		<p style='color:red;'>
			There are <u><b>1361 rows (predictions)</b></u> that would have a low overall quality rating of 1 or 2. This is very close to the number of rows
			we would expect to have since our test data is 20% of all the data (1361 rows in predicted results vs. <b>1319 expected rows</b>)
		</p>
		<pre><code>true_values = smaller_provider_info[smaller_provider_info['Overall Rating'] &lt;=2]</code></pre>
		<pre><code>0.2 * smaller_provider_info.shape[0] * true_values.shape[0]/smaller_provider_info.shape[0]</code></pre>
		<p style='color:navy;'>1319 expected rows</p>
		<p style='color:red;'>
			<b>1330 rows</b> in our predictions for low rankings have Medicare provider IDs (CMS Certification Number) that match Medicare provider IDs 
			that actually have a low ranking of 1 or 2.
		</p>
		<pre><code>
			len([cms for cms in set(predictions['CMS Certification Number (CCN)']) if cms in true_values['CMS Certification Number (CCN)'].values])
		</code></pre>
		<p style='color:navy;'> <u>1330 rows with matching IDs</u></p>
		<pre><code>1330/1361 * 100</code></pre>
		<p style='color:navy;'>This means about <b><u>97.72%</u></b> of predicted low-ranking instances had provider IDs that matched actual low-ranking providers.
			This is a good indication our model is performing well.</p>
		<h3>Cost Analysis</h3>
		First, let's investigate the <u>total amount of fines</u> for healthcare providers.
		<li>The mean total amount of fines in dollars for low ranking providers (ranks 1-2) is <b>$69,789.79.</b></li>
		<li>The mean total amount of fines in dollars for high ranking providers (ranks 3-5) is <b>$13,328.98.</b></li>
		<p>
			The mean total amount of fines PREDICTED for low ratings (1, 2) is over <b>5 times greater</b> than the mean total amount of fines PREDICTED for high ratings (3-5). <br>
			The model suggests that <b>fines are an important indicator</b> in predicting overall star ratings in the dataset.<br>
			We can quickly confirm that high ranking providers have significantly less fines than low ranking providers with a significance test.
			Recall earlier we performed a permutation test to see if there was a significant difference between <i>all observed </i> rating categories.
			In this test, we will analyze whether <b>providers predicted to have higher ratings</b> have significantly less fines than 
			<b>providers predicted to have lower ratings</b>.
		</p>
		<ul class='tests'>
			<b>Question of Interest:</b> Is the mean total amount of fines significantly greater in healthcare providers with lower predicted rankings? <br><br>
			<b>H<sub>0</sub>:</b> &mu;<sub>1</sub> = &mu;<sub>2</sub>, where &mu;<sub>1</sub> is the mean total amount of fines for healthcare providers
			with predicted low rankings (1-2) and &mu;<sub>2</sub> is the mean total amount of fines for healthcare providers predicted to have high
			rankings (3-5).<br>
			<b>H<sub>1</sub>:</b> &mu;<sub>1</sub> ≥ &mu;<sub>2</sub> &rarr; lower-ranking providers are expected to have more fines.<br>
			We are comparing the means of two groups, so we will perform a two-sample t-test. <br><br>
			<b>Assumptions:</b>
			<li>The populations are independent of each other because low-ranking and high-ranking providers are mutually exclusive.</li>
			<li>The Central Limit Theorem balance condition is met in each sample (at least 30 providers for low and high rankings each).</li>
			<li>The data are independent in each sample (assume randomization, and not enough info to confirm sample is &lt;10% of population)</li><br>
			
			After conducting a two-sample t-test, we retrieve a t-stat of about 13.846 and a p-value of about 1.355 * 10^(-42).
			Since our p-value is 1.35 * 10^(-42) &lt; 0.05, we <b>reject the null hypothesis</b>. The total amount of fines is significantly greater 
			in healthcare providers with low predicted rankings.
		</ul>
		Now, we will look at <u>payment denials.</u>
		<li>The mean number of payment denials for low ranking providers (ranks 1-2) is <b>about 0.399.</b></li>
		<li>The mean number of payment denials for high ranking providers (ranks 3-5) is <b>about 0.984.</b></li>
		At a glance, the mean payment denials for the predicted lower & higher rankings are very similar. We will need to conduct another 2-sample 
		t-test to investigate whether this difference is significant.

		<ul class='tests'>
			<b>Question of Interest:</b> Is the mean number of payment denials significantly greater in healthcare providers with lower predicted rankings? <br><br>
			<b>H<sub>0</sub>:</b> &mu;<sub>1</sub> = &mu;<sub>2</sub>, where &mu;<sub>1</sub> is the mean number of payment denials for healthcare providers
			with predicted low rankings (1-2) and &mu;<sub>2</sub> is the mean number of payment denials for healthcare providers predicted to have high
			rankings (3-5).<br>
			<b>H<sub>1</sub>:</b> &mu;<sub>1</sub> ≥ &mu;<sub>2</sub> &rarr; lower-ranking providers are expected to have more payment denials.<br>
			We are comparing the means of two groups, so we will perform a two-sample t-test. <br><br>
			<b>Assumptions:</b>
			<li>The populations are independent of each other because low-ranking and high-ranking providers are mutually exclusive.</li>
			<li>The Central Limit Theorem balance condition is met in each sample (at least 30 providers for low and high rankings each).</li>
			<li>The data are independent in each sample (assume randomization, and not enough info to confirm sample is &lt;10% of population)</li><br>
			
			After conducting a two-sample t-test, we retrieve a t-stat of about 14.928 and a p-value of about 6.043 * 10^(-49).
			Since our p-value is 6.043 * 10^(-49) &lt; 0.05, we <b>reject the null hypothesis</b>. The number of payment denials is significantly greater 
			in healthcare providers with low predicted rankings.
		</ul>
	</div>
	<!--STRATEGY DEVELOPMENT -->
	<h1>Strategy Development</h1>
	<div class='lavender_box'>
		<h2>Healthcare Terminology</h2>
		<div style='display:flex; justify-content:flex-start;'>
			<ul>
				<li><b>Providers: </b>a person or organization that provides healthcare services, such as hospitals, doctors, clinics, and nursing homes</li>
				<li><b>Beneficiaries: </b>the people benefitting from the Medicare plan they are enrolled in; the customer</li>
				<li style='color:blue;'><b>Medicare: </b>federal health insurance for seniors aged 65+; also applies to some disabled people under age 65</li>
				<li><b>Medicaid: </b>a joint federal + state health insurance program that helps cover costs for people with low income and 
					resouces. Unlike Medicare, Medicaid also offers services such as nursing home care and personal care services.</li>
				<li><b>MediCal: </b>the Medicaid program for California</li>
			</ul>
			<div class='caption'>
				<b>Parts of Medicare:</b>
				<ol type='I'>
					<li><b>PART A: Hospital Insurance</b>-- covers patient care in hospitals, nursing facilities, and home health care.</li>
					<li><b>PART B: Medical Insurance</b>-- covers preventative services (eg vaccines, tests, screenings), doctor services, 
						and medical equipment.</li>
					<li><b>PART C: Medicare Advantage</b>-- offered by private insurance companies, but covers and provides the same services
						as Original Medicare.</li>
					<li><b>PART D: Drug Coverage</b>-- covers the cost of prescribed drugs, including many shots + vaccines</li>
				</ol>
			</div>
		</div>
		<h2>Problem Analysis</h2>
		<b>Providers</b> are evaluated on the level of care they provide. They are not impacted directly, but may experience some indirect
			 consequences if they provide poor care. <br><br>
		
		There are two crucial types of penalties for nursing homes: <b>fines</b> and <b>payment denials.</b><br>
		<li><b>Fines: </b>Medical Advantage plans need to pay when they are discovered to be non-compliant with regulations
			from the Centers for Medicare & Medicaid Services (CMS). This is typically associated with the quality of care
			that beneficiaries receive.</li>
		<li><b>Payment Denials: </b> The Medicare plan denies payment to the healthcare provider for a service or medication 
			the beneficiary received if its quality was not satisfactory.</li>

		<h2>Steps Health Insurance Companies Can Take</h2>
		<i>#2-5 expand on how to improve quality care.</i><br>
		<ol type='1'>
			<li><b>Better Quality Care: </b>Fines and payment denials are significant factors that affect the quality of care healthcare services 
				and professionals provide. By focusing on enhanced care, insurance companies will face less fines and payment denials, thus 
				improving the customer's overall experience. In particular, it is vital to consistently achieve high health inspection ratings
				 in order to provide the beneficiaries with optimal service and excellent care. </li>
			<li><b>Enhanced Staff Tranings: </b>Insurance companies could boost the quality of care they provide by enhancing 
				their staff trainings. Staff could be better trained on how to handle situations such as medical emergencies,
				 controlling infections, and meeting safety guidelines and standards. There are two types of important trainings
				  insurance companies could implement: <b>Patient Care Practices</b> and <b>Facility Maintenance</b>. </li><br>
			<ul>
				<li>
					<b>Patient Care Practices:</b> These would be focused on how to provide premium care to patients, including 
					proper medical procedures, handling medication and relevant medical documents safely, and ensuring the patient
					 has a comfortable experience. It is essential to also practice meeting health standards by maintaining a clean
					  environment, washing hands frequently, and using PPE (personal protective equipment).
				</li>
				<li>
					<b>Facility Maintenance: </b> It is also key to maintain a hygienic environment by regularly cleaning and 
					checking facilities, including bathrooms, patient rooms, treatment rooms, etc. It is also important to check 
					all equipment is functional and to keep track of which equipments may be wearing down quickly. Mock inspections
					 could help identify room for improving health inspection ratings.
				</li><br>
				For both of these practices, health insurance companies should have mandatory video modules for staff training as well as in-person training.
			</ul>
			<li><b>Customer Service: </b>Health insurance companies could improve the quality of communication they have with their customers. 
				This may mean communicating their message more effectively or decreasing their response time.</li>
			<li><b>Management of Chronic Conditions: </b>Health insurance companies could have programs that focus on supporting groups with
				 chronic conditions, such as victims of heart and lung diseases. </li>
			<li><b>Preventative Care: </b>A healthy customer is a happy customer, so insurance companies could monitor more frequent requirements
				 for screening and tests. Catching diseases and infections early on is important before the condition deteriorates. Customers 
				 may also watch simple modules online that explain how to prevent and treat non-fatal infections and illnesses. These modules
				  could be shown to them via a personal portal, where the videos shown vary depending on age and relevant risk factors. </li>
			
		</ol>
		<br>
		<b>Note: </b> While a correlation exists between the following actions and higher ratings, this does not imply causation.
		 However, implementing these steps may increase a health insurance company's likelihood of being perceived favorably.
	</div>
	<h1>Additional Links</h1>
	<div class='pink_box'>
		<div style='display:flex; justify-content:space-between; width:300px;'>
			<a href='https://docs.google.com/document/d/1xPBsm6Xnv7x20dzEuUOr6x0PXMItEABKPlU18cWJqxA/edit' class='button'>Works Cited</a>
			<a href='https://github.com/heiditam/medicare_stars_website' class='button'>GitHub</a>
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
		text-align: center;
		-webkit-text-stroke: 1px black;
	}

	h2{
		color: darkblue;
		font-size: 2em;
		margin-bottom: 10px;
		font-family: 'Lato', sans-serif;
	}

	h3{
		color: #006400;
		font-size: 1.5em;
		margin-bottom: 10px;
		font-family: 'Lato', sans-serif;
	}

	.blue_box{
		width: 100%;
		box-sizing: border-box;
		background-color: lightcyan;
		border-bottom: 2px solid navy;
		border-top: 2px solid navy;
		font-family: 'Lato', sans-serif;
		font-size: 1em;
		padding: 20px 50px 10px 50px;
		text-align: left;
	}

	.green_box{
		width: 100%;
		box-sizing: border-box;
		background-color: #E8FFF5;
		border: 4px solid navy;
		border-bottom: 2px solid navy;
		border-top: 2px solid navy;
		font-family: 'Lato', sans-serif;
		font-size: 1em;
		padding: 20px 50px 10px 50px;
		text-align: left;
	}

	.lavender_box{
		width: 100%;
		box-sizing: border-box;
		background-color: lavender;
		border: 4px solid navy;
		border-bottom: 2px solid navy;
		border-top: 2px solid navy;
		font-family: 'Lato', sans-serif;
		font-size: 1em;
		padding: 20px 50px 10px 50px;
		text-align: left;
	}
	
	.pink_box{
		width: 100%;
		box-sizing: border-box;
		background-color: lightpink;
		border: 4px solid navy;
		border-bottom: 2px solid navy;
		border-top: 2px solid navy;
		font-family: 'Lato', sans-serif;
		font-size: 1em;
		justify-content: center;
		align-items: center;
		display: flex;
	}

	.df1{
		max-height: 500px;
		overflow:auto;
		margin-top: 20px;
		background-color: white;
		padding: 10px;
		border: 1px solid #ccc;
	}

	.df2{
		max-height: 500px;
		overflow:auto;
		margin-top: 20px;
		background-color: white;
		padding: 10px;
		border: 1px solid #ccc;
		display: inline-block;
		width: auto;
	}

	.caption{
		background-color: #708090;
		color: white;
		text-align: left;
		padding-left: 20px;
		padding-right: 20px;
		font-family: 'Lato', sans-serif;
	}

	.tests{
		background-color: #FFE8E3;
		border: 1px solid #E39A86;
		color: black;
		text-align: left;
		padding-left: 20px;
		padding-right: 20px;
		font-family: 'Lato', sans-serif;
	}

	.button{
		background-color: navy;
		border: 0.5px solid grey;
		color: white;
		text-align: center;
		font-family: 'Lato', sans-serif;
		padding: 10px;
		margin: 20px 0px;
	}


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>