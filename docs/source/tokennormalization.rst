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


