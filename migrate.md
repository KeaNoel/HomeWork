###�ܽ᣺��ķ���
���ȴ������ű�lagou_city�����б�    ��china_city������ȡ��
		lagou_company����˾�� �� lagou_position_bk �з����
		lagou_position��ְλ������˾��������Ϣ����������ȥ
------
���ȫ����ʡ������Ϣ����china_city������ȡ����
lagou_city�����б���Ҫ����ֶΣ�id��province��city��district��shortCity
		      ʹ��join��ѯ
		      ��ѯ���������ص����ݣ���ѯ�������е����ݣ��ڴ���lagou_city��
����ʵ�֣�
create table lagou_city as
select d.id, p.cityName as province, c.cityName as city, d.cityName as district from
  (select * from china_city where depth=3) d
    join china_city c on d.parentId = c.id and c.depth=2
    join china_city p on c.parentId = p.id and p.depth=1
union
select c.id, p.cityName as province, c.cityName as city, null as district from (select * from china_city where depth=2) c
  join china_city p on c.parentId = p.id and p.depth = 1;
------

���еĹ�˾���� lagou_position_bk �з������
lagou_company����˾����Ҫ����ֶΣ�t.company_id(cid)�� t.company_full_name(name)�� t.company_short_name(short_name)�� t.company_size(size)�� t.financestage(financestage)

����ʵ�֣�
drop table if exists lagou_company;
create table lagou_company as
  select distinct t.company_id         as cid,
                  t.company_short_name as short_name,
                  t.company_full_name  as full_name,
                  t.company_size       as size,
                  t.financestage
  from lagou_position_bk t;

------

����˾��������Ϣ����������ȥ:�ó�lagou_position��ְλ��
1���Ȳ�ѯlagou_city��id
2����ѯlagou_company��id
3����ѯbk�����position�й�����
4�����������ݲ���lagou_position��

����ʵ�֣�
create table lagou_position
as
select pid, cid as city, company_id as company, position, field, salary_min, salary_max, workyear, education, ptype, pnature, advantage, published_at, updated_at from
(
  -- position ���� district Ϊ�յ�����
  select p.*, c.cid from (select * from lagou_position_bk where district is null) p
     join lagou_city c on c.city like concat(p.city, '%') and c.district is null

  union all

  -- position ���� district ��Ϊ�յ�����
  select p.*, c.cid from (select * from lagou_position_bk where district is not null) p
    join lagou_city c on c.city like concat(p.city, '%') and c.district like concat(p.district, '%')

) as ppp;