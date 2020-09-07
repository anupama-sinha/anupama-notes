### Definitions
> Solr is the popular, blazing-fast, open source enterprise search platform built on Apache Lucene (Refer [Source](https://lucene.apache.org/solr/))

> Solr is a standalone enterprise search server with a REST-like API. You put documents in it (called "indexing") via JSON, XML, CSV or binary over HTTP. You query it via HTTP GET and receive JSON, XML, CSV or binary results. (Refer [Source](https://lucene.apache.org/solr/features.html))

### Download & Setup
* Pre-requisite : 64 bit OS + JRE 8
* [Download Link](https://www.apache.org/dyn/closer.lua/lucene/solr/8.6.2/solr-8.6.2.zip)
* Env Variables(SOLR_HOME : D:\solr\solr-8.6.2\solr-8.6.2\server\solr & Path : solr/bin)

### Points to Note
* NoSQL DB with indexing technology
* Inverted full text indexing(Word as queries searched in document)
* Solr Default Port : 8983
* Default cores provided(For indexing & searching) : techproducts, dih, schemaless, cloud
* post.jar - Helps in posting files to core and placed at solr-8.6.2\example\exampledocs.
* Reponse structure contains responseHeader and responses with tag <docs>
* Faceted Browsing - Narrow search results
* Solr has RESTful APIs

### Commands
* Help
> solr <any-cmd> -help
* Start Solr at Foreground
> solr start -f
* Start Solr at Background
> solr start or solr.cmd start
* Different port
> solr start -p 8984
* Shutdown Solr
> solr stop -p 8983
* Start with specific bundled example
> solr -e techproducts
* Status
> solr status
* Create Core
> solr create -c anupamacore
* Addirg files to Core
> java -Dc=anupamacore -jar post.jar anu500.xml

###Sample XMLs in solr-8.6.2\example\exampledocs(manufacturers.xml)
```xml
<add>
  <doc>
    <field name="id">adata</field>
    <field name="compName_s">A-Data Technology</field>
    <field name="address_s">46221 Landing Parkway Fremont, CA 94538</field>
  </doc>
  <doc></doc>
</add>
```

### Query with default example XMLs
* Find text "sd500" in documents in techproducts and return as json
> http://localhost:8983/solr/techproducts/select?q=sd500&wt=json

* Find name & id of all documents in techproducts with inStock=false 
> http://localhost:8983/solr/techproducts/select?q=inStock:false&wt=json&fl=id,name

* Find text "an500" in custom created core
> http://localhost:8983/solr/anupamacore/select?q=an500&wt=json

* Faceted Browsing with price range and grouping by category
> http://localhost:8983/solr/techproducts/select?q=price:[0 TO 400]&fl=id,name,price&facet=true&facet.field=category

### Sample Response of Query
* URL : http://localhost:8983/solr/techproducts/select?q=an500&wt=json
* Response 
```xml
{
  "responseHeader":{
    "status":0,
    "QTime":5,
    "params":{
      "q":"an500",
      "wt":"json"}},
  "response":{"numFound":1,"start":0,"numFoundExact":true,"docs":[
      {
        "id":"9885A777",
        "name":"Canon PowerShot AN500",
        "manu":"Canon Inc.",
        "manu_id_s":"canon",
        "cat":["electronics",
          "camera"],
        "features":["3x zoop, 7.1 megapixel Digital ELPH",
          "movie clips up to 640x480 @30 fps",
          "2.0\" TFT LCD, 118,000 pixels",
          "built in flash, red-eye reduction"],
        "includes":"32MB SD card, USB cable, AV cable, battery",
        "weight":6.4,
        "price":329.95,
        "price_c":"329.95,USD",
        "popularity":7,
        "inStock":true,
        "manufacturedate_dt":"2006-02-13T15:26:37Z",
        "store":"45.19614,-93.90341",
        "_version_":1677166124425281536,
        "price_c____l_ns":32995}]
  }}
```

### Configuration files in Standalone mode[Path : server/solr]
* solr.xml - server instance configurations 
* <core-name>/core.properties - core configurations such as names, locations and files in the core
* <core-name>/conf/solrconfig.xml - core configurations for field guessing, directories, query settings, spell checking, keyword highlighting and query * response formats
* <core-name>/conf/managed-schema - core configurations for field processing managed with two Solr tools 
* <core-name>/conf/schema.xml - core configurations for field processing managed by hand.
* data - To store data

* [Solr Core files includes last 5 core files.Either managed-schema/schema.xml is used.]

### Steps for application integration
1. Define schema(Solr's home directory + Configuration)
2. Deploy Solr
3. Feed Solr Documents for searching
4. Expose Search in application

### Spring Boot Implementation (** Work in Progress **)
###  POJO for Solr
```java
@SolrDocument(solrCoreName="product")
public class Product{
  @Field
  Long id;
  
  @Field
  String productName;
}
```

### Reference Links
* https://lucene.apache.org/solr/7_0_0/quickstart.html
* https://factorpad.com/tech/solr/reference/
* http://blog.comperiosearch.com/blog/2014/08/28/indexing-database-using-solr/
