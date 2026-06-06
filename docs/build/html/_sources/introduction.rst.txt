======
Giriş
======

Bu bölümde temel kavramlar açıklanmaktadır.  Derneğimizin açtığı bu kursla ilişkili kurslar şunlardır:

- **Python Programlama Dili Kursu:** Bu kurs Python Programlama Dilini kapsamlı bir biçimde öğretmeyi
  hedeflemektedir. *Python Programlama Dili* kursumuzun Dropbox bağlantısı şöyledir:

  https://www.dropbox.com/sh/hez3g36x6xa97cu/AAAJzxu2Yrza9cCcO1DJWrWia?dl=0

- **Python Uygulamaları Kursu:** Bu kurs Python Programlama Dilini bilenler için bir uygulama kursudur.
  Programlama dilinin dışındaki pek çok kütüphane ve framework'ün kullanımı bu kursun konusu içerisindedir.
  NumPy, Pandas, Matplotlib gibi yapay zeka ve makine öğrenmesinde yoğun kullanılan kütüphaneler resmi
  olarak bu kursta ele alınmaktadır. *Python Uygulamaları* kursumuzun Dropbox bağlantısı şöyledir:

  https://www.dropbox.com/sh/vylimm3evek0nnl/AABS_KdWdRMO6Xh0Fh6HT3rFa?dl=0

- **Yapay Zeka, Makine Öğrenmesi ve Veri Bilimi Kursu:** Bu kurs genel bir yapay zeka, makine öğrenmesi
  ve veri bilimi kursudur. Kurs içerisinde sinir ağları, çeşitli istatistiksel öğrenme yöntemleri ve veri
  bilimine ilişkin pek çok konu ele alınmaktadır. *Yapay Zeka, Makine Öğrenmesi ve Veri Bilimi* kursumuzun
  Dropbox bağlantısı şöyledir:

  https://www.dropbox.com/sh/xvnprjjs7w74x05/AADi9NaU4aiHYHYREwKZgIzQa?dl=0


Uygulama Dili Olarak Python
===========================

Kitabımızda programlama dili olarak Python kullanılacaktır. Python "genel amaçlı, yüksek seviyeli,
matematiksel alana yakın ve nispeten basit" bir programlama dilidir. Python "yapay zeka, makine öğrenmesi,
veri bilimi ve doğal dil işlemede" halen en çok kullanılan programlama dilidir. Python son 10 senedir bir
atak yapmış ve dünyanın en yaygın kullanılan programlama dili haline gelmiştir. Python'un neden bu kadar
popüler hale geldiği ve neden yapay zeka, makine öğrenmesi ve veri bilimi alanlarında yaygın biçimde
kullanıldığı konusundaki tespitlerimiz şöyledir:

- Python nispeten basit bir dildir. Python'un basitliği başka alanlardan gelip de ana uğraşı alanı
  programlama olmayan kişiler için uygun bir seçenek oluşturmaktadır.

- Python matematiksel ve veri işleme alanları için uygun bir tasarıma sahiptir.

- Python çeşitli yüksek seviyeli veri yapılarını bünyesinde barındırmaktadır. Python'da az tuşa basılarak
  çok şey yapmak mümkündür.

- Python'da yukarıda belirttiğimiz alanlara yönelik pek çok hazır kütüphane ve framework bulunmaktadır.
  Bu alanlardaki algoritmaları tasarlayanlar onları birincil olarak Python'da gerçekleştirmektedir.

- Python programlama dili olarak özellikle 3'lü versiyonlarla birlikte oldukça iyileştirilmiştir.

- ABD'de MIT gibi üst düzey üniversiteler belli bir süredir Python dilini *Programlamaya Giriş
  (Introduction to Programming)* gibi derslerde kullanmaya başlamıştır. Bu da dilin prestijini artırmıştır.

- Python'un geniş bir standart kütüphanesi vardır. Bu duruma Python dünyasında esprili olarak *batteries
  included* denilmektedir. Python Standart Kütüphanesi içerisinde çok çeşitli konulara yönelik büyük ölçüde
  platform bağımsız olan hazır fonksiyonlar ve sınıflar bulunmaktadır. Bu da programlamayı oldukça
  kolaylaştırmaktadır. Her ne kadar Python Standart Kütüphanesi içerisinde olmasa da NumPy gibi, Pandas
  gibi, Matplotlib gibi yardımcı kütüphaneler Python'da veri analizine ilişkin işlemleri oldukça
  kolaylaştırmaktadır.

- Python bir prototip dil olarak da kullanılmaktadır. Bir algoritma ya da uygulama *acaba oluyor mu diye*
  hızlı bir biçimde Python'da oluşturulabilmektedir. Bazı uygulamacılar ve firmalar bu prototipleri daha
  sonra C/C++, Rust, Java, C# gibi dillerde ürüne dönüştürebilmektedir.

Python Gerçekleştirimleri ve Dağıtımları
----------------------------------------

Python temel olarak yorumlayıcı tabanlı (interpretive) bir programlama dilidir. Python kodlarını çalıştıran
programlara Python dünyasında genel olarak "Python gerçekleştirimleri (Python Implementations)"
denilmektedir. Python'un sürdürümünden sorumlu olan "Python Software Foundation" kurumunun ana Python
gerçekleştirimine (buna İngilizce *reference implementation* da denilmektedir) "CPython" denilmektedir.
(CPython ismi bu yorumlayıcının C dilinde yazılmış olduğundan dolayı verilmiştir.) Python
gerçekleştirimlerinin yanı sıra birtakım araçları da bünyesinde barındıran çeşitli Python dağıtımları
oluşturulmuştur. Yapay zeka, makine öğrenmesi ve veri bilimi alanlarında en çok Anaconda isimli dağıtım
kullanılmaktadır. Biz de kursumuzda bu dağıtımı kullanacağız. Anaconda dağıtımını aşağıdaki bağlantıdan
indirebilirsiniz:

https://www.anaconda.com/download/success

Anaconda dağıtımının temel GUI arayüzü *Anaconda Navigator* denilen programdır. Anaconda Navigator
dağıtım içerisindeki çeşitli araçları bünyesinde barındıran ve bazı işlemlerin yapılmasını kolaylaştıran
yönetici bir program gibidir. Anaconda dağıtımı birincil olarak *Spyder* isimli IDE'yi kullanmaktadır.
Spyder IDE'si aslında bağımsız bir projedir. Yani bu IDE'yi Anaconda olmadan da bilgisayarınıza yükleyerek
kullanabilirsiniz. Anaconda dağıtımı kurulduğu zaman Python Standart Kütüphanesinin dışında pek çok
yardımcı kütüphane de kurulmuş olmaktadır. Yani NumPy, Pandas, Matplotlib gibi temel kütüphaneleri ayrıca
kurmaya gerek kalmamaktadır.

Python diğer bazı yüksek seviyeli diller gibi komut satırı çalışmasına da izin vermektedir. Bu tür komut
satırlı çalışmalara son zamanlarda "REPL (Read and Evaluate and Print Loop)" da denilmektedir. Bu çalışma
biçiminde bir prompt çıkar. Bu prompt'ta kullanıcı bir Python deyimi yazarak ENTER tuşuna basar. Python
yorumlayıcısı da o deyimi o anda çalıştırır ve yeniden prompt'a düşer.

CPython dağıtımında komut satırında doğrudan *python* programı çalıştırılır; ancak bir program dosyası
komut satırı argümanı olarak verilmezse komut satırına düşülmektedir. *python* programı çalıştırılırken
yanına komut satırı argümanı olarak bir kaynak dosya ismi verilirse Python yorumlayıcısı komut satırına
düşülmeden o dosya çalıştırılmaktadır. Aslında Python'da deyimleri tek tek çalıştıran başka REPL
uygulamaları da vardır. Bunların en yaygın kullanılanlarından biri *IPython* denilen uygulamadır. (Spyder
IDE'si de sağ tarafta IPython konsolunu kullanmaktadır.) IPython da aslında bağımsız olarak kurulabilen
ayrı bir uygulamadır. IPython'daki bu komut satırı süreçlerine *kernel* da denilmektedir. Spyder'daki sağ
bölmede birden fazla IPython konsolu açılabilmektedir.

Jupyter Notebook
----------------

Yapay zeka, makine öğrenmesi ve veri bilimi eğitiminde çok kullanılan *Jupyter Notebook* denilen bir araç
da vardır. Jupyter Notebook açıklama yazılarıyla Python kodlarını bir arada farklı hücrelerde
tutabilmektedir. Pek çok bulut sistemi Jupyter Notebook hizmeti de vermektedir. Ancak biz kursumuzda
çalışma hızını yavaşlattığı gerekçesiyle Jupyter Notebook kullanmayacağız. Anaconda Navigator içerisinde
Jupyter Notebook da bulunmaktadır. Jupyter Notebook artık pek çok IDE'ye de entegre edilmiştir. Örneğin
VSCode IDE'sinde bir eklenti olarak da yüklenebilmektedir.

Yaygın Kullanılan Üçüncü Parti Kütüphaneler
===========================================

Yapay zeka, makine öğrenmesi ve veri biliminde Python'un standart kütüphanesinden ziyade birtakım üçüncü
parti kütüphaneler çok daha yoğun bir biçimde kullanılmaktadır. Bu bölümde bu kütüphaneler hakkında bazı
temel bilgiler vereceğiz.

NumPy
-----

NumPy, Python'da vektörel işlemlerin yapılabilmesine olanak sağlayan en temel kütüphanelerden biridir.
NumPy sayesinde örneğin bir dizideki elemanların hepsi tek hamlede işlemlere sokulabilmekte, NumPy dizisi
çarpıldığında dizinin karşılıklı elemanları çarpılabilmektedir. Ya da örneğin bir NumPy dizisinin sinüsü
alındığında dizinin tüm elemanlarının sinüsü elde edilebilmektedir. Bu tür özelliklere sahip olan dillere
*dizisel diller (array languages)* de denilmektedir. Matlab gibi R gibi matematiksel alana yakın olan
dillerin bu biçimde vektörel işlemler yapabilme yeteneği vardır. İşte NumPy kütüphanesi Python'a bu
yeteneği kazandırmaktadır.

NumPy kütüphanesi C'de yazılmıştır. Dolayısıyla aslında bu kütüphane kullanılarak işlemler yapılırken
arka planda C'de yazılmış olan kodlar çalıştırılmaktadır. Bu durum NumPy işlemlerinin Python'un doğal
çalışmasına göre daha hızlı yapılabildiği anlamına gelmektedir.

Pandas
------

Pandas kütüphanesi NumPy kullanılarak yazılmıştır. Pandas istatistiksel veri tablolarını oluşturabilme
olanağını sunmaktadır. İstatistikte, veri biliminde ve makine öğrenmesinde her sütunu farklı türden
olabilen veri tablolarıyla çok sık karşılaşılmaktadır. Bu tür veri tablolarında satırlarda varlıklar,
sütunlarda ise onların özellikleri bulunur. Örneğin:

.. list-table:: **Örnek Veri Tablosu**
   :header-rows: 1
   :widths: 10 10 15 20

   * - Boy
     - Kilo
     - Cinsiyet
     - Tercih Edilen Renk
   * - 170
     - 68
     - Erkek
     - Mavi
   * - 165
     - 55
     - Kadın
     - Yeşil
   * - 180
     - 82
     - Erkek
     - Siyah
   * - 158
     - 52
     - Kadın
     - Mor
   * - 175
     - 74
     - Erkek
     - Gri
   * - 162
     - 58
     - Kadın
     - Kırmızı
   * - 185
     - 90
     - Erkek
     - Lacivert
   * - 168
     - 60
     - Kadın
     - Turuncu
   * - 172
     - 70
     - Erkek
     - Beyaz
   * - 160
     - 54
     - Kadın
     - Pembe

Burada sütunlar farklı türlerdendir. İşte NumPy ile böyle bir temsil oluşturulamamaktadır. Çünkü
NumPy'daki iki boyutlu dizilerin her elemanı aynı türden olmak zorundadır. İşte Pandas bizim yukarıdaki
gibi veri tablolarını oluşturmamıza olanak sağlamaktadır. Pandas'taki bu biçimde veri tablosu temsilinin
yapılmasını sağlayan sınıfa ``DataFrame`` denilmektedir. Pandas denilince akla ``DataFrame`` sınıfı gelir.
``DataFrame`` nesnelerinin sütunlarına da Pandas'ta *Series* denilmektedir. Yukarıdaki veri tablosu Pandas
kütüphanesi ile bir ``DataFrame`` olarak şöyle oluşturulabilmektedir:

.. code-block:: python

   import pandas as pd

   data = {
       "Boy": [170, 165, 180, 158, 175, 162, 185, 168, 172, 160],
       "Kilo": [68, 55, 82, 52, 74, 58, 90, 60, 70, 54],
       "Cinsiyet": [
           "Erkek", "Kadın", "Erkek", "Kadın", "Erkek",
           "Kadın", "Erkek", "Kadın", "Erkek", "Kadın"
       ],
       "Tercih Edilen Renk": [
           "Mavi", "Yeşil", "Siyah", "Mor", "Gri",
           "Kırmızı", "Lacivert", "Turuncu", "Beyaz", "Pembe"
       ]
   }
   df = pd.DataFrame(data)
   print(df)

Programın çıktısı şöyle olacaktır:

.. code-block:: text
 
      Boy  Kilo Cinsiyet Tercih Edilen Renk
   0  170    68    Erkek               Mavi
   1  165    55    Kadın              Yeşil
   2  180    82    Erkek              Siyah
   3  158    52    Kadın                Mor
   4  175    74    Erkek                Gri
   5  162    58    Kadın            Kırmızı
   6  185    90    Erkek           Lacivert
   7  168    60    Kadın            Turuncu
   8  172    70    Erkek              Beyaz
   9  160    54    Kadın              Pembe


Matplotlib
----------

Yapay zeka, makine öğrenmesi ve veri bilimi alanında görselleştirme için en yaygın kullanılan kütüphane
*Matplotlib* denilen kütüphanedir. Bu kütüphanenin yanı sıra *Seaborn* ve *Plotly* kütüphaneleri de
yaygın kullanılmaktadır. Bu alanlardaki uygulamalarda bir biçimde görselleştirmenin yapılmasına gereksinim
duyulmaktadır. Kursumuzda ağırlıklı olarak bu tür görselleştirmeler için Matplotlib kütüphanesini
kullanacağız.

scikit-learn
------------

Makine öğrenmesinde en çok kullanılan kütüphanelerden biri de *scikit-learn* denilen kütüphanedir. Bu
kütüphane makine öğrenmesinin istatistiksel, olasılıksal ve matematiksel yöntemlerine ilişkin işlemler
yapan sınıfları ve fonksiyonları barındırmaktadır. Bugün en yaygın kullanılan makine öğrenmesi teknikleri
yapay sinir ağlarını kullanmaktadır. scikit-learn ise yapay sinir ağlarına ilişkin yöntemleri
içermemektedir. (scikit-learn içerisinde ilkel düzeyde yapay sinir ağlarına yönelik birkaç sınıf
bulunuyorsa da bunlar son derece yüzeyseldir.)

scikit-learn kütüphanesi Python'da yazılmıştır ve NumPy kütüphanesini taban kütüphane olarak
kullanmaktadır. Bu kütüphanenin diğer programlama dillerinden kullanılması doğrudan mümkün değildir.
Ancak dolaylı biçimlerde —verimli olmasa da— kullanım sağlanabilmektedir. Eskiden TensorFlow gibi, PyTorch
gibi yapay sinir ağlarına ilişkin kütüphaneler (bunlara *framework* de diyebiliriz) bazı temel işlemleri
bünyesinde barındırmıyordu. Bu nedenle scikit-learn kullanılması neredeyse zorunlu hale geliyordu. Ancak
daha sonraları TensorFlow gibi PyTorch gibi yapay sinir ağı framework'leri de scikit-learn tarafından
sunulan bazı işlemleri bünyesine katmıştır. Biz kursumuzda özellikle klasik doğal dil işleme
yöntemlerinde scikit-learn kütüphanesini kullanacağız. scikit-learn Anaconda dağıtımında varsayılan olarak
yüklü bulunmaktadır. Ancak diğer ortamlar için bu kütüphaneyi şöyle kurabilirsiniz:

.. code-block:: console

   pip install scikit-learn

scikit-learn Kütüphanesinde fit/transform/predict Kalıbı
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

scikit-learn kütüphanesinde işlemler belli bir kalıba uygun bir biçimde yürütülmektedir. Buna
*fit/transform/predict kalıbı* diyebiliriz. Kütüphane büyük ölçüde nesne yönelimli biçimde
tasarlanmıştır. fit/transform/predict kalıbı şöyle kullanılmaktadır:

**1) Nesne Yaratma**

Önce ilgili sınıf türünden bir nesne yaratılır. Yaratım sırasında ilgili konuya ilişkin çeşitli argümanlar
da girilmektedir. Örneğin:

.. code-block:: python

   cv = CountVectorizer(...)

**2) Eğitim (fit)**

İlgili sınıf türünden nesne yaratıldıktan sonra eğitimin yapılması gerekir. Burada "eğitim (training)"
demekle kullanılacak algoritmanın işletilmesi ve kestirim ya da sonuç için gereken bilgilerin elde edilerek
nesnenin özniteliklerinde (attributes) saklanması kastedilmektedir. Örneğin doğrusal regresyon uygulamak
için regresyon doğrusuna ilişkin (genel olarak *hyper-plane* denilmektedir) katsayıların elde edilmesi
gerekir. İşte ``LinearRegression`` sınıfının ``fit`` metodu bu katsayıları elde edip nesnenin özniteliklerinde
saklamaktadır. ``fit`` metoduna tipik olarak bir NumPy dizisi verilir. Ancak bu metotlar Python listeleriyle
ve Pandas'ın ``Series`` nesneleriyle de çalışabilmektedir. Örneğin:

.. code-block:: python

   cv.fit(dataset)

*fit* işlemiyle eğitim sonucunda elde edilen bilgiler nesnenin özniteliklerinde (attributes)
saklanmaktadır.

**3) Dönüştürme (transform)**

Artık sıra sonucu elde etmeye gelmiştir. Bunun için *transform* metotları kullanılmaktadır. *transform*
metotları programcıdan dönüştürülecek verileri bir NumPy dizisi olarak (Python listeleri ve Pandas *Series*
nesneleri de kullanılabilmektedir) alır ve kestirimi yaparak sonucu bize verir. Sınıfların pek çoğunda
(ama hepsinde değil) ``fit`` ve ``transform`` işlemlerini birlikte yapan ``fit_transform`` metotları da
bulunmaktadır. (Bazı sınıflarda ``inverse_transform`` metotları da vardır. *inverse_transform* metotları ters
işlemi yapmaktadır.) Örneğin:

.. code-block:: python

   transformed_dataset = cv.transform(target_dataset)

**4) Kestirim (predict)**

scikit-learn içerisindeki sınıfların bazıları dönüştürme, bazıları ise kestirim yapmaktadır. Dönüştürme
yapan sınıflarda dönüştürmeler genel olarak ``transform`` isimli metotlarla, kestirim yapan sınıflarda ise
kestirimler ``predict`` isimli metotlarla yapılmaktadır. ``predict`` metotlarına kestirilecek veriler argüman
verilir, metotlar da sonucu bize verir. Örneğin:

.. code-block:: python

   lr = LinearRegression(...)
   lr.fit(dataset)
   predict_result = lr.predict(predict_dataset)

Boru Hattı (Pipeline) Mekanizması
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

scikit-learn kütüphanesindeki fit/transform/predict kalıbı boru hattı (pipeline) mekanizmasının
uygulanmasını da mümkün hale getirmektedir. Makine öğrenmesinde *boru hattı (pipeline)* bir işlemin
çıktısının diğer işleme girdi yapılması, onun çıktısının da diğerine girdi yapılması ve böylece işlemlerin
peşi sıra daha zahmetsiz bir biçimde gerçekleştirilmesi anlamına gelmektedir. Örneğin önce *SimpleImputer*
işlemini, onun çıktısı üzerinde de ``StandardScaler`` işlemini yapmak isteyelim. ``StandardScaler`` işleminin
çıktısını da SVM işlemine sokmak isteyelim. Boru hattı kullanmadan bunları aşağıdaki gibi tek tek yapmak
zorunda kalırız:

.. code-block:: python

   si = SimpleImputer(...)
   si_transformed = si.fit_transform(...)

   ss = StandardScaler(...)
   ss_transformed = ss.fit_transform(si_transformed)

   svm = SVM(...)
   result = svm.fit_transform(ss_transformed)

İşte boru hattı mekanizması sayesinde bu işlemler tek tek değil önceki çıktıyı sonraki girdiye vererek
otomatik bir biçimde de yapılabilmektedir:

.. code-block:: python

   pl = PipeLine([('Imputing', SimpleImputer(...)), ('Scaling', StandardScaler(...)), ('SVM', SVM(...))])
   result = pl.fit_transform(...)

SciPy
-----

Bilimsel uygulamalarda en fazla kullanılan kütüphanelerden biri de *SciPy* isimli kütüphanedir. SciPy
temelde bir nümerik analiz kütüphanesidir. Bu kütüphane pek çok matematiksel işlem yapan sınıflara ve
fonksiyonlara sahiptir. Örneğin bir fonksiyonun maksimum ve minimum noktalarının elde edilmesi gibi, bir
fonksiyonun belli bir noktadaki türevinin elde edilmesi gibi, bir denklemin köklerinin bulunması gibi
işlemler SciPy kütüphanesiyle yapılabilmektedir. SciPy lineer cebir ve istatistik paketlerine de sahiptir.
Örneğin SciPy ile doğrusal denklem sistemleri çözülebilmekte, bazı istatistiksel hipotez testleri
gerçekleştirilebilmektedir.

Anaconda dağıtımında SciPy kütüphanesi de yüklü olarak gelmektedir. Ancak diğer ortamlara kütüphaneyi
şöyle kurabilirsiniz:

.. code-block:: console

   pip install scipy

SciPy taban kütüphane olarak NumPy kütüphanesini kullanmaktadır.

TensorFlow ve PyTorch
---------------------

Yapay sinir ağlarına ilişkin iki taban (base) kütüphane yaygın olarak kullanılmaktadır: TensorFlow ve
PyTorch. TensorFlow kütüphanesi Google kökenli, PyTorch ise Meta (Facebook) kökenlidir. Eskiden TensorFlow
en yoğun kullanılan kütüphaneydi. Ancak son yıllarda PyTorch kütüphanesini tercih edenler TensorFlow
kütüphanesini tercih edenlerden fazla hale gelmiştir. Bunların yanı sıra eskiden Theano isimli bir
kütüphane de özellikle akademik çevreler tarafından kullanılıyordu. Ancak 2017 yılından beri bu
kütüphanenin geliştirilmesi durdurulmuştur.

TensorFlow ve PyTorch için "kütüphane" yerine "framework" terimi de kullanılabilmektedir. Bir yazılımsal
araca "framework" denilebilmesi için onun "akışı bazen programcıdan alıp, gerektiğinde programcının
belirlediği fonksiyonları çağırabilmesi (inversion of control)"" gerekmektedir. Bu açıdan bakıldığında
TensorFlow ve PyTorch için *framework* nitelemesi de yapılabilir. Ancak biz kursumuzda bunlara framework
yerine kütüphane demeyi tercih edeceğiz.
/
TensorFlow ve PyTorch kütüphaneleri (ya da framework'leri) eskiden yalnızca yapay sinir ağlarına ilişkin
temel işlemleri barındırıyordu. Yani bunlar aşağı seviyeli kütüphanelerdi. Ancak zamanla bu kütüphaneler
gelişti ve yüksek seviyeli katmanlara da sahip olmaya başladı. Örneğin yüksek seviyeli yapay sinir ağları
işlemlerini yapmak için tasarlanmış olan *Keras* isimli bir kütüphane bulunmaktadır. Keras başlangıçta
bağımsız bir kütüphaneydi. Ancak daha sonraları TensorFlow bünyesine katıldı. Biz kursumuzda yettiği
sürece bu Keras kütüphanesini kullanacağız. PyTorch'a da zamanla Keras gibi yüksek seviyeli katmanlar
eklenmiştir.