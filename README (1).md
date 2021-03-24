# Data Modelling with Postgres

## Context

A startup called Sparkify wants to analyze their overall user activity on their new music streaming app specifically their song data. Currently, they don't have an easy way to query their data. The data currently lives in a  directory of JSON logs on user activity on the app, there is also a directory with JSON metadata on the songs in their app.

### Song datasets: 
    - all json files are nested in subdirectories under */data/song_data*.

### Log datasets: 
    -all json files are nested in subdirectories under */data/log_data*.