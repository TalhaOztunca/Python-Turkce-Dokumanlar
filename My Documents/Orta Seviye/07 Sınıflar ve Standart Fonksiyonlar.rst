.. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Sınıflar ve Standart Fonksiyonlar/ Classes and Built-in Functions
**************************

Evet geçen paylaşımda operatörlerin kendi oluşturduğumuz sınıflarla kullanımını işlemiştik bu dökümanımızda pythonda hali hazırda gelen fonksiyonların kendi sınıflarla beraber nasıl kullanabiliriz onu görüceğiz. Hali hazırda gelen fonksiyonlardan kastımız len() abs() gibi fonksiyonlar. Gelin bir önceki seferde oluşturduğumuz vektör kalsörüne abs fonksiyonu ekleyelim. Bu fonksiyonu eklemeden önce abs'nin ne olduğu ve açılımı ayrıca vektörlerin abs'sinin nasıl bir fonksiyon olabileceğini açıklayalım. abs absolute kelimesinin kısaltması ve mutlak değer anlamına geliyor. Mutlak değer de birşeyin sıfıra olan uzaklığı diyebiliriz, 3 boyutlu vektörler için de birbirine dik olan doğrulardan oluştuğunu düşünsek öklid formülünü kullanarak bulabiliriz.::

    class Vektor:
        def __init__(self, x, y, z):
            self.x = x
            self.y = y
            self.z = z
            
        def __add__(self, iv): # iv -> ikinci vektör
            assert type(iv) is Vektor, "Sadece vektörlerle vektörler toplanabilir!!"
            return Vektor(self.x+iv.x, self.y+iv.y, self.z+iv.z)
        
        def __sub__(self, iv): # iv -> ikinci vektör
            assert type(iv) is Vektor, "Bir vektörden sadece başka bir vektör çıkarılabilir!!"
            return Vektor(self.x-iv.x, self.y+iv.y, self.z-iv.z)
            
        def __mul__(self, sayi):
            assert type(sayi) is int, "Bir vektörle sadece tam sayıyı çarpabilirsiniz!!"
            return Vektor(self.x*sayi, self.y*sayi, self.z*sayi)
        
        def __rmul__(self, sayi):
            assert type(sayi) is int, "Bir vektörle sadece tam sayıyı çarpabilirsiniz!!"
            return Vektor(self.x*sayi, self.y*sayi, self.z*sayi)
            
        def __neg__(self):
            return Vektor(-self.x, -self.y, -self.z)
            
        def __abs__(self):
            return (self.x ** 2 + self.y ** 2 + self.z ** 2) ** 0.5
        
    vek1 = Vektor(3, 4, 12)
    vek1_mutlak_degeri = abs(vek1)
    print(vek1_mutlak_degeri)
    
    ###sonuç
    13.0
    
Gördüğünüz gibi bazı built-in fonksiyonları kendi oluşturduğumuz sınıflarla beraber de kullanabiliyoruz. Şimdi uzunluk adlı bir class oluşturalım ve bazı eklemeler yapalım.::

    class Uzunluk:
        def __init__(self, uzunluk, birim = "m"):
            assert birim in ("m", "dm", "cm", "mm"), "Uzunluğun birimi sadece m(metre), dm(desimetre), cm(santimetre) \
    veya mm(milimetre olabilir)!! ancak siz parametre olarak '{}' yolladınız!!".format(birim)
            assert type(uzunluk) is int or type(uzunluk) is float, "uzunluk sadece tam sayı veya ondalıklı sayı olabilir!! \
    ama siz parametre olarak {} yolladınız.".format(uzunluk)
            assert uzunluk >= 0, "Uzunluk - bir değer olamaz!! ancak siz {} değerini yolladınız.".format(uzunluk)
            self.uzunluk = uzunluk
            self.birim = birim
        
        def _ekogy(self, iu): # en küçük ölçekliye göre yolla ve iu = ikinci uzunluk
            """ Bu fonksiyon iki uzunluğu en küçük ölçeğe göre yollar yani birinin birimi m diğeri cm ise ikisini de cm cinsinden yollar. """
            birimler = {"m":1000, "dm":100, "cm":10, "mm":1}
            isaret1 = birimler[self.birim]
            isaret2 = birimler[iu.birim]
            if isaret1 == isaret2:
                return self, iu
            elif isaret1 < isaret2:
                return self, Uzunluk(iu.uzunluk * (isaret2 / isaret1), self.birim)
            elif isaret1 > isaret2:
                return Uzunluk(self.uzunluk * (isaret1 / isaret2), iu.birim), iu
        
        def __add__(self, iu): # iu = ikinci uzunluk
            """ + operatörü iki uzunluğu toplamak için kullanılır. Dönecek uzunluk iki uzunlukdan en küçük ölçeklinin birimindendir."""
            assert type(iu) is Uzunluk, "Sadece bir uzunluk başka bir uzunlukla toplanılabilir!! Fakat yollanan parametre {}".format(type(iu))
            uzunluk1, uzunluk2 = self._ekogy(iu)
            return Uzunluk(uzunluk1.uzunluk + uzunluk2.uzunluk, uzunluk1.birim)
            
        def __sub__(self, iu): # iu = ikinci uzunluk
            """ - operatörü bir uzunluğu diğerinden çıkarmak için kullanılır. Dönecek uzunluk iki uzunlukdan en küçük ölçeklinin birimindendir."""
            assert type(iu) is Uzunluk, "Sadece bir uzunluk başka bir uzunluktan çıkarılabilir!! Fakat yollanan parametre {}".format(type(iu))
            uzunluk1, uzunluk2 = self._ekogy(iu)
            assert uzunluk1.uzunluk > uzunluk2.uzunluk, "Uzunluk negatif olamayacağından çıkartılacak ikinci uzunluk kısa olmamalı!! \
            {0} {2} - {1} {2}".format(uzunluk1.uzunluk, uzunluk2.uzunluk, uzunluk1.birim)
            return Uzunluk(uzunluk1.uzunluk - uzunluk2.uzunluk, uzunluk1.birim)
            
        def __bool__(self):
            """ bool(nesne) fonksiyonu if deyiminde kullanıldığında True veya False vermesini sağlar. 
            Burda uzunluk 0 ise False vermesini aksi halde True vermesini istiyoruz."""
            return self.uzunluk != 0
            
Evet şu an oluşturduğumuz uzunluk sınıfında bir önceki dersten operatörleri ve ondan önceki dersten olan asserti görüyorsunuz eğer onlarla anlamadığınız bir nokta varsa önceki konuları inceleyebilirsiniz. Uzunluk adlı klasımızın m dm cm mm harici bir birim alamayacığını ve sıfırdan küçük olamayacağını varsaydık. Ayrıca + ve - operetörleri oluşturduk burda yeni olarak gözünüze çarpacak şey __bool__ fonksiyonu olmalı. __bool__ fonksiyonu standart fonksiyonlardan biri olan bool() fonksiyonu için kullanılır ayrıca if deyimiyle beraber kullanınca True veya False döndürecektir. Hadi birtane de dikdörtgen adlı bir sınıf oluşturalım, bu arada uzunluk sınıfının hali hazırda üstteki haliyle yazıldığını varsayıyorum.::

    class Dikdortgen:
        def __init__(self, genislik, yukseklik):
            assert type(genislik) is Uzunluk and type(yukseklik) is Uzunluk, "genişlik ve yükseklik değerleri uzunluk cinsinden olmalı!! \
    onun yerine genişlik : {}, uzunluk : {}".format(type(genislik), type(yukseklik))
            assert genislik, "genişlik 0 olamaz uzunluk geçerli değil!!"
            assert yukseklik, "yükseklik 0 olamaz uzunluk geçerli değil!!"
            self.genislik = genislik
            self.yukseklik = yukseklik

        def __reversed__(self):
            """Bu fonksiyon diktörtgeni çevirir yani genişlik uzunluk olur uzunluk da genişlik olur."""
            return Dikdortgen(self.yukseklik, self.genislik)

Evet ikinci sınıfımız dikdörtgen. Dikdörtgen iki parametre alıyor ve bunların uzunluk olmasını bekliyor. Aynı zamanda uzunlukları True olup olmadığını kontrol ediyor bu örnek için 0 olmaması. reversed() fonksiyonuyla kullanılıncada genişlikle yüksekliği yer değiştiriyor. Biraz daha eğlenceli şeyler yapalım mesela 8 sayısını stringe dönüştürdüğümüzde "8" oluyor acaba bunu dikdörtgen sınıfımız içinde yapabilirmiyiz?::

    class Dikdortgen:
        def __init__(self, genislik, yukseklik):
            assert type(genislik) is Uzunluk and type(yukseklik) is Uzunluk, "genişlik ve yükseklik değerleri uzunluk cinsinden olmalı!! \
    onun yerine genişlik : {}, uzunluk : {}".format(type(genislik), type(yukseklik))
            assert genislik, "genişlik 0 olamaz uzunluk geçerli değil!!"
            assert yukseklik, "yükseklik 0 olamaz uzunluk geçerli değil!!"
            self.genislik = genislik
            self.yukseklik = yukseklik
        
        def __reversed__(self):
            """Bu fonksiyon diktörtgeni çevirir yani genişlik uzunluk olur uzunluk da genişlik olur."""
            return Dikdortgen(self.yukseklik, self.genislik)

        def __str__(self):
            """Bu fonksiyon dikdörtgeni string olarak gösterir."""
            sonuc = """
                  {} {}
            ****************
            *              *
            *              * {} {}
            *              *
            ****************
            """.format(self.genislik.uzunluk, self.genislik.birim, self.yukseklik.uzunluk, self.yukseklik.birim)
            return sonuc
    
    uz1 = Uzunluk(3)
    uz2 = Uzunluk(266, "cm")
    di1 = Dikdortgen(uz1, uz2)
    print(str(di1))
    
    ### sonuc
    
                  3 m
            ****************
            *              *
            *              * 266 cm
            *              *
            ****************

Gördüğünüz gibi burda string'e dönüştürmeye yarayan bir fonksiyon oluşturduk str, dict, int gibi fonksiyonları kullanırken kesin dikkat etmemiz gereken nokta şudur ki str ise kesin olarak str bir değer döndürüyor olmalı. Evet şimdi uzunluk fonksiyonumuza int'e çeviren bir fonksiyon ekleyelim ki bu fonksiyonumuz mm cinsine çevirip uzunluğunu yollasın ve de ardından dikdörtgen sınıfımıza len fonksiyonu ekleyelim ki bu bize dikdörtgenin çevresini döndürüyor olsun.::

    class Uzunluk:
        def __init__(self, uzunluk, birim = "m"):
            assert birim in ("m", "dm", "cm", "mm"), "Uzunluğun birimi sadece m(metre), dm(desimetre), cm(santimetre) \
    veya mm(milimetre olabilir)!! ancak siz parametre olarak '{}' yolladınız!!".format(birim)
            assert type(uzunluk) is int or type(uzunluk) is float, "uzunluk sadece tam sayı veya ondalıklı sayı olabilir!! \
    ama siz parametre olarak {} yolladınız.".format(uzunluk)
            assert uzunluk >= 0, "Uzunluk - bir değer olamaz!! ancak siz {} değerini yolladınız.".format(uzunluk)
            self.uzunluk = uzunluk
            self.birim = birim
        
        def _ekogy(self, iu): # en küçük ölçekliye göre yolla ve iu = ikinci uzunluk
            """ Bu fonksiyon iki uzunluğu en küçük ölçeğe göre yollar yani birinin birimi m diğeri cm ise ikisini de cm cinsinden yollar. """
            birimler = {"m":1000, "dm":100, "cm":10, "mm":1}
            isaret1 = birimler[self.birim]
            isaret2 = birimler[iu.birim]
            if isaret1 == isaret2:
                return self, iu
            elif isaret1 < isaret2:
                return self, Uzunluk(iu.uzunluk * (isaret2 / isaret1), self.birim)
            elif isaret1 > isaret2:
                return Uzunluk(self.uzunluk * (isaret1 / isaret2), iu.birim), iu
        
        def __add__(self, iu): # iu = ikinci uzunluk
            """ + operatörü iki uzunluğu toplamak için kullanılır. Dönecek uzunluk iki uzunlukdan en küçük ölçeklinin birimindendir."""
            assert type(iu) is Uzunluk, "Sadece bir uzunluk başka bir uzunlukla toplanılabilir!! Fakat yollanan parametre {}".format(type(iu))
            uzunluk1, uzunluk2 = self._ekogy(iu)
            return Uzunluk(uzunluk1.uzunluk + uzunluk2.uzunluk, uzunluk1.birim)
            
        def __sub__(self, iu): # iu = ikinci uzunluk
            """ - operatörü bir uzunluğu diğerinden çıkarmak için kullanılır. Dönecek uzunluk iki uzunlukdan en küçük ölçeklinin birimindendir."""
            assert type(iu) is Uzunluk, "Sadece bir uzunluk başka bir uzunluktan çıkarılabilir!! Fakat yollanan parametre {}".format(type(iu))
            uzunluk1, uzunluk2 = self._ekogy(iu)
            assert uzunluk1.uzunluk > uzunluk2.uzunluk, "Uzunluk negatif olamayacağından çıkartılacak ikinci uzunluk kısa olmamalı!! \
            {0} {2} - {1} {2}".format(uzunluk1.uzunluk, uzunluk2.uzunluk, uzunluk1.birim)
            return Uzunluk(uzunluk1.uzunluk - uzunluk2.uzunluk, uzunluk1.birim)
        
        def __int__(self):
            """ Uzunluğu önce mm cinsinden bulur ve uzunluğunu integer olarak döndürür."""
            birimler = {"m":1000, "dm":100, "cm":10, "mm":1}
            sonuc = self.uzunluk * birimler[self.birim]
            return int(sonuc)
            
        def __bool__(self):
            """ bool(nesne) fonksiyonu if deyiminde kullanıldığında True veya False vermesini sağlar. 
            Burda uzunluk 0 ise False vermesini aksi halde True vermesini istiyoruz."""
            return self.uzunluk != 0
        
    class Dikdortgen:
        def __init__(self, genislik, yukseklik):
            assert type(genislik) is Uzunluk and type(yukseklik) is Uzunluk, "genişlik ve yükseklik değerleri uzunluk cinsinden olmalı!! \
    onun yerine genişlik : {}, uzunluk : {}".format(type(genislik), type(yukseklik))
            assert genislik, "genişlik 0 olamaz uzunluk geçerli değil!!"
            assert yukseklik, "yükseklik 0 olamaz uzunluk geçerli değil!!"
            self.genislik = genislik
            self.yukseklik = yukseklik
        
        def __len__(self):
            """Bu fonksiyon dikdörtgenin kenarları toplamını int cinsinden yollar. Eğer float sayılar varsa tam olarak doğru çalışmayabilir."""
            return int(self.genislik + self.genislik + self.yukseklik + self.yukseklik)
        
        def __reversed__(self):
            """Bu fonksiyon diktörtgeni çevirir yani genişlik uzunluk olur uzunluk da genişlik olur."""
            return Dikdortgen(self.yukseklik, self.genislik)

        def __str__(self):
            """Bu fonksiyon dikdörtgeni string olarak gösterir."""
            sonuc = """
                  {} {}
            ****************
            *              *
            *              * {} {}
            *              *
            ****************
            """.format(self.genislik.uzunluk, self.genislik.birim, self.yukseklik.uzunluk, self.yukseklik.birim)
            return sonuc
            
Evet konsepti genel olarak anladığınızı düşünerek değiştirilme ihtimali olan standart fonksiyonları ve çalışması için gereken hallerini liste halinde veriyorum.::

    Bunun altındakiler o sınıftan temel veri tiplerini oluşturmak üzere
    ve mutlaka o sınıftan bir üye döndürmeli!
    __int__(self):      -> int
    __str__(self):      -> str
    __float__(self):    -> float
    __list__(self):     -> list
    __tuple__(self):    -> tuple
    __bool__(self):     -> bool
    __chr__(self):      -> chr
    __set__(self):      -> set
    __frozenset__(self) -> frozenset
    __dict__(self):     -> dict
    __complex__(self):  -> complex
    
    Mutlak değer anlamına gelmektedir. Herhangi birşey döndürebilmesine rağmen integer
    döndürmek ve mantıklı yerlerde kullanmak daha iyi olacaktır.
    __abs__(self): -> herhangi birşey
    
    Divmod tam böleni ve modunu beraber gönderen bir fonksiyondur. Rdivmod ise 
    sağdan sola doğru işleyecek.
    __divmod__(self, ip): -> herhangi birşey
    
    Pow fonksiyonu aynı zamanda standart fonksiyon olan pow içinde kullanılır.
    3. yü herhangi birşeye eşitleyerek a ** b gibi bir kullanımı da sabit tutabilirsiniz.
    rpow da tahmin edeceğiniz üzere tersten işleyeni
    __pow__(self, ip, up = ...): -> herhangi birşey
    __rpow__(self)
    
    Repr representten geliyor olmalı yani kendini tanıtmak printe parametre olarak yolladığınızda
    bastırılıcak şey ve string döndürmesi şart.
    __repr__(self): -> str
    
    Len fonksiyonu eleman sayısıyla alakalıdır ve integer döndürmesi şart.
    __len__(self): -> int
    
    Reversed fonksiyonu ters çevirilmiş hali demek.
    __reversed__(self): -> herhangi bir şey
    
    Round da yuvarlama anlamında ikincisini standart bir şeye eşitleyebilirsiniz.
    __round__(self, ip = ...): -> herhangi bir şey
    
    
NOT: Çoğunu kendim dir komutuyla arayarak ve classlar üzerinde deneyerek buldum bir çok eksiklikler olması muhtemel eğer bulursanız lütfen düzeltin.
