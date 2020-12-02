 .. meta::
   :description: Nitelikler/ Properties
   :keywords: property

.. highlight:: py3

**************************
Sınıflar ve Nitelikler/ Classes and Properties
**************************

Getter ve setter metodları objelerimizin belirli niteliklerine ulaşılmak istendiğinde kullanılır. Buna neden ihtiyacımız var önce bir örnek verelim.::

    class Araba:
        def __init__(self, hiz_limiti):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabilir!"
            assert hiz_limiti > 0, " Negatif bir hız olamaz"
            self.hiz_limiti = hiz_limiti

Şimdi çalışma esnasında hız limitini değiştirecek bir olay olduğunu ve değiştirmemiz gerektiğini düşünelim. Aşşağıdak bir yol izleyebiliriz.::

    class Araba:
        def __init__(self, hiz_limiti):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabilir!"
            assert hiz_limiti > 0, " Negatif bir hız olamaz"
            self.hiz_limiti = hiz_limiti
            
    reno = Araba(80)
    reno.hiz_limiti = -8

Fakat bu şekilde yaparak negatif hız olamaz kuralını bozmuş oluyoruz. Aynı zamanda negatif sayı gibi integer olmayan birşeyler de atayarak tamamen bozabiliriz. 

Arabamızın marka gibi bir özelliği daha olduğunu ve değiştirilmemesini beklediğimizi umalım.::

    class Araba:
        def __init__(self, hiz_limiti, marka):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabilir!"
            assert hiz_limiti > 0, " Negatif bir hız olamaz"
            assert type(marka) is str, " Arabanın markası sadece string olabilir "
            self.hiz_limiti = hiz_limiti
            self.marka = marka
            
    reno = Araba(80, "Reno")
    reno.marka = "Wolswogen"
    
Gördüğünüz gibi değiştirlmemesini istediğimiz halde markayı değiştirebildik. Peki bu iki sorunu nasıl önleriz. Nitelik olarak oluşturmadan önce diğer dillerde kullandığımız setter ve getter yöntemini görelim. Get bir özelliğin değerini almak için set de bir özelliğe yeni değer atamak için kullanılır.::

    class Araba:
        def __init__(self, hiz_limiti, marka):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabilir!"
            assert hiz_limiti > 0, " Negatif bir hız olamaz"
            assert type(marka) is str, " Arabanın markası sadece string olabilir "
            self._hiz_limiti = hiz_limiti
            self._marka = marka
            
        def get_hiz_limiti(self):
            return self._hiz_limiti
            
        def get_marka(self):
            return self._marka
            
        def set_hiz_limiti(self, yeni_limit):
            if type(yeni_limit) is not int:
                print("Hız limiti değiştirilemedi vermek istediğiniz hız limiti tam sayı olmalı")
            elif yeni_limit < 0:
                print("Hız limiti değiştirilemedi limit pozitif olmalı")
            else:
                self._hiz_limiti = yeni_limit

Kodu açıklamadan önce neden self.marka yerine self._marka tercih ettiğimizden bahsedelim. Fonksiyonların önüne ´_´ getirmek programcılar arasında bu fonksiyonun iç işleyişiyle alakalı buna dokunma gibi bir anlam taşır. Biz de doğrudan bu nitelikleri kullanmaları işleyişimizi bozacağından önüne ´_´ koyuyoruz.

Set ve get metodlarını kullanarak yukardaki şekilde yazabiliriz. Fakat estetik olarak ´reno.marka´ yazmaktansa ´reno.get_marka()´ yazmak çok hoş değil. Bunu daha güzel bir şekilde şöyle ifade edebiliriz::

    class Araba:
        def __init__(self, hiz_limiti, marka):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabilir!"
            assert hiz_limiti > 0, " Negatif bir hız olamaz"
            assert type(marka) is str, " Arabanın markası sadece string olabilir "
            self._hiz_limiti = hiz_limiti
            self._marka = marka
        
        @property
        def hiz_limiti(self):
            return self._hiz_limiti
            
        @property
        def marka(self):
            return self._marka
            
        @hiz_limiti.setter
        def hiz_limiti(self, yeni_limit):
            if type(yeni_limit) is not int:
                print("Hız limiti değiştirilemedi vermek istediğiniz hız limiti tam sayı olmalı")
            elif yeni_limit < 0:
                print("Hız limiti değiştirilemedi limit pozitif olmalı")
            else:
                self._hiz_limiti = yeni_limit
    
    
    reno = Araba(80, "Reno")
    print("renonun hızı: {}  markası: {}".format(reno.hiz_limiti, reno.marka))
    reno.hiz_limiti = -5
    print("renonun hızı: {}  markası: {}".format(reno.hiz_limiti, reno.marka))
    reno.hiz_limiti = 20
    print("renonun hızı: {}  markası: {}".format(reno.hiz_limiti, reno.marka))
    
    ### sonuç ###
    renonun hızı: 80  markası: Reno
    Hız limiti değiştirilemedi vermek istediğiniz hız limiti pozitif olmalı
    renonun hızı: 80  markası: Reno
    renonun hızı: 20  markası: Reno

Gördüğünüz gibi property ve propertyler için setter'ları bu şekilde kullanarak hem sınfın iç işleyişini iyi bir halde tutup hem de görünüşten taviz vermeyebiliriz.
