# SONGS LIST

http://wynkmusic.com

## FEATURE

    Users should able to view all the songs
    
### FEATURE 1:List of songs

QUERY:

```sql
create table song_list(
song_name varchar(100) not null unique,
song_duration number not null,
constraints dura_cq check(song_duration>10),
song_size number not null,
constrains size_ck check(song_size>0),
music_director varchar(100) not null,
released_year number not null,
constraints year_ck check(released_year>=1900),
movie_name varchar(100) not null,
song_lang varchar(50) not null,
constraints lang_ck check(status in('TAMIL','ENGLISH','MALAYALAM','TELUGU')),
actor_name varchar(100),
);

```
