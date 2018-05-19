.. meta::
   :description: With
   :keywords: with

.. highlight:: py3

**************************
Sınıflar ve With/ Classes and With
**************************

Bugünkü konumuzda muhtemelen dosya işlemlerinden aşina olduğunuz bir deyimden bahsedeceğiz başlıktan da anlaşılacağı üzere with deyimi. Mesela bir dosya oluşturup içine bişeyler yazdıralım.::

    with open("merhaba.txt", "w") as f:
        f.write("Merhaba with deyimi.")

Peki bunu with deyimiyle kullanmak bize ne sağladı? Bunu withle yazınca kesin olarak biliyoruz ki açılan bu dosya kapatılacak. Bir nevi try except blokları kullanmak gibi peki nerede işimize yarayabilir düşünelim. Mesela bir program yazıyoruz ve belirli kayıtları bir dosyaya kaydedicek eğer bir hata olursa eski verileri kaybetmemek için dosyaya kaydetmeliyiz. Try except ile yapmak mümkün fakat hem daha karmaşık gözükecek hem de birçok yerde kullanabiliriz bunun için with gibi çok daha estetik bir ifade kullanmak çok daha uygun olacaktır. Bunu nasıl mı yaparız daha önce operatörleri ve standart fonksiyonları kullanırken yapdığımız gibi __ ile başlayıp __ ile biten iki fonksiyonu eklemek. ::

    class defter:
        def __init__(self):
            self.ogrenci_bilgileri = [] # ("Ali", 110251105, 3.46), ...
        
        def ogrenci_ekle(self):
            ad = input("adı girin: ")
            numara = input("okul numarasını girin: ")
            assert numara.isdecimal(), "Numara tam sayılardan oluşmak zorunda!!"
            numara = int(numara)
            ortalama = input("ortalamayı girin: ")
            assert ortalama.replace('.','',1).isdigit(), "Ortalama ondalıklı sayı olmalı!!"
            ortalama = float(ortalama)
            self.ogrenci_bilgileri.append((ad, numara, ortalama))
            
        def __enter__(self):
            return self
            
        def __exit__(self, type, value, traceback):
            with open("bilgiler.txt", "w") as f:
                for i in self.ogrenci_bilgileri:
                    f.write("Adı: {}, Numarası: {}, Ortalaması: {}\n".format(i[0], i[1], i[2]))
                    
    with defter() as d:
        tekrar = int(input("Kaç öğrencinin bilgisi girilecek?: "))
        for _ in range(tekrar):
            d.ogrenci_ekle()

Burda with deyiminden hemen sonra __enter__ ardından __exit__ çalışıyor. Bu koda örnek olarak çalıştıralım 10 öğrenci bilgisi girileceğini söyliyelim fakat 4. öğrencide bir hata olmasını sağlayalım.::

    #### örnek
    Kaç öğrencinin bilgisi girilecek?: 10
    adı girin: Ahmet Talha Öztunca
    okul numarasını girin: 110510125
    ortalamayı girin: 3.75
    adı girin: Ali
    okul numarasını girin: 110251105
    ortalamayı girin: 3.46
    adı girin: Hüseyin
    okul numarasını girin: 110343118
    ortalamayı girin: 2.80
    adı girin: Murat
    okul numarasını girin: 11045m
    Traceback (most recent call last):
      File "test.py", line 34, in <module>
        d.ogrenci_ekle()
      File "test.py", line 16, in ogrenci_ekle
        assert numara.isdecimal(), "Numara tam sayılardan oluşmak zorunda!!"
    AssertionError: Numara tam sayılardan oluşmak zorunda!!
    #### dosyamızın içi
    Adı: Ahmet Talha Öztunca, Numarası: 110510125, Ortalaması: 3.75
    Adı: Ali, Numarası: 110251105, Ortalaması: 3.46
    Adı: Hüseyin, Numarası: 110343118, Ortalaması: 2.8

Görüldüğü üzere hata olduğu halde dosyamıza o noktaya kadar gelinen yerler yazılmış. Peki ya bunu with olmadan yapsaydık yani kod şöyle olsa.::

    class defter:
        def __init__(self):
            self.ogrenci_bilgileri = [] # ("Ali", 110251105, 3.46), ...
        
        def ogrenci_ekle(self):
            ad = input("adı girin: ")
            numara = input("okul numarasını girin: ")
            assert numara.isdecimal(), "Numara tam sayılardan oluşmak zorunda!!"
            numara = int(numara)
            ortalama = input("ortalamayı girin: ")
            assert ortalama.replace('.','',1).isdigit(), "Ortalama ondalıklı sayı olmalı!!"
            ortalama = float(ortalama)
            self.ogrenci_bilgileri.append((ad, numara, ortalama))
            
        def __enter__(self):
            return self
            
        def __exit__(self, type, value, traceback):
            with open("bilgiler.txt", "w") as f:
                for i in self.ogrenci_bilgileri:
                    f.write("Adı: {}, Numarası: {}, Ortalaması: {}\n".format(i[0], i[1], i[2]))
                    
    tekrar = int(input("Kaç öğrencinin bilgisi girilecek?: "))
    d = defter()
    for _ in range(tekrar):
        d.ogrenci_ekle()
    with open("bilgiler.txt", "w") as f:
        for i in d.ogrenci_bilgileri:
            f.write("Adı: {}, Numarası: {}, Ortalaması: {}\n".format(i[0], i[1], i[2]))
            
10 kez doğru bilgiler girerseniz çalışacaktır ama 4. öğrencinin bilgileri hatalı ise o zaman 3 öğrencinin bilgileri de kaybolmuş olacaktır.
