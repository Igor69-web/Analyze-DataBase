```sql
select   a.nnap,
convert2dat (a.dnap) dnap,
--dd.classkod,
--dd.nkod,
a.nkod_poi,
a.fa,a.im,
a.ot, convert2dat (a.dr) dr,
a.org_poi,
a.org_contr,
a.model,
a.price,
k.nkod,
convert2dat (a.dat_stat) dat_stat,
a.regnum,
a.reestr,
convert2dat (a.paydate) paydate,
a.num_dog,
convert2dat (a.date_dog) date_dog,
case when a.vr_805=1 then 'ФБ' when a.vr_805=2 then 'ГБ' else '' end as bud_poi,
case when a.vr_805_korr=1  then 'ФБ' when a.vr_805_korr=2 then 'ГБ' else '' end as bud_poi_korr,
e.pku,
e.nps,
e.famil,
e.imja,
e.otch,
convert2dat (e.drog) drog,
s2.nkod as priz_uch,
d.classkod as classkod_misso,
d.nkod as nkod_misso,
convert2dat (c.datpl) datpl,
g.comment,
case when c.kif=100 then 'ФБ' when c.kif=200 then 'ГБ' else '' end as bud_misso,
case when m.issvo='true' then 'да' else '' end as svo 
--into tempdb.poii_otch_test
from tsr.poi_otchet a
left join f_tsr b on a.nnap=b.directnumbrequest and a.dnap::date=b.daterequest::date
left join f6izm l on l.id=b.f6izm_id
left join f6 s on s.id=l.f6_id 
left join s2 on s2.id=l.isu
left join f16 m on m.id=b.f16_id
left join f7 c on c.f_tsr_id=b.id
left join f7comment g on g.f7_id=c.id
left join s_tsr d on d.id=b.s_tsr_id/10000*10000 and d.ur=2 
left join s_tsr dd on dd.code_esui=a.kodm::int and dd.ur=2 
left join f2 e on e.id=s.f2_id
left join tsr.ed_izm_poi k on k.kod=a.vr_808::int
where dat_stat between @@@D1 and @@@D2 and stat=5 
order by a.regnum,a.num_dog,a.paydate;
```
