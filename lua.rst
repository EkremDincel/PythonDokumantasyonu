.. meta::
   :description: Bu bölümde fonksiyonlar konusunu inceleyeceğiz.
   :keywords: python, fonksiyon, lambda, recursive, decorator, closure,
              özyinelemeli, bezeyiciler, kapalı fonksiyonlar ,
              nested , nonlocal , nested function , iç ,
              iç içe , iç içe fonksiyonlar, generator, üreteç , yield ,
              iterate , iterator

.. highlight:: lua

Merhaba. Bu bir kod::

	io.write("Kaça kadar olan asalları yazıyım? ")
	sinir = io.read()

	for i=2, sinir-1 do
		yaz = true
		for j=2, i-1 do
			if i%j == 0 then
				yaz = false
				break
			end
		end

		if yaz then print(i) end
	end




