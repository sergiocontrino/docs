the gff general parsing can be make working again (**TODO**: why was not enabled?)
<h2>step 0: prepare your intermine environment </h2>
for example: in your home directory $HOME
```
mkdir .intermine
mkdir git
```
in your .intermine dir put your properties file mymine.properties

in your git directory place your mine, follow instructions [here](https://intermine.readthedocs.io/en/latest/get-started/create-your-mine/) . 

Note: 
```
cd git
git clone https://github.com/mygithub/mymine.git
```

Alternatively, you could just use a checkout of biotestmine, see [here](https://github.com/intermine/biotestmine/wiki)

Note:
```
cd git
git clone https://github.com/intermine/biotestmine
```

<h2> install locally intermine </h2>

```
cd git
git clone https://github.com/intermine/intermine.git 
cd intermine
```

edit the file *bio/sources/settings.gradle*: you need to add the source bio-source-gff (add the lines marked with **+** below)

```
+':bio-source-gff',
 ':bio-source-go',
```
and 
```
+project(':bio-source-gff').projectDir = new File(settingsDir, './gff')
 project(':bio-source-go-annotation').projectDir = new File(settingsDir, './go-annotation')
```
then:

```
cd bio/sources
./gradlew bio-source-gff:install --stacktrace
```

<h2> in your mine add your gff source </h2>

add to your *project.xml* file something like (assuming human data -> taxid=9606): 
git clone https://github.com/intermine/biotestmine
```xml
<source name="exgff3" type="gff">
  <property name="gff3.taxonId" value="9606"/>
  <property name="gff3.seqClsName" value="Chromosome"/>
  <property name="src.data.dir" location="/datadir"/>
  <property name="gff3.dataSourceName" value="yoursourcename"/>
  <property name="gff3.dataSetTitle" value="your dataset title"/>
  <!-- add licence here -->
  <property name="gff3.licence" value="https://chttps://github.com/intermine/intermine.gitreativecommons.org/licenses/by-sa/3.0/" />
</source>
```
<h2> troubleshooting </h2>

* if you deal with human data, then you need to check also the file
*bio/core/src/main/resources/gff_config.properties*
in your intermine directory
where there are some default settings.

<h2>Issues with your gff files</h2>

* **important**: the third field in the gff file must be a feature that is modelled in the mine. 
so if one of the core ones (gene, protein, etc) there is nothing to do, otherwise you need to add to the mine model your new entities.

to run a build, i had to change all the various 'association' fields (Exterior_Association, Health_Association, Reproduction_Association, etc) to something in the model. I used 'gene'

- some of the records are missing the locations, and the '.' to mark its absence.
you need to have something like 

>Gene . . . . .

rather than 

>Gene   . . .

- in the attributes field (the last one) you must have name=value pairs. sometime this is not the case
e.g. 
>FlankMarkers;

in 

>Chr.16	Animal QTLdb	Exterior_Association	62860102	62943884	.	.	.	QTL_ID=17668;Name="Maternal infanticide";Abbrev=MATINF;PUBMED_ID=21303561;trait_ID=538;trait=Maternal infanticide;FlankMarkers;VTO_name="parental behavior trait";Map_Type=Linkage;Model=Mendelian;peak_cM=57.2;Significance=Significant;P-value=0.009;Likelihood_Ratio=9.485;gene_ID=100144487;gene_IDsrc=NCBIgene

and that breaks the parsing.


