Effective Java kitabını duymuşsunuzdur, Joshua Bloch efsane kitabında madde madde sizlere nasıl kaliteli Java kodu yazacağınızı anlatır. Bu kitapta anlatılanlarla ilgili yüzlerce İngilizce kaynak bulmak mümkün ama ben Türkçe bir kaynak bulamayınca sizlere bu kitabı özetlemeye karar verdim. Direk kitabın tam çevirisi olmasa da kendimce eklemeler çıkarmalar yaparak ve önemli gördüğüm yerleri vurgulayarak kitaptaki bölümlerden daha kısa ama aşağı yukarı aynı şeyi anlatan yazılar paylaşacağım. Başlangıç olarak kitaptaki ilk madde olan statik fabrika metotlarıyla başlıyoruz.

Normal şartlarda bir sınıf kendisinden nesne oluşturulmasını istiyorsa public bir sınıf yapıcı (constructor) tanımlar ve diğer sınıflar bunu kullanarak nesne oluşturabilir. Ancak her yazılımcının bilmesi gereken başka bir nesne yaratma yöntemi daha var. Bu yöntemde sınıf, dönüş değeri kendi nesnesi olan statik bir fabrika metodu (static factory method) tanımlar ve bu sınıftan nesne oluşturmak isteyenler bu metodu kullanırlar.

Örnek olarak JDK içerisinde bulunan Boolean sınıfından bir metot verebiliriz. Bu metot ilkel bir boolean değeri (true veya false) Boolean türünde bir nesneye dönüştürür.


<pre><code>
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
</code></pre>

Burada şunu da belirtmek gerekir ki statik fabrika metotları Factory Method tasarım deseninden farklıdır. Burada anlatılan statik fabrika metotlarının direk karşılığı olan bir tasarım deseni bulunmamaktadır.

Bir sınıf, statik fabrika metotlarını kullanıcılarına sınıf yapıcılar yerine veya sınıf yapıcılara ek olarak sunabilir. Public bir sınıf yapıcı yerine statik fabrika metodu kullanmanın hem avantajları hem de dezavantajları vardır.

Statik fabrika metotlarının avantajlarından bir tanesi isimlerinin olmasıdır. Bildiğiniz gibi sınıf yapıcılar isimsiz olduğu için çağırdığınızda ne tür bir nesne yaratılacağını kestirmeniz zordur, ancak geçtiğiniz parametrelere bakarak tahminde bulunabilirsiniz. Ancak statik bir fabrika metodu tanımlarsanız bu metoda istediğiniz ismi verip metodu kullanacak kişilere kolaylık sağlayabilirsiniz. Örneğin:

<pre><code>
public class Doner{
  public Doner(boolean soganli){
    //...
  }
}
 
//...
 
// Buradaki parametre (true) ne anlama geliyor? Açıp dokümantasyonu okumak lazım.
Doner doner = new Doner(true);
</code></pre>
Yukarıdaki gibi sınıf yapıcı kullanmak yerine statik fabrika metotları kullansak:


<pre><code>
public class Doner{
  public static Doner soganliDoner(){
    //...
  }
 
  public static Doner sogansizDoner(){
    //...
  }
}
 
// ...
 
// Bu kodu okumak çok daha kolay!
Doner doner = Doner.soganliDoner();
Doner doner = Doner.sogansizDoner();
</code></pre>

Statik fabrika metotlarının ikinci avantajı, her çağrıldığında yeni bir nesne yaratmak zorunda olmamasıdır. Bu avantaj size sınıf içerisinde daha önceden yaratılmış nesneleri ön bellekten (cache) döndürmenize olanak sağlar ve böylece aynı nesneden tekrar tekrar yaratmak zorunda kalmazsınız. Eğer nesneler sıklıkla yaratılıyorsa ve/veya bu nesneleri yaratmak pahalıysa, tekrardan aynı nesneyi yaratmak yerine önceden yaratılmış nesneleri kullanmak size ciddi oranda performans artışı sağlayacaktır. Yukarıda örnek olarak verdiğimiz Boolean.valueOf(boolean) metodu da bu tekniği kullanmaktadır.

Statik fabrika metotlarından döndürülecek nesneyi kontrol altında tutabilmek, o sınıfa çalışma zamanında (runtime) hangi nesneden kaç tane tutulacağını belirleme imkanı sağlar. Örnek vermek gerekirse, bu yöntemi kullanarak aşağıdaki gibi singleton (sadece bir defa yaratılan) nesneler oluşturabilirsiniz. (multithreading problemlerini bir kenara koyarsak..)

<pre><code>
public class Singleton {
   private static Singleton instance = null;
   private Singleton() {
      // Bu sınıftan nesne yaratılamaz
   }
   public static Singleton getInstance() {
      if(instance == null) {
         instance = new Singleton();
      }
      return instance;
   }
}
</code></pre>

Statik fabrika metotları kullanmanın üçüncü avantajı bu metotlardan alt türlere (subtype) ait nesneler de döndürebilmektir. Bu durum size döndürülecek nesnenin türünü belirlemede esneklik ve kapsülleme (encapsulation) imkanı sağlar. API tasarlayan yazılımcılar bu özelliği sıklıkla kullanır ve arayüz tabanlı (interface-based) API yazarlar. Bunu yapmak için statik fabrika metodunun dönüş değerini bir arayüz türü olarak belirlemek ve metot içerisinde bu arayüzü gerçekleştiren (implementation) bir gerçek nesne döndürmek yeterlidir. Böylece sizin yazdığınız kütüphaneyi kullanacak bir yazılımcı arayüz türünü gerçekleştiren bir nesne döneceğini bilir, ama siz arka tarafta o arayüzü gerçekleştiren herhangi bir nesneyi döndürebilir, hatta duruma göre bu nesneyi kullanıcıları etkilemeden değiştirebilirsiniz. Java Collections kütüphanesi bu duruma çok güzel bir örnektir. Collections sınıfı içerisinde çok sayıda statik fabrika metodu bulunmaktadır.

Bu kullanıma başka bir güzel örnek ise Enum türleridir. EnumSet sınıfı içerisinde public bir sınıf yapıcı yoktur, onun yerine statik fabrika metotları kullanılmıştır. Bu statik metotlar kullanılan Enum türüne göre iki farklı nesne döndürebilir: eğer Enum türü 64 veya daha az elemana sahipse RegularEnumSet, 65 veya daha fazla elemana sahipse JumboEnumSet nesnesi döner. Ancak biz bu sınıfı kullanan kişiler olarak bu ayrımın farkında bile olmayız çünkü bizim beklediğimiz dönüş değeri EnumSet türündedir. Dolayısıyla bu sınıfı kalıtan herhangi bir türün dönmesi bizim için yeterlidir, arkada ne olup bittiğiyle ilgilenmeyiz. Bu durum EnumSet sınıfını tasarlayan yazılımcılara büyük bir esneklik sağlar, çünkü bir sonraki versiyonda istedikleri taktirde yine EnumSet sınıfını kalıtan yeni bir nesne döndürebilir veya RegularEnumSet kullanmaktan vazgeçebilirler. Böyle bir değişiklikten bizim kodumuz etkilenmeyecektir.

Bu konsept çok önemli olduğu için bir örnekle açıklamakta fayda var. JDBC gibi servis sağlayıcı kütüphanelerde (service provider framework) bu yöntemin nasıl kullanıldığını inceleyelim. Bu kütüphanelerde servis sağlayıcılar bir servis arayüzünü gerçekleştirirler ve kayıt arayüzünü kullanarak sisteme kayıt olurlar. Sistem de bu sınıfları statik fabrika metotları aracılığıyla size sunar. Şimdi bu sistemin taslak koduna bakalım:

<pre><code>
// Servis arayüzü
public interface Service {
    // Servisle alakalı metotlar buraya..
}
 
// Servis sağlayıcı arayüzü
public interface Provider {
    Service newService();
}
 
// Servis kayıt ve erişimi için kullanılan sınıf
public class Services {
 
    private Services() { 
        // Bu sınıftan nesne yaratılamaz
    }  
 
    // Servis isimleriyle sağlayıcıların eşleştirilmesi
    private static final Map<String, Provider> providers = new ConcurrentHashMap<String, Provider>();
    public static final String DEFAULT_PROVIDER_NAME = "<def>";
 
    // Servis sağlayıcı kayıt arayüzü
    public static void registerDefaultProvider(Provider p) {
        registerProvider(DEFAULT_PROVIDER_NAME, p);
    }
     
    public static void registerProvider(String name, Provider p){
        providers.put(name, p);
    }
     
    // Servis erişim arayüzü
    public static Service newInstance() {
        return newInstance(DEFAULT_PROVIDER_NAME);
    }
     
    public static Service newInstance(String name) {
        Provider p = providers.get(name);
        if (p == null) {
            throw new IllegalArgumentException("No provider registered with name: " + name);
        }
        return p.newService();
    }
}
</code></pre>

Yukarıda gördüğünüz gibi servis erişim arayüzü bizim gönderdiğimiz isim parametresine göre bir servis sağlayıcıyı kullanarak bir servis yaratıp onu döndürüyor. Biz kullanıcı olarak bu sisteme dinamik olarak yeni servis sağlayıcılar kaydedip, erişim arayüzünden de yeni kaydettiğimiz servislere erişebiliriz. Bu sistemin tamamen dinamik olarak geliştirilebilmesi için tasarımın arayüz tabanlı (interface-based) yapılması gerekmektedir.

Şimdi gelelim statik fabrika metotlarının Java 1.6 ve önceki versiyonlarında sağladığı başka bir avantaja. Eğer generic bir sınıftan bir nesne yaratıyorsanız aşağıdaki gibi yazmanız gerekir:


<pre><code>
Map<String, List<String>> m = new HashMap<String, List<String>>();
</code></pre>
Bir kere Map içerisinde tür parametlerini tanımlamışken HashMap içerisinde tekrar tanımlamak zorunda olmanız çok hoş bir durum değil. Bu durumu aşağıdaki yöntemle aşabilirsiniz:


<pre><code>
public static <K, V> HashMap<K, V> newInstance() {
    return new HashMap<K, V>();
}
 
Map<String, List<String>> m = HashMap.newInstance();
</code></pre>
Ancak daha önce de belirttiğim gibi bu sorun Java 1.7 ile giderildi. (type inference). Yani JDK 1.7 ve üzeri versiyonlarda aşağıdaki kodu başarılı bir şekilde derleyebilirsiniz:


<pre><code>
Map<String, List<String>> m = new HashMap<>();
</code></pre>
Statik fabrika metotlarının hep avantajlarından bahsettik, hiç kötü tarafı yok mu diyecek olursanız onlardan da bahsedelim. Mesela bir sınıfa sadece statik fabrika metotları koyup public veya protected bir sınıf yapıcı koymazsanız o sınıfı kalıtamazsınız. Ancak biraz düşününce bu bir avantaja da dönüşebilir çünkü bu durum yazılımcıları kalıtım yerine kompozisyon kullanmaya yöneltir, bu da genel olarak faydalı bir programlama alışkanlığıdır. (…daha fazla bilgi)

İkinci bir dezavantaj da statik fabrika metotlarının klasik statik metotlardan bir bakışta ayırt edilememesi sayılabilir. Yani bir sınıf dökümantasyonuna bakarken statik bir metot nesne yaratmak için mi kullanılmış anlamak için kodu okumanız gerekebilir. Bunu aşmak için de güzel dokümantasyon yapmak ve anlaşılabilir isimler kullanmak faydalı olacaktır. Mesela Java’da sıklıkla kullanılan bazı statik fabrika metot isimleri şöyle sıralanabilir:

valueOf: Parametre olarak geçtiğiniz değere sahip olan bir nesne döndürür.
of: valueOf için kısaltılmış bir alternatif. EnumSet sınıfı çokça kullanır.
getInstance: Parametrelerle tanımlanmış bir nesne döndürür ancak değerinin parametrelerle aynı olması gerekmez. Singleton bir sınıfsa parametre almaz ve singleton nesneyi döndürür.
newInstance: getInstance gibidir ancak her çağırdığınızda farklı bir nesne döner.

Özetleyecek olursak, tabi ki her durumda statik fabrika metotları kullanmalıyız gibi birsey söyleyemeyiz. Ancak sınıf yapıcı kullandığınız birçok durumda aslında statik fabrika metotlarıyla daha okunabilir ve kaliteli kodlar yazmanız mümkün olabilir. Dolayısıyla nesne yaratırken elinizde bir değil iki seçenek olduğunu unutmayın, ve iki kere düşünmeden kod yazmayın.


<pre><code>
</code></pre>
