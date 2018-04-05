# Graph-KB
-------------------------
Graph-KB is a general graph exploring tool, which has following functionalities:
	1. Finding K-shortest path between two nodes.
	2. Exploring paths around a given node.
	3. Infering relations between source and target node of a given path.

The knowledge-graph that is used to build this tool is the **UMLS dataset** which is freely available at [UMLS website](https://uts.nlm.nih.gov/home.html)

In order to use Graph-Kb you should have:
* Neo4j 
* JVM
* Python 3.x

The following tutorial supports only **Unix-Systems**

##[Neo4j-Setup]:

### Neo4j 3.2 requires the Java 8 runtime. To install java 8 on ubuntu:
* `echo "deb http://httpredir.debian.org/debian jessie-backports main" | sudo tee -a /etc/apt/sources.list.d/jessie-backports.list`
* `sudo apt-get update`
* `sudo add-apt-repository ppa:webupd8team/java`
* `sudo apt-get update`
* `sudo apt-get install oracle-java8-installer`


### Add the repository: 
* `wget -O - https://debian.neo4j.org/neotechnology.gpg.key | sudo apt-key add -`
* `echo 'deb http://debian.neo4j.org/repo stable/' | sudo tee -a /etc/apt/sources.list.d/neo4j.list`
* `sudo apt-get update`

### Installing NEO4j:
- [] `sudo apt-get install neo4j=3.2.2`

### To check if NEO4j is installed

	- [] `systemctl start neo4j` to start neo4j.
	- [] `systemctl status neo4j` to check the status whether neo4j is running.
	- [] `systemctl stop neo4j` (After testing whether neo4j intance is running, please disconnect the neo4j service).

### Change the password of NEO4j server(Important)
	- [] After starting the neo4j service (`systemctl start neo4j`) please open [http://localhost:7474/browser/]

	- [] **Username:** *neo4j* **Password:** *neo4j*, this will redirect you to set a new password.

###  Add server plugin to the NEO4j  
	- [] Stop the neo4j instance before performing below steps (`systemctl stop neo4j`)
	- [] `cp  com.dfki.LT.OntologyExplorer-1.0-SNAPSHOT.jar /var/lib/neo4j/plugins/`


## Database for Neo4j:

### Create database for Neo4j (If you don't have graph.DB file)
* Here we used neo4j's bulk import utility:
* In order to create DB for NEO4j we need 4 files(2 header files and 2 content files)

### Header files # 
* ### nheader(:ID, ConceptID, ConceptName) 
 ConceptID and ConceptName are the properties of the node

* ### rheader (:START_ID,:END_ID,:TYPE,weight)
  * :START_ID,:END_ID are the node ID
   * :Type is the vocabulary
   * RelationLable is the relation name
   * weight is the edge weight

### Content file #
* ### nodefile 
This file contains data for node in 3 columns comma separated, without header.

* ### Relationfile
This file contains data for relation in 4 columns comma separated, without header.


#### Script for creating db #

#### neo4j-import --into graph.db --nodes:UMLSConcepts "nheader,node" --relationships "rheader,relation"  --skip-duplicate-node true

* --nodes:UMLSConcepts : node label, we are using only one label for entire dataset then we provide this label in script itself, but when there are different label then we provide it in file
*  "nheader,node" : Name of the node header file and node content file
* "rheader,relation" : Name of the relation header file and relation content file
* --skip-duplicate-node true : We skip the duplicate node


After we run the above script then a graph.db folder is created in the present directory, we can then paste this folder in the /var/lib/neo4j/data/database/




### Default location of neo4j graph DB is under folder /var/lib/neo4j/data/databases/.

 ### To change the graph db to any other folder:

    * open noe4j.conf by: gedit /etc/neo4j/neo4j.conf
    * replace line dbms.directories.data=/var/lib/neo4j/data with dbms.directories.data=<folder of your choice>
    * copy the graph.db into <folder of your choice>/databases/
    * Change the password for this new DB at localhost:7474 to user: neo4j password: 123 (we can change this as well, but then we have to provide same in web application config file as well)




### Add the graphDB to the data folder of NE4j (folder attached in the repo)

* cp -r graph.db /var/lib/neo4j/data/databases/


### Everytime we add a new graph.db file to the neo4j we have to stop the neo4j instance otherwise we corrupt the database

### For UI 
* unzip the shortest-path-1.0-SNAPSHOT.zip 
* To start the UI service cd /bin
*  And then ./shortest-path


