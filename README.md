 JSL_TASK



    İÇİNDEKİLER

1. Ner_Pipeline.ipynb  
    1. Lisans Dosyası Yükleme  
    2. Gerekli Kütüphanelerin Kurulumu  
    3. Spark NLP Oturumunun Başlatılması  
    4. Veri Yükleme ve Hazırlama  
    5. NLP Pipeline Kurulumu  
    6. Farklı NER Modelleriyle Çıktı Alma  
    7. Sonuçların Birleştirilmesi ve CoNLL Formatına Dönüştürülmesi  
    8. Sonuçların Kaydedilmesi ve Github’a Yüklenmesi  

2. Custom_Ner_Model.ipynb  
    1. Lisans Dosyası Yükleme  
    2. Gerekli Kütüphanelerin Kurulumu  
    3. Spark NLP Oturumunun Başlatılması  
    4. Veri Hazırlama ve Filtreleme  
    5. Embedding ve Parquet’e Kaydetme  
    6. Kendi NER Modelini Eğitme  
    7. Modeli Değerlendirme  



1. Ner_Pipeline.ipynb

 1. Lisans Dosyası Yükleme  
Spark NLP Healthcare ve Spark NLP JSL gibi ücretli kütüphaneleri kullanabilmek için, öncelikle bir lisans dosyasının yüklenmesi gerekir. 
Bu dosya genellikle bir JSON formatında olur ve içindeki anahtarlar hem Python ortamına hem de sistem ortam değişkenlerine eklenir. 
Lisans dosyası olmadan Spark NLP JSL fonksiyonları çalışmaz; bu nedenle bu adım, projenin başında mutlaka tamamlanmalıdır.

2. Gerekli Kütüphanelerin Kurulumu  
Projede kullanılacak olan Spark NLP, Spark NLP JSL ve diğer yardımcı kütüphaneler yüklenir. 
Burada özellikle kütüphane versiyonlarının birbiriyle uyumlu olmasına dikkat edilmelidir. 
Yanlış bir versiyon kurulursa, ilerleyen adımlarda uyumsuzluk ve hata çıkma ihtimali yüksektir. 
Kurulumlar tamamlandıktan sonra, her şeyin doğru şekilde yüklendiği kontrol edilir.


3. Spark NLP Oturumunun Başlatılması  
Kütüphaneler kurulduktan sonra, Spark oturumu başlatılır. 
Spark oturumu başlatılırken, kullanılacak bellek miktarı ve diğer bazı parametreler ihtiyaca göre ayarlanır. 
Spark NLP ve Spark NLP JSL’in yüklü versiyonları ekrana yazdırılarak, ortamın doğru şekilde hazırlandığı teyit edilir. 
Spark oturumu başlatılmadan Spark tabanlı NLP işlemleri yapılamaz.

 4. Veri Yükleme ve Hazırlama  
Çalışmada kullanılacak veriler, hem kullanıcıdan yüklenebilir hem de internetten indirilebilir. 
Yüklenen dosyalar önce pandas ile okunur, ardından Spark DataFrame’e dönüştürülür. Spark tabanlı pipeline’larda verinin bu formatta olması gerekir. 
Bu aşamada, verinin yapısı ve içeriği de incelenerek, sonraki adımlar için uygun olup olmadığı kontrol edilir.


5. NLP Pipeline Kurulumu  
Metin verileri üzerinde doğal dil işleme adımlarını uygulayabilmek için bir pipeline kurulur. 
Bu pipeline’da önce cümleler tespit edilir, ardından kelimelere bölme (tokenizasyon) işlemi yapılır ve son olarak kelime gömme (embedding) işlemleri gerçekleştirilir.
Pipeline, veriye uygulandığında, metinler makine öğrenimi için uygun bir formata dönüştürülmüş olur. 
Bu adım, sonraki modelleme işlemleri için temel oluşturur.

6. Farklı NER Modelleriyle Çıktı Alma  
Hazırda bulunan farklı NER (Varlık Adı Tanıma) modelleriyle metinlerdeki varlıklar tespit edilir. 
Her model, metinlerdeki farklı türdeki varlıkları (örneğin ilaç, hastalık, tarih gibi) bulur ve bunları etiketler. 
Sonuçlar tablo halinde incelenir ve modellerin hangi tür varlıkları daha iyi tespit ettiği gözlemlenir. 
Bu adımda, model çıktılarının güven skorları da değerlendirilir.

7. Sonuçların Birleştirilmesi ve CoNLL Formatına Dönüştürülmesi  
Birden fazla modelin çıktısı birleştirilirken, öncelik sırasına göre birleştirme yapılır.
Yani, bazı modellerin bulduğu varlıklar diğerlerine göre daha öncelikli kabul edilebilir.
Biz burda öncelikle ner_posology modelini daha sonra ise  ner_deid_generic_augmented ve en son olarak ner_clinical modelini yerleştirdik.
Bütün modellerin birleştiği pipeline dan aldığımız data frame conll file oluşturmak iki farklı tarzda pandas data frame çevirdik.
Bu iki data frame conll file oluşturma fonksiyonuna parametre olarak verdik.
Sonuçlar, NER eğitiminde yaygın olarak kullanılan CoNLL formatına dönüştürülür. 
CoNLL formatı, her kelimenin karşısında hangi varlık türüne ait olduğunu gösterir ve model eğitimi için standart kabul edilir. 
Bu dönüşüm, verinin başka projelerde veya modellerde de kullanılabilmesini sağlar.

8. Sonuçların Kaydedilmesi ve Github’a Yüklenmesi  
Elde edilen sonuçlar dosyaya kaydedilir ve versiyon kontrolü için Github’a yüklenir. 
Bu sayede hem çalışmanın bir yedeği alınmış olur hem de başkalarıyla paylaşmak kolaylaşır. 
Ayrıca, ileride yapılacak güncellemeler ve değişiklikler de bu şekilde takip edilebilir.



2. Custom_Ner_Model.ipynb

1. Lisans Dosyası Yükleme  
Spark NLP Healthcare için gerekli olan lisans dosyası yüklenir ve ortam değişkenlerine eklenir. 
Bu adım, Spark NLP JSL fonksiyonlarının çalışabilmesi için gereklidir ve projenin başında tamamlanmalıdır.

2. Gerekli Kütüphanelerin Kurulumu  
Projede kullanılacak olan tüm Python kütüphaneleri yüklenir. 
Özellikle Spark NLP ve Spark NLP JSL’in doğru versiyonlarının kurulmasına dikkat edilir. 
Kütüphaneler yüklendikten sonra, ortamın hazır olup olmadığı kontrol edilir.

3. Spark NLP Oturumunun Başlatılması  
Spark oturumu başlatılır ve gerekli parametreler ayarlanır. 
Spark NLP ve Spark NLP JSL’in yüklü versiyonları ekrana yazdırılarak, ortamın doğru şekilde hazırlandığı teyit edilir. 
Spark oturumu başlatılmadan Spark tabanlı işlemler yapılamaz.

4. Veri Hazırlama ve Filtreleme  
Eğitim verisinde, modelin öğrenmesini istenmeyen bazı varlık türleri (örneğin konum, dozaj gibi) ‘O’ etiketiyle değiştirilir. 
Bu sayede modelin sadece ilgilenilen varlık türlerini öğrenmesi sağlanır ve eğitim verisi sadeleştirilmiş olur. 
Filtreleme işlemi tamamlandıktan sonra, verinin yeni hali kaydedilir.


5. Embedding ve Parquet’e Kaydetme  
Eğitim ve test verisi için kelime gömme (embedding) işlemleri yapılır. 
Elde edilen veriler, Spark’ın hızlı okuyabileceği Parquet formatında kaydedilir. 
Bu format, model eğitimi sırasında veriye hızlı erişim sağlar ve büyük veri setlerinde performansı artırır.

6. Kendi NER Modelini Eğitme  
Kendi NER modelini eğitmek için önce modelin graph dosyası oluşturulur, ardından eğitim süreci başlatılır. 
Eğitim sırasında modelin başarısı, epoch, loss ve f1 gibi metriklerle takip edilir. 
Eğitim tamamlandığında, modelin kendi veri setine ve ihtiyaca uygun şekilde çalıştığı görülür.

7. Modeli Değerlendirme  
Eğitimi tamamlanan modelin başarısı, precision, recall ve f1 gibi metriklerle değerlendirilir. 
Her bir varlık türü için ve genel olarak modelin ne kadar iyi çalıştığı analiz edilir. 
Bu değerlendirme sayesinde modelin güçlü ve zayıf yönleri net bir şekilde ortaya konur.

