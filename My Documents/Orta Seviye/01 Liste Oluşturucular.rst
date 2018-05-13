.. meta::
   :description: List Comprehensions
   :keywords: liste, liste oluşturucu, list, list comprehensions

.. highlight:: py3

**************************
Liste Oluşturucular/ List Comprehensions
**************************

Aslında bu konun giriş seviye olduğu kanısındayım ama ilk öğrenirken ben de tam anlamamış ve es geçmiştim bundan dolayı bu konuyu biraz irdeleme ihtiyacı hissettim.

Normalde bir liste oluştururken izlenen yollardan biri şudur ki içinde 0'den 10'a kadar olan sayıları bulundursun(burdan itibaren her e kadar diyişimde üst sınırın dahil olmadığını varsayıyorum)::
    
    liste = []
    for i in range(10):
        liste.append(i)

Bu kod çalışan bir kod olmasına karşın yeterince okunaklı değildir ve ayrıcı tanımlandığı satırda ne olduğunu doğrudan okutmak hem kısa hem de daha güzel görünecektir. İşte bunun için pythonda şöyle bir ifade şekli var.::

    liste = [i for i in range(10)] # -> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

Bu arada elemanlar üzerinde oynama yapabilirsiniz. Dediğimi bir örnekle açıklayayım. Bu sefer 0'dan 6'ya kadar olan sayıların karelerini tutmak isteyelim.::

    [(i*i) for i in range(6)] # -> [0, 1, 4, 9, 16, 25]

Bu arada sadece range kullanılmış olması sizi yanıltmasın. Iterable herhangi bir şey üzerinde bunu uygulayabilirsiniz. Mesela elinizdeki stringdeki karakterlerden oluşan bir liste::

    [i for i in "Ahmet Talha"] # -> ['A', 'h', 'm', 'e', 't', ' ', 'T', 'a', 'l', 'h', 'a']

Peki bu listenin içinde küçük a geçmemesini isteseydik::

    [i for i in "Ahmet Talha" if i != "a"] # -> ['A', 'h', 'm', 'e', 't', ' ', 'T', 'l', 'h']

Dönen değerle işimiz olmadan içinde 6 tane sıfır bulunan bir liste için::

    [0 for _ in range(6)] # -> [0, 0, 0, 0, 0, 0]

Bu sefer i yerine _ kullanmamızın sebebi bir python geleneği eğer döngüde dönen değerle işimiz yoksa bunu göstermek için i, j, k, deger gibi bir ifade yerine _ kullanırız ki başkaları tarafından daha rahat anlaşılabilsin.

Sonraki kısma biraz dikkat etmenizde fayda var ben de lambda da gördüğüm bir kuraldan yola çıkarak bunu keşfettim. Normalde if kullandığınızda o listeye eleman eklemeyi bırakıyor 'a' eklenmemesinde olduğu gibi peki ya biz 'Ahmet Talha' ifadesi içinde 'm' geçtiği yerlerde listeye eklenmemesi yerine büyük 'M' yazılmasını isteseydik.::

    [i if i != 'm' else 'M' for i in "Ahmet Talha"] # -> ['A', 'h', 'M', 'e', 't', ' ', 'T', 'a', 'l', 'h', 'a']

Şimdi burada tam olarak ne yaptık? ifadeyi iki parçaymış gibi okumaya çalışalım. İlk olarak 'for i in "Ahmet Talha"' bu kısım bir string üzerinde i'yi döndürüyor, '(i) if (i != m) else ("M")' bu kısımı şu şekilde ayırırsak anlaması daha kolay olacakdır kısaca eğer i değeri m değerinden farklı ise listeye i'nin değerini elemanı koy aksi halde listeye 'M' elemanı koy.

Elinizde bir fonksiyon var ve bunun bir listedeki tüm elemanlara uygulanmış haline mi ihtiyacınız var.::

    def func(x): return x**2 + 3*x + 8
    liste1 = [8, 6, 13, 21]
    liste2 = [func(i) for i in liste1] # -> [96, 62, 216, 512]

Şuana kadar hep tek for döngüsü kullandık birazda iki for döngüsüyle oluşturulan listelerle nasıl oynıyabileceğimize göz atalım. Çarpım tablosununun 3x4'lere kadar olan kısmını string halinde tutan listeye bakalım.::
    
    [("{}x{}={}".format(x,y,x*y)) for x in range(4) for y in range(5)] 
    
    """ ['0x0=0', '0x1=0', '0x2=0', '0x3=0', '0x4=0', 
    '1x0=0', '1x1=1', '1x2=2', '1x3=3', '1x4=4', 
    '2x0=0', '2x1=2', '2x2=4', '2x3=6', '2x4=8', 
    '3x0=0', '3x1=3', '3x2=6', '3x3=9', '3x4=12'] """

Belkide iki boyutlu diziler oluşturmak istiyoruzdur mesela sayının kendisi iki katı ve karesini bir arada tutmak 6'ya kadar olanları diyelim::

    [[i, i*2, i**2] for i in range(6)] 
    
    """ [[0, 0, 0], [1, 2, 1], 
    [2, 4, 4], [3, 6, 9], 
    [4, 8, 16], [5, 10, 25]] """

Ya da 3x4 içi 0 dolu olan bir liste::

    [[0 for _ in range(4)] for _ in range(3)] # -> [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]

Bir iki boyutlu listeye istediğiniz fonksiyonu uygulamak::

    def func(x):return x**2 + 3
    liste = [[0, 5, 4], [2, 4]] 
    yeni_liste = [[func(y) for y in x] for x in liste] # -> [[3, 28, 19], [7, 19]]

6' ya kadar olan sayıları çarpanlarıyla beraber depolamak bu kısmı üç adımda anlatıyım::
    
    [i for i in range(6)] # 6'ya kadar olan sayıları depolar
    [x for x in range(1,i+1) if i%x == 0] # bu da i sayısının çarpanlarını depolar 1'den i sayısına kadar modu 0 yapan sayılar
    [[i,[x for x in range(1,i+1) if i%x == 0]] for i in range(6)]
    """ [[0, []], 
    [1, [1]], 
    [2, [1, 2]], 
    [3, [1, 3]], 
    [4, [1, 2, 4]], 
    [5, [1, 5]]]"""
    
