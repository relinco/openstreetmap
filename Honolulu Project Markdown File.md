---
output:
  pdf_document: default
  html_document: default
---
# OpenStreetMap Project - Honolulu

### Map Area

Honolulu, Hawaii, United States


I have selected Honolulu as the city for the OpenStreetMaps project as I’ve visited the city many years ago and plan on going again in the near future. It is a beautiful city with a deep history and culture and a city I have enjoyed visiting. Below is a quick description of Honolulu from Wikipedia.

“Honolulu is the capital and largest city of the U.S. state of Hawaii. It is an unincorporated part of and the county seat of the City and County of Honolulu on the island of Oahu.The city is the main gateway to Hawaii and a major portal into the United States. The city is also a major hub for international business, military defense, as well as famously being host to a diverse variety of east-west and Pacific culture, cuisine, and traditions.” <sup>[0]
</sup>  

The links below is the source of my dataset and a Google Maps link for the reader to see exactly where in the world my city is:
- [OpenStreetMaps](https://www.openstreetmap.org/relation/119231) <br/>
- [Google Maps - Honolulu, Hawaii](https://www.google.com/maps/place/Honolulu,+HI/)

### Background and Quick Exploration of Data

The dataset was taken from an OSM download from OpenStreetMaps, which had the dataset in a XML format. An example of the format can be found here:

- [OpenStreetMaps - OSM XML](http://wiki.openstreetmap.org/wiki/OSM_XML)

To explore what is exactly in this map file, I used mapparser_f.py to use iterative parsing to process the map file and to get a sense of the tags and number of tags that appear in the initial OSM file from OpenStreetMaps.

Running this file for my Honolulu OSM file gives me the following result:

The basic structure of the XML dataset is that it has an element, which contains the following:
- a block of nodes, which contains the tags of each node
- a block of ways, which contains the references to its nodes for each way, and the tags of each way
- a block of relations, which contains the references to its members for each relation, and the tags of each relation

### Summary of Python Project Files

I will briefly write a description of what the purpose and use of the included .py files included does in order of use in this project:

```
{'bounds': 1,
 'member': 2015,
 'nd': 332307,
 'node': 278529,
 'osm': 1,
 'relation': 494,
 'tag': 124380,
 'way': 31017}
```



### Auditing OSM File

The ultimate goal is to query the data from the OpenStreetsMaps OSM dataset. However, the file is in an XML format and before I enter the data into a SQL dataset, I would like to audit the data to fix any problems or to standardize the data so it will show up properly in a query. I would like to do the following two simple audits and fixes for the data:
1. Over­abbreviated street names (“Blvd --> Boulevard”)
2. Inconsistent postal codes -  (“NC28226”, “28226­0783”, “28226”)

### Auditing for Overabbreviated Street Names

I ran the audit.py file for a sample subset of the honolulu OSM file, which checks the street names for overabbreviated files. The only names which were unexpected were the following:
- Blvd
- Highway
- Circle
- Loop

Everything but Blvd are fine, so I added the Blvd to the update dictionary mapping. From the sample, it looks as if there aren't many overabbreviated files. I threw the unaudited data into a dataset to just make a quick check of other potential overabbreviated street names so I could just update this now. I queried the street names with the following query and obtained the result:

```SQL
SELECT value, COUNT(*) as count
from nodes_tags where type = "addr" and key = "street"
GROUP BY value
ORDER by value asc
```

```
"2nd Street",1
"3375 Koapaka Street",2
"Ahui Street",1
"Ainamakua Drive",1
"Ala Ike Street",1
"Ala Lilikoi Street",5
"Ala Moana Blvd",1
"Ala Moana Boulevard",20
"Alakea Street",1
"Arizona Memorial Drive",1
"Atkinson Drive",1
"Auahi Street",4
"Beach Walk",2
"Bethel Street",5
"Bishop Street",6
"California Avenue",3
"Cartwright Road",1
"Coelho Way",1
"Date Street",1
"Dillingham Boulevard",2
"Dudoit Lane",1
"Dukes Lane",1
"East Manoa Road",2
"Ena Road",1
"Farrington Highway",7
"Fort Street Mall",2
"Hamakua Drive",1
"Hekili Street",1
"Hikimoe Street",1
"Hobron Lane",10
"Ilihau Street",1
"Iliili Road",1
"Isenberg Street",1
"Ka Makana Alii Mall",1
"Ka Uka Boulevard",2
"Kaahumanu Street",3
"Kaelepulu Dr, Kailua,",1
"Kailua Road",2
"Kaimuki Avenue",1
"Kalaimoku Street",1
"Kalakaua Avenue",6
"Kalani Street",1
"Kalaniana’ole Highway",1
"Kalia Road",3
"Kaloapau Street",1
"Kalākaua Avenue",14
"Kamani Street",3
"Kamehameha Highway",31
"Kamehameha Hwy",3
"Kanaina Avenue",1
"Kaneohe Bay Drive",2
"Kaonohi Street",2
"Kapahulu Avenue",1
"Kapiolani Boulevard",2
"Kapolei Parkway",2
"Keahole Street",1
"Keaunui Drive",4
"Keeaumoku Street",5
"Keehi Place",1
"Keolu Drive",2
"Kilani Avenue",2
"Kipapa Dr",1
"Kipapa Drive",5
"Koa Avenue",3
"Koapaka Street",1
"Kuaaina Way",1
"Kuahelani Avenue",1
"Kuhaulua Street",1
"Kuhio Avenue",7
"Kunia Road",1
"Kupuohi Street",2
"Lanikuhana Avenue",1
"Lemon Road",1
"Leoku Street",1
"Lewers Street",4
"Liliha Street",1
"Lumiana Street",1
"Makiki Heights Drive",1
"Manawai Street",2
"Maunakea Street",1
"Meheula Parkway",13
"Meheula Pkwy",1
"Merchant Street",1
"Moanalua Road",3
"Moanalua, Honolulu",1
"Monsarrat Avenue",2
"Nimitz Highway",1
"North Beretania Street",1
"North King Street",3
"North Kuakini Street",1
"North Nimitz Highway",1
"North School Street",1
"Paiea Street",4
"Pali Momi",1
"Peke Lane",1
"Pensacola Street",6
"Piikoi Street",3
"Pohakulana Place",2
"Prince Edward Street",1
"Pualei Circle",1
"Republican Street",1
"Royal Hawaiian Avenue",3
"Seaside Avenue",2
"Smith Street",2
"South Beretania Street",8
"South Hotel Street",2
"South King",1
"South King Street",7
"State Highway 83",1
"Ukee Street",2
"Uluniu Avenue",2
"Uluniu Street",1
"University Avenue",1
"Waiakamilo Road",1
"Waialae Avenue",2
"Waikele Road",1
"Waimea Valley Road",1
"Waipahu Street",1
"Ward Avenue",1
"Wilder Avenue",8
"Young Street",1
"kanehameha highway",1
"keeaumoku Street",1
king,1
```

Running the entire honolulu OSM through audit.py, I decided to add "Highway", "Mall",  "Circle", "Walk", "Way", "Loop", "Terrace", and "King" to the list of OK names as these aren't abbreviated streets and are just uncommon. "Kailua" is also uncommon and I did some research online to see the following definition of Kailua:

"In the Hawaiian language Kailua means "two seas," or "two currents," a contraction of the words kai (meaning sea or sea water) and ʻelua (meaning two); it is so named because of the two lagoons in the district or the two currents which run through Kailua Bay." <sup>[1]</sup>

My first impression of the results of the query is that the data is very clean from what I initially expected. I have no idea what the last value of 'king' is in terms of street. Some updates I will make to the audit will be to update Dr to Drive (Kipapa Dr) and Hwy to Highway

### Auditing Postcodes to a Consistent Format

I ran a SQL query to see what postcodes were in the Honolulu map data to see if there was any auditing I could do in this area. The query is below:

```SQL
SELECT tags.value, COUNT(*) as count

FROM (SELECT * FROM nodes_tags
	  UNION ALL
      SELECT * FROM ways_tags) tags
	  WHERE tags.key='postcode'

GROUP BY tags.value
ORDER BY count DESC;
```

```
96822,395
96815,277
96814,161
96826,102
96813,72
96819,47
96817,35
96712,33
96734,18
96821,16
96706,13
96818,12
96816,11
96707,10
96786,10
96782,9
96789,9
96797,9
96732,6
96701,5
96744,4
96825,3
96717,2
96762,2
96791,2
96792,2
96815-2834,2
96712-9998,1
96734-9998,1
96753,1
96783,1
96795,1
96815-2518,1
96815-2830,1
96817-1713,1
96825-9998,1
96826-4427,1
96841,1
98622,1
"HI 96819",1
```
The first thing I noticed is that overall the data is pretty clean. There are no wildly unexpected data entries that deviate from the standard US 5-digit postcode format of xxxxx or sometimes a 9-digit postcode of xxxxx-xxxx, where x denotes a number 0-9. The difference in the 5-digit and 9-digit format in the US zip code convention is that the latter is known to be a ZIP+4 format with the last 4 digits to be an additional identification code to aid in sorting and delivery efficiency with the preceding 5 digits to be the same as the 5-digit format <sup>[2]</sup>. The only entry that was kind of different was the last value of "HI 96819" but this only came up once as shown below to make sure:

```SQL
SELECT tags.value, COUNT(*) as count

FROM (SELECT * FROM nodes_tags
	  UNION ALL
      SELECT * FROM ways_tags) tags
	  WHERE tags.key='postcode' and tags.value = 'HI 96819'

GROUP BY tags.value
ORDER BY count DESC;
```

```
"HI 96819",1
```

Similar to the audit.py file as above when auditing and updating abbreviated street names, I used a similar function to go through all the postcodes


## Post Auditing files

### Clean Street Names

I ran the query above for street names and zip code as a check that my python script actually did go through and updated the abbreviated street names and zip codes as I wanted:

```
"2nd Street",1
"3375 Koapaka Street",2
"Ahui Street",1
"Ainamakua Drive",1
"Ala Ike Street",1
"Ala Lilikoi Street",5
"Ala Moana Boulevard",21
"Alakea Street",1
"Arizona Memorial Drive",1
"Atkinson Drive",1
"Auahi Street",4
"Beach Walk",2
"Bethel Street",5
"Bishop Street",6
"California Avenue",3
"Cartwright Road",1
"Coelho Way",1
"Date Street",1
"Dillingham Boulevard",2
"Dudoit Lane",1
"Dukes Lane",1
"East Manoa Road",2
"Ena Road",1
"Farrington Highway",7
"Fort Street Mall",2
"Hamakua Drive",1
"Hekili Street",1
"Hikimoe Street",1
"Hobron Lane",10
"Ilihau Street",1
"Iliili Road",1
"Isenberg Street",1
"Ka Makana Alii Mall",1
"Ka Uka Boulevard",2
"Kaahumanu Street",3
"Kaelepulu Dr, Kailua,",1
"Kailua Road",2
"Kaimuki Avenue",1
"Kalaimoku Street",1
"Kalakaua Avenue",6
"Kalani Street",1
"Kalaniana’ole Highway",1
"Kalia Road",3
"Kaloapau Street",1
"Kalākaua Avenue",14
"Kamani Street",3
"Kamehameha Highway",34
"Kanaina Avenue",1
"Kaneohe Bay Drive",2
"Kaonohi Street",2
"Kapahulu Avenue",1
"Kapiolani Boulevard",2
"Kapolei Parkway",2
"Keahole Street",1
"Keaunui Drive",4
"Keeaumoku Street",5
"Keehi Place",1
"Keolu Drive",2
"Kilani Avenue",2
King,1
"Kipapa Drive",6
"Koa Avenue",3
"Koapaka Street",1
"Kuaaina Way",1
"Kuahelani Avenue",1
"Kuhaulua Street",1
"Kuhio Avenue",7
"Kunia Road",1
"Kupuohi Street",2
"Lanikuhana Avenue",1
"Lemon Road",1
"Leoku Street",1
"Lewers Street",4
"Liliha Street",1
"Lumiana Street",1
"Makiki Heights Drive",1
"Manawai Street",2
"Maunakea Street",1
"Meheula Parkway",14
"Merchant Street",1
"Moanalua Road",3
"Moanalua, Honolulu",1
"Monsarrat Avenue",2
"Nimitz Highway",1
"North Beretania Street",1
"North King Street",3
"North Kuakini Street",1
"North Nimitz Highway",1
"North School Street",1
"Paiea Street",4
"Pali Momi",1
"Peke Lane",1
"Pensacola Street",6
"Piikoi Street",3
"Pohakulana Place",2
"Prince Edward Street",1
"Pualei Circle",1
"Republican Street",1
"Royal Hawaiian Avenue",3
"Seaside Avenue",2
"Smith Street",2
"South Beretania Street",8
"South Hotel Street",2
"South King",1
"South King Street",7
"State Highway 83",1
"Ukee Street",2
"Uluniu Avenue",2
"Uluniu Street",1
"University Avenue",1
"Waiakamilo Road",1
"Waialae Avenue",2
"Waikele Road",1
"Waimea Valley Road",1
"Waipahu Street",1
"Ward Avenue",1
"Wilder Avenue",8
"Young Street",1
"kanehameha highway",1
"keeaumoku Street",1
```
This looks good. Also, I wanted to make sure the zip codes were all 5 digits (along with their counts) as well as having no other random unexpected characters.

### Clean Zip Codes

```
96822,395
96815,281
96814,161
96826,103
96813,72
96819,48
96817,36
96712,34
96734,19
96821,16
96706,13
96818,12
96816,11
96707,10
96786,10
96782,9
96789,9
96797,9
96732,6
96701,5
96744,4
96825,4
96717,2
96762,2
96791,2
96792,2
96753,1
96783,1
96795,1
96841,1
98622,1
```

This looks good as well. I will now move on to actually exploring the data set with SQL queries.

### City Names

I ran a quick audit check for the city names (which is strange because this dataset is from the Honolulu city) using the following query:


```SQL
SELECT tags.value, COUNT(*) as count
FROM (SELECT * FROM nodes_tags UNION ALL
      SELECT * FROM ways_tags) tags
WHERE tags.key LIKE 'city'
GROUP BY tags.value
ORDER BY tags.value ASC;
```

```
Aiea,14
"Ewa Beach",13
"Hale'iwa",19
Haleiwa,8
"Hale‘iwa",3
Hauula,2
Honlulu,6
Honolulu,1057
"Honolulu, HI",4
Kailua,14
Kaneohe,2
Kapolei,11
Laie,1
Mililani,6
"Pearl City",6
Wahiawa,4
Waialua,1
Waimanalo,2
Waipahu,1
honolulu,5
kailua,1
```

Although the city for the project is Honolulu, querying all the values for city with tags.key = 'city' brings up values other than Honolulu. Doing a quick check of the first result 'Aiea' in Wikipedia explains what this region is:


>Aiea (Hawaiian: ʻAiea) is a census-designated place (CDP) located in the City and County of Honolulu, Hawaii, United States. As of the 2010 Census, the CDP had a total population of 9,338.

I will discuss this city issue in the end of the project when I discuss suggestions on improvement. For now we will move on to other data information.

## Data Information
I ran a few more queries for some basic dataset information as well to get a big picture of the dataset I am working with:

#### Number of nodes
```SQL
sqlite> SELECT COUNT(*) FROM nodes;
```
```
278530
```

#### Number of ways

```SQL
sqlite> SELECT COUNT(*) FROM ways;
```
```
31018
```

#### Number of unique users

```SQL
sqlite> SELECT COUNT(DISTINCT(temptable.uid))          
FROM (SELECT uid FROM nodes UNION ALL SELECT uid FROM ways) temptable;
```
```
558
```

#### Top 10 contributing users
```SQL
sqlite> SELECT temptable.user, COUNT(*) as num
FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) temptable
GROUP BY temptable.user
ORDER BY num DESC
LIMIT 10;
```
```
Tom_Holland,99908
cbbaze,33031
ikiya,12715
julesreid,12014
kr4z33,10243
"Chris Lawrence",9205
pdunn,8945
woodpeck_fixbot,8217
aaront,7880
abishek_magna,6726
```

#### Number of users appearing only once (having 1 post)

```SQL
SELECT COUNT(*)
FROM
    (SELECT temptable.user, COUNT(*) as num
     FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) temptable
     GROUP BY temptable.user
     HAVING num=1);
```
```
122
```


### Additional Data Exploration

Using the sample project provided as a guide by the Udacity course<sup>[3]</sup>, I used similar queries to my own dataset to see what were some popular things in Honolulu:

#### Top 10 appearing amenities

```SQL
SELECT value, COUNT(*) as num
FROM nodes_tags
WHERE key='amenity'
GROUP BY value
ORDER BY num DESC
LIMIT 10
```
```
restaurant,272
fast_food,117
parking,75
cafe,74
toilets,72
bench,38
waste_basket,38
parking_entrance,31
fire_station,30
bank,29
```

#### Biggest religion
```SQL
SELECT nodes_tags.value, COUNT(*) as num
FROM nodes_tags
    JOIN (SELECT DISTINCT(id) FROM nodes_tags WHERE value='place_of_worship') i
    ON nodes_tags.id=i.id
WHERE nodes_tags.key='religion'
GROUP BY nodes_tags.value
ORDER BY num DESC
LIMIT 1;
```
```
christian,8
```

#### Most popular cuisines

```SQL
SELECT nodes_tags.value, COUNT(*) as num
FROM nodes_tags
    JOIN (SELECT DISTINCT(id) FROM nodes_tags WHERE value='restaurant') i
    ON nodes_tags.id=i.id
WHERE nodes_tags.key='cuisine'
GROUP BY nodes_tags.value
ORDER BY num DESC
LIMIT 15;
```

```
japanese,14
pizza,10
american,9
chinese,8
regional,8
indian,6
sushi,6
asian,5
italian,5
international,4
korean,4
thai,4
vietnamese,3
burger,2
mexican,2
```
### Exploration of Other 'Keys'

Now that I had a better sense of what I could find in my dataset, I used the following is a query to explore the dataset at a high level, specifically the 'key' dataset as I saw above that 'city' is a key within the dataset. I basically wanted to see what other items in the dataset are avaiable for me to explore.

```SQL
SELECT table.key, COUNT(*) as count
FROM (SELECT * FROM nodes_tags UNION ALL
      SELECT * FROM ways_tags) table
GROUP BY table.key
ORDER BY count desc
LIMIT 40
```
```
highway,20512
name,15212
building,7810
county,6379
cfcc,6182
name_base,5747
name_type,5447
reviewed,4539
service,3694
source,3375
oneway,2587
height,1973
amenity,1905
natural,1743
lanes,1602
street,1457
levels,1446
housenumber,1365
postcode,1280
leisure,1264
city,1180
state,1041
ref,940
material,837
landuse,772
surface,759
colour,648
layer,633
access,602
bridge,600
power,566
part,515
golf,494
man_made,473
name_1,462
en,453
sport,451
aeroway,444
shape,426
tlid,414
```

The first thing that stood out was the key 'sport' near the bottom of the list as I enjoy playing sports. I wanted to explore this a little bit so I ran the following query:

```SQL
SELECT table.value , count(*) as count
FROM (SELECT * FROM nodes_tags UNION ALL
      SELECT * FROM ways_tags) table
WHERE table.key = 'sport'
GROUP BY table.value
ORDER BY count desc
LIMIT 10
```

```
tennis,202
basketball,100
baseball,58
golf,15
swimming,13
volleyball,12
multi,10
american_football,8
athletics,5
running,5
```

And since basketball is my favorite sport:

```SQL
SELECT *
FROM (SELECT * FROM nodes_tags UNION ALL
      SELECT * FROM ways_tags) table
WHERE table.value = 'basketball'
LIMIT 10
```

```
4664197957,sport,basketball,regular
157188622,sport,basketball,regular
158436264,sport,basketball,regular
218127602,sport,basketball,regular
230065126,sport,basketball,regular
230066074,sport,basketball,regular
244434735,sport,basketball,regular
256483402,sport,basketball,regular
256483403,sport,basketball,regular
267608840,sport,basketball,regular
```

I noted that the numbers we are selecting all columns of the nodes_tags table and the ways_tags table that is being joined on table.value = 'basketball' . The numbers appear to be related to id numbers for nodes. Knowing that I had a ways_nodes table as well, I checked to see if I could pull up any other information for the second result, 157188622. The ways_nodes table has columnes for id, node_id, and position. I'm not sure what position is but I ran the id to see if I could retreive the node_id to use in the nodes table. I obtained the following result:

```SQL
SELECT *
FROM ways_nodes
WHERE id = '157188622'
```
```
157188622,1694001340,0
157188622,1694001375,1
157188622,1694001353,2
157188622,1694001318,3
157188622,1694001340,4
```

Great. Since I now had a node_id, I wanted to see if I could use this ID to see if I can find any other information in the nodes table. I ran the following query initially with all columns, which had id, lat, lon, user, uid, version, changeset, and timestamp as possible columns to select from. I wanted to see if lat and lon would give me latitude and longitude for this location and ran the following query with just those two columns and the id from above:

```SQL
SELECT lat, lon
FROM nodes
WHERE id = '1694001340'
```

And obtained this result:
```
21.2733237,-157.7047885
```

Interesting. Let's try to see what this is.

### Google Maps Investigation

I entered the coordinates indicated above to see what would show up in Google Maps. This is what came up: <br/><br/>
![image](https://raw.githubusercontent.com/rh201/openstreetmap/master/googlemap1.png) <br/><br/>

It seems to be a school named Koko Elementary School. I looked around to see something that seems to resemble a basketball court which is what we were initially querying for: <br/><br/>
![image](https://raw.githubusercontent.com/rh201/openstreetmap/master/googlemap2.png) <br/><br/>

Zooming in it does indeed seem to be a basketball court on the school grounds: <br/><br/>
![image](https://raw.githubusercontent.com/rh201/openstreetmap/master/googlemap3.png) <br/><br/>

This gives me a little bit of more confidence in the OpenStreetMaps dataset as I can actually see a real basketball court from what was a drill down of queries from a sport tag to basketball tag to eventual latitude and longitude that I could look up on Google Maps.

## Additional Ideas for improvement

As noted above, running a sql query for tags.keys LIKE 'city' brings up a lot of tags that are anything but Honolulu, the city we are investigating. From a quick search for 'Aiea' in Wikipedia as well as personal knowledge from having visited Hawaii, these tags are not city indicators but are regions or districts within Honolulu and the surrounding areas. It is good to ahve this information as it is meaningful and certainly not wrong, but having it forced within the 'city' tag is misleading.

A suggestion would be adding this information in another tag, maybe as a 'district' tag within the OpenStreetMaps OSM file. This would provide a space for these district data to be included as well as maintaining the original city of Honolulu without misleading any user.

Some issues with this is that a lot of areas in the world might not necessarily have any districts at all within the city. Smaller cities with lower population and land areas do not need to be distinguished by areas of the city and forcing the creator to provide this information might make it confusing. Another issue is that a lot of data would need to be inconsistent if this was implemented as a lot of cities are already set up this way. Any future changes might be detrimental instead of adding any value.


## References
###### Resources Used: <br/>
- [Data Wrangling with MongoDB Course by Udacity](https://www.udacity.com/course/data-wrangling-with-mongodb--ud032)

###### Links
- [0] https://en.wikipedia.org/wiki/Honolulu <br/>
- [1] http://www.kailuachamber.com/aboutkailua <br/>
- [2] https://en.wikipedia.org/wiki/ZIP_Code#ZIP.2B4 <br/>
- [3] https://gist.github.com/carlward/54ec1c91b62a5f911c42#file-sample_project-md <br/>
