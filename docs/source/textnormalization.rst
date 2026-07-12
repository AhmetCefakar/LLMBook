=====================
Metin Normalizasyonu
=====================

Şimdi de doğal dil işlemede önişlem adımlarından biri olan metin normalizasyonu üzerinde duracağız. Metin
normalizasyonu, henüz bir işleme sokulmamış olan ham metinlerin makine öğrenmesi modelleri için standart ve
tutarlı bir biçime dönüştürülmesi sürecidir. Normalizasyon sırasında bilgi kayıpları kaçınılmazdır. Örneğin
*harika!!!!*, *harika!!*, *harika!* gibi yazı parçalarını biz tek ! kullanarak *harika!* biçiminde normalize
edebiliriz. Ancak burada duygu yoğunluğu bakımından bir kayıp oluşacaktır. Normalizasyondaki kayıplar her zaman
olumsuzluk yaratmak zorunda değildir. Hatta bu kayıplar bazı uygulamalarda zarardan daha çok fayda bile
sağlayabilmektedir.

Normalizasyon süreci aslında hedefe uygun biçimde yapılmaktadır. Hedeflenen şey neyse kayıplar ve kazançlar ona
göre göz önüne alınmalıdır. Biz aşağıda normalizasyonda kullanılan temel alt yöntemleri açıklayacağız. Ancak bu
alt yöntemlerin hepsinin bir doğal dil işleme sürecinde uygulanması gerekmemektedir. Bunların amaca göre
gerektiği kadar uygulanması gerekir. Doğal dil işleme uzmanı *ne kaybettiğini ve ne kazandığını bilerek*
işlemleri yürütmelidir.

Atomlara ayırma öncesindeki metin normalizasyon işlemini çeşitli alt başlıklara ayırabiliriz:

- Karakter Düzeyinde Normalizasyon

  - Unicode Normalizasyonu
  - Harflendirme Normalizasyonu ya da Büyük/Küçük Harf Dönüşümü (Case Normalization)
  - ASCII Dönüşümü (Transliteration)
  - Aksanları (Diacritics) Kaldırma

- Yapısal Normalizasyon

  - Boşluk (Whitespace) Normalizasyonu
  - Noktalama Normalizasyonu
  - Tırnak (Apostrophe) Normalizasyonu
  - Satır Sonu ve Kontrol Karakterleri

- İçerik Normalizasyonu

  - URL Normalizasyonu
  - E-posta Normalizasyonu
  - Sayı Normalizasyonu
  - Emoji ve Özel Sembollerin Normalizasyonu
  - Hashtag ve Mention Normalizasyonu
  - Resmi Olmayan (Informal) Yazım Genişletme (Türkçeye Özel)
  - Eş Anlamlı Sözcüklerin Normalizasyonu
  - Yinelenen Karakterleri Azaltma

İzleyen paragraflarda bu alt başlıkları tek tek ele alarak Türkçe için metin normalizasyon işlemlerini
gerçekleştireceğiz.

Karakter Düzeyinde Normalizasyon
=================================

Karakter düzeyinde normalizasyon *metinleri karakter temelinde ele alarak onları standart biçime
dönüştürmeyi* hedeflemektedir. Karakter düzeyinde normalizasyondaki ilk aşama genellikle Unicode metinlerin
normalizasyonudur. Bir karakter print edildiğinde görüntüsü aynı olduğu halde aslında bu görüntü farklı
Unicode karakter kombinasyonlarıyla oluşturulabilmektedir. Örneğin ``'ğ'`` karakteri tek bir Unicode karakter
biçiminde ya da ``'g'`` ve üstü tırtıllı (breve) karakterden oluşan iki Unicode karakter biçiminde de temsil
edilebilmektedir. Doğal dil işlemeyi yapan uygulamacı için bu iki temsil aslında aynıdır. Eğer bunlar aynı
biçime dönüştürülmezse algoritmalar bunları farklı karakter olduğunu sanacaktır.

Bir harfin okunuşunu ya da anlamını değiştirmek için onun üzerine ya da altına getirilen küçük işaretlere
*aksan karakterleri (diacritical marks)* denilmektedir. Örneğin Ö, Ü, ğ, â, ş karakterlerinin üzerindeki ve
altındaki işaretler aksan karakterleridir. Unicode tabloda bazı karakterler hem tek bir karakter olarak hem
de aksanlı biçimde iki karakter olarak temsil edilebilmektedir.

Unicode Normalizasyon Biçimleri: NFC, NFD, NFKC, NFKD
------------------------------------------------------

Unicode normalizasyonu dört biçimde yapılabilmektedir: NFC, NFD, NFKC ve NFKD.

.. list-table:: Unicode Normalizasyon Biçimleri
   :header-rows: 1
   :widths: 10 35 35

   * - Form
     - Açıklama
     - Ne Zaman Kullanılır?
   * - NFC
     - Normalized Form Composed
     - Genel kullanım, insan okunabilirliği
   * - NFD
     - Normalized Form Decomposed
     - Karakter analizi, aksan kaldırma
   * - NFKC
     - Compatibility Composed
     - ÖNERİLEN - Varyasyonları birleştir
   * - NFKD
     - Compatibility Decomposed
     - İleri analiz için

Unicode normalizasyonu Python standart kütüphanesindeki ``unicodedata`` modülünde bulunan ``normalize``
isimli fonksiyonla yapılabilmektedir. Bu fonksiyonun parametrik yapısı şöyledir:

.. code-block:: python

   unicodedata.normalize(form, unistr)

Fonksiyonun birinci parametresi normalizasyon türünü belirtir. Bu parametre ``'NFC'`` gibi ``'NFD'`` gibi
yazısal biçimde girilmelidir. Fonksiyonun ikinci parametresi normalize edilecek yazıyı almaktadır. Fonksiyon
normalize edilmiş yazıyla geri dönmektedir.

NFC ve NFD Dönüştürmeleri
--------------------------

NFC normalizasyonunda aksan karakterleriyle oluşturulmuş Unicode karakterler mümkünse tek bir karakter
biçimine dönüştürülmektedir. Yani örneğin eğer yazıda ``'g'`` ve üstü tırtıl (breve) karakterleri yan yana
ise NFC normalizasyonu bunu tek bir ``'ğ'`` karakteri haline getirmektedir. Tabii yazıda zaten tek karakter
olarak ``'ğ'`` varsa onun üzerinde bir işlem yapılmamaktadır. Yani NFC normalizasyonu aksanlı karakterleri
mümkünse kompakt biçime dönüştürmektedir. (Her aksanlı karakterin tek karakterlik bir karşılığının
olmayabileceğine dikkatinizi çekmek istiyoruz.)

NFD normalizasyonu NFC normalizasyonunun tam ters işlemini yapmaktadır. Yani tek bir karakter biçiminde
yazılmış olan Unicode karakteri iki karakter biçiminde aksanlı hale getirmektedir. Örneğin:

.. code-block:: pycon

   >>> s = 'Ülkü öğretmen'

Buradaki s yazısının UTF-16 hex karşılığı şöyledir:

.. code-block:: pycon

   >>> s.encode('utf-16-le').hex(' ')
   'dc 00 6c 00 6b 00 fc 00 20 00 f6 00 1f 01 72 00 65 00 74 00 6d 00 65 00 6e 00'

Şimdi bu s yazısını NFD normalizasyonuna sokalım:

.. code-block:: pycon

   >>> text = unicodedata.normalize('NFD', s)
   >>> text
   'Ülkü öğretmen'

Biz burada NFD normalizasyonundan elde edilen yazıya bakarak sanki değişen bir şeyin olmadığını
sanabilirsiniz. Aslında glyph aynı olsa da s yazısı ile text yazısı aynı Unicode kod numaralarından
oluşmamaktadır:

.. code-block:: pycon

   >>> s == text
   False

text değişkeninin tuttuğu yazının UTF-16 hex karşılığı şöyledir:

.. code-block:: pycon

   >>> text.encode('utf-16-le').hex(' ')
   '55 00 08 03 6c 00 6b 00 75 00 08 03 20 00 6f 00 08 03 67 00 06 03 72 00 65 00 74 00 6d 00 65 00 6e 00'

Görüldüğü gibi burada ``'Ü'`` karakteri tek bir Unicode karakter olarak değil ``'U'`` ve üstü iki nokta
karakteri biçimine dönüştürülmüştür (55 00 08 03). Şimdi biz bunu yeniden NFC dönüştürmesine sokalım:

.. code-block:: pycon

   >>> text2 = unicodedata.normalize('NFC', text)
   >>> text2.encode('UTF-16-le').hex(' ')
   'dc 00 6c 00 6b 00 fc 00 20 00 f6 00 1f 01 72 00 65 00 74 00 6d 00 65 00 6e 00'

Burada elde edilen bytes dizisinin ilk s yazısının bytes dizisi ile aynı olduğuna dikkat ediniz. ``encode``
metodunda encoding belirtilmezse default encoding ``utf-8`` alınmaktadır. UTF-16 ve UTF-32 için encoding
belirtilirken eğer Endian'lık belirtilmezse ``encode`` metodu geri döndürdüğü bytes dizisinin başına
Endian'lığı belirtmek için BOM belirtecini (BOM marker) yerleştirmektedir.

Bir karakterin üzerinde birden fazla aksan karakteri olabilir. Örneğin bir karakter hem şapkalı hem de
çengelli olabilir. Bu durumda biz bu karakteri NFD normalizasyonuna soktuğumuzda bu karakter üç karakter
biçimine dönüştürülecektir. İlki asıl karakter sonraki ikisi aksan karakterleri olacaktır.

NFKC ve NFKD Dönüştürmeleri
----------------------------

NFKC normalizasyonu farklı glyph'lere ilişkin ama aynı anlama gelen karakterlerin aynı kod numarasıyla
temsil edilmesini sağlamaktadır. Bu bakımdan daha ileri bir normalizasyon yapmaktadır. Örneğin ``fi`` yazısı
Unicode karakter tablosunda tek bir karakter olarak bulunmaktadır. (İki karakterin birleştirilerek sanki tek
bir karakter gibi tek bir glyph ile temsil edilmesine İngilizce *ligature* denilmektedir. Bu sözcüğü Türkçe
*bitişik harf* biçiminde ifade edebiliriz.) İşte bir yazıda ``fi`` geçtiğinde ya da bitişik harfli ``fi``
geçtiğinde anlamsal bir farklılık oluşmamaktadır. O halde bu iki karakterin aynı karakter olarak
dönüştürülmesi uygun olur. Benzer biçimde örneğin matematikte de Yunanca bir fi harfi vardır. Bunların da
diğer fi'lerle aynı biçimde temsil edilmesi istenebilir. NFKC dönüştürmesi bu tür karakterleri aynı
karakterlerden oluşacak biçimde dönüştürmektedir. Örneğin:

.. code-block:: pycon

   >>> s = "office"     # burada karakterleri ayrı ayrı kullandık
   >>> len(s)
   6
   >>> k = "oﬃce"      # burada tek karakter olan ffi karakterini kullandık
   >>> len(k)
   4

``ffi`` biçiminde bir bitişik karakter vardır. Bu nedenle yukarıdaki string'ler aslında aynı anlama geldiği
halde farklı uzunlukta yazılar halindedir. İşte NFKC dönüştürmesi bunları aynı karakterlerden oluşacak hale
getirmektedir. Örneğin:

.. code-block:: pycon

   >>> s_n = unicodedata.normalize('NFKC', s)
   >>> k_n = unicodedata.normalize('NFKC', k)
   >>> len(s_n)
   6
   >>> len(k_n)
   6
   >>> s == k
   False
   >>> s_n == k_n
   True

NFKC dönüştürmesinde karakterler daha yalın, yani daha temel biçime dönüştürülmektedir. Ancak daha yalın
biçim daha az karakterli biçim anlamına gelmemektedir. Daha yalın biçim daha temel karakterlerle ifade edilen
biçimdir. Örneğin:

.. code-block:: pycon

   >>> unicodedata.normalize('NFKC', '⓪')
   '0'

Unicode tablodaki yuvarlaklı rakamlar NFKC dönüştürmesinde yalnızca rakama dönüştürülmektedir.

NFKD dönüştürmesi de NFKC dönüştürmesi gibidir. Ancak sonuçta elde edilen karakterleri aksansal olarak
ayrıştırmaktadır. Yani başka bir deyişle NFC ile NFD arasındaki ilişki NFKC ile NFKD arasındaki ilişkiye
benzerdir. NFKD normalizasyonu da yine anlamsal olarak aynı olan farklı karakterleri aynı biçime dönüştürür
ancak dönüştürülmüş biçim eğer aksansal karakterler içeriyorsa bunları birden çok karakter biçiminde açar.
Örneğin:

.. code-block:: pycon

   >>> s = "Ankara'da ²⁰²⁴ yılında ①⓪⓪ bin kişi yaşıyor"
   >>> text = unicodedata.normalize('NFKC', s)
   >>> text
   "Ankara'da 2024 yılında 100 bin kişi yaşıyor"
   >>> text = unicodedata.normalize('NFKD', s)
   >>> text
   "Ankara'da 2024 yılında 100 bin kişi yaşıyor"
   >>> text = unicodedata.normalize('NFKC', s)
   >>> len(text)
   43
   >>> text = unicodedata.normalize('NFKD', s)
   >>> len(text)
   45

Türkçe için en uygun Unicode normalizasyonu çoğu durumda NFKC normalizasyonudur. Bu normalizasyonu bir
fonksiyonla sarmalayabiliriz:

.. code-block:: python

   def unicode_normalize(text):
       return unicodedata.normalize('NFKC', text)

Harflendirme (Büyük/Küçük Harf) Normalizasyonu
-----------------------------------------------

Harflendirme normalizasyonu ya da büyük harf/küçük harf normalizasyonu yazıdaki alfabetik karakterlerin
küçük harfe ya da büyük harfe dönüştürülmesi anlamına gelmektedir. Böylece uygulamacı aynı kavramı temsil
eden büyük harfli ve küçük harfli yazımları aynı biçime getirir. Bu sayede karakter sayısını ve dolayısıyla
da atom (token) sayısını azaltmış olur. Harflendirme normalizasyonu oldukça sık biçimde kullanılmaktadır.

Harflendirme normalizasyonunda uygulamacılar genellikle alfabetik karakterleri küçük harflere
dönüştürürler. Tabii yazıdaki alfabetik karakterler küçük harfe dönüştürüldüğünde anlam kayıpları da
oluşabilecektir. Örneğin böyle bir dönüştürme sonucunda ``Apple`` sözcüğü ile ``apple`` sözcüğü arasında
fark kalmaz. Oysa ``Apple`` sözcüğü büyük harf yazıldığı için Apple firmasını temsil ediyor olabilir. Bu tür
durumlarda eğer kayıp önemli olarak değerlendiriliyorsa özel isimler ve kısaltmalar büyük harfli de
bırakılabilir. Konuya girişte de belirttiğimiz gibi normalizasyon faaliyetleri aslında amaca uygun biçimde
yürütülmelidir. Bazı uygulamalarda ``Apple`` isminin küçük harfe dönüştürülmesinde bir kayıp olmayabilir.

Harflendirme normalizasyonu Python'daki str sınıfının ``lower`` ya da ``upper`` metotlarıyla yapılabilir.
Ancak bu metotlar maalesef Türkçe karakterleri dikkate almamaktadır. Örneğin:

.. code-block:: pycon

   >>> s = 'Izgara'
   >>> s.lower()
   'izgara'

Burada görüldüğü gibi ``'I'`` karakterini ``lower`` metodu ``'i'`` karakterine dönüştürmektedir. Maalesef bu
durum ``locale`` ayarlarıyla da düzeltilememektedir. Aslında aynı durum ``'İ'`` dönüştürmesinde de vardır:

.. code-block:: pycon

   >>> s = 'İzmir'
   >>> k = s.lower()
   >>> k
   'i̇zmir'
   >>> k == 'izmir'
   False
   >>> len(s)
   5
   >>> len(k)
   6

Görüldüğü gibi ``'İ'`` karakterini küçük harfe dönüştürdüğümüzde ``'i'`` elde etmedik. Peki ne elde ettik?
Yazının UTF-16 kodlamasına bakalım:

.. code-block:: pycon

   >>> k.encode('utf-16-le').hex(' ')
   '69 00 07 03 7a 00 6d 00 69 00 72 00'

Burada ``'İ'`` karakteri küçük harfe dönüştürüldüğünde ``'i'`` ve üzerinde nokta olan aksan karakteri elde
edilmiştir. Ancak bu karakter daha sonra NFC dönüştürmesine sokulsa bile normal ``'i'`` karakteri haline
getirilememektedir. O halde ``lower`` metoduyla küçük harfe dönüştürme yapılırken iki sorunlu Türkçe karakter
vardır: ``'İ'`` ve ``'I'``. Diğer Türkçe karakterlerin küçük harfe dönüştürülmesinde bir sorun ortaya
çıkmamaktadır. Örneğin:

.. code-block:: pycon

   >>> s = 'PİJAMALI HASTA YAĞIZ ŞOFÖRE ÇABUCAK GÜVENDİ'
   >>> k = s.lower()
   >>> k
   'pi̇jamali hasta yağiz şoföre çabucak güvendi̇'

O halde Türkçe metinlerdeki alfabetik karakterler küçük harflere aşağıdaki gibi dönüştürülebilir:

.. code-block:: python

   def case_normalize(text):
       return text.replace('İ', 'i').replace('I', 'ı').lower()

Tabii biz bir sözlük kullanarak da bu işlemi yapabilirdik:

.. code-block:: python

   turkish_special_chars = {
       'Ç': 'ç',
       'Ğ': 'ğ',
       'I': 'ı',
       'İ': 'i',
       'Ö': 'ö',
       'Ş': 'ş',
       'Ü': 'ü'
   }

   def case_normalize_dict(text):
       result = ''
       for c in text:
           result += turkish_special_chars.get(c, c.lower())
       return result

Bu fonksiyonu biraz daha hızlandırmak için şöyle bir yol izleyebiliriz: Yukarıdaki sözlüğü anahtar ve
değerlerini elde ederek dolaşırız. Her anahtarı str sınıfının ``replace`` metoduyla değere dönüştürürüz.
Örneğin:

.. code-block:: python

   def case_normalize_dict(text):
       for key, value in turkish_special_chars.items():
           text = text.replace(key, value)
       return text.lower()

Aslında en hızlı yöntem muhtemelen str sınıfının ``translate`` metodunu kullanmaktır. ``translate`` metodu
parametre olarak bir sözlük alır ve yukarıdaki işlemin aynısını yapar. CPython gerçekleştiriminin standart
kütüphanesindeki built-in sınıfların kodları C'de yazılmıştır. Dolayısıyla yapılan işlem aynı olsa bile
``translate`` metodu daha hızlı çalışacaktır. Bu sözlükte anahtarlar ve değerler Unicode kod numaralarından
oluşmaktadır. Bu sözlüğü oluşturmak için str sınıfının ``maketrans`` static metodundan faydalanılmaktadır.
Eğer bu biçimdeki sözlükte değerler ``None`` ise ``translate`` onları silmektedir. ``maketrans`` static
metodunun üçüncü parametresi silinecek karakterleri belirtmektedir. Metot ürettiği sözlükte bu karakterlerin
anahtarlarını ``None`` yapmaktadır. Örneğin:

.. code-block:: python

   def case_normalize_dict(text):
       return text.translate(str.maketrans('ÇĞIİÖŞÜ', 'çğıiöşü')).lower()

.. code-block:: pycon

   >>> s = 'BUGÜNXYXY HXYAVA ÇOK XXXXYGÜZEL'
   >>> s.translate(str.maketrans('ÇĞIİÖŞÜ', 'çğıiöşü', 'XY')).lower()
   'bugün hava çok güzel'

.. note::

   Bu derste geçen tüm Python kod örnekleri (NFC/NFD/NFKC/NFKD dönüştürmeleri, ``lower``/``translate``
   örnekleri) gerçekten çalıştırılarak doğrulanmıştır. Tüm çıktılar orijinal metinde verilenlerle birebir
   örtüşmektedir.

ASCII Dönüştürmesi ve Aksan Karakterlerinin Kaldırılması
=========================================================

ASCII Dönüştürmesi (Transliteration)
-------------------------------------

Seyrek de olsa bazı uygulamalarda metinlerdeki o dile özgü karakterlerin (bizim için Türkçe) onlara en yakın
ASCII karakterleriyle yer değiştirmesi istenebilir. Bu dönüştürmeye *ASCII dönüştürmesi* ya da daha genel
olarak İngilizce *transliteration* denilmektedir. Türkçe'deki özel karakterleri onlara yakın ASCII
karakterlerine dönüştürme işlemini yine str sınıfının ``translate`` metodu ile aşağıdaki gibi kolayca
yapabiliriz:

.. code-block:: python

   def asciify_turkish_normalize(text):
       return text.translate(str.maketrans('çğıöşüÇĞİÖŞÜ', 'cgiosuCGIOSU'))

Burada önce str sınıfının static ``maketrans`` fonksiyonu ile dönüştürme sözlüğü elde edilmiş, o sözlükle de
``translate`` metodu çağrılmıştır.

Aşağıda bu fonksiyonun bir örnek üzerindeki çalışmasını gösteriyoruz:

.. code-block:: pycon

   >>> asciify_turkish_normalize("Çağrı öğretmenin şişesi güzeldi, İzmir'de Üsküdar'a gitti.")
   "Cagri ogretmenin sisesi guzeldi, Izmir'de Uskudar'a gitti."

Aksan Karakterlerinin (Diacritical Characters) Kaldırılması
------------------------------------------------------------

Karakter düzeyinde yapılan diğer bir normalizasyon da *aksan karakterlerinin (diacritical characters)*
kaldırılmasıdır. Örneğin şapkalı a'daki şapkayı kaldırmak isteyebiliriz. Türkçe'nin dışındaki diğer bazı
dillerde çok fazla aksan karakterleri bulunabilmektedir. Bunlardan yazının arındırılması bazı uygulamalarda
fayda sağlayabilmektedir. Tabii bazı uygulamalarda burada oluşacak bilgi kaybının olumsuz etkileri de
olabilir.

Unicode karakter tablosunda pek çok dilin karakterleri bir arada bulunmaktadır. ``unicodedata`` modülündeki
``category`` isimli fonksiyon bir Unicode karakteri parametre olarak alıp onun kategorisini vermektedir. Bazı
Unicode kategorilerini aşağıda veriyoruz:

.. list-table:: Bazı Unicode Kategorileri
   :header-rows: 1
   :widths: 12 25 45

   * - Kategori Kodu
     - Kategori Adı
     - Açıklama
   * - Lu
     - Uppercase Letter
     - Büyük harfler (A, B, C, Ş, Ğ)
   * - Ll
     - Lowercase Letter
     - Küçük harfler (a, b, c, ş, ğ)
   * - Nd
     - Decimal Number
     - Ondalık sayılar (0-9)
   * - Zs
     - Space Separator
     - Boşluk karakterleri
   * - Po
     - Other Punctuation
     - Noktalama işaretleri (. , ! ?)
   * - Sm
     - Math Symbol
     - Matematiksel semboller (+, =, <, >)
   * - Pd
     - Dash Punctuation
     - Tire işaretleri (-, –, —)
   * - Ps
     - Open Punctuation
     - Açılış parantezleri ( [ {
   * - Pe
     - Close Punctuation
     - Kapanış parantezleri ) ] }
   * - So
     - Other Symbol
     - Diğer semboller (©, ®, €, ♪)
   * - Sc
     - Currency Symbol
     - Para birimleri (₺, $, €, £)
   * - Mn
     - Nonspacing Mark
     - Aksan karakteri (ünsüz)
   * - Cc
     - Control
     - Kontrol karakterleri (\\n, \\t)
   * - Lo
     - Other Letter
     - Diğer dil harfleri (漢, أ, א)

Kategori listesinin tamamına aşağıdaki bağlantıdan erişebilirsiniz:

``https://www.unicode.org/reports/tr44/tr44-34.html#General_Category_Values``

İşte Unicode yazı üzerinde önce NFD normalizasyonunu uygulayıp daha sonra aksan karakterlerini atabiliriz.
Ancak burada bir sorun vardır. Türkçeye özgü karakterlerin bazıları NFD normalizasyonuna sokulduğunda üstteki
tırtılları, noktaları ve alttaki çengelleri aksan karakteri olarak ayrıştırılmaktadır. Bu dönüştürmeden sonra
aksan karakterleri silindiğinde bu Türkçe karakterler de değişmiş olacaktır. Örneğin dönüştürmeyi aşağıdaki
gibi bir fonksiyonla yapmaya çalışırsak Türkçe karakterleri de kaybederiz:

.. code-block:: python

   def diacritical_normalize(text):
       ntext = unicodedata.normalize('NFD', text)
       return ''.join((c for c in ntext if unicodedata.category(c) != 'Mn'))

Bu fonksiyonu çalıştırdığımızda Türkçe karakterlerin de bozulduğunu görürüz; örneğin:

.. code-block:: pycon

   >>> diacritical_normalize("Çağrı öğretmenin şişesi güzeldi, İzmir'de Üsküdar'a gitti.")
   "Cagrı ogretmenin sisesi guzeldi, Izmir'de Uskudar'a gitti."

Görüldüğü gibi ``İ`` harfi ``I`` harfine dönüşmüş, ``ı`` harfi ise (NFD'de ayrışmadığı için) olduğu gibi
kalmıştır; bu da Türkçe metinler için istenmeyen bir sonuçtur.

Fonksiyonu şöyle düzeltebiliriz:

.. code-block:: python

   def diacritical_normalize(text):
       chars = []
       for c in text:
           if c in 'çğıöşüÇĞİÖŞÜ':
               chars.append(c)
           else:
               nfd = unicodedata.normalize('NFD', c)
               for d in nfd:
                   if unicodedata.category(d) != 'Mn':
                       chars.append(d)
       return ''.join(chars)

Düzeltilmiş fonksiyonu aynı örnek üzerinde çalıştırdığımızda Türkçe karakterlerin korunduğunu görürüz:

.. code-block:: pycon

   >>> diacritical_normalize("Çağrı öğretmenin şişesi güzeldi, İzmir'de Üsküdar'a gitti.")
   "Çağrı öğretmenin şişesi güzeldi, İzmir'de Üsküdar'a gitti."

Burada biz yazıdaki karakterleri tek tek gözden geçirip eğer karakter özel Türkçe karakterlerinden biriyse
onu ``chars`` listesine ekledik. Ancak karakter özel Türkçe karakterlerinden biri değilse onu NFD
normalizasyonuna sokup aksan karakterlerini yok ettik. Kodun bu kısmına dikkat ediniz:

.. code-block:: python

   nfd = unicodedata.normalize('NFD', c)
   for d in nfd:
       if unicodedata.category(d) != 'Mn':
           chars.append(d)

Genellikle bir karakteri NFD normalizasyonuna soktuğumuzda karakter aksanlı ise dönüştürme sonucunda iki
karakter elde edilmektedir: Asıl karakter ve aksan karakteri. Ancak yukarıda da belirttiğimiz gibi aksan
karakterleri bir tane olmak zorunda değildir ve aslında asıl karakterin başta bulunması da zorunlu değildir.
Bu nedenle biz yukarıda NFD normalizasyonuna soktuğumuz karakterleri bir döngü ile dolaşarak aksan
karakterlerini elimine etme yoluna gittik.

Yukarıdaki fonksiyon daha hızlı çalışacak hale de getirilebilir. Örneğin her defasında ``in`` operatörü ile
sıralı arama yapmak yerine bunun için bir sözlük oluşturup bir çeşit *lookup table* ile hızlandırma
sağlanabilir. Ayrıca NFD normalizasyonundan sonraki döngü *liste içlemiyle* ya da *üretici ifade* ile de
hızlandırılabilir. Python'daki kodu hızlandırmak istediğinizde mutlaka öngörünüzü doğrulamak için
``timeit`` modülü ile test ediniz. Ya da bu konuda LLM'lere başvurunuz.

.. note::

   Bu derste geçen ``asciify_turkish_normalize`` ve ``diacritical_normalize`` (hatalı ve düzeltilmiş
   sürümleri) fonksiyonları örnek Türkçe ve yabancı kelimeler üzerinde gerçekten çalıştırılarak
   doğrulanmıştır. Hatalı sürümün Türkçe karakterleri (özellikle ``İ`` ve ``ğ``/``ş``/``ç``/``ü``/``ö``
   harflerinin aksan kısımlarını) bozduğu, düzeltilmiş sürümün ise bunları koruyarak yalnızca diğer
   dillerden gelen aksan işaretlerini kaldırdığı doğrulanmıştır.

Yapısal Normalizasyon
======================

Biz yukarıda karakter temelinde normalizasyon işlemlerini ele aldık. Şimdi yapısal normalizasyonlar üzerinde
duralım. Yapısal normalizasyonlar olarak grupladığımız normalizasyonları anımsatmak istiyoruz:

- Yapısal Normalizasyon

  - Boşluk (Whitespace) Normalizasyonu
  - Noktalama Normalizasyonu
  - Tırnak (Apostrophe) Normalizasyonu
  - Satır Sonu ve Kontrol Karakterlerinin Normalizasyonu

Boşluk (Whitespace) Normalizasyonu
-----------------------------------

Metinde tutarsız biçimde bulunan *boşluk karakterlerinin (white space)* aynı biçime (örneğin tek boşluk
biçimine) dönüştürülmesi işlemine *boşluk normalizasyonu* denilmektedir. Boşluk karakterleri atomlara ayırma
(tokenization) sırasında da metinden çıkartılabilmektedir. Ancak uygulamacının bu işlemi özel olarak yapması
daha iyi bir kontrol sağlayabilmektedir.

Boşluk karakterleri boşluk algısı oluşturan karakterlerdir. SPACE, NL (New Line), TAB, VTAB ve CR (Carriage
Return) karakterleri ASCII tablosunda da bulunan tipik boşluk karakterleridir. Ancak Unicode tabloda temel
ASCII tablosunda bulunmayan boşluk karakterleri de vardır. Dolayısıyla böyle bir normalizasyonun ayrı bir
biçimde yapılması çoğu zaman önerilmektedir. Unicode tablodaki tüm boşluk karakterlerini aşağıda tablo
halinde veriyoruz:

.. list-table:: Unicode Boşluk Karakterleri
   :header-rows: 1
   :widths: 15 40

   * - Unicode
     - Karakter Adı
   * - U+0009
     - Horizontal Tab (HT)
   * - U+000A
     - Line Feed (LF)
   * - U+000B
     - Vertical Tab (VT)
   * - U+000C
     - Form Feed (FF)
   * - U+000D
     - Carriage Return (CR)
   * - U+0020
     - Space
   * - U+00A0
     - No-Break Space
   * - U+1680
     - Ogham Space Mark
   * - U+2000
     - En Quad
   * - U+2001
     - Em Quad
   * - U+2002
     - En Space
   * - U+2003
     - Em Space
   * - U+2004
     - Three-Per-Em Space
   * - U+2005
     - Four-Per-Em Space
   * - U+2006
     - Six-Per-Em Space
   * - U+2007
     - Figure Space
   * - U+2008
     - Punctuation Space
   * - U+2009
     - Thin Space
   * - U+200A
     - Hair Space
   * - U+200B
     - Zero Width Space
   * - U+200C
     - Zero Width Non-Joiner
   * - U+200D
     - Zero Width Joiner
   * - U+202F
     - Narrow No-Break Space
   * - U+205F
     - Medium Mathematical Space
   * - U+2060
     - Word Joiner
   * - U+3000
     - Ideographic Space
   * - U+FEFF
     - Zero Width No-Break Space (BOM)

Daha önceden de belirttiğimiz gibi regex dili standardize edilmiş bir dil değildir. Dolayısıyla regex
motorları arasında farklılıklar bulunmaktadır. Regex kalıplarında kullanılan karakter tablosu Python'un regex
motorunda default olarak Unicode tablodur. Ancak daha önceden de belirttiğimiz gibi Python'daki regex
fonksiyonlarının son parametrelerine ayarlama bayrakları girilebilmektedir. Bu bayrakların listesi şöyledir:

.. list-table:: Python Regex Bayrakları
   :header-rows: 1
   :widths: 20 15 45

   * - Bayrak
     - Kısa Kullanım
     - Açıklama
   * - ``re.IGNORECASE``
     - ``re.I``
     - Büyük/küçük harf duyarsız eşleme yapar
   * - ``re.MULTILINE``
     - ``re.M``
     - Çok satırlı mod - ^ ve $ her satırın başında/sonunda çalışır
   * - ``re.DOTALL``
     - ``re.S``
     - Nokta (.) karakterinin yeni satır karakterini de eşlemesini sağlar
   * - ``re.UNICODE``
     - ``re.U``
     - Unicode karakterler için (Python 3'te varsayılan)
   * - ``re.ASCII``
     - ``re.A``
     - ASCII modu - \\w, \\b, \\d gibi ifadeleri ASCII ile sınırlar
   * - ``re.VERBOSE``
     - ``re.X``
     - Regex içinde yorum ve boşluklara izin verir
   * - ``re.LOCALE``
     - ``re.L``
     - Yerel ayarlara göre eşleme yapar (Tavsiye edilmez, Unicode kullanın)

``re.UNICODE`` bayrağı *Unicode karakter kümesini kullan* anlamına gelmektedir. Yukarıda da belirttiğimiz
gibi bu bayrak Python için default durumdadır. ``re.ASCII`` bayrağı *standart ASCII karakterlerinin*
kullanılmasını sağlamaktadır. ``re.LOCALE`` ise seçilen locale dikkate alınarak belirleme yapılacağı anlamına
gelmektedir.

Ayrıca regex motorlarında bazı bayraklar ve direktifler kalıp içerisinde de ``(?XY...)`` biçiminde
belirtilebilmektedir. Biz bu konunun ayrıntılarına girmeyeceğiz. Ancak bayrak ve direktif belirten
karakterleri bir tablo halinde aşağıda vermek istiyoruz:

.. list-table:: Regex Önek Direktifleri
   :header-rows: 1
   :widths: 15 50

   * - Önek
     - Anlamı
   * - ``(?:...)``
     - Yakalamayan grup (non-capturing group)
   * - ``(?=...)``
     - Pozitif ileriye bakış - eşleşir ama yakalamaz (lookahead)
   * - ``(?!...)``
     - Negatif ileriye bakış (negative lookahead)
   * - ``(?<=...)``
     - Pozitif geriye bakış (lookbehind)
   * - ``(?<!...)``
     - Negatif geriye bakış (negative lookbehind)
   * - ``(?P<ad>...)``
     - İsimli yakalama grubu (named group)
   * - ``(?i)``
     - Büyük/küçük harf duyarsız (ignore case)
   * - ``(?m)``
     - Çok satır modu (multiline)
   * - ``(?s)``
     - Nokta her şeyi eşleştirir (dotall)
   * - ``(?u)``
     - Unicode modu
   * - ``(?x)``
     - Verbose mod - boşluk ve yorum satırı izin verir

Burada özellikle dört özel kalıbı kısaca açıklamak istiyoruz: Pozitif ileriye bakış, negatif ileriye bakış,
pozitif geri bakış ve negatif geri bakış. İleriye bakış *yazının sağı*, geriye bakış ise *yazının solu*
anlamına gelmektedir. Pozitif bakışta *parantez içerisindeki kalıp bulunmak zorundadır, ancak parantez
içerisindeki kalıp elde edilen kalıba dahil değildir ve tüketilmez*. Negatif bakışta ise *parantez
içerisindeki kalıp bulunmamak zorundadır, ancak yine parantez içerisindeki kalıp elde edilen kalıba dahil
değildir ve tüketilmez*. Örneğin:

.. code-block:: text

   (?<!\d)\W+(?=\d)

Burada parantezli kısımlar bulunan kalıba dahil değildir. İlk parantez negatif geriye bakış, ikinci parantez
pozitif ileriye bakıştır. Bu kalıp şunu demektedir: *Bir grup alfabetik olmayan karakter dizilimi elde
edilmek isteniyor. Ancak bunun solunda nümerik bir karakter olmamalı fakat sağında bir nümerik karakter
olmalıdır.*

Regex dilinde Unicode karakter kümesindeki yukarıdaki karakterlerin çoğu ``\s`` ile uyuşmaktadır. Ancak
istisna olarak aşağıdaki karakterler uyuşum sağlamamaktadır:

.. list-table:: ``\s`` İle Uyuşmayan Boşluğa Benzer Karakterler
   :header-rows: 1
   :widths: 15 40

   * - Unicode
     - Karakter Adı
   * - U+200B
     - Zero Width Space
   * - U+200C
     - Zero Width Non-Joiner
   * - U+200D
     - Zero Width Joiner
   * - U+2060
     - Word Joiner
   * - U+FEFF
     - Zero Width No-Break Space (BOM)

Bu karakterleri de boşluk normalizasyonuna dahil edebiliriz:

.. code-block:: python

   def whitespace_normalize(text):
       return re.sub(r'\s+|[\u200B\u200C\u200D\u2060\uFEFF]+', ' ', text).strip()

Burada birkaç noktaya değinmek istiyoruz. Biz yazıdaki birden fazla boşluk karakterini tek bir SPACE
karakteri ile değiştirdik. Ancak bu durumda yazının başında ve sonunda bir tane SPACE karakteri
kalabilmektedir. Bunları da ``strip`` metoduyla sildik. Python'da ve pek çok dilde ``\uHHHH`` biçimindeki
karakter dizilimi HHHH hex kod numarasına ilişkin Unicode karakter anlamına gelmektedir. Burada HHHH ilgili
Unicode karakterin UTF-16 değeridir. UTF-32 için ``\UHHHHHHHH`` sentaksı kullanılmaktadır. Buradaki Unicode
dönüşümü yorumlayıcı tarafından ilk aşamada yapıldığı için ``sub`` fonksiyonu ters bölü karakterlerini hiç
görmeyecektir.

Aşağıda bu fonksiyonun gerçek bir örnek üzerindeki çalışmasını gösteriyoruz:

.. code-block:: pycon

   >>> s = "  Bu\tbir   test\u200b\u200bmetnidir.\n\nYeni satır da var.  "
   >>> whitespace_normalize(s)
   'Bu bir test metnidir. Yeni satır da var.'

Noktalama Normalizasyonu
-------------------------

Noktalama normalizasyonu peşi sıra gelen aynı noktalama işaretlerinden yalnızca bir tanesini ya da n tanesini
alıp diğerlerini atarak gerçekleştirilmektedir. Tabii daha önceden de belirttiğimiz gibi bu tür
normalizasyonlar duygu kaybına yol açabilmektedir. Örneğin *dikkat!* ile *dikkat!!!* arasında bir vurgu farkı
olabilir. Bazı alanlarda ise birden fazla noktalama karakterinin peşi sıra gelmesi özel bir anlam ifade
edebilmektedir. Örneğin satrançta ``!!`` çok iyi hamle için ``??`` çok kötü hamle için kullanılan bir
gösterimdir. Tabii daha önceden de belirttiğimiz gibi normalizasyon hedefe göre gerçekleştirilen bir süreçtir.
Burada ele aldığımız tüm normalizasyonların buradaki gibi uygulanması zorunlu değildir.

Tireleme (Dash) Normalizasyonu
+++++++++++++++++++++++++++++++

Önce tireleme normalizasyonu üzerinde duralım. Unicode tabloda tire görüntüsüne benzer görüntüye sahip olan
birden fazla karakter vardır. Onların listesini de aşağıda veriyoruz:

.. list-table:: Tireye Benzer Unicode Karakterleri
   :header-rows: 1
   :widths: 12 8 35

   * - Unicode
     - Char
     - Karakter Adı
   * - U+002D
     - \-
     - Hyphen-Minus
   * - U+00AD
     - ­
     - Soft Hyphen
   * - U+2010
     - ‐
     - Hyphen
   * - U+2011
     - ‑
     - Non-Breaking Hyphen
   * - U+2012
     - ‒
     - Figure Dash
   * - U+2013
     - –
     - En Dash
   * - U+2014
     - —
     - Em Dash
   * - U+2015
     - ―
     - Horizontal Bar
   * - U+2017
     - ‗
     - Double Low Line
   * - U+2E3A
     - ⸺
     - Two-Em Dash
   * - U+2E3B
     - ⸻
     - Three-Em Dash
   * - U+2212
     - −
     - Minus Sign
   * - U+FE58
     - ﹘
     - Small Em Dash
   * - U+FE63
     - ﹣
     - Small Hyphen-Minus
   * - U+FF0D
     - －
     - Fullwidth Hyphen-Minus

O halde bu karakterleri ASCII tablosundaki ``-`` karakteri ile normalize edebiliriz:

.. code-block:: python

   text = re.sub(r'[\u00AD\u2010\u2011\u2012\u2013\u2014\u2015\u2017\u2E3A\u2E3B\u2212\uFE58\uFE63\uFF0D]',
                 '-', text)

.. code-block:: pycon

   >>> text = "Ankara\u2013İstanbul arası 450 km\u2014civarında, soft\u00adhyphen, minus\u2212sign."
   >>> text
   'Ankara–İstanbul arası 450 km—civarında, soft\xadhyphen, minus−sign.'
   >>> re.sub(r'[\u00AD\u2010\u2011\u2012\u2013\u2014\u2015\u2017\u2E3A\u2E3B\u2212\uFE58\uFE63\uFF0D]',
   ...        '-', text)
   'Ankara-İstanbul arası 450 km-civarında, soft-hyphen, minus-sign.'

Bilindiği gibi pek çok latin dilinde peşi sıra gelen üç nokta ayrı bir noktalama işarettir. Buna İngilizce
*ellipsis* denilmektedir. Ancak Unicode tabloda yan yana üç nokta için ``\u2026`` numaralı ayrı bir karakter
de bulundurulmuştur. Bu karakteri de ... nokta ile temsil edebiliriz:

.. code-block:: python

   text = re.sub(r'\u2026', '...', text)

.. code-block:: pycon

   >>> text = "Bekleyelim\u2026 sonra bakarız\u2026"
   >>> re.sub(r'\u2026', '...', text)
   'Bekleyelim... sonra bakarız...'

Fullwidth Noktalama İşaretleri
+++++++++++++++++++++++++++++++

Ayrıca Unicode tabloda klasik noktalama işaretlerine benzeyen başka noktalama işaretleri de vardır.
Bunların da normalize edilmesi gerekebilir:

.. code-block:: python

   text = re.sub(r'[\uFF01]', '!', text)   # fullwidth !
   text = re.sub(r'[\uFF1F]', '?', text)   # fullwidth ?
   text = re.sub(r'[\uFF0E]', '.', text)   # fullwidth .
   text = re.sub(r'[\uFF0C]', ',', text)   # fullwidth ,
   text = re.sub(r'[\uFF1B]', ';', text)   # fullwidth ;
   text = re.sub(r'[\uFF1A]', ':', text)   # fullwidth :

Buradaki Unicode karakterler klasik ASCII noktalama karakterlerine oldukça benzemektedir. Biz yukarıdaki
işlemle onların hepsini standart bir biçime dönüştürmüş olduk.

.. code-block:: pycon

   >>> text = "Bu nasıl\uff1f Çok iyi\uff01 Devam\uff0c sonra\uff1b bitir\uff1a son\uff0e"
   >>> text
   'Bu nasıl？ Çok iyi！ Devam， sonra； bitir： son．'
   >>> # yukarıdaki altı re.sub çağrısı sırayla uygulandığında:
   'Bu nasıl? Çok iyi! Devam, sonra; bitir: son.'

Tekrarlanan Noktalama İşaretlerinin Normalizasyonu
+++++++++++++++++++++++++++++++++++++++++++++++++++

Birden fazla noktalama karakterinin peşi sıra bulunması durumunda bunların normalizasyonu için birkaç teknik
kullanılabilmektedir:

1) Peşi sıra gelen aynı noktalama karakterlerinden yalnızca birini alarak normalize etmek.

2) Peşi sıra gelen aynı noktalama karakterlerini *bir ya da çok* biçiminde normalize etmek. Bunun için *çok*
   durumunu aynı karakterden iki taneyle temsil edilebilir. Örneğin yazıdaki bir tane ! karakteri ! olarak
   bırakılır. Ancak !!!!! gibi birden fazla ! karakteri !! biçiminde temsil edilebilir. Ya da çok !
   ayrı bir atomla da temsil edilebilir. Bunun için çok seyrek kullanılan bir Unicode karakteri
   seçebilirsiniz. Bazen uygulamacılar bu özel atomları açısal parantezler içerisine alınmış bir sözcükle de
   temsil edebilmektedir. Örneğin çok sayıda ! karakteri ``<EXCLMS>`` gibi bir yazıyla temsil edilebilir.
   Atomlarına ayırma sırasında da bu yazı tek bir atom olarak ayrıştırılabilir. Birden çok aynı noktalama
   karakterinin iki tane ile temsil edilmesi (collapsing) ile açısal parantez içerisinde bir sözcükle temsil
   edilmesinin avantajları ve dezavantajları vardır. Bu avantajları ve dezavantajları bir tablo halinde
   listeliyoruz:

.. list-table:: ``Collapse`` Stratejisi ( !!! → !! )
   :header-rows: 1
   :widths: 40 40

   * - Artıları
     - Eksileri
   * - - Pretrained vocab değişmez
       - ! → !! gradyan sürekliliği sağlar
       - Ham metin okunabilir kalır
       - BPE / WordPiece ile doğal uyum
       - Az miktarda ince ayar verisi yeterli
     - - Yoğunluk bilgisi kaybolur (!! = !!!! = !!!!!)
       - Karışık !?!? kural yönetimi karmaşıklaşır
       - Eşik seçimi keyfi kalabilir
       - !! ile ! sınırı belirsizleşebilir

.. list-table:: ``Token`` Stratejisi ( !!! → <EXCLMS> )
   :header-rows: 1
   :widths: 40 40

   * - Artıları
     - Eksileri
   * - - Atomik semantik birim oluşturur
       - Vocab boyutu öngörülebilir
       - Sınıflandırmada net sinyal verir
       - Yoğunluk kategorisi korunur
       - Sıfırdan eğitim için ideal
     - - Pretrained modelde embedding matrisi genişletilmeli
       - Yeni embedding'ler sıfırdan öğrenir
       - Ham metnin okunabilirliği bozulur
       - Az veriyle eğitim gürültü yaratır
       - Eşik değeri kritik hiperparametre

Biz birden fazla aynı noktalama karakterini iki tane olacak biçimde normalize etmek isteyelim. Bu işlemi
şöyle yapabiliriz:

.. code-block:: python

   text = re.sub(r'\.{3,}', '...', text)         # dört ve üzeri noktayı üç noktaya indir
   text = re.sub(r'!{2,}',  '!!',   text)        # tekrarlanan ünlemi iki tane yap
   text = re.sub(r'\?{2,}', '??',   text)        # tekrarlanan soru işaretini iki tane yap
   text = re.sub(r',{2,}',  ',,',   text)        # tekrarlanan virgülü iki tane yap
   text = re.sub(r';{2,}',  ';;',   text)        # tekrarlanan noktalı virgülü iki tane yap
   text = re.sub(r':{2,}',  '::',   text)        # tekrarlanan iki noktayı iki tane yap
   text = re.sub(r'-{2,}',  '--',   text)        # tekrarlanan tireyi iki tane yap
   text = re.sub(r'(\?!)+|(!\?)+',  '!?', text)  # tekrarlanan ?! için !? yerleştir

.. code-block:: pycon

   >>> text = "Çok güzel!!!!! Gerçekten mi????  Tamam,,,,, peki....  Bekle;;; sonra::: devam---- !?!?!?"
   >>> # yukarıdaki sekiz re.sub çağrısı sırayla uygulandığında:
   'Çok güzel!! Gerçekten mi??  Tamam,, peki...  Bekle;; sonra:: devam-- !?'

Yazılarda noktalama işaretlerinden önce boşluk karakteri bırakılmaz. Fakat noktalama işaretlerinden sonra bir
SPACE boşluk bırakılır. Ancak yazıları yazanlar bu dilbilgisi kuralına uymayabilmektedir. Dolayısıyla bu
biçimde de bir normalizasyonun yapılması çoğu kez uygun olmaktadır. Bu işlemi şöyle yapabiliriz:

.. code-block:: python

   # noktalama işaretlerinden önceki boşlukları kaldır
   text = re.sub(r'\s+([.,!?;:\)])', r'\1', text)
   # noktalama işaretlerinden sonra bir boşluk yerleştir
   text = re.sub(r'([.,!?;:])(?![.,!?;:])(?=\S)', r'\1 ', text)

.. code-block:: pycon

   >>> text = "Merhaba ,dünya !Nasılsın ?İyiyim ;sen nasılsın :iyiyim."
   >>> text = re.sub(r'\s+([.,!?;:\)])', r'\1', text)
   >>> text = re.sub(r'([.,!?;:])(?![.,!?;:])(?=\S)', r'\1 ', text)
   >>> text
   'Merhaba, dünya! Nasılsın? İyiyim; sen nasılsın: iyiyim.'

İlk satırda noktalama işaretlerinin solundaki boşluk karakterleri elimine edilmiştir. İkinci satırda ise
noktadan sonra sıfır tane ya da daha fazla boşluk karakteri tek bir boşluk karakteriyle değiştirilmiştir.
Ancak bu ikinci kalıpta ? ya da ! karakterinin solunda da bunlardan biri varsa bu ikisinin arasına boşluk
karakteri konulması engellenmiştir. Regex'te ``(?=...)`` biçimindeki direktife *ileriye bakış (lookahead)*,
``(?!...)`` biçimindeki direktife ise *olumsuz ileriye bakış (negative lookahead)* denilmektedir. ``(?=...)``
direktifinde ``...`` ile temsil ettiğimiz kalıp uyuşum sağlamak zorundadır, ancak kalıba dahil edilmez ve
tüketilmez. Yukarıdaki ikinci kalıptaki ``(?=\S)`` kısmına dikkat ediniz. Bu kısım şu anlama gelmektedir:
*Burada boşluk karakterlerinin dışında bir karakter olmak zorunda ama bu karakter kalıpta yer almayacak ve
tüketilmeyecek*. Yani sonraki arama bu karakterden başlatılacaktır. ``(?!...)`` direktifinde ``...`` temsil
ettiğimiz kalıp ise uyuşum sağlamamak zorundadır, ancak kalıba dahil edilmez ve tüketilmez. İkinci kalıptaki
``(?![.,!?;:])`` kısım şu anlama gelmektedir: *Burada .,!?;: karakterinden biri olmayan bir karakter bulunmak
zorunda ama bu karakter kalıpta yer almayacak ve tüketilmeyecek*.

.. note::

   Bu derste geçen tüm regex tabanlı normalizasyon kod parçaları (``whitespace_normalize`` fonksiyonu, tire
   normalizasyonu, üç nokta normalizasyonu, fullwidth noktalama dönüşümleri, tekrarlanan noktalama
   sıkıştırma kalıpları ve noktalama çevresindeki boşluk düzenleme kalıpları) örnek metinler üzerinde
   gerçekten çalıştırılarak doğrulanmıştır. Tüm çıktılar metinde anlatılan davranışla örtüşmektedir.

Noktalama, Tırnak Normalizasyonu ve Normalizasyon Boru Hatları
===============================================================

Noktalama Normalizasyonu Fonksiyonu
------------------------------------

Şimdi de noktalama normalizasyonunu yapan bir fonksiyon yazalım:

.. code-block:: python

   def punctuation_normalize(text):
       text = re.sub(r'[\u00AD\u2010\u2011\u2012\u2013\u2014\u2015'
                   '\u2017\u2E3A\u2E3B\u2212\uFE58\uFE63\uFF0D]', '-', text)
       text = re.sub(r'\u2026', '...', text)
       text = re.sub(r'[\uFF01]', '!', text)           # fullwidth !
       text = re.sub(r'[\uFF1F]', '?', text)           # fullwidth ?
       text = re.sub(r'[\uFF0E]', '.', text)           # fullwidth .
       text = re.sub(r'[\uFF0C]', ',', text)           # fullwidth ,
       text = re.sub(r'[\uFF1B]', ';', text)           # fullwidth ;
       text = re.sub(r'[\uFF1A]', ':', text)           # fullwidth :
       text = re.sub(r'\.{3,}', '...', text)           # dört ve üzeri noktayı üç noktaya indir
       text = re.sub(r'!{2,}',  '!!',   text)          # tekrarlanan ünlemi iki tane yap
       text = re.sub(r'\?{2,}', '??',   text)          # tekrarlanan soru işaretini iki tane yap
       text = re.sub(r',{2,}',  ',,',   text)          # tekrarlanan virgülü iki tane yap
       text = re.sub(r';{2,}',  ';;',   text)          # tekrarlanan noktalı virgülü iki tane yap
       text = re.sub(r':{2,}',  '::',   text)          # tekrarlanan iki noktayı iki tane yap
       text = re.sub(r'-{2,}',  '--',   text)          # tekrarlanan tireyi iki tane yap
       text = re.sub(r'(\?!)+|(!\?)+',  '!?', text)    # tekrarlanan ?! için !? yerleştir
       text = re.sub(r'\s+([.,!?;:\)])', r'\1', text)
       text = re.sub(r'([.,!?;:])(?![.,!?;:])(?=\S)', r'\1 ', text)
       return text

Fonksiyonda yukarıda belirttiğimiz işlemler peşi sıra yapılmıştır.

Tırnak (Apostrophe) Normalizasyonu
-----------------------------------

Şimdi de *tırnak (apostrophe) normalizasyonu* üzerinde duralım. Unicode tabloda tırnak anlamına gelen farklı
karakterler bulunmaktadır. Bunların da aynı biçime dönüştürülmesi gerekebilmektedir. Unicode tabloda bulunan
tırnak karakterlerinin listesini tablo halinde aşağıda veriyoruz:

.. list-table:: Unicode Tırnak Karakterleri
   :header-rows: 1
   :widths: 12 8 40

   * - Unicode
     - Char
     - Karakter Adı
   * - U+0022
     - "
     - Quotation Mark
   * - U+0027
     - '
     - Apostrophe
   * - U+00AB
     - «
     - Left-Pointing Double Angle Quote
   * - U+00BB
     - »
     - Right-Pointing Double Angle Quote
   * - U+2018
     - '
     - Left Single Quotation Mark
   * - U+2019
     - '
     - Right Single Quotation Mark
   * - U+201A
     - ‚
     - Single Low-9 Quotation Mark
   * - U+201B
     - ‛
     - Single High-Reversed-9 Quotation Mark
   * - U+201C
     - "
     - Left Double Quotation Mark
   * - U+201D
     - "
     - Right Double Quotation Mark
   * - U+201E
     - „
     - Double Low-9 Quotation Mark
   * - U+201F
     - ‟
     - Double High-Reversed-9 Quotation Mark
   * - U+2032
     - ′
     - Prime
   * - U+2033
     - ″
     - Double Prime
   * - U+2034
     - ‴
     - Triple Prime
   * - U+2035
     - ‵
     - Reversed Prime
   * - U+2036
     - ‶
     - Reversed Double Prime
   * - U+2037
     - ‷
     - Reversed Triple Prime
   * - U+2039
     - ‹
     - Single Left-Pointing Angle Quote
   * - U+203A
     - ›
     - Single Right-Pointing Angle Quote
   * - U+FF02
     - ＂
     - Fullwidth Quotation Mark
   * - U+FF07
     - ＇
     - Fullwidth Apostrophe

Burada dış çerçevenin kusurlu olmasının nedeni VSCode editöründe bazı Unicode karakterlerin diğerlerinden
farklı genişlikte görüntülenmesindendir (bu not orijinal ders notlarındaki kutu çizimine ilişkindir; aşağıdaki
tablo bu sorunu içermez). Bu karakterleri onlara uygun tek bir karakterle temsil edebiliriz:

.. code-block:: python

   text = re.sub(r'[\u201C\u201D\u201E\u201F\u2033\u2036]', '"', text)     # çift tırnak varyantları
   text = re.sub(r'[\u2018\u2019\u201A\u201B\u2032\u2035]', "'", text)     # tek tırnak varyantları

Tırnak normalizasyonunu yapan fonksiyonu aşağıdaki gibi yazabiliriz:

.. code-block:: python

   def apostrophe_normalize(text):
       text = re.sub(r'[\u201C\u201D\u201E\u201F\u2033\u2036]', '"', text)     # çift tırnak varyantları
       text = re.sub(r'[\u2018\u2019\u201A\u201B\u2032\u2035]', "'", text)     # tek tırnak varyantları
       return text

Şimdi de yukarıda yazdığımız tüm fonksiyonları manuel bir boru hattı biçiminde test edelim:

.. code-block:: python

   s = """
       “Merhaba Dünya!!! Nasılsın?İyi misin???”
   """

   text = unicode_normalize(s)
   text = case_normalize(text)
   text = diacritical_normalize(text)
   text = whitespace_normalize(text)
   text = punctuation_normalize(text)
   text = apostrophe_normalize(text)

   print(text)

.. note::

   Bu kod parçası, sonraki bölümde tanımlanan ``NormalizerPipeline`` sınıfı kullanılmadan, fonksiyonların
   art arda manuel olarak çağrılmasıyla aynı normalizasyon zincirini uygulamaktadır.

Normalizasyon Boru Hattının Nesneye Yönelik Hale Getirilmesi
-------------------------------------------------------------

Biz yukarıdaki örnekte manuel boru hattı uyguladık. Yani bir fonksiyonun çıktısını diğerinin girdisine
verdik. Bu boru hattını nesne yönelimli hale getirebiliriz. Boru hatları genellikle sınıflarla temsil
edilmektedir. Boru hattının elemanları da (bunlara adım *step* de diyebiliriz) genellikle ikili demetlerden
oluşmaktadır. Demet elemanlarının ilki boru hattı elemanına verilen ismi, ikincisi ise çağrılabilir
(callable) nesneyi belirtmektedir.

Boru hattını temsil eden ``NormalizerPipeline`` sınıfını şöyle yazabiliriz:

.. code-block:: python

   class NormalizerPipeline:
       def __init__(self, steps=None):
           self.steps= steps or []

       def add_step(self, name, f):
           self.steps.append((name, f))

       def pop_step(self):
           return self.steps.pop()

       def insert_step(self, index, name, f):
           self.steps.insert(index, (name, f))

       def remove_step(self, name):
           for i in range(len(self.steps)):
               if self.steps[i][0] == name:
                   return self.steps.pop(i)

       def __call__(self, text):
           for _, f in self.steps:
               text = f(text)
           return text

Sınıfın ``__init__`` metodunda boru hattı elemanları bir liste olarak verilebilmektedir. Aynı zamanda bu
elemanlar daha sonra ``add_step`` metoduyla da eklenebilmektedir. ``pop_step`` metodu son elemanı,
``remove_step`` metodu ise belli bir isme ilişkin elemanı boru hattından çıkarmaktadır. ``insert_step``
metodu da boru hattının arasına eleman eklemektedir. Burada ``__call__`` metodunun nasıl yazıldığına dikkat
ediniz:

.. code-block:: python

   def __call__(self, text):
       for _, f in self.steps:
           text = f(text)
       return text

Metot boru hattı elemanlarındaki fonksiyonları önceki elemanın çıktısını sonraki elemanın girdisine vererek
tek tek çağırmaktadır. Metodun son elemanın çıktısıyla geri döndürüldüğüne dikkat ediniz.

Yukarıdaki ``NormalizerPipeline`` sınıfını aşağıdaki gibi test edebiliriz:

.. code-block:: python

   text = """
       “Merhaba Dünya!!! Nasılsın?İyi misin???”
   """

   npl = NormalizerPipeline([
       ('UN', unicode_normalize),
       ('CN', case_normalize),
       ('DCN', diacritical_normalize),
       ('WSN', whitespace_normalize),
       ('PN', punctuation_normalize),
       ])

   npl.add_step('AN', apostrophe_normalize)
   npl.pop_step()
   npl.remove_step('CN')

   normaized_text = npl(text)
   print(normaized_text)

Orijinal ders notlarında bu kodun şu çıktıyı vereceği belirtilmektedir:

.. code-block:: text

   “Merhaba Dünya!! Nasılsın?İyi misin??”

.. note::

   Bu kod gerçekten çalıştırılıp doğrulanmıştır. ``add_step`` ile eklenen ``AN`` adımı hemen ardından
   ``pop_step`` ile geri çıkarıldığından ve ``remove_step('CN')`` ile büyük/küçük harf normalizasyonu da
   kaldırıldığından, çalışan boru hattı yalnızca ``UN``, ``DCN``, ``WSN`` ve ``PN`` adımlarından oluşur
   (tırnak normalizasyonu uygulanmaz, bu nedenle çift tırnak karakterleri orijinal eğri tırnak biçiminde
   kalır). Gerçek çalıştırmada elde edilen çıktı şudur:

   .. code-block:: text

      “Merhaba Dünya!! Nasılsın? İyi misin?? ”

   Yukarıdaki orijinal metinde belirtilen çıktıdan küçük bir farkla ayrılmaktadır: ``punctuation_normalize``
   içindeki ``([.,!?;:])(?![.,!?;:])(?=\S)`` kalıbı, bir noktalama işaretini -- bir başka noktalama işareti
   değilse ve hemen ardından boşluk olmayan bir karakter geliyorsa -- bir boşlukla takip eder. Bu kural
   ``?`` işaretinden sonra gelen ``İ`` harfi için de, ikinci ``?`` işaretinden sonra gelen kapanış tırnağı
   (``”``) için de geçerli olduğundan gerçek çıktıda ek boşluklar oluşmaktadır. Bu, kodun kendisinin doğru
   çalıştığını, ancak orijinal ders notundaki örnek çıktının bu ayrıntıyı hesaba katmadığını göstermektedir.

Bir boru hattı nesnesi boru hattı elemanı olarak da kullanılabilir. Ne de olsa o da fonksiyon gibi
*çağrılabilir (callable) bir nesnedir*. Örneğin:

.. code-block:: python

   npl1 = NormalizerPipeline([
       ('UN', unicode_normalize),
       ('CN', case_normalize),
       ('DCN', diacritical_normalize),
       ])

   npl2 = NormalizerPipeline([
       ('NPL1', npl1),
       ('WSN', whitespace_normalize),
       ('PN', punctuation_normalize),
       ('AN', apostrophe_normalize),
       ])

   text = npl2(text)
   print(text)

Burada birinci boru hattı nesnesi fonksiyon gibi çağrıldığında o boru hattındaki fonksiyonlar çağrılacaktır.

.. note::

   Bu kod da gerçekten çalıştırılmıştır. Bu defa ``npl1`` (``UN``, ``CN``, ``DCN``) bir adım olarak ``npl2``
   içine yerleştirildiğinden hem büyük/küçük harf normalizasyonu hem de tırnak normalizasyonu (``AN``)
   uygulanmaktadır. Elde edilen gerçek çıktı şöyledir:

   .. code-block:: text

      "merhaba dünya!! nasılsın? iyi misin?? "

Dile Özgü Normalizasyon Boru Hatları İçin Yardımcı Fonksiyonlar
----------------------------------------------------------------

Biz yukarıdaki örneklerimizde Türkçeyi temel aldık. Normalizasyonların dile de bağlı olduğunu anımsayınız.
Örneğin İngilizce'de ``isn't`` gibi ``aren't`` gibi kısaltmaların da normalize edilmesi gerekmektedir. Ancak
Türkçe'de bu biçimde yaygın bir kısaltma kalıbı yoktur. Bizim farklı diller için çok sayıda normalizasyon
fonksiyonu yazdığımızı düşünün. İşte bu tür durumlarda uygun bir dile ilişkin boru hattı nesnesini kolay
oluşturmak için yardımcı fonksiyonlardan da faydalanılmaktadır. Örneğin:

.. code-block:: python

   def build_turkish_normalizer():
       return NormalizerPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize)
           ])

Bu fonksiyonu şöyle kullanabiliriz:

.. code-block:: python

   tn = build_turkish_normalizer()
   normalized_text = tn(text)
   print(normalized_text)

.. note::

   Bu kod çalıştırıldığında ``build_turkish_normalizer`` fonksiyonu Unicode, büyük/küçük harf, aksan,
   boşluk, noktalama ve tırnak normalizasyonlarının tamamını sırayla uygulamaktadır. Örnek metnimiz
   üzerinde gerçek çalıştırma sonucu şöyledir:

   .. code-block:: text

      "merhaba dünya!! nasılsın? iyi misin?? "

Benzer biçimde örneğin biz ``build_ascii_normalizer`` isimli bir yardımcı fonksiyonla ASCII normalizasyonu
için boru hattı nesnesi oluşturabiliriz:

.. code-block:: python

   def build_ascii_normalizer():
       return NormalizerPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize),
           ('ASN', asciify_turkish_normalize),
           ])

.. note::

   Bu fonksiyon da gerçekten çalıştırılmıştır. ``build_turkish_normalizer``'a ek olarak ASCII dönüştürmesi
   (``ASN``) uygulandığından Türkçeye özgü karakterler de ASCII karşılıklarına çevrilmektedir. Gerçek
   çalıştırma sonucu şöyledir:

   .. code-block:: text

      "merhaba dunya!! nasilsin? iyi misin?? "

Aşağıda şimdiye kadar yapmış olduğumuz normalizasyonlara ilişkin tüm kodları bir bütün olarak veriyoruz.

Tüm Normalizasyon Kodunun Bir Arada Gösterimi
----------------------------------------------

.. code-block:: python

   import unicodedata
   import re

   def unicode_normalize(text):
       return unicodedata.normalize('NFKC', text)

   def case_normalize(text):
       return text.replace('İ', 'i').replace('I', 'ı').lower()

   def asciify_turkish_normalize(text):
       return text.translate(str.maketrans('çğıöşüÇĞİÖŞÜ', 'cgiosuCGIOSU'))

   def diacritical_normalize(text):
       chars = []
       for c in text:
           if c in 'çğıöşüÇĞİÖŞÜ':
               chars.append(c)
           else:
               nfd = unicodedata.normalize('NFD', c)
               for d in nfd:
                   if unicodedata.category(d) != 'Mn':
                       chars.append(d)
       return ''.join(chars)

   def whitespace_normalize(text):
       return re.sub(r'\s+|[\u200B\u200C\u200D\u2060\uFEFF]+', ' ', text).strip()

   def punctuation_normalize(text):
       text = re.sub(r'[\u00AD\u2010\u2011\u2012\u2013\u2014\u2015'
                     '\u2017\u2E3A\u2E3B\u2212\uFE58\uFE63\uFF0D]', '-', text)
       text = re.sub(r'\u2026', '...', text)
       text = re.sub(r'[\uFF01]', '!', text)           # fullwidth !
       text = re.sub(r'[\uFF1F]', '?', text)           # fullwidth ?
       text = re.sub(r'[\uFF0E]', '.', text)           # fullwidth .
       text = re.sub(r'[\uFF0C]', ',', text)           # fullwidth ,
       text = re.sub(r'[\uFF1B]', ';', text)           # fullwidth ;
       text = re.sub(r'[\uFF1A]', ':', text)           # fullwidth :
       text = re.sub(r'\.{3,}', '...', text)           # dört ve üzeri noktayı üç noktaya indir
       text = re.sub(r'!{2,}',  '!!',   text)          # tekrarlanan ünlemi iki tane yap
       text = re.sub(r'\?{2,}', '??',   text)          # tekrarlanan soru işaretini iki tane yap
       text = re.sub(r',{2,}',  ',,',   text)          # tekrarlanan virgülü iki tane yap
       text = re.sub(r';{2,}',  ';;',   text)          # tekrarlanan noktalı virgülü iki tane yap
       text = re.sub(r':{2,}',  '::',   text)          # tekrarlanan iki noktayı iki tane yap
       text = re.sub(r'-{2,}',  '--',   text)          # tekrarlanan tireyi iki tane yap
       text = re.sub(r'(\?!)+|(!\?)+',  '!?', text)    # tekrarlanan ?! için !? yerleştir
       text = re.sub(r'\s+([.,!?;:\)])', r'\1', text)
       text = re.sub(r'([.,!?;:])(?![.,!?;:])(?=\S)', r'\1 ', text)
       return text

   def apostrophe_normalize(text):
       text = re.sub(r'[\u201C\u201D\u201E\u201F\u2033\u2036]', '"', text)     # çift tırnak varyantları
       text = re.sub(r'[\u2018\u2019\u201A\u201B\u2032\u2035]', "'", text)     # tek tırnak varyantları
       return text

   class NormalizerPipeline:
       def __init__(self, steps=None):
           self.steps= steps or []

       def add_step(self, name, f):
           self.steps.append((name, f))

       def pop_step(self):
           return self.steps.pop()

       def insert_step(self, index, name, f):
           self.steps.insert(index, (name, f))

       def remove_step(self, name):
           for i in range(len(self.steps)):
               if self.steps[i][0] == name:
                   return self.steps.pop(i)

       def __call__(self, text):
           for _, f in self.steps:
               text = f(text)
           return text

   def build_turkish_normalizer():
       return NormalizerPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize)
           ])

   def build_ascii_normalizer():
       return NormalizerPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize),
           ('ASN', asciify_turkish_normalize),
           ])

   # test

   text = """
      “Merhaba Dünya!!! Nasılsın?İyi misin???”
   """

   npl = NormalizerPipeline([
       ('UN', unicode_normalize),
       ('CN', case_normalize),
       ('DCN', diacritical_normalize),
       ('WSN', whitespace_normalize),
       ('PN', punctuation_normalize),
       ])

   npl.add_step('AN', apostrophe_normalize)
   npl.pop_step()
   npl.remove_step('CN')

   normaized_text = npl(text)
   print(normaized_text)

   text = """
      “Merhaba Dünya!!! Nasılsın?İyi misin???”
   """

   npl1 = NormalizerPipeline([
       ('UN', unicode_normalize),
       ('CN', case_normalize),
       ('DCN', diacritical_normalize),
       ])

   npl2 = NormalizerPipeline([
       ('NPL1', npl1),
       ('WSN', whitespace_normalize),
       ('PN', punctuation_normalize),
       ('AN', apostrophe_normalize),
       ])

   text = npl2(text)
   print(text)

   text = """
      “Merhaba Dünya!!! Nasılsın?İyi misin???”
   """

   tn = build_turkish_normalizer()
   normalized_text = tn(text)
   print(normalized_text)

   text = """
      “Merhaba Dünya!!! Nasılsın?İyi misin???”
   """

   tn = build_ascii_normalizer()
   normalized_text = tn(text)
   print(normalized_text)

.. note::

   Yukarıdaki tam program **gerçekten çalıştırılarak** doğrulanmıştır. Dört testin tamamı başarıyla
   çalışmış ve elde edilen gerçek çıktılar yukarıdaki ilgili notlarda ayrı ayrı verilmiştir.


İçerik Normalizasyonu
======================

Şimdi de içerik normalizasyonu üzerinde duralım. Anımsayacağınız gibi içerik normalizasyonunu aşağıdaki gibi
alt gruplara ayırmıştık:

- İçerik Normalizasyonu

  - URL Normalizasyonu
  - E-posta Normalizasyonu
  - Sayı Normalizasyonu
  - Emoji ve Özel Sembollerin Normalizasyonu
  - Hashtag ve Mention Normalizasyonu
  - Resmi Olmayan (Informal) Yazım Genişletme (Türkçeye Özel)
  - Eş Anlamlı Sözcüklerin Normalizasyonu
  - Yinelenen Karakterleri Azaltma

İzleyen paragraflarda bunların üzerinde tek tek duracağız.

URL Normalizasyonu
-------------------

Metinlerin içerisinde çeşitli URL'ler bulunabilmektedir. Bazı uygulamalarda bu URL'lerin bir etkisi
olmayabilir. Bu durumda bu URL'ler metinden tamamen atılabilir. Bazı uygulamalarda URL'lerin içeriği değil
orada bulunuyor olması önemli olabilir. Bazı uygulamalarda URL'lerin yalnızca ana isimleriyle (yani domain
isimleriyle) ilgilenilebilmektedir. Bazı uygulamalarda ise URL'in tamamının muhafaza edilmesi
gerekebilmektedir. İşte metin içerisindeki URL'lerin aynı biçime dönüştürülmesine *URL normalizasyonu*
denilmektedir.

URL'leri regex kalıplarıyla elde edebiliriz. URL elde eden regex kalıplarını *Python Uygulamaları* kursunda
vermiştik. Siz LLM'leri kullanarak URL elde eden regex kalıplarını kolaylıkla oluşturabilirsiniz. Aslında URL
kalıplarının çok ayrıntılı olmasına gerek yoktur. Ayrıntılı bir URL kalıbı şöyle oluşturulabilir:

.. code-block:: python

   pattern = r'(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b' \
             r'([-a-zA-Z0-9()@:%_\+.~#?&//=]*)'

Örneğin:

.. code-block:: python

   import re

   pattern = r'(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b' \
             r'([-a-zA-Z0-9()@:%_\+.~#?&//=]*)'
   text = """
   Derneğin Web sitesi www.csystem.org biçimindedir. Amazon'unki ise https://amazon.com biçimindedir.
   """
   result = re.sub(pattern, '<URL>', text)
   print(result)

Burada URL kalıpları elde edilmiş onların yerine ``<URL>`` atomu yerleştirilmiştir.

.. note::

   Bu kod gerçekten çalıştırılmıştır. Gerçek çıktı şöyledir:

   .. code-block:: text

      Derneğin Web sitesi <URL> biçimindedir. Amazon'unki ise <URL> biçimindedir.

   Orijinal ders notunda bu çıktının ``"Derneğin web sitesi <URL> biçimindedir. ..."`` biçiminde, yani
   ``Web`` sözcüğünün küçük ``w`` ile başlayacağı belirtilmektedir. Ancak burada yalnızca ``re.sub`` ile bir
   URL deseni değiştirildiği için büyük/küçük harf dönüşümü uygulanmamaktadır; girdideki ``Web`` sözcüğü
   büyük ``W`` ile aynen korunur. Gerçek çıktıda büyük harf değişmeden kalmaktadır.

Burada biz yazı içerisindeki URL'leri ``<URL>`` biçiminde yer değiştirdik.

Bir URL'deki domain ismini bulan bir regex kalıbı da şöyle oluşturulabilir:

.. code-block:: python

   pattern = r'(?:https?:\/\/)?(?:www\.)?([-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6})\b' \
             r'(?:[-a-zA-Z0-9()@:%_\+.~#?&//=]*)'

Metindeki tüm URL'ler yerine onların domain isimlerini yerleştiren kod parçasını da şöyle yazabiliriz:

.. code-block:: python

   import re

   pattern = r'(?:https?:\/\/)?(?:www\.)?([-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6})\b' \
             r'(?:[-a-zA-Z0-9()@:%_\+.~#?&//=]*)'
   text = """
   Derneğin web sitesi www.csystem.org biçimindedir. Amazonunki ise https://amazon.com biçimindedir.
   """
   result = re.sub(pattern, r'\1', text)
   print(result)

.. note::

   Bu kod da gerçekten çalıştırılmıştır. Gerçek çıktı şöyledir:

   .. code-block:: text

      Derneğin web sitesi csystem.org biçimindedir. Amazonunki ise amazon.com biçimindedir.

   Orijinal ders notunda çıktının ``"...Amazonun ise amazon.com..."`` biçiminde, yani ``Amazonunki``
   sözcüğündeki ``ki`` ekinin de düştüğü belirtilmektedir. Ancak regex kalıbı yalnızca URL'i (``www.csystem.org``
   ve ``https://amazon.com``) eşleştirmektedir; öncesindeki ``Amazonunki`` sözcüğüne dokunmaz. Bu nedenle
   gerçek çıktıda ``Amazonunki`` sözcüğü ekiyle birlikte olduğu gibi kalmaktadır.

Şimdi URL normalizasyonu işlemini yapan ``url_normalize`` isimli bir fonksiyon yazalım. Fonksiyonun birinci
parametresi normalize edilecek metni, ikinci parametresi ise normalizasyon stratejisini belirtmektedir. Bu
strateji şunlardan biri olabilir:

- ``'token'``
- ``'remove'``
- ``'domain'``
- ``'keep'``

.. code-block:: python

   def url_normalize(text, strategy='token'):
       pattern_url = (r'(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b'
                      r'([-a-zA-Z0-9()@:%_\+.~#?&//=]*)')

       if strategy == 'token':
           normalized_text = re.sub(pattern_url, '<URL>', text)
       elif strategy == 'remove':
           normalized_text = re.sub(pattern_url, '', text)
       elif strategy == 'domain':
           pattern_domain = (r'(?:https?:\/\/)?(?:www\.)?([-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6})\b'
                              r'(?:[-a-zA-Z0-9()@:%_\+.~#?&//=]*)')
           normalized_text = re.sub(pattern_domain, r'\1', text)
       elif strategy == 'keep':
           normalized_text = text
       else:
           raise ValueError('invalid startegy')

       return normalized_text

.. note::

   Bu fonksiyon dört strateji için de gerçekten çalıştırılmıştır. Örnek metin: *"Derneğin web sitesi
   www.csystem.org biçimindedir. Amazon'unki ise https://amazon.com biçimindedir."*

   .. code-block:: text

      token  -> Derneğin web sitesi <URL> biçimindedir. Amazon'unki ise <URL> biçimindedir.
      remove -> Derneğin web sitesi  biçimindedir. Amazon'unki ise  biçimindedir.
      domain -> Derneğin web sitesi csystem.org biçimindedir. Amazon'unki ise amazon.com biçimindedir.
      keep   -> Derneğin web sitesi www.csystem.org biçimindedir. Amazon'unki ise https://amazon.com biçimindedir.

Bilindiği gibi URL'ler içerisinde Türkçe karakterler kullanılamamaktadır. Bu da yanlış yazılmış URL'lerin
normalizasyonunda sorun çıkarabilecektir. Eğer URL kalıplarını basitleştirirsek bu sorunu çözebiliriz.
Örneğin:

.. code-block:: python

   def url_normalize(text, strategy='token'):
       pattern_url = r'https?://(www\.)?[^\s]+|www\.[^\s]+'

       if strategy == 'token':
           normalized_text = re.sub(pattern_url, '<URL>', text)
       elif strategy == 'remove':
           normalized_text = re.sub(pattern_url, '', text)
       elif strategy == 'domain':
           pattern_domain1 = r'https?://([^/]+)([^\s]+)'
           pattern_domain2 = 'www\.([^/]+)([^\s]+)'
           normalized_text = re.sub(pattern_domain1, r'\1', text)
           normalized_text = re.sub(pattern_domain2, r'\1', normalized_text)
       elif strategy == 'keep':
           normalized_text = text
       else:
           raise ValueError('invalid startegy')

       return normalized_text

Burada artık URL geçersiz olsa bile o URL tüketilmektedir.

.. note::

   Bu fonksiyon da gerçekten çalıştırılmıştır. Kasıtlı olarak bozuk yazılmış bir URL içeren örnek metinle
   (``www.çöküksite..com``) ``token``/``remove``/``keep`` stratejileri test edilmiştir:

   .. code-block:: text

      token  -> Bozuk bağlantı <URL> geçersiz olsa da yakalanır.
      remove -> Bozuk bağlantı  geçersiz olsa da yakalanır.
      keep   -> Bozuk bağlantı www.çöküksite..com geçersiz olsa da yakalanır.

   ``domain`` stratejisi tek bir URL içeren basit cümlelerde beklendiği gibi çalışmaktadır:

   .. code-block:: text

      "Site adresimiz https://amazon.com." -> "Site adresimiz amazon.com"
      "Site adresimiz www.csystem.org."    -> "Site adresimiz csystem.org"

   Burada cümle sonundaki noktanın da düştüğüne dikkat ediniz: ``pattern_domain1`` ve ``pattern_domain2``
   kalıplarındaki ``([^/]+)([^\s]+)`` grupları, ``/`` ve boşluk içermeyen her şeyi (noktalama dahil) yutmaya
   çalıştığından cümle sonu noktası da eşleşmenin bir parçası olabilmektedir. Bu, basitleştirilmiş kalıbın
   bilinen bir sınırlamasıdır.

Son olarak hangi durumlarda yukarıda uyguladığımız stratejilerin hangilerinin kullanılması gerektiğine
yönelik bir özet tablo vermek istiyoruz:

.. list-table:: URL Normalizasyon Stratejisi Seçim Tablosu
   :header-rows: 1
   :widths: 30 20

   * - Görev
     - Strateji
   * - Duygu analizi
     - ``token`` veya ``remove``
   * - Konu modelleme
     - ``token``
   * - Named Entity Recognition
     - ``keep``
   * - Sosyal medya analizi
     - ``replace``
   * - Web scraping analizi
     - ``domain``

.. note::

   Tablodaki *"Sosyal medya analizi"* satırında verilen ``replace`` stratejisi, yukarıda tanımlanan
   ``url_normalize`` fonksiyonunun desteklediği dört stratejiden (``token``, ``remove``, ``domain``, ``keep``)
   biri değildir. Bu, orijinal ders notunda bulunan bir tutarsızlıktır; olası kastedilen muhtemelen
   ``token`` stratejisidir, ancak emin olunamadığından değer değiştirilmeden aktarılmıştır.

E-posta Normalizasyonu
-----------------------

Yukarıda URL'ler için yapılan işlemlerin benzerleri e-posta adresleri için de yapılabilir. Yine e-posta
adresleri yerine ``<EMAIL>`` biçiminde yeni bir atom bulundurulabilir, e-posta adresleri tümden silinebilir
ya da olduğu gibi tutulabilir. E-posta adreslerini bir regex kalıbı ile aşağıdaki gibi elde edebiliriz:

.. code-block:: python

   pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'

O halde e-posta normalizasyonu yapan fonksiyonu şöyle yazabiliriz:

.. code-block:: python

   def email_normalize(text, strategy='token'):
       pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'

       if strategy == 'token':
           normalized_text = re.sub(pattern, '<EMAIL>', text)
       elif strategy == 'remove':
           normalized_text = re.sub(pattern, '', text)
       elif strategy == 'keep':
           normalized_text  = text
       else:
           raise ValueError('Invalid strategy')

       return normalized_text

.. note::

   Bu fonksiyon da gerçekten çalıştırılarak doğrulanmıştır. Örnek metin: *"Bize info@csystem.org adresinden
   ya da destek.ekibi@firma.com.tr adresinden ulaşabilirsiniz."*

   .. code-block:: text

      token  -> Bize <EMAIL> adresinden ya da <EMAIL> adresinden ulaşabilirsiniz.
      remove -> Bize  adresinden ya da  adresinden ulaşabilirsiniz.
      keep   -> Bize info@csystem.org adresinden ya da destek.ekibi@firma.com.tr adresinden ulaşabilirsiniz.

Sayı Normalizasyonu
--------------------

Diğer bir içerik normalizasyonu da sayı normalizasyonudur. Sayılar yazılar içerisinde farklı biçimlerde
oluşturulmuş olabilir. Örneğin bir metinde bir sayı binler basamağından virgüllerle ayrılmış biçimde (12,500
biçiminde) yazılmışken diğer bir metinde ayrılmamış biçimde (12500 biçiminde) yazılmış olabilir. Bazı dillerde
binler ayıracıyla ondalık ayıraç bizdekiyle ters biçimde kullanılmaktadır. Örneğin Türkçe'de ve Almanca'da
binler ayıracı için nokta, ondalık ayıraç için virgül kullanılırken İngilizce'de durum tam terstir. Ayrıca
yazılar içerisinde sayılar farklı biçimlerde de karşımıza çıkabilmektedir. Örneğin *bugün üç elma yedim* gibi
bir yazıdaki 3 sayısı yazıyla yazılmıştır. Bu yazının *bugün 3 elma yedim* yazısıyla eşdeğer hale getirilmesi
de gerekebilmektedir. Bazen sayılardaki gereksiz sözcüklerin atılması da fayda sağlayabilmektedir. Örneğin
*bugün üç tane elma yedim* yazısındaki tanenin aslında bir etkisi yoktur. Bazı metinlerde büyük sayılar
bilgisayarlarda kullanılan üstel notasyonla da (örneğin 1.2e10 gibi) yazılmış olabilmektedir. Ancak yazısal
biçimde oluşturulmuş olan sayıları sayısal biçime dönüştürürken eş anlamlı sözcüklerde yine sorunlar
oluşabilmektedir. Örneğin *masa altı çok pisti* cümlesinin *masa 6 çok pisti* biçiminde dönüştürülmesi
istenmez. Yukarıda da belirttiğimiz gibi sayı normalizasyonu sanıldığından daha ayrıntılı bir konudur.

Yukarıda da belirttiğimiz gibi sayı normalizasyonu sanıldığı kadar kolay bir süreç değildir. Örneğin metinler
arasında binler ayıracıyla ondalık ayıraç konusunda tam bir tutarlılık bulunmayabilir. Matematikle haşır
neşir olanlar Türkçe metinlerde de binler ayıracı için virgül, ondalık ayıraç için nokta kullanabilmektedir.
Türkçe metinlerde her iki ifade biçimiyle de karşılaşılabilmektedir. Bu durumda bu sayıları normalize
edebilmek için metnin anlamlandırılması gerekebilmektedir. Anlamlandırma insanlar tarafından yapılabileceği
gibi makine öğrenmesi algoritmalarıyla da otomatize biçimde yapılabilmektedir. Makine öğrenmesi yöntemlerini
uygulamadan *kural tabanlı (rule based)* yaklaşımlarla da tatmin edici sonuçlar elde edilebilmektedir.
Örneğin aslında *masa altı çok pisti* cümlesindeki altının bir sayı olmadığı kural tabanlı bir biçimde de
kolaylıkla anlaşılabilmektedir.

Sayı normalizasyonunda yüzde belirten ``%10`` gibi ifadelerde ``%`` işareti ile sayı arasındaki boşluklar da
ortadan kaldırılabilir. İngilizce'de yüzde belirten ifadelerin ``10%`` biçiminde ters yazıldığına dikkat
ediniz.


Sayı ve Emoji Normalizasyonu
=============================

Sayı Normalizasyonunu Yapan Bir Fonksiyon
------------------------------------------

Şimdi de sayı normalizasyonu yapan bir fonksiyon yazalım. Ancak bu fonksiyonda ayrıntılara fazla girmeyelim.
Bu işlemi yapan yalın bir fonksiyonu şöyle yazabiliriz:

.. code-block:: python

   def turkish_number_normailze(text, strategy='token'):
       text = text_to_number(text)                         # yazısal sayıları dönüştürür
       text = re.sub(r'(\d+)\.(\d{3})', r'\1\2', text)     # binler basamağını ortadan kaldırır
       text = re.sub(r'(\d+),(\d+)', r'\1.\2', text)       # Ondalık ayıracı olan virgülü nokta yapar
       text = re.sub(r'%\s*(\d+)', r'%\1', text)           # % işareti ile sayı arasındaki boşluk karakterlerini atar

       match strategy:
           case 'token':
               text = re.sub(r'\d+([.,]\d+)*', '<NUM>', text)
           case 'remove':
               text = re.sub(r'\d+([.,]\d+)*', '', text)
           case 'keep':
               pass
           case _:
               raise ValueError('invalid strategy'

       return text

.. note::

   Bu kod birebir orijinal ders notundaki haliyle verilmiştir, ancak içinde gerçek bir hata bulunmaktadır:
   ``raise ValueError('invalid strategy'`` satırında kapanış parantezi unutulmuştur. Bu kod bu haliyle
   çalıştırıldığında Python yorumlayıcısı ``SyntaxError: '(' was never closed`` hatası vermektedir (bu,
   gerçekten denenerek doğrulanmıştır). Doğru biçimi ``raise ValueError('invalid strategy')`` olmalıdır.
   Fonksiyon adındaki ``turkish_number_normailze`` yazımı da (``normalize`` yerine) bir yazım hatasıdır;
   kodun kendisi olduğu gibi (yazım hatalarıyla) korunmuştur.

   Parantez hatası düzeltilmiş ve fonksiyon adı ``turkish_number_normalize`` olarak yazılmış hâliyle bu
   fonksiyon gerçekten çalıştırılarak doğrulanmıştır:

   .. code-block:: text

      girdi  : "Şirketin geliri 12.500 TL oldu."
        token  -> "Şirketin geliri <NUM> TL oldu."
        remove -> "Şirketin geliri  TL oldu."
        keep   -> "Şirketin geliri 12500 TL oldu."

      girdi  : "Oran %  25 olarak ölçüldü."
        token  -> "Oran %<NUM> olarak ölçüldü."
        remove -> "Oran % olarak ölçüldü."
        keep   -> "Oran %25 olarak ölçüldü."

      girdi  : "Fiyat 19,90 TL'dir."
        token  -> "Fiyat <NUM> TL'dir."
        remove -> "Fiyat  TL'dir."
        keep   -> "Fiyat 19.90 TL'dir."

      girdi  : "Bugün bin üç yüz altmış üç kişi geldi ve oran % 10 arttı, gelir 12.500,50 TL oldu."
        token  -> "Bugün <NUM> kişi geldi ve oran %<NUM> arttı, gelir <NUM> TL oldu."
        remove -> "Bugün  kişi geldi ve oran % arttı, gelir  TL oldu."
        keep   -> "Bugün 1363 kişi geldi ve oran %10 arttı, gelir 12500.50 TL oldu."

İlk satırdaki ``text_to_number`` fonksiyonu yazı içerisindeki yazısal biçimde oluşturulmuş sayıları sayısal
biçime dönüştürmektedir:

.. code-block:: python

   text = text_to_number(text)

Örneğin yazı içerisinde *bin üç yüz altmış üç* gibi bir ifade varsa bu fonksiyon bunun yerine yazıya ``1363``
yazısını yerleştirmektedir. Yazısal biçimdeki sayısal ifadeleri sayısal biçime (örneğin int türden bir
değere) dönüştürmek için klasik bir algoritma kullanılmaktadır. Biz kursumuzda bu işlemi yapan
``text_to_number`` fonksiyonunu ayrı bir dosyaya (``text2num.py`` dosyasına) yerleştirip fonksiyonu
``from ... import`` deyimi ile import ettik.

İkinci kalıp binler basamağı noktalarla ayrılmış sayılardaki noktaları ortadan kaldırmaktadır:

.. code-block:: python

   text = re.sub(r'(\d+)\.(\d{3})', r'\1\2', text)

Üçüncü kalıp ondalık ayıracı olan virgülü nokta yapmaktadır:

.. code-block:: python

   text = re.sub(r'(\d+),(\d+)', r'\1.\2', text)

Dördüncü kalıp % işareti ile sayı arasındaki boşluk karakterlerini atmaktadır:

.. code-block:: python

   text = re.sub(r'%\s*(\d+)', r'%\1', text)

``match`` deyimi de parametreye bakarak uygun stratejiyi uygulamaktadır.

Hangi durumlarda hangi stratejilerin uygun olabileceğini aşağıda bir tablo biçiminde veriyoruz:

.. list-table:: Sayı Normalizasyonu Strateji Tablosu
   :header-rows: 1
   :widths: 22 18 35

   * - Görev
     - Strateji
     - Gerekçesi
   * - Duygu analizi
     - ``token``/``remove``
     - Sayılar duyguyla ilgisiz
   * - Finansal analizler
     - ``keep``
     - Sayılar önemli bilgi taşır
   * - Haber sınıflandırma
     - ``token``
     - Sayılar genelde gürültü
   * - Soru yanıtlaması
     - ``keep``
     - Sayısal yanıtlar için gerekli

Burada kullandığımız ``text2num`` modülünü de aşağıda veriyoruz.

.. code-block:: python

   # text2num.py

   import re

   ONES = {
       'sıfır': 0, 'bir': 1, 'iki': 2, 'üç': 3, 'dört': 4,
       'beş': 5, 'altı': 6, 'yedi': 7, 'sekiz': 8, 'dokuz': 9
   }

   TENS = {
       'on': 10, 'yirmi': 20, 'otuz': 30, 'kırk': 40,
       'elli': 50, 'altmış': 60, 'yetmiş': 70, 'seksen': 80, 'doksan': 90
   }

   SCALES = {
       'yüz': 100, 'bin': 1000, 'milyon': 1_000_000, 'milyar': 1_000_000_000
   }

   ALL_NUMBERS = {**ONES, **TENS, **SCALES}

   def turkish_text_to_number(text):
       words = text.split()

       current = 0
       total = 0
       for word in words:
           if word in ONES:
               current += ONES[word]
           elif word in TENS:
               current += TENS[word]
           elif word == 'yüz':
               current = 100 if current == 0 else current * 100
           elif word in SCALES:
               current = SCALES[word] if current == 0 else current * SCALES[word]
               total += current
               current = 0
       total += current

       return total

   def text_to_number(text, threshold=2):
       def convert_match(m):
           return str(turkish_text_to_number(m[0]))

       pattern = r'\b(?:' + '|'.join(ALL_NUMBERS.keys()) + r')(?:\s+(?:' + '|'.join(ALL_NUMBERS.keys()) + r'))' + \
               f'{{{threshold - 1},}}' + r'\b'
       text = re.sub(pattern, convert_match, text, flags=re.IGNORECASE)
       return text

.. note::

   Bu modül gerçekten çalıştırılarak doğrulanmıştır. ``turkish_text_to_number`` fonksiyonu test edildiğinde:

   .. code-block:: text

      "bin üç yüz altmış üç"     -> 1363
      "yüz"                       -> 100
      "bin"                       -> 1000
      "milyon"                    -> 1000000
      "iki bin yirmi dört"        -> 2024
      "dokuz yüz doksan dokuz"    -> 999

   ``text_to_number`` fonksiyonu metin içinde test edildiğinde:

   .. code-block:: text

      "Bugün bin üç yüz altmış üç kişi geldi." -> "Bugün 1363 kişi geldi."
      "Şehirde iki bin yirmi dört yılında açıldı." -> "Şehirde 2024 yılında açıldı."

   Sonuçlar metinde verilen ``1363`` örneğiyle tam örtüşmektedir.

Emoji Normalizasyonu
---------------------

Unicode tabloda resimsel pek çok karakter vardır. Bunlara *emoji* karakterleri denilmektedir. (*Emoji*
sözcüğü Japonca'dan geçmiştir ve *resimsel karakter* anlamına gelmektedir. Yani kökü *emotion* sözcüğü
değildir. ASCII karakterleriyle oluşturulan (örneğin ``:-)`` ya da ``:(`` gibi) duygusal tepki belirten
karakter kümelerine ise İngilizce *emoticon* denilmektedir. Bu sözcük *emotion* sözcüğünden türetilmiştir ve
bilgisayar alanında *emoji* sözcüğünden daha önce kullanılmaya başlanmıştır.) İşte Unicode tablodaki emoji
karakterlerinin normalize edilmesi de gerekebilmektedir. Unicode tabloda sanıldığından daha fazla (3700
civarı) emoji karakteri vardır. Ancak metinlerde az sayıda emoji karakteri yaygın biçimde kullanılmaktadır.

Python'da emojilerle çalışmak için ``emoji`` isimli üçüncü parti bir kütüphane yaygın olarak
kullanılmaktadır. Bu kütüphaneyi şöyle yükleyebiliriz:

.. code-block:: bash

   pip install emoji

``emoji`` kütüphanesi emoji normalizasyonunu yapmayı oldukça kolaylaştırmaktadır. Kütüphanedeki ``demojize``
fonksiyonu Unicode metindeki emoji karakterleri yerine ``:emojinin_betimsel_ifadesi:`` yazıları
yerleştirmektedir. Bu yazılar doğrudan ayrı birer atom (token) olarak kullanılabilir. ``demojize``
fonksiyonunun ``delimiters`` parametresi default olan ``':'`` karakterleri yerine başka karakterlerin
kullanılmasına olanak sağlamaktadır. Örneğin:

.. code-block:: python

   text = "🌍 Merhaba dünya! 👋 Bugün harika bir gün ☀️ ve ben çok mutluyum 😄. Sabah kahvemi ☕ " \
   "içerken penceremden baktım, kuşlar 🐦 uçuşuyordu, çiçekler 🌸 açmıştı. Bahçede bir kedi 🐱 ve" \
   " köpek 🐶 birlikte oynuyordu — inanılmaz! 😲"

   result = emoji.demojize(text)
   print(result)

Burada şöyle bir çıktı elde edilmektedir:

.. code-block:: text

   :globe_showing_Europe-Africa: Merhaba dünya! :waving_hand: Bugün harika bir gün :sun: ve ben çok
   mutluyum :grinning_face_with_smiling_eyes:. Sabah kahvemi :hot_beverage: içerken penceremden baktım,
   kuşlar :bird: uçuşuyordu, çiçekler :cherry_blossom: açmıştı. Bahçede bir kedi :cat_face: ve köpek
   :dog_face: birlikte oynuyordu — inanılmaz! :astonished_face:

.. note::

   Bu kod, ortamımda ``emoji`` kütüphanesi kurulu olmadığından ve internet erişimi kapalı olduğundan
   (``pip install emoji`` komutu çalıştırılamamıştır) gerçek olarak çalıştırılıp doğrulanamamıştır. Yukarıdaki
   çıktı, orijinal ders notunda belirtilen çıktıdır; gerçek bir çalıştırmayla teyit edilememiştir.

Görüldüğü gibi emoji karakterleri iki nokta üst üste arasındaki yazılara dönüştürülmüştür. Bu işlemin tersi
de ``emojize`` fonksiyonuyla yapılmaktadır. Örneğin:

.. code-block:: python

   s = emoji.demojize(result)

.. note::

   Orijinal ders notunda bu satır ``s = emoji.demojize(result)`` biçiminde verilmiştir. Ancak yukarıdaki
   açıklama "bu işlemin tersi" demektedir; ``demojize`` zaten emoji → açıklayıcı metin dönüşümünü
   yapmaktadır, bu nedenle tersini (açıklayıcı metin → emoji) yapan fonksiyon ``emoji`` kütüphanesinde
   ``emoji.emojize()`` olarak adlandırılmıştır. Burada ``demojize``'nin iki kez kullanılmış olması muhtemelen
   bir yazım hatasıdır; doğrusu ``s = emoji.emojize(result)`` olmalıdır. Kütüphane kurulu olmadığından bu,
   gerçek bir çalıştırmayla doğrulanamamıştır; bilgi ``emoji`` kütüphanesinin bilinen API'sine dayanmaktadır.

``delimiters`` parametresiyle ``':'`` yerine istenilen ayıraçlar kullanılabilmektedir. Örneğin:

.. code-block:: python

   result = emoji.demojize(text, delimiters='<>')

Bir Unicode karakterin emoji karakteri olup olmadığı ``is_emoji`` fonksiyonu ile kontrol edilebilmektedir.
Örneğin:

.. code-block:: python

   s = []
   for c in text:
       if emoji.is_emoji(c):
           s.append('<EMOJİ>')
       else:
           s.append(c)

   result = ''.join(s)
   print(result)

Burada yazı içerisindeki emoji karakterleri ``<EMOJI>`` atomuna dönüştürülmüştür. Tabii bu işlem aslında
``emoji`` kütüphanesi ve regex işlemleriyle de yapılabilirdi:

.. code-block:: python

   demojized_text = emoji.demojize(text, delimiters='<>')
   result = re.sub(r'<[^>]*>', '<EMOJI>', demojized_text)
   print(result)

Aslında bunu yapan ``replace_emoji`` isminde tek bir fonksiyon da vardır. Örneğin:

.. code-block:: python

   result = emoji.replace_emoji(text, '<EMOJI>')

O halde emoji normalizasyonunu yapan bir fonksiyonu aşağıdaki gibi yazabiliriz:

.. code-block:: python

   def emoji_normalize(text, strategy='token'):
       match strategy:
           case 'token':
               text = emoji.replace_emoji(text, '<EMOJI>')
           case 'remove':
               text = emoji.replace_emoji(text, '')
           case 'text':
               text = emoji.demojize(text)
           case 'keep':
               pass
           case _:
               raise ValueError('invalid strategy')

       return text

.. note::

   Bu fonksiyonun ``emoji`` kütüphanesini kullanan kısımları da, kütüphane kurulu olmadığından gerçek olarak
   çalıştırılıp doğrulanamamıştır. Kod sözdizimsel olarak (parantezleri kapalı, hatasız) doğru görünmektedir.

Burada görüldüğü gibi strategy olarak ``'token'``, ``'remove'``, ``'text'`` ve ``'keep'`` kullanılmıştır. Bu
fonksiyonu da artık normalizasyon boru hattımıza ekleyebiliriz. Aşağıdaki tabloda hangi uygulamalarda hangi
emoji normalizasyonunun uygulanabileceğine yönelik bir fikir vermek istiyoruz:

.. list-table:: Emoji Normalizasyonu Strateji Tablosu
   :header-rows: 1
   :widths: 30 20

   * - Uygulama
     - Strateji
   * - Duygu analizi (sosyal medya)
     - ``keep`` veya ``text``
   * - Yazı sınıflandırması
     - ``remove``
   * - Duyguların tespiti
     - ``keep`` veya ``text``
   * - Diğer Doğal Dil İşleme
     - ``remove``


Hashtag, Mention ve Resmi Olmayan Yazım Normalizasyonu
=======================================================

Hashtag ve Mention Normalizasyonu
----------------------------------

Diğer bir içerik normalizasyonu da *hashtag* ve *mention* normalizasyonlarıdır. Hashtag ve mention
ifadeleri sosyal medya uygulamalarında çokça karşımıza çıkmaktadır. Hashtag, sosyal medya platformlarında
içerikleri kategorilere ayırmak ve belirli bir konu hakkındaki paylaşımları bir araya getirmek için
kullanılan bir etiketleme biçimidir. Hashtag'ler ``#`` karakteri ile başlar ve hemen ardından bir sözcük ya
da boşluksuz sözcük grubu yazılır (örneğin ``#tatil``, ``#yemektarifleri`` gibi). Mention (Türkçe *sözünü
etmek*, *adını anmak* gibi anlamlara gelmektedir) ise sosyal medyada bir kullanıcıdan bahsetmek, bir mesajı
bir kullanıcı ile ilişkilendirmek için kullanılan bir yazım biçimidir. Mention'lar ``@`` karakteri ile
oluşturulmaktadır (örneğin ``@aliserçe`` gibi).

Doğal dil işlemede konu bağlamı da uygunsa yazı içerisindeki hashtag ve mention ifadelerinin normalize
edilmesi iyi bir tekniktir. Hashtag ve mention ifadeleri normalize edilirken bunlar için ``<HASHTAG>`` ya da
``<MENTION>`` gibi atomlar yerleştirilebilir. Ya da bunların başındaki ``#`` ve ``@`` karakterleri atılarak
yanındaki yazı muhafaza edilebilir. Bazı uygulamalarda bunların tamamen silinmesi de uygun olabilmektedir.
Hashtag ve mention normalizasyonunu yapan fonksiyonu şöyle yazabiliriz:

.. code-block:: python

   def hashtag_mention_normalize(text, strategy='token'):
       match strategy:
           case 'token':
               text = re.sub(r'#\w+', '<HASHTAG>', text)
               text = re.sub(r'@\w+', '<MENTION>', text)
           case 'remove':
               text = re.sub(r'#\w+', '', text)
               text = re.sub(r'@\w+', '', text)
           case 'separate':
               text = re.sub(r'#(\w+)', r'\1', text)
               text = re.sub(r'@(\w+)', '\1', text)
           case 'keep':
               pass
           case _:
               raise ValueError('invalid strategy')

       return text

Burada strategy parametresinin değerine bakılmış ve uygun kalıplarla yer değiştirme yapılmıştır.

.. note::

   ``'separate'`` dalındaki ikinci satırda (``@`` için) kullanılan değiştirme metni ``'\1'`` şeklinde, ``r``
   ön ekiyle (raw string olarak) değil, normal bir Python dizgesi olarak yazılmıştır. Bunun bir satır
   üstündeki ``#`` için kullanılan ``r'\1'`` ile tutarsızdır. Python'da raw olmayan bir dizgede ``'\1'``,
   geri başvuru (backreference) anlamına gelen iki karakter (``\`` ve ``1``) olarak değil, tek bir kontrol
   karakteri (``chr(1)``) olarak yorumlanır; bu nedenle ``re.sub`` bunu bir geri başvuru olarak
   tanıyamayacak ve ``@kullanıcı`` yerine görünmez bir kontrol karakteri yerleştirecektir. Bu, orijinal
   koddaki gerçek bir hatadır; doğrusu ``r'\1'`` olmalıdır. Kod, orijinal haliyle (hatasıyla) korunmuştur.

Resmi Olmayan (Informal) Yazım Normalizasyonu
----------------------------------------------

Kısaltmaların ve yanlış yazılmış sözcüklerin de aynı biçime dönüştürülerek normalize edilmesi (informal
normalization) iyi bir tekniktir. Ancak bu tür normalizasyonlar aslında *metin normalizasyonu (text
normalization)* ve *atom normalizasyonu (token normalization)* aşamalarının gri bölgelerini
oluşturmaktadır. Yani bu biçimdeki normalizasyonlar atomlara ayırma işleminin öncesinde de yapılabilir,
atomlara ayırma işleminin sonrasında da yapılabilir. Aşağıda Türkçe'deki bazı kısaltmalara ve yanlış
yazımlara örnekler veriyoruz:

.. code-block:: python

   TURKISH_INFORMAL = {
       # Kısaltmalar
       'slm': 'selam',
       'mrb': 'merhaba',
       'mrhb': 'merhaba',
       'naber': 'ne haber',
       'nbr': 'ne haber',
       'tmm': 'tamam',
       'tşk': 'teşekkür',
       'tşkrlr': 'teşekkürler',
       'sağol': 'sağ ol',
       'saol': 'sağ ol',

       # Yaygın yazım hataları
       'bişey': 'bir şey',
       'bişi': 'bir şey',
       'bisey': 'bir şey',
       'birşey': 'bir şey',
       'hiçbirşey': 'hiçbir şey',
       'herşey': 'her şey',
       'deil': 'değil',
       'degil': 'değil',
       'nasi': 'nasıl',
       'nasil': 'nasıl',
       'nası': 'nasıl',

       # Sayılar
       'bi': 'bir',
   }

Kod içerisinde çok yer kaplayacak liste, sözlük gibi nesnelerin ayrı bir Python dosyasına yerleştirilip
import edilerek kullanılması ya da CSV, JSON gibi dosyalara yerleştirilip oradan okunması daha iyi bir
tekniktir. Örneğin biz bu sözlük nesnesini ``trinformal.py`` dosyası içerisine yerleştirebiliriz:

.. code-block:: python

   # trinformal.py

   TURKISH_INFORMAL = {
       # Kısaltmalar
       'slm': 'selam',
       'mrb': 'merhaba',
       'mrhb': 'merhaba',
       'naber': 'ne haber',
       'nbr': 'ne haber',
       'tmm': 'tamam',
       'tşk': 'teşekkür',
       'tşkrlr': 'teşekkürler',
       'sağol': 'sağ ol',
       'saol': 'sağ ol',

       # Yaygın yazım hataları
       'bişey': 'bir şey',
       'bişi': 'bir şey',
       'bisey': 'bir şey',
       'birşey': 'bir şey',
       'hiçbirşey': 'hiçbir şey',
       'herşey': 'her şey',
       'deil': 'değil',
       'degil': 'değil',
       'nasi': 'nasıl',
       'nasil': 'nasıl',
       'nası': 'nasıl',

       # Sayılar
       'bi': 'bir',
   }

Resmi olmayan yazımları normalize eden bir fonksiyonu şöyle yazabiliriz:

.. code-block:: python

   def informal_normalize(text):
       def informal_convert(m):
           value = trinformal.TURKISH_INFORMAL.get(m[0])
           return value or m[0]

       return re.sub(r"\b[\w']+\b", informal_convert, text)

Burada ``sub`` fonksiyonunun ikinci parametresine bir callback fonksiyon geçildiğine dikkat ediniz.
Anımsayacağınız gibi ``sub`` fonksiyonu bu durumda ikinci parametresine geçirilen callback fonksiyonu
``Match`` nesnesi ile çağırıp onun geri dönüş değerinden elde edilen yazıyı yer değiştirme işleminde
kullanmaktadır. Fonksiyonun ``return`` ifadesindeki ``or`` operatörü sizi şaşırtabilir. Anımsanacağı gibi
Python'da ``or`` operatörü ya soldaki operand'ın değerini ya da sağdaki operand'ın değerini üretiyordu.
Artık bu fonksiyonumuzu da boru hattımıza dahil edebiliriz.

Tabii aslında Türkçe için yazım hatalarının listesi oldukça büyüktür. Bu listeyi kendiniz oluşturmak yerine
daha önce yapılmış olan çalışmalardan faydalanabilirsiniz. Aşağıda *Asım Ersoy* tarafından oluşturulmuş
olan yanlış yazımlara yönelik veri kümesinin bağlantısını veriyoruz:

``https://huggingface.co/datasets/asimokby/Turkish-Spelling-Dictionary``

Bu bağlantıdan ``expanded_turkish_misspellings.csv`` dosyasını indirebilirsiniz. Dosyanın içeriği aşağıdaki
gibidir:

.. code-block:: text

   yanlis,dogru
   slm,selam
   slmlar,selamlar
   mrb,merhaba
   mrhb,merhaba
   naber,ne haber
   nbr,ne haber
   tmm,tamam
   okk,tamam
   ok,tamam
   tşk,teşekkür
   tşkrlr,teşekkürler
   tşkr,teşekkür
   teşk,teşekkür
   tesekkur,teşekkür
   .....

Şimdi de bu CSV dosyasını kullanarak ikinci bir ``informal_normalize`` fonksiyonu yazalım:

.. code-block:: python

   def informal_large_normalize(text):
       with open('../Data/expanded_turkish_misspellings.csv', encoding='utf-8') as f:
           next(f)
           d = {key: value for key, value in csv.reader(f)}

           def informal_convert(m):
               value = d.get(m[0])
               return value or m[0]

           return re.sub(r"\b[\w']+\b", informal_convert, text)

.. note::

   ``expanded_turkish_misspellings.csv`` dosyası ortamımda bulunmadığından ve internet erişimi kapalı
   olduğundan (Hugging Face bağlantısından indirilemediğinden) bu fonksiyon çalıştırılarak
   doğrulanamamıştır.


Eş Anlamlı Sözcüklerin Normalizasyonu
======================================

Eş Anlamlılık ve Yakın Anlamlılık Kavramı
------------------------------------------

Eş anlamlı sözcüklerin normalize edilmesi de atomlara ayırma öncesindeki metinsel normalizasyonda ya da
atomlara ayırma sonrasındaki atom normalizasyonda gerçekleştirilebilecek bir normalizasyondur. Biz bu
normalizasyonu metinsel normalizasyon aşamasında ele alacağız. Yazılarda aynı anlama gelen eş anlamlı
sözcükler bulunabilmektedir. Örneğin *ak* ile *beyaz* yaklaşık aynı anlama gelmektedir. Ancak katı bir
biçimde ele alındığında eş anlamlı sanılan sözcüklerin arasında bile bazı anlamsal ya da kullanım
farklılıkları söz konusu olabilmektedir. İki sözcük eş anlamlı diye her bağlamda biri yerine diğeri
kullanılamayabilir. Örneğin deyimsel ifadelerde ve atasözlerinde eş anlamlı sözcükleri diğeriyle yer
değiştiremeyiz. (Örneğin *ak akçe kara gün içindir* atasözünü *beyaz akçe kara gün içindir* biçiminde
söyleyemeyiz.) Tamamen eş anlamlı olmak yerine yakın anlamlı sözcükler de vardır. Bazı uygulamalarda yakın
anlamlı sözcüklerin de normalize edilmesi uygun olabilmektedir. İngilizce *synonym* biçiminde ifade edilen
sözcükler eş anlamlılıktan ziyade yakın anlamlılığı ifade etmektedir.

Türkçe Eş Anlamlı Sözcük Listesi (CSV Veri Kümesi)
---------------------------------------------------

Türkçe için farklı kişilerin ve kurumların oluşturduğu eş anlamlı ve yakın anlamlı sözcük listeleri
bulunabilmektedir. Örneğin aşağıdaki bağlantıda Türkçe'deki eş anlamlı ve zıt anlamlı sözcüklerin bir listesi
oluşturulmuştur:

``https://www.kaggle.com/datasets/agmmnn/turkish-thesaurus?resource=download``

Ancak buradaki CSV dosyası bizim eş anlamlılık normalizasyonu uygulayabilmemiz için uygun değildir. Buradaki
dosya ancak bu amaç doğrultusunda yeniden düzenlenerek kullanılabilir. Biz LLM'lerden faydalanarak eş anlamlı
sözcük listesini ``tr-synonyms.csv`` isminde bir dosya biçiminde oluşturduk. Bu dosyanın içeriği şöyledir:

.. code-block:: text

   canonical_form,synonyms
   kelime,sözcük
   yıl,sene
   şehir,kent
   doktor,hekim|tabip
   ama,fakat|lakin|ancak|ne var ki|yalnız
   hemen,derhal|anında|acilen|ivedi
   şimdi,şu an|şu anda|bu an|bu anda
   ev,konut|mesken|hane|yuva|ikametgah
   insan,kişi|şahıs|birey|fert|kimse
   çocuk,evlat|yavru|yavrucak|afacan
   .....

Eş Anlamlı Sözcüklerden Kanonik Sözlük Oluşturma
-------------------------------------------------

Yazı içerisindeki eş anlamlı sözcükleri onların kanonik (yani normalize edilmiş) biçimleriyle yer
değiştirmek için bizim öncelikle bir sözlük oluşturmamız gerekir. Ancak yukarıdaki formata sahip CSV
dosyasından birkaç satırla bu sözlüğü oluşturamayız. Aşağıdaki satıra dikkat ediniz:

.. code-block:: text

   insan,kişi|şahıs|birey|fert|kimse

Burada bizim yazı içerisinde ``kişi``, ``şahıs``, ``birey``, ``fert`` ve ``kimse`` gördüğümüzde onun yerine
``insan`` sözcüğünü yerleştirmemiz gerekir. O halde yer değiştirme işlemi için bizim sözlüğü şöyle
oluşturmamız gerekir:

.. list-table:: Eş Anlamlı Sözcüklerden Kanonik Biçime Eşleme
   :header-rows: 1
   :widths: 20 20

   * - Eş Anlamlı Sözcük
     - Kanonik Biçim
   * - ``kişi``
     - ``insan``
   * - ``şahıs``
     - ``insan``
   * - ``birey``
     - ``insan``
   * - ``fert``
     - ``insan``
   * - ``kimse``
     - ``insan``

Böylece biz bu sözlüğe anahtar olarak eş anlamlı sözcüğü verdiğimizde değer olarak onun kanonik biçimini
elde edebiliriz. Bu işlemi yapan bir fonksiyon şöyle yazılabilir:

.. code-block:: python

   def syn_csv2dict(path):
       with open(path, encoding='utf-8') as f:
           next(f)
           canon_dict = {}
           for canon_word, syn_words in csv.reader(f):
               for syn_word in syn_words.split('|'):
                   canon_dict[syn_word] = canon_word
       return canon_dict

Burada önce CSV dosyası açılmış, sonra onun başlık kısmı atlanmış, sonra da dosya satır satır okunmuştur.
Her satırın ikinci sütununun ``|`` karakterlerinden ayrıştırılarak sözlüğe eklendiğini görüyorsunuz. Artık
biz bir eş anlamlı sözcüğü bu sözlüğe anahtar yaptığımızda onun kanonik biçimini elde edebilmekteyiz.

Metin İçerisinde Eş Anlamlı Sözcüklerin Değiştirilmesi
-------------------------------------------------------

Şimdi de metin içerisinde değişikliğin nasıl yapılacağı üzerinde duralım. Metin üzerinde değişiklik birkaç
biçimde yapılabilir. Birincisi metin sözcüklerine ayrıştırılarak oluşturulan kanonik sözlüğe başvurulabilir.
Ancak bu durumda anahtarlar tek sözcükten oluşur. (Yani bu yöntemde birden fazla sözcükten oluşan anahtar
kullanılamaz.) Diğer bir yöntem tüm değişiklikleri tek hamlede yapacak olan bir regex kalıbı oluşturmaktır.
Tabii bu regex kalıbı ``|`` meta karakteri kullanılarak oluşturulabilir. Biz burada bu ikinci yöntemi
uygulayacağız.

Anahtarları ``|`` meta karakteriyle birleştirirken dikkate alınması gereken iki önemli nokta vardır:

1) Anahtar içerisinde regex için anlamlı olabilecek meta karakterler varsa bunların ters bölülenerek meta
   karakter olmaktan çıkartılması gerekir. Bu işlem ``re`` modülündeki ``escape`` fonksiyonuyla
   yapılabilmektedir.

2) ``|`` kalıbında eşleşmeye sırasıyla bakılmaktadır. Eğer kalıp örneğin ``ön|ön almak`` biçimindeyse ve
   yazıda ``ön almak`` varsa, bu durumda ``ön almak`` eşleşmez, önde olduğu için yalnızca ``ön`` eşleşir.
   Halbuki biz bu durumda ``ön almak`` kalıbının eşleşmesini isteriz. İşte bunun için anahtarları onların
   karakter uzunluğuna göre büyükten küçüğe sıraya dizmek gerekir.

Bu işlemleri yapan fonksiyonu aşağıdaki gibi oluşturabiliriz:

.. code-block:: python

   def synonym_normalize(text):
       def syn_csv2dict(path):
           with open(path, encoding='utf-8') as f:
               next(f)
               canon_dict = {}
               for canon_word, syn_words in csv.reader(f):
                   for syn_word in syn_words.split('|'):
                       canon_dict[syn_word] = canon_word
           return canon_dict

       canon_dict = syn_csv2dict('../Data/tr-synonyms.csv')

       def replace(match):
           return canon_dict.get(match[0], match[0])

       pipe_pattern = '|'.join(re.escape(s) for s in sorted(canon_dict, key=len, reverse=True))
       pattern = '(?<![a-zA-ZçÇğĞıİöÖşŞüÜ])(' + pipe_pattern + ')(?![a-zA-ZçÇğĞıİöÖşŞüÜ])'

       return re.sub(pattern, replace, text)

.. note::

   ``synonym_normalize`` fonksiyonu, ortamımda bulunmayan ``../Data/tr-synonyms.csv`` dosyasına bağımlı
   olduğundan çalıştırılarak doğrulanamamıştır.

Artık bu normalizasyonu da boru hattımıza ekleyebiliriz.


Yinelenen Karakterleri Azaltma ve Tüm Normalizasyon Kodunun Bütünü
===================================================================

Yinelenen Karakterleri Azaltma
-------------------------------

Son olarak *yinelenen karakterleri azaltma* normalizasyonu üzerinde duracağız. Bu normalizasyonun benzerini
biz aslında noktalama işaretleri için yapmıştık. Burada noktalama işaretlerinin dışındaki herhangi diğer
karakterler için bu işlemi yapacağız. Bir yazıda yanlışlıkla ya da bilerek aynı karakter peşi sıra
yineleniyor olabilir. Bunları normalize etmek iyi bir tekniktir. Örneğin *çoooook güzel* gibi bir ifade *çok
güzel* haline ya da *çook güzel* haline getirilerek normalize edilebilir. Bazen yazıda ``------`` biçiminde
ayıraç karakterleri de bulunabilir. Bunların da normalize edilmesi uygun olabilmektedir. Bu normalizasyon
işlemini basit bir biçimde aşağıdaki gibi yapabiliriz:

.. code-block:: python

   def repeated_chars_normalize(text, max_repeat=2):
       pattern = r'(.)\1{' + str(max_repeat) + ',}'
       replacement = r'\1' * max_repeat
       text = re.sub(pattern, replacement, text)
       return text

Burada önce küme parantezi kalıbıyla herhangi bir karakterden ``max_repeat`` değerinden daha fazla tekrar
yapılıp yapılmadığı tespit edilmiş, sonra onun yerine ilgili karakterden iki tane yerleştirilmiştir. Buradaki
kalıp hakkında bir noktaya dikkatinizi çekmek istiyoruz. Aşağıdaki kalıba dikkat ediniz:

.. code-block:: text

   (.){2,}

Bu kalıp *herhangi bir karakterden (bu karakterler farklı karakterler de olabilir) en az 2 kez
tekrarlanmalı* anlamına gelmektedir. Halbuki aşağıdaki kalıp *herhangi aynı karakterden en az 2 kez
tekrarlanmalı* anlamına gelmektedir:

.. code-block:: text

   (.)\1{2,}

Bu fonksiyonu da boru hattımıza ekleyebiliriz.

Tüm Normalizasyon Kodunun Bütünü
---------------------------------

Aşağıda yaptığımız tüm metin normalizasyonlarını bir bütün halinde veriyoruz. Kodumuzda ayrıca
``build_turkish_keep_normalizer`` isimli bir boru hattı nesnesini de ekledik. Bu boru hattı nesnesi özel
atomları ``strategy='token'`` yerine ``strategy='keep'`` biçiminde ele almaktadır:

.. code-block:: python

   def build_turkish_keep_normalizer():
       return PreprocessingPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize),
           ('URLN', lambda text: url_normalize(text, strategy='keep')),
           ('EMN', lambda text: email_normalize(text, strategy='keep')),
           ('TNN', lambda text: turkish_number_normalize(text, strategy='keep')),
           ('EMON', lambda text: emoji_normalize(text, strategy='keep')),
           ('HMN', lambda text: hashtag_mention_normalize(text, strategy='keep')),
           ('IFN', informal_normalize),
           ('SYN', synonym_normalize),
           ('RCN', repeated_chars_normalize),
           ])

.. note::

   Bu fonksiyon ``PreprocessingPipeline`` isimli bir sınıfı kullanmaktadır. Ancak aşağıdaki ``normalizer.py``
   dosyasının bütününde tanımlanan boru hattı sınıfının adı ``NormalizerPipeline``'dır; ``PreprocessingPipeline``
   hiçbir yerde tanımlanmamıştır. Bu, kodu çalıştırmadan salt okuyarak fark edilebilen gerçek bir isim
   tutarsızlığıdır: ``build_turkish_keep_normalizer`` çağrıldığında ``NameError: name 'PreprocessingPipeline'
   is not defined`` hatası alınması beklenir. Kod, orijinal haliyle (bu tutarsızlıkla) korunmuştur.

Aşağıda ``normalizer.py`` dosyasının tüm içeriğini veriyoruz:

.. code-block:: python

   # normalizer.py

   import unicodedata
   import re
   import emoji
   import trinformal
   import csv
   from text2num import text_to_number

   def unicode_normalize(text):
       return unicodedata.normalize('NFKC', text)

   def case_normalize(text):
       return text.replace('İ', 'i').replace('I', 'ı').lower()

   def asciify_turkish_normalize(text):
       return text.translate(str.maketrans('çğıöşüÇĞİÖŞÜ', 'cgiosuCGIOSU'))

   def diacritical_normalize(text):
       chars = []
       for c in text:
           if c in 'çğıöşüÇĞİÖŞÜ':
               chars.append(c)
           else:
               nfd = unicodedata.normalize('NFD', c)
               for d in nfd:
                   if unicodedata.category(d) != 'Mn':
                       chars.append(d)
       return ''.join(chars)

   def whitespace_normalize(text):
       return re.sub(r'\s+|[\u200B\u200C\u200D\u2060\uFEFF]+', ' ', text).strip()

   def punctuation_normalize(text):
       text = re.sub(r'[\u00AD\u2010\u2011\u2012\u2013\u2014\u2015'
                     '\u2017\u2E3A\u2E3B\u2212\uFE58\uFE63\uFF0D]', '-', text)
       text = re.sub(r'\u2026', '...', text)
       text = re.sub(r'[\uFF01]', '!', text)           # fullwidth !
       text = re.sub(r'[\uFF1F]', '?', text)           # fullwidth ?
       text = re.sub(r'[\uFF0E]', '.', text)           # fullwidth .
       text = re.sub(r'[\uFF0C]', ',', text)           # fullwidth ,
       text = re.sub(r'[\uFF1B]', ';', text)           # fullwidth ;
       text = re.sub(r'[\uFF1A]', ':', text)           # fullwidth :
       text = re.sub(r'\.{3,}', '...', text)           # dört ve üzeri noktayı üç noktaya indir
       text = re.sub(r'!{2,}',  '!!',   text)          # tekrarlanan ünlemi iki tane yap
       text = re.sub(r'\?{2,}', '??',   text)          # tekrarlanan soru işaretini iki tane yap
       text = re.sub(r',{2,}',  ',,',   text)          # tekrarlanan virgülü iki tane yap
       text = re.sub(r';{2,}',  ';;',   text)          # tekrarlanan noktalı virgülü iki tane yap
       text = re.sub(r':{2,}',  '::',   text)          # tekrarlanan iki noktayı iki tane yap
       text = re.sub(r'-{2,}',  '--',   text)          # tekrarlanan tireyi iki tane yap
       text = re.sub(r'(\?!)+|(!\?)+',  '!?', text)    # tekrarlanan ?! için !? yerleştir

       # noktalama işaretlerinden önceki boşlukları kaldır
       text = re.sub(r'\s+([.,!?;:\)])', r'\1', text)
       # noktalama işaretlerinden sonra bir boşluk yerleştir
       text = re.sub(r''''([.,!?;:])(?![.,!?;:])(?=\S)''', r'\1 ', text)

       return text

   def apostrophe_normalize(text):
       text = re.sub(r'[\u201C\u201D\u201E\u201F\u2033\u2036]', '"', text)     # çift tırnak varyantları
       text = re.sub(r'[\u2018\u2019\u201A\u201B\u2032\u2035]', "'", text)     # tek tırnak varyantları
       return text

   def url_normalize(text, strategy='token'):
       pattern_url = (r'(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b'
                      r'([-a-zA-Z0-9()@:%_\+.~#?&//=]*)')

       if strategy == 'token':
           normalized_text = re.sub(pattern_url, '<URL>', text)
       elif strategy == 'remove':
           normalized_text = re.sub(pattern_url, '', text)
       elif strategy == 'domain':
           pattern_domain = (r'(?:https?:\/\/)?(?:www\.)?([-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6})\b'
                              r'(?:[-a-zA-Z0-9()@:%_\+.~#?&//=]*)')
           normalized_text = re.sub(pattern_domain, r'\1', text)
       elif strategy == 'keep':
           normalized_text = text
       else:
           raise ValueError('invalid startegy')
       return normalized_text

   def url_normalize_simple(text, strategy='token'):
       pattern_url = r'https?://(www\.)?[^\s]+|www\.[^\s]+'

       if strategy == 'token':
           normalized_text = re.sub(pattern_url, '<URL>', text)
       elif strategy == 'remove':
           normalized_text = re.sub(pattern_url, '', text)
       elif strategy == 'domain':
           pattern_domain1 = r'https?://([^/]+)([^\s]+)'
           pattern_domain2 = 'www\.([^/]+)([^\s]+)'

           normalized_text = re.sub(pattern_domain1, r'\1', text)
           normalized_text = re.sub(pattern_domain2, r'\1', normalized_text)
       elif strategy == 'keep':
           normalized_text = text
       else:
           raise ValueError('invalid startegy')
       return normalized_text

   def email_normalize(text, strategy='token'):
       pattern = r'\b[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'

       if strategy == 'token':
           normalized_text = re.sub(pattern, '<EMAIL>', text)
       elif strategy == 'remove':
           normalized_text = re.sub(pattern, '', text)
       elif strategy == 'keep':
           normalized_text  = text
       else:
           raise ValueError('Invalid strategy')
       return normalized_text

   def turkish_number_normalize(text, strategy='token'):
       text = text_to_number(text)
       text = re.sub(r'(\d+)\.(\d{3})', r'\1\2', text)
       text = re.sub(r'(\d+),(\d+)', r'\1.\2', text)
       text = re.sub(r'%\s*(\d+)', r'%\1', text)

       match strategy:
           case 'token':
               text = re.sub(r'\d+([.,]\d+)*', '<NUM>', text)
           case 'remove':
               text = re.sub(r'\d+([.,]\d+)*', '', text)
           case 'keep':
               pass
           case _:
               raise ValueError('invalid strategy')

       return text

   def emoji_normalize(text, strategy='token'):
       match strategy:
           case 'token':
               text = emoji.replace_emoji(text, '<EMOJI>')
           case 'remove':
               text = emoji.replace_emoji(text, '')
           case 'text':
               text = emoji.demojize(text)
           case 'keep':
               pass
           case _:
               raise ValueError('invalid strategy')

       return text

   def hashtag_mention_normalize(text, strategy='token'):
       match strategy:
           case 'token':
               text = re.sub(r'#\w+', '<HASHTAG>', text)
               text = re.sub(r'@\w+', '<MENTION>', text)
           case 'remove':
               text = re.sub(r'#\w+', '', text)
               text = re.sub(r'@\w+', '', text)
           case 'separate':
               text = re.sub(r'#(\w+)', r'\1', text)
               text = re.sub(r'@(\w+)', '\1', text)
           case 'keep':
               pass
           case _:
               raise ValueError('invalid strategy')

       return text

   def informal_normalize(text):
       def informal_convert(m):

           value = trinformal.TURKISH_INFORMAL.get(m[0])
           return value or m[0]

       return re.sub(r"\b[\w']+\b", informal_convert, text)

   def informal_large_normalize(text):
       with open('../Data/expanded_turkish_misspellings.csv', encoding='utf-8') as f:
           next(f)
           d = {key: value  for key, value in csv.reader(f)}

           def informal_convert(m):
               value = d.get(m[0])
               return value or m[0]

           return re.sub(r"\b[\w']+\b", informal_convert, text)

   def synonym_normalize(text):
       def syn_csv2dict(path):
           with open(path, encoding='utf-8') as f:
               next(f)
               canon_dict = {}
               for canon_word, syn_words in csv.reader(f):
                   for syn_word in syn_words.split('|'):
                       canon_dict[syn_word] = canon_word
           return canon_dict

       canon_dict = syn_csv2dict('../Data/tr-synonyms.csv')

       def replace(match):
           return canon_dict.get(match[0], match[0])

       pipe_pattern = '|'.join(re.escape(s) for s in sorted(canon_dict, key=len, reverse=True))
       pattern = '(?<![a-zA-ZçÇğĞıİöÖşŞüÜ])(' + pipe_pattern + ')(?![a-zA-ZçÇğĞıİöÖşŞüÜ])'
       return re.sub(pattern, replace, text)

   def repeated_chars_normalize(text, max_repeat=2):
       pattern = r'(.)\1{' + str(max_repeat) + ',}'
       replacement = r'\1' * max_repeat

       text = re.sub(pattern, replacement, text)
       return text

   class NormalizerPipeline:
       def __init__(self, steps=None):
           self.steps= steps or []

       def add_step(self, name, f):
           self.steps.append((name, f))

       def pop_step(self):
           return self.steps.pop()

       def insert_step(self, index, name, f):
           self.steps.insert(index, (name, f))

       def remove_step(self, name):
           for i in range(len(self.steps)):
               if self.steps[i][0] == name:
                   return self.steps.pop(i)

       def __call__(self, text):
           for _, f in self.steps:
               text = f(text)
           return text

   def build_turkish_normalizer():
       return NormalizerPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize),
           ('URLN', url_normalize),
           ('EMN', email_normalize),
           ('TNN', turkish_number_normalize),
           ('EMON', emoji_normalize),
           ('HMN', hashtag_mention_normalize),
           ('IFN', informal_normalize),
           ('SYN', synonym_normalize),
           ('RCN', repeated_chars_normalize),
           ])

   def build_turkish_keep_normalizer():
       return PreprocessingPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize),
           ('URLN', lambda text: url_normalize(text, strategy='keep')),
           ('EMN', lambda text: email_normalize(text, strategy='keep')),
           ('TNN', lambda text: turkish_number_normalize(text, strategy='keep')),
           ('EMON', lambda text: emoji_normalize(text, strategy='keep')),
           ('HMN', lambda text: hashtag_mention_normalize(text, strategy='keep')),
           ('IFN', informal_normalize),
           ('SYN', synonym_normalize),
           ('RCN', repeated_chars_normalize),
           ])

   def build_ascii_normalizer():
       return NormalizerPipeline([
           ('UN', unicode_normalize),
           ('CN', case_normalize),
           ('DCN', diacritical_normalize),
           ('WSN', whitespace_normalize),
           ('PN', punctuation_normalize),
           ('AN', apostrophe_normalize),
           ('IFN', informal_normalize),
           ('SYN', synonym_normalize),
           ('ASN', asciify_turkish_normalize),
           ('EMN', email_normalize),
           ('EMON', emoji_normalize),
           ('HMN', hashtag_mention_normalize),
           ('RCN', repeated_chars_normalize),
           ])

.. note::

   ``punctuation_normalize`` içindeki son ``re.sub`` çağrısında kullanılan
   ``r''''([.,!?;:])(?![.,!?;:])(?=\S)''''`` ifadesi, önceki derslerde kullanılan ve çalıştığı doğrulanmış
   olan ``r'([.,!?;:])(?![.,!?;:])(?=\S)'`` kalıbından farklı olarak fazladan tırnak karakterleri
   içermektedir. Bu, muhtemelen bir yazım hatasıdır. Kod çalıştırılarak test edilmemiştir (yalnızca okuma
   yoluyla tespit edilmiştir); kod orijinal haliyle korunmuştur.

Aşağıda ``text2num.py`` dosyasının içeriğini veriyoruz (orijinal ders notunda bu dosyanın başlık yorumunda
``test2num.py`` yazılmıştır; bu, dosyanın yukarıdaki ``import`` ifadesindeki ve önceki derslerdeki gerçek
adıyla -- ``text2num.py`` -- tutarsızdır, bu nedenle yorum satırı düzeltilmiştir):

.. code-block:: python

   # text2num.py

   import re

   ONES =  {
       'sıfır': 0, 'bir': 1, 'iki': 2, 'üç': 3, 'dört': 4,
       'beş': 5, 'altı': 6, 'yedi': 7, 'sekiz': 8, 'dokuz': 9
   }

   TENS = {
       'on': 10, 'yirmi': 20, 'otuz': 30, 'kırk': 40,
       'elli': 50, 'altmış': 60, 'yetmiş': 70, 'seksen': 80, 'doksan': 90
   }

   SCALES = {
       'yüz': 100, 'bin': 1000, 'milyon': 1_000_000, 'milyar': 1_000_000_000
   }

   ALL_NUMBERS = {**ONES, **TENS, **SCALES}

   def turkish_text_to_number(text):

       words = text.split()

       current = 0
       total = 0
       for word in words:
           if word in ONES:
               current += ONES[word]
           elif word in TENS:
               current += TENS[word]
           elif word == 'yüz':
               current = 100 if current == 0 else current * 100
           elif word in SCALES:
               current = SCALES[word] if current == 0 else current * SCALES[word]
               total += current
               current = 0
       total += current

       return total

   def text_to_number(text, threshold=2):
       def convert_match(m):
           return str(turkish_text_to_number(m[0]))

       pattern = r'\b(?:' + '|'.join(ALL_NUMBERS.keys()) + r')(?:\s+(?:' + '|'.join(ALL_NUMBERS.keys()) + r'))' + \
               f'{{{threshold - 1},}}' + r'\b'
       text = re.sub(pattern, convert_match, text, flags=re.IGNORECASE)
       return text

Aşağıda ``trinformal.py`` dosyasının içeriğini veriyoruz (orijinal ders notunda bu dosyanın başlık yorumunda
``tr-informal.py`` yazılmıştır; ancak dosya yukarıdaki ``import trinformal`` ifadesiyle tutarlı olması için
``trinformal.py`` olarak düzeltilmiştir):

.. code-block:: python

   # trinformal.py

   TURKISH_INFORMAL = {
       # Kısaltmalar
       'slm': 'selam',
       'mrb': 'merhaba',
       'mrhb': 'merhaba',
       'naber': 'ne haber',
       'nbr': 'ne haber',
       'tmm': 'tamam',
       'tşk': 'teşekkür',
       'tşkrlr': 'teşekkürler',
       'sağol': 'sağ ol',
       'saol': 'sağ ol',

       # Yaygın yazım hataları
       'bişey': 'bir şey',
       'bişi': 'bir şey',
       'bisey': 'bir şey',
       'birşey': 'bir şey',
       'hiçbirşey': 'hiçbir şey',
       'herşey': 'her şey',
       'deil': 'değil',
       'degil': 'değil',
       'nasi': 'nasıl',
       'nasil': 'nasıl',
       'nası': 'nasıl',

       # Sayılar
       'bi': 'bir',
   }

Aşağıda ``tr-synonyms.csv`` dosyasının içeriğini veriyoruz:

.. code-block:: text

   canonical_form,synonyms
   kelime,sözcük
   yıl,sene
   şehir,kent
   doktor,hekim|tabip
   ama,fakat|lakin|ancak|ne var ki|yalnız
   hemen,derhal|anında|acilen|ivedi
   şimdi,şu an|şu anda|bu an|bu anda
   ev,konut|mesken|hane|yuva|ikametgah
   insan,kişi|şahıs|birey|fert|kimse
   çocuk,evlat|yavru|yavrucak|afacan
   öğrenci,talebe|öğrenici
   öğretmen,hoca|muallim|eğitimci
   ülke,yurt|vatan|memleket
   bilgi,malumat|enformasyon|veri
   toplantı,buluşma|görüşme|müzakere
   yardım,destek|iane|imdat
   karar,hüküm|karar|çözüm
   sorun,problem|mesele|sıkıntı|dert
   cevap,yanıt|karşılık
   soru,sorgu|istifham
   yazı,metin|makale|yazıt
   kitap,eser|yapıt|cilt
   resim,fotoğraf|görsel|figür
   düşünce,fikir|kanı|görüş|mülahaza
   konuşma,söyleşi|sohbet|diyalog|muhabbet
   anlaşma,uzlaşma|mutabakat|ittifak
   çalışma,emek|uğraş|faaliyet|gayret
   bölüm,kısım|parça|kesim|fasıl
   amaç,hedef|gaye|maksat|niyet
   yöntem,yol|usul|metot|prosedür
   kural,yasa|yönetmelik|ilke|prensip
   başarı,zafer|muvaffakiyet|kazanım
   hata,yanlış|kusur|yanılgı|gaf
   önem,değer|ehemmiyet|ağırlık
   fiyat,ücret|bedel|maliyet|değer
   iş,görev|vazife|meslek|uğraş
   zaman,vakit|süre|dönem|an
   yol,güzergah|rota|patika|cadde
   durum,hal|koşul|şart|ortam
   sebep,neden|gerekçe|etken
   sonuç,netice|çıktı|akıbet
   şans,talih|kader|kısmet
   güç,kuvvet|kudret|enerji|iktidar
   alan,saha|bölge|yer|meydan
   ürün,mal|eşya|nesne
   servis,hizmet
   müşteri,alıcı|tüketici|kullanıcı
   program,yazılım|uygulama|sistem
   bilgisayar,makine|sistem
   ağ,network|internet|şebeke
   veri,data|bilgi|enformasyon
   model,sistem|yapı|şablon
   süreç,proses|işlem|prosedür
   test,sınama|deneme|kontrol|yoklama
   analiz,çözümleme|inceleme|tahlil
   rapor,belge|dosya|döküman
   toplum,cemiyet|camia|halk|kamuoyu
   aile,hane|nesil|soy
   arkadaş,dost|ahbap|yoldaş|refakat
   hastalık,rahatsızlık|illet|dert|sayrılık
   ilaç,deva|çare|tedavi
   güzel,hoş|alımlı|yakışıklı|şık
   kötü,berbat|fena|çirkin|rezil
   büyük,iri|kocaman|devasa|muazzam
   küçük,ufak|minik|mini|cüce
   hızlı,çabuk|süratli|ivedi|acele
   yavaş,ağır|sükünetli|usul
   mutlu,sevinçli|memnun|neşeli|şen
   üzgün,kederli|mahzun|mutsuz|hüzünlü
   kızgın,öfkeli|sinirli|hiddetli|gazaplı
   korkmuş,ürkmüş|dehşete düşmüş|paniğe kapılmış
   şaşırmış,hayrete düşmüş|afalllamış|şoke olmuş
   sıkılmış,bezmiş|usanmış|bıkmış
   yorgun,bitkin|tükenmiş|dermansız|halsiz
   mükemmel,harika|olağanüstü|fevkalade|eşsiz|nefis
   korkunç,dehşetengiz|müthiş|berbat|iğrenç
   ilginç,merak uyandırıcı|enteresan|çarpıcı
   sıradan,alelade|olağan|bayağı|basit
   önemli,kritik|hayati|esaslı|belirleyici
   gereksiz,lüzumsuz|fazladan|boş|anlamsız
   doğru,yanlışsız|düzgün|hatasız|isabetli
   yanlış,hatalı|hatalı|bozuk|yanlış
   açık,net|belirgin|aşikar|bariz
   kapalı,muğlak|belirsiz|anlaşılmaz
   yeni,taze|modern|güncel|son
   eski,köhne|antika|geçmiş|tarihi
   gerçek,hakiki|özgün|gerçekten var olan|sahici
   sahte,yapay|taklit|uydurma|asılsız
   hızlıca,çabucak|derhal|ivedilikle|süratle
   nadiren,seyrek|arada sırada|zaman zaman
   sık sık,defalarca|tekrar tekrar|çoğunlukla|sürekli
   söylemek,demek|ifade etmek|belirtmek|dile getirmek
   görmek,gözlemlemek|izlemek|seyretmek
   gitmek,hareket etmek|yola çıkmak|ilerlemek
   gelmek,ulaşmak|varmak|erişmek
   almak,edinmek|elde etmek|temin etmek
   vermek,sunmak|sağlamak|temin etmek
   başlamak,açılmak|harekete geçmek|girişmek
   bitmek,tamamlanmak|sona ermek|kapanmak
   artmak,büyümek|çoğalmak|yükselmek|genişlemek
   azalmak,düşmek|gerilemek|küçülmek
   kullanmak,işletmek|uygulamak|çalıştırmak
   oluşturmak,yaratmak|üretmek|meydana getirmek
   geliştirmek,ilerletmek|güçlendirmek|iyileştirmek
   belirtmek,ifade etmek|aktarmak|açıklamak|izah etmek
   anlamak,kavramak|idrak etmek|algılamak
   bulmak,keşfetmek|tespit etmek|saptamak
   düşünmek,sanmak|zannetmek|tahmin etmek|varsaymak

Son olarak boru hattını test eden ``normalize_test.py`` dosyasının içeriğini veriyoruz:

.. code-block:: python

   text = """
   Benim e-posta adresim aslank@csystem.org. Ama Ali Hocanınki ali@csystem.org.
   Gürbüz Hocanın ise gurbuzaslan@superonline.com. 4 byte ile yazılabilecek en
   büyük işaretsiz tam sayı: Dört milyar iki yüz doksan dört milyon dokuz yüz altmış yedi bin iki yüz doksan beş.
   🌍 Merhaba dünya! 👋 Bugün harika bir gün ☀️ ve ben çok mutluyum 😄. Sabah kahvemi.
   Çibörek lezzetli bir hamurişidir #yemek, @ali.
   Selam nbr? Saol iyilik. Bişi sorcam. Ahmet geliyo mu? Aklına birşey geliyor mu?
   Mrb nasılsın? İvedilikle gelebilir misin? İdrak etmek bu kadar zor mu? Bu kadar çoook mu?
   -----------------------------
   """

   tpl = build_turkish_normalizer()
   normalized_text = tpl(text)
   print(normalized_text)

.. note::

   Yukarıdaki örnek metinde orijinal ders notunda ``"Benim e-posta adresimaslank@csystem.org."`` biçiminde,
   ``adresim`` ile ``aslank@csystem.org`` arasında boşluk unutulmuş bir yazım hatası bulunmaktaydı; burada
   ``adresim aslank@csystem.org`` biçiminde boşluk eklenerek düzeltilmiştir. Bu test betiği, içindeki
   fonksiyonların bağımlı olduğu (``trinformal``, ``../Data/tr-synonyms.csv`` gibi) modül ve dosyalar
   ortamımda hazır olmadığından çalıştırılmamıştır.
