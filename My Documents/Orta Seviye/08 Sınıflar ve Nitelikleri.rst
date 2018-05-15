 .. meta::
   :description: Nitelikler/ Properties
   :keywords: property

.. highlight:: py3

**************************
Sınıflar ve Nitelikler/ Classes and Properties
**************************

Doldur????????????????????

Getters and Setters
**************
Getter ve setter metodları objelerimizin belirli niteliklerine ulaşılmak istendiğinde kullanılır. Buna neden ihtiyacımız var önce bir örnek verelim.::

    class Araba:
        def __init__(self, hiz_limiti):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabileceği varsayılmıştır fakat siz {} \
    tipinde değer yolladınız".format(type(hiz_limiti))
            assert hiz_limiti > 0, " - bir hız olamaz ama arabanıza - bir hız verdiniz. {}".format(hiz_limiti)
            self.hiz_limiti = hiz_limiti

Şimdi çalışma esnasında hız limitini değiştirecek bir olay olduğunu ve değiştirmemiz geektiğini düşünelim şöyle bir yol izleyebilirdik ama bu şekilde değiştirdiğimizde hız limitinin - olmaması gerektiğini söyleyen kuralı kırabilecek miyiz?::

    class Araba:
        def __init__(self, hiz_limiti):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabileceği varsayılmıştır fakat siz {} \
    tipinde değer yolladınız".format(type(hiz_limiti))
            assert hiz_limiti > 0, " - bir hız olamaz ama arabanıza - bir hız verdiniz. {}".format(hiz_limiti)
            self.hiz_limiti = hiz_limiti
            
    reno = Araba(80)
    reno.hiz_limiti = -8
    
Aynı zamanda -'li bir değer yazabileceğimiz gibi string de atayabilir ve çalışma zamanında hatalar oluşturabiliriz. Aynı şekilde arabamızın marka gibi bir özelliği daha olduğunu ve değiştirilmemesini beklediğimizi umalım.::

    class Araba:
        def __init__(self, hiz_limiti, marka):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabileceği varsayılmıştır fakat siz {} \
    tipinde değer yolladınız".format(type(hiz_limiti))
            assert hiz_limiti > 0, " - bir hız olamaz ama arabanıza - bir hız verdiniz. {}".format(hiz_limiti)
            assert type(marka) is str, " Arabanın markası sadece string bir değer olabilir ama siz {} tipi bir değer yolladınız {} ".format(marka)
            self.hiz_limiti = hiz_limiti
            self.marka = marka
            
    reno = Araba(80, "Reno")
    reno.marka = "Wolswogen"
    
Gördüğünüz gibi yeni bir değer atanmamasını istediğimiz halde markayı değiştirebildik. Peki bu iki sorunu nasıl önleriz. Bezeyici olarak kullanmadan önce fonksiyon olarak getter ve setter ları nasıl kullanabileceğimizi gösterelim. Get bir özelliğin değerini almak için set de bir değere yenisini atamak için kullanılır.::

    class Araba:
        def __init__(self, hiz_limiti, marka):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabileceği varsayılmıştır fakat siz {} \
    tipinde değer yolladınız".format(type(hiz_limiti))
            assert hiz_limiti >= 0, " - bir hız olamaz ama arabanıza - bir hız verdiniz. {}".format(hiz_limiti)
            assert type(marka) is str, " Arabanın markası sadece string bir değer olabilir ama siz {} tipi bir değer yolladınız {} ".format(marka)
            self.hiz_limiti = hiz_limiti
            self.marka = marka
            
        def get_hiz_limiti(self):
            return self.hiz_limiti
            
        def get_marka(self):
            return self.marka
            
        def set_hiz_limiti(self, yeni_limit):
            if type(yeni_limit) is not int:
                print("Hız limiti değiştirilemedi vermek istediğiniz limit int(tam sayı) olmalı!!", \
    "sizin gönderdiğiniz parametrenin türü {}".format(type(yeni_limit)))
            elif yeni_limit < 0:
                print("Hız limiti değiştirilemedi limit pozitif olmalı!! sizin denemeniz {}".format(yeni_limit))
            else:
                self.hiz_limiti = yeni_limit

Evet set ve get metodlarını kullanarak bu şekilde bir yöntem kullanırsak değerlerin doğru bir şekilde değişmesini sağlarız ama hem x.y = 13 gibi bir atama yapmak varken veya print(x.y) yazdırmak varken x.set_y(13) veya x.get_y() yazmak zor hemde o şekilde bir kullanımın önüne geçememiş olduk işte tam burda gerekli bezeyicileri kullanarak yapabiliriz. Nasıl mı? (Bu arada bundan sonra değişkenlerin başına _ koyuyoruz bu onların sınıf iç işleyişiyle alakalı olduğunu ve dışardan doğrudan değiştirilmemesi gerektiğini anlatır.)::

    class Araba:
        def __init__(self, hiz_limiti, marka):
            assert type(hiz_limiti) is int, " Hız limitinin sadece pozitif tam sayı olabileceği varsayılmıştır fakat siz {} \
    tipinde değer yolladınız".format(type(hiz_limiti))
            assert hiz_limiti >= 0, " - bir hız olamaz ama arabanıza - bir hız verdiniz. {}".format(hiz_limiti)
            assert type(marka) is str, " Arabanın markası sadece string bir değer olabilir ama siz {} tipi bir değer yolladınız {} ".format(marka)
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
                print("Hız limiti değiştirilemedi vermek istediğiniz limit int(tam sayı) olmalı!!", \
    "sizin gönderdiğiniz parametrenin türü {}".format(type(yeni_limit)))
            elif yeni_limit < 0:
                print("Hız limiti değiştirilemedi limit pozitif olmalı!! sizin denemeniz {}".format(yeni_limit))
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
    Hız limiti değiştirilemedi limit pozitif olmalı!! sizin denemeniz -5
    renonun hızı: 80  markası: Reno
    renonun hızı: 20  markası: Reno

Gördüğünüz gibi property ve propertyler için setter'lar bu şekilde oluşturuluyor.
