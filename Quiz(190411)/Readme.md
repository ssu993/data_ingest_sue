# Quiz : Hive/Oozie 이해와 활용 (수강생 : 이수경)

## 1.
<pre><code>select prod_id, brand, name, count(o.order_id) as total_sell
from products p join order_details o on p.prod_id=o.prod_id
group by p.prod_id, p.brand, p.name
order by total_sell desc limit 3;
</pre></code>
<pre><code>+----------+-------------+--------------------------------------------------------------------+-------------+--+
| prod_id  |    brand    |                                name                                | total_sell  |
+----------+-------------+--------------------------------------------------------------------+-------------+--+
| 1274348  | Byteweasel  | Tablet PC (10 in. display, 64 GB)                                  | 119801      |
| 1274021  | Dualcore    | 8GB DDR3-1600 (PC3-12800) Dual Channel Desktop Memory Kit (2x4GB)  | 29053       |
| 1273970  | Megachango  | F Jack Male-to-Male Cable (12 in.)                                 | 14566       |
+----------+-------------+--------------------------------------------------------------------+-------------+--+
3 rows selected (64.115 seconds)
</pre></code>
