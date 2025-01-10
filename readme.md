The project uses the sample data provided with each instance of Splunk Observability Cloud. Data will also be generated in real-time by a microservice based e-commerce platform.

The project is designed for DevOps and Site-Reliability Engineers, plus anyone else wanting to gain an understanding of Splunk Observability Cloud using a hands-on environment.

Scenario:
Buttercup Enterprise is a large national online retailer operating in the US, which sells a variety of books, clothing and other gifts through its online webstore.
Buttercup Enterprises have recently invested in Splunk and now they want to start making use of it across the business.

Provide insight to users throughout the company, teams that require support:
- IT Operations team: Investigate successful versus unsuccessful web server requests over time.
- DevOps team: Show the most common customer operating systems and which web browsers are experiencing the most failures.
- Business Analytics: Show lost revenue from the Buttercup Enterprises website.
- Security and Fraud: Show website activity by geographic location

Project covers the following:
- Accessing Lab Environment
- Creating an App and Adding Data
- Exploring Data and Searching
- Visualizing Data with Charts
- Customizing Dashboards

Search Processing Language (SPL) Queries
1. Investigate successful versus unsuccessful web server requests over time.
- sourcetype=combined | timechart count by status limit = 10
//Sourcetype is combined meaning that it takes all the data from the app into account. Timechart is the command used to create a chart. Limit equals to 10 meaning only the top 10 distinct values for status. Status shows the HTTP status code.

2. Show the most common customer operating systems and which web browsers are experiencing the most failures.
- index=main sourcetype=access_combined status >= 400| timechart count by useragent limit=5 useother=f
//Index is set to main meaning it specifies the index for search. Status is set equal to greater than 400 because these typically represent client 4xx or 5xx HTTP errors. Displays the top 5 users agents and useother is set to false so it won't show up in the chart.

3. Show lost revenue from the Buttercup Enterprises website.
- sourcetype="access_combined" action=purchase status>=400 | lookup product_codes.csv product_id | timechart sum(product_price)
// Action is set to purchase because it narrows down the search for purchases, used lookup to join the results from the product_codes csv and adds it to the splunk data, enriching the data by adding fields, product_price, from the CSV. Sum(product_price) calculates the total sum of the product_price field for events and shows it in the graph with timechart.

4. Show website activity by geographic location.
- index=main sourcetype="access_combined" |iplocation clientip | geostats count by City
// Iplocation clientip enriches each event by using the clientip field (assumed to contain an IP address) to retrieve geographic data. Geostats count by City aggregates the data geographically. By City groups the counts based on the city.
