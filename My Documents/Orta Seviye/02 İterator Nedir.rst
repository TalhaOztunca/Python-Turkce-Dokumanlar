.. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Iteratorlar Nedir?/ What are Iterators?
**************************

Iteratorler içinde birden fazla elemana sahip olan veya üreten objelerin bu verilerine ulaşmak için kullandığımız bir yapıdır.

Farkında olmadan bu yapıyı hepimiz kullanıyoruz. Örneğin for döngüsü yardımıyla bir listenin elemanlarına ulaşırken::

    liste1 = [3, 8, 11, 6, 41]
    toplam = 0
    for i in liste1:
        toplam += i
    
Yukarıdaki kod aslında aşşağıdakiyle aynı şeyi ifade ediyor::
    
    liste1 = [3, 8, 11, 6, 41]
    toplam = 0
    
    liste1in_iteratoru = iter(liste)
    while True:
        try:
            i = next(liste1in_iteratoru)
            toplam += i
        except StopIteration:
            break

Yapıyı anlamak için adım adım kodların ne yaptığını inceleyelim::

    liste1in_iteratoru = iter(liste)

Iter fonksiyonu yardımıyla iterable([ilerde açıklandı]()) bir objeden verilerin üstünde dolaşmamızı sağlayan iterator oluşturuyoruz::
    
    i = next(liste1in_iteratoru)
    
Next fonksiyonu bize o iteratorün üstünde dolaştığı sonraki elemanı verir. Eğer döndürebileceği sonraki bir eleman yoksa StopIteration hatasını yollar::

    try:
        i = next(liste1in_iteratoru)
    except StopIteration:
        break

Burada iteratorün sonraki elemanını i'ye eşitlemeye çalıştık. Eğer bir elemenaı varsa başarılı bir şekilde çalışmaya devam etti. 
Fakat içinde eleman kalmadıysa StopIteration hatası verdi ve exceptionın içine girerek break komutunu çağırdı::

    liste1in_iteratoru = iter(liste)
    while True:
        try:
            i = next(liste1in_iteratoru)
            toplam += i
        except StopIteration:
            break
            
Sonuç olarak kodu şu şekilde özetleyebiliriz. liste'nin üstünde dolaşan bir iterator yap. 
Döngüden çıkılmadığı sürece iteratorın sonraki elemanını i'ye eşitle.
Yapmak istediğin adımları uygula. toplam'ı i kadar arttırmak gibi.
Eğer iteratorun sonraki elemanı kalmadıysa döngüyü bitir.
    
