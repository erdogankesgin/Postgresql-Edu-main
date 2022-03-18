# POSTGRESQL EĞİTİM #1
## CREATE TABLE,SELECT,ALTER TABLE,UPDATE ve JOIN KULLANIMI
`Ödeve başlamadan önce Postgresql ile ilgili bilgisi olmayan arkaşdaşların Youtube Eğitim Videosunu izlemesini tavsiye ederim`
- Postgresql Veritabanın yönetmek için gereken araç :  [PgAdmin](asset/Pgadmin_User.png) 
- Panogps.txt tablosunu panogpsline_user  olarak veritabanına import edelim.Import için şimdilik Qgis Kullanabilirsiniz. (user yerine isminizi yazınız)

[Upload Görsel 1](asset/upload1.png)

[Upload Görsel 2](asset/upload2.png)

[panogps.txt](asset/panogps.txt)


- Panogps tablosundan panogpsline_user olarak oluşturalım.
```sql
CREATE TABLE panogpsline_user as
SELECT filename, st_makeline(geom order by stamp) as geom
FROM panogps
GROUP BY filename
```

- panogpsline_user tablosuna yon (integer) ve kkn ( string) kolonları açalım
```sql
ALTER TABLE panogpsline_user ADD kkn text  ;

ALTER TABLE panogpsline_user ADD yon integer;
```

- panogpsline_kuser tablosundaki yon ve integer öznitelikleri aşağıdaki filenamelere göre güncelleyelim. (İsteyen alttaki joinleme sql ile bu tabloyu da veritabanına aldıktan sonra joinleyip güncelleyebilir.)

[yol_kkn_yon.csv](asset/yol_kkn_yon.csv)

```sql
UPDATE panogpsline_user SET kkn="AB-01" WHERE filename='dadasdasd'
UPDATE panogpsline_user SET yon=2 WHERE filename='dadasdasd'
UPDATE panogpsline_user SET kkn="AB-01" WHERE filename in ('dadasdasd','dasdad')
```

- Sıra geldi panogps_user tablosuna kkn ve yön bilgisini yazmak. Bunun için yok ve kkn bilgisi olan panogpsline_user tablosunu ortak olan kolondan (filename) joinleyip panogps_user kkn ve yon verilerini güncellememiz gerekiyor.

| Tablolar      | Kısaltmalar |
| ----------- | ----------- |
| panogps_user    | t1       |
| panogpsline_user   | t2        |
| panogps_user.kkn    | c1       |
| panogps_user.yon    | c2        |
| panogps_user.filename    | c3        |
| panogpsline_user.kkn    | c4       |
| panogpsline_user.yon    | c5       |
| panogpsline_user.filename    | c6       |

 `Yukardaki kısaltmalar aşağıdaki sorguda direk yerine yazılacak`


```sql
UPDATE 
    t1 -- buraya yukardaki tablodan bakılarak panogps_user yazılacak. (user: sizin adınız olacak)
SET 
    t1.c1 = t2.c3  
FROM 
    t1 p -- p nin anlamı o tabloyu kısa bir isim veriyor. Sorgunun devamında uzun ismi yerine p harifini kullanabiliyoruz.
    LEFT JOIN t2 ON p.c3=t2.c6;
```
- Panogps_user tablosundan sadece kkn='O-7/30'  ve yon=1 olan verileri yeni oluşturacağımız kkn_user isimli tabloyu veritabanına kaydedelim.

Postgresql ile ilgili linkler.

[tutorialspoint](https://www.tutorialspoint.com/postgresql/index.htm
)

[postgresqltutorial.com](https://www.postgresqltutorial.com/
)

[Youtube PgAdmin Dersleri (Ödev İçin 3. Dersi kesinlikle izleyin)](https://youtube.com/playlist?list=PLKnjBHu2xXNOooo9pcx5Dw-6zOyk1rwyO)
