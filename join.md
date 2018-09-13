###查询出所有广东省的市\县、思路总结
------
### 1. 将问题拆分为：
					(1)、查询广东省所有的市
					(2)、在查询广东省所有的县(区)
	
### 2.select x.id(县id)，城市名字，s.cityName(省城市),x.cityName(县名字) from s_provinces x（省）

	
### 3. 开始用join连接查询：
					(1)、将省份和县id进行关联查询(on关联)
					(2)、省份和省id进行关联查询(on关联)
					条件：p.cityName='广东省'
					
					Union all （是对数据进行并集操作，包括重复行，不进行排序）
					
				select s.id(省id),p.cityName(城市名),s.cityName(城市名),null from s_provinces(省) s
					join s_provinces p on s.parentId = p.id
					条件：p.cityName='广东省'
### 4.详细代码：

select x.id,p.cityName,s.cityName,x.cityName
from s_provinces x
        join s_provinces s on x.parentId = s.id
        join s_provinces p on s.parentId = p.id
where  p.cityName='广东省'
union all
select s.id,p.cityName,s.cityName,null  from s_provinces s
    join s_provinces p on s.parentId = p.id
where p.cityName ='广东省';	