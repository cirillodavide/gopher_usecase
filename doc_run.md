# Running Gopher
To run the application from the src directory execute the following command:
```
$ ./gopher
```

You can get a help message for its parameters running as ```$ ./gopher -h``` or ```$ ./gopher --help```.

You can specify an input FILE by using ```$ ./gopher -f FILES``` or ```$ ./gopher --files FILES```.
It can either be a gopher-generated file (for the input or the found paths), a file containing paths to input files, an specie taxonomy file, or a classifier file or directory. If no input taxonomy is specified, human taxonomy will be used.

A file containing paths to input files must start with ```GOPHER-FP``` followed with a line for each input file path: 
```CODE:/path/to/file```, where CODE can be:
```
Phenotypes for Phenotypes file
Genotypes for Genotypes file
Filter for Taxon/Filter file
GeneDictionary for Gene Dictionary file
Genes2Phenotypes for Genes to Phenotypes file
Genes2Genotypes for Genes to Phenotypes file
```
You can run an exploring command on the graph by using ```./gopher -c COMMAND (PARAMETERS)``` or ```./gopher --command COMMAND (PARAMETERS)```.
Multiples commands can be specified; they will be executed in the specified order. COMMAND and PARAMETERS can be:
* ```PATH typeORIGIN:typeDESTINATION:DISTANCE``` Searches for paths between elements with ID ORIGIN of a certain type (p for phenotype, g for genotype) to ID DESTINATION at a maximum DISTANCE.

    e.g. ```PATH p4:g7:5``` search for a path between phenotype with id 4 and genotype with id 7 within a maximum distance of 5.
* ```TYPE ORIGIN:DESTINATION:PATHTYPE``` Same as PATH, but limits the search of a path of type PATHTYPE, which describes the type of each element in the path. PATHTYPE must be a string of g(genotype), p(phenotype), G(gene). 

 	e.g. ```ppGgg``` search for paths starting at a phenotype, connected to another phenotype, then to a gene connected to a genotype, which is connected to the destination genotype.
* ```DIRECT``` Search all direct connections, which are paths of PATHTYPE pGg (or gGp).
* ```ADD``` TYPE:POSITION Add all connected elements of TYPE (p for phenotype, g for genotype) to current list of existing paths at a certain POSITION(f for front, b back). This command requires an input path list.
* ```ALTFILE FILE``` For each entry in the current list of existing paths, searches for alternative paths in the specified FILE. This File must be a Gopher-generated PATHS file.
* ```ALTDIRECT PATHTYPE(:typePART(:SEED))``` Searches all direct connections, and for each one searches for alternative paths of PATHTYPE, as described with TYPE. If PART (value indicating percentage, 1-99) is specified, only a random subset will be explored. According to type, the PART can be a (p)ercentage or a (v)alue. If SEED is specified, the random subset will be selected according to it.

	e.g. ```-c ALTDIRECT pGgg``` Searches for alternatives paths of type pGgg for ALL the direct connections.

	e.g. ```-c ALTDIRECT ppGg:p50``` Searches for alternatives paths of type ppGg for 50% of the direct connections.

	e.g. ```-c ALTDIRECT ppGg:v100:5``` Searches for alternatives paths of type ppGg for 100 of the direct connections. The seed to generate the random subset will be 5.
* ```ALTDISCON PATHTYPE(:typePART(:SEED))``` Searches all disconnected phenotypes and genotypes, and for each pair searches for alternative paths of PATHTYPE, as described with TYPE. If PART (value indicating percentage, 1-99) is specified, only a random subset will be explored.
* ```SORT```	Sorts the current list of existing paths, first by starting element ID, then by last element ID
* ```PATHFILE FILE:DISTANCE```	For each entry in the FILE, searches for paths between its phenotype and genotype at a maximum DISTANCE. The first FILE line must be either a 'p' (for phenotype) or a 'g' (for genotype), defining the type of the first element for the following entries. The following lines must be pairs of identifiers, separated by a space.
* ```TYPEFILE FILE:PATHTYPE```	For each entry in the FILE, searches for paths between its phenotype and genotype of type PATHTYPE, as described with TYPE. The lines must be pairs of identifiers, separated by a space.
* ```ALLPATHSDIRECT MAXLEN(:typePART(:SEED))``` Searches all direct connections, and for each one searches for alternatives paths of path types up to MAXLEN length. If PART (value indicating percentage, 1-99) is specified, only a random subset will be explored. According to type, the PART can be a (p)ercentage or a (v)alue. If SEED is specified, the random subset will be selected according to it. We recommend to redirect the output of this command.
* ```ALLPATHSDISCON MAXLEN(:typePART(:SEED))``` Searches all disconnected phenotypes and genotypes, and for each pair searches for alternative paths of path types up to MAXLEN length. If PART (value indicating percentage, 1-99) is specified, only a random subset will be explored. According to type, the PART can be a (p)ercentage or a (v)alue. If SEED is specified, the random subset will be selected according to it. We recommend to redirect the output of this command.
* ```ALLPATHS MAXLEN:type1:type2(:typePART(:SEED))``` For each pair of elements of type1 and type2, searches for alternative paths of path types up to MAXLEN length. If PART (value indicating percentage, 1-99) is specified, only a random subset will be explored. According to type, the PART can be a (p)ercentage or a (v)alue. If SEED is specified, the random subset will be selected according to it. We recommend to redirect the output of this command.
* ```CLASSIFY typeORIGIN:typeDESTINATION``` Searches for paths between elements with ID ORIGIN of a certain type (p for phenotype, g for genotype) to ID DESTINATION, for all path types described at the classifier files and calculates the connections likelyhood of the pair. A classifier file or directory must be defined by the use of the option -f or --file. If a directory is defined, all the files at the directory will be used. For each classifier, a connection likelyhood value will be calculated. Classifier file must fit the following pattern:

   The classifier FILE must start with either ```CON``` or ```DIS```, to define if the following data will be for CONnected or DISconnected.
   The following lines will contain the base value, or the mean and standard deviation for each kind of path.
   
   ```prio: VALUE``` defines the base value.
   
   ```PATHTYPE: MEAN, STDEV```	defines the mean and standard deviation for each kind of path.
   
   In the likely case that VALUE, MEAN or STDEV is a non-integer number, use a dot (.) instead of a comma(,) as decimal separator.
   When the base and all paths have been defined for either CONnected or DISconnected, leave a blank line before defining the other set. Make sure that both CONnected and DISconnected have the same set of PATHYPEs, and that the set of PATHTYPES is constant between files when using multiple files in a same execution.
   Example files can be found at  _[classifier files](../inputs/classifiers)_.

To generate a simplified input FILE use ```./gopher -g TYPE FILE``` or ```./gopher --generate TYPE FILE```.
Depending on TYPE, it can contain the resulting GRAPH or the generated PATHS. When executing multiple commands and generating PATHS, the last obtained list of PATHS will be the generated one.

For example ```./gopher -f INPUT.FP -g GRAPH OUTPUT.GF``` will generate a simplified input file (OUTPUT.GF) with the information from file INPUT.FP. And ```./gopher -f INPUT.FP -c DIRECT -g PATHS DIRECT.PF``` will generate a file (DIRECT.PF) which lists all the DIRECT  connections in the graph.

You can print a list of edges of the ontologies by using ```./gopher -e TYPE``` or ```./gopher --edges TYPE```.
This prints a list of all edges of a given TYPE. TYPE can be g(genotype), p(phenotype) or G(gene).

```./gopher -v (TYPE)``` or ```./gopher --avoid (TYPE)``` enables avoid option, avoiding edges of direct connections. If TYPE is defined, it avoid (p)henotypes edges, (g)enotypes edges or (b)oth. If it is not defined, it avoid both.

To print a summary of the number of elements and edges on the loaded ontologies, run ```./gopher -s``` or ```./gopher --summary```.

## Debug options
The following options are only enabled when NDEBUG is not defined.

You can enable printing a progress bar for the costly commands by using ```./gopher -p``` or ```./gopher --print```

Using ```./gopher -st COMMAND``` or ```./gopher --statistics COMMAND``` gathers and shows ontologies statistics. The possible commands are:
* ```acl(:TYPE)``` Shows the average number of connections to genes for each ontology level. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth)
* ```cl(:TYPE)``` Shows the number of connections to genes for each ontology level. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth)
* ```anl(:TYPE)``` Shows the average number of connections to the same ontology for each ontology level. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth)
* ```nl(:TYPE)``` Shows the number of connections to the same ontology for each ontology level. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth)
* ```el(:TYPE)``` Shows the number of elements for each ontology level. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth)
* ```l:DIV(:TYPE)``` Shows the average level for each rank. DIV specifies the number of divisions (or ranks) of the ontology. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth).
* ```c:DIV(:TYPE)``` Shows the average number of connections for each rank. DIV specifies the number of divisions (or ranks) of the ontology. TYPE can be used to specify the ontology: g(enotypes), p(henotypes) or b(oth).

# Running at MareNostrum 4
Since Gopher has a high memory usage, when running at MareNostrum 4, we recommend to add ```#SBATCH --constraint=highmem```  to you job script.
