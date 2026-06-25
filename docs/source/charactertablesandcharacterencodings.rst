===========================================
Karakter Tabloları ve Karakter Kodlamaları
===========================================

Şimdi de karakter tabloları ve karakter kodlamaları üzerinde temel bilgiler vereceğiz. Doğal dil işleme etkinlikleri
yazılar üzerinde yapıldığı için bu alanda çalışacak kişilerin karakter tabloları ve karakter kodlamaları konusunda temel
bilgilere sahip olması gerekmektedir.

Temel Kavramlar
================

Yazıda yer kaplayan en küçük birime karakter (character) denilmektedir. Yazılar karakterlerden oluşan bir dizi gibi
düşünülebilir. Karakterler ise sayılarla temsil edilmektedir. Örneğin *ankara* yazısı karakterlerden oluşmaktadır.
Karakterler de sayılarla temsil edildiği için bu yazı aslında sayılardan oluşan bir dizi gibi düşünülebilir. Text
editörler bir dosyayı görüntülerken onun içerisindeki sayılara karşı gelen karakter temsillerini bize göstermektedir.
İşte hangi karakterlerin hangi sayılarla temsil edildiğini belirlemek için çeşitli *karakter tabloları* oluşturulmuştur.

Dünyada bilişim alanında kullanılan ilk karakter tablosu *ASCII (American Standard Code Information Interchange)*
denilen tablodur. Orijinal ASCII tablosu 7 bitlikti, 128 farklı karaktere birer numara karşılık düşürülmüştü. (Örneğin
'a' karakteri tablonun 97'inci karakteridir, 'b' karakteri 98'inci karakteridir.) ASCII tablosunun yanı sıra daha
sonraları *EBCDIC (Extended Binary Coded Decimal Interchange Code)* gibi, *WISCII* gibi başka karakter tabloları da
geliştirilmiştir. IBM sistemleri uzun süre EBCDIC tablosunu kullanmıştır. Bu tablo hala IBM'in bazı sistemlerinde
geçmişe uyumluluğun sağlanması amacıyla kullanılmaktadır.

Bir karakter tablosunda üç önemli kavram vardır:

1) Glyph
2) Kod numarası (Code Point)
3) Karakter Kodlaması (Character Encoding)

Karakter tablosunun desteklediği karakterlerin görsel temsiline *glyph (glif biçiminde okunuyor)* denilmektedir.
Karakter tablosu içerisindeki her glyph'e 0'dan itibaren bir numara karşılık düşürülmüştür. Buna ilgili karakterin *kod
numarası (code point)* denilmektedir. Örneğin ASCII tablosunda 'a' karakterinin kod numarası 97'dir. Bir glyph'e ilişkin
kod numarası doğrudan 2'lik sistemde bir sayı biçiminde kodlanarak dosyalarda saklanabilir. Ancak karakter tabloları kod
numaralarını dosyalarda saklamak için onlar üzerinde bazı dönüşümler de yapabilmektedir. İşte yapılan bu dönüştürmeye
*karakter kodlaması (character encoding)* denilmektedir. Bazı tablolarda kod numaraları birden fazla karakter kodlaması
ile sayısal biçime dönüştürülebilmektedir.

Bir Byte'lık ve Geniş Karakter Tabloları
=========================================

Karakter tabloları kabaca *bir byte'lık karakter tabloları* ve *geniş karakter tabloları* olmak üzere ikiye
ayrılmaktadır. Bir byte'lık karakter tablolarında karakterlere ilişkin kod numaraları bir byte ile ifade
edilebilmektedir. Dolayısıyla bu tablolarda en fazla 256 tane karakter (glyph) tanımlanabilmektedir. Geniş karakter
tablolarında kod numaraları bir byte'tan daha fazla byte ile (örneğin 2 byte ile, 4 byte ile) oluşturulmaktadır. Geniş
karakter tablolarında yazılar bellekte daha fazla yer kaplıyor olsa da bu karakter tablolarında çok fazla sayıda
karakterin temsili yapılabilmektedir. ASCII, EBCDIC genel olarak 1 byte'lık karakter tablolarıdır. Belli bir süredir bir
byte'lık karakter tabloları artık yetersiz kalmıştır.

Bir byte'lık karakter tablolarının yetersiz kalmasından dolayı ismine *Unicode Karakter Tablosu* denilen geniş bir
karakter tablosu zaman içerisinde yaygınlaşmış ve yeni programlama dillerinde neredeyse default karakter tablosu haline
gelmiştir. Unicode karakter tablosu özünde iki byte'lık (16 bitlik) bir karakter tablosudur. Ancak zaman içerisinde
yapılan eklemelerle 21 bitlik bir tablo haline gelmiştir. Bu tablo neredeyse dünyanın bütün dillerindeki karakterler
için, pek çok işaret için glyph bulundurmaktadır. Unicode karakter tablosunun temel kısmına 2^16 = 65536 farklı karakter
tanımlanmıştır. Böylece aynı dosya içerisinde dünyanın bütün dillerine ilişkin yazılar bir arada bulunabilmektedir.
Unicode tablonun sürdürümü *Unicode Consortium* (``www.unicode.org``) tarafından yapılmaktadır. Unicode tablo bazı
farklılıklarla ISO tarafından da *ISO/IEC 10646* koduyla standardize edilmiştir.

Unicode tabloda eskiden (1996'ya kadarki zaman diliminde) her glyph için 16 bitlik bir kod numarası karşılık
getirilmişti. Sonra tabloya yeni glyph'ler eklendi. Bugün her karakterin kod numarası 21 bitle ifade edilmektedir. Yani
yukarıda biz Unicode karakter tablosunun tipik olarak 2 byte'lık (16 bitlik) bir karakter tablosu olduğunu söylemiş
olsak da aslında güncel durumda 21 bitlik bir karakter tablosudur.

Unicode tablonun ilk 128 karakteri standart ASCII tablosu ile aynıdır. Örneğin 'a' karakterinin ASCII tablosundaki kod
numarası da Unicode tablodaki kod numarası da 97'dir. Unicode tablonun [128-255] arasındaki karakterleri ASCII *Latin-1*
kod sayfası ile aynıdır. Türkçe'ye özgü karakterlerin bazılarının (Örneğin 'ş' gibi, 'ğ' gibi) kod numaraları 256'dan
büyüktür. Aşağıda bu karakterlerin Unicode tablodaki kod numaralarını veriyoruz:

.. list-table:: Türkçe Özel Karakterlerin Unicode Code Point'leri (Büyük Harfler)
   :header-rows: 1
   :widths: 25 35

   * - Karakter
     - Unicode Kod Numarası
   * - Ç
     - U+00C7 (ondalık: 199)
   * - Ğ
     - U+011E (ondalık: 286)
   * - İ
     - U+0130 (ondalık: 304)
   * - Ö
     - U+00D6 (ondalık: 214)
   * - Ş
     - U+015E (ondalık: 350)
   * - Ü
     - U+00DC (ondalık: 220)

.. list-table:: Türkçe Özel Karakterlerin Unicode Code Point'leri (Küçük Harfler)
   :header-rows: 1
   :widths: 25 35

   * - Karakter
     - Unicode Kod Numarası
   * - ç
     - U+00E7 (ondalık: 231)
   * - ğ
     - U+011F (ondalık: 287)
   * - ı
     - U+0131 (ondalık: 305)
   * - i
     - U+0069 (ondalık: 105)
   * - ö
     - U+00F6 (ondalık: 246)
   * - ş
     - U+015F (ondalık: 351)
   * - ü
     - U+00FC (ondalık: 252)

ASCII Kod Sayfaları ve Tarihsel Gelişimi
=========================================

Peki Unicode öncesi devirlerde Türkçe gibi dillerin karakterleri nasıl ifade ediliyordu? İşte zaman içerisinde standart
ASCII tablosu diğer dillerin karakterlerini (glyph'lerini) de içerecek biçimde genişletildi. Bu genişletmede ASCII
tablosunun [0-127] arası karakterlerine ilişkin kod numaraları sabit bırakıldı. [128-255] arasındaki kod numaralarına
latin dillerinin çeşitli karakterleri yerleştirildi. Ancak bu konuda bir standardizasyon yapılmamıştı. ASCII tablosunun
genişletilmiş bu halleri için sıklıkla *kod sayfası (code page)* terimi kullanılıyordu. Türkçe karakterler için zaman
içerisinde değişik kod sayfaları (code pages) kullanılmıştır. DOS zamanlarında IBM'in 754 kod sayfası kullanılıyordu.
Sonra Microsoft Türkçe karakterler için 1254 biçiminde isimlendirdiği kod sayfasını tasarladı. Nihayet 1999 yılına
gelindiğinde ISO durumdan vazife çıkardı ve kod sayfalarını ISO 8859-X kod numarasıyla standardize etti. Örneğin 8859-1
kod sayfasına *Latin-1* kod sayfası denilmektedir. ISO Türkçe karakterler için ISO 8859-9 kod sayfasını oluşturmuştur.
Bu kod sayfası bugün Türkçe karakterler için en yaygın kullanılan bir byte'lık karakter tablosu durumundadır. ISO 8859-9
ile Microsoft'un 1254 kod sayfaları büyük ölçüde örtüşmektedir.

Bugün artık bir byte'lık karakter tablolarının kullanımı iyice azalmış, Unicode tablonun kullanımı ise oldukça
yaygınlaşmıştır. Dolayısıyla ASCII kod sayfalarının karmaşıklığı Unicode tablo sayesinde bertaraf edilmiş durumdadır.

UTF-8, UTF-16 ve UTF-32 Kodlamaları
====================================

Bugün Unicode tablo için UTF-32, UTF-16 ve UTF-8 denilen üç farklı karakter kodlaması (character encoding)
kullanılmaktadır. UTF-32'de Unicode karakterlerin kod numaraları 32 bitlik bir sayı biçiminde, UTF-16'da ise 16 bitlik
bir sayı biçiminde kodlanmaktadır. UTF-16 kodlamasında Unicode tablodaki ilk 65536 karakterin dışındaki karakterler ya
hiç kodlanmamaktadır ya da iki ayrı 16 bit ile kodlanmaktadır. Bu kodlamaya *surrogate çifti* denilmektedir. Ancak bugün
en yaygın kullanılan Unicode kodlaması UTF-8'dir. UTF-8 multibyte bir kodlamadır. Bu kodlamada bazı karakterler 1 byte
ile, bazıları 2 byte ile, bazıları 3 byte ile ve bazıları da 4 byte ile kodlanmaktadır. Bugün kullandığımız text
editörlerin hemen hepsi default durumda metin dosyalarını Unicode UTF-8 olarak açıp save etmektedir. Unicode UTF-8
kodlamasında ASCII tablosunun ilk 128 karakteri 1 byte'la kodlanmaktadır. Türkçe karakterler ise 2 byte ile
kodlanmaktadır, Japonca ve Çince Kanjiler 3 byte ile kodlanmaktadır. Tamamen İngilizce karakterle yazılmış olan
metinlerin ASCII temsili ile Unicode UTF-8 temsilinin aynı olduğuna dikkat ediniz.

UTF-16 ve UTF-32 kodlamaları hem *Little Endian* hem de *Big Endian* biçiminde yazılabilmektedir. Tabii UTF-8 için böyle
bir Endian'lık durumu yoktur.

Artık pek çok programlama dillerinin derleyicileri ya da yorumlayıcıları kaynak kodları Unicode UTF-8 formatında kabul
etmektedir. Tabii bu durum o dillerin resmi dokümanlarında belirtilmiştir.

Karakter Kodlamasının Tespiti ve BOM Belirteçleri
==================================================

Yazılarla ilgili işlem yapan sistemlerde yazıyı oluşturan tarafla yazıyı yorumlayan tarafın aynı karakter tablosu ve
karakter kodlaması üzerinde anlaşmış olmaları gerekir. Örneğin bir metin Unicode UTF-8 ile oluşturulmuşsa bu metni diğer
taraf ASCII 8859-9 olarak yorumlarsa karakterler anlamsız biçimde görüntülenecektir. Peki bizim elimizde bir metin
dosyası varsa biz onda kullanılan karakter tablosunu ve karakter kodlamasını nasıl tespit edebiliriz? Maalesef bu
tespitin yapılabilmesinin güvenilir ve sağlam bir yolu yoktur. Bazı editörler *sezgisel yöntemlerle (heuristics)* bunu
tespit etmeye çalışmaktadır. Ancak yukarıda da belirttiğimiz gibi bunun sağlam bir yolu yoktur. Unicode metin dosyaları
için *isteğe bağlı olarak (optional)* dosyanın başında *BOM (Byte of Order) marker* denilen bir belirteci
bulundurulabilmektedir. Eğer dosyada bu BOM belirteci varsa dosyanın Unicode karakter tablosuyla ve karakter
kodlamasıyla oluşturulduğu tespit edilebilmektedir. Ancak yukarıda da belirttiğimiz gibi BOM belirteci bir dosyanın
başında bulunmak zorunda değildir. BOM belirteçleri şunlardır:

.. list-table:: Unicode BOM Belirteçleri Tablosu
   :header-rows: 1
   :widths: 25 25

   * - Kodlama Türü
     - BOM (Hex)
   * - UTF-8
     - EF BB BF
   * - UTF-16 BE
     - FE FF
   * - UTF-16 LE
     - FF FE
   * - UTF-32 BE
     - 00 00 FE FF
   * - UTF-32 LE
     - FF FE 00 00

İşte text editörler genellikle önce BOM belirteçlerine bakmakta, eğer dosyada BOM belirteci yoksa default olarak
belirledikleri bir karakter tablosu ve kodlamasıyla dosyayı açmaktadırlar.

Programlama Dillerinde Karakter Kodlaması
==========================================

Şimdi de karakter tablolarının programlama dillerini ilgilendiren tarafları üzerinde duralım. Yukarıda da belirttiğimiz
gibi artık programlama dillerinin önemli bir bölümüne ilişkin derleyiciler ve yorumlayıcılar kaynak dosyaların Unicode
UTF-8 olmasını beklemektedir. Örneğin C#, Java ve Python dillerinde kaynak dosyalar Unicode UTF-8 biçiminde olmalıdır. C
Programlama Dilinde kaynak dosyalar için belli bir karakter tablosu ya da karakter kodlaması belirlenmemiştir. Ancak bu
dillerin standartları alfabetik ve nümerik karakterlerin kaynak kodda 1 byte yer kaplaması gerektiğini belirtmektedir.
Dolayısıyla C ve C++ derleyicileri ASCII kod sayfalarına ilişkin dosyaları ve Unicode UTF-8 dosyalarını kabul
edebilmektedir. Tabii programcının derleyiciye komut satırı argümanlarıyla bu kodlama bilgisini vermesi gerekir. Aksi
takdirde derleyici kendisinin belirlediği default bir kodlamayı kullanmaktadır. Java ve C# dillerinde char türü zaten 2
byte uzunluktadır ve string'ler bunların derleyicileri tarafından Unicode UTF-16 ile tutulmaktadır.

Python'da char biçiminde bir tür yoktur. Python'un str türü 3'lü versiyonlarla birlikte Unicode karakterleri tutma
yeteneği kazanmıştır. Yani Python'da programcı artık tüm string'lerin karakterlerinin Unicode karakterler olduğunu
varsaymalıdır. Ancak CPython gerçekleştirimi aslında içsel olarak kendisi string'leri mümkün olduğunca daha az yer
kaplayacak biçimde tutar. Tabii programcının bu içsel ayrıntıları bilmesine gerek yoktur.

Artık ana konularımıza başlamak için bazı ön bilgileri edinmiş durumdayız. Şimdi yavaş yavaş klasik doğal dil işlemenin
temel konularına giriş yapacağız.

