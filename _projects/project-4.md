---
title: "Faculty Analytics: Find the Faculty you want to connect!"
excerpt: "Connection App using Python Dash, Flask, Neo4j, MySQL and MongoDB.<br/><img src='/images/Faculty Analytics.png'>"
collection: projects
---

The application scenarios involve users who are interested in connecting with a professor due to shared research interests, as well as professors who are seeking a university to join. 

The intended users of this application are professors who need research interest analytics for computer science faculty at universities in the United States. Moreover, professors who wish to add faculty to their university are also considered as target users.

The objectives of the application are to provide users with the ability to search for professors and access information about their research interests, publication history, and citation numbers. Additionally, users have the ability to add new professors to their university and remove professors who have left.


**[Faculty Analytics Demo](https://mediaspace.illinois.edu/media/t/1_uhjjx6f1)**


**Implementation**

The application has integrated Dash and dash-bootstrap-components for the frontend, which includes features such as maps, line graphs, dropdowns, sunburst charts and data tables. 

On the backend, the Dash app serves as the main application server based on Flask app. The "dbs" module contains all Python files related to database utilities, while "widgets" contains all Python files related to 8 widgets. 

Regarding the database, "neo4j_util.py" is used to connect to Neo4j database, "mongodb_util.py" to connect to MongoDB database, and "MySQL_util.py" to connect to MySQL database. 

The application utilizes several libraries including dash, neo4j, pandas, sqlalchemy, pymongo, plotly, flask, pymysql, certifi, geopy, and dash-bootstrap-components to achieve its functionality. RESTful API based on Flask app was used to wrap around Dash, with several routes registered to support restful API calls. 


**The design of the application**

The application involves 8 independent widgets using the Dash. The widgets could divided into two categories: querying and updating backend databases. The querying widgets include show citation trends, sunburst chart, a college on US map, display faculty, search publication by keyword, and search publication by faculty. The updating widgets include add faculty and delete a faculty.

-	The line chart widget used neo4j and mysql The dropdown queries were performed in neo4j, and the line chart was created from the data query in mysql.

-	The Find the publication by keyword widget used mysql database to query publications based on keyword input. The query joined multiple tables, providing abundant information including all authors of the publication, processed based on the dataframe returned from mysql query.

-	The Find the publication by faculty widget used mysql database to query publications based on faculty input. A callback function was used to return the data.

-	The Sunburst Chart widget used Neo4j database, with functions written to retrieve all keywords and institutes. Callback functions were used to interactively generate the chart showing the percentage of faculties in certain keyword areas.

-	The college on map widget used MongoDB database, with external API calls used to generate geography data for each university. To speed up performance, cache was used to save all information in a CSV file.

-	The Display Faculty widget used Neo4j database to query data, with callback functions used to generate two cards showing the data.

-	The Add Faculty widget used Neo4j database to query and add new faculty node and university node, creating a relationship edge between these new nodes. A callback function was used to show success/fail query information in the model.

-	The Delete Faculty widget used Neo4j database to query and delete the relationship edge between faculty node and university node, then delete the faculty node. A callback function was used to show success/fail query information in the model.


**The Usage**

-	The "Citations number trend on a line chart" widget allows users to select a keyword, faculty name, and year range to visualize the citation number of publications related to the keyword from the professor. 

-	The "Find publication by keyword" widget prompts users to input a keyword for the search area. It returns a table containing publication information and a list of authors. 

-	The "Find publication by faculty name" widget prompts users to type a faculty member's name in the search area. It returns a table containing all their publications sorted by number of citations and year.

-	The "Compare Keywords interested in Different Institutes" widget displays a sunburn chart depicting the total number of faculty interested in specific keywords from the center, with faculty names on the outer rings. Users can modify the chart by adding or deleting keywords and institute names in the dropdowns. 

-	The "CS Faculty Count in US Map" widget displays universities with professors in the CS department classified into 0-50, 51-100, and so on. Users can view university names, CS faculty count, and geo location by hovering over the markers on the map. 

-	The "Find the Faculty Data" widget displays two cards: one is a datatable showing faculty contact information and their top five interest keywords, while the other displays the faculty's name, position, and photo. Users can search for different faculty by selecting institutes and faculty names in the dropdowns. 

-	In the "Modify Faculty" section, the "Add Faculty" widget allows users to input five basic properties for a faculty. If a new faculty information is added, a model will show the success information. 

-	The "Delete Faculty" widget allows users to delete a faculty from a university by entering their name and the university name. If a faculty information is deleted, a model will show the success information. 

-	Users can access raw college and faculty information at http://127.0.0.1:8050/college_and_faculty or view faculty and keyword data at http://127.0.0.1:8050/faculty_and_keyword.


**Keywords**: Neo4j, MySQL, MongoDB  
**Packages**: Dash, Flask, Neo4j, pandas, sqlalchemy, pymongo, plotly, flask, pymysql, certifi, geopy, dash-bootstrap-components


