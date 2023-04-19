# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.

INSERT INTO yazar( yazarad,yazarsoyad) values ('KEMAL','UYMAZ') 
	
	2) Biyografi türünü tür tablosuna ekleyiniz.
INSERT INTO tur( turadi) values ('Biyografi') 
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.

INSERT INTO ogrenci(ograd,ogrsoyad,cinsiyet,sinif) VALUES ('ÇAĞLAR','ÜZÜMCÜ','E','9B') ,('LEYLA','ALAGÖZ','K','11C'),('Ayşe','Bektaş','K') 
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

INSERT INTO yazar(yazarad,yazarsoyad) SELECT ograd,ogrsoyad from ogrenci WHERE RAND() LIMIT 1;

SELECT @@IDENTITY; //son eklenenin id nosunu veriyor 
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
INSERT INTO yazar(yazarad,yazarsoyad) SELECT ograd,ogrsoyad from ogrenci WHERE ogrno between 10 and 30
//
INSERT INTO yazar(yazarad,yazarsoyad) select ograd, concat(trim(ogrsoyad),'-',ogrno) from ogrenci where ogrno between 10 and 30

    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)

INSERT INTO yazar(yazarad,yazarsoyad) VALUES('Nurettin','Belek') 7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
UPDATE ogrenci SET SELECT sinif form ogrenci WHERE ogrno=3 8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
UPDATE ogrenci
SET sinif= '10A' where sinif='9A'

    9) Tüm öğrencilerin puanını 5 puan arttırın.
    UPDATE ogrenci

SET puan= puan+5 10) 25 numaralı yazarı silin.
DELETE FROM yazar where yazarno=25

    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
    SELECT * from ogrenci where dtarih is null

    12) Doğum tarihi null olan öğrencileri silin.
    DELETE FROM ogrenci where dtarih is null

    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

    UPDATE kitap

SET puan= puan+2
WHERE where kitapadi like 'a%'

    14) Kişisel Gelişim isimli bir tür oluşturun.

INSERT INTO tur (turadi) values ('Kişisel Gelişim') 15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
UPDATE tur SET turadi='Kişisel Gelişim' WHERE(SELECT \*
from kitap as k
INNER JOIN tur as t on t.turno=k.turno where k.kitapadi='Başarı Rehberi')

    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen 'ogrencilistesi' adında bir prosedür oluşturun.


    17) Öğrenci tablosuna yeni öğrenci eklemek için 'ekle' adında bir prosedür oluşturun.
    CREATE PROCEDURE EKLE(
    	IN name VARCHAR(50), //tablo değişkenlerinin regex
    	IN surname VARCHAR(50)
    	)
    BEGIN
    	INSERT INTO ogrenci(ograd,ogrsoyad)
    	VALUES(name,surname);
    END;

    	CALL Ekle('onur','soyad');	//ekle fonsiyonunu çalıştırıyor.ogrenci db sine yeni öğrenci ekliyor
    )

    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan 'sil' adında bir prosedür oluşturun.


    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.


    20) Öğrenci adı ve soyadını 'Ad Soyad' olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
     SELECT CONCAT(ograd,ogrsoyad) as Ad_Soyad from ogrenci

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.


    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
     SELECT *

from kitap
where turno in
(select turno from tur where turno=1) 23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
SELECT \* from kitap as k INNER JOIN yazar as y ON y.yazarno=k.yazarno where y.yazarad like 'e%'

    24) Kitap okumayan öğrencileri listeleyiniz.
     SELECT * from kitap as k INNER JOIN islem as i ON i.kitapno=k.kitapno INNER JOIN ogrenci as o on o.ogrno=i.ogrno where i.atarih is null

    25) Okunmayan kitapları listeleyiniz

SELECT k.kitapadi from kitap as k INNER JOIN islem as i ON i.kitapno=k.kitapno where i.atarih is null group by k.kitapadi 

	26) Mayıs ayında okunmayan kitapları listeleyiniz.
SELECT k.kitapadi from kitap as k INNER JOIN islem as i ON i.kitapno=k.kitapno where not month(i.atarih)=05 group by k.kitapadi
