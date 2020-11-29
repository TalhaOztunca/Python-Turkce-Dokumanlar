 .. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Assert (Güzel bir Türkçe Karşılık Bulamadım)
**************************

Muhtemelen bug'ları düzeltmek için harcadığınız zaman kodu yazmak için harcadığımız zamandan fazladır. Özellikle projenin çapı büyüdükçe bu oran aynı seviyede artıyor. Python'da fonksiyonların parametrelerini de belli bir tiple sınırlandıramayınca hata alıp başını ordan oraya gidebiliyor. Mesela hatalı bir kod parçası örneği::

    def uc_kati(sayi):
        return sayi*3
        
    def bol(bolunen, bolen):
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(input("Birinci sayıyı girin: "))
        y = input("İkinci sayıyı girin: ")
        sonuc = bol(x, y)
        print(sonuc)

Programı çalıştırıp sonucu bekliyoruz ve bize bol fonksiyonunda bir hata olduğunu söylüyor. Çeşitli inputlarla deniyoruz ama ortada problem yok. Örnek için inputu yazdırdık ve görüyoruz ki x'in içinde 333 var input 3 olmasına rağmen bu şekilde uğraşmaya devam ediyoruz. Problem nerede diye soracak olursanız input'larımızı integer değerlere dönüştürmedik. Bunu dönüştürelim ve karşılaşabileceğimiz başka bir hataya göz atalım.::

    def uc_kati(sayi):
        return sayi*3
        
    def bol(bolunen, bolen):
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(int(input("Birinci sayıyı girin: ")))
        y = int(input("İkinci sayıyı girin: "))
        sonuc = bol(x, y)
        print(sonuc)

Denemelerimizi yaptık herşey yerli yerinde gözüküyor. Fakat 4 ve 0 inputunu girdiğimizde bir hatayla karşılaştık. Bu sefer neden daha iyi anlaşılabilir ZeroDivisionError.

Peki burada python bize neler sunuyor. 2 seçeneğimiz var. Birisi hata göndermek. İkincisi assert kullanmak. Öncelikle assert kullanarak nasıl yazılabileceğini görelim::

    def uc_kati(sayi):
        assert type(sayi) is int, "uc_kati fonksiyonuna sadece int yollamalısınız!!"
        return sayi*3
        
    def bol(bolunen, bolen):
        assert bolen != 0, "Bolen sayı 0 olamaz bölme işleminde hata var!!"
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(int(input("Birinci sayıyı girin: ")))
        y = int(input("İkinci sayıyı girin: "))
        sonuc = bol(x, y)
        print(sonuc)

Örnekte incelenebildiği üzere assert üç parçadan oluşuyor. Önce ´assert´ yazıyoruz ardından o an programdan beklentimizin ne olduğunu yazıyoruz bolen'in sıfır olmaması sayi'nın integer olması gibi. Ardından virgül ile beraber beklediğimiz koşul doğru olmaz ise ne yazdırarak programı sonlandıracağımızı yazıyoruz.

Burda koşulumuzu iyi bir şekilde belirlemeliyiz. Örneğin yukarıdaki kod float değerler için de çalışması problem değilken bu haliyle çalışamıyor veya int değerinden türetilmiş sınıflarla da çalışamıyor. Onun yerine herhangi bir sayıyla(float, int, complex veya bunlardan türeyen her şey) çalışsın demek için aşşağıdaki kodu kullanabilirdik::

    import numbers
    def uc_kati(sayi):
        assert isinstance(sayi, numbers.Number), "uc_kati fonksiyonuna sadece sayı yollamalısınız!!"
        return sayi*3
        
    def bol(bolunen, bolen):
        assert bolen != 0, "Bolen sayı 0 olamaz bölme işleminde hata var!!"
        return bolunen / bolen
        
    if __name__ == "__main__":
        x = uc_kati(int(input("Birinci sayıyı girin: ")))
        y = int(input("İkinci sayıyı girin: "))
        sonuc = bol(x, y)
        print(sonuc)

Peki hata mı vermeliyiz assert mi kullanmalıyız sorusuna gelirsek. Duruma bağlı. İlk önce öngörülebilir ve düzeltilebilir mi? Eğer öyleyse hata kullanıp düzeltmek için açık kapı bırakmakta fayda var. Mesela 0'la bölünemez hatasını yollarsak dışardan kullanıcıdan tekrar input alarak bir çözüm yazılabilirdi. Fakat şöyle bir senaryo düşünün biz bir dosyayı bir formattan başka bir formata dönüştürüyoruz ama dosya tanımlı formatla uyuşmayacak veriler içeriyor bu noktada herhangi bir değişiklik yapmak yerine programı doğrudan sonlandırarak kullanıcıya bir uyarı verebiliriz.
