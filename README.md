# Visualizing data for Adverse Drug Reactions by [Vector Consulting](http://www.vectorconsulting.com/)

## Live Prototype
[Click here to view the prototype](http://162.243.149.238/index.html)

## Description
This tool presents the top ten adverse reactions reported. While the reports to FDA do not require that a causal relationship between a product and event be proven, and reports do not always contain enough detail to properly evaluate an event, we have provided the drill down on each adverse reaction to reflect the occurrence of SUSPECT drug reported in the adverse action report. The intent was to explore, understand and consume as many APIs related to adverse reaction and come up with an analytics solution that gives meaningful insight to further identify area of research. Prototype was built using data from https://open.fda.gov.

The tool also lays out the data of food recall events per state for USA on a map, indicating which states have the most instances of food recall.

## Source code

The Source code for the prototype is hosted at https://github.com/vectorconsulting/openfda-prototype

All the source files, installation documentation, readme.md file, supporting documentation, license file, evidence, test results etc. are uploaded here.

## Container Image

The container image for the prototype can be found - https://registry.hub.docker.com/u/anilallewar/openfda-prototype/

## Approach (750 Word Limit):

In this phase we went through the available APIs and interviewed sample users to come up with a use case that would demo a solution that provides meaningful insight into the adverse reaction data. The use case described above provides drill down exploration capability from list of "Adverse drug reactions" to list of drugs that caused those reactions.

While building the prototype, we followed "US Digital Services Playbook" as our guide to ensure we deliver successful Agile Service prototype.


### Requirements
The team began looking at the APIs of openFDA and understanding the capabilities of the system. This was followed by identifying a set of users and conducting the interviews. The data from the interviews and the research on the APIs lead us to requirements for "Visualizing data for adverse drug reactions" and "Visualizing Food recall patterns". Next step was to identify the User Stories on an Internal Jira Server. These User Stories also recorded Functionality, Usability 508 compliance, Security and Technical considerations of the feature.

### Team:

The team included 
 *	One Architect (Rohit Ghatol), 
 *	Two Frontend Developers (Mayuresh Phadke and Nikhil Walvekar) and 
 * One Devops Engineer (Anil Allewar) 
 
The team was lead by Mayuresh Phadke. Each team member contributed to the Git repository.
as evidenced by commits made by developers and devops engineer to the https://github.com/vectorconsulting/openfda-prototype/commits/master .

### User Experience Design
One of our experienced Team member analysed the end to end requirements, and proposed an high level flow of the Application. This was followed by quick picture based mockups and finally a working mockup in HTML5. Our overall design was inspired by industry UI Conventions by Twitter BootStrap and Zurb Foundation. 
One of the key requirements was to ensure the UI is responsive and accessible by any Modern Smartphone browsers and we think it meets the requirements

### Technical Design

The first step was to determine top 10 adverse reactions.  The query returns medicine name as “term” and the count. No options was available to apply order by but removing the limit indicated that the results returned are in descending order. It returned the data in descending order.

```
https://api.fda.gov/drug/event.json?search=seriousnesslifethreatening:1&limit=10&count=patient.drug.medicinalproduct.exact
```

Once user chosses the drug, they can drill down numbers further by gender (patientsex = 1 for male; patientsex=2 for female and patientsex=0 for Unknown) using the URL format below.

```
https://api.fda.gov/drug/event.json?search=seriousnesslifethreatening:1+AND+patient.drug.medicinalproduct:"ASPIRIN"+AND+patient.patientsex:1&limit=10
```

Since the API doesn’t permit returning all, we used combination of limit and skip to paginate the results. In the example below, the results would skip the first 100 records and give the next 10.

```
https://api.fda.gov/drug/event.json?search=seriousnesslifethreatening:1+AND+patient.drug.medicinalproduct:%22ASPIRIN%22+AND+patient.patientsex:1&limit=10&skip=100
```

These are the other serious parameters that can be used in the first filter

```
https://api.fda.gov/drug/event.json?search=seriousnessdeath:1
https://api.fda.gov/drug/event.json?search=seriousnesscongenitalanomali:1
https://api.fda.gov/drug/event.json?search=receivedate:[2004-01-01+TO+2015-07-04]+AND+seriousnesscongenitalanomali:1
https://api.fda.gov/drug/event.json?search=seriousnessdisabling:1
https://api.fda.gov/drug/event.json?search=seriousnesshospitalization:1
```

For visualizing Food recall we used the API
```
https://api.fda.gov/food/enforcement.json?&count=state
```
Using this API we laid out the numbers for food recall events per state on the map of USA.
The states were color coded, with the states having colors according to instances of food recall. The color coding is explained on the page.

We choose to keep the Design as Simple as Possible, using industry conventions for rapid development and low cost. 
Following Frameworks were used for building this Prototype

* [Twitter BootStrap](http://getbootstrap.com/) - For Responsive and Modern UI Design
* [Angular Js](https://angularjs.org/) - For JavaScript based Single Page Application
* [Zing Charts](http://www.zingchart.com/) - For Charts with export options
* [D3JS Visualization Framework] (http://d3js.org/) - For Data Visualization
* [Gulp](http://gulpjs.com/) - Build tool for Javascript world
* [Docker](https://www.docker.com/) - For Running the "Adverse Drug Reactions" Docker Image



 
### Deployment
Dockerfile was defined along with the Source Code. On the Development environment Docker Container was built and pushed to docker hub. On the Digital Ocean Production machine, a Docker Machine was procured and Container Image was pulled and run.

```
Docker Image - https://registry.hub.docker.com/u/anilallewar/openfda-prototype/
```

#### Instructions to Deploy Docker Image

* Procure a Machine with Docker Installed. e.g Docker Droplet on DigitalOcean
* $>docker pull anilallewar/openfda-prototype
* $>docker run -d -p 80:8000 anilallewar/openfda-prototype


Open the url http://&lt;&lt;ip addresss&gt;&gt;/index.html in browser
