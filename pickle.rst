.. meta::
   :description: Bu bölümde pickle modülünü inceleyeceğiz.
   :keywords: python, modül, import, pickle

*************
pickle Modülü
*************

**Kaynak Kodu:** https://github.com/python/cpython/blob/3.5/Lib/pickle.py
**Belge Kaynağı:** https://docs.python.org/3.5/library/pickle.html

Bu modül çoğu Python nesnesini ``bytes`` türüne çevirebilmektedir. Bu özelliği
veritabanlarına kayıtlardan tutun internet üzerinden bir değişken gönderme işlemine
kadar çok farklı yerlerde kullanabilirsiniz.

.. note:: Pickle modülü ile kaynağından emin olmadığınız veya bozulmuş
        `bytes` verileri üzerinde işlem yapmak programınıza zarar verebilir.
        Resmi Python dökümantasyonu şu uyarıyı yapmaktadır: Pickle modülü hatalı 
        veya kötü amaçlı oluşturulmuş verilere karşı güvenli değildir. Asla 
        güvenilir olmayan veya doğrulanmayan bir kaynaktan alınan verileri kullanmayın.


Fonksiyonlar
=============

Pickle modülünün içinde burada bahsetmeyeceğimiz bir kaç yardımcı sınıf bulunmaktadır
ancak inceleyeceğimiz 4 temel fonksiyon kullanılarak da sınıflar ile yapabilecek-
lerimizin çoğu yapılabilir.

**pickle.dumps(obj, protocol=None , * , fix_imports=True)**

    Bu fonksiyonda ilgileneceğimiz tek parametre ``obj`` olacak. Fonksiyon
    ``obj`` parametresine verilen Python değişkenini ``bytes`` haline çevirerek döndürür.
    Bir örnek verelim::

        >>> import pickle
        >>> byte_değeri = pickle.dumps(1)
        >>> print(byte_değeri)
        b'\x80\x03K\x01.'

    Gördüğünüz gibi fonksiyonumuz bize ``1`` değerinin ``bytes``'a çevrilmiş halini verdi.
    
.. note:: Bu çevrilmiş hal ``bytes(1)`` ile karıştırılmamalıdır. Çünkü pickle modülündeki amaç
          bu ``bytes``'ın daha sonra tekrar ``1`` verisinin elde edilebilmesinde kullanılmasıdır
          ve çalışma şekli farklıdır. ``bytes(1)`` ise bize verilen sayı miktarında byte'lardan
          oluşan boş bir liste vermektedir.


**pickle.loads(data , * , fix_imports=True , encoding='ASCII' , errors='strict')**

    Bu fonksiyonda sadece ``data`` parametresi ile ilgileneceğiz. Fonksiyonumuz daha önce-
    den ``pickle.dumps`` fonksiyonu ile ``bytes``'a çevrilen veriyi tekrar oluşturup geri 
    döndürür. Bir örnek verelim::

        >>> import pickle
        >>> değişken = pickle.loads(b'\x80\x03K\x01.') # Bir üst örnekteki bytes'ı argüman olarak veriyoruz.
        >>> print(değişken)
        1

    Gördüğünüz gibi fonksiyonumuz bize bir önceki örnekte ``bytes``'a çevirdiğimiz 
    değeri yani ``1``'i döndürdü.


**pickle.dump(obj , file, protocol=None, * , fix_imports=True)**

    Bu fonksiyonun ``dumps`` fonksiyonundan tek farkı verilen ``obj``'yi verilen 
    ``file`` dosya akışına (iostream) yazmasıdır. Bir örnek verelim::

        >>> import pickle
        >>> with open("data.txt","wb") as dosya: # wb ile açtık çünkü byte yazacağız
                pickle.dump(1,dosya)

    Dosyamızı ``wb+`` ile de açabilirdik. Önemli olan dosyamızı byte 
    yazacak şekilde açmamız çünkü değişkenlerimiz ``bytes``'a çevriliyor.
    Aslında bu fonksiyon, ``dumps`` fonksiyonunu kullansaydık ondan dönecek olan değeri 
    dosyaya yazmış oldu.

    Bu fonksiyon bir değer döndürmez.


**pickle.load(file , * , fix_imports=True , encoding='ASCII' , errors='strict')**
    
    Bu fonksiyonun da ``loads`` fonksiyonundan tek farkı verilen ``file`` dosya akışının
    içeriğini okuyarak değişkene dönüştürmektir. Az önce kullandığımız ``data.txt`` 
    üzerinden bir örnek verelim::

        >>> import pickle
        >>> with open("data.txt","rb") as dosya: # rb ile açtık çünkü byte olarak okuyacağız
                değişken = pickle.load(dosya)
        >>> print(değişken)
        1

    Gördüğünüz gibi fonksiyonumuz bize bir önceki örnekte dosyaya yazdığımız değeri
    yani ``1``'i döndürdü.


Buraya kadar yaptıklarımız size çok basit gelmiş olabilir. Sonuçta bir dosyaya 
kendiniz de '1' yazabilir ve sonra okuyarak ``int`` türüne dönüştürebilirsiniz.
Bu modülün asıl özelliği en başta da söylediğimiz gibi neredeyse bütün Python
nesneleri ile çalışabilmesidir. Bunlara ``dict`` , ``list`` , ``tuple`` , ``set``
ve hatta sizin kendi tanımladığınız sınıflar bile dahildir.

Şimdi gerçek bir örnek verelim. Varsayalım ki bir oyun yapıyorsunuz ve oyunun
karakteri için bir sınıfın örneğine sahipsiniz::

    class Karakter:
        def __init__(self):
            self.can = 100
            self.hız = 5
            self.güç = 20
            self.özellikler = ['uçma gücü','wall hack','aim bot']

        def güçlendir(self,değer):
            self.güç += değer

    oyuncu = Karakter()

    ...

    oyuncu.güçlendir(5)
    oyuncu.can -= 10

Şimdi oyunu oynayan kişi oyuna ara verip kapamak istiyor. Bizim de ``oyuncu``
değişkenini daha sonra tekrar kullanmak için kaydetmemiz lazım. İşte bunun gibi
durumlarda ``oyuncu`` örneğimizin ne kadar çok niteliği olursa olsun ``dump`` fonksiyonu
imdadımıza yetişiyor::

    with open("oyuncu bilgileri.txt","wb") as dosya:
        pickle.dump(oyuncu,dosya)

Artık oyun bir daha açıldığında oyuncu karakterimizi bütün nitelikleri aynı olacak
şekilde yeniden yükleyebiliriz::

    with open("oyuncu bilgileri.txt","rb") as dosya:
        oyuncu = pickle.load(dosya)

Eğer gerçek bir uygulamada kullanıcı bu dosyayı değiştirmeye çalışırsa 
(kullanıcının pickle modülünü kullanmayı bilmediğini varsayıyoruz :D) kendi lehine
bir şey yapamaz çünkü pickle verileri okunabilir değildir,
en fazla dosyayı bozar ki bu da kullanıcı için tehlikelidir.
Ama sonuç olarak bu ``bytes`` verileri üzerinde şifreleme de uygulanabilir.

Pickle modülünün ne kadar çok faydalı olabileceğinin anlaşıldığını düşünüyorum.

Ayrıca internet üzerinde veri aktarımında da her zaman ``bytes`` veri tipi kullanıldığı
için pickle modülünü kullanarak herhangi bir değişkeni internet üzerinden başka
 bilgisayarlara aktarmak da mümkündür. Python ile internet üzerinden haberleşme
 örneklerine bakmak istiyorsanız internette 'python socket library' aramasını yapabilirsiniz.

Şimdi pickle ile json arasındaki birkaç önemli farktan bahsedelim.
    • JSON verileri unicode olarak, çoğunlukla da ``utf-8``
    olarak saklar. Pickle modülü ise bunu ``bytes`` olarak saklar.
    • JSON insanlar tarafından okunulabilecek bir formatta iken, 
    pickle çıktıları okunulabilir değildir.
    • JSON diller arasında da veri aktarımında yoğun olarak kullanılırken pickle, Python'a özgüdür.
    • Python'daki ``json`` modülü sadece bazı gömülü veri tiplerini (``int``,``str`` gibi) 
    saklayabilirken ``pickle`` modülü neredeyse bütün python nesneleri saklayabilir.

Son olarak pickle modülü ile kullanılamayacak birkaç nesne türünden bahsetmek istiyorum:
    • Türü ``class <module>`` olan nesneler (örneğin ``math`` modülü)
    • Generator nesneleri (örneğin: ``(i for i in range(10))``)
    • Lambda fonksiyonları (normal fonksiyonlar pickle ile kullanılabilir)
    • Bildiğiniz gibi Python, C dili ile yazılmıştır. Bu sayede C dili ile Python 
    eklentileri yazılabilmektedir. Bunların bazıları pickle ile kullanılamayabilir.
    • Eğer kendi tanımladığınız bir sınıf örneğini kaydettikten sonra ``load`` fonksiyonu
    ile yüklemeye çalışıyorsanız ama bunu yapan kod içerisinde bu sınıf silinmiş veya
    henüz bu sınıf tanımlanmamış ise, yani kodunuz bu sınıfı herhangi bir şekilde (siz tanımlamış 
    veya import etmiş olabilirsiniz) içermiyor ise veri yüklenemeyecek ve hata 
    yükseltilecektir. Aynı şey fonksiyonlar için de geçerlidir.