# SONGS LIST

http://wynkmusic.com

FEATURES

    Users should able to view all the songs
    
FEATURE 1:List of songs

create table song_list(
song_id number,
constraint sid_fk foreign key(song_id) references industry(song_id),
song_name varchar(100) not null unique,
song_duration number not null,
constraints dura_cq check(song_duration>0),
song_size number not null,
music_director varchar(100) not null,
released_year number,
movie_name varchar(100) not null,
song_lang varchar(50) not null,
actor_name varchar(100),
);

