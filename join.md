###��ѯ�����й㶫ʡ����\�ء�˼·�ܽ�
------
### 1. ��������Ϊ��
					(1)����ѯ�㶫ʡ���е���
					(2)���ڲ�ѯ�㶫ʡ���е���(��)
	
### 2.select x.id(��id)���������֣�s.cityName(ʡ����),x.cityName(������) from s_provinces x��ʡ��

	
### 3. ��ʼ��join���Ӳ�ѯ��
					(1)����ʡ�ݺ���id���й�����ѯ(on����)
					(2)��ʡ�ݺ�ʡid���й�����ѯ(on����)
					������p.cityName='�㶫ʡ'
					
					Union all ���Ƕ����ݽ��в��������������ظ��У�����������
					
				select s.id(ʡid),p.cityName(������),s.cityName(������),null from s_provinces(ʡ) s
					join s_provinces p on s.parentId = p.id
					������p.cityName='�㶫ʡ'
### 4.��ϸ���룺

select x.id,p.cityName,s.cityName,x.cityName
from s_provinces x
        join s_provinces s on x.parentId = s.id
        join s_provinces p on s.parentId = p.id
where  p.cityName='�㶫ʡ'
union all
select s.id,p.cityName,s.cityName,null  from s_provinces s
    join s_provinces p on s.parentId = p.id
where p.cityName ='�㶫ʡ';	