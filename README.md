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
1- primitive ve reference tipler (heap ve stack)
$- primite ve reference type karşılaştırması
4-instance variable
5-instance method
2-static variable
3-static method -> udemy akın hoca
6 pass by value -> https://metinalniacik.medium.com/java-pass-by-reference-m%C4%B1-yoksa-pass-by-value-mu-a0f8935772b5
7-stack ve heap -> https://medium.com/t%C3%BCrkiye/stack-ve-heap-kavram%C4%B1-59adcb29d454
8-this -> https://medium.com/@bits.rahulgupta/java-this-keyword-77ada255f321  // -> udemy akın hoca

 PRİMİTİVE TYPE NEDİR ?

stack ve heap bellekte yani ramda bulunan mantıksal yapılara verilen isimdir.
Primitif tip dediğimiz int, short, byte, long, decimal, double, float gibi değerler value type olarak adlandırılırlar ve stack bölümünde tutulurlar.
Runtimeden yani Çalışma zamanından önce bu değerlerin bilinmesi gerekiyor. Çünkü işletim sistemi program çalışmadan önce stack de belirli bir yer ayırır eğer bu bölüm kodu yazan kişi tarafından aşılırsa stack taşma hatası ile (stack overflow) karşılaşılabilir. 

REFERENCE TYPE NEDİR ?
Pointer’ları stackte değerleri heap(yığın) de bulunan veri tipleridir. Örnek olarak; String, int[] verebiliriz. Herhangi bir değer girilmediğinde varsayılan değer null olacaktır.

instance variable
Instance Variable(member variables) , hatirlayacagimiz gibi sinif seviyesindeki degiskenlerdi. Yani bu instance variable’lar bir metot, yapilandirici, kod blogu icerisinde tanimlanamaz.
Instance Variable’lara varsayilan olarak deger atamasi yapilir. Asagidaki tabloda varsayilan olarak atanan bu degerleri gorebiliriz.

LOCAL VARİABLE
YAŞAM SÜRESİ VARDIR. BLOKTAN VEYA DÖNGÜDEN ÇIKINCAYA KADARDIR. ÇIKTIKTAN SONRA ÖLÜRLER.



</code></pre>

------------------------------------------------------------------------------------------------------------------------------------------------


### Spring
------------------------------------------------------------------------------------------------------------------------------------------------
#### DEPENCY INJECTİON

<pre><code>
bu işlemi depency inversion ilkesi ile yaparız. bu ilke yüksek seviye modüllerin düşük seviye modüllere bağlı olamaması gerektiğini belirtir.
</code></pre>

#### JPA & Hibernate
<pre><code>Tüm ilişkileri lazy tanımla
*OneToOne ve OneToMany ilişkilerinde cascade kullan onun dışındakilerde kullanma.
*Embedded/Embeddable yerine her zaman Entity kullan (sebebi : nesne üzerinde sorgu işlemi yapamıyoruz)
*derin inheritance hiyararşilerinden kaçının (SİNGLE_TABLE) kullanının -> Performans için
*GeneratedValue implemantasyonunda 'SEQUENCE' kullan (her sql desteklemez)
* Hiçbir zaman Entity dönme , Her senaryoya ait kendi dtonu oluştur. -> 
*LazyInilization Excepiton probleminin en iyi çözüm yolu 'FETCH JOIN'dir.




</code></pre>



------------------------------------------------------------------------------------------------------------------------------------------------

<pre><code>aaa
</code></pre>

