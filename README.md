# Data Modeling with PostgreSQL

The project "Data Modeling with PostgreSQL" is divided in two sets. One set of Python scripts and SQL querys that creates and fills the database. And another set of Jupyter Notebooks with the process followed to develop the exercise.  

## 1. Purpose of the database

### Sparkify the startup

Sparkify is a fictional startup for music streaming where the user can select which song to play from their favourite artists. The app is currently working and there is already information of the songs, artists, users and sessions stored in different JSONs.  


### Purpose of the database

The analytica team wants to get information of what songs users are listening to, but the current storage of information makes impossible to process that information. To solve that, the purpose of the database and ETL is to collect the song data and the logs, cross it and store it in a PostgreSQL database, where it could be consulted by the analytics teams in order to get the KPIs that they need.  


## 2. Database schema design

The design of the database is a Star schema. The cental fact table is Songplays where the information of each reproduction is stored. This fact table is connected to four different dimensions table:  
        - Users table: Table where the users information as name, gender or level is stored.  
        - Songs table: Where the songs data is stored, as title, year or duration.  
        - Artist table: The data about the artist, like name and location is stored here.  
        - Time table: Table where each timestamp is translated from a number in ms to a hour, day, month, year format.  
The main advantages of a Star Schema is that it is that can be used with simple querys, is fast to aggregate data and it is improved for reading performance. The problem is that this cames with the inconvinients of a denormalized database, as problems with the data integrity and duplicate information.  
To create this database, the "create_tables.py" script must be executed from terminal.  


## 3. ETL Pipeline

The ETL is the process that extracts, process and inserts the information from the JSONs where the information in currently stored to the databases created with the "create_tables.py" script.  This pipeline is divided in two main functions, one to process the songs files and a second one to process the logs, name respectively "process_song_file" and "process_log_file".  
The function that reads the songs file moves the song and artist info to the database using the queries created in "sql_queries.py".  
The function that reads the logs data does several actions:  
    - Extracts the information.  
    - Filters it for getting the "NexSong" logs.  
    - Converts the timestamps to a more understandable format and stores it into a different table.  
    - Creates the users table.  
    - Crosses the title and artist with the songs and artists tables to add the song id and artist id.  
This pipeline is simple but efective in its use, as it avoid duplicates and data integrity problems by implementing safeguards as the PRIMARY KEY in the databases and JOINS to get the id of a song instead of just the name.  

## 4. Use
    
To create the database, the "create_tables.py" script must be execute it from terminal. This will create all the necesesairy tables.  
To read and fill the tables, the script "etl.py" has to be executed, proving the information in two different directories, "data/log_data" for logs and "data/song_data" for song data. That information must be in JSON structure, following the next template:  

***Logs:***
    {
        "artist": null,
        "auth": "Logged In",
        "firstName": "Walter",
        "gender": "M",
        "itemInSession": 0,
        "lastName": "Frye",
        "length": null,
        "level": "free",
        "location": "San Francisco-Oakland-Hayward, CA",
        "method": "GET",
        "page": "Home",
        "registration": 1540919166796.0,
        "sessionId": 38,
        "song": null,
        "status": 200,
        "ts": 1541105830796,
        "userAgent": "\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"",
        "userId": "39"
    }

***Songs:***
    {
        "num_songs": 1,
        "artist_id": "ARD7TVE1187B99BFB1",
        "artist_latitude": null,
        "artist_longitude": null,
        "artist_location": "California - LA",
        "artist_name": "Casual",
        "song_id": "SOMZWCG12A8C13C480",
        "title": "I Didn't Mean To",
        "duration": 218.93179,
        "year": 0
    }
