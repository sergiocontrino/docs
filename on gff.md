the gff general parsing can be make working again (**TODO**: why was not enabled?)

in the **intermine** repo:

file *bio/sources/settings.gradle*: you need to add the source bio-source-gff
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

`./gradlew bio-source-gff:install --stacktrace`

add to your *project.xml* file something like (assuming human data -> taxid=9606): 

```xml
<source name="exgff3" type="gff">
  <property name="gff3.taxonId" value="9606"/>
  <property name="gff3.seqClsName" value="Chromosome"/>
  <property name="src.data.dir" location="/datadir"/>
  <property name="gff3.dataSourceName" value="yoursourcename"/>
  <property name="gff3.dataSetTitle" value="your dataset title"/>
  <!-- add licence here -->
  <property name="gff3.licence" value="https://creativecommons.org/licenses/by-sa/3.0/" />
</source>
```

if you deal with human data, then you need to check also the file
*bio/core/src/main/resources/gff_config.properties*
where there are some default settings.

**important**: the third field must be a feature that is modelled in the mine. 
so if one of the core ones (gene, protein, etc) there is nothing to do, otherwise you need to add to the mine model your new entities.



