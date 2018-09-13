###总结：表的分离
首先创建三张表：lagou_city（城市表）    从china_city表中提取出
		lagou_company（公司表） 从 lagou_position_bk 中分离出
		lagou_position（职位表）将公司、城市信息从主表分离出去
------
查出全国的省市县信息，从china_city表中提取出。
lagou_city（城市表）：要求的字段：id、province、city、district、shortCity
		      使用join查询
		      查询出所有区县的数据，查询出所有市的数据，在创建lagou_city表
代码实现：
create table lagou_city as
select d.id, p.cityName as province, c.cityName as city, d.cityName as district from
  (select * from china_city where depth=3) d
    join china_city c on d.parentId = c.id and c.depth=2
    join china_city p on c.parentId = p.id and p.depth=1
union
select c.id, p.cityName as province, c.cityName as city, null as district from (select * from china_city where depth=2) c
  join china_city p on c.parentId = p.id and p.depth = 1;
------

所有的公司表，从 lagou_position_bk 中分离出。
lagou_company（公司表）：要求的字段：t.company_id(cid)、 t.company_full_name(name)、 t.company_short_name(short_name)、 t.company_size(size)、 t.financestage(financestage)

代码实现：
drop table if exists lagou_company;
create table lagou_company as
  select distinct t.company_id         as cid,
                  t.company_short_name as short_name,
                  t.company_full_name  as full_name,
                  t.company_size       as size,
                  t.financestage
  from lagou_position_bk t;

------

将公司、城市信息从主表分离出去:得出lagou_position（职位表）
1、先查询lagou_city表id
2、查询lagou_company表id
3、查询bk表的与position有关数据
4、将所有数据插入lagou_position表

代码实现：
create table lagou_position
as
select pid, cid as city, company_id as company, position, field, salary_min, salary_max, workyear, education, ptype, pnature, advantage, published_at, updated_at from
(
  -- position 表中 district 为空的数据
  select p.*, c.cid from (select * from lagou_position_bk where district is null) p
     join lagou_city c on c.city like concat(p.city, '%') and c.district is null

  union all

  -- position 表中 district 不为空的数据
  select p.*, c.cid from (select * from lagou_position_bk where district is not null) p
    join lagou_city c on c.city like concat(p.city, '%') and c.district like concat(p.district, '%')

) as ppp;