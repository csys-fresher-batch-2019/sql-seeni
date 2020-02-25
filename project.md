#ONLINE MUSIC PLAYER

```sql

TABLE QUERY

drop table adminlogin;
create table adminlogin(
email_id varchar2(40),
constraints admi_pk primary key(email_id),
password varchar2(40) not null
);

INSERT QUERY

insert into adminlogin(email_id,password) values('bala@gmail.com','bala1234');
insert into adminlogin(email_id,password) values('ramesh@gmail.com','ramesh123');
select * from adminlogin;

TABLE

| EMAIL-ID         | PASSWORD  |
|------------------|-----------|
| bala@gmail.com   | bala1234  |
| ramesh@gmail.com | ramesh123 |

TABLE QUERY

drop table userlogin;
create table userlogin(
user_id number not null,
--constraints uid_uk unique(user_id),
username varchar2(30) not null,
email_id varchar2(50) not null unique,
password varchar2(30) not null,
--constraint pwd_ck check(length(password)>=8),
constraints uname_pwd_uq unique(username,password),
constraints ename_pwd_uq unique(email_id,password),
mobile_no number not null,
constraint mno_ck check(mobile_no between 1000000000 and 9999999999)
);
create sequence uid_seq start with 100 increment by 1;

INSERT QUERY

insert into userlogin(user_id,username,email_id,password,mobile_no) values(uid_seq.nextval,'SATHISH','s@gmail.com',12345678,9182734579);
insert into userlogin(user_id,username,email_id,password,mobile_no) values(uid_seq.nextval,'RAJU','r@gmail.com',87654321,9127287587);
insert into userlogin(user_id,username,email_id,password,mobile_no) values(uid_seq.nextval,'RAJ','ra@gmail.com',12676831,9387892572);
select * from userlogin;


TABLE

| USER_ID | USERNAME | EMAIL_ID     | PASSWORD | MOBILE_NO  |
|---------|----------|--------------|----------|------------|
| 100     | SATHISH  | s@gmail.com  | 12345678 | 9182734579 |
| 101     | RAJU     | r@gmail.com  | 87654321 | 9127287587 |
| 102     | RAJ      | ra@gmail.com | 12676831 | 9387892572 |


TABLE QUERY

drop table song_list;

create table song_list(
song_number number unique,
song_name varchar2(100) not null,
music_director varchar2(50) not null,
lyricist varchar2(20) not null,
singers varchar2(30) not null,
movie_name varchar2(40),
constraints mname_sname_uq unique(song_name,movie_name),
song_link varchar2(70) not null
);
create sequence song_seq start with 1 increment by 1;


INSERT QUERY


insert into song_list(song_number,song_name,music_director,lyricist,singers,movie_name,song_link)
values(song_seq.nextval,'KADHAL AASAI','YUVAN SHANKAR RAJA','KABILAN','YUVAN SHANKAR RAJA','ANJAAN','myAudio.mp3');
insert into song_list(song_number,song_name,music_director,lyricist,singers,movie_name,song_link) 
values(song_seq.nextval,'ALAPPORAN TAMILAN','AR RAHMAN','VIVEK','SATHYA PRAKASH','MERSAL','Aalaporaan.mp3');
insert into song_list(song_number,song_name,music_director,lyricist,singers,song_link) 
values(song_seq.nextval,'OH BABY','JUSTIN BIEBER','JUSTIN BIEBER','JUSTIN BIEBER','baby.mp3');
insert into song_list(song_number,song_name,music_director,lyricist,singers,movie_name,song_link)
values(song_seq.nextval,'JAI HO','AR RAHMAN','AR RAHMAN','AR RAHMAN','SLUMDOG MILLIONAIRE','jai ho.mp3');

select * from song_list;


TABLE

| SONG_NUMBER | SONG_NAME         | MUSIC_DIRECTOR     | LYRICIST      | SINGERS            | MOVIE_NAME          | SONG_LINK      |
|-------------|-------------------|--------------------|---------------|--------------------|---------------------|----------------|
| 1           | KADHAL AASAI      | YUVAN SHANKAR RAJA | KABILAN       | YUVAN SHANKAR RAJA | ANJAAN              | myAudio.mp3    |
| 2           | ALAPPORAN TAMILAN | AR RAHMAN          | VIVEK         | SATHYA PRAKASH     | MERSAL              | Aalaporaan.mp3 |
| 3           | OH BABY           | JUSTIN BIEBER      | JUSTIN BIEBER | JUSTIN BIEBER      | (NULL)              | baby.mp3       |
| 4           | JAI HO            | AR RAHMAN          | AR RAHMAN     | AR RAHMAN          | SLUMDOG MILLIONAIRE | jai ho.mp3     |



TABLE QUERY

drop table userchoice;

create table userchoice(
user_id number not null,
--constraints uid_fk foreign key(user_id) references userlogin(user_id),
song_sequence varchar2(30) default 'SEQUENCE',
constraints sseq_ck check(song_sequence in ('SHUFFLE ALL','SEQUENCE','REPEAT MODE')),
song_rating number not null,
constraints srating_ck check(song_rating between 0 and 5),
button varchar2(10) not null,
constraints button_ck check (button in ('PAUSE','PLAY')),
song_wants_to_play number not null,
--constraints wtp_fk foreign key(song_wants_to_play) references song_list(song_number),
add_to_favourites char(1),
constraints atf_ck check(add_to_favourites in('Y','N')),
premium_amount number default 400
);

INSERT QUERY


insert into userchoice(user_id,song_sequence,song_rating,button,song_wants_to_play,add_to_favourites) values(100,'SHUFFLE ALL',4.5,'PLAY',3,'Y');
insert into userchoice(user_id,song_sequence,song_rating,button,song_wants_to_play,add_to_favourites) values(101,'SEQUENCE',2.5,'PAUSE',1,'N');
insert into userchoice(user_id,song_sequence,song_rating,button,song_wants_to_play,add_to_favourites) values(102,'SEQUENCE',3,'PAUSE',1,'Y');

select * from userchoice;

TABLE

| USER_ID | SONG_SEQUENCE | SONG_RATING | BUTTON | SONG_WANTS_TO_PLAY | ADD_TO_FAVOURITES | PREMIUM_AMOUNT |
|---------|---------------|-------------|--------|--------------------|-------------------|----------------|
| 100     | SEQUENCE      | 4.5         | PLAY   | 3                  | Y                 | 400            |
| 101     | SEQUENCE      | 2.5         | PAUSE  | 1                  | N                 | 400            |
| 102     | SEQUENCE      | 3           | PAUSE  | 1                  | Y                 | 400            |


TABLE QUERY

DROP TABLE ACCOUNT_INFO;
CREATE TABLE ACCOUNT_INFO(
WANTS_TO_PREMIUM CHAR(1),
CONSTRAINTS WT_CK CHECK(WANTS_TO_PREMIUM IN('Y','N')),
ACCOUNT_NO VARCHAR(16) NOT NULL UNIQUE,
DEBIT_CARD NUMBER,
CVV NUMBER,
CONSTRAINTS CVV_DC_UK UNIQUE(DEBIT_CARD,CVV),
EXPIRY_MONTH NUMBER,
EXPIRY_YEAR NUMBER,
--CONSTRAINTS AC_NO_CK CHECK(LENGTH(ACCOUNT_NO)==16),
USER_ID NUMBER NOT NULL,
--CONSTRAINTS UI_FK FOREIGN KEY(USER_ID) REFERENCES USERLOGIN(USER_ID),
BALANCE NUMBER NOT NULL,
CONSTRAINTS BAL_CK CHECK(BALANCE>=0),
RECHARGED_DATE DATE,
UPTO_DATE DATE,
SONG_DOWNLOAD VARCHAR(3),
CONSTRAINTS SD_CK CHECK(SONG_DOWNLOAD IN('YES','NO')),
PREMIUM CHAR(1),
CONSTRAINTS PR_CK CHECK(PREMIUM IN('Y','N')),
REMAINING_BAL NUMBER
);

INSERT QUERY

INSERT INTO ACCOUNT_INFO(WANTS_TO_PREMIUM,ACCOUNT_NO,DEBIT_CARD,CVV,EXPIRY_MONTH,EXPIRY_YEAR,USER_ID,BALANCE) VALUES('Y',1234567890123456,6078897645231287,432,04,2024,101,500);
INSERT INTO ACCOUNT_INFO(WANTS_TO_PREMIUM,ACCOUNT_NO,DEBIT_CARD,CVV,EXPIRY_MONTH,EXPIRY_YEAR,USER_ID,BALANCE) VALUES('Y',3948123456789019,3214563245625543,342,10,2020,100,300);
INSERT INTO ACCOUNT_INFO(WANTS_TO_PREMIUM,ACCOUNT_NO,DEBIT_CARD,CVV,EXPIRY_MONTH,EXPIRY_YEAR,USER_ID,BALANCE) VALUES('Y',1981234156273890,2366556674556622,567,11,2022,102,400);

UPDATE ACCOUNT_INFO SET PREMIUM='Y',SONG_DOWNLOAD='YES',REMAINING_BAL=BALANCE-400,UPTO_DATE=90+SYSDATE,RECHARGED_DATE=SYSTIMESTAMP WHERE BALANCE>=400 AND WANTS_TO_PREMIUM='Y';

UPDATE ACCOUNT_INFO SET PREMIUM='N',SONG_DOWNLOAD='NO',REMAINING_BAL=BALANCE WHERE (BALANCE<400 AND WANTS_TO_PREMIUM='Y') OR (BALANCE>=400 AND WANTS_TO_PREMIUM='N'); 

SELECT * FROM ACCOUNT_INFO;

TABLE

| WANTS_TO_PREMIUM | ACCOUNT_NO       | DEBIT_CARD       | CVV | EXPIRY_MONTH | EXPIRY_YEAR | USER_ID | BALANCE | RECHARGED_DATE | UPTO_DATE | SONG_DOWNLOAD | PREMIUM | REMAINING_BAL |
|------------------|------------------|------------------|-----|--------------|-------------|---------|---------|----------------|-----------|---------------|---------|---------------|
| Y                | 1234567890123456 | 6078897645231287 | 432 | 4            | 2024        | 101     | 500     | 25-02-20       | 
25-05-20  | YES           | Y       | 100           |
| Y                | 3948123456789019 | 3214563245625543 | 342 | 10           | 2020        | 100     | 300     | (NULL)         | (NULL)    | NO            | N       | 300           |
| Y                | 1981234156273890 | 2366556674556622 | 567 | 11           | 2022        | 102     | 400     | 25-02-20       |
25-05-20  | YES           | Y       | 0             |
