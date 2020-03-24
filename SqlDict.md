# YBB

## ALL

###### 日期-国家

```mysql
select cast(d_date as date) dd,
ifnull(sum(case when c_countyname ='日本' then n_sale end ),0) 日本,
ifnull(sum(case c_countyname when '韩国' then n_sale end ),0) 韩国,
ifnull(sum(case c_countyname when '全球' then n_sale end ),0) 全球,
ifnull(sum(case c_countyname when '美国' then n_sale end ),0) 美国,
ifnull(sum(case c_countyname when '澳大利亚' then n_sale end ),0) 澳大利亚,
ifnull(sum(case c_countyname when '欧洲' then n_sale end ),0) 欧洲,
ifnull(sum(case c_countyname when '英法俄' then n_sale end ),0) 英法俄,
ifnull(sum(case c_countyname when '中国' then n_sale end ),0) 中国

 from allerp where c_custname='携程客户'  group by cast(d_date as date) order by cast(d_date as date);
```

###### 各渠道价格

```
 select c_agentname as `供应商\代理商`,sum(n_sale) as `自2020-1-1起销售金额`,count(n_sale) 订单量,round(sum(n_sale)/count(n_sale)) 平均单价 from allerp group by c_agentname order by sum(n_sale) desc ;
```



# 飞猪

### 电话卡

###### 总利润

```mysql
select cost as 电话卡广告费,sell 电话卡销售额,sell/cost 投产比,sell*0.20-cost `在20%毛利下的利润-广告费` ,sell*0.15-cost `在15%毛利下的利润-广告费`,sell*0.10-cost `在10%毛利下的利润-广告费` from
(
select 
(select sum(花费分)/100 as card_car from run_car where 宝贝名称 like '%电话卡%' ) cost,
(select sum(erp.n_sale) as all_card_sell from erp where c_discode like '%电话卡%') sell
) z

;
```

