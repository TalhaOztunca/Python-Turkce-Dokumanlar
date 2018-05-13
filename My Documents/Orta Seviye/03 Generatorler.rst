.. meta::
   :description: İteratorlar / Iterators
   :keywords: iterator

.. highlight:: py3

**************************
Generatorler / Generators
**************************

Generatorler iterator yapmaya yarar. Peki bu generatorlerin bize faydası nedir bir örnek üzerinden inceleyelim. 

18'den küçük fibonnacci sayılarınının toplamını bulan bir program yazalım.::

    def fibonnacci_sayilarini_bul(sinir):
        x = 1
        y = 1
        sayilar = [1, 1]
        while y < sinir:
            x, y =  y, x+y
            sayilar.append(y)
        return sayilar
        
    sayilar = fibonnacci_sayilarini_bul(18)
    toplam = 0
    for i in sayilar:
        toplam += i

Bu tarz bir kod işimize yarıyor gibi peki neden bunu listeye çevirip onu iterator yapmak yerine neden doğrudan generator yapmak isteyelim. Çünki bize sağladığı çok önemli faydalar var. Bu özellikleri saymadan önce nasıl yapıldığını görelim. Aslında fonksiyonlara benziyorlar.::

    def fibonnacci_sayilarini_bul(sinir):
        x = 1
        y = 1
        yield 1
        yield 1
        while y < sinir:
            x, y = y, x+y
            yield y

Söylediğim gibi fonksiyon gibiler ama arasındaki en büyük yazım farkı return deyimi yerine yield kullanılması. iteratordan her sonraki nesne istendiğinde fonkiyona dönüyor yield görene kadar işlemleri yaptırıyor ve tekrar istendiğinde aynı kaldığı yerden devam ediyor. Avantajlarına gelecek olursak:
A) Farkettiyseniz ikincisinde elemanları listede tutmadık yani atıyorum 1 milyon eleman üzerinde yapmamız gereken bir işlem olsaydı hepsini hafızamızda tutmamız gerekecekti ama öte yandan generatorümüzde sadece iki veriyi hafızamızda tutuyoruz.
B) Generatorlerde her ihtiyacımız olduğunda veriyi alıyoruz ama fonksiyonda bütün veriler tamamlandığında yani bizim aradığımız şey ikinci elemandaysa ama liste 10000 elemanlıysa 2 birim zamanda elde edip gerisine ihtiyacımız olmayacak şey için fuzuli zaman harcamış olacaktık.
C) B dekine benzer şekilde örneğin biryerden 5 dakikada bir veri alıyorsak (internet üzerinden olur, robottan olur) ve 16 adet veriye ihtiyacımız varsa biz bu verileri işlemeye başalamadan önce 80 dakika bekleyecek ve ondan sonra tamamını işleyecektik onun yerine veriyi alıp işlesek ve 5 dakika sonra diğer veriyi alıp işlesek o 80 dakikadaki programın boş durmasını engellerdik ayrıca B'deki gibi aradığımız veri 2. sinyal arasındaysa belkide 11 dakikada yapılacak işi 85 dakikada yapmak zorunda kalacaktık.


Ayrıca bu generatorleri oluşturmak için liste oluşturuculara benzer ifadeler de vardır. Örneğin::

    [i*3 for i in range(6)] # bu kod 0'dan 6'ya kadar olan sayıların 3 katının listesini oluşturuyordu.
    gen1 = (i*3 for i in range(6)) # bu ise 0'dan 6'ya kadar olan sayıların 3 katını dönderecek bir generator oluşturuyor.
    for i in gen1: # gen1'in elemanları arasında dön
        print(i) # i'yi bastır
    
Liste oluşturucularda verilen tüm örnekler [] generatorler için de geçerlidir deneyerek görebilirsiniz ()

Ayrıca ufak bir hatırlatma olarak generatorler hafızada yer kaplamaz yani bu şu manaya geliyor üstünden geçdikten sonra o veriye tekrar ulaşamazsınız generator1[3] gibi bir olay söz konusu değil yani veriyi hafızanızda tutmak istiyorsanız bunu aklınızda bulundurmak iyi olabilir.
