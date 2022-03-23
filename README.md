# HQL
题目一：
select u.age,avg(r.rate) avgrate
from t_rating r join t_movie m join t_user u 
on r.movieid=m.movieid and r.userid=u.userid
where m.movieid='2116'
group by u.age
order by u.age;

题目二：
select sex,moviename,avgrate,total from (
select u.sex,m.moviename,avg(r.rate) avgrate,count(u.sex) total
from t_rating  r join t_movie m join t_user u 
on r.movieid=m.movieid and r.userid=u.userid
where u.sex='M' 
group by u.sex,m.moviename) t1 where t1.total >50 
order by t1.avgrate desc
limit 10;

题目三：
select m2.movieid,m2.moviename,avg(r2.rate) avgrate from 
(select movieid,rate from t_rating where userid in (
select userid from (
select u.userid,count(*) total
from t_rating  r join t_movie m join t_user u 
on r.movieid=m.movieid and r.userid=u.userid
where u.sex='F' 
group by u.userid
order by total desc 
limit 1) t1)
order by rate desc
limit 10) t2 join t_movie m2 join t_rating r2
on t2.movieid=m2.movieid and t2.movieid=r2.movieid
group by m2.movieid,m2.moviename
order by avgrate desc;
