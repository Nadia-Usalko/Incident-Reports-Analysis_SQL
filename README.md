
# Incident-Reports-Analysis_SQL

The following analysis summarizes synthetic data to define trends as well as opportunitites to improve the hotel operation based on customer feedback onboard a cruise ship. All the guest reports get recorded in the system, from where we are able to pull out the cruise start dates, room numbers, incident category and subcategory.

Attached you can find the csv. data as well as a project file of the analysis.

This analysis will allow stakeholders understand trends and identify opportunities to increase guest satisfaction. 


To complete this project I worked in:

-DB Browser for SQLite

-Tableau Public


Here are some of our findings that we were able to present to our stakeholders as opportunitties for future improvements.
Illustrations were created in Tableau Public.


1. Total amount of incident reports within the given time period:

```ruby
SELECT COUNT (*) AS Total_amount_of_incident_reports
FROM Incident_Reports
```
Output:

![image](https://github.com/user-attachments/assets/4138059f-b6c9-47d6-8bb4-8a016baf118e)


2. Top 10 categories reported by guests - the areas where stakeholders want to pay more attention to:

```ruby
SELECT COUNT (*) AS Amount_of_Incidents_Reports,
	Incident_Category
FROM Incident_Reports
GROUP BY Incident_Category
ORDER BY Amount_of_incidents_reports DESC
LIMIT 10
```
Output:

![image](https://github.com/user-attachments/assets/c75f4685-70aa-449c-8e9d-639749197ef9)

![image](https://github.com/user-attachments/assets/499af07b-04bc-4e9e-8a17-81ea5e9b4ee2)

  
3. As we discovered "Room" is the category guests are mostly concerned about. In order to gain deeper understanding of the issue, let's have a look at subcategories and the amount of reports:

```ruby
SELECT Incident_Subcategory,
	COUNT (*) AS Total_Room_Incidents_by_Subcategory
FROM Incident_Reports
WHERE Incident_Category = 'Room'
GROUP BY Incident_Subcategory
ORDER BY Total_Room_Incidents_by_Subcategory DESC
```

Stakeholders are now able to look into the issues in detail and make conclusions on potential future improvements: the highest amount of guest complaints is about TV/Room Controls as well as Shower/Plumbing. Serious maintenance evaluation might be the next step to define the action plan.

![image](https://github.com/user-attachments/assets/41d238d1-0326-46f5-9993-b0c7d74c4348)


4. In order to clean up the data let's correct a few lines: some reports still show "Assigned" status, however guests already left. We are going to update the status to "Closed-Unresolved":

```ruby
UPDATE Incident_Reports
SET Status = 'Closed-Unresolved'
WHERE Status = 'Assigned'
```

Let's find out the status of all these reports:

```ruby
SELECT COUNT (*) AS Amount_of_Reports,
	Status
FROM Incident_Reports
GROUP BY Status
```
Output:

![image](https://github.com/user-attachments/assets/61ecee74-6855-47c0-b854-cdffb14a0483)


Stakeholders might want to intriduce changes in customer approach to ensure every single issue gets resolved - 22 issues with no solution is not a great tendency for a business that is looking for loyal customers.

5. Top 5 unresolved incidents by subcategory. In order to understand challenges of customer service agents, let's dive deeper into unresolved issues:

```ruby
SELECT Incident_Category,
	Incident_Subcategory,
	Status
FROM Incident_Reports
WHERE Status = 'Closed-Unresolved'
GROUP BY Incident_Subcategory
ORDER BY Incident_Category
```
Output: 

![image](https://github.com/user-attachments/assets/2cb8d6bc-71c2-4932-b912-dc24436c4af0)

Bar and Dining Experience are definitely the areas to pay close attention to.

7. To give more insights to Bar team, let's display the descriptions of the reports related to Bar Experience for further investigation:

```ruby
SELECT Cruise_Start_Date,
	Incident_Category,
	Incident_Subcategory,
	Room_Number,
	Guest_Type,
	Status,
	Description
FROM Incident_Reports
WHERE Status = 'Closed-Unresolved' AND Incident_Category = 'Bar Experience'
ORDER BY Cruise_Start_Date
```

8. Incident reports related to food allergies - Stakeholder's priority is safety of their guests and they want to put effort into making it right:

```ruby
SELECT COUNT (*) AS Amount_of_Incidents_Related_to_Food_Allergies
FROM Incident_Reports
WHERE Description LIKE "%allergy%" OR Description LIKE "%allergies%"
```
Output: 

![image](https://github.com/user-attachments/assets/272f651d-0ad8-4123-a30c-fef24b6c7597)


9. Percentage of reports by Guest Type: Stakeholder wants to make sure that VIP guests are not facing major issues:

```ruby
SELECT Guest_Type,
ROUND(COUNT(*)*100.00/sum(count(*)) over(),2) AS Percent_of_reports
FROM Incident_Reports
GROUP BY Guest_Type
```
Output:

![image](https://github.com/user-attachments/assets/6150f047-01d8-4d27-9b7c-69297fc6bc85)

![image](https://github.com/user-attachments/assets/304dcad6-fef7-40fa-a09d-295ab2cdcc94)


The above analysis is giving insights to the operations team into improvement opportunites. 
I hope this research was insightful. Feel free to [follow me on Linked-In](https://www.linkedin.com/in/Nadia-usalko/) and check [my Tableau profile](https://public.tableau.com/app/profile/nadezhda.usalko/vizzes).
