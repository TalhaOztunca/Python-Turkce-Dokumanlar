 .. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Bezeyiciler / Decarators
**************************

Not: Anlatımı ve zaman örneği hoşuma gittiği için kaynakça'da belirtilen vidyodan alıntı ve esinlenme yapılmıştır. 45.20'den itaberen decoratorlere değiniliyor.

Decaratorler türkçesiyle bezeyiciler bir fonksiyonu modifiye ederler. Mesela bir proje yaptınız ve yavaş çalışıyor elinizde de birsürü fonksiyon var ve hangisinin yavaşlığa neden olduğunu bulmak istiyorsunuz. Ne yapılabilir bir fonksiyon üzerinden gösterelim.::
    def topla(x, y):
        return x + y

Ne kadar zamanda yapıldığını bulmak için fonksiyon başlarken zamanı bulur ve döndürmeden önce bulduğumuz zamandan çıkarıp yazdırırız.::
    from time import time
    def topla(x, y):
        baslangic = time()
        sonuc = x + y
        bitis = time()
        print("su kadar zamanda tamamlandı: ", bitis - baslangic)
        return sonuc

Evet bir çözüm ama öte yandan kodlar ne kadar karmaşıklaştı daha kötüsü eğer 100 fonksiyon varsa herbirine bunu yapmak çok fazla zamanınızı alacaktır ve şuan uğraştığımız dil python olduğundan kesinlikle daha iyi bir yolu olduğunu tahmin edebilirsiniz.::
    from time import time
    def zaman_olcumlu_fonksiyon(func):
        def degistirilmis_fonksiyon(x, y):
            baslangic = time()
            sonuc = func(x, y)
            bitis = time()
            print("su kadar zamanda tamamlandı: ", bitis - baslangic)
            return sonuc
        return degistirilmis_fonksiyon
    
    def topla(x, y):
        return x + y
    add = zaman_olcumlu_fonksiyon(add)

    def çıkar(x, y):
        return x - y
    çıkar = zaman_olcumlu_fonksiyon(çıkar)
    
Gördüğünüz gibi iki fonksiyonda çok fazla değişiklik yapmadan değişti. Bu noktada python bunun daha güzel görünmesi için şöyle bir şey eklemiş.::
    from time import time
    def zaman_olcumlu_fonksiyon(func):
        def degistirilmis_fonksiyon(x, y):
            baslangic = time()
            sonuc = func(x, y)
            bitis = time()
            print("su kadar zamanda tamamlandı: ", bitis - baslangic)
            return sonuc
        return degistirilmis_fonksiyon
    
    @zaman_olcumlu_fonksiyon
    def topla(x, y):
        return x + y

    @zaman_olcumlu_fonksiyon
    def çıkar(x, y):
        return x - y

Böylelikle daha pratik gözüküyor. Bu noktada bir soru eğer karesini al diye tek parametre alan bir fonksiyonumuz olsaydı zaman_olcumlu_fonksiyon'u tek parametre yollayacak bir fonksiyon yollayacak şekilde değiştirmemiz gerekirdi ama bu istemediğimiz birşey bu yüzden::
    from time import time
    def zaman_olcumlu_fonksiyon(func):
        def degistirilmis_fonksiyon(*args, **kwargs):
            baslangic = time()
            sonuc = func(*args, **kwargs)
            bitis = time()
            print("su kadar zamanda tamamlandı: ", bitis - baslangic)
            return sonuc
        return degistirilmis_fonksiyon
    
    @zaman_olcumlu_fonksiyon
    def topla(x, y):
        return x + y

    @zaman_olcumlu_fonksiyon
    def çıkar(x, y):
        return x - y

    @zaman_olcumlu_fonksiyon
    def karesini_al(x):
        return x*x

Şeklinde düzenliyoruz ve artık tüm fonksiyonlara da uygulayabiliriz. Ama ben hangi fonksiyonun çaprıldığına yazdırdığım şeye bişeyler eklemek istiyorsam ne yapmalıyım hepsi için ayrı yazmaya geri mi döneceğiz yoksa::
    from time import time
    def bezeyici_gonder(yazirilacak_ad):
        def zaman_olcumlu_fonksiyon(func):
            def degistirilmis_fonksiyon(*args, **kwargs):
                baslangic = time()
                sonuc = func(*args, **kwargs)
                bitis = time()
                print("{} adlı fonksiyon {} kadar zamanda tamamlandı.".format(yazirilacak_ad, baslangic-bitis))
                return sonuc
            return degistirilmis_fonksiyon
        return zaman_olcumlu_fonksiyon
    
    @zaman_olcumlu_fonksiyon
    def topla(x, y):
        return x + y

    @zaman_olcumlu_fonksiyon
    def çıkar(x, y):
        return x - y

    @zaman_olcumlu_fonksiyon
    def karesini_al(x):
        return x*x

Ne mi yapdık öncelikle bir soluklanıp anlatmaya başlayalım. Bezeyici fonksiyon aslında fonksiyonumuzu alıp bezeyicideki fonksiyona yollayarak yerini değiştiriyor ve eğer siz fonksiyon() şeklinde bir çağrı yaparsanızda fonksiyon çağrılır. Biz @fonk(x, y) yazarsak önce fonk fonksiyonunu x ve y parametreleriyle çalıştırmış oluruz ki bu fonksiyon bize başka bir fonksiyon döndürmeli ki onu bezeyici olarak kullanalım. Aynı zamanda döndürdüğü fonksiyon yani bezeyici işi itibariyle başka bir fonksiyon döndürmeli kısaca yukarda yapılan değişkenlerle ve adlarlar oynayarak kendiniz daha iyi kavramaya çalışabilirsiniz.










Kaynakça:
https://www.youtube.com/watch?v=7lmCu8wz8ro
