.. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Generatorler / Generators
**************************

Generatorler tek başına fonksiyon olarak iterator yapmaya yararlar.

Öncelikle belirli bir sınıra kadar olan fibonnacci sayılarını liste olarak gönderen bir fonksiyon yazalım. Ardından 18'e kadar olan fibonacci sayılarının toplamını bulan bir program yazalım.::

    def fibonnacci_sayilarini_bul(sinir):
        x = 0
        y = 1
        sayilar = [0, 1]
        while y < sinir:
            x, y =  y, x+y
            sayilar.append(y)
        return sayilar
        
    sayilar = fibonnacci_sayilarini_bul(18)
    toplam = 0
    for i in sayilar:
        toplam += i

Bunu yukarıdaki gibi yapabileceğimiz gibi generatorleri kullanarak da yapabiliriz::

    def fibonnacci_sayilarini_bul(sinir):
        x = 0
        y = 1
        while y < sinir:
            yield x
            x, y = y, x+y
    
    toplam = 0
    for i in sayilar:
        toplam += i
            
Bu generator ile biz iterator oluşturuyoruz ve next dediğimizde fonksiyon çalışmaya başlıyor. Fakat yield keyword'ü geldiği anda o değer geri döndürülüyor. Fakat fonksiyonlardan farklı olarak tekrar next çağrıldığında eski yield ettiği yerin sonraki adımından başlayarak tekrar yield ifadesi görene kadar devam ediyor. Sonuna geldiğinde StopIteration hatası veriyor.
            
İki yöntemle de istediğimizi elde edebiliyorsak neden generatorleri kullanmak isteyelim? 
   1) İlk yöntemde biz liste döndürmüştük yani bu hafızada üstünde dolaşmak istediğimiz veri kadar yer tutmak manasına geliyor. Fakat biz generator kullandığımızda ihtiyacımız olduğunda veriyi kullanıp sonrasında hafızamızda tutmamız gerekmez.
   2) Veriyi işlemeye başlamadan önce oluşturmamız gerekiyor. Mesela bize internet üzerinden 5 dakikada bir veri geldiğini ve bizim bu veriyi 2 dakikada işlediğimizi düşünelim. Sonuç olarak 10 tane veri alalım. Fonksiyon kullandığımızda 45 dakika veriyi almak için harcayıp 20 dakika da bu verileri işlemek için harcarız sonuç olarak 65 dakikada program biter. Fakat generator kullanırsak ilk veriyi alır işler sonra ikinci verinin gelmesini bekleriz ve bu böyle devam eder. Sonuç olarak 47 dakikada bitmiş olur.
   3) Bir farklı neden de kaç tane veri istediğimizi bilmiyor olabiliriz veya ulaşmak istediğimiz şeye ulaşınca kalanını kullanmamız gerekmeyebilir. Yukarıdaki örneği ele alırsak istediğimiz bilgi gelen ilk veri içinde olsun. Fonksiyon kullandığımızda 45 dakika tüm verilerin gelmesini ve 2 dakika ilk verinin işlenmesini bekleriz toplamda 47 dakika. Generator kullandığımızda ilk veri gelir 2 dakika içinde işler aradığımızı bulur ve 2 dakika içinde programı bitiririz.

Kaç veri istediğimizi bilmememize örnek olarak fibonacci sayılarının toplamının belirli bir sayıdan düşük en büyük halini istediğimizi düşünelim. Kaç eleman kullanarak bu sayıyı geçeceğimizi bilmediğimizden fonksiyonlarla bunu çözmek mantıklı olmayacaktır::

    def fibonacci():
        a = 0
        b = 1
        while True:
            yield a
            a, b = b, a+b
            
    res = 0
    limit = 15
    for i in fibonacci():
        print(f"{i=}, {res=}")
        if res+i > limit:
            break
        res += i
        
!!! Fibonacci kodları örnek açısından verilmiştir. Özellikle büyük adımları bulmada kullanabileceğiniz çok daha iyi alternatif çözümleri vardır

Başka bir generatorden üretilen verileri aktarmak için ´yield from´ ifadesini kullanabiliriz. Örneğin 4 için 1, 1, 2, 1, 2, 3, 1, 2, 3, 4 dizisini üreten bir kod tasarlayalım::

    def nKadar(n):
        for i in range(1, n+1):
            yield i

    def nKadarN(n):
        for i in range(1, n+1):
            yield from nKadar(i)
    
    for i in nKadarN(4):
        print(i)

Graphlar üzerinde dfs yapmak için yield from'u kullanalım::

    def dfs(node, connection, visited):
        if node not in visited:
            yield node
            visited.add(node)
            for i in connection.get(node, []):
                yield from dfs(i, connection, visited)        
            
    node = 0
    connection = {0:[1,3,4],
        1:[0],
        2:[1, 3, 5],
        3:[0, 5],
        4:[0, 5],
        5:[2, 3, 4]}
    visited = set()

    for i in dfs(node, connection, visited):
        print(i)

Ayrıca bu generatorleri oluşturmak için liste oluşturuculara benzer ifadeler de vardır::

    [i*3 for i in range(6)] # bu kod 0'dan 6'ya kadar olan sayıların 3 katlarının listesini oluşturuyordu.
    gen1 = (i*3 for i in range(6)) # bu ise 0'dan 6'ya kadar olan sayıların 3 katını dönderecek bir generator oluşturuyor.
    for i in gen1: # gen1'in elemanları arasında dön
        print(i) # i'yi bastır
 
Liste oluşturucularla verilen tüm örnekleri generatorler için de [] yerine () kullanarak deneyebilirsiniz

Hafızada yer kaplamaması duruma göre dezavantaj da oluşturabilir. Örneğin fibonacci örneğinde iki önceki elemanı ver 3. elemandan tekrarla gibi ifadeler kullanamayız. Fakat bir liste olsaydı üstünde istediğimiz değişiklikleri yapabilirdik. 
