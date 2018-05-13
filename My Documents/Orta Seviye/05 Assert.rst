 .. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Assert (Güzel bir Türkçe Karşılık Bulamadım)
**************************

Muhtemelen açıkları düzeltmek için harcadığınız zaman yazmak için harcadığınızdan fazladır. Özellikle projenin çapı büyüdükçe bu oran aynı seviyede artıyor işte assert kavramı burda devreye giren bir konsept. Bazen bir fonksiyon bir veriyi hatalı işlemesine rağmen bunun programı durduran bir hataya dönüşmesi de uzun sürebiliyor ardından başlayan baş ağrıtan süreç işte tamda bu yüzden eğer hata olduğu yerde bilinirse hem yapması hem de runtime'da daha sağlıklı çalışması sağlanabilir. Konuya örnek olması açısından bir program yazalım.::

    def uc_kati(sayi):
        return sayi*3
        
    def bol(bolunen, bolen):
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(input("Birinci sayıyı girin: "))
        y = input("İkinci sayıyı girin: ")
        sonuc = bol(x, y)
        print(sonuc)
        
Programımızı heyecanla yazdık ve sorun bol fonksiyonundan kaynaklı çıktı. Evet burda anlaması kolay hata bize / işlecinin str üzerinde çalışmadığını söylüyor buradan inputu int'e çevirmediğimizi farkedebiliriz ama sorun ilk çalışan fonksiyon uc_kati olmasına rağmen neden hata vermedi? çünki * işleci hem string'de hem de int'te çalışıyor ama biz bariz bir şekilde orada int olmasını istiyoruz. Hatayı düzeltelim bu seferde runtime bir soruna bakalım.::

    def uc_kati(sayi):
        return sayi*3
        
    def bol(bolunen, bolen):
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(int(input("Birinci sayıyı girin: ")))
        y = int(input("İkinci sayıyı girin: "))
        sonuc = bol(x, y)
        print(sonuc)

Birkaç kez denedik ve bu sefer girdilerden kaynaklı bir sorun çıktı ilk sayıyı 4 girdiler ikinci sayıyı 0 ve hata aldık. Yine anlaşılabilir bir hata ZeroDivisionError. Fakat ne her hata bu kadar yanlışımızı göstericek ne de kodlarımız bu kadar kısa olacak. Bu konunun önemini anladıktan sonra konumuza geçebiliriz. Peki assert ile bu durumu nasıl çözerdik?::

    def uc_kati(sayi):
        assert type(sayi) is int or type(sayi) is float, "uc_kati fonksiyonuna sadece int veya float yollamalısınız!!"
        return sayi*3
        
    def bol(bolunen, bolen):
        assert bolen != 0, "Bolen sayı 0 olamaz bölme işleminde hata var!!"
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(int(input("Birinci sayıyı girin: ")))
        y = int(input("İkinci sayıyı girin: "))
        sonuc = bol(x, y)
        print(sonuc)

Gördüğünüz üzere assert ifadesinden sonra sonucu bool olacak bir ifade ve virgülden sonra bir string yazıyoruz bool'un True olmadığı her durumda hata fırlayacak. Böylelikle hem hata başka yerlere sıçramadan çözebilir hem de kodunuzu başkaları kullanacaksa onlara yol gösterebilirsiniz. Ayrıca hatayı anlatan string'in anlaşılır olmasına dikkat edin.
