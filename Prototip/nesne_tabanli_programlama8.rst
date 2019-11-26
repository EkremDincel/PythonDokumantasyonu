.. meta:: 
	:description: Bu bölümde nesne tabanlı programlamadan söz edeceğiz.
    :keywords: python, python3, nesne, oop, sınıf, class, magic,
           sihirli, nesne yönelimli programlama, nesne tabanlı programlama,
           object oriented programming, self , other ,instance, örnek,
           metod, sihirli metotlar, magic methods,

.. highlight:: py3

*******************************************
Nesne Tabanlı Programlama (Devamı)
*******************************************

Nesne tabanlı programlamaya ilişkin bu bölümde önceki derslerde farkında 
olmadan biraz incelemiş olduğumuz bir konuyu ayrıntıları ile göreceğiz.


Sihirli Methodlar (Magic Methods) Nedir?
******************************************

Biz pythonda sınıf metotlarının ve örnek metotlarının ne olduklarını biliyoruz.
Sihirli metotlar da birer örnek metotlarıdırlar. Ancak her örnek metodu bir
sihirli metot değildir. Peki sihirli metotları diğer metotlardan ayıran ne? 
Ne gibi özellikleri var?

Öncelikle nelere sihirli metot dendiğinden başlayacak olursak ``__`` ile başlayan
ve yine ``__`` ile biten örnek metodlarına "sihirli metot" diyoruz. Tabii her
önüne gelen metot bu şekilde yazıldığında sihirli olmuyor. Sihirli metotlar
belirli ve sınırlıdır. Örneğin ``__init__()`` ve ``__new__()`` birer sihirli metoddur.
Peki ``__init__`` metodunu diğer örnek metotlarından ayıran ne gibi özellikler var?
Bir kere ``__init__`` metodu python tarafından özel bir iş için ayrılmıştır. 
Ki bu özellik 'ilklendirmedir'. Aynı bunun gibi bizzat python tarafından özel görevler
verilmiş metotlara sihirli metotlar diyoruz.

Sihirli metotlar çoğunlukla söz dizimi (syntax) ile veya ifadeler ile alakalıdır.
Örneğin ``__new__`` metodu ile bir örnek verelim::

	class Deneme:
		def __new__(self,*args,**kwargs):
			return object.__new__(*args,**kwargs)

		def __init__(self,*args,**kwargs):
			pass

	örnek = Deneme()

Aslında burada ``Deneme`` sınıfını çağırdığımızda ``Deneme.__new__`` *python tarafından* 
çağırılmış oluyor. Ama yukarıdaki örneğimizde ``__init__`` çağırılmıyor. Onu kendimiz 
yapmak zorunda kalıyoruz::

	örnek.__init__()

İşte sihirli metotlar, bunun gibi şeyleri yapmakla uğraşmayalım diye
söz dizimini kısaltıp arkada bizden habersiz işler çeviriyorlar. Yani
normalde sadece ``Deneme()`` yazdığınızda ``__new__``, dolayısıyla da ``__init__`` çağırılmış
oluyor. Metasınıfları işlediğimizde bu örneği daha iyi anlayacağınızı düşünüyorum.
Şimdi daha basit ama daha önce bahsetmediğimiz bir örnek verelim::

	>>> "a"+"b"
	'ab'
	>>> "a".__add__("b")
	'ab'

Evet, burada neler döndüğünü yavaş yavaş anlamaya başlıyoruz. Aslında biz her
``+`` işlecini kullandığımızda, *python ``__add__`` sihirli metodunu çağırıyor*.
**Aynı bunun gibi her bir işlece karşılık gelen, yani pythonun o işleci gördüğünde çağırdığı bir fonksiyon var.**
Bu da dediğimiz gibi söz dizimini kolaylaştırıyor ve kodun okunulabilirliğini arttırıyor. 
Her seferinde ``__add__`` yazmak yerine ``+`` yazıyoruz.

Ayrıca sihirli metotlar yazılımcılara, dilin içinde barındırdığı işleçler , ifadeler ve fonksiyonlar
üzerinde daha iyi kontrol sahibi olabilmeleri için yol açar. Bu bölümü bitirdiğinizde
bu cümle ile ne kastetmek istediğimi anlayacağınızı düşünüyorum.

Sihirli metotların ne için kullanıldığını anladığımıza göre şimdi hangisinin ne 
yaptığını ayrıntıları ile inceleyebiliriz.

Sihirli Methodlar
*****************

``__init__`` ve ``__new__`` metotları hakkında önceki bölümlerde yeterince konuştuğumuz 
için bu bölümde onlara değinmeyeceğiz.


``__repr__`` Metodu
======================

Bu metot ``repr`` fonksiyonu tarafından çağırılmaktadır. Örneğin::

	>>> a = [1,2,3,4,5]
	>>> repr(a)
	'[1, 2, 3, 4, 5]'
	>>> a.__repr__()
	'[1, 2, 3, 4, 5]'

Peki ne işe yarar bu ``repr`` fonksiyonu? Bundan daha önce burada_
.. _buraya: https://belgeler.yazbel.com/python-istihza/karakter_kodlama.html#repr
bahsetmiştik ama yine de kısa bir şekilde değineceğim. Aslında biz
etkileşimli kabuğa bir şey yazdığımızda::

	>>> a
	[1, 2, 3, 4, 5]

burada Python'un yaptığı şu komutu işlemektir::

	>>> print(repr(a))
	[1, 2, 3, 4, 5]

Veya da bunu::

	>>> print(a.__repr__())
	[1, 2, 3, 4, 5]

Sonuçta ``__repr__`` metodu ``repr`` fonksiyonu tarafından çağırıldığı için bunu
kendi yazdığımız sınıfların örneklerini etkileşimli kabukta daha iyi bir
şekilde görebilmek için kullanabiliriz. Örneğin şöyle bir sınıfımız ve
bu sınıfa ait bir örneğimiz bulunsun::

	class Öğrenci:
		def __init__(self,isim,yaş):
			self.isim = isim
			self.yaş = yaş

	örnek = Öğrenci("Ahmet",12)

.. note:: Unutmaylım ki şuanda Öğrenci sınıfımız biz yazmasak da object sınıfını miras
		  alıyor. Bu yüzden biz tanımlamasak da şuan Öğrenci sınıfımız bir __repr__
		  metoduna sahip ve bu da object sınıfından miras alınan __repr__ metodudur.

Daha sonra programı çalıştırdığımızda etkileşimli kabuğa ``örnek`` yazarsak pek iç
açıcı bir çıktı almıyoruz::

	>>> örnek
	<__main__.Öğrenci object at 0x00000264884B5488>
	>>> id(örnek)
	2630806623368
	>>> hex(id(örnek))
	'0x264884b5488'

Etkileşimli kabuğa ``örnek`` yazmamız bize sadece değişkenimizin 
ID'sinin 16'lık sistemdeki halini veriyor. Eğer biz öğrencimizin isim veya yaşı
 gibi işe yarar nitelikleri göstermek istersek kendimiz bu sınıfa ``__repr__`` 
metodu ekleyerek bunu yapabiliriz::

	class Öğrenci:
		def __init__(self,isim,yaş):
			self.isim = isim
			self.yaş = yaş

		def __repr__(self):
			return "isim: {}, yaş: {}".format(self.isim,self.yaş)

	örnek = Öğrenci("Ahmet",12)

Artık programı çalıştırıp etkileşimli kabuğa ``örnek`` yazdığımızda işe yarar bir sonuç 
alıyoruz::

	>>> örnek
	isim: Ahmet, yaş: 12

.. note:: Herhangi bir sınıfın '__repr__' metodunun döndürdüğü değişkeninin türü 'str'
	      olmak zorundadır. Aksi taktirde 'repr' fonksiyonu kullanıldığında
	      TypeError' hatası yükselecektir.

İşleç Metotları
================

Bu bölümde işleçler ile alakalı metotları inceleyeceğiz.


Aritmetik İşleç Metotları
--------------------------

Bu bölümde aritmetik işleçler ile alakalı metotları inceleyeceğiz.

``__add__`` , ``__radd__`` ve ``__iadd__`` Metotları
......................................................

Bu metotların üçü de toplama işlemi ile alakalıdır.

Konunun başında da gördüğümüz gibi ``__add__`` metodu ``+`` işleci gibi çalışmaktadır,
*aslında python ``+`` işlecini gördüğünde ``__add__`` metodunu çağırmaktadır*::

	>>> "a"+"b"
	'ab'
	>>> "a".__add__("b")
	'ab'

``__radd__`` ve ``__iadd__`` metotlarını anlayabilmek için ise kendi sınıfımızı yazmamız
daha iyi olacak. Madem matematik işlemleri yapacağız, iki boyutlu bir vector 
sınıfı yazalım::

	class Vector:
		def __init__(self,x,y):
			self.x = x
			self.y = y

		def __add__(self,other):
			return Vector(self.x + other.x , self.y + other.y)

		def __repr__(self):
			return "Vector({},{})".format(self.x,self.y)


.. note:: Vektörlerin ne olduğunu bilmiyorsanız endişelenmenize gerek yok. Basitçe
		  anlatmak gerekirse her boyut için bir sayısal büyüklüğe sahip olduklarını 
		  söyleyebiliriz. Örneğin yazacağımız vektör iki boyutlu olacağı için x ve y 
		  değerlerine sahip olacak.

Sınıfımızı yazdık. Şimdi biraz deneme yapalım::

	>>> b = Vector(1,2)
	>>> a = Vector(3,-4)
	>>> b
	Vector(1,2)
	>>> a
	Vector(3,-4)
	>>> a+b # şunun ile aynı : Vector.__add__(a,b) veya a.__add__(b)
	Vector(4,-2)

Yukarıda ``__add__`` metodunu kendi sınıflarımızda nasıl kullanabileceğimizi
gördük. Peki şöyle bir şey yapmak istersek ne yapmalıyız::

	>>> a+1
	AttributeError: 'int' object has no attribute 'x'

Örneğin biz burada Vector(4,-3) değerini almak yani vektörümüzün hem ``x`` hem de ``y`` 
değerini verilen sayı kadar arttırmak istersek ``__add__`` fonksiyonumuz şu hale getirebiliriz::

	def __add__(self,other):
        if type(other)==Vector:
            return Vector(self.x + other.x , self.y + other.y)
        elif type(other)== int or type(other)== float:
            return Vector(self.x + other,self.y + other)

Şimdi ``Vector`` örneklerimizi ``int`` ve ``float``'lar ile de toplayabiliyoruz::

	>>> a = Vector(3,-4)
	>>> a + 1 # şunun ile aynı : Vector.__add__(a,1) veya a.__add__(1)
	Vector(4,-3)
	>>> a + 2.5 # şunun ile aynı : Vector.__add__(a,2.5) veya a.__add__(2.5)
	Vector(5.5,-1.5)

	>>> 1 + a
	TypeError: unsupported operand type(s) for +: 'int' and 'Vector'

Her şey yolunda iken en sonda hata aldık. Peki bunun sebebi ne? Gelin daha ayrıntılı 
bakalım::

	>>> (1).__add__(a)
	NotImplemented

.. note:: Burada 1'i parantez içine almamızın sebebi 1.__add__ yazarsak pythonun '1.'i
		bir float tanımlaması sanarak ondan sonra gelen '_' işaretini görünce SyntaxError
		hatası verecek olmasıdır. Şunun gibi de düşünebilirsiniz: '1._' , SyntaxError verir.

Gördüğünüz gibi ``int`` sınıfını oluşturan programcılar (normal olarak) bizim ``Vector``
sınıfımızı düşünmemişler. Bu yüzden ``NotImplemented`` (uygulanamadı gibi bir anlama geliyor)
değerini döndürüyorlar. İşte burada da imdadımıza ``__radd__`` yetişiyor. 
'Reflection add'ın kısaltması olan ``__radd__`` metodu;
ilk nesnenin, yani örneğimizde ``1``'in ``__add__`` methodu
``NotImplemented`` döndürdüğünde python tarafından ikinci nesnede, yani örneğimizde
``a`` da aranır. Tabii bu ``+`` işleci kullanıldığında gerçekleşir, ``__add__``
fonksiyonunu tek başına çağırdığımızda değil. Eğer ikinci nesnede de
``__radd__`` tanımlanmamış ise, veya o da ``NotImplemented`` döndürüyorsa Python
bize aynı burada olduğu gibi ``TypeError`` hatası verecektir::

	>>> 1 + a
	TypeError: unsupported operand type(s) for +: 'int' and 'Vector'

Şimdi vektör sınıfımızda ``__radd__`` tanımlayalım::

		def __radd__(self,other):
			if type(other) == int or type(other) == float:
				return Vector(self.x + other,self.y + other)

Burada ``Vector`` tipi için bir daha kontrol eklemedik çünkü zaten iki nesnemiz de ``Vector`` 
türünde ise ilk nesnenin ``__add__`` metodu başarı ile çalışacaktır. Artık şu işlemi de
yapabiliriz::

	>>> a = Vector(1,2)
	>>> 1 + a # şununla aynı işi yapıyor : Vector.__radd__(a,1)
	Vector(2,3)

	>>> (1).__add__(a)
	NotImplemented
	>>> a.__radd__(1)
	Vector(2,3)

Şu anda herşey yolunda gibi gözüküyor ama bir eksiğimiz var::

	>>> "a" + Vector(1,2)
	>>> 

Normalde burada hata verilmesi gerekiyordu. Peki neden verilmedi? Daha dikkatli bakalım::

	>>> "a".__add__(Vector(1,2))
	>>> TypeError: can only concatenate str (not "Vector") to str
	>>> Vector(1,2).__radd__("a")
	>>>

Gördüğünüz gibi ilk işlem ``TypeError`` yükseltiyor ancak python bu hatayı yakalıyor ve
daha sonra ``Vector.__radd__`` metodunu deniyor. Bu metot hiçbir şey döndürmüyor, yani aslında
``None`` döndürüyor. Python, hem birinci nesnede ``__add__``, hem de ikinci nesnede ``__radd__`` 
metodları bulunduğundan her ikisinden de beraber işlemin geçersiz olduğuna dair bir değer dönmez ise
``TypeError`` yükseltmiyor. Bizim ``Vector.__radd__`` metodu istediğimiz şekilde çalışmadığında
``NotImplemented`` döndürmemiz, Python'un da ``TypeError`` yükseltmesine sebep olacaktır.
Bu, programımızdaki hataları yakalamamız için bize kolaylık sağlayacaktır. Aynı şey
``__add__`` fonksiyonu için de geçerlidir. Şimdi Vector sınıfımızın tamamını bir gözden
geçirelim::

	class Vector:
    	def __init__(self,x,y):
        	self.x = x
        	self.y = y

    	def __add__(self,other):
        	if type(other)==Vector:
            	return Vector(self.x + other.x , self.y + other.y)
        	elif type(other)== int or type(other)== float:
            	return Vector(self.x + other,self.y + other)
            else:
            	return NotImplemented

   		def __radd__(self,other):
        	if type(other) == int or type(other) == float:
             	return Vector(self.x + other,self.y + other)
            else:
            	return NotImplemented


    	def __repr__(self):
        	return "Vector({},{})".format(self.x,self.y)

Artık geçersiz bir işlem denedeğimizde hata alacağız::

	>>> "a" + Vector(1,2)
	TypeError: can only concatenate str (not "Vector") to str
	>>> Vector(1,2) + "a" 
	TypeError: can only concatenate str (not "Vector") to str
	>>> Vector(1,2).__add__("a")
	NotImplemented
	>>> Vector(1,2).__radd__("a")
	NotImplemented

Evet, artık her şey gerektiği gibi çalışıyor. Hem ``str.__add__`` hem de 
``vector.__radd__`` metotlarının işlemin gerçekleşemeyeceğine dair
bir değer döndürmesi (``NotImplemented`` değeri) veya bir ``TypeError``
yükseltmesi Python'un da bize ``TypeError`` ile geri dönmesine sebep oluyor.
Unutmayalım ki bunları programımızda bir hata olduğunda bunun sessizlikte kaybolması
yerine bizim de haberimizin olması için yaptık. Yoksa hataları bulmak
(özellikle büyük programlarda) çok zor olabilir.

Şimdi ``__add__`` ve ``__radd__`` ile olan işimizi bitirdiğimize göre ``__iadd__``'dan da 
bahsedebiliriz. Şöyle bir örnekle başlayalım::

	>>> a = Vector(1,2)
	>>> a += Vector(1,0)
	>>> a
	Vector(2,2)

Burada ``+=`` işleci aslında şu şekilde çalışıyor::

    >>> a = Vector(1,2)
	>>> a = a + Vector(1,0)
	>>> a
	Vector(2,2)

	>>> a = a.__add__(Vector(1,0))
	>>> a
	Vector(3,2)

Şöyle söyleyelim, Vector sınıfımızda ``__iadd__`` metodu bulunmadığı için ``+=`` işleci
``__add__`` metodundan faydalanıyor. Ama eğer Vector sınıfımızda ``__iadd__`` metodu 
bulunursa ``+=`` işleci ilk olarak onu çağıracaktır. Bu özellikten; ``+=`` işlecinin, ``+`` işlecinden
farklı çalışmasını istediğimiz yerlerde faydalanabiliriz. ``Vector`` sınıfımızda böyle bir şeye 
gerek yok ama yine de ``__iadd__`` metodunu anlamak için onu da ekleyip birkaç örnek verelim::

		def __iadd__(self,other):
			print("__iadd__ çağırıldı.")
			return self.__add__(other)

Şimdi ``+=`` işlecini deneyelim::

	>>> a = Vector(1,2)
	>>> a += 1
	__iadd__ çağırıldı.
	>>> a
	Vector(2,3)

	>>> a.x = 1 ; a.y = 2
	>>> a = a.__iadd__(1)
	__iadd__ çağırıldı.
	>>> a
	Vector(2,3)

Şonuç olarak Vector sınıfımız şöyle gözüküyor::

	class Vector:
	    def __init__(self,x,y):
        self.x = x
        self.y = y

    	def __add__(self,other):
        	if type(other)==Vector:
            	return Vector(self.x + other.x , self.y + other.y)
        	elif type(other)== int or type(other)== float:
            	return Vector(self.x + other,self.y + other)
        	else:
            	return NotImplemented

    	def __radd__(self,other):
        	if type(other) == int or type(other) == float:
            	return Vector(self.x + other,self.y + other)
        	else:
            	return NotImplemented

    	def __iadd__(self,other):
        	print("__iadd__ çağırıldı.")
        	return self.__add__(other)

    	def __repr__(self):
        	return "Vector({},{})".format(self.x,self.y)

Bu bölümde her şeyi yavaş yavaş ve sindirerek anlamaya çalıştık. Artık diğer işleç
metotlarında hızlıca ilerleyebiliriz. 


``__sub__`` , ``__rsub__`` ve ``__isub__`` Metotları
........................................................

Bu metotların üçü de çıkarma işlemi ile alakalıdır.

Bir örnekle başlayalım::

	>>> a = 1
	>>> b = 2
	>>> b-a
	1
	>>> b.__sub__(a)
	1

Gördüğünüz gibi ``__sub__`` sihirli metodu ``-`` işleci tarafından çağırılmaktadır.

Bir önceki başlıkta olayın mantığını zaten kavradık. Şimdi ``Vector`` sınıfımıza hızlıca 
``__sub__`` metodunu ekleyelim::

		def __sub__(self,other):
			if type(other) == Vector:
				return Vector(self.x-other.x , self.y-other.y)
			elif type(other) == int or type(other) == float:
				return Vector(self.x - other , self.y - other)
			else:
				return NotImplemented

Şimdi de birkaç örnek yapalım::

	>>> a = Vector(5,3)
	>>> b = Vector(2,4)
	>>> a - b
	Vector(3,-1)
	>>> a.__sub__(b)
	Vector(3,-1)
	>>> b - a
	Vector(-3,1)
	>>> a - 2
	Vector(3,1)
	>>> b - 1
	Vector(1,3)

	>>> 1 - b
	TypeError: unsupported operand type(s) for -: 'int' and 'Vector'
	>>> (1).__sub__(b)
	NotImplemented

Gördüğünüz gibi son örnek hariç hepsi doğru çalıştı. Şimdi de ``__rsub__`` metodunu 
ekleyelim::

		def __rsub__(self,other):
			if type(other) == int or type(other) == float:
				return Vector(other - self.x , other - self.y)
			else:
				return NotImplemented

``__radd__``'da da yaptığımız gibi ``__rsub__``'a da ``if type(other) == Vector`` eklemedik
çünkü iki nesne de ``Vector`` sınıfının örneği ise ``__sub__`` metodu başarıyla çalışacaktır.
Ayrıca ``__rsub__``'da ``__sub__``'daki ``self.x - other , self.y - other`` bölümünün aksine
``other - self.x , other - self.y`` kullandığımıza dikkat edin. Çünkü bu sefer ``self`` nesnemiz
çıkarma işleminde çıkan , ``other`` ise eksilen olmuş oluyor.
Şimdi birkaç örnek verelim::

	>>> 1 - Vector(1,1)
	Vector(0,0)
	>>> 2 - Vector(5,3)
	Vector(-3,-1)
	>>> (2).__sub__(Vector(1,2))
	NotImplemented
	>>> Vector(1,2).__rsub__(2)
	Vector(1,0)

Şimdi de ``__isub__`` metodunu ekleyelim. Aslında aynı ``__iadd__``'deki gibi buna da ihtiyacımız yok
çünkü ``__isub__`` yerine (tanımlanmış ise) ``__sub__`` çalışır::

	>>> a = Vector(1,2)
	>>> a -= Vector(1,0)
	>>> a
	Vector(0,2)

Biz yine de ``__isub__`` tanımlayalım::

		def __isub__(self,other):
			print("__isub__ çağırıldı.")
			return self.__sub__(other)


	>>> a = Vector(3,4)
	>>> a -= 2
	__isub__ çağırıldı.
	>>> a
	Vector(1,2)

Şimdi sırada çarpma işlemi var.


``__mul__`` , ``__rmul__`` ve ``__imul__`` Metotları
......................................................

Bu üç metod da çarpma işlemi ile alakalıdır.

``__mul__`` methodu ``*`` işleci , ``__imul__`` methodu da ``*=`` işleci için çağırılmaktadır.
``__imul__`` methodu bulunmazsa onun yerine ``__mul__`` çağırılır.
``Vector``  sınıfımız için bu metotları tanımlayalım::

		def __mul__(self , other):
			if type(other) == Vector:
				return Vector(self.x * other.x , self.y * other.y)
			elif type(other) == int or type(other) == float:
				return Vector(self.x * other , self.y * other)
			else:
				return NotImplemented

		def __rmul__(self , other):
			if type(other) == int or type(other) == float:
				return Vector(self.x * other , self.y * other)
			else:
				return NotImplemented

		def __imul__(self , other):
			print("__imul__ çağırıldı.")
			return self.__mul__(other)

Artık ``Vector`` sınıfımız üzerinde ``*`` işlecini kullanabiliriz::

	>>> a = Vector(2,3)
	>>> b = Vector(4,2)
	>>> a * b
	Vector(8,6)
	>>> a * 2
	Vector(4,6)
	>>> 2 * a
	Vector(4,6)

	>>> a
	Vector(2,3)
	>>> a *= 3
	__imul__ çağırıldı.
	>>> a
	Vector(6,9)


``__truediv__`` , ``__rtruediv__`` ve ``__itruediv__`` Metotları
..................................................................

Bu methodların üçü de bölme işlemi ile alakalıdır.

``__truediv__``, ``/`` işleci için , ``__itruediv__`` de ``/=`` işleci için çağırılmaktadır.
``__itruediv__`` methodu bulunmazsa onun yerine ``__truediv__`` çağırılır.
``Vector`` sınıfımızda bu metotları tanımlayalım::

		def __truediv__(self , other):
			if type(other) == Vector:
				return Vector(self.x / other.x , self.y / other.y)
			elif type(other) == int or type(other) == float:
				return Vector(self.x / other , self.y / other)
			else:
				return NotImplemented

		def __rtruediv__(self , other):
			if type(other) == int or type(other) == float:
				return Vector(other / self.x  , other / self.y)
			else:
				return NotImplemented

		def __itruediv__(self , other):
			print("__itruediv__ çağırıldı.")
			return self.__truediv__(other)

Dikkat ederseniz ``__truediv__``'de ``self.x / other , self.y / other`` yazarken 
``__rtruediv__``'de ``other / self.x  , other / self.y`` yazdık. Çünkü ``__rtruediv__``
çağırıldığında ``other`` işlemin solundaki bölünen, ``self`` ise sağdaki bölen oluyor.

Artık ``Vector`` sınıfımız üzerinde ``/`` işlecini de kullanabiliriz::

	>>> a = Vector(9,6)
	>>> b = Vector(3,2)
	>>> a / b
	Vector(3.0,3.0)
	>>> b / 2
	Vector(1.5,1.0)
	>>> 3 / b
	Vector(1.0,1.5)

	>>> a
	Vector(9,6)
	>>> a /= 3
	__itruediv__ çağırıldı.
	>>> a
	Vector(3.0,2.0)



``__floordiv__`` , ``__rfloordiv__`` ve ``__ifloordiv__`` Metotları
......................................................................

Bu methodların üçü de tam bölme işlemi ile alakalıdır.

``__floordiv__``, ``//`` işleci için , ``__ifloordiv__`` de ``//=`` işleci için çağırılmaktadır.
``__ifloordiv__`` methodu bulunmazsa onun yerine ``__floordiv__`` çağırılır.
``Vector`` sınıfımızda bu metotları tanımlayalım::

		def __floordiv__(self , other):
			if type(other) == Vector:
				return Vector(self.x // other.x , self.y // other.y)
			elif type(other) == int or type(other) == float:
				return Vector(self.x // other , self.y // other)
			else:
				return NotImplemented

		def __rfloordiv__(self , other):
			if type(other) == int or type(other) == float:
				return Vector(other // self.x  , other // self.y)
			else:
				return NotImplemented

		def __ifloordiv__(self , other):
			print("__ifloordiv__ çağırıldı.")
			return self.__floordiv__(other)

Dikkat ederseniz ``__floordiv__``'de ``self.x // other , self.y // other`` yazarken 
``__rfloordiv__``'de ``other // self.x  , other // self.y`` yazdık. Çünkü ``__rfloordiv__``
çağırıldığında ``other`` işlemin solundaki bölünen, ``self`` ise sağdaki bölen oluyor.

Artık ``Vector`` sınıfımız üzerinde ``//`` işlecini de kullanabiliriz::

	>>> a = Vector(5,3)
	>>> b = Vector(2,1)
	>>> a // b
	Vector(2,3)
	>>> 3 // b
	Vector(1,3)
	>>> a //= 2
	__ifloordiv__ çağırıldı.
	>>> a
	Vector(2,1)


``__mod__`` , ``__rmod__`` ve ``__imod__`` Metotları
......................................................

Bu methodların üçü de modülo işlemi ile alakalıdır.

``__mod__``, ``%`` işleci için , ``__imod__`` de ``%=`` işleci için çağırılmaktadır.
``__imod__`` methodu bulunmazsa onun yerine ``__mod__`` çağırılır.
``Vector`` sınıfımızda bu metotları tanımlayalım::

		def __mod__(self , other):
			if type(other) == Vector:
				return Vector(self.x % other.x , self.y % other.y)
			elif type(other) == int or type(other) == float:
				return Vector(self.x % other , self.y % other)
			else:
				return NotImplemented

		def __rmod__(self , other):
			if type(other) == int or type(other) == float:
				return Vector(other % self.x  , other % self.y)
			else:
				return NotImplemented

		def __imod__(self , other):
			print("__imod__ çağırıldı.")
			return self.__mod__(other)

Dikkat ederseniz ``__mod__``'da ``self.x % other , self.y % other`` yazarken 
``__rmod__``'da ``other % self.x  , other % self.y`` yazdık. Çünkü ``__rmod__``
çağırıldığında ``other`` işlemin solundaki eleman, ``self`` ise sağdaki eleman oluyor.

Artık ``Vector`` sınıfımız üzerinde ``%`` işlecini de kullanabiliriz::

	>>> a = Vector(5,3)
	>>> b = Vector(2,2)
	>>> a % b
	Vector(1,1)
	>>> 3 % a
	Vector(3,0)
	>>> a %= 4
	__imod__ çağırıldı.
	>>> a
	Vector(1,3)


``__pow__`` , ``__rpow__`` ve ``__ipow__`` Metotları
.......................................................

Bu methodların üçü de üs alma işlemi ile alakalıdır.

``__pow__``, ``**`` işleci için , ``__imod__`` de ``**=`` işleci için çağırılmaktadır.
``__ipow__`` methodu bulunmazsa onun yerine ``__pow__`` çağırılır.
``Vector`` sınıfımızda bu metotları tanımlayalım::

		def __pow__(self , other):
			if type(other) == Vector:
				return Vector(self.x ** other.x , self.y ** other.y)
			elif type(other) == int or type(other) == float:
				return Vector(self.x ** other , self.y ** other)
			else:
				return NotImplemented

		def __rpow__(self , other):
			if type(other) == int or type(other) == float:
				return Vector(other ** self.x  , other ** self.y)
			else:
				return NotImplemented

		def __ipow__(self , other):
			print("__ipow__ çağırıldı.")
			return self.__pow__(other)

Dikkat ederseniz ``__pow__``'da ``self.x ** other , self.y ** other`` yazarken 
``__rpow__``'da ``other ** self.x  , other ** self.y`` yazdık. Çünkü ``__rpow__``
çağırıldığında ``other`` işlemin solundaki taban, ``self`` ise sağdaki üs oluyor.

Artık ``Vector`` sınıfımız üzerinde ``**`` işlecini de kullanabiliriz::

	>>> a = Vector(3,4)
	>>> b = Vector(3,2)
	>>> a ** b
	Vector(1,1)
	>>> 4 ** a
	Vector(64,256)
	>>> a **= 2
	__ipow__ çağırıldı.
	>>> a
	Vector(9,16)



İşaret Metotları (Unary Methods)
---------------------------------

Bu metotlar bir değişkenin önüne ``+`` veya ``-`` işareti konduğunda çağırılır. 
Ancak bunları toplama ve çıkarma işlemi ile karıştırmamak lazım.
Şöyle bir örnek verelim::

	>>> a = 3
	>>> a
	3
	>>> -a # a.__neg__()
	-3
	>>> +a # a.__pos__()
	3
	>>> 0 - a # (0).__sub__(a)
	-3
	>>> 0 + a # (0).__add__(a)
	3

	>>> b = -2
	>>> -b # b.__neg__()
	2
	>>> +b # b.__pos__()
	-2
	>>> 0 + b # (0).__add__(b)
	-2
	>>> 0 - b # (0).__sub__(b)
	2

Yukardaki bazı ifadelerin sonuçları birbirleri ile aynı da olsa
 çağırdıkları fonksiyonlar farklıdır.

``__neg__`` Metodu
....................

Yukarıdaki örnekte de gösterdimiz gibi bir değişkenin önüne ``-`` işareti gelince çağırılır.
Bunu ``Vector`` sınıfımıza ekleyelim. Yapmak istediğimiz şey hem ``x`` hem de ``y`` değerini '-1' ile
çarpıp yeni bir ``Vector`` örneği döndürmek::

		def __neg__(self):
			return Vector(-self.x , -self.y) # bunu "return Vector(self.x.__neg__() ,self.y.__neg__())" olarak da yazabilirdik.

Bir örnek verelim::

	>>> a = Vector(2,3)
	>>> -a
	Vector(-2,-3)

	>>> b = Vector(-1,2)
	>>> -b
	Vector(1,-2)

Kendi sınıflarınıza uygularken istediğiniz gibi yapabilirsiniz ancak ``Vector`` örneğimizde ve
``int`` sınıfında, ``__neg__`` metodunun sayıları her zaman negatif hale getirmediğini, sadece
'-1' ile çarpmış gibi işaretini ters çevirdiğine dikkat edin.


``__pos__`` Metodu
....................

Yukarıdaki örnekte de gösterdimiz gibi bir değişkenin önüne ``+`` işareti gelince çağırılır.
Bunu da ``Vector`` sınıfımıza ekleyelim. Yapmak istediğimiz şey aynı vektörün kopyasını döndürmek
çünkü '+1' çarpmada etkisiz elemandır::

		def __pos__(self):
			return Vector(self.x , self.y) 

Bir örnek verelim::

	>>> a = Vector(3,-2)
	>>> +a
	Vector(3,-2)

	>>> b = Vector(1,1)
	>>> +b
	Vector(1,1)

Kendi sınıflarınıza uygularken istediğiniz gibi yapabilirsiniz ancak ``Vector`` örneğimizde ve
``int`` sınıfında, ``__pos__`` metodunun sayıları pozitif hale getirmediğini, sadece
'+1' bir ile çarpılmış gibi aynı halini döndürdüğüne dikkat edin.




Karşılaştırma İşleçleri Metotları
----------------------------------

Bu bölümde karşılaştırma işleçleri ile alakalı sihirli metotları
inceleyeceğiz.


``__eq__`` ve ``__ne__`` Metotları
....................................

Bu metotlar ingilizce *equal* ve *not equal* kelimelerin kısaltmasıdır.
``==`` ile ``!=`` işleçleri bu metotları çağırmaktadır. Burada ``__radd__``'da
olduğu gibi bir yansıma (*reflection*) metoduna sahip değiliz. Bu iki metot için
herbiri kendisinin yansımasıdır diyebiliriz. Yani ``a`` ve ``b``
adında iki değişkenimiz olduğunu düşündüğümüzde::

	>>> a == b

durumunda ilk önce ``a.__eq__(b)`` metodu çağırılır. Eğer bu ``NotImplemented``
değeri döndürüse daha sonra da ``b.__eq__(a)`` metodu denenir. Eğer bu da 
``NotImplemented`` değeri döndürürse Python ``a is b`` ifadesinin
değerini döndürür. Bunu şu şekilde kısa bir deneme ile görebiliriz::

	>>> class sınıf:
			def __eq__(self,other):
				return NotImplemented

	>>> a = sınıf()
	>>> b = a
	>>> c = sınıf()
	>>>
	>>> a is b
	True
	>>> a == b
	True
	>>> a.__eq__(b)
	NotImplemented
	>>> b.__eq__(a)
	NotImplemented
	>>>
	>>> a is c
	False
	>>> a == c
	False

Burada Python'un hem ``a.__eq__(b)`` hem de ``b.__eq__(a)`` metodu ``NotImplemented`` döndürmesi
durumunda bir hata yükseltmek yerine ``a is b``veya ``id(a)==id(b)`` işlemini yaptığını ve bunun değerini
döndürdüğünü görebiliriz. Aslında sınıfımızda bir ``__eq__`` metodu tanımlamadığımızda 
``object`` sınıfından miras alınan ``__eq__`` metodu da iki nesnenin ID'lerini karşılaştırarak
değer döndürür. Yani bu ``==`` işlecinin nasıl çalıştığını şu şekilde özetleyebiliriz::

	a == b

İşlemi ile şu işlem aynıdır::

	def equal(a,b):
		sonuç = a.__eq__(b)
		if sonuç != NotImplemented:
			return sonuç
		else:
			sonuç = b.__eq__(a)
			if sonuç != NotImplemented:
				return sonuç
			else:
				return a is b

Şimdi ``__eq__`` metodunu ``Vector`` sınıfımıza ekleyelim. Yapmak istediğimiz şey
hem ``x`` hem de ``y`` niteliği aynı ise ``True``, değilse ``False``, eğer nesnenin
türü ``Vector`` değilse de ``NotImplemented`` döndürmek::

		def __eq__(self,other):
			if type(other) == Vector:
				return self.x == other.x and self.y == other.y
			else:
				return NotImplemented

	>>> a = Vector(1,2)
	>>> b = Vector(2,3)
	>>> c = Vector(1,2)
	>>> a == c
	True
	>>> b == c
	False

Şimdi ``!=`` işlecinin çağırdığı ``__ne__`` metodunun da şu şekilde çalıştığını
söyleyebiliriz::

	a != b

İşlemi aslında şu şekilde çalışır::

	def not_equal(a,b):
		sonuç = a.__ne__(b)
		if sonuç != NotImplemented:
			return sonuç
		else:
			sonuç = not a.__eq__(b)
			if sonuç != NotImplemented:
				return sonuç
			else:
				sonuç = b.__ne__(a)
				if sonuç != NotImplemented:
					return sonuç
				else:
					sonuç = not b.__eq__(a)
					if sonuç != NotImplemented:
						return sonuç
					else:
						return a is not b



.. note:: Burada kod çok karmaşık olacağı için nesnemizde ``__ne__`` metodununun bulunup
		  bulunmadığı kontrol etmedik. Eğer bulunmaz ise yukarıdaki kod ``a.__ne__(b)``
		  bölümünde hata verecektir. Normalde nesnenin bu metoda sahip olup olmadığı
		  ``hasattr`` ve ``callable`` fonksiyonları ile kontrol edilir. Bunun tam halini
		  sihirli metotlar konumuzun son başlığında bulabilirsiniz.

Gördüğünüz gibi iki nesnemizde de ``__ne__`` metodu çalışamadığında (``NotImplemented``
döndürdüğünde) veya bulunmadığında, ``__eq__`` metoduna bakılıyor ve tersi döndürülüyor. 
Eğer ``__eq__`` metodu da ikisinde de çalışamaz ise ``a is not b`` işlemi çalıştırılıyor. 
Ayrıca ``__ne__`` metodu ``object`` sınıfında bulunmadığı için miras alınmaz. ``Vector``
sınıfımız için ``!=`` işleci ``__ne__`` metodunu bulunamayınca ``__eq__`` metodunu çağıracaktır 
ve döndürdüğü değer ``True`` ise ``False``, ``False`` is ``True`` döndürecektir.


``__lt__`` ve ``__gt__`` Metotları
....................................

Bu metotlar ingilizce *litter than* ve *greater than* kelimelerin kısaltmasıdır.
``<`` ile ``>`` işleçleri bu metotları çağırmaktadır.


``__le__`` ve ``__ge__`` Metotları
....................................

Bu metotlar ingilizce *little than or equal* ve *greater than or equal* kelimelerin kısaltmasıdır.
``<=`` ile ``>=`` işleçleri bu metotları çağırmaktadır.




Aitlik İşleci Metodu
----------------------

Bildiğiniz gibi Python'da bir tane aitlik işleci bulunur bu da ``in`` işlecidir.
Hatırlama amacıyla şöyle bir örnek ile başlayalım::

	>>> listem = [1,2,3,4]
	>>> 1 in listem
	True
	>>> 5 in listem
	False

Tahmin edebileceğiniz gibi bu ``in`` işleci de bir sihirli metodu çağırıyor.
O da ``__contains__`` metodudur. Gene bir örnek verelim::

	>>> listem = [1,2,3]
	>>> listem.__contains__(1)
	True
	>>> listem.__contains__(4)
	False

Artık ``in`` işlecinin nasıl çalıştığını öğrendiğimize göre kendi sınıflarımızı da 
bu işleç ile çalışacak şekilde tasarlayabiliriz. Ancak dikkat edeceğimiz 
iki şey var:
	* ``__contains__`` metodu iki parametre alır. Bunların biri ``self``'dir. Diğeri de nesnemizin içinde olup olmadığını kontrol edeceğimiz ``other``'dır. Tabii ki bu parametrelerin isimlerini değiştirebilirsiniz ancak ikisi de Python topluluğu içerisinde kabul görmüş isimlerdir. Özellikle sihirli metotların çoğunda ikinci parametre ``other`` olarak adlandırılmaktadır.
	* ``in`` işleci kullanarak ``__contains__`` metodumuzdan döndüreceğimiz değer her zaman ``bool`` türüne dönüştürülerek bize geri verilecektir.

Şimdi bu methodu ``Vector`` sınıfımıza ekleyelim. Yapmak istediğimiz şey verilen değer,
örneğimizin ``x`` veya ``y`` değerine eşit ise ``True``, değil ise ``False`` döndürmek::

		def __contains__(self,other):
			if self.x == other or self.y == other:
				return True
			return False

Şimdi de bir örnek verelim::

	>>> a = Vector(1,2)
	>>> 1 in a
	True
	>>> 2 in a
	True
	>>> 3 in a
	False



Fonksiyon Metotları 
====================

Python'da sihirli fonksiyon metotları, gömülü fonksiyonlar tarafından çağırılan
metotlardır. Örneğin ilk başta işlediğimiz ``__repr__`` metodu da bu gruptandır.
Ancak ``Vector`` sınıfımızda bunu hep kullandığımız için bunu en başta anlatmayı 
tercih ettim.

Fonksiyon metotlarının çoğu ``'__' + fonksiyon_ismi + '__'`` şeklinde adlandırılmıştır.
Yine bir kaç örnek verelim::

	>>> listem = [1,2,3]
	>>> len(listem)
	3
	>>> listem.__len__()
	3

Gördüğünüz gibi ``len`` fonksiyonu aldığı parametrenin ``__len__`` methodunu çağırmaktadır.
Bundan faydalanarak kendi sınıflarımızı da ``len`` fonksiyonu ile çalışacak hale 
getirebiliriz. Bu konunun anlaşılır olduğunu düşündüğüm için ve çok fazla fonksiyon
metodu bulunduğu için sadece ``__len__`` ile örnek vereceğim. Diyelim ki ``Vector``
sınıfımızın örnekleri üzerine ``len`` fonksiyonu uygulandığında 'x' ve 'y' değerlerinin
toplamını döndürmek istiyoruz::

		def __len__(self):
			return self.x + self.y

	>>> a = Vector(1,2)
	>>> len(a)
	3

Şimdi gömülü fonksiyonlarımızın çağırdıkları metotları sıralayarak kısaca bilgi verelim.


``__len__`` Metodu
---------------------

``len`` fonksiyonu tarafından çağılır. Sadece ``self`` parametresi alır.
Dönüş değeri ``int`` olmalıdır.

``__repr__`` Metodu
-------------------------

``repr`` fonksiyonu tarafından çağılır. Sadece ``self`` parametresi alır.
Dönüş değeri ``str`` olmalıdır. Tanımlanmasa bile object sınıfından miras alınır.

``__str__`` Metodu
--------------------

``str`` sınıfı tarafından çağırılır. Aslında str bir fonksiyon değil sınıftır ancak
``str`` sınıfını çağırmak ``str.__new__`` fonksiyonunu çağırmak ile aynı olduğundan
``str`` tarafından çağırıldığını söyleyebiliriz. Aynı şey ``int``,``float`` gibi sınıflar 
için de geçerlidir. Sadece ``self`` parametresi alan ``__str__`` metodunun dönüş
değeri ``str`` olmalıdır.
Ayrıca ``__str__`` metodu tanımlanmasa bile (object sınıfı miras alındığı için)
``__repr__`` metoduna eşittir.

``__int__`` Metodu
--------------------

``int`` sınıfı tarafından çağırılmaktadır. Sadece ``self`` parametresi alır. 
Dönüş değeri ``int`` olmalıdır.

``__float__`` Metodu
-----------------------

``float`` sınıfı tarafından çağırılmaktadır. Sadece ``self`` parametresi alır. 
Dönüş değeri ``float`` olmalıdır.

``__oct__`` Metodu
---------------------

``oct`` fonksiyonu tarafından çağırılır. Sadece ``self`` parametresi alır. Dönüş 
değeri ``str`` olmalıdır.

``__hex__`` Metodu
----------------------

``hex`` fonksiyonu tarafından çağırılır. Sadece ``self`` parametresi alır. Dönüş 
değeri ``str`` olmalıdır.

``__bool__`` Metodu
---------------------

``bool`` sınıfı tarafından çağırılır. Sadece ``self`` parametresi alır. Dönüş 
değeri ``bool`` olmalıdır.

``__abs__`` Metodu
--------------------

``abs`` fonksiyonu tarafından çağırılır. Sadece ``self`` parametresi alır. Dönüş 
değeri ``int`` veya ``float`` olmak zorunda değildir ancak mantıken öyle olmalıdır.

``__dir__`` Metodu
---------------------

``dir`` fonksiyonu tarafından çağılır. Sadece ``self`` parametresi alır.
Dönüş değeri ``list`` olmalıdır. Bu liste içinde sınıf veya örnek ile ilgili bilgi
verilmelidir (``dir`` fonksiyonunun amacı budur).



Başka fonksiyonlar tarafından çağırılan metotlar da vardır ancak büyük ihtimalle
hiç ihtiyacınız olmayacaktır. ``__str__`` veya ``__int__`` gibi methodlar ise
nesnemizi ``str`` veya ``int`` sınıflarına çevirirken çok işimize yarar.
Örneğin ``nesne`` adındaki bir değişkeni ``str`` türüne çevirmek istersek ne yaparız? 
``str(nesne)``'yi kullanırız. İşte bu da ``nesne.__str__()`` ile aynıdır.

.. note:: Dikkat ederseniz Python'da farklı sınıfların örnekleri üzerinde kullanılabilen
          ``str`` , ``int`` ve ``len`` gibi fonksiyonların bu kadar farklı
          tür nesneler ile çalışabilmesinin bir sebebinin de bu sihirli
          metotlar olduğunu anlayabilirsiniz. Çünkü bu metotlar kendilerine
          argüman olarak verilen nesnenin türüne bakmadan onun
          ``__str__`` , ``__int__`` , ``__len__`` gibi metotlarını
          çağırmakta ve ondan dönen değeri tekrar geri döndürmektedir.
          Bu sebeple ``str`` , ``int`` ve ``len`` gibi fonksiyonlar her 
          tür nesne için ortak olarak kullanılıp söz dizimini kolaylaştırırken her nesnenin 
          ``__str__`` , ``__int__`` , ``__len__`` gibi metotları kendine
          özgü olmakta ve farklı çalışmaktadır. Bu söz dizimi kolaylığını C,C++ gibi
          derlenen dillerde göremeyiz. Python dilinde böyle bir özelliğin
          bulunması bunu Python'un yorumlanan bir dil olmasına borçludur.
          Bunun daha ince ayrıntılarını ileride konuşacağız.


İfade Metotları 
================

İfade metotları Python'un ``with`` ifadesi ve ``for`` döngüsü gibi kolay söz dizimleri
ile nesnelerimizi kullanmamızı sağlar. Yani kendi tanımladığımız sınıfların örnekleri 
bu metotlara sahip olduğunda ``with`` ve ``for`` ifadesi ile kullanılabilirler.

'with' İfadesi ile Çalışmak
-----------------------------

Bir nesnenin::

	with nesne as n:
		...

şeklinde kullanılabilmesi için iki sihirli metoda sahip olması gerekmektedir.
Bu metotlar ``__enter__`` ve ``__exit__``'dir. Bu fonksiyonları neye göre
kullanacağımızı iyice anlamak için şimdi yukarıdaki ``with`` ifadesini, ``with``
kullanmadan yazmaya çalışacağız::

	mgr = nesne
	value = mgr.__enter__()
	exc = True

	try:
    	try:
        	n = value
        	...
    	except Exception as e:
        	exc = False
        		if not mgr.__exit__(type(e) , e , e.__traceback__):
            		raise e
	finally:
    	if exc:
        	mgr.__exit__(None, None, None)

.. note:: Bu kod resmi python dökümasyonundan alınarak üzerinde biraz oynama 
		  yapılmıştır. Buradaki 'mrg', 'exc' ve 'value' kullanıcı tarafından erişilemeyen
		  ancak 'with' ifadesi çalışırtılırken python yorumlayıcısında bulunan değişkenlerdir. 
		  Daha fazla bilgi için buraya_ bakabilirsiniz.
.. _buraya: https://www.python.org/dev/peps/pep-0343/#specification-the-with-statement

Örneğin şu ifade::

	with open("dosya.txt") as dosya:
		fonksiyon()

Aslında bununla aynı şekilde çalışıyor::

	mgr = open("dosya.txt")
	value = mgr.__enter__()
	exc = True
	
	try:
    	try:
        	dosya = value
        	fonksiyon()
    	except Exception as e:
        	exc = False
        		if not mgr.__exit__(type(e) , e , e.__traceback__):
            		raise e
	finally:
    	if exc:
        	mgr.__exit__(None, None, None)

Bu kodu dikkatlice incelersek şu çıkarımları yapabiliriz:
	* En başta ``open("dosya.txt").__enter__`` fonksiyonu çağırılmaktadır.
	* Daha sonra ``dosya`` değişkenine, çağırılan ``open("dosya.txt").__enter__`` fonksiyonunun döndürdüğü değer verilmektedir.
	* Daha sonra ``with`` ifadesinin içindeki bölüm, yani örneğimizde ``fonksiyon()`` çalıştırılmaktadır.
	* Eğer ``fonksiyon`` çalışırken bir hata yükselirse bu hata yakalanmaktadır;
		* ``exc``'nin değeri ``False`` yapıldığı için daha sonra ``finally`` içindeki ``if`` şartı sağlanmamaktadır,
		* ``mgr.__exit__(type(e) , e , e.__traceback__)`` işlemi yapılmaktadır ve dönüş değeri ``False`` ise yakalanan hata tekrar yükseltilerek kullanıcıya ulaştırılmaktadır.
	* Eğer ``fonksiyon`` çalışırken bir hata yükselmez ise ``finally`` içindeki ``if`` şartı sağlanır ve ``mgr.__exit__(None, None, None)`` işlemi yapılır.

Şimdi bu yaptığımız çıkarımlardan da bu metotları kendi sınıflarımıza eklerken
kullancağımız başka çıkarımlarda bulunalım:
	* ``__enter__`` metodu sadece ``self`` parametresi alır. Fazladan bir parametre almaz.
	* ``__enter__`` metodundan dönen değer ``with nesne as n`` ifadesindeki ``n`` değişkenine atanmaktadır.
	* ``__exit__`` metodu her zaman ``self`` parametresinin yanında 3 parametre daha alır.
	* ``__exit__`` metodundan ``False`` döndürür isek ve ``with`` ifdesinin içerisinde bir hata yükseldi ise bu hata yükselmeye devam eder. Eğer ``True`` döndürürsek hata yükselmez. ``True`` mu yoksa ``False`` mı döndüreceğimizi belirlemek için ``__exit__`` metoduna verilen ve yükselen hata ile ilgili olan 3 parametreden faydalanabiliriz. Bu parametrelerden ilki yükseltilen hatanın türü (örneğin TypeError) , ikincisi hatanın kendisi , üçüncüsü ise hatanın ``__traceback__`` niteliğidir. ``__traceback__`` nesnesinin niteliklerini kullanarak da hatanın kaçıncı satırda gerçekleştiği gibi bilgilere ulaşabiliriz.
	

Bu saydığımız kuralları göz önünde tutarak kendi sınıflarımızı istediğimiz şekilde
``with`` ifadesi ile çalışacak hale getirebiliriz. Ama bu kadar teorik konuştuğumuz
yeter. Şimdi bir örnek yapalım. Düşünelim ki bir sınıfımız var ve bu sınıfı
veritabanımıza erişmek için kullanıyoruz. Veritabanın güvenli bir şekilde
açılması, okunması ve kapatılması lazım. Böyle durumlarda alınacak önlemler çoğunlukla 
bellidir. Yani veritabanı güvenli bir şekilde kullanılırken yapılması gerekenler
bir fonksiyon haline getirilebilir. Şimdi sınıfımızı yazmaya başlayalım::

	class VeriTabanıBağlantısı:
		def __init__(self , veri_tabanı_ismi ):
			self.isim = veri_tabanı_ismi 

		def __enter__(self):
			# Güvenli bir şekilde veri tabanımıza bağlanıyoruz
			return self # self döndürüyoruz çünkü 'with nesne as' ifadesinden sonra gelen değişkenin yine nesne'ye eşit olmasını istiyoruz.

		def __exit__(self, exception_type , exception , traceback):
			return True # hatanın yükseltilmemesini istiyoruz

		def write(self, veri): pass
			# veritabanına veri yazıyoruz

		def read(self, isim): pass
			# veritabanındaki bilgileri okuyup döndürüyoruz

Şimdi bu sınıfı with ifadesi ile kullanalım::

	with VeriTabanıBağlantısı("kullanıcı şifreleri") as veri_tabanı:
		veri_tabanı.write({"Ahmet" : "123456"})
		şifre = veri_tabanı.read("Ali")

Tabii bu örnek biraz soyut kaldı ama piyasadaki bazı üçüncü şahıs modül ve 
kütüphanelerde ``with`` ifadesi ile birlikte çalışabilecek sınıfların
bulunduğunu görebilirsiniz.


'for' İfadesi ile Çalışmak
--------------------------------------------

``for`` döngüsü Python'da bolca kullanıldığı için büyük ihtimalle ``with``
ifadesinden daha çok işinize yarayacaktır. Aslında başlıkda ``for`` ifadesi var ama
bizim burda öğreneceğimiz şeyi genele vurursak yineleyiciler (iterators) ile çalışmak
diyebiliriz. Çünkü burada öğreneciğimiz şey kendi sınıflarımızı nasıl birer
üretece , daha dorusu yinelenebilir bir nesneye dönüştürmek olacak da diyebiliriz.
Başlangıç olarak ``with`` ifadesinde yaptığımız gibi ``for`` ifadesinin de 
aslında ne olduğu ile başlayalım.::

	for i in yinelenebilir:
		...

İşlemi şu şekilde çalışmaktadır::

	yineleyici = iter(yinelenebilir)

	while True:
		try:
			i = next(yineleyici)
		except StopIteration:
			break

		...


.. note:: Buradaki 'yineleyici' nesnesi bizim erişemediğimiz ancak 'for'
		  döngüsü çalışırken Python yorumlayıcısında bulunan bir değişkendir.

Gördüğünüz gibi aslında ``for`` döngüsü sonsuz bir ``while`` döngüsüdür, 
``next`` fonksiyonun yinelediği yinelenebilir nesnenin bitip ``next`` fonksiyonun
``StopIteration`` yükseltmesine sebep olana kadar da devam etmektedir. Buradaki
gömülü ``next`` fonksiyonunun ne olduğunu zaten üreteçler konusundan hatırlıyoruz.
Bize yabancı gelen bir fonksiyon varsa o da yine gömülü olan ``iter`` fonksiyonudur.
``iter`` fonksiyonu aslında kendisine argüman olarak verilen nesnenin ``__iter__`` sihirli
metodunu çağırıp onun dönüş değerini döndürmektedir. Aslına bakarsanız üreteçler
konusunda hiç bahsetmemiş olsak da ``next`` metodu da kendisine verilen nesnenin
``__next__`` sihirli metodunu çağırır ve yine onun döndürdüğü değeri geri
döndürür. Yani aslında yukarıdaki kodu şu şekilde de yazabiliriz::

	yineleyici = yinelenebilir.__iter__()

	while True:
		try:
			i = yineleyici.__next__()
		except StopIteration:
			break

		...


Şimdi yine bazı çıkarımlarda bulunalım:
	* En başta yinelenebilir nesnenin ``__iter__`` metodu çağrılmaktadır.
	* Daha sonra her döngüde ``__iter__`` metodunun döndürdüğü değerin ``__next__`` metodu çağırılarak ``for i in yinelenebilir`` dönüş değeri ifadesindeki ``i`` değişkenine atanmaktadır.
	* Eğer çağırılan bu ``__next__`` methodu ``StopIteration`` yükseltirse ``while`` döngüsü kırılmakta, dolayısı ile de ``for`` döngümüz bitmektedir.

Artık bu bilgilerden faydalanarak kendi sınıflarımıza ``__iter__`` ve ``__next__``
metotlarını şu kurallar doğrultusunda ekleyebiliriz:
	* Yineleme işlemi başlamadan önce hazır hale getirmemiz gereken bir şey varsa bunu ``__iter__`` metodunun içerisinde yapabiliriz.
	* ``__next__`` metodu çağırılıcak nesne ``__iter__`` metodunun dönüş değeri olmalıdır. Eğer istersek bu bir üreteç veya kendi nesnemiz yani ``self`` olabilir. İkisi için de örnek vereceğiz. 
	* ``__next__`` metodumuzun döndüreceği değer her seferinde ``for i in yinelenebilir`` ifadesindeki ``i`` değişkenine atanacağı için döndüreceğimiz değeri buna göre belirlemeliyiz.
	* Nesnemizin yinelenmesi bitince ``__next__`` metodumuzdan ``StopIteration`` hatası yükseltmeliyiz.

Şimdi en başlarda kullandığımız ``Vector`` sınıfımıza ``__iter__`` ve ``__next__``
metotlarını eklemeye çalışalım. Yapmak istediğimiz şey ``Vector`` örneğimizin
sırası ile ``x`` ve ``y`` niteliğini döndürdükten sonra ``StopIteration`` yükselterek
döngüyü sonlandırması::

	class Vector:
		def __init__(self,x,y):
			self.x = x
			self.y = y

		def __iter__(self):
			self.yineleme = -1
			return self

		def __next__(self):
			self.yineleme += 1
			if self.yineleme == 0:
				return self.x
			elif self.yineleme == 1:
				return self.y
			else:
				raise StopIteration

	>>> nesne = Vector(1,3)
	>>> nesne.x
	1
	>>> nesne.y
	3
	>>> for i in nesne:
			print(i)

	1
	3
	>>>

Gördüğünüz gibi ``Vector`` sınıfımıza gerekli metotları doğru bir şekilde
eklediğimiz için ``Vector`` sınıfımızın örneklerini ``for`` döngüsü ile kullanabilmekteyiz. 
Bu örneğimi şu şekilde yazıp açıklayalım::

	>>> nesne = Vector(1,3)
	>>> yineleyici = nesne.__iter__() ## yineleyici = iter(nesne)
	>>> while True:
			try:
				i = yineleyici.__next__() ## i = next(yineleyici)
			except StopIteration:
				break
			print(i)

İlk önce ``Vector`` sınıfımızı örnekleyerek dönen değeri ``nesne`` değişkenimize
atadık. Daha sonra ``nesne.__iter__`` metodunu çağırarak dönüş değerini ``yineleyici``
değişkenine atadık. Yani artık ``nesne.yineleme``'nin değeri ``-1``'e, ``yineleyici``'nin
değeri de ``nesne`` değişkenimize eşit olmuş oldu. Çünkü ``nesne.__iter__`` metodundan
``self`` değerini yani yine ``nesne`` değişkenimizi döndürmüş olduk. Daha sonra ``nesne.__next__``
metodunu çağırdık. Burada ``nesne.__next__`` metodu normal bir fonksiyondur. ``nesne.yineleme``
değişkeni ``1`` arttırılarak ``0`` oldu. Döndürdüğü ``self.x`` değeri yani ``1`` değeri ``i`` 
değişkenine atandıktan sonra ekrana yazıldı. Daha sonra döngü başa döndü ve tekrar ``nesne.__next__``
çağırıldı. Bu sefer ``nesne.yineleme`` değişkeni ``1`` oldu ve ``self.y`` değişkeni, yani
``3`` değeri döndürüldü. ``i`` bu sefer ``3`` oldu ve tekrar ekrana yazdırıldı. Tekrar başa
döndükten sonra ``__next__`` fonksiyonunda ``nesne.yineleme`` değişkeni ``2`` olduğu için 
``StopIteration`` hatası yükseltildi. Bu hata yakalandı ve döngü sonlandırıldı.

Bu örneğimizde ``__iter__`` metodu ``self`` değerini döndürdüğü için ``__iter__`` metodu
çağırılan nesnemizin aynı zamanad ``__next__`` metoduna da sahip olması gerekiyordu. Ancak 
istersek ``__iter__`` metodundan değer olarak bir üreteç, yani türü ``generator``
olan bir nesne de döndürebiliriz. Gömülü ``next`` fonksiyonunun üreteçler ile
kullanılabilmesi üreteçlerin zaten ``__next__``  metoduna sahip olduğu anlamına gelir.
Şimdi yukarıda yaptığımız örneği ``Vector`` sınıfımıza ``__next__`` metodu ekleyerek yapmak
yerine, ``__iter__`` metodundan bir üreteç nesnesi döndürerek yapalım::

	def üreteç(x,y):
		yield x
		yield y

	class Vector:
		def __init__(self,x,y):
			self.x = x
			self.y = y

		def __iter__(self):
			return üreteç(self.x,self.y)

	>>> nesne = Vector(1,3)
	>>> nesne.x
	1
	>>> nesne.y
	3
	>>> for i in nesne:
			print(i)

	1
	3
	>>>

Gördüğünüz gibi ``Vector`` sınıfımıza ``__next__`` metodu eklemek yerine ``Vector.__iter__``
metodundan ``__next__`` metoduna sahip olan başka bir nesne döndürerek de aynı işlemi
yapabiliyoruz. Ancak for döngüsü ile birlikte kullanacağımız nesnenin kesinlikle
``__iter__`` metoduna sahip olması gerekmektedir. Çünkü hep o nesnemizin ``__iter__``
metodu çağırılacaktır. Yukarıdaki son örneğimizi şu şekilde de yazabileceğimizi unutmayın::

	>>> nesne = Vector(1,3)
	>>> yineleyici = iter(nesne)
	>>> type(yineleyici)
	<class 'generator'>
	>>> while True:
			try:
				i = next(yineleyici) 
			except StopIteration:
				break
			print(i)

	1
	3
	>>>

Şimdi farklı bir örnek daha verip bir sonraki konuya geçelim. Kendisine
verilen sayıya kadar olan sayıların karesini döndüren bir sınıf yapalım::

	def üreteç(sayı):
		for i in range(sayı):
			yield i**2

	class kare_alıcı:
		def __init__(self,sayı):
			self.sayı = sayı

		def __iter__(self):
			return üreteç(self.sayı)

	>>> k = kare_alıcı(5)
	>>> k.sayı
	5
	>>> for i in k:
			print(i)

	0
	1
	4
	9
	16
	>>>



Diğer Metotlar  (getitem setitem vs)
===============



Çağırma Metodu  
===============

Pythonda bazı nesneler çağırılabilirken bazı nesneler değildir. Örneğin
fonksiyon ve sınıflar çağırılabilir (*callable*) iken bu sınıfların
örnekleri (örneğin ``5`` veya ``"Ali"``) çağırılabilir değildir.
Bir nesnenin çağırılabilir olup olmadığını gömülü ``callable`` 
fonksiyonunu kullanabiliriz::

	>>> callable("ali")
	False
	>>> callable(134)
	False
	>>> callable(lambda: None)
	True

Aslında bir nesnenin çağırılabilir olması demek şu şekilde kullanılabilmesi
anlamına gelir::

	>>> nesne()

Peki biz de kendi yazdığımız sınıf örneklerini bu şekilde kullanabilmek 
için ne yapmalıyız. ``__call__`` sihirli metodu bize bunu yapmamız için 
olanak sağlamaktadır. Kısacası şu kod::

	>>> nesne(*args,**kwargs)

Şunun ile eşdeğerdir::

	>>> nesne.__call__(*args,**kwargs)

Bu bilgiyi kullanarak amacımıza ulaşabiliriz. Basit bir örnek yapalım.
Yazacağımız sınıfın örnekleri çağırıldığında ``yazı`` niteliklerini 
değer olarak döndürsünler yazdırsınlar::

	class Sınıf:
		def __init__(self,yazı):
			self.yazı = yazı

		def __call__(self):
			return self.yazı

	>>> s = Sınıf("Merhaba")
	>>> callable(s)
	True
	>>> type(s)
	<class '__main__.Sınıf'>
	>>> s()
	'Merhaba'
	>>> değişken = s()
	>>> print(değişken)
	Merhaba

Burada istersek nesnemizi parametreler ile çağırılabilecek hale de getire-
biliriz. Bütün parametreler nesnemizin ``__call__`` metoduna verilecektir::

	class Çakma_Fonksiyon:
		def __call__(self,parametre):
			print(parametre)

	>>> d = Çakma_Fonksiyon()
	>>> d("Merhaba")
	Merhaba
	>>> d("Dünya")
	Dünya

Bu özellik aslında oldukça faydalıdır. Fonksiyonların çağırılabilecek tek
nesne olduğunu düşünürsek sınıfları çağırdığımızda da bir ``__call__`` metodu çalıştırılmakta
(bu ``__call__`` metodu bizim yukarıdaki örneklerde tanımladığımız method değildir)
ve ``__call__ `` metodu da ``__new__`` metodunu çağırıp onun döndürdüğü değeri tekrar
geri döndürmektedir. Bu konu hakkında metasınıflarda tekrar konuşacağız.

..  Garbage collector ve referans count ile ilgili daha sonra ekleme yapılacak

 Silme Metodu 
 =============

 Python'da ``del`` ifadesi bir değişkeni silmek için kullanılmaktadır.
 Şöyle bir örnek verelim::

	>>> değişken = 1
	>>> değişken
	1
	>>> del değişken
	>>> değişken
	NameError: name 'a' is not defined

 Tabii ki bir değişken Python programlarında sadece bu yolla silinmez.
 Python çöp toplayıcı (*garbage collector* veya kısaca GC) sistemine sahiptir.
 Bu da demek olur ki Python yorumlayıcısı gereksiz olduğuna karar verdiği
 değişkenleri otomatik olarak silerek hafızada gereksiz yer kaplanmasını engeller.
 Peki hangi durumlarda bir değişken gerekiz olur. Birkaç örnek verelim::

	>>> id(12345)
	2266819152976
	>>> id(12345)
	2266819153104

 Burada aynı sayının ard arda ID'sini kontrol ettiğimizde farklı sonuçlar
 almaktayız. Bunun sebebi ``12345`` nesnesinin oluşturularak ``id``
 fonksiyonuna argüman olarak verildikten sonra saklanmaya devam etmesi
 için hiçbir sebep kalmamış olmasıdır. Biz ``12345`` nesnesini
 bir değişkene atamadık. Bu nesneye tekrardan erişmemizi sağlayacak
 hiçbir yol yok. Burada *referans* terimi işin içine girmektedir.
 ``12345`` nesnesinin ``id`` fonksiyonu tarafından kullanımı bittikten
 sonra bu nesneye referans eden, yani bu nesnenin yerini bize göstererek
 nesneye ulaşmamızı sağlayacak bir değişkenimiz bulunmamaktadır. Ancak bu 
 değişken sadece bizde bulunmamaktadır, Python yorumlayıcısında bulunmaktadır. 
 Fakat Python da bizim bu değişkene ulaşmamızın bir yolunun olmadığını bildiği
 için bu değişkenin önemsiz olduğuna karar verip değişkeni silmektedir.
 Sonuçta biz ``12345`` nesnesine tekrar erişmek isteseydik onu bir değişkene 
 atardık.
