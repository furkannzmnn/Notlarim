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
Nesne Yaratılmasını İstemediğiniz Sınıfları Private Constructor  İle Güçlendirin
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

