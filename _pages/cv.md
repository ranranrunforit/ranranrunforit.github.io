---
layout: archive
title: "Curriculum Vitae / Resume"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}


Education
======
* M.C.S. in Data Science, University of Illinois Urbana-Champaign, May 2025
* B.C.S. in Computer Science with Co-operative Education program, Dalhousie University, August 2018
  * Specialization of Data Science
  * Specialization of Artificial Intelligence and Intelligent Systems
  * Teaching Assistant For Discrete Math & Network Computing


Certifications
======
* AI Agents Course - Certificate of Excellence, Hugging Face, April 2025
  * Credential ID: ranranrunforit
  * Credential URL: [link](https://huggingface.co/datasets/agents-course/certificates/resolve/main/certificates/ranranrunforit/2025-04-24.png)
* Deep Reinforcement Learning Course - Certificate of Excellence, Hugging Face, April 2025
  * Credential ID: ranranrunforit
  * Credential URL: [link](https://huggingface.co/datasets/agents-course/certificates/resolve/main/certificates/ranranrunforit/2025-04-24.png)


Work Experience
======
* Software Engineer, NCS; Beijing, China, September 2019 – December 2022
  * Use C#, .NET 4.0, Entity Framework, and SQLite to develop desktop software, ICP Expert, that feature user-friendly worksheet interfaces, easy-to-operate method development, and preset methods to simplify chemical analysis.
  * Program AI based algorithms to help reduce speculation in method development and automate troubleshooting.
  * Deploy a steel sample detector that is capable of processing steel sample images with glare or shadows by using Keras Segmentation models in Python and achieving a pixel accuracy of 99.8%.
  * Implement and test a C# testing software for Electric Circuits and Optical Room that sends and responds data to CAN-bus Device Driver.
  * Architect, design, and test a system that connects between the client software and the spark-optical emission spectrometer software used C#, .Net Framework.
  * Act as the primary liaison between client and technical team with key project management tasks such as responding to client feature requests, providing training for new functionalities and understanding client business needs.
Technology Used: C#, Python, SQLite, Visual Studio, C#, .NET Framework, .NET Core, NHibernate/Entity Framework

* Data Engineer, Awesense Inc.; Vancouver, Canada, September 2018 – July 2019
  * Automated efficient, robust, and scalable big data processing solutions for utilities data.
  * Designed, optimized, and troubleshoot distributed processing infrastructure using Spark and Scala.
  * Developed statistical and computing methods to analyze data at the PB level. Revamped ETL processes to integrate unstructured data from multiple sources.
Technology Used: Python, SQL, Scala, Spark, Zeppelin, Google Geocode API, Google Place API, Google BigQuery, ETL

* ASP.NET/SQL Developer, Maritime Travel; Halifax, Canada, September 2017 – January 2018
  * Developed full-stack web app using MVC ASP.NET, C# and MySQL in a test-driven development setting.
  * Improved and optimized workflow by implementing an infrastructure for the platform.
  * Implemented integration with Campaigns Monitor by creating a tool that extracts JSON data from system-wide search database and provides data to the company website.
Technology Used: ASP .NET, C#, Visual Basic, MySQL, Express.js, Node.js, AWS, C, Visual Studio, SQL, SAS, MongoDB, JSON
 
* Software Developer - Data, Densitas Inc; Halifax, Canada, December 2016 – April 2017
  * Launched a breast density application to correct for rater bias on a continuous scale using Python, TensorFlow and implemented machine learning models such as SVM and Random Forest.
  * Created a Data Mining models to analyze and classify electronic mammogram data using R and R Studio.
  * Prototyped and implemented a new feature that allows users to match approximate strings between multiple DB.
Technology Used: Python, R, R studio, TensorFlow, SVM, Random Forest, PANDAS, SQL, NumPy, Sklearn, Deep Learning

* IT Developer, Government of Nova Scotia; Halifax, Canada, May 2016 – August 2016
  * Implemented a VB .NET web application that allows users to post, search and download data.
  * Developed an Express.js user interface that is responsive to 60+ different user setting.
Technology Used: VB .NET, Express.js, Node.js, Microsoft Azure
  
Skills
======
* Programming languages: C#, Python, SQL, R, MATLAB, Java, SQL, Bash, etc.;
* Proficient in Python packages: PyTorch, Numpy, Pandas, Matplotlib, PyHealth, Keras, Sklearn, Tensorflow, Flask, Django, etc.;
* Proficient in R packages: tidyverse, lubridate, glmnet, readr, dplyr, xgboost, caret, randomForest, text2vec, slam, pROC, ggplot2,  shiny, recommenderlab, etc.;
* AI/ML framework: PyTorch, Tensorflow, Hugging Face, CUDA, Keras, Sklearn, etc.;
* Cloud computing: AWS S3, EC2, Lambda, API Gateway, Step Functions, EKS, CloudFormation, CloudWatch, DynamoDB, etc.; GCP, BigQuery; Docker containers, etc.;
* Software development tools: IntelliJ, Jenkins, Git, etc.

Projects
======
  <ul>{% if site.project_category %}
  {% for category in site.project_category  %}
    {% assign title_shown = false %}
    {% for post in site.projects reversed %}
      {% if post.category != category[0] %}
        {% continue %}
      {% endif %}
      {% unless title_shown %}
        <h2>{{ category[1].title }}</h2>
        {% assign title_shown = true %}
      {% endunless %}
      {% include archive-single-talk-cv.html %}
    {% endfor %}
  {% endfor %}
{% else %}
  {% for post in site.projects reversed %}
    {% include archive-single-talk-cv.html %}
  {% endfor %}
{% endif %}</ul>

<!---
Projects
======
  <ul>{% for post in site.projects reversed %}
    {% include archive-single-talk-cv.html %}
  {% endfor %}</ul>

Python
======
  <ul>{% for post in site.python reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
  
R shiny
======
  <ul>{% for post in site.rshiny reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

-->
<br/>

Hire Me
======
&nbsp; <i class="fa fa-file-download"></i> &nbsp; **[Download Resume]({{ site.url }}/files/Chaoran Zhou Resume.pdf)**
