# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
SELECT ograd, ogrsoyad, atarih
FROM ogrenci, islem
WHERE ogrenci.ogrno = islem.ogrno 2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
SELECT kitapadi, turadi
FROM kitap, tur
WHERE kitap.turno = tur.turno
AND turadi IN('Fıkra', 'Hikaye') 3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
SELECT ogrenci.ogrno, ograd, ogrsoyad, kitapadi
FROM ogrenci, islem, kitap
WHERE ogrenci.ogrno = islem.ogrno
AND islem.kitapno = kitap.kitapno
AND ogrenci.sinif IN('10B', '10C')

    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o
    INNER JOIN islem as i
    ON o.ogrno = i.ogrno

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    SELECT k.kitapadi, t.turadi FROM kitap as k
    INNER JOIN tur as t
    ON k.turno = t.turno
    WHERE t.turadi IN('Fıkra', 'Hikaye')

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi FROM ogrenci as o
    JOIN islem as i ON o.ogrno = i.ogrno
    JOIN kitap as k ON k.kitapno = i.kitapno
    WHERE o.sinif IN('10B', '10C')
    ORDER BY o.ograd

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o
    LEFT JOIN islem as i
    ON o.ogrno = i.ogrno

    8) Kitap almayan öğrencileri listeleyin.

    SELECT o.*, i.atarih FROM ogrenci as o
    LEFT JOIN islem as i
    ON o.ogrno = i.ogrno
    WHERE i.atarih is null

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    SELECT k.kitapno, k.kitapadi, count(i.kitapno) FROM kitap as k
    JOIN islem as i
    ON k.kitapno= i.kitapno
    GROUP BY k.kitapno, k.kitapadi
    ORDER BY kitapno

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT k.kitapno, k.kitapadi, count(i.kitapno) FROM kitap as k
    LEFT JOIN islem as i
    ON k.kitapno= i.kitapno
    GROUP BY k.kitapno, k.kitapadi
    ORDER BY kitapno

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    SELECT  o.ograd, o.ogrsoyad, k.kitapadi FROM ogrenci as o
    JOIN islem as i ON o.ogrno = i.ogrno
    JOIN kitap as k ON k.kitapno = i.kitapno


    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    SELECT  o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi, i.atarih FROM ogrenci as o
    LEFT JOIN islem as i ON o.ogrno = i.ogrno
    LEFT JOIN kitap as k ON k.kitapno = i.kitapno
    LEFT JOIN yazar as y ON k.yazarno = y.yazarno
    LEFT JOIN tur as t ON t.turno = k.turno

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

    SELECT o.ograd, o.ogrsoyad, count(i.islemno) FROM ogrenci as o
    JOIN islem as i ON o.ogrno = i.ogrno
    JOIN kitap as k ON k.kitapno = i.kitapno
    WHERE o.sinif IN('10A', '10B')
    GROUP BY o.ograd, o.ogrsoyad


    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG
    SELECT avg(sayfasayisi) FROM kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

    SELECT * FROM kitap WHERE sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    SELECT COUNT(ogrno) FROM ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    SELECT COUNT(ogrno) AS 'Toplam Öğrenci Sayısı' FROM ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    SELECT COUNT(distinct ograd) FROM ogrenci

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    SELECT MAX(sayfasayisi) FROM kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    SELECT kitapadi, sayfasayisi FROM kitap
    WHERE sayfasayisi = (SELECT MAX(sayfasayisi) FROM kitap)

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    SELECT MIN(sayfasayisi) FROM kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    SELECT kitapadi, sayfasayisi FROM kitap
    WHERE sayfasayisi = (SELECT MIN(sayfasayisi) FROM kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    SELECT MAX(k.sayfasayisi) FROM kitap as k
    JOIN tur as t
    ON k.turno = t. turno
    WHERE t.turadi = 'Dram'

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    SELECT SUM(k.sayfasayisi) FROM kitap as k
    JOIN islem as i
    ON i.kitapno = k.kitapno
    WHERE i.ogrno = 15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    SELECT ograd, COUNT(ograd) FROM ogrenci
    GROUP BY ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.

    SELECT sinif, COUNT(ogrno) FROM ogrenci
    GROUP BY sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

    SELECT sinif, cinsiyet, COUNT(cinsiyet) FROM ogrenci
    GROUP BY sinif, cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    SELECT o.ograd, o.ogrsoyad, SUM(k.sayfasayisi) FROM ogrenci as o
    JOIN islem as i ON o.ogrno = i.ogrno
    JOIN kitap as k ON k.kitapno = i.kitapno
    GROUP BY o.ograd, o.ogrsoyad
    ORDER BY SUM(k.sayfasayisi) DESC

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

    SELECT o.ograd, o.ogrsoyad, COUNT(i.kitapno) FROM ogrenci as o
    JOIN islem as i
    ON i.ogrno = o.ogrno
    GROUP BY o.ograd, o.ogrsoyad
