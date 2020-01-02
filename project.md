SONGS LIST

```sql

TABLE QUERY

drop table userlogin;
create table userlogin(
user_id number not null,
constraints uid_uk unique(user_id),
username varchar2(30) not null,
email_id varchar2(50) not null,
password varchar2(30) not null,
constraint pwd_ck check(length(password)>=8),
constraints uname_pwd_uq unique(username,password),
constraints ename_pwd_uq unique(email_id,password),
mobile_no number not null,
constraint mno_ck check(mobile_no between 1000000000 and 9999999999)
);

INSERT QUERY

insert into userlogin(user_id,username,email_id,password,mobile_no) values(101,'SATHISH','s@gmail.com',12345678,9182734579);
insert into userlogin(user_id,username,email_id,password,mobile_no) values(102,'RAJU','r@gmail.com',87654321,9127287587);
insert into userlogin(user_id,username,email_id,password,mobile_no) values(103,'RAJ','ra@gmail.com',12676831,9387892572);
select * from userlogin;


TABLE

| USER_ID | USERNAME | EMAIL_ID     | PASSWORD | MOBILE_NO  |
|---------|----------|--------------|----------|------------|
| 101     | SATHISH  | s@gmail.com  | 12345678 | 9182734579 |
| 102     | RAJU     | r@gmail.com  | 87654321 | 9127287587 |
| 103     | RAJ      | ra@gmail.com | 12676831 | 9387892572 |


TABLE QUERY

drop table song_list;
create table song_list(
song_number number not null,
constraints sno_uq unique(song_number),
song_name varchar2(100) not null,
music_director varchar2(50) not null,
lyricist varchar2(20) not null,
singers varchar2(30) not null,
movie_name varchar2(40),
constraints mname_sname_uq unique(song_name,movie_name)
);

INSERT QUERY


insert into song_list(song_number,song_name,music_director,lyricist,singers,movie_name)
values(1,'KADHAL AASAI','YUVAN SHANKAR RAJA','KABILAN','YUVAN SHANKAR RAJA','ANJAAN');
insert into song_list(song_number,song_name,music_director,lyricist,singers,movie_name) 
values(2,'ALAPPORAN TAMILAN','AR RAHMAN','VIVEK','SATHYA PRAKASH','MERSAL');
insert into song_list(song_number,song_name,music_director,lyricist,singers) 
values(3,'OH BABY','JUSTIN BIEBER','JUSTIN BIEBER','JUSTIN BIEBER');
insert into song_list(song_number,song_name,music_director,lyricist,singers,movie_name) 
values(4,'JAI HO','AR RAHMAN','AR RAHMAN','AR RAHMAN','SLUMDOG MILLIONAIRE');
select * from song_list;


TABLE


| SONG_NUMBER | SONG_NAME         | MUSIC_DTRECTOR     | LYRICIST      | SINGERS            | MOVIE_NAME          |
|-------------|-------------------|--------------------|---------------|--------------------|---------------------|
| 1           | KADHAL AASAI      | YUVAN SHANKAR RAJA | KABILAN       | YUVAN SHANKAR RAJA | ANJAAN              |
| 2           | ALAPPORAN TAMILAN | AR RAHMAN          | VIVEK         | SATHYA PRAKASH     | MERSAL              |
| 3           | OH BABY           | JUSTIN BIEBER      | JUSTIN BIEBER | JUSTIN BIEBER      | (NULL)                    |
| 4           | JAI HO            | AR RAHMAN          | AR RAHMAN     | AR RAHMAN          | SLUMDOG MILLIONAIRE |



SELECT QUERY

drop table year;
create table year(
song_number number,
constraints sno_fk foreign key(song_number) references song_list(song_number),
song_released_year number not null,
constraints syear_ck check(song_released_year between 2000 and 2020),
song_tone varchar(20) not null,
constraints stune_ck check(song_tone in('MELODY','LOVE','ROCK','JAZZ','HIP HOP')),
song_duration_in_seconds number not null,
constraints sduration_ck check(song_duration_in_seconds>0),
song_size_in_mb number not null,
constraints ssize_ck check(song_size_in_mb>0),
song_lang varchar2(30) not null,
constraints slang_ck check(song_lang in ('TAMIL','ENGLISH','HINDI')),
download_date date,
constraints ddate_ck check(download_date>=song_released_date)
);

INSERT QUERY

insert into year(song_number,song_released_date,song_tone,song_duration_in_seconds,song_size_in_mb,song_lang,download_date) values(1,to_date('11-10-2016','dd-mm-yyyy'),'LOVE',243,5.64,'TAMIL',to_date('28-10-2016','dd-mm-yyyy'));
insert into year(song_number,song_released_date,song_tone,song_duration_in_seconds,song_size_in_mb,song_lang,download_date) values(2,to_date('11-10-2019','dd-mm-yyyy'),'JAZZ',310,10.8,'TAMIL',to_date('21-10-2019','dd-mm-yyyy'));
insert into year(song_number,song_released_date,song_tone,song_duration_in_seconds,song_size_in_mb,song_lang,download_date) values(3,to_date('05-08-2012','dd-mm-yyyy'),'LOVE',217,6.66,'ENGLISH',to_date('11-11-2013','dd-mm-yyyy'));
insert into year(song_number,song_released_date,song_tone,song_duration_in_seconds,song_size_in_mb,song_lang,download_date) values(4,to_date('09-09-2009','dd-mm-yyyy'),'JAZZ',346,10.66,'HINDI',to_date('14-3-2012','dd-mm-yyyy'));
select * from year;

TABLE


| SONG_NUMBER | SONG_RELEASED_YEAR | SONG_TONE | SONG_DURATION_IN_SECONDS | SONG_SIZE_IN_MB | SONG_LANG | DOWNLOAD_DATE |
|-------------|--------------------|-----------|--------------------------|-----------------|-----------|---------------|
| 1           | 11-10-16           | LOVE      | 243                      | 5.64            | TAMIL     | 28-10-16      |
| 2           | 11-10-19           | JAZZ      | 310                      | 10.8            | TAMIL     | 21-10-19      |
| 3           | 05-08-12           | LOVE      | 217                      | 6.66            | ENGLISH   | 11-11-13      |
| 4           | 09-09-09           | JAZZ      | 346                      | 10.66           | HINDI     | 14-03-12      |

TABLE QUERY

drop table userchoice;
create table userchoice(
user_id number not null,
constraints uid_fk foreign key(user_id) references userlogin(user_id),
song_sequence varchar2(30) default 'SEQUENCE',
constraints sseq_ck check(song_sequence in ('SHUFFLE ALL','SEQUENCE','REPEAT MODE')),
song_rating number not null,
constraints srating_ck check(song_rating between 0 and 5),
button varchar2(10) not null,
constraints button_ck check (button in ('PAUSE','PLAY')),
song_wants_to_play number not null,
constraints wtp_fk foreign key(song_wants_to_play) references song_list(song_number),
add_to_favourites char(1),
constraints atf_ck check(add_to_favourites in('Y','N'))
);

INSERT QUERY

insert into userchoice(user_id,song_sequence,song_rating,button,song_wants_to_play,add_to_favourites) values(101,'SHUFFLE ALL',4.5,'PLAY',3,'Y');
insert into userchoice(user_id,song_sequence,song_rating,button,song_wants_to_play,add_to_favourites) values(102,'SEQUENCE',2.5,'PAUSE',1,'N');
insert into userchoice(user_id,song_sequence,song_rating,button,song_wants_to_play,add_to_favourites) values(103,'SEQUENCE',3,'PAUSE',1,'Y');
update userchoice set song_sequence='SEQUENCE' where user_id=101;
select * from userchoice;

TABLE

| USER_ID | SONG_SEQUENCE | BUTTON | ADD_TO_FAVOURITES | SONG_WANTS_TO_PLAY | SONG_RATING |
|---------|---------------|--------|-------------------|--------------------|-------------|
| 101     | SEQUENCE      | PLAY   | Y                 | 3                  | 4.5         |
| 102     | SEQUENCE      | PAUSE  | N                 | 1                  | 2.5         |
| 103     | SEQUENCE      | PAUSE  | Y                 | 1                  | 3           |


