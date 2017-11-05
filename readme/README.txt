Summary of Python Project Files
I will briefly write a description of what the purpose and use of the included .py files included does in order of use in this project:

honolulu_hawaii2.osm: original full OSM dataset taken from OpenStreetMaps for Honolulu, HI

mapparser_f.py: uses iterative parsing of the OSM file to see what tags are available in the dataset and the number of tags

sampling_osm_f.py: used to take a subset of the original OSM file above as the file is large for auditing and testing of data

tags_f: explores the OSM for the "k" values for each <tag> to see if there's any potential problems to use for audting

audit_f.py: used to audit and return any abbreviated street name values using mapping to update the result as necessary

audit_zip.py: used to audit and return any postcodes/zip codes using mapping to update the result as necessary

data_f.py: main file used create a csv file by parsing the elements in the OSM XML file from document format to tabular format, auditing and cleaning the data as it called audit_f.py and audit_zip.py as it parses through

schema: the scheme used for the CSV file for entering into a SQL database