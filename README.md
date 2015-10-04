## 2015 Canadian Election Data

### Candidates

For [a project I’m working on](http://ppgd.lucascherkewski.com), I needed a list of all federal election candidates, linked to their ridings. Elections Canada provides this, in a tab-delimited CSV file ([obtainable here](http://www.elections.ca/content2.aspx?section=can&dir=cand/lst&document=index&lang=e), and in the repo as `Candidates.txt`), but CSV is a pain to work with on the web—so I used [csvkit](https://github.com/onyxfish/csvkit) to convert the CSV file into a set of JSON files, one for each province.

This is the command used to generate the compressed JSON files:

`csvgrep -t -e utf-16-le -c 1 -r "{REGION_CODE}[0-9][0-9][0-9]" data.csv | csvjson > {REGION_ABBREVIATION}.json`

This is the command used to generate the uncompressed JSON files:

`csvgrep -t -e utf-16-le -c 1 -r "{REGION_CODE}[0-9][0-9][0-9]" data.csv | csvjson -i 4 > {REGION_ABBREVIATION}.json`

The region codes are the two number prefixes employed by Elections Canada for their Electoral District Numbers, and the region abbreviation is the standard two-letter abbreviation used by Canada Post for each region of Canada. The following table lists that information:

Proper Name | Region Code | Region Abbreviation
------------|-------------|--------------------
Newfoundland and Labrador | 10 | NL
Prince Edward Island | 11 | PE
Nova Scotia | 12 | NL
New Brunswick | 13 | NB
Quebec | 24 | QC
Ontario | 35 | ON
Manitoba | 46 | MB
Saskatchewan | 47 | SK
Alberta | 48 | AB
British Columbia | 59 | BC
Yukon | 60 | YT
Northwest Territories | 61 | NT
Nunavut | 62 | NU

### Ridings

For the same project, I need a list of all 338 ridings. Fortunately, we can process our `data.csv` file to give us that, too:

`csvcut -t -e utf-16-le -c 1,2,3 data.csv | uniq > ridings.csv`

Because there are multiple entries per riding in `data.csv`, we have to filter out duplicates with `uniq`. To make this more usable on the web, we convert it to JSON:

`csvjson ridings.csv > compressed/ridings.json` (compressed)

`csvjson -i 4 ridings.csv > uncompressed/ridings.json` (uncompressed)