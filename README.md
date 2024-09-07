
# Incident_Reports_Analysis_SQL

Let's imagine that the client for this case study is a start-up cruise company, asking to analyze data of the reported incidents to understand trends and identify opportunities to increase guest satisfaction. All the guest complaints get recorded in the system, from where we are able to pull out the voayge start dates, cabin numbers, incident category and subcategory.

**Technologies**

To complete this project I worked in:

-DB Browser for SQLite

-Tableau Public

Attached you can find the csv. data as well as a project file of our analysis.

Here are some of our findings that we were able to present to our stakeholders as opportunitties for future improvements.
Illustrations were created in Tableau Public.

+ Top 10 Incidents reported, by category: most frequent issues that happend to guests and have a huge impact on the experience:
  
```
SELECT COUNT (*) AS "Amount_of_Incidents",
Incident_Category
FROM Incident_Reports_Analysis_SQL
GROUP BY Incident_Category
ORDER BY "Amount_of_Incidents" DESC
LIMIT 10
```

![image](https://github.com/user-attachments/assets/d9f7c875-2bf4-426d-a125-a411e87a9bc5)

+ Most reported category is related to Cabin, we created a breakdown by subcategory. Stakeholders want to focus on this area to minimize the amount of guest complaints:

```
SELECT Incident_Subcategory, 
count(*) AS Total_reports_by_Subcategory
FROM Incident_Reports_Analysis_SQL
WHERE Incident_Category = 'Cabin'
GROUP BY Incident_Subcategory
ORDER BY Total_reports_by_subcategory DESC
```

![image](https://github.com/user-attachments/assets/2c215c35-0ea7-48ec-b3f1-e43f90c3979b)


+ Top 5 Closed Unresolved incidents by Subcategory: Stakeholders main goal is to look at improvements in the specific areas:

```
SELECT
 Incident_Category,
 Incident_Subcategory,
 Status
FROM Incident_Reports_Analysis_SQL
WHERE Status = 'Closed-Unresolved'
GROUP BY Incident_Subcategory
ORDER BY Incident_Category
```

+ Percentage of reports by Guest Type: Stakeholder wants to make sure our VIP guests are not facing major issues:

```
SELECT Guest_Type, ROUND(count (*) *100.0/sum(count(*)) over(),2) AS 'Percent_from_Total_Incidents'
FROM Incident_Reports_Analysis_SQL
GROUP BY Guest_Type
```

![image](https://github.com/user-attachments/assets/ba837405-02a2-4ae6-a683-c490102825ef)



+ Incident reports related to food allergies - our Stakeholder's priority is safety of their guests and they want to put effort to make it right:

```
SELECT count (*) AS Amount_of_incidents_related_to_Food_Allergies
FROM Incident_Reports_Analysis_SQL
WHERE Description LIKE "%allergy%" OR Description LIKE "%allergies%"
```
