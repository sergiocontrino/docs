the gff general parsing can be make working again (**TODO**: why was not enabled?)


<h2> install locally intermine </h2>

```
git clone https://github.com/intermine/intermine.git 
```
cd intermine

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


* **important**: the third field in the gff file must be a feature that is modelled in the mine. 
so if one of the core ones (gene, protein, etc) there is nothing to do, otherwise you need to add to the mine model your new entities.



