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

Peki bunu with deyimiyle kullanmak bize ne sağladı? Bunu withle yazınca kesin olarak biliyoruz ki açılan bu dosya kapatılacak. Bir nevi try except blokları kullanmak gibi peki nerede işimize yarayabilir düşünelim. Mesela bir program yazıyoruz ve belirli kayıtları bir dosyaya kaydedicek eğer bir hata olursa eski verileri kaybetmemek için dosyaya kaydetmeliyiz. Try except ile yapmak mümkün fakat hem daha karmaşık gözükecek hem de birçok yerde kullanabiliriz bunun için with gibi çok daha estetik bir ifade kullanmak çok daha uygun olacaktır. Operatörleri ve standart fonksiyonları kullanırken yapdığımıza benzer şekilde ´__enter__´ ve ´__exit__´ fonksiyonlarını ekleyerek sınıfımızın with deyimi ile çalışmasını sağlarız. ::

    class defter:
        def __init__(self):
            self.ogrenci_bilgileri = [] # ("Ali", 110251105, 3.46), ...
        
        def ogrenci_ekle(self):
            ad = input("adı girin: ")
            numara = int(input("okul numarasını girin: "))
            ortalama = float(input("ortalamayı girin: "))
            self.ogrenci_bilgileri.append((ad, numara, ortalama))
            
        def __enter__(self):
            return self
            
        def __exit__(self, type, value, traceback):
            with open("bilgiler.txt", "w") as f:
                for ad, numara, ortalama in self.ogrenci_bilgileri:
                    f.write("Adı: {ad}, Numarası: {numara}, Ortalaması: {ortalama}\n")
                    
    with defter() as d:
        tekrar = int(input("Kaç öğrencinin bilgisi girilecek?: "))
        for _ in range(tekrar):
            d.ogrenci_ekle()

Burda with deyiminin bloğuna girerken __enter__ çalışıp geri döndürülen değer as olarak atanıyor. Blok bittiği anda __exit__ çalışıyor. Bu kodu aşşağıdaki örnekle çalıştıralım.

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
    # burda 11045m int'e dönüştürülemediği için hata aldık ve program bitti.
    #### dosyanın içi
    Adı: Ahmet Talha Öztunca, Numarası: 110510125, Ortalaması: 3.75
    Adı: Ali, Numarası: 110251105, Ortalaması: 3.46
    Adı: Hüseyin, Numarası: 110343118, Ortalaması: 2.8

Görüldüğü üzere program yarıda kesilip hata aldığımız halde bile exit fonksiyonu çalışarak o ana kadar girilen verileri kaydetmiş oldu. Bu kullanımı with olmadan hatayı gözardı etseydik şöyle bir programımız olabilirdi.::

    class defter:
        def __init__(self):
            self.ogrenci_bilgileri = [] # ("Ali", 110251105, 3.46), ...
        
        def ogrenci_ekle(self):
            ad = input("adı girin: ")
            numara = int(input("okul numarasını girin: "))
            ortalama = float(input("ortalamayı girin: "))
            self.ogrenci_bilgileri.append((ad, numara, ortalama))
        
        def dosyaya_kaydet(self):
            with open("bilgiler.txt", "w") as f:
                for ad, numara, ortalama in self.ogrenci_bilgileri:
                    f.write("Adı: {ad}, Numarası: {numara}, Ortalaması: {ortalama}\n")
        
    tekrar = int(input("Kaç öğrencinin bilgisi girilecek?: "))
    d = defter()
    for _ in range(tekrar):
        d.ogrenci_ekle()
    d.dosyaya_kaydet()
            
10 kez hata almayacak şekilde bilgileri girerseniz çalışacaktır ama 4. öğrencinin bilgileri hatalı ise o zaman 3 öğrencinin bilgilerini de kaydetmeden kapanır.

With'in try finally karşılığı da aşşağıdaki gibidir::

    class defter:
        def __init__(self):
            self.ogrenci_bilgileri = [] # ("Ali", 110251105, 3.46), ...
        
        def ogrenci_ekle(self):
            ad = input("adı girin: ")
            numara = int(input("okul numarasını girin: "))
            ortalama = float(input("ortalamayı girin: "))
            self.ogrenci_bilgileri.append((ad, numara, ortalama))
        
        def dosyaya_kaydet(self):
            with open("bilgiler.txt", "w") as f:
                for ad, numara, ortalama in self.ogrenci_bilgileri:
                    f.write("Adı: {ad}, Numarası: {numara}, Ortalaması: {ortalama}\n")
    try:
        tekrar = int(input("Kaç öğrencinin bilgisi girilecek?: "))
        d = defter()
        for _ in range(tekrar):
            d.ogrenci_ekle()
    finally:
        d.dosyaya_kaydet()

Bu kod with ile aynı işi yapıyor fakat görsel olarak gördüğünüz üzere o kadar estetik değil. Ayrıca kodu kullanacak diğerleri için başlarken ve çıkarken nasıl davranacakları konusunda uyarmak yerine doğrudan çalışan bir with alternatifi sunuyorsunuz.
