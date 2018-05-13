.. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Iteratorlar Nedir?/ What are Iterators?
**************************

Iterable nedir. Aslında ne olduğunu bilmeseniz bile iteratorleri pek çok defa kullandınız. En basit olarak for i in x: ifadesinde x yerine yazdığınızda hata almadığınız her şey iterable'dır yani o nesneden iterator oluşturulabilir. Daha net kavramak için bir for döngüsünde geçen olayları inceliyelim.::

    liste1 = [3, 8, 11, 6, 41]
    toplam = 0
    for i in liste1:
        toplam += i
    
Burada elimizdeki listenin sayılarını toplayan bir döngü yazdık bunun while karşılığının nasıl bir şey olduğuna bakalım::
    
    liste1 = [3, 8, 11, 6, 41]
    toplam = 0
    
    liste1in_iteratoru = iter(liste)
    while True:
        try:
            i = next(liste1in_iteratoru)
            toplam += i
        except StopIteration:
            break

Şu noktada anlamamak çok normal adım adım olanları açıklayalım. ilk başta iter fonksiyonu::
    
    liste1in_iteratoru = iter(liste)
iter fonsiyon iterable(yani üstünde dönülebilir diyebiliriz sanırım) nesnelerin iteratorunu oluşturmayı sağlar.::

    try:
    expect:
Blokları eğer bir hata oluşursa ki aslında burda beklenen hata son elemandan sonra gelen bir eleman olmaması döngüden çıkılıyor.::
    
    next(liste1in_iteratoru)
bu da sonraki elemanı almak için.



Biraz daha açıklayıcı olması için adım adım liste1 üzerinde anlatıyım.::

    liste1 = [3, 8, 11, 6, 41]
    toplam = 0
    
    liste1in_iteratoru = iter(liste) # listeden iterator oluşturduk.
    
    i = next(liste1in_iteratoru) # i'nin değerine liste1'in ilk elemanı geçti
    toplam += i
    
    i = next(liste1in_iteratoru) # i'nin yerine liste1'in ikinci elemanı geçti
    toplam += i
    
    i = next(liste1in_iteratoru) # i'nin yerine liste1'in üçüncü elemanı geçti
    toplam += i
    
    i = next(liste1in_iteratoru) # i'nin yerine liste1'in dördüncü elemanı geçti
    toplam += i
    
    i = next(liste1in_iteratoru) # i'nin yerine liste1'in beşinci elemanı geçti
    toplam += i
    
    i = next(liste1in_iteratoru) # eğer bunu yazarsanız hata alırsınız çünki 5. elemandan sonra gelen bir eleman yok!! try exceptler tam olarak bunun için


Bundan sonra benimde geçmişte yaptığım hatalardan birini size gösteriyim::

    liste = [1, 2, 8, 13, 9]
    for i in range(len(liste)):
        if liste[i] == 2:
            liste[i] = 5

Bu bayağı hatalı bir kullanım çünki her seferinde listeyi o elemana kadar devamlı tarıyor ve n adımda yapılacak olay n*n/2 adımda yapılıyor. Onun yerine::
    liste = [1, 2, 8, 13, 9]
    for sira, i in enumerate(liste):
        if i == 2:
            liste[sira] = 5
Bunu kullanmanız çok daha sağlıklı.


!!! Önemli not. Bu ders sadece Iteratorlar'ın ne olduğuna giriş tarzındadır. Sonraki derste generatorleri kullanarak ,nesne tabanlı konulara geçildiğinde iterable nesneler oluşturarak ve modülleri inceleme kısmında da iterator modülünü kullanarak daha yararlı şeyler yapılacaktır. Sadece ön hazırlık ve bakış açınızı genişletmek niteliğindedir.
