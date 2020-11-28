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

Iter fonksiyonu yardımıyla iterable(ilerde açıklandı) bir objeden verilerin üstünde dolaşmamızı sağlayan iterator oluşturuyoruz::
    
    i = next(liste1in_iteratoru)
    
Next fonksiyonu bize o iteratorün üstünde dolaştığı sonraki elemanı verir. Eğer döndürebileceği sonraki bir eleman yoksa StopIteration hatasını yollar::

    try:
        i = next(liste1in_iteratoru)
    except StopIteration:
        break

Burada iteratorün sonraki elemanını i'ye eşitlemeye çalıştık. Eğer bir elemen'ı varsa başarılı bir şekilde çalışmaya devam etti. 
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
    
    
## Iterable Nedir
Basitçe \_\_iter\_\_(self) ve \_\_next\_\_(self) fonksiyonları tanımlı objeler diyebiliriz. 
Python'ın kendiyle beraber gelen bir çok sınıf için hali hazırda nasıl çalıştığı tanımlanmış olup 
biz de kendi sınıflarımız için tanımlar yapabiliriz.

Örneğin bir öğrenci topluluğumuz olsun ve bu sınıfta not ortamaları ve isimleriyle öğrenciler olsun. Biz belirli bir notun üstündeki öğrencileri tebrik edelim::

    from random import random as rnd
    from random import choice as rndc
    class Ogrenci:
        def __init__(self, isim, gpa):
            self.isim = isim
            self.gpa = gpa
            
    class Topluluk:
        class IterableGpaLimit: # Sınıftaki öğrenciler üstünde dolaşmak için kullanacağımız obje tanımlaması
            def __init__(self, sinif, gpaLimit): # iteratoru sınıf ve gpa limiti ile ilklendiriyoruz
                self.sinif = sinif
                self.gpaLimit = gpaLimit
                
            def __iter__(self): # iterator çağrıldığında yapılacaklar
                self.index = 0 # ilk elemandan kontrole başlayacağımız için indexi 0 yapıyoruz
                return self
            
            def __next__(self):
                # sınıfta eleman kaldıysa ve o kişi istediğimiz gpa'in altında oldukça sonraki kişiye geç
                while self.index < len(self.sinif) and self.sinif[self.index].gpa < self.gpaLimit:
                    self.index += 1 
                    
                if self.index >= len(self.sinif): # eğer kimse kalmadıysa
                    raise StopIteration # StopIteration hatası gönder
                    
                ogrenci = self.sinif[self.index] # öğrenciyi al
                self.index += 1 # sonraki sefer için indexi artır
                return ogrenci # öğrenciyi geri döndür
        
        def __init__(self, ogrenciler=None):
            try:
                self.sinif = [i for i in ogrenciler] # eğer öğrenciler iterable ise onlardan sınıfı oluştur
            except TypeError: # iterable değil ise
                self.sinif = [] # boş bir sınıf oluştur
        
        def addOgrenci(self, ogrenci):
            self.sinif.append(ogrenci)
            
        def gpasiXdenBuyukOgrenciler(self, x):
            return self.IterableGpaLimit(self.sinif, x)
            
    isimler = ["Ahmet", "Talha", "Hasan", "Mustafa", "Harun", "Oktay", "Erdem", "Cansu", "Hilal", "Gökçe", "Semra", "Ahu"]
    # rastgele isim ve 0~4 aralığında gpa'yi olan 7 öğrenci oluştur
    rastgeleOgrenciler = [Ogrenci(rndc(isimler), 4*rnd()) for _ in range(7)]
    topluluk = Topluluk(rastgeleOgrenciler) # rastgele öğrencilerle Topluluk oluştur

    print("Aşşağıdaki öğrencileri başarısından dolayı tebrik ederim")
    for i in topluluk.gpasiXdenBuyukOgrenciler(2.8):
        print(f"{i.isim} ({i.gpa:.2f})")
        
    print("Aşşağıdakileri üstün başarısından dolayı ayrıca tebrik ederim")
    for i in topluluk.gpasiXdenBuyukOgrenciler(3.5):
        print(f"{i.isim} ({i.gpa:.2f})")
        
    """ output::
    Aşşağıdaki öğrencileri başarısından dolayı tebrik ederim
    Gökçe (3.59)
    Oktay (3.14)
    Harun (3.18)
    Cansu (3.66)
    Aşşağıdakileri üstün başarısından dolayı ayrıca tebrik ederim
    Gökçe (3.59)
    Cansu (3.66)
    """


Ya da tree'mizdeki değerleri dfs ile dolaşalım::

    class Tree:
        class dfsTree:
            def __init__(self, root, connections):
                self.r = root
                self.c = connections
                
            def __iter__(self):
                self.stack = [self.r] # stack'i ilk elemanı root olacak şekilde ayarla
                return self
            
            def __next__(self):
                if len(self.stack) == 0: # stack'te eleman kalmadıysa StopIteration hatası göndererek iteration'ı bitir
                    raise StopIteration
                it = self.stack[-1] # son elemanı al
                self.stack.pop() # son elemanı stack'ten çıkar
                self.stack.extend(self.c.get(it, [])) # stacke'e son elemanın çocuklarını ekle eğer connection oluşturlmadıysa boş liste kullan
                return it # o elemanı geri gönder
            
        def __init__(self, root):
            self.root = root
            self.childs = {root:[]}
            
        def addConnection(self, parent, child):
            if parent not in self.childs:
                self.childs[parent] = []
            self.childs[parent].append(child)
            
        def dfs(self):
            return self.dfsTree(self.root, self.childs)

    """
                   0
             /     |      \
             1     5     'ali'
            / \    |      / \
           9  'k' (0,3)  2   'm'     
    """        
            
    tree = Tree(0)
    tree.addConnection(0, 'ali')
    tree.addConnection(0, 5)
    tree.addConnection(0, 1)
    tree.addConnection('ali', 'm')
    tree.addConnection('ali', 2)
    tree.addConnection(5, (0,3))
    tree.addConnection(1, 'k')
    tree.addConnection(1, 9)

    for i in tree.dfs():
        print(i)
    """
    0
    1
    9
    k
    5
    (0, 3)
    ali
    2
    m
    """
