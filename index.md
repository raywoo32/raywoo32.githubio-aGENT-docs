### Broad Overview  

>aGENT is a Gene Regulatory Network (GRN) curator and visualizer for the [bar](http://www.bar.utoronto.ca/). GRN were manually curated from the literature, stored in a mySQL database and then visualized using javascript libraries React.js and [Cytoscape.js.](https://js.cytoscape.org/) 

## Setting Up 
### Setting Up Front End for Development 
#### Prerequisites

1. Install [Node.js](https://www.npmjs.com/get-npm)
2. Git installed
2. Request access to Private git [repo](https://github.com/VinLau/aGENT)

#### Installation 

```markdown
git clone https://github.com/VinLau/aGENT
cd aGENT
npm install
npm audit fix #If flag given 
cd node_modules
cd react-search-box
npm install 
cd ..
cd ..
npm run start 
```

#### Running 

When the command below is run, you will automatically have aGENT running on localhost:3000/AGENT 

```markdown
npm run start 
```

### Setting Up Backend for Development 

Adding items to the backend has the following organization
1. Manual curation in specific .sif file format, examples found [here](https://github.com/raywoo32/grnAnnotation)
2. Use [script](https://github.com/raywoo32/readSIF) to upload to mySQL [database](https://github.com/VinLau/BAR-interactions-database)  

#### Prerequisites

1. Get username, port number and password for the bar
2. Get username, password for mySQL user 

#### ssh into the BAR

```markdown
ssh -p <PORTNUM> <BAR-USERNAME>@bar.utoronto.ca 
```

#### Access mySQL database 

ssh into the bar first! 

```markdown
mysql -u <MYSQL-USERNAME> -p #when prompted type password
use interactions_vincent_v2;
```

## Organization
### Back End Organization 

The crux of the backend is the mySQL [database](https://github.com/VinLau/BAR-interactions-database) which is accessed via APIs by the front end app. 

#### Important Files 

- [sif files](https://bar.utoronto.ca/GRN_SIF_Files/) are stored for users to download and view as well as a backup to the mySQL database (add with [this](https://github.com/raywoo32/grnAnnotation))
- [grn images](https://bar.utoronto.ca/GRN_Images/) 

Backup sif files: [here](https://github.com/raywoo32/grnAnnotation) and [here](https://github.com/VinLau/aGENT-GRNs)

#### APIs

The APIs query information stored on the BAR, which can then be visualized by the front end app. 

1. [interactions api](https://bar.utoronto.ca/interactions_api)
2. [suba4](https://bar.utoronto.ca/~vlau/suba4.php)
3. [get samples](https://bar.utoronto.ca/~bpereira/webservices/get_sample/getSample.php)
4. [interactions2](https://bar.utoronto.ca/interactions2/)

You can find example calls in aGENT/src/helper-fns/api-calls.js

### Front End Organization 

The front-end of aGENT was made using [create react app](https://reactjs.org/docs/create-a-new-react-app.html). There are 3 main pages in the app each which will be explored below: 

1. Home Page
2. List GRNs
3. GRN visualizer 

#### Home Page
> The home page is where the user is presented with the logo and a search box to search by gene/tag/experiment/condition. 

When the search query is complete aGENT will present the 

#### Main Components 
#### Feature Components 

### Current Bug List 

- On installation react-search-box does not auto install 
- 

### TODO
1. Update schema https://github.com/VinLau/BAR-interactions-database
2. Update docs for readSIF 
3. Merge grn curation .sif files to one repo 
4. Make aGENT github user with all important working repositories forked? readSIF, aGENT, grnAnnotations, mySQL database, apis https://github.com/VinLau/react-search-box Stop deprecation and disuse. Or make location on BAR very clear. 


```markdown
![Image](src)
```

