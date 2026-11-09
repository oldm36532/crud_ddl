# crud_ddl
crud_lesson
create table reviews(
 id serial primary key,
 product_id integer not null references products(id),
 user_id integer not null references users(id),
 rating integer not null check(rating>=1 and rating<=5),
 comment text ,
 created_at timestamp not null default now(),
 unique(user_id, product_id)
 );
 
alter table users add column phone_number text;

alter table orders add check (quantity<=10);

insert into products (name,category,price,stock)
values ('Studio Desk Lamp','Office',45.50,25)
returning id

update products 
set price=price*1.10
where category='Furniture'

update orders
set status='cancelled'
where status='pending' and created_at<now() - interval'30 days'

delete from products 
where id=1;
SQL Error [23503]: ERROR: update or delete on table "products" violates foreign key constraint "orders_product_id_fkey" on table "orders"
  Подробности: Key (id)=(1) is still referenced from table "orders".
удалить его нельзя так как он есть в orders и удаленние нарушит целостность данных в нём

create index idx_orders_status_created_at on orders(status,created_at);

begin;
insert into orders (user_id,product_id,quantity,status)
values(1,1,2,'pending');

update products 
set stock=stock-2
where id=1;
commit;
