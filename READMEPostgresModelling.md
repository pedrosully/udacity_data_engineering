# Data Modelling with Postgres

## Context

A startup called Sparkify wants to analyze their overall user activity on their new music streaming app specifically their song data. Currently, they don't have an easy way to query their data. The data currently lives in a  directory of JSON logs on user activity on the app, there is also a directory with JSON metadata on the songs in their app.

### Song datasets: 
    - all json files are nested in subdirectories under */data/song_data*.

### Log datasets: 
    -all json files are nested in subdirectories under */data/log_data*.
    
## Database Schema
The database schema for this context is the Star Schema: 
    - Main fact table containing all the measures associated to each song play:
        -songplays (records with page NextSong)
        -songplay_id (INT) PRIMARY KEY 
        -start_time (DATE) NOT NULL
        -user_id (INT) NOT NULL
        -level (TEXT) (plan type)
        -song_id (TEXT) NOT NULL
        -artist_id (TEXT) NOT NULL
        -session_id (INT) NOT NULL
        -location (TEXT) (User location)
        -user_agent (TEXT) (Method of access to the platform)
### 4 dimensional tables
    - Users table:
        -user_id (TEXT) PRIMARY KEY
        -first_name (TEXT) NOT NULL
        -last_name (TEXT) NOT NULL
        -gender (TEXT) NOT NULL
        -level (TEXT) NOT NULL (plan type)
    - Songs table:
        -song_id (TEXT) NOT NULL
        -title (TEXT) NOT NULL
        -artist_id (TEXT) NOT NULL
        -year (INT) NOT NULL
        -duration (FLOAT) NOT NULL
    - Artists table:
        -artist_id (TEXT) PRIMARY KEY
        -name (TEXT) NOT NULL
        -location (TEXT) NOT NULL
        -latitude (FLOAT) NOT NULL
        longitude (FLOAT) NOT NULL
    - Time table:
        -start_time (DATE) PRIMARY KEY
        -hour (INT)
        -day (INT)
        -week (INT)
        -month (INT)
        -year (INT)
        -weekday (TEXT)
        