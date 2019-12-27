drop table books;
create table books(
book_id number not null unique,
book_name varchar2(100) not null unique,
author_name varchar(100) not null,
price number not null,
publisher varchar(50),
edition_no number,
category_name varchar(100)not null,
active char(1),
constraint active_ck check(active in('y','n'))
);
insert into books(book_id,book_name,author_name,price,publisher,edition_no,category_name,active) values(1,'CODE COMPLETE','SEENI',200,'SEENI',2,'PROGRAMMING','y');
select * from books;
drop table orders;
create table orders(
order_id number not null ,
username varchar(50) not null,
book_id number,
constraint bid_fk foreign key(book_id) references books(book_id),
ordered_date timestamp default systimestamp,
delivered_date date,
price number,
quantity number,
constraint quant_ck check(quantity>=1),
status varchar(50),
constraint status_ck check(status in('DELIVERED','PROCESSING','PENDING')),
comments varchar(100),
constraint comments_ck check(comments in('NOT AVAILABLE','INVALID ADDRESS'))
);
insert into orders(order_id,username,book_id,price,quantity,comments,status) values(2380,'VASAN',1,200,1,'NOT AVAILABLE','PENDING');
update orders set delivered_date=sysdate;
update orders set quantity=2 where quantity =1;
select * from orders;
