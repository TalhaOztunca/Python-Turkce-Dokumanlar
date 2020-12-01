.. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Sınıflar ve Standart Fonksiyonlar/ Classes and Built-in Functions
**************************

Evet geçen paylaşımda operatörlerin kendi oluşturduğumuz sınıflarla kullanımını işlemiştik. Bu dökümanımızda pythonda hali hazırda gelen fonksiyonların(´len´, ´abs´ gibi) kendi sınıflarımızla beraber nasıl kullanabiliriz onu göreceğiz. Bir önceki seferde oluşturduğumuz vektör kalsörüne abs fonksiyonu ekleyelim. Bu fonksiyonu eklemeden önce abs'nin ne olduğu ve açılımı ayrıca vektörlerin abs'sinin nasıl bir fonksiyon olabileceğini açıklayalım. abs absolute kelimesinin kısaltması ve mutlak değer anlamına geliyor. Mutlak değer de birşeyin sıfıra olan uzaklığı diyebiliriz, 3 boyutlu vektörler için de birbirine dik olan doğrulardan oluştuğunu düşünsek öklid formülünü kullanarak bulabiliriz.::

    class Vektor:
        def __init__(self, x, y, z):
            self.x = x
            self.y = y
            self.z = z
            
        def __add__(self, iv): # iv -> ikinci vektör
            assert isinstance(iv, Vektor), "Sadece vektörlerle vektörler toplanabilir!!"
            return Vektor(self.x+iv.x, self.y+iv.y, self.z+iv.z)
        
        def __sub__(self, iv): # iv -> ikinci vektör
            assert isinstance(iv, Vektor), "Bir vektörden sadece başka bir vektör çıkarılabilir!!"
            return Vektor(self.x-iv.x, self.y+iv.y, self.z-iv.z)
            
        def __mul__(self, sayi):
            assert isinstance(iv, Vektor), "Bir vektörle sadece tam sayıyı çarpabilirsiniz!!"
            return Vektor(self.x*sayi, self.y*sayi, self.z*sayi)
        
        def __rmul__(self, sayi):
            assert isinstance(iv, Vektor), "Bir vektörle sadece tam sayıyı çarpabilirsiniz!!"
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

Burada abs fonksiyonun yaptığı iş mutlak değerini yani sıfıra olan uzaklığını bulmak. Bir vektörün sıfıra olan uzaklığını da l^2 = a^2 + b^2 + c^2 denklemini kullanarak bulabiliriz. 

Bunun yanında biz bu classımızı string ve bool gibi değerlere nasıl dönüşeceğini belirten fonskiyonlar da yazabiliriz::

    class Vektor:
        def __init__(self, x, y, z):
            self.x = x
            self.y = y
            self.z = z
            
        def __add__(self, iv): # iv -> ikinci vektör
            assert isinstance(iv, Vektor), "Sadece vektörlerle vektörler toplanabilir!!"
            return Vektor(self.x+iv.x, self.y+iv.y, self.z+iv.z)
        
        def __sub__(self, iv): # iv -> ikinci vektör
            assert isinstance(iv, Vektor), "Bir vektörden sadece başka bir vektör çıkarılabilir!!"
            return Vektor(self.x-iv.x, self.y+iv.y, self.z-iv.z)
            
        def __mul__(self, sayi):
            assert isinstance(iv, Vektor), "Bir vektörle sadece tam sayıyı çarpabilirsiniz!!"
            return Vektor(self.x*sayi, self.y*sayi, self.z*sayi)
        
        def __rmul__(self, sayi):
            assert isinstance(iv, Vektor), "Bir vektörle sadece tam sayıyı çarpabilirsiniz!!"
            return Vektor(self.x*sayi, self.y*sayi, self.z*sayi)
            
        def __neg__(self):
            return Vektor(-self.x, -self.y, -self.z)
            
        def __abs__(self):
            return (self.x ** 2 + self.y ** 2 + self.z ** 2) ** 0.5
            
        def __str__(self):
            return f"<{self.x}, {self.y}, {self.z}>"
              
        def __bool__(self):
            return self.x>0 or self.y>0 or self.z>0
            
            
    v1 = Vektor(3, 5, 6)
    print(v1) # <3,5,6> yazdırırken kendisi otomatik string halini aldı
    if v1:
        print("Ben sıfır vektörü değilim") # yazdırıldı

Stringe dönüşmesini matematiksel gösterimlerinden biri olan "<a, b, c>" haline çevirdik ve sıfır vektörü False kalanlar True olacak şeklinde tanımladık.

Her ne kadar bu fonksiyonları mantığına göre tanımlamak zorunda olmasanızda benzer mantıkla tanımlamanız veya başka bir ad vermeniz tavsiye edilir.
            
Evet konsepti genel olarak anlatıldığından değiştirilme ihtimali olan standart fonksiyonları ve çalışması için gereken hallerini liste halinde veriyorum.::

    Bunun altındakiler o sınıftan temel veri tiplerini oluşturmak üzere
    ve mutlaka o sınıftan bir üye döndürmeli!
    __int__(self):      - > int
    __str__(self):      - > str
    __float__(self):    - > float
    __bool__(self):     - > bool
    __complex__(self):  - > complex
    
    Mutlak değer anlamına gelmektedir. 
    __abs__(self): - > herhangi birşey
    
    Divmod biricinin ikinciye (tam bölen, kalan) olarak gönderen bir fonksiyondur. Rdivmod ise 
    sağdan sola tanımlı.
    __divmod__(self, ip): - > herhangi birşey
    
    Repr daha belirsiz bir şekilde kendinisini tanıtması.
    __repr__(self): - > str
    
    Len fonksiyonu uzunluk veya eleman sayısıyla alakalıdır ve integer döndürmesi şart.
    __len__(self): - > int
    
    Reversed fonksiyonu ters çevirilmiş hali demek.
    __reversed__(self): - > herhangi bir şey
    
    Round da yuvarlama anlamında ikincisini standart bir şeye eşitleyebilirsiniz.
    __round__(self, ip = ...): - > herhangi bir şey
    
    
NOT: Çoğunu kendim dir komutuyla arayarak ve classlar üzerinde deneyerek buldum eksiklikler veya yanlış kullanımlar olma ihtimali var. Eğer bulursanız lütfen düzeltin.
