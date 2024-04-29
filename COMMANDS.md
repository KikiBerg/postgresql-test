# 02 - PostgreSQL from the Command Line

### Quit the entire Postgres CLI
- `\q`

### Connect to the "chinook" Postgres CLI database
- `psql -d chinook`

### Display all tables on the "chinook" database
- `\dt`

### Quit the query / return back to CLI after a query
- `q`

### Retrieve all data from the "Artist" table
- `SELECT * FROM "Artist";`
- `SELECT * FROM "artist";`

### Retrieve only the "Name" column from the "Artist" table
- `SELECT "Name" FROM "Artist";`
- `SELECT "name" FROM "artist";`

### Retrieve only "Queen" from the "Artist" table
- `SELECT * FROM "Artist" WHERE "Name" = 'Queen';`
- `SELECT * FROM "artist" WHERE "name" = 'Queen';`

### Retrieve only "Queen" from the "Artist" table, but using the "ArtistId" of '51'
- `SELECT * FROM "Artist" WHERE "ArtistId" = 51;`
- `SELECT * FROM "artist" WHERE "artist_id" = 51;`

### Retrieve all albums from the "Album" table, using the "ArtistId" of '51'
- `SELECT * FROM "Album" WHERE "ArtistId" = 51;`
- `SELECT * FROM "album" WHERE "artist_id" = 51;`

### Retrieve all tracks from the "Track" table, using the "Composer" of 'Queen'
- `SELECT * FROM "Track" WHERE "Composer" = 'Queen';`
- `SELECT * FROM "track" WHERE "composer" = 'Queen';`

---

## OPTIONAL

### Copy the results into a .CSV file
- `\copy (SELECT * FROM "Track" WHERE "Composer" = 'Queen') TO 'test.csv' WITH CSV DELIMITER ',' HEADER;`
- `\copy (SELECT * FROM "track" WHERE "composer" = 'Queen') to 'test.csv' WITH CSV DELIMITER ',' HEADER;` 

### Copy the results into a .JSON file
- Line 1: `\o test.json`
- Line 2: `SELECT json_agg(t) FROM  (SELECT * FROM "Track" WHERE "Composer" = 'Queen') t;`
 `SELECT json_agg(t) FROM (SELECT * FROM "track" WHERE "composer" = 'Queen') t;`

---

# 03 - Installing the Libraries and Setting Up

### Install the "psycopg2" Python package
- `pip3 install psycopg2`

### Create a new file: "sql-psycopg2.py"
- `touch sql-psycopg2.py`

---

# 04 - Introducing an ORM

### Install the "SQLAlchemy" Python package
- `pip3 install sqlalchemy==1.4.46`

---

# 05 - Running Basic Queries

### Create a new file called "sql-expression.py"
- `touch sql-expression.py`

### Query 1 - select all records from the "artist" table
- `select_query = artist_table.select()`

### Query 2 - select only the "name" column from the "artist" table
- `select_query = artist_table.select().with_only_columns([artist_table.c.name])`

### Query 3 - select only 'Queen' from the "artist" table
- `select_query = artist_table.select().where(artist_table.c.name == "Queen")`

### Query 4 - select only by 'artist_id' #51 from the "artist" table
- `select_query = artist_table.select().where(artist_table.c.artist_id == 51)`

### Query 5 - select only the albums with 'artist_id' #51 on the "album" table
- `select_query = album_table.select().where(album_table.c.artist_id == 51)`

### Query 6 - select all tracks where the composer is 'Queen' from the "track" table
- `select_query = track_table.select().where(track_table.c.composer == "Queen")`

---

# 06 - Introducing Class-Based Models

### Create a new file called "sql-orm.py"
- `touch sql-orm.py`

### Query 1 - select all records from the "artist" table
```python
artists = session.query(artist)
for artist in artists:
    print(artist.artist_id, artist.name, sep=" | ")
```

### Query 2 - select only the "Name" column from the "artist" table
```python
artists = session.query(artist)
for artist in artists:
    print(artist.name)
```

### Query 3 - select only "Queen" from the "artist" table
```python
artist = session.query(artist).filter_by(name="Queen").first()
print(artist.artist_id, artist.name, sep=" | ")
```

### Query 4 - select only by "artist_id" #51 from the "artist" table
```python
artist = session.query(artist).filter_by(artist_id=51).first()
print(artist.artist_id, artist.name, sep=" | ")
```

### Query 5 - select only the albums with "ArtistId" #51 on the "Album" table
```python
albums = session.query(album).filter_by(artist_id=51)
for album in albums:
    print(album.album_id, album.title, album.artist_id, sep=" | ")
```

### Query 6 - select all tracks where the composer is "Queen" from the "Track" table
```python
tracks = session.query(track).filter_by(composer="Queen")
for track in tracks:
    print(
        track.track_id,
        track.name,
        track.album_id,
        track.media_type_id,
        track.genreId,
        track.composer,
        track.milliseconds,
        track.bytes,
        track.unit_price,
        sep=" | "
    )
```

---

# 07 - CodeAlong: Create and Read

### Create a new file called "sql-crud.py"
- `touch sql-crud.py`

---

