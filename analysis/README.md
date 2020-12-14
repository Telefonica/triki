Triki analysis
==============

Auxiliary scripts to facilitate `Triki` results analysis.

## Table of contents

* [Triki SQLite database](#triki-sqllite-database)
    * [Database structure](#Database-structure)
* [Triki click analysis](#triki-click-analysis)

## Triki SQLite database

We have created an script that walks through the results generated by `Triki` usually stored in the `data` folder and creates an SQLite database to facilitate the analysis of the results.

### Creation of DB and initial data load
To run the script you need to execute:

```bash
./triki_database.py -i <DATA_PATH>
```

#### Output
This will create the database in `db/site_cookies.db` and load the cookies and stats obtained by `Triki` in two separate tables. see [Database structure section](#database-structure)

### Add more results to DB
If you want to add more results to an existing SQLite database you can do so by running:

```bash
./triki_database.py -k -i <OTHER_DATA_PATH>
```

#### Output
It will load the new results in `OTHER_DATA_PATH` to the existing DB in `db\site_cookies.db`


### Database structure
The database is made up of two tables. **Cookies** and **Stats**.

The `cookies` table contains detailed information for each cookie. Each attribute is detailed below:

* **id** autoincremented field used as a primary key
* **url:** It is the url where the cookie is being used.
* **date:** Date on which the data was recorded.
* **flow:** Navigation flow type in which the cookie is used (reject, accept, browse).
* **block_third_party:** Binary attribute that indicates if third-party cookie blocking has been used.
    * **0 / false:** Third-party cookie blocking has not been used.
    * **1 / true:** Third-party cookie blocking has been used.
* **host:** domain where the cookie comes from.
* **name:** Name of the cookie.
* **value:** Cookie value, currently blank for security concerns.
* **path:**  Path where the cookie is applied.
* **expires_utc:** cookie expiration date in utc
* **is_secure:** Binary attribute that indicates if the cookie has the secure flag activated.
* **is_httponly:** Binary attribute that indicates if the cookie has the httpOnly flag activated.
* **has_expires:** Binary attribute that indicates if the cookie expires, or not.
* **is_persistent:** Binary attribute that indicates if the cookie is persistent, or not.
* **priority:** Attribute that indicates the priority of the cookie.
*  **samesite:** Flag indicating the type of samesite:
   *  **-1:** valueless.
   *  **0:** Lax value.
   *  **1:** Strict value.
*  **source_scheme:**  source scheme.

The `cookies` table has a composite unique key made of the following fields:
* **url:**
* **date:**
* **flow:**
* **block_third_party:**
* **host:**
* **name:**
* **path:**

The `stats` table contains information about statistics by domain. It contains the following attributes:

* **url:** It is the url where the cookie is being used.
* **date:** Date on which the data was recorded.
* **flow:** Navigation flow type in which the cookie is used (reject, accept, browse).
* **block_third_party:** Binary attribute that indicates if third-party cookie blocking has been used.
    * **0 / false:** Third-party cookie blocking has not been used.
    * **1 / true:** Third-party cookie blocking has been used.
* **total:** Total number of cookies.
* **session:** Total number of session cookies.
* **max_exp_days:** Indicates the maximum number of days a cookie expires.
* **avg_exp_days:** Shows the average value of days a cookie expires.
* **secure_flag:** Total number of cookies that have the secure flag enabled.
* **httpOnly_flag:** Total number of cookies that have the httpOnly flag enabled.
* **samesite_none_flag:** Total number of cookies with the samesite flag with the value none.
* **samesite_lax_flag:** Total number of cookies with the flag samesite with the value lax.
* **samesite_stric_flag:** Total number of cookies with the samesite flag with the value strict.

The `stats` table has a composite unique key made of the following fields:
* **url:**
* **date:**
* **flow:**
* **block_third_party:**

## Triki click analysis
Auxiliary module to calculate differences between sites when accepting or rejecting cookies, relies on a yaml configuration file created for browser cookie analysis automation

To run the script you need to execute:

```bash
./triki_click_analysis.py
```

### Output

The script calculates the differences and dumps the results to standard output it also stores the results in a json file `click_stats.json` for further scrutiny.