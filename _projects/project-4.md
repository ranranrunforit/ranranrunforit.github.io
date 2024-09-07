---
title: "Faculty Analytics: Find the Faculty you want to connect!"
excerpt: "Connection App using Python Dash, Neo4j, MySQL and MongoDB.<br/><img src='/images/Faculty Analytics.png'>"
collection: projects
---

The application scenarios involve users who are interested in connecting with a professor due to shared research interests, as well as professors who are seeking a university to join. 

The intended users of this application are professors who need research interest analytics for computer science faculty at universities in the United States. Moreover, professors who wish to add faculty to their university are also considered as target users.

The objectives of the application are to provide users with the ability to search for professors and access information about their research interests, publication history, and citation numbers. Additionally, users have the ability to add new professors to their university and remove professors who have left.


**[Demo](https://mediaspace.illinois.edu/media/t/1_uhjjx6f1)**


●	The "Citations number trend on a line chart" widget allows users to select a keyword, faculty name, and year range to visualize the citation number of publications related to the keyword from the professor. 

●	The "Find publication by keyword" widget prompts users to input a keyword for the search area. It returns a table containing publication information and a list of authors. 

●	The "Find publication by faculty name" widget prompts users to type a faculty member's name in the search area. It returns a table containing all their publications sorted by number of citations and year.

●	The "Compare Keywords interested in Different Institutes" widget displays a sunburn chart depicting the total number of faculty interested in specific keywords from the center, with faculty names on the outer rings. Users can modify the chart by adding or deleting keywords and institute names in the dropdowns. 

●	The "CS Faculty Count in US Map" widget displays universities with professors in the CS department classified into 0-50, 51-100, and so on. Users can view university names, CS faculty count, and geo location by hovering over the markers on the map. 

●	The "Find the Faculty Data" widget displays two cards: one is a datatable showing faculty contact information and their top five interest keywords, while the other displays the faculty's name, position, and photo. Users can search for different faculty by selecting institutes and faculty names in the dropdowns. 

●	In the "Modify Faculty" section, the "Add Faculty" widget allows users to input five basic properties for a faculty. If a new faculty information is added, a model will show the success information. 

●	The "Delete Faculty" widget allows users to delete a faculty from a university by entering their name and the university name. If a faculty information is deleted, a model will show the success information. 

●	Users can access raw college and faculty information at http://127.0.0.1:8050/college_and_faculty or view faculty and keyword data at http://127.0.0.1:8050/faculty_and_keyword.

**Keywords**: Neo4j, MySQL, MongoDB
**Packages**: dash, neo4j, pandas, sqlalchemy, pymongo, plotly, flask, pymysql, certifi, geopy, dash-bootstrap-components, flask

