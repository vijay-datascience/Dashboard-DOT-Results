# Dashboard-DOT-Results
### DOT (Data Observation Toolkit) Results Dashboard

### Background
With funding from UNICEF and Rockefeller Foundation, Living Goods is leading the iCoHS consortium with partners BRAC, Medic, and DataKind to support the Ministry of Health, Uganda to use digital health technologies to address bottlenecks in community health. 

As a technical partner in the consortium, DataKind’s contribution has been to develop a community-informed data quality and integrity monitoring toolkit called the Data Observation Toolkit (DOT).
About DOT
The Data Observation Toolkit (DOT) can be configured to monitor data integrity issues such as blank values, duplicates and referential consistency as well as more advanced tests such as outlier detection and domain-specific issues identified using custom SQL queries.

Currently, DOT runs on deployments of Medic’s digital health platform, the Community Health Toolkit (CHT) and aims to support its users to identify, monitor and manage inconsistent or problematic (IoP) data.


#### Context / Why is data quality important? 
Community Health Workers (CHWs) perform vital front-line medical work - often in places where health infrastructure and resources are frequently lacking. CHWs collect routine health monitoring data that are critical inputs to monitoring and managing the provision of health services, especially by the public sector.

Digital tools specially created to support the work of CHWs have been making it easier to collect data to improve the coverage, speed, delivery and quality of essential and life saving health services. However, while there is mounting evidence that digital tools can strengthen community health systems, the full impact of these technologies has not yet been realized because data collected by CHWs are often considered to be of low quality and unreliable for data-driven decision-making. This has implications for data use and perpetuates lapses in data quality assurance while creating “an environment with a greater tolerance for poor data quality”.  


#### The Solution + Desired Impact
The DataKind team will harness a user-centered process to develop a dashboard using <a href = "https://superset.apache.org/">Superset<a> to demonstrate the functionality of DOT and the insights it generates using a sample dataset provided by BRAC. 

The project will be implemented with the guiding principle to ensure reproducibility of the approach  such that demo dashboards can be generated for when DOT is presented to increasingly wider audiences (in the near, post-iCoHS future, the dashboard will use a synthetic dataset that will be released alongside DOT - currently in development by another DataKind team). 

The demo dashboard will help end-users understand the insights into IoP data flagged by DOT. As individuals at these organizations become more comfortable with DOT, the dashboard can be used for training others and also on making decisions on remediation actions to be prioritized. Thus, the utility of the dashboard will grow - motivating users to invest the time to customize their dashboards per their needs in the future. 

## Data Summary
### To provide the team with inputs for the dashboard, DataKind collaborators performed the below tasks:
#### 1. Preparing a static DB dump using the BRAC sample dataset
#### 2. Configuring DOT against this data to create some test results
#### 3. Producing a DB dump which has the BRAC data and associated DOT stuff
#### 4. Designing a dashboard that allows to map KPIs onto superset 

This is a sample dataset that has been used to build this dashboard

<a href="https://drive.google.com/drive/folders/1bQdNNBqnaODDFzQosn4J5zRZBD8EYDsE">Sample data dump</a>
  
### Some useful Superset dashboard related queries
  
### Select entities and test results performed on entities in DOT
  
#####  SELECT 
#####   tr.test_id,
#####   tr.status,
#####   dot.get_test_result_data_record(ce.entity_name, tr.id_column_name, 
#####   tr.id_column_value,'public_tests')
##### FROM
#####   dot.scenarios s,
#####   dot.configured_tests ct,
#####   dot.configured_entities ce,
#####   dot.test_results tr
##### WHERE 
#####   s.scenario_id=ct.scenario_id AND
#####   tr.test_id=ct.test_id and 
#####   ce.entity_id=ct.entity_id
##### LIMIT 10

### Select internal between DOT runs to interpret results
  
##### select
##### min(run_start)::date as previous_run_date
##### ,max(run_start)::date as current_run_date
##### ,case
#####		when (max(run_start::date)-min(run_start::date))=0 then max(run_start)::date + 7
#####		else max(run_start)::date + (max(run_start::date)-min(run_start::date))
#####	 end as next_run_date
#####	,case
#####		when (max(run_start::date)-min(run_start::date))=0 then 7
#####		else (max(run_start::date)-min(run_start::date) )
#####	 end as cadence_in_days
##### from
##### (
#####	select *
#####	from dot.run_log rl
#####	where project_id =‘Brac’
#####	order by run_start desc
#####	limit 2
##### )



