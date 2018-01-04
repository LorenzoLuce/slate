---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

includes:
  - errors

search: true
---

# BigProfiles API

Welcome to the BigProfiles API! You can use this API to access our profiling technology.

Thanks to this API, you can profile individuals, but keep in mind that BigProfiles is thought to perform on mass profiling.

The API request takes as input information (in various formats) about name, residence address, age, gender, tax ID and email address.

The API response outputs a set of information about residence, owned companies, marital status, education level, professional status, housing, interests and income.

We have language bindings in Shell (more coming soon)! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "x-api-key: yourAPIkey"
```

> Make sure to replace `yourAPIkey` with your API key and `api_endpoint_here` with the API endpoint you want to use.

BigProfiles API uses API keys to allow access to the API. Please request your BigProfiles API key by emailing at info@bigprofil.es.

BigProfiles expects for the API key to be included in all API requests to the server in a header that looks like the following:

`x-api-key: yourAPIkey`

<aside class="success">
You must replace <code>yourAPIkey</code> with your personal API key.
</aside>

# Profiling Endpoint

## Request

> Request example:

```shell
curl "https://api.bigprofiles.it/v1/profile?"+
  "full_name=Lorenzo%20Luce&"+
  "italian_tax_id=LCULNZ92B03H501Y&"+
  "full_address=Via%20Costantino%2025,%20Roma&"+
  "email=lorenzo.luce@yahoo.it"
  -H "x-api-key: yourAPIkey"
```

> Another request example:

```shell
curl "https://api.bigprofiles.it/v1/profile?"+
  "full_name=Lorenzo%20Luce&"+
  "gender=m&"+
  "birth_date=1992-02-03&"+
  "full_address=Via%20Costantino%2025,%20Roma&"+
  "email=lorenzo.luce@yahoo.it"
  -H "x-api-key: yourAPIkey"
```

The `Profiling` endpoint profiles an individual starting from basic information about him.

### HTTP Request

`GET http://api.bigprofiles.it/v1/profile`

### Query Parameters

Accepted parameters are the following:

Parameter | Type | Example | Description
--------- | ------- | ------- | -----------
full_name | Name | Valerio De Blando | Full name (full text)
given_name | Name | Valerio | Given name (full text)
family_name | Name | De Blando | Family name (full text)
gender | Name | m | Gender (one letter, m for male, f for female)
birth_date | Birth | 1993-10-02 | Birth date (YYYY-MM-DD)
birth_year | Birth | 1993 | Birth year (YYYY)
age | Birth | 24 | Age (numbers)
italian_tax_id | Birth | LCULNZ92B03H501Y | Italian tax ID code
full_address | Location | Via Costantino 73L, Roma | Residence address (full text)
postal_code | Location | 00145 | Residence postal code (five numbers)
street | Location | Via Costantino | Residence street name (full text)
house_number | Location | 73 | Residence house number (numbers)
province | Location | RM | Residence province code (two letters)
city | Location | Roma | Residence city (full text)
latitude | Location | 41.8572957 | Residence latitude (latitude coordinates)
longitude | Location | 12.4844907 | Residence longitude (longitude coordinates)
email | Contact | valerio@deblando.it | Email address
telephone | Contact | +393385434562 | Phone number (with country code)

<aside class="success">
Remember — not all the parameters are required, and some of them are equivalent!
</aside>

### Equivalent Parameters

```shell
param1 OR param2
```
> This means that `param1` is equivalent to `param2`, and they must not be used together.

```shell
param1 AND/OR param2
```

> This means that `param1` can be used with `param2`, as they are not exactly equivalent.

```shell
param1 AND param2
```

> This means that `param1` and `param2` must be used together.

Some parameters must not be used together, because they are equivalent.
Here you have a list of equivalent parameters:

- Name Parameters: `full_name` OR (`given_name` AND `family_name`) AND/OR `gender`
- Birth Parameters: `birth_date` OR `birth_year` OR `age` OR `italian_tax_id`
- Location Parameters: `full_address` OR (`latitude` AND `longitude`) OR (`province` AND/OR `city` AND/OR `postal_code` AND/OR `street` AND/OR `house_number`)
- Contact Parameters: `email` OR `telephone`

### Required Parameters

No parameter is required, but there is a required minimum set of parameters.
Required parameters are:

- At least one Name Parameter (best: `full_name`)
- At least one Birth Parameter (best: `italian_tax_id`)
- At least one Location Parameter (best: `full_address`)

### Anonymous Profiling

> An example of an anonymous profiling request:

```shell
curl "https://api.bigprofiles.it/v1/profile?"+
  "gender=m&"+
  "birth_date=1992-02-03&"+
  "full_address=Via%20Costantino%2025,%20Roma&"+
  -H "x-api-key: yourAPIkey"
```

An anonymous profiling is a type of profiling that does not involve the use of identifying parameters in the request.
In fact, many use cases benefits of this feature, manly for two reasons:

- a complete privacy protection
- an anonymous input (i.e. anonymous visits)

This type of profiling works very well both in Analytics and Predictions; the greater is the number of anonymous profiling, the better its performance will be.

## Response

> The response of the `Profiling` endpoint is a JSON; here is an example:

```json
// response begin
{
  "statusCode":"200",
  "message": {
```

The response is organized in packages.
Main packages are: BASE, COMPANIES, MARITAL_STATUS, EDUCATION, WORK, HOUSING, INCOME, WEB_&_SOCIALS, INTERESTS.

### BASE package

```json
    "BASE": {
      "AGE_GROUP":"25-29",
      "GENDER":"Male",
      "RESIDENCE": {
        "GEO": {
          "LATITUDE":"41.90644",
          "LONGITUDE":"12.446946"
        },
        "LABEL":"ROMA",
        "POPULATION":">250000",
        "REGION":"LAZ",
        "ZONE":"Centro",
        "STATE":"Italy"
      }
    },
```

The first one is BASE. It contains all the basic information about the individual.

Parameter | Example | Description
--------- | ------- | -----------
BASE / AGE_GROUP | 18-24 | Age group (Under18, 18-24, 25-29, 30-39, 40-49, 50-59, 60-69, 70-75, Over 75)
BASE / GENDER | Male | Gender (Male, Female)
BASE / RESIDENCE / GEO / LATITUDE | 41.90644 | Residence latitude
BASE / RESIDENCE / GEO / LONGITUDE | 12.446946 | Residence longitude
BASE / RESIDENCE / LABEL | ROMA | Residence city
BASE / RESIDENCE / POPULATION | >250000 | Residence city population (<5000, 5000-25000, 25000-100000, 100000-250000, >250000)
BASE / RESIDENCE / REGION | LAZ | Residence region
BASE / RESIDENCE / ZONE | Centro | Residence country zone
BASE / RESIDENCE / STATE | Italy | Residence country

<aside class="notice">
BASE fields are pretty useful both in Analytics and Predictions!
</aside>

### COMPANIES package

```json
    "COMPANIES": {
      "SOCIETA_INDIVIDUALI": {
        "PARTITA_IVA":"0",
        "DITTA_INDIVIDUALE":"0"
      },
      "SOCIETA_DI_PERSONE": {
        "SOCIETA_SEMPLICE":"0",
        "SOCIETA_NOME_COLLETTIVO":"0",
        "SOCIETA_ACCOMANDITA_SEMPLICE":"0",
        "SOCIETA_DI_FATTO":"0"
      },
      "SOCIETA_DI_CAPITALI": {
        "SOCIETA_PER_AZIONI":"0",
        "SOCIETA_RESPONSABILITA_LIMITATA":"1"

      },
      "ALTRE_FORME": {
        "ONLUS":"0",
        "CONSORZIO":"0",
        "COOPERATIVA":"0"
      },
      "SOCIETA_IN_LIQUIDAZIONE":"0"
    },
```

The second main package is COMPANIES. It contains the number and type of companies owned by the individual.

Parameter | Example | Description
--------- | ------- | -----------
COMPANIES / SOCIETA_INDIVIDUALI / PARTITA_IVA | 1 | Number of "Partita IVA" owned
COMPANIES / SOCIETA_INDIVIDUALI / DITTA_INDIVIDUALE | 0 | Number of "Ditta Individuale" owned
COMPANIES / SOCIETA_DI_PERSONE / SOCIETA_SEMPLICE | 0 | Number of "Società Semplice" owned
COMPANIES / SOCIETA_DI_PERSONE / SOCIETA_NOME_COLLETTIVO | 2 | Number of "Società in Nome Collettivo" owned
COMPANIES / SOCIETA_DI_PERSONE / SOCIETA_ACCOMANDITA_SEMPLICE | 0 | Number of "Società in Accomandita Semplice" owned
COMPANIES / SOCIETA_DI_PERSONE / SOCIETA_DI_FATTO | 0 | Number of "Società di Fatto" owned
COMPANIES / SOCIETA_DI_CAPITALI / SOCIETA_PER_AZIONI | 3 | Number of "Società Per Azioni" owned
COMPANIES / SOCIETA_DI_CAPITALI / SOCIETA_RESPONSABILITA_LIMITATA | 1 | Number of "Società a Responsabilità Limitata" owned
COMPANIES / ALTRE_FORME / ONLUS | 0 | Number of "Onlus" owned
COMPANIES / ALTRE_FORME / CONSORZIO | 0 | Number of "Consorzio" owned
COMPANIES / ALTRE_FORME / COOPERATIVA | 0 | Number of "Cooperativa" owned
COMPANIES / SOCIETA_IN_LIQUIDAZIONE | 1 | The number of companies in liquidation

<aside class="notice">
COMPANIES fields are useful in Analytics!
</aside>

### MARITAL_STATUS package

```json
    "MARITAL_STATUS": {
      "STATUS":"SINGLE",
      "SCORE": {
        "SINGLE":"0.935844731636",
        "MARRIED":"0.049271701253",
        "SEPARATE":"0.007437498450",
        "WIDOWER":"0.007303158157",
        "DIVORCED":"0.000142910504"
      }
    },
```

The fourth main package is MARITAL_STATUS. It contains information about the marital status of the individual.

Parameter | Example | Description
--------- | ------- | -----------
MARITAL_STATUS / STATUS | SINGLE | Estimated marital status
MARITAL_STATUS / SCORE / SINGLE | 0.935844731636 | Score of the single status (0-1)
MARITAL_STATUS / SCORE / MARRIED | 0.049271701253 | Score of the married status (0-1)
MARITAL_STATUS / SCORE / SEPARATE | 0.007437498450 | Score of the separate status (0-1)
MARITAL_STATUS / SCORE / WIDOWER | 0.007303158157 | Score of the widower status (0-1)
MARITAL_STATUS / SCORE / DIVORCED | 0.000142910504 | Score of the divorced status (0-1)

<aside class="notice">
MARITAL_STATUS/STATUS is useful in Analytics, while MARITAL_STATUS/SCORE works well in Predictions!
</aside>

### EDUCATION package

```json
    "EDUCATION": {
      "STATUS":"UNIVERSITY",
      "LAST_DEGREE":"Master of Engineering (MEng) @ Università degli Studi di Roma Tre",
      "SCORE": {
        "UNIVERSITY":"0.729302930544",
        "HIGH_SCHOOL":"0.166297254561",
        "SECONDARY_SCHOOL":"0.087812357777",
        "PRIMARY_SCHOOL":"0.004662618321",
        "LITERATE":"0.005274161333",
        "ILLITERATE":"0.006650677465"
      }
    },
```

The fifth main package is EDUCATION. It contains information about the education of the individual.

Parameter | Example | Description
--------- | ------- | -----------
EDUCATION / STATUS | HIGH_SCHOOL | Estimated highest education level reached
EDUCATION / LAST_DEGREE | Master Degree @ Columbia University | Last education degree owned
EDUCATION / SCORE / UNIVERSITY | 0.087812357777 | Score of the university education level (0-1)
EDUCATION / SCORE / HIGH_SCHOOL | 0.729302930544 | Score of the high school education level (0-1)
EDUCATION / SCORE / SECONDARY_SCHOOL | 0.166297254561 | Score of the university education level (0-1)
EDUCATION / SCORE / PRIMARY_SCHOOL | 0.004662618321 | Score of the university education level (0-1)
EDUCATION / SCORE / LITERATE | 0.005274161333 | Score of the university education level (0-1)
EDUCATION / SCORE / ILLITERATE | 0.006650677465 | Score of the university education level (0-1)

<aside class="notice">
EDUCATION/STATUS and EDUCATION/LAST_DEGREE are useful in Analytics, while EDUCATION/SCORE works well in Predictions!
</aside>

### WORK package

```json
    "WORK": {
      "STATUS":"EMPLOYED",
      "LAST_JOB_TITLE":"CEO @ BigProfiles",
      "SCORE": {
        "EMPLOYED":"0.349942125092",
        "UNEMPLOYED":"0.262991244281",
        "STUDENT":"0.271371482560",
        "RETIRED":"0.005355782471",
        "OTHER":"0.110339365597"
      }
    },
```

The sixth main package is WORK. It contains information about the work of the individual.

Parameter | Example | Description
--------- | ------- | -----------
WORK / STATUS | UNEMPLOYED | Estimated work status
WORK / LAST_JOB_TITLE | CFO @ Microsoft | Last job title owned
WORK / SCORE / EMPLOYED | 0.349942125092 | Score of the employed status (0-1)
WORK / SCORE / UNEMPLOYED | 0.262991244281 | Score of the unemployed status (0-1)
WORK / SCORE / STUDENT | 0.271371482560 | Score of the student status (0-1)
WORK / SCORE / OTHER | 0.110339365597 | Score of other status (0-1)
WORK / SCORE / RETIRED | 0.005355782471 | Score of the retired status (0-1)

<aside class="notice">
WORK/STATUS and WORK/LAST_JOB_TITLE are useful in Analytics, while WORK/SCORE works well in Predictions!
</aside>

### HOUSING PACKAGE

```json
    "HOUSING": {
      "STATUS":"FAMILY_OWNED",
      "SLOT":"Quartiere Urbano",
      "SCORE": {
        "FAMILY_RENTAL":"0.177777777778",
        "FAMILY_OWNED":"0.703703703704",
        "OTHER":"0.118518518519"
      }
    },
```

The seventh main package is HOUSING. It contains information about the housing of the individual.

Parameter | Example | Description
--------- | ------- | -----------
HOUSING / STATUS | FAMILY_OWNED | Estimated housing status
HOUSING / SLOT | Quartiere Urbano | House slot type
HOUSING / SCORE / FAMILY_RENTAL | 0.177777777778 | Score of the family rental status (0-1)
HOUSING / SCORE / FAMILY_OWNED | 0.703703703704 | Score of the family owned status (0-1)
HOUSING / SCORE / FAMILY_OTHER | 0.118518518519 | Score of other status (0-1)

<aside class="notice">
HOUSING/STATUS and HOUSING/SLOT are useful in Analytics, while HOUSING/SCORE works well in Predictions!
</aside>

### INCOME package

```json
    "INCOME": {
      "SCORE":"53"
    },
```

The eighth main package is INCOME. It contains information about the income of the individual.

Parameter | Example | Description
--------- | ------- | -----------
INCOME / SCORE | 34 | Score of the income (0-100)

<aside class="notice">
INCOME fields are pretty useful both in Analytics and Predictions!
</aside>

### WEB_&_SOCIALS package

```json
    "WEB_&_SOCIALS": {
      "LINKS": "[\"https://www.linkedin.com/in/lorenzoluce/\", \"https://www.facebook.com/lorenzo.luce\"]"
    },
```

The ninth main package is WEB_&_SOCIALS. It contains information about the digital life of the individual.

Parameter | Example | Description
--------- | ------- | -----------
WEB_&_SOCIALS / LINKS | ["http://facebook.com/38596", "www.personalwebsite.com"] | Array of web and social links belonging to the individual

### INTERESTS package

```json
    "INTERESTS": {
      "ANIMALS_&_PETS":"0",
      "ART_&_CULTURE":"72",
      "BEAUTY_COSMETIC_&_PERSONAL_CARE":"0",
      "BOOKS":"23",
      "BUSINESS_&_FINANCE":"0",
      "CHARITY_&_NON_PROFIT":"0",
      "EDUCATION":"16",
      "ENTERTAINMENT_&_NIGHTLIFE":"93",
      "FOOD_&_COOKING":"79",
      "HOME_&_FAMILY":"0",
      "MEDICAL_&_HEALTH":"0",
      "MUSIC":"58",
      "NEWS":"86",
      "POLITICS":"0",
      "RELIGION":"0",
      "SCIENCE_TECHNOLOGY_&_ENGINEERING":"0",
      "SHOPPING_&_FASHION":"44",
      "SPORTS":"100",
      "TRAVEL":"0",
      "TV_&_MOVIES":"51",
      "VIP_&_GOSSIP":"30"
    }
  }
}
// response end
```

The tenth main package is INTERESTS. It contains information about interests of the individual.

Parameter | Example | Description
--------- | ------- | -----------
INTERESTS / ANIMALS_&_PETS | 0 | Score of the animals and pets interest (0-100)
INTERESTS / ART_&_CULTURE | 72 | Score of the art and culture interest (0-100)
INTERESTS / BEAUTY_COSMETIC_&_PERSONAL_CARE | 0 | Score of the beauty, cosmetic and personal care interest (0-100)
INTERESTS / BOOKS | 23 | Score of the books interest (0-100)
INTERESTS / BUSINESS_&_FINANCE | 0 | Score of the business and finance interest (0-100)
INTERESTS / CHARITY_&_NON_PROFIT | 0 | Score of the charity and non profit interest (0-100)
INTERESTS / EDUCATION | 16 | Score of the education interest (0-100)
INTERESTS / ENTERTAINMENT_&_NIGHTLIFE | 93 | Score of the animals & pets interest (0-100)
INTERESTS / FOOD_&_COOKING | 79 | Score of the food and cooking interest (0-100)
INTERESTS / HOME_&_FAMILY | 0 | Score of the home and family interest (0-100)
INTERESTS / MEDICAL_&_HEALTH | 0 | Score of the medical and health interest (0-100)
INTERESTS / MUSIC | 58 | Score of the music interest (0-100)
INTERESTS / NEWS | 86 | Score of the news interest (0-100)
INTERESTS / POLITICS | 0 | Score of the politics interest (0-100)
INTERESTS / RELIGION | 0 | Score of the religion interest (0-100)
INTERESTS / SCIENCE_TECHNOLOGY_&_ENGINEERING | 0 | Score of the science, technology and engineering interest (0-100)
INTERESTS / SHOPPING_&_FASHION | 44 | Score of the shopping and fashion interest (0-100)
INTERESTS / SPORTS | 100 | Score of the sports interest (0-100)
INTERESTS / TRAVEL | 0 | Score of the travel interest (0-100)
INTERESTS / TV_&_MOVIES | 51 | Score of the TV and movies interest (0-100)
INTERESTS / VIP_&_GOSSIP | 30 | Score of the VIP and gossip interest (0-100)

<aside class="notice">
INTERESTS fields are pretty useful both in Analytics and Predictions!
</aside>

# Request/Response Complete Example

```shell
curl "https://api.bigprofiles.it/v1/profile?"+
  "full_name=Lorenzo%20Luce&"+
  "italian_tax_id=LCULNZ92B03H501Y&"+
  "full_address=Via%20Costantino%2025,%20Roma&"+
  "email=lorenzo.luce@yahoo.it"
  -H "x-api-key: yourAPIkey"
```

```json
{
  "statusCode":"200",
  "message": {
    "BASE": {
      "AGE_GROUP":"18-24",
      "GENDER":"Male",
      "RESIDENCE": {
        "GEO": {
          "LATITUDE":"41.90644",
          "LONGITUDE":"12.446946"
        },
        "LABEL":"ROMA",
        "POPULATION":">250000",
        "REGION":"LAZ",
        "ZONE":"Centro",
        "STATE":"Italy"
      }
    },
    "COMPANIES": {
      "SOCIETA_INDIVIDUALI": {
        "PARTITA_IVA":"0",
        "DITTA_INDIVIDUALE":"0"
      },
      "SOCIETA_DI_PERSONE": {
        "SOCIETA_SEMPLICE":"0",
        "SOCIETA_NOME_COLLETTIVO":"0",
        "SOCIETA_ACCOMANDITA_SEMPLICE":"0",
        "SOCIETA_DI_FATTO":"0"
      },
      "SOCIETA_DI_CAPITALI": {
        "SOCIETA_PER_AZIONI":"0",
        "SOCIETA_RESPONSABILITA_LIMITATA":"0"

      },
      "ALTRE_FORME": {
        "ONLUS":"0",
        "CONSORZIO":"0",
        "COOPERATIVA":"0"
      },
      "SOCIETA_IN_LIQUIDAZIONE":"0"
    },
    "MARITAL_STATUS": {
      "STATUS":"SINGLE",
      "SCORE": {
        "SINGLE":"0.935844731636",
        "MARRIED":"0.049271701253",
        "SEPARATE":"0.007437498450",
        "WIDOWER":"0.007303158157",
        "DIVORCED":"0.000142910504"
      }
    },
    "EDUCATION": {
      "STATUS":"HIGH_SCHOOL",
      "LAST_DEGREE":"?",
      "SCORE": {
        "UNIVERSITY":"0.087812357777",
        "HIGH_SCHOOL":"0.729302930544",
        "SECONDARY_SCHOOL":"0.166297254561",
        "PRIMARY_SCHOOL":"0.004662618321",
        "LITERATE":"0.005274161333",
        "ILLITERATE":"0.006650677465"
      }
    },
    "WORK": {
      "STATUS":"UNEMPLOYED",
      "LAST_JOB_TITLE":"?",
      "SCORE": {
        "EMPLOYED":"0.349942125092",
        "UNEMPLOYED":"0.262991244281",
        "STUDENT":"0.271371482560",
        "RETIRED":"0.005355782471",
        "OTHER":"0.110339365597"
      }
    },
    "HOUSING": {
      "STATUS":"FAMILY_OWNED",
      "SLOT":"Quartiere Urbano",
      "SCORE": {
        "FAMILY_RENTAL":"0.177777777778",
        "FAMILY_OWNED":"0.703703703704",
        "OTHER":"0.118518518519"
      }
    },
    "INCOME": {
      "SCORE":"100"
    },
    "WEB_&_SOCIALS": {
      "LINKS": "?"
    },
    "INTERESTS": {
      "ANIMALS_&_PETS":"0",
      "ART_&_CULTURE":"72",
      "BEAUTY_COSMETIC_&_PERSONAL_CARE":"0",
      "BOOKS":"23",
      "BUSINESS_&_FINANCE":"0",
      "CHARITY_&_NON_PROFIT":"0",
      "EDUCATION":"16",
      "ENTERTAINMENT_&_NIGHTLIFE":"93",
      "FOOD_&_COOKING":"79",
      "HOME_&_FAMILY":"0",
      "MEDICAL_&_HEALTH":"0",
      "MUSIC":"58",
      "NEWS":"86",
      "POLITICS":"0",
      "RELIGION":"0",
      "SCIENCE_TECHNOLOGY_&_ENGINEERING":"0",
      "SHOPPING_&_FASHION":"44",
      "SPORTS":"100",
      "TRAVEL":"0",
      "TV_&_MOVIES":"51",
      "VIP_&_GOSSIP":"30"
    }
  }
}
```

Here on the right you have a complete example of a request and a response for the Profiling Endpoint

# Bulk requests

For bulk requests (e.g. 10'000 profiling) please contact our support to access our SFTP server (sftp.bigprofiles.it) , where you can load a CSV file containing all the information, one per line.