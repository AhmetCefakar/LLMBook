=======================
Klasik Doğal Dil İşleme
=======================


Klasik Doğal Dil İşlemeye Giriş
===============================

Biz önceki bölümlerde tüm doğal dil işleme sürecinde kullanılabilen temel önişlemleri gördük. Bu bölümde
klasik doğal dil işleme yöntemlerine ilişkin temel örnekler üzerinde duracağız. Kursumuzun ağırlık noktası
modern doğal dil işleme ve üretici ağlardır. Ancak o konulara geçmeden önce klasik doğal işleme konuları ve
yöntemleri üzerinde temel fikirleri vermek istiyoruz.

Klasik Doğal Dil İşleme Problemleri ve Yöntemleri
-------------------------------------------------

Sinir ağlarının yoğun kullanılmadığı klasik dönemde doğal işleme süreçleri istatistiksel ve olasılıksal
yöntemlerle ele alınıyordu. Klasik doğal dil işleminin uğraştığı temel problemler ve kullanılan temel
yöntemler şunlardır:

.. list-table:: Klasik Doğal Dil İşleme Problemleri
   :header-rows: 1
   :widths: 25 35 40

   * - İşlem
     - Açıklama
     - Yöntemler
   * - Sözcük türü etiketleme (POS)
     - Her atoma sözcük türünün atanması
     - Kural tabanlı yöntemler, HMM, CRF
   * - Varlık ismi tanıma (NER)
     - Kişi, yer, kurum gibi adların bulunması
     - Sözlük temelli yöntemler, kural tabanlı yöntemler, CRF
   * - Metin sınıflandırma
     - Metnin önceden belirlenmiş sınıflara atanması
     - Naive Bayes, lojistik regresyon, SVM, karar ağaçları
   * - Duygu analizi
     - Metindeki duygusal yönelimin belirlenmesi
     - Sözlük temelli yöntemler, Naive Bayes, lojistik regresyon, SVM
   * - Metin kümeleme
     - Metinlerin gözetimsiz biçimde gruplanması
     - K-Means, hiyerarşik kümeleme
   * - Konu modelleme
     - Metinlerdeki gizli konuların çıkarılması
     - LDA, LSA
   * - Benzerlik hesaplama
     - İki metin arasındaki yakınlığın ölçülmesi
     - Kosinüs benzerliği, Jaccard, Levenshtein uzaklığı
   * - Yazım düzeltme
     - Yanlış yazılmış sözcüklerin düzeltilmesi
     - Levenshtein uzaklığı, Norvig yöntemi, sözlük temelli yöntemler

Sözcük türü etiketlemesi (part-of-speech tagging) metin içerisindeki sözcüklerin türlerinin belirlenmesi
problemidir. Örneğin sözcüğün bir *eylem mi*, *nesne mi*, *özel isim mi* belirttiğinin tespit edilmesi.
Varlık ismi tanıma (named entity recognition) yazı içerisindeki kişi, kurum, yer gibi sözcüklerin tespit
edilmesine denilmektedir. Metin sınıflandırma (text classification) en klasik doğal dil işleme
problemlerinden biridir. Örneğin bir haber metninin *politika ile mi ilgili*, *ekonomi ile mi ilgili*,
*sağlık ile mi ilgili* olduğunun tespit edilmesi tipik bir metin sınıflandırma işlemidir. Duygu analizi
(sentiment analysis) en klasik doğal dil işleme problemlerinden biridir. Bir metnin olumlu mu (positive)
yoksa olumsuz mu (negative) yargı içerdiğinin tespit edilmesi sürecidir. Aslında duygu analizi bir çeşit
metin sınıflandırma işlemidir. Duygu analizinde *olumlu*, *olumsuz* sınıflarının dışında
*nötr (neutral)* sınıfı da kullanılabilmektedir. Örneğin IMDB'de filmler hakkında yazılan yorumların
bazıları olumlu bazıları olumsuz yargı içermektedir. Metin kümeleme (text clustering) metinleri
benzerliklerine göre gruplara ayırma işlemidir. Metin sınıflandırma denetimli (supervised) bir yöntemken
metin kümeleme denetimsiz (unsupervised) bir yöntemdir. Makine öğrenmesinde sınıflandırma ve kümeleme
kavramları farklı anlamalara gelmektedir. Sınıflandırmada sınıflar önceden belirlenmiştir. Eğitim
sonrasında yeni bir metnin bu sınıflardan birine sokulması istenmektedir. Kümelemede ise baştan sınıflar
belli değildir. Zaten onlar algoritma tarafından benzerliklerden ve farklılıklardan faydalanılarak
oluşturulmaktadır. Konu modelleme metin içerisindeki gizli konuların açığa çıkartılması sürecidir. Yazım
düzeltme yazı içerisindeki hataların düzeltilmesine ilişkin süreçleri belirtmektedir. Metin üretimi
(text generation) konusunda klasik doğal dil işleme yöntemleri genel olarak başarısız olmaktadır.

Yukarıdaki klasik doğal dil işleme problemlerinin hepsi sinir ağlarıyla oldukça etkin bir biçimde
çözülebilmektedir. Yani yukarıdaki problemler klasik doğal dil işlemeye özgü problemler değildir, klasik
doğal dil işlemenin üzerinde çalıştığı temel problemlerdir. Daha önceden de belirttiğimiz gibi her ne kadar
modern transformer tabanlı derin sinir ağları doğal dil işlemede bir çığır açmış olsa da klasik doğal dil
işleme yöntemleri geçerliliğini tümden kaybetmiş değildir. Bu yöntemler problemlerin bazılarında hala etkin
biçimde kullanılmaktadır. Sinir ağı yöntemleri önemli ölçüde bilgisayar kaynağı kullanmaktadır.

Makine Öğrenmesi Yöntem Grupları
--------------------------------

Makine öğrenmesinde yöntemler kabaca üç bölüme ayrılmaktadır:

1. Denetimli öğrenme yöntemleri (supervised learning)
2. Denetimsiz öğrenme yöntemleri (unsupervised learning)
3. Pekiştirmeli öğrenme modelleri (reinforcement learning)

Kullanılan yöntemde önce bir eğitim süreci varsa, eğitim sonra kestirimler yapılıyorsa bu grup yöntemlere
denetimli öğrenme yöntemleri denilmektedir. Makine öğrenmesindeki modellerin büyük çoğunluğu denetimli
öğrenme yöntemlerini kullanmaktadır. Denetimli öğrenmede önceden hazırlanmış ve etiketlenmiş verilerle
eğitimler yapılır. Makinenin (burada makine demekle bilgisayarı ve algoritmaları kastediyoruz) öğrenmesi
sağlanır. Sonra öğrenilmiş bilgiler kullanılarak kestirimler yapılır. Örneğin elimizde *elma*, *armut* ve
*kayısı* resimleri olsun. Biz makineye resimleri ve onun hangi meyve olduğunu verip onu eğitiyorsak ve
makinede de elmanın, armutun ve kayısının nasıl olduğunu öğreniyorsa bu denetimli bir yöntemdir. Bu eğitim
sonucunda biz bir resmi verdiğimizde makine bize onun elma mı, armut mu, kayısı mı olduğunu söyleyecektir.
Bugün kullanılan modern transformer tabanlı doğal işleme modelleri denetimli modellerdir. Bunlar
eğitilmişlerdir ve eğitimin sonucunda istediklerimizi bize vermektedir.

Kullanılan yöntemde bir eğitim süreci yoksa doğrudan kestirim yapılıyorsa bu tür yöntemlere denetimsiz
(unsupervised) yöntemler denilmektedir. Yukarıda da belirttiğimiz denetimsiz yöntemler denildiğinde akla
kümeleme (clustering) gelmektedir. Denetimsiz yöntemlerde etiketlenmiş verilerle eğitim uygulanmamaktadır.
Bu yöntemlerde biz makineye verileri veririz. Makine de onların arasındaki benzerlik ve farklılıklara
bakarak bizim için sonuçlar üretir. Bazı durumlarda denetimsiz yöntemlerin mecburen kullanılması
gerekebilmektedir. Örneğin biz içeriğini bilmediğimiz resimlerin benzerliklerine göre gruplandırılmasını
isteyebiliriz. Verilerin etiketlenmesi bazen çok zor olabilmektedir. Bu tür durumlarda denetimsiz öğrenme
yöntemleri kullanılmaktadır.

Pekiştirmeli öğrenme (reinforcement learning) psikolojideki *edimsel koşullanma (operant conditioning)*
temel alınarak geliştirilmiş bir yöntem grubudur. Pekiştirmeli öğrenmeden amaç *kendi kendine öğrenmenin*
sağlanmasıdır. Pekiştirmeli öğrenmede *etmen (agent)* dış dünyada eylemlerde bulunur. Bu eylemler olumlu
sonuçlar doğurursa bunlara ödül verilerek bu eylemlerin yinelenmesi sağlanır. Pekiştirmeli öğrenme insan
öğrenmesine en yakın öğrenme modelleridir.

Naive Bayes Yöntemine Giriş
---------------------------

Klasik doğal dil işleme yöntemlerinden biri *naive Bayes* denilen yöntemdir. Burada *naive* (İngilizce
*nai:v* biçiminde okunuyor) sözcüğü yöntemin basitliğinden dolayı verilmiştir. Bayes ünlü olasılık
kuramcısı Thomas Bayes'in *Bayes Teoremi* diye bilinen teoremine atıf yapmaktadır. Naive Bayes yöntemi
yalnızca doğal dil işlemede değil makine öğrenmesinin diğer alanlarında da kullanılmaktadır. Doğal dil
işlemede *naive Bayes* yöntemi şu problemlerde kullanılmaktadır:

- Spam filtreleme: Yöntemin en klasik ve tarihsel olarak en başarılı uygulamasıdır. Bir e-postanın spam
  olup olmadığının belirlenmesi problemi

- Duygu analizi (sentiment analysis): Bir metnin olumlu/olumsuz/nötr olarak sınıflandırılmasında.

- Konu/kategori sınıflandırması: Haber metinlerinin spor, ekonomi, siyaset gibi sınıflara ayrılması

- Dil belirleme (language identification): Bir metnin hangi dilde yazıldığının saptanması

- Yazar belirleme (authorship attribution): Bir metnin hangi yazara ait olduğunun kestirilmesinde
  kullanılmıştır.

Olasılık ve Koşullu Olasılığa Giriş
-----------------------------------

Naive Bayes yönteminin anlaşılabilmesi için temel olasılık ve koşullu olasılık konularının gözden
geçirilmesi gerekir. Biz de önce olasılığın temel kavramlarını gözden geçireceğiz. Sonra Bayes kuralını ve
koşullu olasılık konusunu açıklayacağız. Sonra da naive Bayes yöntemini inceleyeceğiz.

Temel Olasılık Kavramları
-------------------------

Sonucu önceden kestirilemeyen deneylere *rassal deney (random experiment)*, bir rassal deney sonucunda
oluşabilecek tüm durumların kümesine de *örnek uzayı (sample space)* denilmektedir. Örneğin zarın atılması
deneyinde örnek uzayı şu elemanlardan oluşmaktadır:

.. code-block:: text

    S = {1, 2, 3, 4, 5, 6}

Örnek uzayının her alt kümesine *olay (event)* denilmektedir. Örneğin zar atılma deneyinde {3, 5}, {1},
{1, 2, 3} birer olaydır. Örnek uzayının tek elemanlı alt kümelerine yani tek eleman içeren olaylara ise
*basit olaylar (simple events)* denilmektedir. Zar atılmasındaki basit olaylar şunlardır:

.. code-block:: text

    {1}
    {2}
    {3}
    {4}
    {5}
    {6}

Bir olayın olma olasılığı P(E) ile gösterilir. Burada E örnek uzayın bir alt kümesidir. P(E) olasılığının
aksiyomatik olarak n(E) / n(S) değerine eşit olduğu kabul edilmektedir. Yani olaya ilişkin alt kümenin
eleman sayısının örnek uzayın eleman sayısına bölümüdür. Bunun neden böyle olduğunun bir ispatı yoktur. Bu
bir aksiyomdur. Yani sezgisel olarak kabul edilen bir durumdur.

Olasılığın herkes tarafından kabul edilen bir tanımını yapmak zordur. Göreli sıklık (relative frequency)
tanımı sezgisel olarak herkesin kabul edebileceği bir tanımdır. Örneğin bir paranın Yazı gelme olasılığının
0.5 olduğunu hepimiz biliriz. Ancak para 10 kez atıldığında 5 defa yazı 5 defa tura geleceğinin bir
garantisi yoktur. Para milyarlarca kez atıldığında yazı gelme sayısının deney sayısına oranlandığında oran
gitgide 0.5'e yakınsayacaktır. O halde olasılık aslında bir limit durumudur.

Koşullu Olasılık, Bayes Kuralı ve Naive Bayes Yönteminin Matematiksel Temeli
============================================================================

Koşullu Olasılığa Giriş
-----------------------

Bir olayın gerçekleştiği durumda diğer bir olayın gerçekleşme olasılığına *koşullu olasılık* denilmektedir.
Örneğin zarın atılmasında 6 gelme olasılığı 1/6'dır. Ama zarın çift gelmiş olduğu kabul edilirse artık 6
gelme olasılığı 1/3'tür. Çünkü bir olayın gerçekleşmiş olduğu durumda örnek uzayı daraltılmaktadır. Bu
durumda A olayı gerçekleştiğinde B olayının gerçekleşme olasılığı yani P(B|A) = n(A ∩ B) / n(A) biçimindedir.
Bunu şekilsel olarak şöyle gösterebiliriz:

.. figure:: _static/classicnaturallanguageprocessing/kosullu-olasilik-venn.png
   :alt: S örnek uzayı içinde birbiriyle kesişen A ve B olayları
   :align: center

   S örnek uzayı içinde birbiriyle kesişen A ve B olayları.

Burada A olmuşken B'nin olasılığı şekilden de görüldüğü gibi n(A ∩ B) / n(A) biçimindedir. Burada pay ve
paydayı örnek uzayın eleman sayısına bölelim:

.. code-block:: text

    P(B|A) = (n(A ∩ B) / n(S)) / (n(A) / n(S))

Eşitlik şu hale gelir:

.. code-block:: text

    P(B|A) = P(A ∩ B) / P(A)

İki olayın birlikte gerçekleşme olasılığı genellikle P(A ∩ B) biçiminde ya da P(A, B) biçiminde
gösterilmektedir. Biz kursumuzda P(A, B) gösterimini tercih edeceğiz. Kesişim işleminin değişme özelliği
olduğuna göre P(A, B) ile P(B, A) aynı anlama gelmektedir.

Yukarıdaki eşitlikte P(A, B) şu hale gelmektedir:

.. code-block:: text

    P(A, B) = P(B|A) * P(A)

Bayes Kuralının Türetilmesi
---------------------------

P(B|A) olasılığını şöyle ifade etmiştik:

.. code-block:: text

    P(B|A) = P(A, B) / P(A)

Şimdi P(A|B) olasılığını yazalım:

.. code-block:: text

    P(A|B) = P(B, A) / P(B)

Birinci eşitlikten P(A, B)'yi çekelim:

.. code-block:: text

    P(A, B) = P(B|A) * P(A)

Bunu ikinci eşitlikte yerine koyalım:

.. code-block:: text

    P(A|B) = (P(B|A) * P(A)) / P(B)

Genellikle Bayes kuralı denildiğinde yukarıdaki formül kastedilmektedir. Aynı şeyi tersten yaparsak
aşağıdaki eşitliği elde ederiz:

.. code-block:: text

    P(B|A) = P(A|B) * P(B) / P(A)

Bu eşitliklere *Bayes Kuralı* denilmektedir.

Bayes kuralının aslında ispatı yoktur. Açıklama sezgiseldir dolayısıyla Bayes kuralı bir teoremden ziyade
bir aksiyomdur.

Koşullu Olasılıkla İlgili Örnek Problemler
------------------------------------------

Olasılık konusunda koşullu olasılıklara örnek oluşturan tipik sorular *bir koşul altında bir olasılığın
verilmesi ve bunun tersinin sorulması* biçimindedir. Örneğin:

*Bir şirket çalışanları arasında üniversite mezunu olan 46 personelin 6'sının, üniversite mezunu olmayan 54
personelin 22'sinin sigara içtiği biliniyor. Buna göre şirketteki sigara odasında sigara içtiği görülen bir
çalışanın üniversite mezunu olma olasılığı nedir?*

Bu soruda bize verilenler şunlardır:

.. code-block:: text

    P(sigara içiyor|üniversite mezunu) = 6 / 46
    P(sigara içiyor|üniversite mezunu değil) = 22 / 54

Bizden istenen de şudur:

.. code-block:: text

    P(üniversite mezunu|sigara içiyor)

Bayes formülünde bunları yerlerine koyalım:

.. code-block:: text

    P(üniversite mezunu|sigara içiyor) = P(sigara içiyor|üniversite mezunu) * P(üniversite mezunu) / P(sigara içiyor)

    P(üniversite mezunu|sigara içiyor) = ((6 / 46) * (46 / 100)) / (28 / 100)

Koşullu olasılıkla ilgili diğer bir soru çeşidi de *ayrık bütüne tamamlayan olaylarla* ilgilidir. Soruyu
soran kişi ayrık bütüne tamamlayan olayları belirtir. Sonra bunların bazılarının içinde bulunduğu bir
olayın olasılığını sorar. Örneğin bütüne tamamlayan olaylar A, B, C, D, E olsun. Burada bir X olayının söz
konusu olduğunu düşünelim.

.. code-block:: text

    P(X) = P(X|A) + P(X|B) + P(X|C) + P(X|D) + P(X|E)

Burada koşullu olasılıkları da aşağıdaki gibi tersten açabiliriz:

.. code-block:: text

    P(X) = (A|X) * P(X) / P(A) + P(B|X) * P(X) / P(B) + P(C|X) * P(X) / P(C)
           + P(D|X) * P(X) / P(D) + P(E|X) * P(X) / P(E)

Zincir Kuralı (Chain Rule)
--------------------------

İki olayın birlikte gerçekleşme olasılığını yukarıda verdik. P(A, B) = P(B|A) * P(A) ya da P(A|B) * P(B)
idi. Peki birden fazla olayın birlikte gerçekleşmesini koşullu olasılıkla nasıl ifade edebiliriz? Örneğin
P(A, B, C) olasılığını koşullu olasılıkla ifade etmek isteyelim:

.. code-block:: text

    P(A, B, C) = P(A ∣ B, C) * P(B, C)

P(B, C) olasılığını da açalım:

.. code-block:: text

    P(B, C) = P(B ∣ C) * P(C)

Şimdi birleştirelim:

.. code-block:: text

    P(A, B, C) = P(A ∣ B, C) * P(B ∣ C) * P(C)

Örneğin P(A, B, C, D) olasılığı şöyledir:

.. code-block:: text

    P(A, B, C, D) = P(A | B, C, D) * P(B | C, D) * P(C | D) * P(D)

Şimdi de P(A, B | C) olasılığının eşdeğerini yazalım:

.. code-block:: text

    P(A, B | C) = P(A, B, C) / P(C)
    = P(A | B, C) * P(B | C) * P(C) / P(C)
    = P(A | B, C) * P(B | C)

P(A, B, C | D) olasılığının eşdeğerini yazalım:

.. code-block:: text

    P(A, B, C | D) = P(A, B, C, D) / P(D)
    = P(A | B, C, D) * P(B | C, D) * P(C | D) * P(D) / P(D)
    = P(A | B, C, D) * P(B | C, D) * P(C | D)

Bu kurala zincir kuralı denilmektedir. Kuralı şöyle genelleştirebiliriz:

.. code-block:: text

    P(x₁, x₂, ..., xₙ | Y) = ∏ⁿᵢ₌₁ P(xᵢ | xᵢ₊₁, ..., xₙ, Y)

İstatistiksel Bağımsızlık ve Çarpım Kuralı
------------------------------------------

Bir olayın gerçekleşme olasılığının diğer bir olayın gerçekleşme olasılığı ile hiçbir ilişkisi yoksa bu iki
olaya *istatistiksel bağımsız olaylar* denilmektedir. Örneğin bir oyun kartından bir kart çekilmesi ve
sonra bir zar atılması olaylarını düşünelim. Bu iki olay istatistiksel bakımdan bağımsızdır. Çünkü çekilen
kart atılan zarı etkilememektedir. Başka bir deyişle bir olay olmuşsa da olmamışsa da diğer olayın oluşunu
etkilemiyorsa bu iki olay istatistiksel bakımdan bağımsızdır. P(A|B) koşullu olasılığında B'nin olması ile
A'nın olmasının hiçbir ilgisi yoksa bu olasılık P(A) ile de gösterilebilir. Yani B olsa da olmasa da A'nın
olasılığı değişmiyorsa P(A|B) = P(A)'dır. Böylece

P(A, B) = P(A|B) * P(B) olduğuna göre A ve B istatistiksel olarak bağımsızsa bu formül şu hale gelmektedir:

.. code-block:: text

    P(A, B) = P(A) * P(B)

Buna olasılıkta *bağımsız olasılıkların çarpım kuralı* denilmektedir. Örneğin bir oyun destesinden çekilen
kartın kupa ası olma olasılığı ve sonra atılan zarın 6 gelme olasılığı 1/52 * 1/6'dır.

Ancak doğada pek çok olay istatistiksel olarak bağımsız değildir. Çünkü birbirine etki etmektedir. Örneğin
bir kişinin kalp hastası olması ile sigara içmesi istatistiksel bakımdan bağımsız değildir. Sigara içmek
kalp hastalıklarına yatkınlığı artırmaktadır. Çok ince düşünüldüğünde doğada ve özellikle sosyal bilimlerde
birbirinden tamamen bağımsız olaylardan bahsetmek çok zor hale gelmektedir. Konunun *kaos teorisi*
bağlamında felsefi açılımları da vardır.

İkiden fazla istatistiksel olarak bağımsız olayın birlikte gerçekleşmesi de yine çarpım kuralı ile ifade
edilebilir. Örneğin:

.. code-block:: text

    P(A, B, C, D, E) = P(A) * P(B) * P(C) * P(D) * P(E)

Yukarıda P(A, B, C | D) olasılığının eşdeğerini zincir kuralına göre şöyle yazmıştık:

.. code-block:: text

    P(A, B, C | D) = P(A | B, C, D) * P(B | C, D) * P(C | D)

Eğer A, B ve C olayları istatistiksel bakımdan bağımsızsa formül şu hale gelmektedir:

.. code-block:: text

    P(A, B, C | D) = P(A|D) * P(B|D) * P(C|D)

Bunun ispatını şöyle yapabiliriz. Zincir kuralını işletelim:

.. code-block:: text

    P(A, B, C | D) = P(A | B, C, D) * P(B | C, D) * P(C | D)

Burada A, B, C istatistiksel bağımsız olduğuna göre:

.. code-block:: text

    P(A | B, C, D) = P(A | D)       A'yı bilmek için B ve C'ye gerek yok
    P(B | C, D) = P(B | D)          B'yi bilmek için C'ye gerek yok

Yerine koyarsak:

.. code-block:: text

    P(A, B, C | D) = P(A | D) * P(B | D) * P(C | D)

İşte bu formül naive Bayes denilen yöntemin özünü oluşturmaktadır. Biz birtakım olayları istatistiksel
bakımdan bağımsız kabul ettiğimizde formüller oldukça sadeleşmektedir.

Naive Bayes Yönteminin Matematiksel Temeli
------------------------------------------

Elimizde sözcüklerden (genel olarak atomlardan) oluşan dokümanlar olsun. Bu dokümanlar da önceden sınıflara
ayrılmış olsun. Biz bu sınıfları Yk ile temsil edelim. x1, x2, ..., xn yazıdaki sözcükler (genel olarak
atomlar) olmak üzere P(Yk|x1, x2, x3, ..., xn) olasılığı (yani doküman sözcüklerin x1, x2, x3, ..., xn iken
bunun k sınıfına ilişkin olma olasılığı) koşullu olasılık kurallarına göre şöyledir:

.. code-block:: text

    P(Yk|x1, x2, x3, ..., xn) = P(x1, x2, x3, ..., xn|Yk) * P(Yk) / P(x1, x2, x3, ..., xn)

Bu durumda biz x1, x2, x3, ..., xn sözcüklerinin geçtiği dokümanın hangi sınıfa ilişkin olduğunu koşullu
olasılığa dayalı olarak belirleyebiliriz. Zira m tane Y için tek tek P(Yk|x1, x2, x3, ..., xn) olasılıkları
hesaplanıp bunların hangisi yüksekse Yk sınıfı kestirim için seçilebilir. P(Yk|x1, x2, x3, ..., xn) koşullu
olasılığına yeniden dikkat ediniz:

.. code-block:: text

    P(Yk|x1, x2, x3, ..., xn) = P(x1, x2, x3, ..., xn|Yk) * P(Yk) / P(x1, x2, x3, ..., xn)

Zincir kuralına göre P(x1, x2, x3, ..., xn|Yk) olasılığı aşağıdaki gibi açılabilir:

.. code-block:: text

    P(x1, x2, x3, ..., xn|Yk) = P(x1|x2, x3, x4 ... xn) * P(x2|x3, x4, x5... xn) * ... P(xn-1 | xn)

Burada *naive bir varsayımla* x1, x2, x3, ..., xn sözcüklerinin istatistiksel bakımdan bağımsız olduğunu
varsayarsak aşağıdaki durumu elde ederiz (bu varsayımda bulunmazsak buradan faydalı bir sonuç
çıkmamaktadır):

.. code-block:: text

    P(Yk|x1, x2, x3, ..., xn) = (P(x1|Yk) * P(x2|Yk) * P(x3|Yk) * ... * P(xn|Yk) * P(Yk))
                                 / (P(x1) * P(x2) * P(x3) * ... * P(xn))

Burada Yk demekle Y'nin ayrı ayrı m tane kategorisini belirtiyoruz. Bu olasılıkların en büyüğü elde
edileceğine göre ve bu olasılıkların hepsinde paydadaki ifade olduğuna göre bu karşılaştırmada onları
boşuna hesaplamayız:

.. code-block:: text

    argmax(k = 1, ..., m) = P(x1|Yk) * P(x2|Yk) * P(x3|Yk) * ... * P(xn|Yk) * P(Yk)

Elimizde etiketlenmiş (yani sınıflandırılmış) dokümanların olduğunu varsaydığımızda yukarıdaki eşitliğin
sağ tarafı artık hesaplanabilir bir hale gelmiştir. Biz onların içerisinde en büyüğüne ilişkin sınıfı
seçersek dokümanı olasılığı en yüksek sınıfa atamış oluruz. Bu durumu biraz daha anlaşılır bir biçimde
açıklayalım. * karakterlerinin sözcükleri temsil ettiğini düşünelim ve problemin duygu analizi (sentiment
analysis) olduğunu varsayalım. Duygu analizinde dokümanlar iki sınıfa ayrılmaktadır: Pozitif ve negatif:

.. code-block:: text

    * * * * * * * * * * *                       ---> pozitif
    * * * * * * * * * * * * * *                 ---> negatif
    * * * * *                                   ---> pozitif
    * * * * * * * * * * * * *                   ---> pozitif
    * *                                         ---> negatif
    ...                                         ---> ...

Şimdi biz aşağıdaki gibi bir dokümanın sınıfını kestirmeye çalışalım:

.. code-block:: text

    x1 x2 x3 x4 x5  ---> pozitif mi negatif mi?

Burada x1, x2, x3, x4, x5 birer sözcük olsun. Biz aslında burada şöyle bir mantıkla düşünürüz: *Sözcükler
x1, x2, x3, x4, x5 olduğuna göre pozitif olma olasılığı mı daha yüksek, negatif olma olasılığı mı daha
yüksek?* Bunu yukarıda formülüze ettik:

.. code-block:: text

    Pozitif karşılaştırma değeri = P(x1|pozitif) * P(x2|pozitif) * P(x3|pozitif) * ... * P(xn|pozitif) * P(pozitif)
    Negatif karşılaştırma değeri = P(x1|negatif) * P(x2|negatif) * P(x3|negatif) * ... * P(xn|negatif) * P(negatif)

Etiketlenmiş olan (yani sınıflandırılmış olan) dokümanlardan yukarıdaki formüldeki tüm terimler elde
edilebilmektedir. Örneğin etiket pozitifken x1 sözcüğünün geçme olasılığını hesaplamak kolaydır. Örneğin x1
sözcüğü *kötü* sözcüğü olsun. Pozitif etiketliler arasındaki sözcüklerden *kötü* sözcüğünün olasılığını
şöyle hesaplayabiliriz: Pozitif etiketli dokümanların tüm sözcüklerinin sayısını elde ederiz. Sonra bunlar
arasında kaç tane *kötü* sözcüğünün geçtiğine bakarız. n("kötü") / n(pozitif) değeri bize pozitif
etiketlilerdeki *kötü* sözcüğünün görülme olasılığını verir. P(pozitif) ve P(negatif) olasılığının hesabı
da oldukça kolaydır. Örneğin P(pozitif) olasılığı pozitif etiketli dokümanların sayısının toplam
dokümanların sayısına oranıdır.

Burada formülün sadeleşmesinin nedeni sözcüklerin istatistiksel bakımdan bağımsız olduğunun
varsayılmasıdır. Bu varsayım yapılmazsa etiketlenmiş dokümanlardan bu yönde bir fayda sağlanamamaktadır.
Peki gerçekten bir doküman içerisindeki sözcüklerin görülme olasılığının birbirleriyle hiçbir ilgisi yok
mudur? Hayır aslında bu varsayım açıkça hatalıdır. Örneğin bir dokümanda *çok* ile *kötü* sözcüklerinin bir
arada bulunma olasılığı *çok* ile *karpuz* sözcüklerinin bir arada bulunma olasılığından fazladır. Bu naif
varsayım bir hata içermektedir. Dolayısıyla kestirimin gücünü azaltmaktadır. Ancak ne olursa olsun belli
koşullar sağlandığında *naive Bayes* yöntemi iş görür durumdadır.

Sayısal Kararlılık: Logaritma Kullanımı ve Underflow
----------------------------------------------------

Burada bir nokta üzerinde durmak istiyoruz. Biz naive Bayes yöntemini aşağıdaki formülle ifade ettik:

.. code-block:: text

    argmax(k = 1, ..., m) = P(x1|Yk) * P(x2|Yk) * P(x3|Yk) * ... * P(xn|Yk) * P(Yk)

Burada çarpımın terimleri çok fazla olabilir ve söz konusu olasılıklar da çok küçük olabilir. Çok küçük
sayıları birbirleriyle çarptıkça değer iyice küçülmektedir. Bu tür durumlarda değerler o kadar küçülebilir
ki artık işlemcilerin floating point birimleri tarafından işlem yapılamaz hale gelebilir. Buna teknik olarak
*underflow* denilmektedir. Underflow oluşmasa bile sayı küçüldükçe yuvarlama hatalarına olan maruziyet
artabilmektedir. İşte bu nedenle bu çarpım işleminden kurtulmak gerekir. Artan özellikteki f fonksiyonları
için X > y ise f(X) > f(y)'dir. Logaritma fonksiyonu da taban 1'den büyük olmak koşuluyla artan bir
fonksiyondur. Aynı zamanda değerlerin çarpımlarının logaritmalarının logaritmalarının toplamına eşit
olduğunu anımsayınız. O halde biz yukarıdaki karşılaştırmayı şu hale getirebiliriz:

.. code-block:: text

    Pozitif karşılaştırma değeri = log P(x1|pozitif) + log P(x2|pozitif) + log P(x3|pozitif) + ...
                                    + log P(xn|pozitif) + log P(pozitif)
    Negatif karşılaştırma değeri = log P(x1|negatif) + log P(x2|negatif) + log P(x3|negatif) + ...
                                    + log P(xn|negatif) + log P(negatif)

Burada logaritmanın tabanının kaç olduğunun bir önemi yoktur. Bu tür herhangi bir tabanın kullanılabileceği
durumlarda e tabanı tercih edilmektedir.

Laplace Düzeltmesi (Smoothing)
-------------------------------------

Ancak hala bir sorun daha vardır. Eğer bir sözcük ilgili sınıfta hiç yoksa (diğer sınıflarda olabilir) bu
durumda tüm olasılık ya 0'a düşer ya da logaritma kullanılıyorsa 0'ın logaritması tanımsız olduğu için
exception oluşur. Örneğin *karpuz* sözcüğü negatif etiketli dokümanlarda geçiyor olabilir. Yani sözcük
hazinesinde bulunuyor olabilir. Ancak pozitif etiketli dokümanlarda hiç geçmemiş olabilir. Peki bu durumda
ne yapılmaktadır? İşte bu durumda genellikle *Laplace düzeltmesi* denilen yöntem kullanılmaktadır. Laplace
düzeltmesi tipik olarak şöyle uygulanmaktadır:

.. code-block:: text

                        count(x, C) + 1
    P(x | C) = ──────────────────────────────────────
               C sınıftaki toplam sözcük sayısı + |V|

Burada kesrin pay kısmı C sınıfındaki x sözcüklerinin sayısının bir fazlasından oluşmaktadır. Payda ise C
sınıfındaki toplam sözcük sayısı ile sözcük hazinesinin sayısının toplamından oluşmaktadır. Eğer Laplace
düzeltmesi yapılmasaydı P(x | C) olasılığı şöyle hesaplanacaktı:

.. code-block:: text

                        count(x, C)
    P(x | C) = ─────────────────────────────────────
                 C sınıftaki toplam sözcük sayısı

Ancak yukarıda da belirttiğimiz gibi sözcüğün ilgili sınıfta hiç olmaması kesri tanımsız hale getirmektedir.
Aslında Laplace düzeltmesinde kesrin pay kısmında 1 ile toplama zorunlu değildir. Buna alpha değeri de
denilmektedir. Laplace düzeltmesinin genel formülü aslında şöyledir:

.. code-block:: text

                      count(x, C) + α
    P(x | C) = ──────────────────────────────────────────
               C sınıftaki toplam sözcük sayısı + α * |V|

Genel olarak büyük bir sözcük hazinesi ancak seyrek dağılım durumunda küçük alpha değeri; küçük sözcük
hazinesi ve gürültülü durumunda daha büyük alpha değeri seçilebilir.

Biz burada duygu analizi problemini temel alarak Naive Bayes yöntemini açıkladık. Oysa Naive Bayes
yönteminde sınıfların iki ya da üç tane olması zorunlu değildir. Yani yukarıdaki formüller genel olarak
çok sayıda sınıfa sahip doküman sınıflandırmalarında kullanılabilmektedir. Biz yukarıdaki formülleri C bir
sınıf belirtmek üzere şöyle genelleştirebiliriz:

.. code-block:: text

    C karşılaştırma değeri = log P(x1|C) + log P(x2|C) + log P(x3|C) + ... + log P(xn|C) + log P(C)


Naive Bayes Doküman Sınıflandırmasının Python ile Gerçekleştirimi
=================================================================

Gerekli İstatistiksel Bilgiler
------------------------------

Şimdi naive Bayes doküman sınıflandırma işleminin Python gerçekleştirimini yapalım. Bunun için bizim eğitim
sırasında etiketlenmiş dokümanlardan şu bilgileri elde etmemiz gerekir:

- Belli bir sınıftaki tüm sözcüklerin sayısı. Örneğin duygu analizi için *pozitif* ve *negatif* sınıftaki
  tüm sözcüklerin sayısı.
- Her sözcüğün sınıflardaki toplam sayısı. Örneğin duygu analizinde sözcüklerin spesifik olarak *pozitif*
  ve *negatif* etiketli dokümanlarda kaçar kez geçtiğinin sayısı.

Yukarıdaki iki bilgi elde edildiği zaman artık x bir sözcük olmak üzere log P(x1|C) hesabı yapılabilir. Bu
hesap için x sözcüğünün C sınıfındaki yinelenme sayısı, C sınıfındaki tüm sözcüklerin sayısına bölünür.

- Her sınıftan toplam kaç dokümanın olduğu. Örneğin duygu analizi için kaç tane dokümanın *pozitif* kaç
  tane dokümanın *negatif* etiketli olduğunun sayısı. Bu sayede biz sınıfların olasılıklarını yani log
  P(C) değerini hesaplayabiliriz.

Sınıfın __init__ Metodu
-----------------------

Burada biz naive Bayes yöntemi ile sıfırdan Python ile doküman analizine bir örnek vereceğiz. Sınıfımızın
``__init__`` metodu şöyle olabilir:

.. code-block:: python

    class NaiveBayesClassifier:
        def __init__(self):
            self.class_priors = {}
            self.word_counts = {}
            self.class_totals = {}
            self.vocab = set()

        # ...

Buradaki sözlüklerin ne amaçla oluşturulduğunu açıklayalım. ``word_counts`` her sınıftaki sözcüklerin kaçar
kez geçtiğini tutmaktadır. Örneğin duygu analizinde *güzel* sözcüğü *pozitif* ve *negatif* etiketli
dokümanlarda kaç geçmiştir? Bu değer log P(x|C) hesabını yapmak için gerekmektedir. ``class_totals``
sözlüğü sınıflardaki toplam sözcük sayısını tutmaktadır. log P(x|C) hesabını yapmak için bu sözlüğün de
oluşturulması gerekmektedir. ``class_priors`` sözlüğü her sınıftaki dokümanın toplam doküman sayısına
oranını tutmaktadır. Bu sözlük log P(C) değerinin hesaplanması için gerekmektedir.

Her ne kadar ``__init__`` metodunda nesnenin ``word_counts`` ve ``class_totals`` özniteliklerini biz
``dict`` sınıfı ile oluşturduysak da aslında eğitim sırasında bu değişkenlere ``defaultdict`` nesneleri
atanmaktadır. Buradaki ilk değerler yalnızca nesnenin özniteliklerini tanıtmak için kullanılmıştır.
Bildiğiniz gibi Python'da özniteliklerin ``__init__`` metodunda oluşturulması gibi bir zorunluluk yoktur.

fit Metodu
----------

Biz de geleneğe uyarak eğitim işlemini sınıfımız için ``fit`` isimli metotla yapabiliriz:

.. code-block:: python

    def fit(self, documents, labels):
        self.class_priors = {}
        self.word_counts = defaultdict(lambda: defaultdict(int))
        self.class_totals = defaultdict(int)
        self.vocab = set()
        class_doc_counts = defaultdict(int)

        for doc, label in zip(documents, labels):
            class_doc_counts[label] += 1
            for word in self.tokenize(doc):
                self.word_counts[label][word] += 1
                self.class_totals[label] += 1
                self.vocab.add(word)

        total_docs = len(documents)
        for label in class_doc_counts:
            self.class_priors[label] = class_doc_counts[label] / total_docs

Dış döngü içerisindeki işleme dikkat ediniz:

.. code-block:: python

    class_doc_counts[label] += 1

Burada aslında her sınıfta kaç tane doküman olduğu hesabı yapılmaktadır. İç döngüde her dokümandaki
sözcükler (genel olarak atomlar) dolaşılmıştır:

.. code-block:: python

    for word in self.tokenize(doc):
        self.word_counts[label][word] += 1
        self.class_totals[label] += 1
        self.vocab.add(word)

Döngü içerisindeki ilk deyim her sözcüğün sınıflarda kaç kez yinelendiği bilgisini oluşturmaktadır:

.. code-block:: python

    self.word_counts[label][word] += 1

Döngü içerisindeki ikinci deyim ise sınıflardaki toplam sözcük sayısını oluşturmaktadır:

.. code-block:: python

    self.class_totals[label] += 1

Döngü içerisindeki üçüncü deyim de dokümanlardaki sözcük hazinesini oluşturmaktadır:

.. code-block:: python

    self.vocab.add(word)

Sözcük hazinesi Laplace dönüştürmesi için gerekmektedir.

``fit`` metodundaki döngünün dışındaki işlemlere dikkat ediniz:

.. code-block:: python

    total_docs = len(documents)
    for label in class_doc_counts:
        self.class_priors[label] = class_doc_counts[label] / total_docs

Burada her sınıftaki doküman sayısı toplam doküman sayısına bölünerek sınıf olasılıkları oluşturulmuştur.

predict Metodu
--------------

Şimdi de belli bir dokümanın sınıfını tespit eden ``predict`` metodunu yazalım:

.. code-block:: python

    def predict(self, text):
       words = self.tokenize(text)
       vocab_size = len(self.vocab)
       scores = {}

       for label in self.class_priors:
           log_score = math.log(self.class_priors[label])
           for word in words:
               count = self.word_counts[label][word]
               prob = (count + 1) / (self.class_totals[label] + vocab_size)
               log_score += math.log(prob)
           scores[label] = log_score

       return max(scores, key=scores.get)

Burada aslında daha önce oluşturduğumuz aşağıdaki hesap yapılmaktadır:

.. code-block:: text

    C karşılaştırma değeri = log P(x1|C) + log P(x2|C) + log P(x3|C) + ... + log P(xn|C) + log P(C)

Fonksiyonun başında önce metin sözcüklere ayrıştırılmıştır. Bu işlem sınıfın ``_tokenize`` metoduyla
yapılmaktadır:

.. code-block:: python

    words = self._tokenize(text)

``_tokenize`` metodu basit bir biçimde şöyle oluşturulmuştur:

.. code-block:: python

    def _tokenize(self, text):
        return re.findall(r'\w+', text.lower())

Biz burada karmaşıklık oluşmasın diye bu metodu sade bir biçimde tuttuk. Üretime yönelik uygulamalarda
burada daha önce ele aldığımız ön işlemlerin yapılmasını tavsiye ederiz. Fonksiyonun başında daha sonra
sözcük hazinesinin değer uzunluğu hesaplanmıştır:

.. code-block:: python

    vocab_size = len(self.vocab)

Karşılaştırma değerleri ``scores`` isimli bir sözlükte saklanmaktadır:

.. code-block:: python

    scores = {}

Sonra işlemler bir döngü içerisinde aşağıdaki gibi gerçekleştirilmiştir:

.. code-block:: python

    for label in self.class_priors:
        log_score = math.log(self.class_priors[label])
        for word in words:
            count = self.word_counts[label][word]
            prob = (count + 1) / (self.class_totals[label] + vocab_size)
            log_score += math.log(prob)
        scores[label] = log_score

Burada her sınıf için karşılaştırma değeri hesaplanıp ``scores`` sözlüğünde saklanmaktadır. Dış döngüdeki
``log_score`` kısmına dikkat ediniz:

.. code-block:: python

    log_score = math.log(self.class_priors[label])

Kodun burası formüldeki log P(C) hesabını yapmaktadır. Formülün log P(x1|C) + log P(x2|C) + log P(x3|C) +
... + log P(xn|C) kısmı ise iç döngüyle yapılmaktadır:

.. code-block:: python

    for word in words:
        count = self.word_counts[label][word]
        prob = (count + 1) / (self.class_totals[label] + vocab_size)
        log_score += math.log(prob)

Ancak burada Laplace düzeltmesinin de kullanıldığına dikkat ediniz.

Kodun tamamını aşağıda veriyoruz.

.. code-block:: python

    from collections import defaultdict
    import math
    import re

    class NaiveBayesDocClassifier:
        def __init__(self):
            self.class_priors = {}
            self.word_counts = {}
            self.class_totals = {}
            self.vocab = set()

        def fit(self, documents, labels):
            self.class_priors = {}
            self.word_counts = defaultdict(lambda: defaultdict(int))
            self.class_totals = defaultdict(int)
            self.vocab = set()
            class_doc_counts = defaultdict(int)

            for doc, label in zip(documents, labels):
                class_doc_counts[label] += 1
                for word in self._tokenize(doc):
                    self.word_counts[label][word] += 1
                    self.class_totals[label] += 1
                    self.vocab.add(word)

            total_docs = len(documents)
            for label in class_doc_counts:
                self.class_priors[label] = class_doc_counts[label] / total_docs

        def _tokenize(self, text):
            return re.findall(r'\w+', text.lower())

        def predict(self, text):
           words = self._tokenize(text)
           vocab_size = len(self.vocab)
           scores = {}

           for label in self.class_priors:
               log_score = math.log(self.class_priors[label])
               for word in words:
                   count = self.word_counts[label][word]
                   prob = (count + 1) / (self.class_totals[label] + vocab_size)
                   log_score += math.log(prob)
               scores[label] = log_score

           return max(scores, key=scores.get)


    # test

    import pandas as pd

    df = pd.read_csv('../Data/turkish_movie_sentiment_labeled.csv')
    comments = df['comment']
    labels = df['label']

    nbdc = NaiveBayesDocClassifier()
    nbdc.fit(comments, labels)

    predict_text = """
        Güncelleme: Spielberg bence yaşlanmış. Filmin temposunu yavaş buldum. Elimden gelse ileri sarardım.
        Emily Blunt dışında oyunculukları da beğenmedim. Sinema da para vermeye değmez:) Günü ( Disclosure Day )
        10 Haziran da vizyona girecek . Yönetmen Steven Spielberg olunca insanın doğal olarak ilgisini çekiyor.
        Fragmanından izlediğim kadar İfşa Günü uzaylılar , istila, savaştan çok, evrende yalnız olmadığımızı
        öğrendiğimiz zaman nasıl bir bireysel ve toplum olarak nasıl bir psikolojiye bürüneceğimizi konu alıyor
        gibi. Bilim kurgu filmlerini çok sevdiğim için ilk fırsatta izleyip, yorumu güncelleyeceğim.
    """

    predict_result = nbdc.predict(predict_text)
    print(predict_result)               # nötr

scikit-learn ile Naive Bayes: sklearn.naive_bayes Modülü
--------------------------------------------------------

Naive Bayes yöntemiyle sınıflandırma işlemi için scikit-learn kütüphanesinde ``sklearn.naive_bayes``
modülü içerisinde çeşitli sınıflar bulundurulmuştur:

.. list-table:: sklearn.naive_bayes Modülündeki Sınıflar
   :header-rows: 1
   :widths: 20 80

   * - Sınıf
     - Kullanım Durumu
   * - ``GaussianNB``
     - Sürekli (gerçek değerli) özellikler için kullanılır; her sınıf içinde özelliklerin normal dağıldığı
       varsayılır. Sözcük frekansları ayrık sayım verisi olduğundan doküman sınıflandırma için uygun
       değildir.
   * - ``MultinomialNB``
     - Ayrık sayım verileri (sözcük frekansları, n-gram sayıları, TF-IDF ağırlıkları) için kullanılır.
       Doküman sınıflandırma için en uygun ve en yaygın kullanılan sınıftır.
   * - ``ComplementNB``
     - ``MultinomialNB``'nin dengesiz sınıf dağılımlarına uyarlanmış biçimidir; parametreler her sınıfın
       tümleyeninden hesaplanır. Sınıf dağılımı dengesiz veri kümelerinde doküman sınıflandırma için
       uygundur.
   * - ``BernoulliNB``
     - İkili (binary) özellikler için kullanılır; sözcüğün kaç kez geçtiği değil, dokümanda bulunup
       bulunmadığı dikkate alınır. Özellikle çok kısa dokümanların sınıflandırılması için uygundur.
   * - ``CategoricalNB``
     - Kategorik (nominal) özellikler için kullanılır; her özellik sonlu bir kategori kümesinden değer
       alır. Metin temsilleriyle doğrudan çalışmadığından doküman sınıflandırma için tipik olarak
       kullanılmaz.

Tablodan da görüldüğü gibi doküman sınıflandırma için en uygun sınıf ``MultinomialNB`` sınıfıdır. Sınıfın
``__init__`` metodunun parametrik yapısı şöyledir:

.. code-block:: python

    class sklearn.naive_bayes.MultinomialNB(*, alpha=1.0, force_alpha=True, fit_prior=True, class_prior=None)

Buradaki ``alpha`` değeri Laplace düzeltmesindeki alpha değeridir. Diğer parametreler için scikit-learn
dokümanlarına başvurabilirsiniz. Eğitim işlemi diğer scikit-learn sınıflarında olduğu gibi ``fit`` metoduyla
yapılmaktadır. ``fit`` metodunun parametrik yapısı şöyledir:

.. code-block:: python

    fit(X, y, sample_weight=None)

Metot yazıları değil, sıklık sayılarını ve etiketleri parametre olarak almaktadır. Uygulamacının önce
yazıları ``CountVectorizer`` ya da ``TfidfVectorizer`` sınıfı ile sayısal hale getirmesi gerekir.

Kestirim işlemi yine scikit-learn içerisindeki ``predict`` metoduyla yapılmaktadır:

.. code-block:: python

    predict(X)

Metot sıklık sayılarının bulunduğu vektörü parametre olarak alıp kestirilen sınıf değeri ile geri
dönmektedir.

scikit-learn ile Naive Bayes Duygu Analizi, Pipeline ve Model Doğruluğu
=======================================================================

scikit-learn ile Naive Bayes Duygu Analizi
------------------------------------------

Şimdi yukarıda manuel yaptığımız naive Bayes duygu analizini bu kez scikit-learn kütüphanesi ile yapalım.
Önce veri kümesini yine okuyalım ve ayrıştıralım:

.. code-block:: python

    df = pd.read_csv('../Data/turkish_movie_sentiment_labeled.csv')
    comments = df['comment']
    labels = df['label']

Sonra her yorumu sözcük hazinesi kadar sütuna sahip, yorum sayısı kadar satıra sahip bir matrise
dönüştürelim:

.. code-block:: python

    def turkish_lowercase(text):
        return text.replace('İ', 'i').replace('I', 'ı').lower()

    cv = CountVectorizer(preprocessor=turkish_lowercase, lowercase=False)

``CountVectorizer`` sınıfının ``__init__`` metoduna girilen ``preprocessor`` parametresi yazıların vektör
haline dönüştürülmeden önce sokulacağı fonksiyonu belirtmektedir. Biz bu fonksiyonu yazıyı Türkçe küçük
harfe dönüştürecek biçimde oluşturduk. ``preprocessor`` fonksiyonunda önce metin normalizasyonu sonra da
gövdeleme gibi sözlüksel biçime dönüştürme gibi işlemleri de uygulamalıyız. Ancak biz burada örnek yalın
kalsın diye bunları yapmadık. Bundan sonra eğitim veri kümesi sayısal hale getirilebilir:

.. code-block:: python

    cv.fit(comments)
    comment_vectors = cv.transform(comments)

Anımsanacağı gibi scikit-learn vectorizer sınıfları çıktıyı seyrek matris biçiminde vermektedir.
scikit-learn içerisindeki pek çok sınıf (ama hepsi değil) seyrek matris girdilerini kabul etmektedir.
Bundan sonra ``MultinomialNB`` nesnemizi yaratıp ``fit`` işlemi yapabiliriz:

.. code-block:: python

    mnb = MultinomialNB()
    mnb.fit(comment_vectors, labels)

``fit`` metoduna verilen etiketlerin yazı biçiminde olduğuna dikkat ediniz. Genellikle uygulamalarda y
değerleri de yazı biçiminde değil, 0, 1, 2 gibi sayısal biçimde verilmektedir. Kategorik yazısal değerleri
sayısal değerlere dönüştürmek için scikit-learn içerisinde ``LabelEncoder`` ve ``OrdinalEncoder`` sınıfları
bulunmaktadır. Ancak biz örneğimizde y değerlerini doğrudan yazı olarak ``fit`` metoduna verdik.

Artık ``predict`` işlemi yapılabilir. ``MultinomialNB`` sınıfının ``predict`` metodu birden fazla vektörü
parametre olarak alıp tüm vektörler için kestirim yapmaktadır. Tabii yine önce bizim bu yazıları
``CountVectorizer`` ile vektörel hale getirmemiz gerekir:

.. code-block:: python

    predict_comments = ["""
        Güncelleme: Spielberg bence yaşlanmış. Filmin temposunu yavaş buldum. Elimden gelse ileri sarardım.
        Emily Blunt dışında oyunculukları da beğenmedim. Sinema da para vermeye değmez:) Günü ( Disclosure
        Day ) 10 Haziran da vizyona girecek. Yönetmen Steven Spielberg olunca insanın doğal olarak ilgisini
        çekiyor. Fragmanından izlediğim kadar İfşa Günü uzaylılar, istila, savaştan çok, evrende yalnız
        olmadığımızı öğrendiğimiz zaman nasıl bir bireysel ve toplum olarak nasıl bir psikolojiye
        bürüneceğimizi konu alıyor gibi. Bilim kurgu filmlerini çok sevdiğim için ilk fırsatta izleyip,
        yorumu güncelleyeceğim.
    """,
    """
        Bu film berbattı. Söylenecek başka bir şey yok
    """
    ]

    predict_vectors = cv.transform(predict_comments)
    predict_result = mnb.predict(predict_vectors)
    print(predict_result)

Programın kodlarını bütünsel olarak aşağıda veriyoruz:

.. code-block:: python

    import pandas as pd

    df = pd.read_csv('../Data/turkish_movie_sentiment_labeled.csv')
    comments = df['comment']
    labels = df['label']

    from sklearn.feature_extraction.text import CountVectorizer
    from sklearn.naive_bayes import MultinomialNB

    def turkish_lowercase(text):
        return text.replace('İ', 'i').replace('I', 'ı').lower()

    cv = CountVectorizer(preprocessor=turkish_lowercase, lowercase=False)
    cv.fit(comments)
    comment_vectors = cv.transform(comments)


    mnb = MultinomialNB()
    mnb.fit(comment_vectors, labels)

    predict_comments = ["""
        Güncelleme: Spielberg bence yaşlanmış. Filmin temposunu yavaş buldum. Elimden gelse ileri sarardım.
        Emily Blunt dışında oyunculukları da beğenmedim. Sinema da para vermeye değmez:) Günü ( Disclosure
        Day ) 10 Haziran da vizyona girecek. Yönetmen Steven Spielberg olunca insanın doğal olarak ilgisini
        çekiyor. Fragmanından izlediğim kadar İfşa Günü uzaylılar, istila, savaştan çok, evrende yalnız
        olmadığımızı öğrendiğimiz zaman nasıl bir bireysel ve toplum olarak nasıl bir psikolojiye
        bürüneceğimizi konu alıyor gibi. Bilim kurgu filmlerini çok sevdiğim için ilk fırsatta izleyip,
        yorumu güncelleyeceğim.
        """,
        """
            Bu film berbattı. Söylenecek başka bir şey yok
        """
    ]

    predict_vectors = cv.transform(predict_comments)
    predict_result = mnb.predict(predict_vectors)
    print(predict_result)

Pipeline (Boru Hattı) Mekanizması ile Naive Bayes
-------------------------------------------------

Kursumuzun giriş bölümlerinde makine öğrenmesine ilişkin kütüphanelerdeki *boru hattı (pipeline)*
mekanizmasına değinmiştik. Boru hattı mekanizması bir grup nesneyi alarak bizim sanki o nesneler tek bir
nesneymiş gibi işlem yapmamızı sağlamaktadır. Boru hattındaki nesneler işletilirken önceki öğeye ilişkin
çıktı sonraki öğeye girdi olarak verilmektedir. Biz giriş bölümlerinde scikit-learn kütüphanesinde de
``Pipeline`` isimli bir sınıfın bulunduğunu belirtmiştik. Yukarıdaki işlemi sanki tek bir öğeymiş gibi
``Pipeline`` sınıfını kullanarak da yapabiliriz:

.. code-block:: python

    pl = Pipeline(
    [
        ('CountVectorizer', CountVectorizer(preprocessor=turkish_lowercase, lowercase=False)),
        ('MultinomialNB', MultinomialNB())
    ])

Buradaki ``Pipeline`` nesnesi ile ``fit`` işlemi yapıldığında sırasıyla her öğeye ``fit`` uygulanmaktadır.
Ancak yukarıda da belirttiğimiz gibi önceki öğenin çıktısı sonrakine girdi yapılmaktadır. Örneğin:

.. code-block:: python

    pl.fit(comments, labels)

Burada önce ``CountVectorizer`` nesnesi ile ``fit`` yapılır. Bu metodun geri dönüş değeri ile
``MultinomialNB`` nesnesine ``fit`` uygulanmaktadır. Burada siz *biz CountVectorizer sınıfının fit
metodunu tek parametreyle, MultinomialNB sınıfının fit metodunu iki parametreyle çağırıyoruz. Bu durumda
bu argüman her metoda aktarıldığına göre sorun çıkmayacak mı?* diye düşünebilirsiniz. İşte boru hattı
mekanizması için ``CountVectorizer`` gibi sınıfların ``fit`` metotlarına kullanılmayan dummy bir ikinci
parametre eklenmiştir. ``CountVectorizer`` sınıfının ``fit`` metodu şöyledir:

.. code-block:: python

    fit(raw_documents, y=None)

scikit-learn dokümanları buradaki y parametresinin metot tarafından kullanılmadığını söylemektedir.

``Pipeline`` nesnesi ile ``predict`` işlemi yapıldığında ``Pipeline`` sınıfının ``predict`` metodu boru
hattındaki son nesne hariç tüm nesneler için ``transform`` metotlarını çağırır, son nesne için ise
``predict`` metodunu çağırır:

.. code-block:: python

    predict_result = pl.predict(predict_comments)

Aşağıda programın ``Pipeline`` ile oluşturulmuş halini de veriyoruz.

.. code-block:: python

    import pandas as pd

    df = pd.read_csv('../Data/turkish_movie_sentiment_labeled.csv')
    comments = df['comment']
    labels = df['label']

    from sklearn.feature_extraction.text import CountVectorizer
    from sklearn.naive_bayes import MultinomialNB
    from sklearn.pipeline import Pipeline

    def turkish_lowercase(text):
        return text.replace('İ', 'i').replace('I', 'ı').lower()

    pl = Pipeline(
        [
            ('CountVectorizer', CountVectorizer(preprocessor=turkish_lowercase, lowercase=False)),
            ('MultinomialNB', MultinomialNB())
        ])

    pl.fit(comments, labels)

    predict_comments = ["""
        Güncelleme: Spielberg bence yaşlanmış. Filmin temposunu yavaş buldum. Elimden gelse ileri sarardım. Emily
         Blunt dışında oyunculukları da beğenmedim. Sinema da para vermeye değmez:) Günü ( Disclosure Day ) 10
         Haziran da vizyona girecek . Yönetmen Steven Spielberg olunca insanın doğal olarak ilgisini çekiyor.
         Fragmanından izlediğim kadar İfşa Günü uzaylılar , istila, savaştan çok, evrende yalnız olmadığımızı
         öğrendiğimiz zaman nasıl bir bireysel ve toplum olarak nasıl bir psikolojiye bürüneceğimizi konu alıyor
         gibi. Bilim kurgu filmlerini çok sevdiğim için ilk fırsatta izleyip, yorumu güncelleyeceğim.
        """,
        """
            Bu film berbattı. Söylenecek başka bir şey yok
        """
    ]

    predict_result = pl.predict(predict_comments)
    print(predict_result)

Model Doğruluğunun (Accuracy) Test Edilmesi
-------------------------------------------

Peki doküman sınıflandırma gibi bir uygulamadan elde edilen sonuçların geçerliliği hakkında ne
söyleyebiliriz? İşte makine öğrenmesinde oluşturulan modelin ne kadar iyi sonuç verdiği metrik değerlerle
ölçülebilmektedir. Sınıflandırma problemlerinde geçerlilik sınaması için en çok *isabet (accuracy)*
denilen metrik kullanılmaktadır. ``accuracy`` modelin etiketlenmiş test verilerinde yüzde kaç isabet
gösterdiğini belirtmektedir. Örneğin naive Bayes uygulamamızda modeli eğittikten sonra, eğitilmiş modeli
kullanarak insanlar tarafından etiketlenmiş olan 1000 tane yazının kestirimini yapmış olalım. Eğer bu
kestirimlerde örneğin 915 tanesi isabetli ise modelimizin ``accuracy`` metriği 0.915'tir. Bu değer
modelimize ne kadar güvenebileceğimiz konusunda bir fikir vermektedir. Makine öğrenmesinde bu sürece
genellikle *modelin test edilmesi* ya da *modelin geçerliliğinin sınanması* denilmektedir.

Model test edilirken kullanılan veri kümesinin eğitimde kullanılan veri kümesinden tamamen farklı olması
gerekir. (Örneğin bir sınava alıştırma yapmak için çözülen soruların aynısı sınavda çıkarsa bu iyi bir
ölçüm olmaz.) Bu durumda uygulamacı tipik olarak n tane elemandan oluşan veri kümesini *eğitim* ve *test*
biçiminde ikiye ayırır. Modeli eğitim veri kümesiyle eğitir, test işlemini de test veri kümesi ile yapar. n
tane verinin yüzde kaçının eğitimde yüzde kaçının testte kullanılması gerektiği konusunda kesin bir şey
söylemek uygun olmaz. Tipik uygulamalarda koşullara da bağlı olarak eğitim ve test veri kümeleri için
%80-%20, %90-%10, %95-%5 gibi oransal değerler kullanılmaktadır.

Veri kümesini eğitim ve test olmak üzere ikiye ayırırken dikkat etmelisiniz. Bazı veri kümeleri dosyalarda
etiketlere göre sırayı dizilmiş bir biçimde olabilmektedir. Bazı veri kümeleri bazı sütunlara göre de
sıraya dizilmiş olabilmektedir. Böyle veri kümelerini bir yüzdeyle doğrudan bölmek yanlılığa yol
açabilmektedir. Bu nedenle önce verileri karıştırıp ondan sonra onu belirlediğiniz oranlarda ikiye
ayırmalısınız.

Aşağıda naive Bayes sınıfımıza %80-%20 oranları ile ``accuracy`` testi uygulanmıştır. Bu testlerden %70
civarında bir ``accuracy`` elde edilmiştir.

.. code-block:: python

    from collections import defaultdict
    import math
    import re

    class NaiveBayesDocClassifier:
        def __init__(self):
            self.class_priors = {}
            self.word_counts = {}
            self.class_totals = {}
            self.vocab = set()

        def fit(self, documents, labels):
            self.class_priors = {}
            self.word_counts = defaultdict(lambda: defaultdict(int))
            self.class_totals = defaultdict(int)
            self.vocab = set()
            class_doc_counts = defaultdict(int)

            for doc, label in zip(documents, labels):
                class_doc_counts[label] += 1
                for word in self._tokenize(doc):
                    self.word_counts[label][word] += 1
                    self.class_totals[label] += 1
                    self.vocab.add(word)

            total_docs = len(documents)
            for label in class_doc_counts:
                self.class_priors[label] = class_doc_counts[label] / total_docs

        def _tokenize(self, text):
            return re.findall(r'\w+', text.lower())

        def predict(self, text):
           words = self._tokenize(text)
           vocab_size = len(self.vocab)
           scores = {}

           for label in self.class_priors:
               log_score = math.log(self.class_priors[label])
               for word in words:
                   count = self.word_counts[label][word]
                   prob = (count + 1) / (self.class_totals[label] + vocab_size)
                   log_score += math.log(prob)
               scores[label] = log_score

           return max(scores, key=scores.get)

    TRAIN_SPLIT = 0.80

    import pandas as pd

    df = pd.read_csv('../Data/turkish_movie_sentiment_labeled.csv')

    shuffled_df = df.sample(frac=1)
    split_row = round(len(df) * TRAIN_SPLIT)

    training_df = shuffled_df[:split_row]
    test_df = shuffled_df[split_row:]

    nbdc = NaiveBayesDocClassifier()
    nbdc.fit(training_df['comment'], training_df['label'])

    predicted_result = [nbdc.predict(row['comment']) for row in test_df.iloc] == test_df['label']
    accuracy = predicted_result.sum() / len(test_df)

    print(accuracy)

