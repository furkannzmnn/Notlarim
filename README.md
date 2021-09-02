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



------------------------------------------------------------------------------------------------------------------------------------------------

<pre><code>aaa
</code></pre>

