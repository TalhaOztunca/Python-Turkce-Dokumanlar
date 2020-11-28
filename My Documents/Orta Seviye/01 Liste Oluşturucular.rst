.. meta::
   :description: List Comprehensions
   :keywords: liste, liste oluşturucu, list, list comprehensions

.. highlight:: py3

**************************
Liste Oluşturucular/ List Comprehensions
**************************

Aslında bu konun giriş seviye olduğu kanısındayım ama ilk öğrenirken ben de tam anlamamış ve es geçmiştim bundan dolayı bu konuyu biraz irdeleme ihtiyacı hissettim.

0'dan 10'a kadar elemanları olan bir liste oluşturmak ::

    liste = []
    for i in range(10):
        liste.append(i)

Yukardaki kod doğru bir şekilde çalışmasına rağmen estetik açıdan okunaklılığı zayıftır. Pythonda bunu daha güzel şu şekilde ifade edebiliriz.::

    liste = [i for i in range(10)] # -> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

Üzerinde iterate ettiğimiz i'yi doğrudan kullanmak yerine üstüne işlem uygulanmış versiyonunu da kullanabilirsiniz.::

    [(i*i) for i in range(6)] # -> [0, 1, 4, 9, 16, 25]
    
Özellikle programlama yarışmalarında sık kullanılan bir satırda birçok tam sayının bulunduğu bir arrayi şu şekilde okuyabiliriz.::

    arr = [int(i) for i in input().split()]

Range dışında iterable herhangi bir obje üzerinde bu işlemi gerçekleştirebilirsiniz::

    [i for i in "Ahmet Talha"] # -> ['A', 'h', 'm', 'e', 't', ' ', 'T', 'a', 'l', 'h', 'a']

Belirli bir kuralı sağlamayan değerleri doğrudan listeye eklemeyebilirsiniz 3'e tam bölünenleri dahil etmeden 11'e kadar sayılar gibi::

    [i for i in range(10) if i%3 != 0] # -> [1, 2, 4, 5, 7, 8, 10]

Iterate ettiğimiz değeri listeyi oluşturuken kullanmıyorsak bir gelenek olarak _ kullanabiliriz. Örneğin içinde 6 tane sıfır bulunan bir liste için::

    [0 for _ in range(6)] # -> [0, 0, 0, 0, 0, 0]

Listeyi oluşturuken bir koşulu sağladığında bir değer sağlamadığında başka bir değer kullanmak istiyorsak şu tarz bir yöntem kullanabiliriz.::

    [i if i<5 else i**2 for i in range(10)] # -> [0, 1, 2, 3, 4, 25, 36, 49, 64, 81]

Yukarda 10'a kadar içinde eğer sayımız 5'ten küçükse kendisi değilse karesini içeren bir liste oluşturmuş olduk.



Bir fonksiyonu Iterable bir nesnedeki herşeye uygulamak için bu yöntemi kullanabilirsiniz.::

    def func(x): return x**2 + 3*x + 8
    liste1 = [8, 6, 13, 21]
    liste2 = [func(i) for i in liste1] # -> [96, 62, 216, 512]

İki for'u peş peşe kullanabilirsiniz. Verilebilecek örneklerden biri çarpım tablosu olabilir.::

    [f"{i}x{j}={i*j}" for i in range(1,10) for j in range(1,11)] # ->
    """
    ['1x1=1', '1x2=2',  '1x3=3',  '1x4=4',  '1x5=5',  '1x6=6',  '1x7=7',  '1x8=8',  '1x9=9',  '1x10=10', 
     '2x1=2', '2x2=4',  '2x3=6',  '2x4=8',  '2x5=10', '2x6=12', '2x7=14', '2x8=16', '2x9=18', '2x10=20', 
     '3x1=3', '3x2=6',  '3x3=9',  '3x4=12', '3x5=15', '3x6=18', '3x7=21', '3x8=24', '3x9=27', '3x10=30', 
     '4x1=4', '4x2=8',  '4x3=12', '4x4=16', '4x5=20', '4x6=24', '4x7=28', '4x8=32', '4x9=36', '4x10=40', 
     '5x1=5', '5x2=10', '5x3=15', '5x4=20', '5x5=25', '5x6=30', '5x7=35', '5x8=40', '5x9=45', '5x10=50', 
     '6x1=6', '6x2=12', '6x3=18', '6x4=24', '6x5=30', '6x6=36', '6x7=42', '6x8=48', '6x9=54', '6x10=60', 
     '7x1=7', '7x2=14', '7x3=21', '7x4=28', '7x5=35', '7x6=42', '7x7=49', '7x8=56', '7x9=63', '7x10=70', 
     '8x1=8', '8x2=16', '8x3=24', '8x4=32', '8x5=40', '8x6=48', '8x7=56', '8x8=64', '8x9=72', '8x10=80', 
     '9x1=9', '9x2=18', '9x3=27', '9x4=36', '9x5=45', '9x6=54', '9x7=63', '9x8=72', '9x9=81', '9x10=90']
    """

İki liste oluşturucuyu iç içe kullanarak da iki boyutlu listeler elde edebilirsiniz. Örnek olarak içi 0 dolu 3x4 bir matris::

    [[0 for _ in range(4)] for _ in range(3)] # -> [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]

Devamı sizin hayal gücünüze kalmış mesela sayıyı kendi çarpanlarıyla beraber tutmak::

    [i for i in range(10)] # 10'a kadar sayılar
    [j for j in range(1, i+1) if i%j == 0] # 1'den i'nin kendisine kadar kendini tam bölen sayılar
    
    [(i, [j for j in range(1, i+1) if i%j == 0]) for i in range(1, 10)]
    
    [i for i in range(6)] # 6'ya kadar olan sayıları depolar
    [x for x in range(1,i+1) if i%x == 0] # bu da i sayısının çarpanlarını depolar 1'den i sayısına kadar modu 0 yapan sayılar
    [[i,[x for x in range(1,i+1) if i%x == 0]] for i in range(6)]
    """ [(1, [1]), 
    (2, [1, 2]), 
    (3, [1, 3]), 
    (4, [1, 2, 4]), 
    (5, [1, 5]), 
    (6, [1, 2, 3, 6]), 
    (7, [1, 7]), 
    (8, [1, 2, 4, 8]), 
    (9, [1, 3, 9])]"""
    
