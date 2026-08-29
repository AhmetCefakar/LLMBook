=================================================
Sözcük Hazinesinin Sayısal Biçime Dönüştürülmesi
=================================================


Sözcük Hazinesinin Sayısal Biçime Dönüştürülmesine Giriş
=========================================================

Biz atomlara ayırma sonrasındaki normalizasyonları da gördük. Şimdi artık tüm bu süreçler sonucunda elde
edilen sözcük hazinesini sayısal biçime dönüştürmeliyiz. Çünkü algoritmalar sayılar üzerinde işlem
yapmaktadır. Bu bölümde oluşturulan sözlük hazinesinin sayısal biçime dönüştürülmesi süreciyle ilgili
bilgiler vereceğiz.

Atomlara Id Atama ve Özel Atomlar
---------------------------------------

Atomlar için sayısal değerler üretilmesi aslında oldukça kolaydır. Örneğin elimizde atomlardan oluşan bir
liste varsa biz bu listeyi dolaşıp sırasıyla onlara 0'dan itibaren bir sayı karşılık getirebiliriz. Karşı
getirilen bu sayıya atom id'si (token id) de denilmektedir. Bazı uygulamacılar atomları önce sıraya dizip
ondan sonra numaralandırabilmektedir. Bu sıraya dizme işlemi atomlarla sayıların anlamlandırılmasını
kolaylaştırabilmektedir. Ancak hangi atomlara düşük numara verileceği konusunda tercih atomları elde etme
yöntemine göre de değişebilmektedir. Uygulamalarda genellikle atomlara karşı gelen id'lerin bir biçimde
tutulması da gerekebilmektedir. Çünkü eğitim sürecinden sonra kestirim işlemlerinde yeniden dönüştürme
yapmak için buna gereksinimimiz olacaktır. Bunun için Python'da en uygun veri yapısı şüphesiz sözlüktür.
Sözlüğün anahtarlarını atomlar, değerlerini de onlara karşı düşürülen id'ler oluşturabilir. Örneğin:

.. code-block:: python

    tokens = ['bugün', 'hava', 'iyi', 'güzel', 'kötü', ]
    tokens.sort()

    vocab_dict = {}
    for idx, token in enumerate(tokens):
        vocab_dict[token] = idx

Burada 0'dan itibaren her atoma bir id numarası karşılık getirilmiştir. Tabii yukarıdaki işlem bir
*sözlük içlemiyle (dictionary comprehension)* tek satırda da yapılabilir:

.. code-block:: python

    vocab_dict = {token: idx for idx, token in enumerate(tokens)}

Sözcük hazinesindeki atomları id'lere eşleyen sözlük oluşturulduktan sonra bunun tersini yapan bir sözlüğün
de oluşturulması bazı uygulamalarda (ancak hepsinde değil) gerekli olmaktadır. Ters sözlük (yani id'ler
verildiğinde atomların elde edildiği sözlük) şöyle oluşturulabilir:

.. code-block:: python

    vocab_dict_rev = {idx: token for token, idx in vocab_dict.items()}

Hangi atomlara hangi id'lerin verileceğinin genel olarak bir önemi yoktur. Ancak bu konuda bazı alışkanlıklar
vardır. Yukarıda da belirttiğimiz gibi bazı uygulamacılar önce atomları sıraya dizip sonra onlara id
atamaktadır. Böylece sözlükte önce olan atomlar düşük id alırlar. Ancak BPE ve WordPiece atomlarına ayırma
yönteminde atomlara genellikle elde edilme sırasına göre id'ler atanmaktadır. Unigram LM ve SentencePiece
atomlarına ayırma yönteminde ise genellikle atomlar log olasılığına göre sıraya dizildikten sonra onlara log
olasılığı yüksek olan düşük id'ye sahip olacak biçimde id atanmaktadır.

Anımsayacağınız gibi bir bilgiyi aşağı seviyeli biçime dönüştürmeye İngilizce *encode* işlemi, aşağı
seviyeli bilgiyi yüksek seviyeye dönüştürmeye de *decode* işlemi denilmektedir. Doğal dil işlemede de
yazıların ya da atomların id'ye dönüştürülmesine biz *encode* işlemi, id'lerin atomlarına dönüştürülmesi
işlemine de *decode* işlemi diyebiliriz.

Doğal dil uygulamalarında bazı özel atomlar için de sözlükte yer ayrılması gerekebilmektedir. Örneğin sözlük
hazinesinde olmayan bir atomu temsil eden ve ``[UNK]`` ya da ``<UNK>`` biçiminde gösterilen özel atomun hemen
her modelde bulunması gerekir. Pek çok modelde önce bir eğitim yapılır. Sonra bir metin verilerek kestirim
yapılır. İşte kestirim sırasında kestirilecek metnin de aynı biçimde id'lere dönüştürülmesi gerekir. (Zaten
``vocab_dict`` sözlüğü bu amaçla bulundurulmaktadır.) Ancak kestirilecek metinde eğitimde kullanılmayan
atomlar da bulunabilmektedir. Bu tür durumlarda algoritmaların çalışması için ya sözlük hazinesinde olmayan
atomların atılması ya da onların ``[UNK]`` atomuyla temsil edilmesi yoluna gidilmektedir. Atomun atılması
yerine bilinmeyen atomların ``[UNK]`` gibi özel bir atomla temsil edilmesi genellikle daha iyi bir yöntemdir.
İşte uygulamacının bu tür özel atomlara da sözcük hazinesi sözlüğünde birer id karşılık düşürmesi gerekir.
Doğal dil işleme modellerinin bazılarında eğitime sokulacak metinlerin eşit sayıda atoma sahip olması
zorunlu bir durumdur. Bu tür modellerde uygulamacı metinler için işin başında bir atom uzunluğu belirler.
Bu atom uzunluğundan daha uzun olan metinleri kırpar, bu atom uzunluğundan daha kısa olan metinleri de özel
bir atomla doldurur. Bu tür atomlar ``[PAD]`` ya da ``<PAD>`` gibi sembollerle temsil edilmektedir. Doldurma
genellikle 0 sayılarıyla yapıldığı için ``[PAD]`` atomuna 0 numaralı id'nin verilmesi yaygın bir uygulamadır.
Bazı uygulamalarda başka birtakım özel atomların da kullanılması gerekebilmektedir. Aşağıdaki tabloda
çeşitli uygulamalarda kullanılan özel atomların listesini veriyoruz:

.. list-table:: Özel Atomlar ve İşlevleri
   :header-rows: 1
   :widths: 15 85

   * - Özel Atom
     - İşlevi
   * - ``[PAD]``
     - Diziyi belirli bir uzunluğa tamamlamak için doldurma atomu
   * - ``[UNK]``
     - Sözlükte bulunmayan atomları temsil eder
   * - ``[CLS]``
     - Dizinin başına eklenir, sınıflandırma görevlerinde cümlenin bütünsel temsilini taşır
   * - ``[SEP]``
     - İki cümle/segment arasındaki sınırı belirtir
   * - ``[MASK]``
     - Maskeli dil modelleme (MLM) sırasında gizlenen atomu temsil eder
   * - ``[BOS]``
     - Dizinin başlangıcını işaretler (beginning of sequence)
   * - ``[EOS]``
     - Dizinin sonunu işaretler (end of sequence)

Özel atomların bir listede bulunduğunu varsayarak özel atomlara düşük numara atanması işlemini şöyle
sağlayabiliriz:

.. code-block:: python

    tokens = ['bugün', 'hava', 'iyi', 'güzel', 'kötü', ]
    tokens.sort()

    special_tokens = ['[PAD]', '[UNK]']

    vocab_dict = {token: idx for idx, token in enumerate(special_tokens)}
    for idx, token in enumerate(tokens, len(special_tokens)):
        vocab_dict[token] = idx

Tabii bu işlemi önce özel atomları listenin başına ekleyerek de yapabiliriz. Ancak genel olarak liste
birleştirme göreli biçimde yavaş olma eğilimindedir:

.. code-block:: python

    vocab_dict = {token: idx for idx, token in enumerate(special_tokens + tokens)}


Encode/Decode İşlemleri ve One-Hot-Encoding Yöntemi
=======================================================

Encode ve Decode Fonksiyonları
------------------------------------

Bir atom listesini önceden oluşturulmuş sözlükleri kullanarak id listesine dönüştürmek oldukça kolaydır:

.. code-block:: python

    def encode(tokens):
        result = []
        for token in tokens:
            idx = vocab_dict.get(token, vocab_dict['[UNK]'])
            result.append(idx)
        return result

Tabii biz bu işlemi liste içlemi kullanarak tek satırda da yapabilirdik:

.. code-block:: python

    def encode(tokens):
        return [vocab_dict.get(token, vocab_dict['[UNK]']) for token in tokens]

Şimdi de bir id listesini atom listesine dönüştüren ``decode`` fonksiyonu yazalım:

.. code-block:: python

    def decode(idxs):
        return [vocab_dict_rev.get(idx) for idx in idxs]

Şimdi bir test yapalım:

.. code-block:: python

    tokens = ['bugün', 'iyi', 'kötü', 'deniz']
    idxs = encode(tokens)
    print(idxs)             # [2, 5, 6, 1]

    tokens = decode(idxs)
    print(tokens)           # ['bugün', 'iyi', 'kötü', '[UNK]']

Burada önce atomlar id'lere dönüştürülmüş sonra da bunun tersi yapılmıştır. *deniz* atomunun sözcük
hazinesinde olmadığına dikkat ediniz. ``encode`` fonksiyonu *deniz* atomu için 1 id değerini vermiştir. 1 id
değeri ``decode`` fonksiyonuna sokulduğunda ``[UNK]`` atomu elde edilmiştir. ``encode`` fonksiyonunun
atomları aldığına dikkat ediniz. Eğer bu fonksiyon yazıyı parametre olarak alsaydı önce onu atomlarına
ayırarak işlemini yapabilirdi.

One-Hot-Encoding Yöntemi
------------------------------

Atomların sayısal id değerlerine dönüştürülerek algoritmalara yazıların değil de bu sayısal id değerlerinin
verildiğini söylemiştik. Ancak bu süreçte bazı ayrıntılar bulunmaktadır. Pek çok algoritma (ama hepsi değil)
sayılarla işlem yaparken sanki onların arasında olmayan bir sayısal ilişki oluşturmaya çalışabilmektedir. Bu
kullanılan algoritmaların doğasından kaynaklanmaktadır. Örneğin sözcük hazinesinde aşağıdaki gibi atomlar
bulunuyor olsun:

.. code-block:: text

    "[UNK]"   ---> 0
    "kırmızı" ---> 1
    "yeşil"   ---> 2
    "mavi"    ---> 3
    ...

Burada *kırmızı* + *mavi* == *yeşil* gibi ya da *yeşil* > *kırmızı* gibi istenmeyen bir etki oluşabilmektedir.
İşte bu etkiyi kırabilmek için algoritmalara doğrudan id'ler değil bu ilişkilerin kırıldığı vektörler
verilmektedir. Bunun için en yaygın kullanılan bir yöntem *one-hot-encoding* denilen yöntemdir.
One-hot-encoding yönteminde her atom sözcük hazinesi büyüklüğünde sütundan oluşan bir vektör haline
getirilir. Bu vektörün yalnızca id numaralı indeksteki elemanı 1, diğer tüm elemanları 0'dır. (*One-hot*
terimi binary bir sayıda yalnızca tek bitin 1 olduğunu anlatmak için kullanılmaktadır. Ancak terim daha çok
matematik ve istatistik bağlamında yaygınlık kazanmıştır. Aşağı seviyeli yazılımda genellikle bu durum
*bit mask* terimi ile ifade edilmektedir.) Örneğin aşağıdaki gibi bir sözlük hazinesi olsun:

.. code-block:: python

    vocab_dict = {
        '[PAD]': 0,
        '[UNK]': 1,
        'bugün': 2,
        'hava': 3,
        'çok': 4,
        'güzel': 5,
        'yarın': 6,
        'kötü': 7,
        'olacakmış': 8,
        'meteoroloji': 9,
    }

Burada *güzel* atomunun id değeri 5'tir. İşte one-hot-encoding yöntemine göre biz algoritmaya bu atom için 5
değerini değil 0000001000 vektörünü veririz:

.. code-block:: text

    0123456789
    ----------
    0000010000

Buradaki vektörün sütun sayısının sözlük hazinesi kadar olduğuna dikkat ediniz. Bu durumda *yarın hava çok
kötü olacakmış* gibi bir yazı one-hot-encoding biçiminde kodlandığında aşağıdaki gibi bir matris elde
edilecektir:

.. code-block:: text

    0123456789
    ----------
    0000001000  ---> yarın
    0001000000  ---> hava
    0000100000  ---> çok
    0000000100  ---> kötü
    0000000010  ---> olacakmış

One-hot-encoding yöntemi istenmeyen sayısal ilişkilerin kırılması için oldukça iyi bir yöntemdir. Ancak
yöntemin en önemli dezavantajı büyük vektörler ve büyük matrisler oluşturmasıdır. Örneğin sözlük hazinesi
40000 atomu içeriyor olsun. Her bir atomun 40000 büyüklüğünde bir vektörle ifade edilmesi önemli bir yer
kaybına yol açabilmektedir. Gerçi yukarıda da gördüğünüz gibi one-hot-encoding matrisleri *seyrek (sparse)*
biçimdedir. Bir matrisin çok fazla elemanı 0, az sayıda elemanı 0'dan farklı ise böyle matrislere
*seyrek matrisler (sparse matrices)* denilmektedir. Seyrek matrislerin daha az yer kaplayacak biçimde ifade
edilmesine yönelik çeşitli veri yapıları kullanılmaktadır. Ancak ne olursa olsun algoritmalar genel olarak
seyrek matrisler üzerinde işlemler yapmamaktadır. Çünkü seyrek matrisler üzerinde işlemler oldukça yavaştır.
Yani one-hot-encoding matrislerinin seyrek olması uygulamada sanıldığı kadar fayda sağlamamaktadır. (Teknik
olarak hangi orandaki matrislere seyrek dendiğinin bir ölçüsü yoktur. Ancak fikir vermesi açısından dolu
eleman oranının %5'ten küçük olduğu matrislere seyrek diyebiliriz.)

One-hot-encoding işlemi NumPy kütüphanesi kullanılarak manuel bir biçimde yapılabilir. (Bu tür işlemlerin
NumPy ve Pandas gibi kütüphaneler kullanılmadan saf Python ile yapılması kötü bir tekniktir.)

.. code-block:: python

    def make_ohe(vocab_dict, text):
        tokens = text.split()
        ohe_matrix = np.zeros((len(tokens), len(vocab_dict)))
        for row, token in enumerate(tokens):
            idx = vocab_dict.get(token, vocab_dict['[UNK]'])
            ohe_matrix[row, idx] = 1
        return ohe_matrix

Fonksiyonun birinci parametresi sözcük hazinesinin bulunduğu sözlük nesnesini, ikinci parametresi
one-hot-encoding yapılacak yazıyı almaktadır. Fonksiyonu şöyle kullanabiliriz:

.. code-block:: python

    vocab_dict = {
        '[PAD]': 0,
        '[UNK]': 1,
        'bugün': 2,
        'hava': 3,
        'çok': 4,
        'güzel': 5,
        'yarın': 6,
        'kötü': 7,
        'olacakmış': 8,
        'meteoroloji': 9,
    }

    one_matrix = make_ohe(vocab_dict, 'yarın hava çok kötü olacakmış diyorlar')
    print(ohe_matrix)

Kodun çalıştırılmasıyla aşağıdaki çıktı elde edilecektir:

.. code-block:: text

    [[0. 0. 0. 0. 0. 0. 1. 0. 0. 0.]
    [0. 0. 0. 1. 0. 0. 0. 0. 0. 0.]
    [0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]
    [0. 0. 0. 0. 0. 0. 0. 1. 0. 0.]
    [0. 0. 0. 0. 0. 0. 0. 0. 1. 0.]
    [0. 1. 0. 0. 0. 0. 0. 0. 0. 0.]]

scikit-learn ile One-Hot-Encoding
----------------------------------------

One-hot-encoding yapmak için pek çok kütüphanede hazır fonksiyonlar ve sınıflar bulunmaktadır. scikit-learn
kütüphanesinde ``OneHotEncoder`` isimli sınıf one-hot-encoding işlemi için genel amaçlı bir sınıf olarak
tasarlanmıştır. Sınıfın kullanımı genel scikit-learn sınıflarında olduğu gibidir. Önce ``OneHotEncoder``
sınıfı türünden nesne yaratılır:

.. code-block:: python

    ohe = OneHotEncoder(sparse_output=False, categories=[list(range(len(vocab_dict)))])

Eskiden bu sınıf default durumda çıktıları normal matris (dense matrix) olarak veriyordu ancak belli bir
süreden sonra artık default durumda seyrek matris vermeye başlamıştır. ``sparse_output`` parametresi
``False`` geçilerek sınıfın normal matris vermesi sağlanabilir. ``OneHotEncoder`` sınıfının ``__init__``
metodunun ``categories`` parametresi olası tüm değerlerin neler olduğunu belirtmektedir. Eğer bu parametre
girilmezse fit işleminde verilen farklı değerlerin tüm kategorileri oluşturduğu varsayılmaktadır. Bu
parametrenin de iki boyutlu bir liste biçiminde girilmesi gerekir. (scikit-learn sınıfları genel olarak iki
boyutlu dizilerle çalışacak biçimde tasarlanmıştır. Bu nedenle ``categories`` parametresi de iki boyutlu
olarak verilmelidir.)

Nesne yaratıldıktan sonra önce ``fit`` sonra ``transform`` işlemi yapılabileceği gibi tek hamlede
``fit_transform`` işlemi de yapılabilmektedir. Ancak bu metotlar iki boyutlu dizileri parametre olarak
almaktadır. Eğer biz tek bir yazının her atomu için ayrı bir vektörün oluşturulmasını istiyorsak bu metotlara
sütun vektörü vermeliyiz. Örneğin:

.. code-block:: python

    ohe_matrix = ohe.fit_transform(idxs_np)

    text = 'yarın hava çok kötü olacakmış'
    tokens = text.split()

    idxs = encode(tokens)
    idxs_np = np.array(idxs).reshape(-1, 1)
    print(idxs_np)

Aşağıdaki gibi çıktı elde edilecektir:

.. code-block:: text

    [[0. 0. 0. 0. 0. 0. 1. 0. 0. 0.]
    [0. 0. 0. 1. 0. 0. 0. 0. 0. 0.]
    [0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]
    [0. 0. 0. 0. 0. 0. 0. 1. 0. 0.]
    [0. 0. 0. 0. 0. 0. 0. 0. 1. 0.]]

Kodu bir bütün olarak aşağıda veriyoruz.

.. code-block:: python

    import numpy as np
    from sklearn.preprocessing import OneHotEncoder

    vocab_dict = {
        '[PAD]': 0,
        '[UNK]': 1,
        'bugün': 2,
        'hava': 3,
        'çok': 4,
        'güzel': 5,
        'yarın': 6,
        'kötü': 7,
        'olacakmış': 8,
        'meteoroloji': 9,
    }

    def encode(tokens):
        return [vocab_dict.get(token, vocab_dict['[UNK]']) for token in tokens]

    text = 'yarın hava çok kötü olacakmış'
    tokens = text.split()

    idxs = encode(tokens)
    idxs_np = np.array(idxs).reshape(-1, 1)

    ohe = OneHotEncoder(sparse_output=False, categories=[list(range(len(vocab_dict)))])
    ohe_matrix = ohe.fit_transform(idxs_np)
    print(ohe_matrix)

Pandas ile One-Hot-Encoding, Sözcük Çantası Yöntemi ve CountVectorizer
==========================================================================

Pandas get_dummies ile One-Hot-Encoding
---------------------------------------------

One-hot-encoding işlemleri Pandas kütüphanesindeki ``get_dummies`` fonksiyonu ile de yapılabilmektedir.
Örneğin:

.. code-block:: python

    import pandas as pd

    words = ['bugün', 'hava', 'çok', 'güzel']

    df = pd.DataFrame({'Words': words})

    ohe_df = pd.get_dummies(df, columns=['Words'], dtype=int)
    ohe = ohe_df.to_numpy()
    print(ohe)

Burada önce ``DataFrame`` nesnesi oluşturulup ``get_dummies`` fonksiyonuyla ``Words`` sütunu
one-hot-encoding işlemine sokulmuştur. Bu işlem sonucunda yine ``DataFrame`` nesnesi elde edilmektedir.
``DataFrame`` sınıfının ``to_numpy`` metodu ile ``DataFrame`` nesnesi NumPy dizisine dönüştürülmüştür.

Sözcük Çantası (Bag of Words) Yöntemi
--------------------------------------------

Bazı uygulamalarda bir yazının her atomunun ayrı bir vektör biçiminde kodlanması yerine yazının tek bir
vektör biçiminde kodlanması yoluna da gidilebilmektedir. Bu yöntemde vektör sözcük hazinesi kadar
uzunluktadır. Vektörün yazıyı oluşturan atomların id'lerine ilişkin indeksteki elemanları 1 yapılır, diğer
elemanlar 0 olarak bırakılır. Böylece yazı *yalnızca onu oluşturan atomlara ilişkin id sütunlarındaki
değerlerin 1 olduğu diğer sütunlardaki değerlerin 0 olduğu* tek bir vektör haline getirilmiş olur. Örneğin
sözcük hazinesi şöyle olsun:

.. code-block:: python

    vocab_dict = {
        '[PAD]': 0,
        '[UNK]': 1,
        'bugün': 2,
        'hava': 3,
        'çok': 4,
        'güzel': 5,
        'yarın': 6,
        'kötü': 7,
        'olacakmış': 8,
        'meteoroloji': 9,
    }

Biz de *yarın hava çok kötü olacakmış* yazısını bu yönteme göre şöyle vektör haline getirebiliriz:

.. code-block:: text

    0123456789
    ----------
    0001101110

Bu yöntemde yazıyı oluşturan atomların sırasının artık bir öneminin kalmadığına dikkat ediniz. Bu yöntem saf
one-hot-encoding yöntemine göre daha zayıf bilgi taşısa da daha az yer kaplaması nedeniyle bazı
uygulamalarda tercih edilebilmektedir. Yazıların bu biçimde vektör haline getirilmesine doğal dil işleme
dünyasında *sözcük çantası (bag of words)* denilmektedir. Sözcük çantası yöntemi kısaca *BoW* biçiminde de
ifade edilebilmektedir.

Yukarıdaki biçimde uygulanan sözcük çantası yönteminde bir sözcüğün yazı içerisinde birden fazla kez
geçmesinin bir önemi kalmamaktadır. Yani örneğin yukarıdaki *yarın hava çok kötü olacakmış* yazısının
kodlanmasıyla *yarın hava çok çok kötü olacakmış* yazısının kodlanması arasında bir fark kalmamaktadır. İşte
bu tür durumlarda aynı atomun yazı içerisinde birden fazla kez yinelenmesinin bir fark oluşturması için
sütunlarda 1 yerine yinelenme frekansları da tutulabilmektedir. Sütunlarda frekans değerlerinin tutulması
yönteminde *yarın hava çok çok kötü olacakmış* yazısı şöyle kodlanacaktır:

.. code-block:: text

    0123456789
    ----------
    0001201110

*çok* atomunun id değeri olan vektör elemanının 2 olduğuna dikkat ediniz. Burada frekans temelli sözcük
çantası oluşturmanın daha fazla bilgi içerdiğinden dolayı daha iyi bir temsil oluşturduğunu
düşünebilirsiniz. Ancak bazı uygulamalarda bu durum sanıldığı kadar önemli bir fayda sağlamamaktadır.
Frekans bilgi açısından binary gösterimin üst kümesi olsa da bu ek bilginin faydası göreve, metinlerin
uzunluğuna ve kullanılan modele göre değişebilmektedir.

scikit-learn CountVectorizer ile Sözcük Çantası Oluşturma
-----------------------------------------------------------------

Yazıların sözcük çantası temsilini oluşturmak için scikit-learn kütüphanesindeki
``feature_extraction.text`` modülünde bulunan ``CountVectorizer`` sınıfı sıkça kullanılmaktadır. Biz
``CountVectorizer`` sınıfını daha önce temel düzeyde görmüştük. Anımsayacağınız gibi ``CountVectorizer``
sınıfının genel kullanımı diğer scikit-learn sınıflarına benziyordu. Yani önce ``CountVectorizer`` türünden
bir nesne yaratılıp, sonra ``fit`` işlemi sonra da ``transform`` işlemi yapılıyordu. ``fit`` işlemi sırasında
sözcük hazinesinin oluşturulduğunu belirtmiştik. İşte ``transform`` işlemi de oluşturulan bu sözcük
hazinesine göre yapılmaktadır. Örneğin:

.. code-block:: python

    from sklearn.feature_extraction.text import CountVectorizer

    corpus = [
        'bugün hava çok güzel, değil mi',
        'evet çok güzel',
        'ama yarın ne olur bilinmez',
        'yarın hava kötü olacakmış',
        'umarın yarın hava güzel olur'
    ]

    texts = ['bugün hava güzel mi']

    cv = CountVectorizer()
    cv.fit(corpus)

    print(cv.vocabulary_, end='\n\n')

    bow_text = cv.transform(texts).toarray()
    print(bow_text)

``fit`` işleminden sonra oluşturulan sözcük hazinesi sınıfın ``vocabulary_`` örnek özniteliğinden elde
edilebilir. ``transform`` vektörü ``csr_matrix`` türünden bir seyrek matris biçiminde vermektedir.
``csr_matrix`` sınıfının ``toarray`` metodu seyrek matrisi NumPy dizisine dönüştürmektedir. Yukarıdaki
programın çalıştırılması sonucunda ekranda şunları göreceksiniz:

.. code-block:: text

    {'bugün': 2, 'hava': 6, 'çok': 14, 'güzel': 5, 'değil': 3, 'mi': 8, 'evet': 4, 'ama': 0, 'yarın': 13, 'ne': 9,
    'olur': 11, 'bilinmez': 1, 'kötü': 7, 'olacakmış': 10, 'umarım': 12}

    [[0 0 1 0 0 1 2 0 0 0 0 0 0 0 0]
    [0 0 0 0 1 3 0 0 0 0 0 0 0 0 1]]

``CountVectorizer`` default durumda frekans temelinde sözcük çantası oluşturmaktadır. Eğer binary sözcük
çantası oluşturulmak isteniyorsa sınıfın ``__init__`` metodunun ``binary`` parametresi ``True`` girilmelidir.
``transform`` sırasında sözcük hazinesinde olmayan bir sözcük ile karşılaşıldığında herhangi bir sorun
oluşmamaktadır. O sözcük yokmuş gibi işleme devam edilmektedir. ``CountVectorizer`` sınıfının ``transform``
metodu tek bir yazıyı değil birden fazla yazıyı alıp vektör haline getirebilmektedir. Bu nedenle
``transform`` her zaman bizden yazıları dolaşılabilir bir nesne biçiminde istemektedir.

``CountVectorizer`` sınıfının ``__init__`` metodunun parametrik yapısını anımsayalım:

.. code-block:: python

    class sklearn.feature_extraction.text.CountVectorizer(*, input='content', encoding='utf-8',
            decode_error='strict', strip_accents=None, lowercase=True, preprocessor=None, tokenizer=None,
            stop_words=None, token_pattern='(?u)\\b\\w\\w+\\b', ngram_range=(1, 1), analyzer='word',
            max_df=1.0, min_df=1, max_features=None, vocabulary=None, binary=False, dtype=numpy.int64)
    )

CountVectorizer Parametreleri: preprocessor ve stop_words
-----------------------------------------------------------------

Default durumda ``CountVectorizer`` yazıları ``token_pattern`` parametresinde belirtilen düzenli ifadeyi
kullanarak sözcüklerine ayırmaktadır. Metodun ``lowercase`` parametresi default durumda ``True``
değerindedir. Yani derlemeyi oluşturan yazılar önce küçük harfe dönüştürülüp sonra sözcüklere
ayrıştırılmaktadır. Türkçe metinlerin küçük harfe dönüştürülmesinde sorun oluştuğunu biliyorsunuz. Bu
nedenle eğer bu sınıfı kullanacaksanız küçük harf dönüştürmesini Türkçeye uygun biçimde sizin yapmanız
gerekir. Tabii sınıfın ``transform`` metodu da yazıyı vektör haline getirmeden önce default durumda küçük
harfe dönüştürmektedir. Metodun ``preprocessor`` parametresine bir fonksiyon girilebilir. Bu durumda
derlemdeki ve ``transform`` işlemindeki metinler önce bu fonksiyona sokulmaktadır. Örneğin:

.. code-block:: python

    cv = CountVectorizer(binary=True, preprocessor=lambda text: text.replace('I', 'ı').lower())

Türkçe için yalnızca 'I' harfinin ``lower`` fonksiyonunda soruna yol açtığını anımsayınız.

``__init__`` metodundaki ``stop_words`` parametresine durak sözcükleri girilebilir. Bu durumda yazılar
sözcüklerine ayrıştırıldıktan sonra bu durak sözcükleri sözcük hazinesinden çıkartılmaktadır. Örneğin:

.. code-block:: python

    from spacy.lang.tr.stop_words import STOP_WORDS

    cv = CountVectorizer(binary=True, preprocessor=lambda text: text.replace('I', 'ı').lower(),
            stop_words=list(STOP_WORDS))
    cv.fit(corpus)

    print(cv.vocabulary_)

Buradaki sözcük hazinesi artık şöyledir:

.. code-block:: text

    {'bugün': 1, 'hava': 4, 'güzel': 3, 'evet': 2, 'yarın': 8, 'bilinmez': 0, 'kötü': 5, 'olacakmış': 6, 'umarım': 7}

Gördüğünüz gibi durak sözcükleri atılmıştır.

CountVectorizer Parametreleri: ngram_range
-----------------------------------------------

``CountVectorizer`` sınıfının ``__init__`` metodunun ``ngram_range`` parametresinin işlevini daha önce
*n-gram atomlarına ayırma* yönteminde görmüştük. Bu parametre bir demet almaktadır. Yan yana sözcüklerin
kaç tanesinden atom yapılacağını belirtmektedir. Daha önce de belirttiğimiz gibi uygulamada genellikle
``ngram_range=(1, 1)`` ya da ``ngram_range=(1, 2)`` kullanılmaktadır. ``ngram_range=(1, 2)`` hem tekil
sözcüklerden atom yapılacağını hem de yan yana iki sözcükten atom yapılacağını belirtmektedir. Tekil
sözcüklerden atom yapmaya *unigram*, yan yana iki sözcükten atom yapmaya *bigram*, yan yana üç sözcükten
atom yapmaya da *trigram* dendiğini anımsayınız. Örneğin:

.. code-block:: python

    from sklearn.feature_extraction.text import CountVectorizer

    corpus = [
        'bugün hava çok güzel, değil mi',
        'evet çok güzel',
        'ama yarın ne olur bilinmez',
        'yarın hava kötü olacakmış',
        'umarım yarın hava güzel olur',
    ]

    texts = [
        'bugün hava güzel hava',
        'güzel güzel evet çok güzel'
        ]

    cv = CountVectorizer(binary=True, preprocessor=lambda text: text.replace('I', 'ı').lower(),
            ngram_range=(1, 2))
    cv.fit(corpus)

    print(cv.vocabulary_, end='\n\n')

    bow_text = cv.transform(texts).toarray()
    print(bow_text)

Buradan şöyle bir çıktı elde edilecektir:

.. code-block:: text

    {
        'bugün': 3, 'hava': 12, 'çok': 29, 'güzel': 9, 'değil': 5, 'mi': 18, 'bugün hava': 4, 'hava çok': 15,
        'çok güzel': 30, 'güzel değil': 10, 'değil mi': 6, 'evet': 7, 'evet çok': 8, 'ama': 0, 'yarın': 26,
        'ne': 19, 'olur': 22, 'bilinmez': 2, 'ama yarın': 1, 'yarın ne': 28, 'ne olur': 20, 'olur bilinmez': 23,
        'kötü': 16, 'olacakmış': 21, 'yarın hava': 27, 'hava kötü': 14, 'kötü olacakmış': 17, 'umarım': 24,
        'umarım yarın': 25, 'hava güzel': 13, 'güzel olur': 11
    }

    [[0 0 0 1 1 0 0 0 0 1 0 0 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
    [0 0 0 0 0 0 0 1 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1]]

TF-IDF Yöntemi, Klasik Doğal Dil İşleme ve Olasılığa Giriş
==========================================================

TF-IDF Yöntemi
--------------

Yazıları vektörel hale dönüştürmek için *sözcük çantası (bag of words)* yönteminin dışında
*TF-IDF (Term Frequency - Inverse Document Frequency)* denilen bir yöntem de klasik doğal dil işlemede
oldukça sık kullanılmaktadır. Bu yöntemde tüm atomlara (tipik olarak sözcük haznesindeki tüm sözcüklere)
birer TF-IDF değeri karşılık getirilir. Böylece yazılar birer TF-IDF vektörüne dönüştürülür. TF-IDF
yönteminde her sözcük için önce TF ve IDF değerleri hesaplanmaktadır. Bu iki değer çarpılarak TF-IDF değeri
elde edilir:

.. code-block:: text

    TF-IDF = TF * IDF

Bir sözcüğün TF değeri şöyle hesaplanmaktadır:

.. code-block:: text

    TF = sözcüğün_metindeki_sayısı / metindeki_sözcük_sayısı

Örneğin yazı şöyle olsun:

*Bu film çok güzel. Film aynı zamanda çok güzel yerlerde çekilmiş.*

Yazıda toplam 11 sözcük var. Bu durumda *film*, *çok*, *güzel* sözcüklerinin TF değerleri 2/11 diğer
sözcüklerin TF değerleri ise 1/11 olur.

Sözcüklerin IDF değerleri ise şöyle hesaplanmaktadır:

.. code-block:: text

    IDF = log(toplam_metinlerin_sayısı / sözcük_geçen_dokümanların_sayısı)

Buradaki N değeri ilgili sözcüğün geçtiği farklı metinlerin sayısını belirtmektedir. Örneğin *muhteşem*
sözcüğü 5 farklı metinde geçiyor olsun. Toplamda da 100 tane metin olsun. Bu durumda *muhteşem* sözcüğünün
IDF değeri log(5 / 100) olacaktır. IDF değeri scikit-learn gibi bazı kütüphanelerde şöyle de
hesaplanmaktadır:

.. code-block:: text

    IDF = log((1 + toplam_metin_sayısı) / (1 + sözcük_geçen_dokümanların_sayısı)) + 1

Yukarıda da belirttiğimiz gibi sözcüğün TF-IDF değeri TF değeri ile IDF değerinin çarpımından elde
edilmektedir.

TF-IDF yönteminde yine tüm atomlar için (sözcük haznesindeki tüm sözcükler için) bir TF-IDF değeri elde
edilir. Böylece tüm metinler sözcük haznesi uzunluğu kadar sütuna, yorum sayısı kadar satıra sahip olan bir
matrisle temsil edilir. Tabii bu matris de yine seyrek (sparse) biçimde olur. Çünkü bir yorumda sözcük
haznesindeki çok az sözcük kullanılmış olacaktır.

TF-IDF değerleri ne anlama gelmektedir? Bu değerler aslında bir sözcük belli bir metinde çok geçtiğinde ama
diğer metinlerde az geçtiğinde yüksek bir skor üretmektedir. Çünkü bu durumda ilgili sözcüğün metindeki
önemi artmaktadır.

.. list-table:: TF, IDF ve TF-IDF Değerlerinin Yorumu
   :header-rows: 1
   :widths: 15 15 15 55

   * - TF
     - IDF
     - TF-IDF
     - Yorum
   * - Yüksek
     - Yüksek
     - Yüksek
     - Belgede sık, derlemde seyrek: karakteristik konu sözcüğü. İdeal durum.
   * - Yüksek
     - Düşük
     - Düşük
     - Belgede sık ama her belgede var: işlev sözcüğü ('bu', 'bir'). IDF bastırır.
   * - Düşük
     - Yüksek
     - Düşük
     - Derlemde seyrek ama bu belgeyle bağı zayıf: seyreklik tek başına yetmez.
   * - Düşük
     - Düşük
     - Sıfıra yakın
     - Hem seyrek hem yaygın: hiçbir ayırt edicilik taşımaz.

Aşağıdaki tabloda sözcük çantası yöntemiyle TF-IDF yönteminin genel niteliklerini karşılaştırıyoruz:

.. list-table:: Sözcük Çantası (BoW) ile TF-IDF Karşılaştırması
   :header-rows: 1
   :widths: 25 35 40

   * - Özellik
     - Sözcük Çantası (BoW)
     - TF-IDF
   * - Hücre değeri
     - Ham sıklık sayımı (tamsayı)
     - TF × IDF ağırlığı (gerçel sayı)
   * - Bilgi kaynağı
     - Yalnızca yerel (tek belge)
     - Yerel + küresel (tüm derlem)
   * - İşlev sözcükleri
     - Yüksek değer alır, baskındır
     - IDF ile kendiliğinden bastırılır
   * - Ayırt edicilik
     - Yok; tüm sözcükler eşit önemde
     - Var; seyrek sözcükler öne çıkar
   * - Derleme bağımlılık
     - Düşük; belge tek başına yeter
     - Yüksek; IDF derlemden hesaplanır
   * - Yeni belge eklendiğinde
     - Vektörler değişmez
     - IDF, dolayısıyla ağırlıklar değişebilir
   * - Hesaplama maliyeti
     - Çok düşük
     - Düşük (ek IDF geçişi gerekir)
   * - Yorumlanabilirlik
     - Doğrudan (kaç kez geçmiş)
     - Dolaylı (göreli önem skoru)
   * - scikit-learn sınıfı
     - ``CountVectorizer``
     - ``TfidfVectorizer``

Hangi tür uygulamalarda sözcük çantası, hangi tür uygulamalarda TF-IDF yönteminin daha uygun olduğuna
ilişkin olduğuna yönelik bir tabloyu aşağıda veriyoruz:

.. list-table:: Sözcük Çantası mı TF-IDF mi Kullanılmalı?
   :header-rows: 1
   :widths: 40 20 40

   * - Durum
     - Tercih Edilen
     - Gerekçe
   * - Belge benzerliği / arama (retrieval)
     - TF-IDF
     - Karakteristik sözcükler benzerliği belirler
   * - Konu temelli metin sınıflandırma
     - TF-IDF
     - Konu sözcükleri öne çıkar, gürültü bastırılır
   * - Naive Bayes (MultinomialNB) ile eğitim
     - BoW
     - Model tamsayı sayım varsayımına dayanır
   * - LDA gibi olasılıksal konu modelleri
     - BoW
     - Üretici model sayım bekler
   * - Çok küçük derlem (birkaç belge)
     - BoW
     - IDF kestirimleri güvenilmez olur
   * - Sözcük sıklığının kendisi analiz konusuysa
     - BoW
     - Ham sayım doğrudan yorumlanır
   * - Kısa metinler (tweet, başlık)
     - TF-IDF
     - TF zaten 0/1'e yakın; IDF ayırt ediciliği sağlar
   * - Stop-word listesi kullanılamıyorsa
     - TF-IDF
     - İşlev sözcükleri matematiksel olarak bastırılır

scikit-learn TfidfVectorizer ile TF-IDF Hesaplama
-------------------------------------------------

Yazıların TF-IDF yöntemi ile vektörize edilmesi scikit-learn içerisinde ``TfidfVectorizer`` sınıfı
kullanılarak yapılabilmektedir. Sınıfın kullanımı diğer scikit-learn sınıflarında olduğu gibidir.
``TfidfVectorizer`` sınıfının ``__init__`` metodunun parametrik yapısı şöyledir:

.. code-block:: python

    class sklearn.feature_extraction.text.TfidfVectorizer(*, input='content', encoding='utf-8',
            decode_error='strict', strip_accents=None, lowercase=True, preprocessor=None, tokenizer=None,
            analyzer='word', stop_words=None, token_pattern='(?u)\\b\\w\\w+\\b', ngram_range=(1, 1),
            max_df=1.0, min_df=1, max_features=None, vocabulary=None, binary=False,
            dtype=numpy.int64, norm='l2', use_idf=True, smooth_idf=True, sublinear_tf=False)

Örnek bir kullanımı şöyle olabilir:

.. code-block:: python

    from sklearn.feature_extraction.text import TfidfVectorizer

    corpus = [
        "Bu bir örnek belgedir",
        "Bu başka bir belgedir",
        "Bu belge TF-IDF kullanılarak vektörize edilecek"
    ]

    vectorizer = TfidfVectorizer()
    vectorizer.fit(corpus)

``fit`` işleminden sonra sözcük hazinesi ve IDF değerleri oluşturulmuş olur. IDF değerleri nesnenin ``idf_``
özniteliğinden elde edilebilir. ``fit`` işlemi de bu bilgiye dayanılarak yapılmaktadır. Örneğin:

.. code-block:: python

    corpus = [
        "Bu bir örnek belgedir",
        "Bu başka bir belgedir",
        "Bu belge TF-IDF kullanılarak vektörize edilecek"
    ]

    text = ['bu belge vektörize edilecek',
            'bu belge bir örnektir']

    from sklearn.feature_extraction.text import TfidfVectorizer

    vectorizer = TfidfVectorizer()
    vectorizer.fit(corpus)
    result = vectorizer.transform(text).todense()
    print(result)

Sözcük hazinesi yine nesnenin ``vocabulary_`` özniteliğinden elde edilebilir.
