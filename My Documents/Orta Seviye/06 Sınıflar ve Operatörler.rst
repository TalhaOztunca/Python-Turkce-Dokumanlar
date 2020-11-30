 .. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Sınıflar ve Operatörler/ Classes and Operators
**************************

Sınıflarla alakalı konular birkaç başlıktan oluşacak ilk olarak kendi oluşturduğumuz sınıflarla beraber operatörlerimizi nasıl kullanacağımıza göz atalım. Mesela ilk olarak 3 boyutlu vektör sınıfı oluşturalım.::

    class Vektor:
        def __init__(self, x, y, z):
            self.x = x
            self.y = y
            self.z = z
            
        def topla(self, iv): # iv -> ikinci vektör
            return Vektor(self.x+iv.x, self.y+iv.y, self.z+iv.z)
            
        def boyutlarını_soyle(self):
            print("boyutlarım: {}, {}, {}.".format(self.x, self.y, self.z))
    
    vek1 = Vektor(3, 4, 5)
    vek2 = Vektor(6, 2, 1)
    vek3 = vek1.topla(vek2)
    vek3.boyutlarını_soyle()
    
    ### çıktı ###
    boyutlarım: 9, 6, 6.

Bir de ´__add__´ kullanarak fonksiyon adı yerine '+' operatörünü kullanılmış haline bakalım ::

    class Vektor:
        def __init__(self, x, y, z):
            self.x = x
            self.y = y
            self.z = z
            
        def __add__(self, iv): # iv -> ikinci vektör
            return Vektor(self.x+iv.x, self.y+iv.y, self.z+iv.z)
            
        def boyutlarını_soyle(self):
            print("boyutlarım: {}, {}, {}.".format(self.x, self.y, self.z))
    
    vek1 = Vektor(3, 4, 5)
    vek2 = Vektor(6, 2, 1)
    vek3 = vek1 + vek2
    vek3.boyutlarını_soyle()
    
    ### çıktı ###
    boyutlarım: 9, 6, 6.

Aynı işi yapıyor ama operator kullanmak daha okunaklı hale getiriyor. Birkaç fonkisyon daha eklersek::

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
            
        def __neg__(self):
            return Vektor(-self.x, -self.y, -self.z)
            
        def boyutlarını_soyle(self):
            print("boyutlarım: {}, {}, {}.".format(self.x, self.y, self.z))
    
    vek1 = Vektor(3, 6, 2)
    vek2 = Vektor(1, 4, 9)
    vek3 = -vek1 + vek2 * 3
    vek3.boyutlarını_soyle()
    
    ###çıktı###
    boyutlarım: 0, 6, 25.

Yazmak ve kodun nasıl çalıştığını incelemek kolaylaştı. Operatör kullanmasaydık vek3 = vek1.negatif().topla(vek2.carp(3)) tarzında bir şey yazmamız gerekiyordu. 

Dikkat edilmesi gereken noktalardan biri çarpma işlemi eğer vek2 * 3 yerine 3 * vek2 yazarsanız çalışmayacaktır. Çünkü ´__mul__´ soldan sağa doğru çalışacak şekilde tanımlı. Int sınıfı da bizim tanımladığımız vektor sınıfıyla çalışacak şekilde tanımlanmadığından hata alacağız. Bunu aşmak için sağdan sola tanımlı ´__rmul__´ fonksiyonunu kullanabiliriz.::

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
            
        def boyutlarını_soyle(self):
            print("boyutlarım: {}, {}, {}.".format(self.x, self.y, self.z))
    
    vek1 = Vektor(3, 6, 2)
    vek2 = Vektor(1, 4, 9)
    vek3 = -vek1 + 3 * vek2
    vek3.boyutlarını_soyle()

Konsepti açıklandığına göre sadece fonksiyon isimlerini ve işaretlerini veriyorum.::
    
    # Soldan sağa işleyenler                       #Operator
    __add__(self, other)                               +
    __sub__(self, other)                               -
    __mul__(self, other)                               *
    __matmul__(self, other)                            @
    __truediv__(self, other)                           /
    __floordiv__(self, other)                          //
    __mod__(self, other)                               %
    __pow__(self, other[, modulo])                     **
    __lshift__(self, other)                            <<
    __rshift__(self, other)                            >>
    __and__(self, other)                               &
    __xor__(self, other)                               ^
    __or__(self, other)                                |
    
    # Sağdan sola işleyenler                       #Operator
    __radd__(self, other)                              +
    __rsub__(self, other)                              -
    __rmul__(self, other)                              *
    __rmatmul__(self, other)                           @
    __rtruediv__(self, other)                          /
    __rfloordiv__(self, other)                         //
    __rmod__(self, other)                              %
    __rpow__(self, other)                              **
    __rlshift__(self, other)                           <<
    __rrshift__(self, other)                           >>
    __rand__(self, other)                              &
    __rxor__(self, other)                              ^
    __ror__(self, other)                               |
    
    # Ön ek olarak gelenler                       #Operator
    __neg__(self)                                      -
    __pos__(self)                                      +
    __invert__(self)                                   ~
