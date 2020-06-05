# Developer Documentation
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

* * * 

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
3. Cytoscape Container 

#### Home Page
The home page is where the user is presented with the logo and a search box to search by gene/tag/experiment/condition. When the search query is complete aGENT will present the List GRNs page where all relevant GRNs will be shown. 

#### List GRNs
For each GRN the following is shown:
- GRN title
- Tagged information
- GRN description
- GRN Image

The choice of the Home.js page and the ListGRNs.js page are for ease of integration with ePlant. 

#### Cytoscape Container 
This is the crux of the exploratory features of the app. The broad flow of information is as follows

- Information in the List GRNs page is passed 
- The externalSource API which accesses information on the GRN paper is queried
- Both the MenuSideBar and Ctyo are rendered and passed props

Importantly, cytoscape container has context - a sort of global state which stores the cytoscape object (among other things). 

##### Cytoscape Initialization 
Occurs in Cyto.js. The information flow is as follows
- interactions API is queried 
- Cytoscape object is initialized in cytoscape-init.js. Along with the base initialization, the following occurs:
  - default layout rendering 
  - event listeners
  - subcellular localization information 
  - gene symbol and description
  - a tooltip 
  - styling from cytoscape-styles.js
- The cytoscape object is bound to the context

##### Context 
The context is a sort of global state that is accessible by its children. The most important part of the context is the cytoscape object. Read more about context [here](https://reactjs.org/docs/context.html#reactcreatecontext)

##### MenuSideBar 
The MenuSideBar houses the on demand exploratory features. Below is an overview of the feature and implementation. This section does not go into the visualization and design decisions, see "Front end visualization decisions" below. 

1. Venn
> The Venn Diagram allow a user to upload any other GRNs, view overlap
2. Expression Overlay
>
3. Centrality
> 
4. Navigate
> A tool to centre and move the cytoscape object in the window. Nav.js 
5. Layout
6. Download
7. Find Selected Targets
8. Motifs
9. Shortest Path
10. Search Box

* * * 

## Front end visualization decisions 

There are a certain number of ways to display information in a graph object that are supported by Cytoscape.js. Different information is conceptually visualized differently to make rich and clear data integration. 

See the chart below for information on what different visual ideas represent. 

| Visual Cue              | Feature                                                             | 
|:------------------------|:--------------------------------------------------------------------|
| Edge colour             | Edge type (yellow ppi and green pdi)                                | 
| Edge width              | Part of Path Display (Motifs and Shortest Path)                     |
| Node colour             | Expression Overlay                                                  | 
| Node selected           | Part of Path Display (Motifs and Shortest Path)                     | 
| Node size               | Centrality                                                          |
| Opacity                 | Venn, clicking the venn makes item outside the clicked transluscent |
| Edge arrow shape        | Edge type (activation or repression)                                |
| Border colour           | Cellular localization                                               |

* * * 

## File System in Front End 

The most important files and file organization in the front end app are explained here. 

node_modules
public
src
package-lock.json
package.json

/aGENT    
├── node_modules // is created after npm install is called, stores dependencies    
├── public   
├── src   
│   ├── components    
│   │   ├── smart-components //Components with their own state and their styling   
│   │   ├── dummy-components //Components without a state and their styling    
│   ├── cytoscape // Related to rendering the cytoscape object   
│   ├── helper-fns    
│   ├── react-context   
│   ├── static-assets  
├── package.json //Package Settings   
├── package-lock.json //Describes dependencies   


* * * 

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

