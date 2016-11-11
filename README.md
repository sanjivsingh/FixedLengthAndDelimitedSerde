# FixedLengthAndDelimitedSerde
This project aims to implement fixed length and delimited SERDE(Serializer/Deserializer) for Apache HIVE. 
 - Currently most of available SerDe either support fixed length or delimited file serialization/deserialization. Even you can't have mulitple field delimiter for single record.
 - Single SerDe for all usage.

### Features
- Supported both fixed length and delimited data serilization/deserialization
- It support both case in single definition.

## SerDe Properites 
Following properties are applicable to Serde. 
- **input.format.string** - mendatory  -  defines complete record format
- **input.format.column.seperator** - (optional) default "#" -  seperator amonng column formats in 'input.format.string'

### Get Started

Clone repository and build.

     git clone https://github.com/sanjivsingh/FixedLengthAndDelimitedSerde.git  
     cd FixedLengthAndDelimitedSerde  
     mvn clean install 
 
 When build is complete, these should be **FixedLengthAndDelimitedSerde-xxxxx.jar** on target  directory. 
 
**Create file with sample data /tmp/FixedLengthAndDelimitedSerdeRegexSerDe.txt**
 
     01mycolumn122015-07-18|100.50#my column 005 values 
     02mycolumn222015-07-19|100.51#my column 015 values 

**Add JAR**

     HIVE> add jar /tmp/FixedLengthAndDelimitedSerde-0.0.1-SNAPSHOT.jar ;
     
 **Create Table**
      
      HIVE>CREATE TABLE FixedLengthAndDelimitedSerdeTest (
        column1 STRING, -- string(2)
        column2 STRING, -- string(10)
        column3 STRING, -- date in format "yyyy-MM-dd" terminated by '|'
        column4 STRING, -- decimal terminated by ','
        column4 STRING  -- string(20)
     )
     ROW FORMAT SERDE 'com.googlecode.hive.serde.FixedLengthAndDelimitedSerde'
     WITH SERDEPROPERTIES  (
     'input.format.string'='FL2#FL10#DM|#DM,#FL20'
     )STORED AS TEXTFILE;
 
 **Load Sample file into table**
 
     HIVE> load data local inpath '/tmp/FixedLengthAndDelimitedSerdeRegexSerDe.txt' overwrite into table FixedLengthAndDelimitedSerdeTest ;
 
 **Now select from Table**
 
      HIVE> select * from FixedLengthAndDelimitedSerdeTest ;
     +---------+------------+------------+---------+----------------------+--+
     | column1 | column2    | column3    | column4 | column5              | 
     +---------+------------+------------+---------+----------------------+--+
     | 01      | mycolumn12 | 2015-07-18 | 100.50  | my column 005 values |
     | 02      | mycolumn22 | 2015-07-19 | 100.51  | my column 015 values |
     +---------+------------+------------+---------+----------------------+--+
