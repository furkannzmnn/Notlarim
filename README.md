# Notlarim

### ELASTİCSEARCH

<pre><code>
elasticsearch kullanmamın sebebi sql sorgularındaki like methodunun maliyetli olması(CPU TÜKETİYOR)

Elasticsearch	->	Database
Index	->	Table
Document	->	Row
Field	->	Column
</code></pre>



### KOTLİN

<pre><code>
entity sınıflarını kotlin ile kullanmamın temel sebebi immutable yapısı ve boilerplate kodlardan kurtarması

</code></pre>

### JAVA
------------------------------------------------------------------------------------------------------------------------------------------------
#### Constructor
<pre><code>
1-Çok Sayıda Parametreyle Karşılaştığınızda veya anlaşılmazlık sorunu olduğunda  Builder Kullanın
 Gördüğünüz gibi Builder aslında sınıf içerisinde tanımlanmış başka bir statik sınıf ve build() harici bütün
    metotlarından this nesnesini döndürüyor. Yani istemci builder nesnesini geri aldığı için
    zincirleme metot çağrıları yaparak asıl oluşturmak istediğimiz BesinDegeri nesnesini yaratabilir.
Burada dikkat edilmesi gereken şey, zincirleme metot çağrılarını yaparken tamamen Builder nesnesini kullanıyoruz, son ana kadar build() metodunu çağırmadan BesinDegeri nesnesi yaratılmıyor. İstemci yaratmak istediği nesnenin alanlarını doldurduktan sonra build() metodunu çağırınca BesinDegeri yapıcı metodu çalıştırılarak bir daha değiştirilmesi mümkün olmayan (immutable) bir nesne oluşuyor. Böyle hem okuması/yazması kolay bir kod hem de her ortamda sorunsuz çalışacak değiştirilemez (immutable) bir nesne yaratmış oluyoruz.

Eğer alanlar için sınırlama getirmek isterseniz (kalori 0’dan küçük olamaz gibi) bunu ister atama metotlarında isterseniz yapıcı metot içerisinde kontrol edebilirsiniz. İstemci uygun olmayan bir değer geçerse IllegalStateException fırlatarak nesnenin oluşmasını engelleyebilirsiniz.

Builder deseni çok sayıda opsiyonel parametre gerektiren nesneleri yaratmada büyük esneklik sağlar, aynı Builder sınıfını kullanarak çok sayıda nesne oluşturabilirsiniz. Buna karşılık olarak yazdığınız kod satırı sayısı biraz artmakta ama sağladığı avantajlar düşünülünce bu durum gözardı edilebilir. Sadece 3 parametreli sınıflarda Builder kullanmak çok gerekli olmayabilir ancak parametre sayısının ileride artacağını düşünüyorsanız sonradan değiştirmektense ilk başta builder kullanmak daha mantıklı olabilir.

Özetleyecek olursak builder tasarım deseni hem JavaBeans yönteminden çok daha güvenli olduğu için hem de çok sayıdaki parametreden dolayı sınıf yapıcı veya statik fabrika metodu kullanmanın getireceği okunabilirlik problemlerini çözdüğü için sıklıkla tercih edilir. 


2- Nesne Yaratılmasını İstemediğiniz Sınıfları Private Constructor  İle Güçlendirin
Bazen herhangi bir yapıcı method(constructor) tanımlanmadığı zaman  Java derleyicisi otomatik olarak parametre almayan public bir varsayılan constructor (default constructor) tanımlayacaktır. Bir istemci açısından bu varsayılan yapıcı metodun, açıkça tanımlanmış diğer yapıcı metotlardan hiçbir farkı yoktur. Bu sebeple, birçok API içerisinde istemeden de olsa nesne yaratılabilen sınıflar bulunmaktadır. Nesne yaratılmasının önüne geçmek için sınıfı Abstract yapmak çözüm sağlamaz. sebebi -> bu sınıf kalıtılarak child classdan nesne yaratılabilir. veya ek olarak kodu okuyan kişi veya yazılımcı bu sınıfın kalıtılmak için tasarlandığı algısını  veya sonucunu çıkarabilir.
Nesne yaratılmasını nasıl önleriz -> Çözümü çok basit. constructor(yapıcı method) tanımlarken bunları private yapın. Biz açıkça bir yapıcı metot tanımladığımız zaman, derleyici varsayılan public yapıcı metodu tanımlamaz. (CONSTRUCTOR -> PhoneNumber classını anlat )
avantajlarından bahsettik şimdi de bir dezavantajından bahsedelim.
 bu private yapıcı metot aynı zamanda sınıfın kalıtılmasını da engeller
Biz yukarıdaki yardımcı sınıfta hem varsayılan yapıcı metodun tanımlanmasını engellediğimiz için hem de kendi yapıcı metodumuzu private tanımladığımız için, bu sınıfı kalıtacak bir çocuk sınıfın çağırabileceği bir yapıcı metot bulunmamaktadır.

</code></pre>


#### Final
<pre><code>
javada final değişmeyen değerlere atamak için kullanırlır. değişkenler parametreler methodlar ve classlar final olabilir.
her kullanımın amacı ve açıklaması aynı değildir. değişkenin final olmaıs demek onun bir daha değiştirilemeyeceği anlamına gelir.
bir nesneni referansının final olması demek o referansa bir daha yeni bir nesne atayamacağın anlamına gelir. bir sınıfın final olması o sınıftaki
tüm fieldların final olması anlamına gelir.
önceden  final değişkenlere değer atanırsa  -> constant variable
</code></pre>

#### Interface

<pre><code>
 NEDİR ? =  kısaca iki birim arasındaki etkileşimi sağlayan ve bu yüzden iki tarafa da anlayabileceği dille konuşması gereken bir yapıdır.
 
 
 Java da Interface 3 Farklı alanda kullanımı bulunur.

1- Sınıf Arayüzü: Sınıf ve enum tiplerin arayüzü vardır = Sahip olduğu bütün methodların arayüzüne denir.

2- Method Arayüzü: Methodların arayüzü vardır. =  Method arayüzü dönüş tipi varsa parametre ve varsa exception gibi değerlerin olduğu arayüzlerdir. 
*O arayüzün ne yaptığını ifade eden bilgisidir

3- Interface: Tam anlamıyla tüm methodları soyut olan sınıftır.
 interfacelerin metotları varsayılan halde public abstract dır.
 abstract ile farkı , abstract sınıfların içinde soyut methodlar haricinde somut yani concrete metotlarda bulunur.
 interface sadece arayüz sağlar. Durum ve gerçekleştirme(state) sağlamaz. Normal bir sınıftan farkı da budur. Normal sınıflar state yönetimi de sağlar. Tabiki şuana kadar bu bilgiyi doğru kabul ettik. Fakat Java SE8 ile birlikte artık interfacelerde de state yönetimi sağlanabiliyor.(ileriki dakikalarda anlatılacak)
 Arayüzlerin constructor yani yapıcı methodları yoktur ve nesnesi oluşturulamaz.
 

Signatür nedir ? 
Bir methodun isim ve parametre bilgisinde oluşan bilgisine imza yani signature nedir.
Not: return tipi ve fırlatılan exceptionlar imzaya dahil değildir.


</code></pre>

#### Temel Sınıf Kavramları

<pre><code>
0- HEAP VE STACK NEDİR
-  KISACA HEAP
-  KISACA STACK
1- primitive ve reference tipler (heap ve stack)
$- primite ve reference type karşılaştırması
-  JVM MİMARİSİ
4-instance variable
5-instance method
2-static variable
3-static method -> udemy akın hoca
6 pass by value -> https://metinalniacik.medium.com/java-pass-by-reference-m%C4%B1-yoksa-pass-by-value-mu-a0f8935772b5
7-stack ve heap -> https://medium.com/t%C3%BCrkiye/stack-ve-heap-kavram%C4%B1-59adcb29d454
8-this -> https://medium.com/@bits.rahulgupta/java-this-keyword-77ada255f321  // -> udemy akın hoca


KISACA STACK ? 

LIFO (Last in First out) son giren ilk çıkar mantığında çalışır.
Veri depolama alanı çok geniş olmadığından kullanımı kolay ve hızlıdır.
Veriler Big and Little Endian (artan ya da azalan) adres mantığında tutulur.
Derleme zamanında oluşturulur.
Life time (yaşam süresi) kısa olan değişkenler tutulur. Ör; local variables (yerel değişkenler).
Static allocation (Kullanılacak depolama alanının boyutu biliniyorsa stack işe yarayacaktır.)
Bir Java uygulamasında sadece tek bir stack yoktur. Her bir thread’in kendi stack’i vardır.
Bir stack üzerindeki veriye kendi thread’inden başka bir thread erişemez.
Doğru kullanılmadığında java.lang.StackOverFlowError hatası alınır.

----------------------------------------------------------------------
KISACA HEAP
Heap stack’e göre daha büyük boyuta sahiptir.
Stack’e göre daha fazla alana sahip olduğundan stack’e göre daha yavaştır.
Heap’teki veriler karışık şekilde sıralanır. 
Çalışma zamanında oluşturulur.
Dynamic allocation (Kullanılacak depolama alanının boyutu bilinmiyorsa ya da sürekli değişken olacak ise heap kullanmak doğru olacaktır.)
Bir Java uygulamasında tüm thread’ler için sadece bir tane heap bulunmaktadır.
Doğru kullanılmadığında java.lang.OutOfMemoryError hatası alınır.

### HEAP KUŞAKLARI
Young Generation(Genç Kuşak): Bu kuşak tüm yeni nesnelerin oluşturulduğu ve yaşlanmaya başladığı kuşaktır. Yeni oluşan nesneler bu kuşakta tutulur. Young Generation dolduğunda ise bu ileride göreceğimiz bir başlık olan ‘Minor Garbage Collection’ işlemini tetikler. Ölü/kullanılmayan/unreferenced nesneler burada çok hızlı dolar ve toplanmayı bekler. Belli bir süre sonra ‘surviving’ hayatta kalan nesneler ‘Old Generation’ alanına taşınır.

Old Generation(Yaşlı Kuşak): Bu kuşak uzun süre hayatta kalan nesnelerin bellekte tutulduğu yerdir. Genç kuşakta belli bir süre(belirlenmiş bir threashold değeri kadar) duran nesneler bu kuşağa alınır. Old Generation dolduğunda ise ‘Major Garbage Collection’ işlemini tetikler.

Stop-the-World: Bütün minor ve major garbage collection işlemleri ‘Stop-the-World’ eventlerdir. Yani garbage collector işini yaparken uygulamadaki tüm diğer threadler durur. Ta ki garbage collector işini yapana kadar. Bu sebeple garbage collector optimizasyonu çok önemlidir. Minor ve Major garbage collection işlemlerinin sayısını düşürerek uygulamanızın daha performanslı ve stabil çalışmasını sağlayabilirsiniz. Tabiki bu durum uygulamadan uygulamaya değişiklik gösterebilir. Bu sebeple Garbage Collector Tuning yapılmalıdır.
Permanent Generation(Kalıcı Kuşak): Bu kuşak JVM tarafından gerekli olan class ve methodlar hakkında metadataları tutar. Uygulamanın çalışma esnasında dolabilir. Ayrıca Java SE’de yer alan kütüphane sınıfları ve metodları da burada tutulabilir. Bu kuşağın çöp toplama işlemi ‘Full Garbage Collection’ içinde yer alır.

----------------------------------------------------------------------
 PRİMİTİVE TYPE NEDİR ?
stack ve heap bellekte yani ramda bulunan mantıksal yapılara verilen isimdir.
Primitif tip dediğimiz int, short, byte, long, double, float gibi değerler value type olarak adlandırılırlar ve stack bölümünde tutulurlar.
Runtimeden yani Çalışma zamanından önce bu değerlerin bilinmesi gerekiyor. Çünkü işletim sistemi program çalışmadan önce stack de belirli bir yer ayırır eğer bu bölüm kodu yazan kişi tarafından aşılırsa stack taşma hatası ile (stack overflow) karşılaşılabilir. 
----------------------------------------------------------------------

REFERENCE TYPE NEDİR ?
Pointer’ları stackte değerleri heap(yığın) de bulunan veri tipleridir. Örnek olarak; String, int[] verebiliriz. Herhangi bir değer girilmediğinde varsayılan değer null olacaktır.
Primitive type’larda (==) kullanırsak bu değişkenlerin içindeki değerlerin eşit olup olmadığına kontrol eder.
Örneğin
int a = 5;
int b = 5;
System.out.println(a == b);
Sonuç true olarak dönecektir.
Reference type’larda ise (==) nesnelerin adreslerinin eşit olup olmadığını kontrol eder. Eğer değişkenlerin içindeki değerlerin eşit olup olmadığını kontrol etmek istiyorsak, bu durumda equals metodu kullanılır.
Örneğin
Long a = new Long(5);
Long b = new Long(5);
System.out.println(a == b);
Sonuç false dönecektir çünkü adresleri farklıdır.
System.out.println(a.equals(b));
Sonuç true dönecektir çünkü değişkenlerin içindeki değerler aynıdır.
Atama (=) işlemi de diğer işlemler gibi farklılık gösterir. Primivite type’larda (=) kullandığımızda değerden yeni bir kopya oluşturur.
Örneğin
int a = 5;
int b = a; // “b”nin değeri de 5 oldu.
a = 10; // “a”nın değeri 10 olarak değişti. Fakat bu durum “b”yi etkilemedi. Çünkü “b” primitive type’dır ve “b”nin değeri hala 5 tir.
Reference type’lar için durum biraz farklıdır.
Örneğin
int[] a = {1,2,3};
int[] b = a; // şu anda b ve a referans tipleri aynı yeri gösteriyor.
b[2] = 10; // bu kısımda b 2. indexdeki veriyi değiştirmesine rağmen a[2] de değişiyor. Çünkü ikinci satırda aynı adresi gösteriyorlar.
System.out.println(a[0] + “, “ + a[1] + “, “ + a[2]); // 1, 2, 10
System.out.println(b[0] + “, “ + b[1] + “, “ + b[2]); // 1, 2, 10

Bazı referans tiplerinde ise güncelleme işlemi gerçekleştirilemez. Güncelleme işlemi yapmaya çalıştığınızda heap üzerinde yeni bir nesne oluşturulur ve pointer yeni nesneyi gösterir. Bu tip nesnelere immutable object (değişmez nesne) denir. Bu duruma örnek olarak String verilebilir.

----------------------------------------------------------------------

instance variable
Instance Variable(member variables) , hatirlayacagimiz gibi sinif seviyesindeki degiskenlerdi. Yani bu instance variable’lar bir metot, yapilandirici, kod blogu icerisinde tanimlanamaz.
Instance Variable’lara varsayilan olarak deger atamasi yapilir. Asagidaki tabloda varsayilan olarak atanan bu degerleri gorebiliriz.
----------------------------------------------------------------------

LOCAL VARİABLE
YAŞAM SÜRESİ VARDIR. BLOKTAN VEYA DÖNGÜDEN ÇIKINCAYA KADARDIR. ÇIKTIKTAN SONRA ÖLÜRLER.
----------------------------------------------------------------------

CONSTRUCTOR VE THİS
kurucular içinde tanımlandığı sınıfın ismini alır.
isimleri diğer sınıfların aksine UPPER CAMEL CASE uyarlar.
kurucuların dönüş tipi ve değeri yoktur.
kurucular nesne oluşturulurken new anahtar kelimesi ile çağrılır.

2 tür constructor vardır.
1- ARGÜMANSIZ KURUCULAR
2- ARGÜMANLI KURUCULAR.

Eğer bir constructor argüman alıyor ise compiler default constructorı artık sağlamaz peki neden ?

-------------
STATİC

Bazen nesnelerin öyle özellikleri olurki nesneden nesneye değişmez. aynı sınıftan oluşturulmuş tüm nesneler için aynıdır.
Çünkü aslen bir tane değişken ve tek değer yeterlidir. 
diğer elemanlar dinamik yani nesne için oluşturuluyor. Ama static ile tanımladığımız static yanş nesne için oluşturulmuyor.

-- static değişkenlerin nesnelerin ortak durumunu oluştururlar.
-- static değişkenlerin tek bir değeri vardır ve tek kopyası , onlar da sınıftadırlar.
-- static değişkenlere ulaşmak için nesneye ihtiyaç yoktur sınıf üzerinden ulaşılırlar.
-- staticlere sadece sınıf üzerinden de değil sınıfların referanslarının üzerinden de ulaşılabilir.
-- static değişkenin değeri tüm nesneler için aynıdır.

-- nesne methodları static olsun olmasın tüm değişken ve methodlara yani tüm üyelere(members) ulaşırlar.
-- Ama static methodlar sadece static değişkenlere doğrudan ulaşabilirler.Nesne değişkenlerine ulaşamazlar.Çünkü statik method zaten nesneyi var saymadığından ancak nesne
var olmadan varolan değişken ve methodlara ulaşması söz konusudur.

NE ZAMAN STATİC
-- nesne merkezli programlamada asıl olan nesnedir.her şeyin, bir nesne olarak düşünülmesi ve yapılması gerekir.

-- Static ve final olan tanımlamlar ile sınıflar üzerinden erişilen ve değeri değişmeyen constant oluşturulabilir. 
Bu şekilde tanımlanan sabitelerde Camel Case kullanılmaz. büyük harflar kullanılır ve kelimeler arasına _ konur.
"public static final double PI = 3.14234233324
E COMMERCE ÖRNEKLEMESİ (STRİNG PRODUCT_NOT_FOUND)


NESNELERİN ANLAMSIZ YA DA GEREKSİZ OLMASI NE DEMEKTİR ?
Nesnesini oluştursak bile öörneğin productNotFoundan bir tane olması gerekir veya Math kütüphanesindeki PI den bir tane olması gerekir.
Bu durumda Sınıf olabilecek tek nesne gibi düşünülebilir. ve bu şekilde kullanılır.


</code></pre>

### ARRAYS
Arrayların'in memoride contiguous özelliği vardır. Arrayler  içi dolu olsa da olmasada memoride belirli bir alan kaplarlar.
Teorik olarak bir array'in alabileceği max item sayısı MAX_INTEGER-1
Arraylerin size değerini Arrays.copyOf ile değiştirebilirim.


------------------------------------------------------------------------------------------------------------------------------------------------


### Spring
------------------------------------------------------------------------------------------------------------------------------------------------
#### DEPENCY INJECTİON

<pre><code>
bu işlemi depency inversion ilkesi ile yaparız. bu ilke yüksek seviye modüllerin düşük seviye modüllere bağlı olamaması gerektiğini belirtir.
</code></pre>

#### JPA & Hibernate
<pre><code>Tüm ilişkileri lazy tanımla -> derin object hiyarerşilerde spesifik bir entitye eriştiğimizde bu entity ile ilişkili pek çok ilgili-ilgisiz diğer entitynin yüklenmesinin önüne geçiyor.

*OneToOne ve OneToMany ilişkilerinde cascade kullan onun dışındakilerde kullanma. -> 

*Embedded/Embeddable yerine her zaman Entity kullan (sebebi : nesne üzerinde sorgu işlemi yapamıyoruz) -> entity içindeki sütünlara karşılık gelen değerlerde tutuluyor

*derin inheritance hiyararşilerinden kaçının (SİNGLE_TABLE) kullanının -> Performans için
*GeneratedValue implemantasyonunda 'SEQUENCE' kullan (her sql desteklemez)
* Hiçbir zaman Entity dönme , Her senaryoya ait kendi dtonu oluştur. -> 
*LazyInilization Excepiton probleminin en iyi çözüm yolu 'FETCH JOIN'dir.




</code></pre>

#### REST API BEST PRACTİCE

<pre><code>
* geriye bir text döndürme , bir kesin kural olmasa da geriye json ile dönüş yapalım.
* “Content-Type: application/json” olarak ayarla.
* URI'lerde fiil kullanma -> 
GET: /books/:slug/generateBookCover/ YERİNE  GET: /books/:slug/bookCover/  KULLANALIM . Çünkü burada zaten http methodu olan GET ne yapmak istediğinizi açıkca ifade ediyor.
"# Don’t do this
POST: /books/createNewBook/
# Do this
POST: /books/"

*  resourcelar için çoğul isimler kullanın
* Response Body'deki hata ayrıntılarını döndürün ->

<pre><code>
{
    "error": "Invalid payload.",
    "detail": {
        "name": "This field is required."
    }
}
</code></pre>
* HTTP durum kodlarına özellikle dikkat edin (message , detail)
* HTTP durum kodlarını tutarlı bir şekilde kullanmalısınız ->
GET: 200 OK
PUT: 200 OK
POST: 201 Created
PATCH: 200 OK
DELETE: 204 No Content

* Resource'ları iç içe geçirme
* Sondaki eğik çizgileri zarif bir şekilde kullanın

*filtering and pagination için querystring  kullanın
* 401 Unauthorized ve 403 Forbidden arasındaki Farkı öğrenin ->
Tüketici kimlik doğrulama bilgileri sağlamadı mı? SSO Jetonları geçersiz/zaman aşımına uğradı mı? 👉 401 Unauthorized.
Tüketicinin kimliği doğru bir şekilde doğrulandı, ancak kaynağa erişmek için gerekli izinlere/uygun izne sahip değil mi? 👉403 Forbidden.
 
*HTTP'yi iyi kullanın
*REST API'lerinde uzmanlaşmış bir framework kullanın 


</code></pre>




------------------------------------------------------------------------------------------------------------------------------------------------

<pre><code>aaa
</code></pre>

