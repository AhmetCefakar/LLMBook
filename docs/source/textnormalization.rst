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
===================================

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
--------------------------------------------------------

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
----------------------------

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
------------------------------

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
--------------------------------------------------

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
============================================================

ASCII Dönüştürmesi (Transliteration)
----------------------------------------

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
----------------------------------------------------------------

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
========================

Biz yukarıda karakter temelinde normalizasyon işlemlerini ele aldık. Şimdi yapısal normalizasyonlar üzerinde
duralım. Yapısal normalizasyonlar olarak grupladığımız normalizasyonları anımsatmak istiyoruz:

- Yapısal Normalizasyon

  - Boşluk (Whitespace) Normalizasyonu
  - Noktalama Normalizasyonu
  - Tırnak (Apostrophe) Normalizasyonu
  - Satır Sonu ve Kontrol Karakterlerinin Normalizasyonu

Boşluk (Whitespace) Normalizasyonu
-------------------------------------

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
---------------------------

Noktalama normalizasyonu peşi sıra gelen aynı noktalama işaretlerinden yalnızca bir tanesini ya da n tanesini
alıp diğerlerini atarak gerçekleştirilmektedir. Tabii daha önceden de belirttiğimiz gibi bu tür
normalizasyonlar duygu kaybına yol açabilmektedir. Örneğin *dikkat!* ile *dikkat!!!* arasında bir vurgu farkı
olabilir. Bazı alanlarda ise birden fazla noktalama karakterinin peşi sıra gelmesi özel bir anlam ifade
edebilmektedir. Örneğin satrançta ``!!`` çok iyi hamle için ``??`` çok kötü hamle için kullanılan bir
gösterimdir. Tabii daha önceden de belirttiğimiz gibi normalizasyon hedefe göre gerçekleştirilen bir süreçtir.
Burada ele aldığımız tüm normalizasyonların buradaki gibi uygulanması zorunlu değildir.

Tireleme (Dash) Normalizasyonu
+++++++++++++++++++++++++++++++++

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
+++++++++++++++++++++++++++++++++

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
+++++++++++++++++++++++++++++++++++++++++++++++++++++

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