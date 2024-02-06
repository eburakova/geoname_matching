# Geoname search

[<img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>](https://colab.research.google.com/github/eburakova/geoname_matching/blob/main/main.ipynb)


*For Yandex Practicum Career Center*

A hackaton project with the aim to create a tool that would suggest the cities whose names match the query string. 
Developed and tested on the cities with population > 15000 people in selected contries of Eastern Europe and Northen Asia.

➡️ Input: query string (potentially containing misspellings)

⏬ Output: a dictionary of `k` best matching entries with the info on country, population as well as geonameID


See [the main notebook](https://github.com/eburakova/geoname_matching/blob/main/main.ipynb) for the detailed description and research.

[Open in CoLab](https://colab.research.google.com/github/eburakova/geoname_matching/blob/main/main.ipynb)

Data sources: http://download.geonames.org/export/dump/

# `geonamesearch.py` demo

```
 from geonamesearch import search 
 > Connection established: geo_v2 at 77.222.36.33
```

```
# Misspelled name

q = 'Ржевск'
search(q, k=3, weight_mode='exp', asdict=False)
```
|   geonameid | name    | region             | country    |   score |
|------------:|:--------|:-------------------|:-----------|--------:|
|      554840 | Izhevsk | Udmurtiya Republic | Russia     |  85.085 |
|      499717 | Rzhev   | Tver Oblast        | Russia     |  82.085 |
|     1528121 | Karakol | Issyk-Kul          | Kyrgyzstan |  77.084 |

```
# Valid historical name

q = 'Сталинград'
search(q, k=3, weight_mode='exp', asdict=False)
```

|   geonameid | name             | region           | country    |   score |
|------------:|:-----------------|:-----------------|:-----------|--------:|
|      472757 | Volgograd        | Volgograd Oblast | Russia     |  88.427 |
|     1526273 | Astana           | Astana           | Kazakhstan |  74.393 |
|      498817 | Saint Petersburg | St.-Petersburg   | Russia     |  72.409 |


```
# Non-existing city

q = 'Milkyway'
search(q, k=3, weight_mode='exp', asdict=False)
```

|   geonameid | name    | region          | country    |   score |
|------------:|:--------|:----------------|:-----------|--------:|
|      561731 | Gay     | Orenburg Oblast | Russia     |  71.084 |
|      527740 | Melenki | Vladimir Oblast | Russia     |  70.391 |
|     1526193 | Arkalyk | Qostanay        | Kazakhstan |  70.391 |

```
# Country out of the scope
# Unlimited area of search

q = 'Berlin'
search(q, k=3, weight_mode='exp', asdict=False, country_selection=None) # runs 10 seconds
```

|   geonameid | name         | region            | country       |   score |
|------------:|:-------------|:------------------|:--------------|--------:|
|     2950159 | Berlin       | Berlin            | Germany       |  99.084 |
|     5164706 | North Canton | Ohio              | United States |  98.391 |
|     2820577 | Überlingen   | Baden-Wurttemberg | Germany       |  90     |

```
# Country out of the scope
# Known area of search

q = 'Berlin'
search(q, k=3, weight_mode='exp', asdict=False, country_selection=['DE'])  # runs 0.7s!
```

|   geonameid | name              | region            | country   |   score |
|------------:|:------------------|:------------------|:----------|--------:|
|     2950159 | Berlin            | Berlin            | Germany   |  99.084 |
|     2820577 | Überlingen        | Baden-Wurttemberg | Germany   |  90     |
|     2950096 | Bernau bei Berlin | Brandenburg       | Germany   |  89.777 |
