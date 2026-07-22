===================
Atom Normalizasyonu
===================

Atomlarına ayırma konusunu da bitirdik. Doğal dil işlemedeki ön işlem adımlarını yeniden anımsatmak istiyoruz:

.. code-block:: text

    ┌────────────────────────────┐
    │  Verilerin Elde Edilmesi   │
    └────────────────────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │    Verilerin Temizlenmesi  │
    └────────────────────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │    Metin Normalizasyonu    │
    └────────────────────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │     Atomlarına Ayırma      │
    │      (Tokenization)        │
    └────────────────────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │ Atomlarına Ayırma Sonrası  │
    │       Normalizasyonu       │
    │    (Atom Normalizasyonu)   │
    └────────────────────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │     Sözcük Hazinesinin     │
    │        Oluşturulması       │
    └────────────────────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │         Atomların          │
    │    Sayısallaştırılması     │
    │        (Encoding)          │
    └────────────────────────────┘

Şimdi sıra *atom normalizasyonu (token normalization)* aşamasına gelmiştir. Atom normalizasyonu aslında kursumuzun ana
hedefi olan transformer tabanlı modellerde uygulanmamaktadır. Atom normalizasyonu daha çok istatistiksel ve
olasılıksal tabanlı klasik doğal dil işleme uygulamalarında kullanılmaktadır.


Gövdeleme ve Sözlüksel Biçime Dönüştürme
========================================

Gövdeleme ve Sözlüksel Biçime Dönüştürmeye Giriş
------------------------------------------------

Atom normalizasyonunda temelde iki işlem yapılmaktadır:

1) Durak sözcüklerinin (stop words) atılması
2) Gövdeleme (stemming)
3) Sözlüksel biçime dönüştürme (lemmatization)

Yazıdaki tek başına önemli bir anlama sahip olmayan sık geçen sözcüklere "**durak sözcükleri (stop words)**" 
denilmektedir. Klasik doğal dil işleme uygulamalarında bu tür sözcüklerin atılması fayda sağlayabilmektedir. Mesela 
İngilizce`deki *the*, *a*, *an*, *is*, *in*, *of*, *and*, *to*, Türkçe'deki *ve*, *ile*, *ama*, *için*, *gibi*, *bu*, 
*şu*, *da*, *de* tipik durak sözcükleridir. Şüphesiz durak sözcüklerinin de aslında metin içerisinde işlevleri vardır. 
Ancak bunların tasşıdığı bilgi bazı uygulamalarda ihmal edilebilecek düzeydedir. Sözcük hazinesini düşürmek için bunların
atılması uygun olabilmektedir. 

İngilizce *stem* sözcüğü *kökün toprak dışında kalan sap ya da gövde kısmı* anlamına gelmektedir. Biz İngilizce
*stemming* yerine Türkçe *gövdeleme* sözcüğünü kullanacağız. *Stemming* terimi bazı Türkçe kaynaklarda *köklerine
ayırma*, *kök bulma* gibi sözcüklerle de ifade edilmektedir. *Lemma* sözcüğü Latinceden gelme bir sözcüktür. *Lemma*,
bir sözcüğün tüm çekimli biçimlerini temsil eden sözlük biçimidir. Biz bu *lemmatization* sözcüğünü Türkçe *sözlüksel
biçime dönüştürme* biçiminde ifade edeceğiz.

Gövdeleme ve *sözlüksel biçime dönüştürme* aynı birlikte kullanılan yöntemler değildir. Yani bunlardan yalnızca biri
kullanılmaktadır. Genellikle *sözlüksel biçime dönüştürme* tercih edilmektedir. Gövdeleme çok seyrek kullanılmaktadır.
Atomsal normalizasyon sözcük temelinde yapılmaktadır. Dolayısıyla *alt sözcük atomlarına ayırma (subword tokenization)*
işleminden sonra atom normalizasyonu uygulanamaz. Biz transformer tabanlı modern yöntemlerin atom normalizasyonu
kullanmadığını belirtmiştik. Transformer tabanlı yöntemler zaten hemen her zaman alt sözcük atomlarına ayırma
yöntemlerini kullanmaktadır.

Gövdeleme (Stemming)
--------------------

Gövdeleme bir sözcüğü temsil eden daha yalın bir sözcüğün kullanılması anlamına gelmektedir. Bu sayede aslında aynı
gövdeye sahip olan sözcükler farklı atomlarla değil aynı atomla temsil edilmiş olur. Gövdelemede gövdesine ayrılmış
sözcüğün anlamlı bir sözcük olması (yani sözlükte bir karşılığının olması) gerekmez, yalnızca aynı gövdeye sahip olan
sözcüklerin indirgendiği ortak bir harf diziliminin olması yeterlidir.

Metinden durak sözcüklerini atmak için onların listesini oluşturmamız gerekir. Pek çok dil için durak sözcükleri
zaten oluşturulmuş durumdadır. Yaygın kullanılan kütüphaneler bunların listesini bize verebilmektedir. NLTK 
kütüphanesinden durak sözcüklerini şöyle elde edebiliriz:

.. code-block:: python

    import nltk

    nltk.download('stopwords')


    from nltk.corpus import stopwords

    english_stops = stopwords.words('english')
    turkish_stops = stopwords.words('turkish')

NLTK'deki Türkçe durak sözcükleri şunlardır:

.. code-block:: python

    ['acaba', 'ama', 'aslında', 'az', 'bazı', 'belki', 'biri', 'birkaç', 'birşey', 'biz', 'bu', 'çok', 'çünkü', 'da', 
    'daha', 'de', 'defa', 'diye', 'eğer', 'en', 'gibi', 'hem', 'hep', 'hepsi', 'her', 'hiç', 'için', 'ile', 'ise', 
    'kez', 'ki', 'kim', 'mı', 'mu', 'mü', 'nasıl', 'ne', 'neden', 'nerde', 'nerede', 'nereye', 'niçin', 'niye', 'o', 
    'sanki', 'şey', 'siz', 'şu', 'tüm', 've', 'veya', 'ya', 'yani']

spaCy kütüphanesinde de İnglizce ve Türkçe durak sözcükleri bulunmaktadır. Ancak bu kütüphanedeki Türkçe durak
sözcükleri 500'ün yukarısındadır. Bunları aşağıdak gibi kullanabilirsiniz:

.. code-block:: python

    from spacy.lang.en.stop_words import STOP_WORDS as english_stops
    from spacy.lang.tr.stop_words import STOP_WORDS as turkish_stops 

Peki atom listesindeki durak sözcüklerinden nasıl kurtulabiliriz. Bunu yapmanın en kolay yolu atom listesini *küme*
*(set)* haline geitirip ``difference`` metodunu ya da çıkartma operatörünü kullanmaktadır. ``difference`` metodunun 
parametresi dolaşılabilir herhangi bir nesne olabilir. Ancak çıkartma operatörünün parametresi ``set`` ya da 
``frozsenset`` türünden olmak zorundadır. Mesela:

.. code-block:: python

    from spacy.lang.tr.stop_words import STOP_WORDS

    tokens = ['bugün', 'hava', 'güzel', 'mi', 'ali', 've', 'veli', 'ile', 'çok', 'dolaştık']
    normalized_tokens = set(tokens) - STOP_WORDS
    print(normalized_tokens)

Durak sözcükleri atıldıktan sonra şu atomlar kalmıştır:

.. code-block:: python

    {'bugün', 'ali', 'güzel', 'dolaştık', 'veli', 'hava'}

Gövdeleme genel olarak *kural tabanlı (rule based)* yöntemlerle uygulanmaktadır. Gövdeleme için çeşitli algoritmalar
önerilmiştir. Ancak bu algoritmaların çoğu İngilizce temel alınarak oluşturulmuştur dolayısıyla Türkçeye uygun
değildir.

*Porter Stemmer* algoritması en yaygın kullanılan gövdeleme algoritmasıdır. 1980 yılında Martin Porter tarafından
geliştirilmiştir. Algoritma İngilizce için oluşturulmuştur. Aşağıdaki kurallar sırasıyla uygulanmaktadır:

**1a) Çoğul ekleri kaldırılır.** Örneğin:

.. code-block:: text

    caresses → caress
    ponies → poni
    cats → cat

**1b) -ed, -ing ekleri kaldırılır:**

.. code-block:: text

    agreed → agre → agree (düzeltme)
    plastered → plaster
    motoring → motor

**1c) Sondaki 'y' harfi 'i' ye dönüştürülür (kökte sesli varsa).** Örneğin:

.. code-block:: text

    happy → happi

**2) Türetme ekleri sadeleştirilir.** Örneğin:

.. code-block:: text

    ATIONAL → ATE      relational'   → relate
    IZATION → IZE      organization  → organize
    FULNESS → FUL      hopefulness   → hopeful
    BILITI  → BLE      sensibiliti   → sensible

**3) Kalan türetme ekleri kaldırılır.** Örneğin:

.. code-block:: text

    ICATE → IC     triplicate   → triplic
    FUL   →        hopeful      → hope
    NESS  →        goodness     → good
    ATIVE →        formative    → form

**4) Son ekler tamamen atılır.** Örneğin:

.. code-block:: text

    ANCE  →     allowance   → allow
    ER    →     airliner    → airlin
    MENT  →     adjustment  → adjust
    ION   →     adoption    → adopt   (kök S veya T ile bitiyorsa)

**5a) Sondaki 'e' harfi atılır.** Örneğin:

.. code-block:: text

    probate → probat
    cease → ceas

**5b) Çift 'l' teke indirilir.** Örneğin:

.. code-block:: text

    controll → control

Yukarıda da belirttiğimiz gibi gövdeleme sonucunda elde edilmiş olan atomlar anlamlı birer sözcük olmak zorunda
değildir.

NLTK kütüphanesinde Porter Stemming işlemini yapan nltk.stem modülü içerisinde PorterStemmer isimli bir sınıf
bulunmaktadır. Sınıfın kullanımı oldukça basittir. PorterStemmer sınıfı türünden bir nesne yaratılır, sonra sınıfın
stem metodu çağrılır. stem metoduna argüman olarak sözcük verilir metot da onun gövdesini verir. Örneğin:

.. code-block:: python

    from nltk.stem import PorterStemmer

    stemmer = PorterStemmer()

    words = [
        'running', 'runs', 'ran', 'runner',
        'studies', 'studying', 'studied',
        'happiness', 'happy', 'happier',
        'organization', 'organize', 'organizer'
    ]

    print('Porter Stemmer Sonuçları:')
    print()
    for word in words:
        stem = stemmer.stem(word)
        print(f'  {word:<20} → {stem}')

Buradan elde edilen çıktı şöyledir:

.. code-block:: text

    Porter Stemmer Sonuçları:

    running              → run
    runs                 → run
    ran                  → ran         ← düzensiz fiil, yanlış!
    runner               → runner
    studies              → studi
    studying             → studi
    studied              → studi
    happiness            → happi
    happy                → happi
    happier              → happier
    organization         → organ
    organize             → organ
    organizer            → organ

Bu sınıfı Türkçe için kullanmaya çalışmayınız.

SnowballStemmer (NLTK)
----------------------

Diğer bir gövdeleme algoritması da *snowball stemmer* denilen algoritmadır. Bu algoritma da kural tabanlıdır. Ancak
İngilizce'nin dışındaki dillere de uygulanabilmektedir. Biz burada algoritmanın işleyişini açıklamayacağız. NLTK
içerisinde nltk.stem modülünde SnowballStemmer isimli bir sınıf bulunmaktadır. Sınıfın desteklediği dilleri şöyle
öğrenebiliriz:

.. code-block:: python

    from nltk.stem import SnowballStemmer

    print('Desteklenen diller:')
    print(SnowballStemmer.languages)

Buradan şöyle bir çıktı elde edilmiştir:

.. code-block:: text

    ('arabic', 'danish', 'dutch', 'english', 'finnish', 'french', 'german', 'hungarian', 'italian', 'norwegian',
    'porter', 'portuguese', 'romanian', 'russian', 'spanish', 'swedish')

Listede Türkçe yoktur. Sınıfın kullanımı şöyledir:

.. code-block:: python

    from nltk.stem import SnowballStemmer

    # Desteklenen dilleri listele
    print('Desteklenen diller:')
    print(SnowballStemmer.languages)

    # İngilizce
    english_stemmer = SnowballStemmer('english')

    # Almanca örneği — çok dilli destek
    german_stemmer = SnowballStemmer('german')

    words_en = ['generously', 'generation', 'generous', 'generosity']
    words_de = ['freundlich', 'Freundlichkeit', 'Freund', 'freunden']

    print('\nSnowball (İngilizce):')
    for word in words_en:
        print(f'  {word:<20} → {english_stemmer.stem(word)}')

    print('\nSnowball (Almanca):')
    for word in words_de:
        print(f'  {word:<20} → {german_stemmer.stem(word)}')

NLTK içerisinde ayrıca LancasterStemmer isimli diğer bir gövdeleme sınıfı da bulunmaktadır.

Türkçe İçin Naif Bir Gövdeleme Fonksiyonu
-----------------------------------------

Türkçe için çok basit bir gövdeleme fonksiyonu yazmak isteyelim. En basit yaklaşım sözcüğün sonundaki temel Türkçe
ekleri atmaktır. Aşağıda böyle naif bir örnek verilmiştir. Örnekte sözcüklerin sonundaki sonekler bir listede
toplanmış sonra sözcüğün sonu bu soneklerin bir tanesiyle bitiyor mu diye bakılmıştır. Eğer sözcüğün sonu bu
soneklerden biriyle bitiyorsa ilgili sonek atılmıştır.

.. code-block:: python

    def naive_turkish_stem(word):
        # Basit Türkçe çoğul ekleri
        suffixes = ['ların', 'lerin', 'ların', 'lerin',
                    'lar', 'ler', 'da', 'de', 'ta', 'te',
                    'dan', 'den', 'tan', 'ten',
                    'ın', 'in', 'un', 'ün',
                    'a', 'e', 'ı', 'i', 'u', 'ü']

        for suffix in suffixes:
            if word.endswith(suffix) and len(word) - len(suffix) >= 2:
                return word[:-len(suffix)]
        return word

    test_words = ['kitaplarda', 'evlerden', 'arabaya', 'çocukların', 'evden']

    for word in test_words:
        stem = naive_turkish_stem(word)
        print(f'{word:<15} → {stem}')

snowballstemmer Kütüphanesi
---------------------------

Türkçe gövdeleme işlemleri için Türkçeye özel gövdeleme sınıfları ve fonksiyonları da oluşturulmuştur. Bunlardan biri
orijinal snowball kütüphanesine *Evren Kapusuz Çilden* tarafından 2007 civarında yapılan Türkçe eklemedir.
snowballstemmer kütüphanesi NLTK'dekinden farklı bir kütüphanedir. Bu kütüphaneyi şöyle kurabilirsiniz:

.. code-block:: console

    pip install snowballstemmer

Snowballstemmer kütüphanesi Martin Porter tarafından geliştirilmiştir. Aslında Porter bunun için snowball isminde
küçük bir domain specific dil de geliştirmiştir. Daha sonra kütüphanenin Python port'u da yapılmıştır. Snowball ismi
SNOBOL denilen eski bir dilden esinlenerek uydurulmuştur. Orijinal kütüphane aşağıdaki dilleri desteklemektedir:

+--------------+--------------------+------------------+
| Dil          | Türkçe Karşılığı   | Algoritma Adı    |
+==============+====================+==================+
| Arabic       | Arapça             | ``'arabic'``     |
+--------------+--------------------+------------------+
| Armenian     | Ermenice           | ``'armenian'``   |
+--------------+--------------------+------------------+
| Basque       | Baskça             | ``'basque'``     |
+--------------+--------------------+------------------+
| Catalan      | Katalanca          | ``'catalan'``    |
+--------------+--------------------+------------------+
| Danish       | Danca              | ``'danish'``     |
+--------------+--------------------+------------------+
| Dutch        | Felemenkçe         | ``'dutch'``      |
+--------------+--------------------+------------------+
| English      | İngilizce          | ``'english'``    |
+--------------+--------------------+------------------+
| Esperanto    | Esperanto          | ``'esperanto'``  |
+--------------+--------------------+------------------+
| Estonian     | Estonca            | ``'estonian'``   |
+--------------+--------------------+------------------+
| Finnish      | Fince              | ``'finnish'``    |
+--------------+--------------------+------------------+
| French       | Fransızca          | ``'french'``     |
+--------------+--------------------+------------------+
| German       | Almanca            | ``'german'``     |
+--------------+--------------------+------------------+
| Greek        | Yunanca            | ``'greek'``      |
+--------------+--------------------+------------------+
| Hindi        | Hintçe             | ``'hindi'``      |
+--------------+--------------------+------------------+
| Hungarian    | Macarca            | ``'hungarian'``  |
+--------------+--------------------+------------------+
| Indonesian   | Endonezce          | ``'indonesian'`` |
+--------------+--------------------+------------------+
| Irish        | İrlandaca          | ``'irish'``      |
+--------------+--------------------+------------------+
| Italian      | İtalyanca          | ``'italian'``    |
+--------------+--------------------+------------------+
| Lithuanian   | Litvanca           | ``'lithuanian'`` |
+--------------+--------------------+------------------+
| Nepali       | Nepalce            | ``'nepali'``     |
+--------------+--------------------+------------------+
| Norwegian    | Norveççe           | ``'norwegian'``  |
+--------------+--------------------+------------------+
| Portuguese   | Portekizce         | ``'portuguese'`` |
+--------------+--------------------+------------------+
| Romanian     | Romence            | ``'romanian'``   |
+--------------+--------------------+------------------+
| Russian      | Rusça              | ``'russian'``    |
+--------------+--------------------+------------------+
| Serbian      | Sırpça             | ``'serbian'``    |
+--------------+--------------------+------------------+
| Spanish      | İspanyolca         | ``'spanish'``    |
+--------------+--------------------+------------------+
| Swedish      | İsveççe            | ``'swedish'``    |
+--------------+--------------------+------------------+
| Tamil        | Tamilce            | ``'tamil'``      |
+--------------+--------------------+------------------+
| Turkish      | Türkçe             | ``'turkish'``    |
+--------------+--------------------+------------------+
| Yiddish      | Yidiş              | ``'yiddish'``    |
+--------------+--------------------+------------------+

Kütüphanenin kullanımı oldukça kolaydır. Önce stemmer isimli fonksiyon çağrılarak bir gövdeleme nesnesi elde edilir.
Örneğin:

.. code-block:: python

    import snowballstemmer

    words = [
        'bugün', 'sinemaya', 'gittim', 'ama', 'film', 'çok', 'kötüydü',
        'kitaplardaki', 'şekiller', 'çok', 'detaylı', 'çizilmiş'
    ]
    stemmer = snowballstemmer.stemmer('turkish')
    stems = stemmer.stemWords(words)

    for word, stem in zip(words, stems):
        print(f'{word:<15} → {stem}')

Buradan aşağıdaki gövdeler elde edilmiştir:

.. code-block:: text

    bugün           → bugu
    sinemaya        → sinema
    gittim          → git
    ama             → am
    film            → film
    çok             → çok
    kötüydü         → köt
    kitaplardaki    → kitap
    şekiller        → şekil
    çok             → çok
    detaylı         → detaylı
    çizilmiş        → çizil

turkishstemmer Kütüphanesi
--------------------------

Türkçe gövdeleme için diğer bir alternatif de turkishstemmer kütüphanesidir. Kütüphaneyi şöyle yükleyebilirsiniz:

.. code-block:: console

    pip install turkishstemmer

Kütüphanenin kullanımı oldukça kolaydır. TurkishStemmer sınıfı türünden bir nesne yaratılır. Sonra sınıfın stem metodu
çağrılır:

.. code-block:: python

    from TurkishStemmer import TurkishStemmer

    stemmer = TurkishStemmer()

    words = [
        'bugün', 'sinemaya', 'gittim', 'ama', 'film', 'çok', 'kötüydü',
        'kitaplardaki', 'şekiller', 'çok', 'detaylı', 'çizilmiş'
    ]

    for word in words:
        stem = stemmer.stem(word)
        print(f'{word} -> {stem}')

Buradan şöyle bir çıktı elde edilmiştir:

.. code-block:: text

    bugün -> bugü
    sinemaya -> sinema
    gittim -> git
    ama -> am
    film -> film
    çok -> çok
    kötüydü -> kötü
    kitaplardaki -> kitap
    şekiller -> şekil
    çok -> çok
    detaylı -> detay
    çizilmiş -> çizil

HuggingFace ve Gövdeleme
------------------------

HuggingFace içerisinde gövdeleme işlemini yapan bir sınıf ya da fonksiyon bulunmamaktadır. Çünkü HuggingFace genel
olarak modern transformer tabanlı modeller için oluşturulmuştur. Stanza, spaCy ve Zeyrek gibi kütüphaneler morfolojik
analiz yapmaktadır. Yani bu kütüphaneler *sözlüksel biçime dönüştürme (lemmatization)* yöntemlerini uygulamaktadır.

Gövdelemenin Kullanım Alanları
------------------------------

Gövdeleme sırasında sözcüğün sonekleri ortadan kaldırıldığı için sözcükler ortak ve yalın bir biçime
dönüştürülmektedir. Dolayısıyla da metinde anlamsal kayıplar oluşması kaçınılmazdır. Peki bu kayıplar sonrasında elde
edilen sözcük hazinesi hangi uygulamalarda kullanılabilir? İşte gövdeleme sonucunda elde edilen sözcük hazinesi genel
olarak temel ve kaba birtakım süreçlerde kullanılabilmektedir.

+----------------------------+--------------------------------------------+------------------------------------+
| Uygulama Alanı             | Neden Uygun Kalır                          | Örnek Görevler                     |
+============================+============================================+====================================+
| Bilgi Getirimi / Arama     | Sorgu ve belge aynı köke indirgenir; kökün | İndeksleme, sorgu genişletme,      |
|                            | gerçek sözcük olması gerekmez              | belge eşleştirme                   |
+----------------------------+--------------------------------------------+------------------------------------+
| Metin Sınıflandırma        | Çekim varyantları tek öznitelikte          | Duygu analizi, spam filtreleme,    |
|                            | birleşir, öznitelik seyrekliği azalır      | konu sınıflandırma                 |
+----------------------------+--------------------------------------------+------------------------------------+
| Kümeleme / Konu Modelleme  | Sözcükler okunabilir çıktı değil,          | LDA, k-means, konu keşfi           |
|                            | dağılımsal örüntü taşıyan sayaçlardır      |                                    |
+----------------------------+--------------------------------------------+------------------------------------+
| Anahtar Sözcük Çıkarımı    | Aynı kavramın çekimleri tek terime iner,   | Anahtar öbek çıkarımı, terim       |
|                            | sayım doğruluğu artar                      | eşleştirme                         |
+----------------------------+--------------------------------------------+------------------------------------+
| Tekilleştirme              | Farklı biçimler tek köke eşlenerek         | Yinelenen içerik tespiti, kayıt    |
| (Deduplication)            | karşılaştırma tutarlı hale gelir           | eşleştirme                         |
+----------------------------+--------------------------------------------+------------------------------------+

Sözlüksel Biçime Dönüştürme (Lemmatization)
===========================================

Sözlüksel biçime dönüştürme (lemmatization) yönteminde yine bir grup sözcük onları temsil eden tek bir sözcüğe
indirgenir. Ancak indirgeme sonucunda oluşturulan atom her zaman anlamlı bir sözcüktür. (Yani sözlükte yeri olan bir
sözcüktür.) Gövdeleme işleminde elde edilen atomun anlamlı bir sözcük olmayabileceğini anımsayınız. Sözlüksel biçime
dönüştürme ile gövdeleme arasındaki temel farklılıklar şunlardır:

- Sözlüksel biçime dönüştürmede sözlük bilgisi kullanır.
- Sözlüksel biçime dönüştürmede dilbilgisel bağlam (POS) dikkate alınabilir. Yani bunun için
  morfolojik analiz gerekebilmektedir.
- Ürün olarak geçerli (yani sözlükte olan) sözcükler elde edilmektedir.

Sözlüksel biçime dönüştürme sırasında eşanlamlı sözcükler de ortak bir sözcüğe dönüştürülebilir. Örneğin *ahçı* ile
*aşçı* aynı anlama gelmektedir. Dolayısıyla bu iki sözcük de ortak biçimde *aşçı* olarak ifade edilebilir.

Sözlüksel biçime dönüştürme için çeşitli yöntemler kullanılmaktadır. Bazı yöntemler basit ve kural tabanlıdır, ancak
bazıları özel bir eğitim gerektirmektedir. Aşağıda kullanılan yöntemleri bir tablo biçiminde gösteriyoruz:

+------------------------------------+----------------------------------------------------------------------------+
| Yöntem                             | Temel Yaklaşım                                                             |
+====================================+============================================================================+
| Sözlük (Lookup) Tabanlı Yöntemler  | Çekimli biçim → lemma eşlemelerini içeren önceden hazırlanmış tablolarda   |
|                                    | doğrudan arama yapılır                                                     |
+------------------------------------+----------------------------------------------------------------------------+
| Kural Tabanlı Yöntemler            | Dilbilgisel ek çıkarma / dönüştürme kuralları elle yazılır; ekler sırayla  |
|                                    | atılır (örn. -ler/-lar, -ing, -ed)                                         |
+------------------------------------+----------------------------------------------------------------------------+
| Sonlu Durumlu Dönüştürücüler (FST) | Morfolojik çözümleme iki seviyeli morfoloji (two-level morphology) ve FST  |
| ile Morfolojik Analiz              | ile modellenir; yüzey biçim ↔ sözcük kökü + morfemler eşlemesi yapılır     |
+------------------------------------+----------------------------------------------------------------------------+
| POS Etiketi Destekli Lemmatization | Önce sözcüğün sözcük türü (POS) belirlenir, lemma bu bilgiye göre seçilir  |
|                                    | (WordNet Lemmatizer bu yaklaşımı kullanır)                                 |
+------------------------------------+----------------------------------------------------------------------------+
| İstatistiksel / Öğrenme Tabanlı    | Çekimli biçim → lemma dönüşümü etiketli veriden öğrenilir; dönüşüm         |
| Yöntemler                          | genellikle düzenleme (edit) işlemleri dizisi olarak sınıflandırılır        |
+------------------------------------+----------------------------------------------------------------------------+
| Sinir Ağı Tabanlı Yöntemler        | Karakter düzeyinde seq2seq (encoder-decoder) modellerle çekimli biçimden   |
| (Seq2Seq)                          | lemma karakter karakter üretilir; bağlam LSTM / Transformer ile            |
|                                    | eklenebilir                                                                |
+------------------------------------+----------------------------------------------------------------------------+

Yukarıdaki yöntemlerin arasındaki avantajları ve dezavantajları da aşağıdaki tabloda veriyoruz:

+--------------------------------+------------------------------------------+------------------------------------------+
| Yöntem                         | Avantajları                              | Dezavantajları                           |
+================================+==========================================+==========================================+
| Sözlük (Lookup) Tabanlı        | Hızlı, deterministik, yüksek doğruluk    | Sözlükte olmayan sözcüklerde (OOV)       |
| Yöntemler                      | (sözlükte varsa)                         | başarısız; büyük bellek gereksinimi;     |
|                                |                                          | sözlük güncelleme maliyetli              |
+--------------------------------+------------------------------------------+------------------------------------------+
| Kural Tabanlı Yöntemler        | Sözlük gerektirmez veya küçük sözlükle   | Kural yazımı uzmanlık ister; kural       |
|                                | çalışır; yorumlanabilir                  | çatışmaları ve istisnalar sorun yaratır; |
|                                |                                          | dile özgü, taşınabilir değil             |
+--------------------------------+------------------------------------------+------------------------------------------+
| Sonlu Durumlu Dönüştürücüler   | Sondan eklemeli dillerde (Türkçe, Fince) | FST'nin elle inşası çok emek ister;      |
| (FST) ile Morfolojik Analiz    | çok başarılı; hem analiz hem üretim      | belirsizlik giderme (disambiguation)     |
|                                | yapabilir; hızlı çalışır                 | ayrı bir aşama gerektirir                |
+--------------------------------+------------------------------------------+------------------------------------------+
| POS Etiketi Destekli           | Belirsizliği azaltır (örn. 'saw' → 'see' | POS etiketleyicinin hatası lemmatizer'a  |
| Lemmatization                  | fiil / 'saw' isim); bağlam duyarlıdır    | yayılır; iki aşamalı boru hattı          |
|                                |                                          | karmaşıklığı                             |
+--------------------------------+------------------------------------------+------------------------------------------+
| İstatistiksel / Öğrenme        | Görülmemiş sözcüklere genelleme yapar;   | Etiketli veri gerektirir; hatalar        |
| Tabanlı Yöntemler              | kural yazımı gerekmez                    | öngörülemez olabilir                     |
+--------------------------------+------------------------------------------+------------------------------------------+
| Sinir Ağı Tabanlı Yöntemler    | En yüksek doğruluk; bağlamı ve OOV       | Eğitim ve çıkarım maliyeti yüksek; veri  |
| (Seq2Seq)                      | sözcükleri iyi işler; çok dilli modeller | gereksinimi fazla; deterministik değil   |
|                                | mümkün                                   |                                          |
+--------------------------------+------------------------------------------+------------------------------------------+

Biz kursumuzda sözlüksel biçime dönüştürme algoritmaları üzerinde durmayacağız. Ancak bu işlemlerin hazır
kütüphanelerle nasıl yapıldığına ilişkin bilgiler vereceğiz.

Sözlüksel biçime dönüştürme işlemi de dile oldukça bağımlıdır. Türkçe uygulamalarda Türkçe için özel yazılmış
kütüphanelerin kullanılmasını tavsiye ediyoruz.


Sözlüksel Biçime Dönüştürme ve Sözcük Türü Etiketleme
=======================================================

NLTK ile Sözlüksel Biçime Dönüştürme
--------------------------------------

Sözlüksel biçime dönüştürme (lemmatization) işlemi çeşitli kütüphaneler tarafından gerçekleştirilmektedir.
HuggingFace gibi transformer tabanlı kütüphanelerde sözlüksel biçime dönüştürmeye ilişkin sınıflar ya da
fonksiyonlar bulunmamaktadır.

NLTK kütüphanesinde İngilizce sözlüksel biçime dönüştürme işlemi için ``nltk.stem`` modülünde
``WordNetLemmatizer`` sınıfı bulunmaktadır. Sınıfın kullanılması oldukça kolaydır. ``WordNetLemmatizer`` sınıfı
türünden bir nesne yaratılır. Sonra bu nesne ile sınıfın ``lemmatize`` metodu çağrılır. ``lemmatize`` metodu bir
sözcük alıp onun sözlük biçimine geri dönmektedir. Örneğin:

.. code-block:: python

    from nltk.stem import WordNetLemmatizer

    lemmatizer = WordNetLemmatizer()

    words = [
        'running', 'better', 'caring', 'mice', 'geese', 'feet', 'children',
        'questioning', 'building', 'going', 'been'
    ]

    for word in words:
        lemma = lemmatizer.lemmatize(word)
        print(f'{word:<15}-> {lemma}')

``lemmatize`` metodunun isteğe bağlı ``pos`` parametresi de vardır. Bu ``pos`` parametresi sözcüğün grubunu
metoda söyler. Metot da bu gruba uygun bir dönüştürme yapar. İkinci parametre şunlardan biri olabilir:

- ``'n'`` -> nouns
- ``'v'`` -> verb
- ``'a'`` -> adjectives
- ``'r'`` -> adverbs
- ``'s'`` -> satellite adjectives

Bu parametrenin default değeri ``'n'`` biçimindedir. ``pos`` parametresi belirtilmişse yalnızca o türden
sözcükler üzerinde dönüştürme yapılmaktadır, diğer sözcükler olduğu gibi bırakılmaktadır. Yukarıdaki programda
``pos='n'`` için şu sonuçlar elde edilmiştir:

Bu program çalıştırıldığında aşağıdaki gibi bir çıktı elde edilecektir:

.. code-block:: text

    running        -> running
    better         -> better
    caring         -> caring
    mice           -> mouse
    geese          -> goose
    feet           -> foot
    children       -> child
    questioning    -> questioning
    building       -> building
    going          -> going
    been           -> bee

Görüldüğü gibi yalnızca isim olan sözcükler üzerinde dönüştürmeler yapılmıştır. Peki biz sözcüğün türünü
bilmiyorsak ne yapabiliriz? Aslında yukarıda da belirttiğimiz gibi sağlıklı bir sözlüksel biçime dönüştürme
işlemi için bizim sözcüğün türünü de bilmemiz gerekir. Fakat bunu da bilmiyorsak tüm türler için aynı işlemleri
yeniden yapmaktan başka çaremiz kalmaz. Örneğin:

.. code-block:: python

    from nltk.stem import WordNetLemmatizer

    lemmatizer = WordNetLemmatizer()

    words = [
        'running', 'better', 'caring', 'mice', 'geese', 'feet', 'children',
        'questioning', 'building', 'going', 'been'
    ]

    pos_types = 'nvars'

    lemmas = words.copy()
    for pos_type in pos_types:
        for i in range(len(lemmas)):
            lemmas[i] = lemmatizer.lemmatize(lemmas[i], pos=pos_type)

    for word, lemma in zip(words, lemmas):
        print(f'{word:<15}→ {lemma}')

Buradan şöyle bir çıktı elde edilmiştir:

.. code-block:: text

    running        → run
    better         → good
    caring         → care
    mice           → mouse
    geese          → goose
    feet           → foot
    children       → child
    questioning    → question
    building       → build
    going          → go
    been           → be

Yukarıda da belirttiğimiz gibi sağlıklı bir sözlüksel biçime dönüştürme işleminden önce aslında sözcük
türlerinin elde edilmiş olması gerekir. Aksi takdirde aşağıdaki gibi sorunlar çıkabilmektedir:

.. code-block:: python

    word = 'leaves'

    lemma =  lemmatizer.lemmatize(word, pos='n')
    print(lemma)            # leaf

    lemma =  lemmatizer.lemmatize(word, pos='v')
    print(lemma)            # leave

Sözcük türü etiketlemesi (*POS tagging (part-of-speech tagging)*) morfolojik analizde önemli bir kavramdır.
Cümle içerisindeki her sözcüğe onun türünü belirten bir etiket atanması işlemine denilmektedir. Morfolojik
analiz işlemi yapan kütüphanelerde her zaman bulunan bir niteliktir. Biz aslında kavramla daha önce de
karşılaşmıştık. NLTK'de sözcük türü etiketlemesi için ``pos_tag`` isimli fonksiyon kullanılmaktadır. Örneğin:

.. code-block:: python

    from nltk import pos_tag

    words = ['i', 'am', 'going', 'to', 'school']
    tag_list = pos_tag(words)       # [('i', 'NN'), ('am', 'VBP'), ('going', 'VBG'), ('to', 'TO'), ('school', 'NN')]

Burada default kullanılan etiketleme sistemi *Penn Treebank* sistemidir. Bunların listesi aşağıdaki tabloda
verilmektedir:

.. list-table:: Penn Treebank Etiket Kümesi
   :header-rows: 1
   :widths: 10 45 25

   * - Etiket
     - Anlam
     - Örnek
   * - CC
     - bağlaç, eşdüzey
     - and, but, or
   * - CD
     - sayı, asıl
     - one, 42
   * - DT
     - belirteç (determiner)
     - the, a, this
   * - EX
     - varoluşsal "there"
     - there (is)
   * - FW
     - yabancı sözcük
     - de facto
   * - IN
     - edat / yantümce bağlacı
     - in, of, because
   * - JJ
     - sıfat (veya sıra sayısı)
     - big, old, first
   * - JJR
     - sıfat, karşılaştırma
     - bigger
   * - JJS
     - sıfat, üstünlük
     - biggest
   * - LS
     - liste öğesi imi
     - 1., a)
   * - MD
     - kiplik yardımcı fiili
     - can, must, will
   * - NN
     - isim, cins, tekil/kütle
     - dog, water
   * - NNS
     - isim, cins, çoğul
     - dogs
   * - NNP
     - özel isim, tekil
     - Ankara, Alice
   * - NNPS
     - özel isim, çoğul
     - the Alps
   * - PDT
     - ön-belirteç
     - all (the), both
   * - POS
     - iyelik eki
     - 's
   * - PRP
     - şahıs zamiri
     - I, she, it
   * - PRP$
     - iyelik zamiri
     - my, her, its
   * - RB
     - zarf
     - quickly, very
   * - RBR
     - zarf, karşılaştırma
     - faster
   * - RBS
     - zarf, üstünlük
     - fastest
   * - RP
     - parçacık (particle)
     - up (give up)
   * - SYM
     - sembol
     - %, &
   * - TO
     - "to" edat/mastar imi
     - to (go)
   * - UH
     - ünlem
     - oh, wow
   * - VB
     - fiil, yalın hal
     - run, see
   * - VBD
     - fiil, geçmiş zaman
     - ran, saw
   * - VBG
     - fiil, -ing biçimi / ulaç
     - running
   * - VBN
     - fiil, past participle
     - seen, taken
   * - VBP
     - fiil, geniş zaman, 3. tekil dışı
     - (I) run
   * - VBZ
     - fiil, geniş zaman, 3. tekil
     - (she) runs
   * - WDT
     - WH-belirteç
     - which, that
   * - WP
     - WH-zamiri
     - who, what
   * - WP$
     - WH-zamiri, iyelik
     - whose
   * - WRB
     - WH-zarfı
     - where, when

Tablodan gördüğünüz gibi, başı belirli bir harfle başlayan etiketler belli bir grubu belirtmektedir. Örneğin
'J' ile başlayan etiketler sıfatlara ilişkindir, 'V' ile başlayan etiketler fiillere ilişkindir. ``pos_tag``
fonksiyonunda ``tagset`` parametresi *universal* girilirse aşağıdaki etiket kümesi kullanılmaktadır:

.. list-table:: Universal Etiket Kümesi
   :header-rows: 1
   :widths: 12 30 40

   * - Etiket
     - Anlam
     - Katlanan Penn Etiketleri
   * - VERB
     - fiil (kiplikler dahil)
     - VB VBD VBG VBN VBP VBZ MD
   * - NOUN
     - isim
     - NN NNS NNP NNPS
   * - PRON
     - zamir
     - PRP PRP$ WP WP$
   * - ADJ
     - sıfat
     - JJ JJR JJS
   * - ADV
     - zarf
     - RB RBR RBS WRB
   * - ADP
     - edat/ilgeç
     - IN
   * - CONJ
     - bağlaç
     - CC
   * - DET
     - belirteç
     - DT PDT WDT EX
   * - NUM
     - sayı
     - CD
   * - PRT
     - parçacık
     - RP TO POS
   * - X
     - diğer/belirsiz
     - FW LS SYM UH
   * - .
     - noktalama
     - . , : ( ) '' \`\` # $ vb.

Örneğin:

.. code-block:: python

    from nltk import pos_tag

    words = ['i', 'am', 'going', 'to', 'scool']
    tag_list = pos_tag(words, tagset='universal')
    print(tag_list)     # [('i', 'NOUN'), ('am', 'VERB'), ('going', 'VERB'), ('to', 'PRT'), ('scool', 'VERB')]

Şimdi de NLTK'de önce sözcüğün tür etiketini elde edip ``lemmatize`` metoduna doğru etiketi geçirelim:

.. code-block:: python

    from nltk.stem import WordNetLemmatizer
    from nltk import pos_tag

    lemmatizer = WordNetLemmatizer()

    words = [
        "The", "falling", "leaves", "drift", "by", "the", "window",
        "The", "autumn", "leaves", "of", "red", "and", "gold",
        "I", "see", "your", "lips,", "the", "summer", "kisses",
        "The", "sun-burned", "hands", "I", "used", "to", "hold"
    ]

    def tag_to_lempos(tag):
        if tag.startswith('J'):
            return 'a'
        elif tag.startswith('V'):
            return 'v'
        elif tag.startswith('R'):
            return 'r'
        else:
            return 'n'

    word_tags = pos_tag(words)
    for word, tag in word_tags:
        lemma = lemmatizer.lemmatize(word, pos=tag_to_lempos(tag))
        print(f'{word:<15}-> {lemma}')

Burada ``tag_to_lempos`` fonksiyonu Penn Treebank sistemine ilişkin etiketi ``lemmatize`` fonksiyonunun ``pos``
parametresine uygun hale getirmektedir. Programın çıktısı şöyledir:

.. code-block:: text

    The            -> The
    falling        -> fall
    leaves         -> leaf
    drift          -> drift
    by             -> by
    the            -> the
    window         -> window
    The            -> The
    autumn         -> autumn
    leaves         -> leave
    of             -> of
    red            -> red
    and            -> and
    gold           -> gold
    I              -> I
    see            -> see
    your           -> your
    lips,          -> lips,
    the            -> the
    summer         -> summer
    kisses         -> kiss
    The            -> The
    sun-burned     -> sun-burned
    hands          -> hand
    I              -> I
    used           -> use
    to             -> to
    hold           -> hold

NLTK kütüphanesinin Türkçe işlem yapamadığını, dolayısıyla yukarıdaki sözlüksel biçime dönüştürme işlemlerinin
Türkçe için yapılamayacağını bir kez daha vurgulamak istiyoruz.

Sözcük Türü Etiketleme Yöntemleri
-------------------------------------

Sözcük türü etiketlemesi bağlamdan bağımsız yapılabilecek bir işlem değildir. Çünkü örneğin *leaves* sözcüğünün
bir bağlam olmadan *terketmek* sözcüğü ile mi yoksa *yapraklar* sözcüğü ile mi ilgili olduğu anlaşılamaz.
Aslında yukarıda gördüğümüz gibi ``pos_tag`` fonksiyonu bağlamı dikkate almaktadır. Tabii bizim ``pos_tag``
fonksiyonundan önce yazıyı sözcük tabanlı atomlarına ayırmamız gerekir. Aslında bu amaçla tam uyumu yakalamak
için daha önce görmüş olduğumuz NLTK'deki ``TreeBankWordTokenizer`` sınıfını ya da doğrudan
``nltk.tokenize`` fonksiyonunu kullanabiliriz:

.. code-block:: python

    from nltk.stem import WordNetLemmatizer
    from nltk import word_tokenize, pos_tag

    lemmatizer = WordNetLemmatizer()

    autumn_leaves = """
        The falling leaves drift by the window
        The autumn leaves of red and gold
        I see your lips, the summer kisses
        The sun-burned hands I used to hold
    """

    def tag_to_lempos(tag):
        if tag.startswith('J'):
            return 'a'
        elif tag.startswith('V'):
            return 'v'
        elif tag.startswith('R'):
            return 'r'
        else:
            return 'n'

    words = word_tokenize(autumn_leaves)
    word_tags = pos_tag(words)
    for word, tag in word_tags:
        lemma = lemmatizer.lemmatize(word, pos=tag_to_lempos(tag))
        print(f'{word:<15}-> {lemma}')

Peki sözcük türü etiketlemesi nasıl yapılmaktadır? İşte bunun için de aslında çeşitli yöntemler
kullanılabilmektedir:

.. list-table:: Sözcük Türü Etiketleme Yöntemleri
   :header-rows: 1
   :widths: 25 75

   * - Yöntem
     - Ana Fikir
   * - Sözlük / Unigram
     - Her sözcüğe eğitim verisindeki en sık etiketini ata; bağlam yok
   * - Kural tabanlı (Regexp)
     - Elle yazılmış örüntüler: '-ing ile bitiyorsa VBG' gibi kurallar
   * - Brill (dönüşüm tabanlı)
     - Kaba tahminle başla; hatayı azaltan düzeltme kurallarını veriden öğren
   * - N-gram (bigram/trigram)
     - Önceki 1-2 etikete koşullu en olası etiketi seç; seyreklik sorunu var
   * - HMM + Viterbi
     - Etiket dizisini gizli durum zinciri say; en olası yolu Viterbi ile bul
   * - TnT
     - Ölçeklenmiş trigram HMM; yumuşatma ve sonek analiziyle pratik/hızlı
   * - MaxEnt (log-linear)
     - Zengin özniteliklerle (ek, büyük harf, komşu) koşullu olasılık modeli
   * - CRF
     - Tüm diziyi tek seferde puanlayan ayrımcı model; etiket etkileşimini öğrenir
   * - Averaged Perceptron
     - Basit lineer güncelleme + ağırlık ortalama; NLTK pos_tag'in motoru
   * - BiLSTM(-CRF)
     - Sözcük gömmeleri + çift yönlü RNN; bağlamı iki yönden okur
   * - Transformer (BERT vb.)
     - Önceden eğitilmiş bağlamsal gömmeler + ince ayar; güncel en iyi sonuç

Görüldüğü gibi POS işlemi için *kural tabanlı yöntemler*, *istatistiksel ve olasılıksal yöntemler*, *sinir ağı
içeren yöntemler* uygulanabilmektedir.

spaCy ile Türkçe Sözlüksel Biçime Dönüştürme
------------------------------------------------

Türkçe sözlüksel biçime dönüştürme işlemleri spaCy kütüphanesiyle yapılabilir. Anımsanacağı gibi spaCy
kütüphanesinin Türkçe desteği bulunmaktadır. Biz spaCy kütüphanesinin nasıl kullanıldığını kabaca görmüştük.
Önce eğitim veri kümesi belirtilerek bir model nesnesi oluşturuluyordu. Bu model nesnesine bir yazı verilerek
ondan doküman nesnesi elde ediliyordu. Faydalı bilgiler bu doküman nesnesinin dolaşılmasıyla elde ediliyordu.
Doküman nesnesinin dolaşılmasıyla elde edilen token nesnelerinin ``lemma_`` özniteliği ilgili sözcüğün
sözlüksel biçimini vermektedir. Örneğin:

.. code-block:: python

    import spacy

    nlp = spacy.load('tr_core_news_md')

    text = 'Araştırmacılar doğal dil işleme yöntemlerini inceliyordu.'
    doc = nlp(text)

    print('spaCy Türkçe Lemmatization:')
    print('┌─────────────────────────┬──────────┬─────────────────────────┐')
    print('│         Sözcük          │   POS    │          Lemma          │')
    print('├─────────────────────────┼──────────┼─────────────────────────┤')
    for token in doc:
        if not token.is_punct and not token.is_space:
            print(f'│ {token.text:<23} │ {token.pos_:<8} │ {token.lemma_:<23} │')
    print('└─────────────────────────┴──────────┴─────────────────────────┘')

Buradan şöyle bir çıktı elde edilmiştir:

.. code-block:: text

    spaCy Türkçe Lemmatization:
    ┌─────────────────────────┬──────────┬─────────────────────────┐
    │         Sözcük          │   POS    │          Lemma          │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ Araştırmacılar          │ NOUN     │ araştırmacı             │
    │ doğal                   │ ADJ      │ doğal                   │
    │ dil                     │ NOUN     │ dil                     │
    │ işleme                  │ VERB     │ işle                    │
    │ yöntemlerini            │ NOUN     │ yöntem                  │
    │ inceliyordu             │ VERB     │ incel                   │
    └─────────────────────────┴──────────┴─────────────────────────┘

Stanza ile Türkçe Sözlüksel Biçime Dönüştürme
--------------------------------------------------

Şimdi de Türkçe sözlüksel biçime dönüştürme işleminin Stanza ile nasıl yapıldığını görelim. Biz Stanza'nın
nasıl kullanıldığını kabaca zaten görmüştük. Stanza önce metni cümlelere dönüştürüp, cümleleri atomlarına
ayırıyordu. Biz daha önce aşağıdaki gibi bir örnek yapmıştık:

.. code-block:: python

    import stanza

    text = """
        Bugün hava çok güzel!!!     Herkes kıralara gitti...... Ama ben Ağrı Dağına çıktım."
    """

    nlp = stanza.Pipeline(lang='tr', processors='tokenize')
    doc = nlp(text)

    for sentence in doc.sentences:
        for token in sentence.tokens:
            print(f'token: {token.text}, start: {token.start_char}, end: {token.end_char}')

Biz daha önce ``sentence`` nesnesinin ``tokens`` özniteliğini kullanmıştık. Sözlüksel biçime dönüştürmede
``tokens`` özniteliği yerine ``words`` özniteliğinin kullanılması daha uygundur. Çünkü ``words`` özniteliği
sözcükleri hedefimize daha uygun bir biçimde atomlarına ayırmaktadır. Ayrıca ``Pipeline`` nesnesini
oluştururken bizim ``processors`` parametresine ``"lemma"`` özelliğini de eklememiz gerekir. Örneğin:

.. code-block:: python

    nlp = stanza.Pipeline(lang='tr', processors='tokenize,pos,lemma')

Artık ``word`` nesnesinin içerisinden ``lemma`` özniteliği ile sözcüğün sözlüksel biçimini elde edebiliriz:

.. code-block:: python

    import stanza

    nlp = stanza.Pipeline(lang='tr', processors='tokenize,pos,lemma')

    text = """
        Bugün hava çok güzel!!!     Herkes kıralara gitti...... Ama ben Ağrı Dağına çıktım."
    """

    doc = nlp(text)

    print('Stanza Türkçe Lemmatization:')
    print('┌─────────────────────────┬──────────┬─────────────────────────┐')
    print('│         Sözcük          │   POS    │          Lemma          │')
    print('├─────────────────────────┼──────────┼─────────────────────────┤')

    for sentence in doc.sentences:
        for word in sentence.words:
            if word.upos != 'PUNCT':
                print(f'│ {word.text:<23} │ {word.upos:<8} │ {word.lemma:<23} │')
                print('└─────────────────────────┴──────────┴─────────────────────────┘')

Buradan şöyle bir çıktı elde edilmiştir:

.. code-block:: text

    ┌─────────────────────────┬──────────┬─────────────────────────┐
    │         Sözcük          │   POS    │          Lemma          │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ Bugün                   │ NOUN     │ bugün                   │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ hava                    │ NOUN     │ hava                    │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ çok                     │ ADV      │ çok                     │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ güzel                   │ ADJ      │ güzel                   │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ Herkes                  │ NOUN     │ herkes                  │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ kıralara                │ NOUN     │ kıra                    │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ gitti                   │ VERB     │ git                     │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ Ama                     │ CCONJ    │ ama                     │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ ben                     │ PRON     │ ben                     │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ Ağrı                    │ NOUN     │ ağır                    │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ Dağına                  │ NOUN     │ dağın                   │
    ├─────────────────────────┼──────────┼─────────────────────────┤
    │ çıktım                  │ VERB     │ çık                     │
    └─────────────────────────┴──────────┴─────────────────────────┘

