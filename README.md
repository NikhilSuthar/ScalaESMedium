# ScalaES - Scala ElasticSearch Client Utility
ScalaES is a Scala-based ElasticSearch Client. It basically works on CURL method to execute Query on ElasticSearch. There are many Scala-based ElasticSeach client are available but most of them do not provide way to authentication with or without SSL configured ElasticSearch.

We have written a code that works with SSL and nonSSL configure ElasticSearch. We are still making more changes to make it more stable and also added more methods that can easily use in code like other ElasticSearch Client.

As of now below methods are available with this Tool:
1. **getElasticInfo**: Method to get Elastic Search Health Information.
2. **getSourceData**: Get all Data from ES index in JSON format
3. **searchWithQry**: Searching or filter out data on the basis of ElasticSearch Query
4. **searchByID**: Get/search ElasticSearch Index's document by passing _ID 
5. **dataParse**: Utility to parse final JSON output to Map
6. **indexExists**: Check Index exists or not.
7. **createIndex**: Create Index by passing mapping as Query 
8. **deleteIN**: Delete document on the basis of one field value
9. **deleteById**: Delete data on the basis of Index _id
10. **deleteByQry**: Delete Index documents on the basis of Query
11. **updateValue**: Update Column value for one particular _id by using passed Query.
12. **insertIntoES**: Insert a single document into Index.

**Key Point**:
* This tool does not require any client to be initialized for Connecting ElasticSearch.
* Index Type parameter in all methods need to pass "**_doc**"  for all ElasticSearch version above 7.x.x.
* This tool can work with all new & old versions of Elastic Search as well as Scala.
* you can pass any Query to Get, Delete or Update Data.


# Quick Start

To work with this you need to first download complate jar and have to add dependecies in SBT or Maven. 

[Click here](https://github.com/NikhilSuthar/ScalaES/raw/master/Jar/scala_elastic_client_0.0.1.jar) to download complete jar.

* You can add dependency of the jar in SBT using below two methods:

   * create **libs** folder in your project root folder and add dependencies in sbt as below[Recommanded]
 
       `unmanagedBase := baseDirectory.value/"libs"`

   * using path of external Jar as below

       `"com.elasticHttp.ESUtil" %% "ESClient" % "0.0.1" from  file:///..../scala_elastic_client_0.0.1.jar`
* Once it done, import jar in code as below:

       import com.elasticHttp.ESUtil.ESClient

# Syntax

Here is a list of the common use cases of all methods:

* **getElasticInfo** 
  * *getElasticInfo(aboutInfo,SSL)*: getElasticInfo method uses to get cluster level information such as Health, nodes alive, master etc.

    **Example**

     * For SSL disable mode: `getElasticInfo("master")`
     * For SSL Enable mode:  `getElasticInfo("master",true)`
              
    **Possible Value**
     * aboutInfo: 
        -   allocation
        -   shards
        -   shards
        -   master
        -   nodes
        -   tasks
        -   indices
        -   segments
        -   count
        -   recovery
        -   health
        -   pending_tasks
        -   aliases
        -   thread_pool
        -   plugins
        -   fielddata
        -   nodeattrs
        -   repositories
        -   snapshots
        -   templates
    * SSL - Optional [Default False]

* **getSourceData** 
  * *getSourceData(indexName,SSL)*: getSourceData method return source data of Index. It takes three parameters in which *indexName* and *typeofIndex* are compulasory and *SSL* is optional.
  
    **Example**
    
     The below command will return all Index Data.
     
     * For SSL disable mode: `getSourceData("test")`
     * For SSL Enable mode:  `getSourceData("test",true)`
   
* **searchWithQry**
  * *searchWithQry(indexName, typeofIndex, Query, SSL = false)*: searchWithQry method return source data of Index Like getSourceData method. But it takes one more parameter *Query* Unlike to getSourceData method. We can get conditional selected data by passing the condition as Query using this method.

    **Example**

      The below command will return all documents on the basis of Query passed. For the below case, it will return all.

        val query="""
           {
               "query": {
                 "match_all": {}
             }
           }""" 

      * For SSL disable mode: `searchWithQry("test","doc",query)`
      * For SSL Enable mode:  `searchWithQry("test","doc",query,true)`
 
* **searchByID**
  * *searchByID(indexName, typeofIndex, IdValue, SSL = false)*: searchByID method return single document of Index where `*_id*` is equal to *IdValue* passed. 

    **Example**
     
      Below Command will return document which has _id column value is "aRs7i20BroL8s9e_w1rA".
    
      * For SSL disable mode: `searchByID("test","doc","aRs7i20BroL8s9e_w1rA")`
      * For SSL Enable mode:  `searchByID("test","doc","aRs7i20BroL8s9e_w1rA",true)`
 
 * **indexExists**
   * *indexExists(indexName, SSL = false)*: indexExists method return true if index exist otherwise it return false. 

     **Example**
       
       Below Command will return true test index exists otherwise false.
        
       * For SSL disable mode: `indexExists("test")`
       * For SSL Enable mode:  `indexExists("test",true")`
    
 * **createIndex**
   * *createIndex(indexName, Query,SSL = false)*: createIndex method create Index. It take Query as a parameter which is uses for Index mapping durring creation. 

     **Example**
  
       Below command will create a new Index with name as "EmpIndex" using passed mapping in Query. We can pass Index Type in mapping as below
    
         val Query = 
            s"""
            |{
            | "mappings": {
            | "doc": {
            | "properties":
            | {
            | "SerialNo":{"type" : "integer"},
            | "FirstName":{"type" : "text"},
            | "LastName":{"type" : "text"},
            | "Age":{"type": "integer"}
            | }
            | }
            | }
            | }
            """.stripMargin

      * For SSL disable mode: `createIndex("EmpIndex",Query)`
      * For SSL Enable mode:  `createIndex("EmpIndex",Query,true)`
    
 * **deleteIN**
   * *deleteIN(indexName, typeofIndex, ColumnName, value, SSL = false)*: deleteIN method delete document on the basis of one columns value. It take column name and it's value as a parameter and delete all document which match condition.

     **Example**
  
       Below command will delete all documents where Status is "Y"

       * For SSL disable mode: `deleteIN("test","doc","Status","Y")`
       * For SSL Enable mode:  `deleteIN("test","doc","Status","Y",true)`
   
  * **deleteById**
    * *deleteById(indexName, typeofIndex, id, SSL = false)*: deleteById method delete document of *_id* passed  in parameter *id*.

      **Example**
        
        Below command will delete documents where *_id* is "aRs7i20BroL8s9e_w1rA"
    
       * For SSL disable mode: `deleteById("test","doc","aRs7i20BroL8s9e_w1rA")`
       * For SSL Enable mode:  `deleteById("test","doc","aRs7i20BroL8s9e_w1rA",true)`
    
  * **deleteByQry**
    * *deleteByQry(indexName, typeofIndex, Query, SSL = false)*: deleteByQry method delete all documents on the basis of Query passed.

      **Example**
 
        The below command will delete all documents where Age is 30.
   
           val Query =
          """
            |{
            |  "query": {
            |    "term": {
            |      "Age": "30"
            |    }
            |  }
            |}
          """.stripMargin

       * For SSL disable mode: `deleteByQry("test","doc",Query)`
       * For SSL Enable mode:  `deleteByQry("test","doc",Query,true)`
   
   * **updateValue**
     * *updateValue(indexName, typeofIndex, id, Query, SSL = false)*: updateValue method update columns passed in *Query* for paricular *id (_id)*.

       **Example**
  
         The below command will update column Status as "N" for documents that have *_id* value is "FupajG0BPuC_QKP9Jh5J".
   
            val Query =
                s"""
               |{
               | "doc": {
               | "Status":"N"
               | }
               | }
                """.stripMargin

        * For SSL disable mode: `updateValue("test","doc","FupajG0BPuC_QKP9Jh5J", Query)`
        * For SSL Enable mode:  `updateValue("test","doc","FupajG0BPuC_QKP9Jh5J", Query,true)`


* **dataParse** 
  * *dataParse(data,column="")*: dataParse method is one of special method which parse data that we get from all Search Method such as searchByID, getSourceData etc. It takes two parameters first is data String that we get from various Search method and another method is Column Name which is optional. If we pass Column Name then is return List of all value of particular Column otherwise it returns List of Map for all columns like below:

     **Example**
  
        val data =  getSourceData("EmpIndex","doc",true)
        println(data)

        val AllValue = dataParse(data)
        println(AllValue)

        val FnameList =dataParse(data,"FirstName")
        println(FnameList)
     
      **Output**
 
           {"took":2,"timed_out":false,"_shards":{"total":5,"successful":5,"skipped":0,"failed":0},
           "hits":{"total":2,"max_score":1.0,"hits":[{"_index":"EmpIndex","_type":"doc","_id":"FTeajG0BPuC_QKPARF34","_score":1.0,"_source":{
          "SerialNo": "1",
          "FirstName": "Nikhil"
          “LastName”: “Suthar”
          “Age” : 25

        }
        [{"_index":"EmpIndex","_type":"doc","_id":"TYjUhosdfBPuC_QKWERCF31","_score":1.0,"_source":{
          "SerialNo": "2",
          "FirstName": "Mansi"
          “LastName”: “Malvaniya”
          “Age” : 22
        }
        }]}}

        List(Map( SerialNo -> 1, FirstName -> Nikhil ,LastName -> Suthar,Age -> 25), Map( SerialNo -> 2, FirstName -> Mansi ,LastName -> Malvaniya,Age -> 22))

        List(Nikhil, Mansi)
      
* **insertIntoES**
  * *insertIntoES(indexName, typeofIndex, Query, SSL = false)*: insertIntoES method insert single documents on the basis of Query passed.

    **Example**
  
      The below command will insert the documents into the Index "test".
   
        val queryIns=s"""
                     | {
                     |  "SerialNo":"1",
                     |  "FirstName":"Nikhil",
                     |  "LastName": "Suthar",
                     |  "Age": "25"
                     | }
        """.stripMargin

      * For SSL disable mode: `insertIntoES("test","doc",queryIns)`
      * For SSL Enable mode:  `insertIntoES("test","doc",queryIns,true)`
   
   
   # How to use in Code 
   
  Here is the implementation of ScalaES utility 
  
       import com.elasticHttp.ESUtil.ESClient
      
       val client = new ESClient("MasterNode", "9200")
         /* For authantication
           val client = new ESClient("MasterNode", "9200","admin","password")
         */
       val data = ESObject.getSourceData("indexname")
         /* For SSL
           val data = ESObject.getSourceData("indexname", true)
          */
       val sourceData = client.dataParse(data)
       println(sourceData)
       
   
# WebSite

