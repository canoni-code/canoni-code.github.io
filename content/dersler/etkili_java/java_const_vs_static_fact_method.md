+++
categories = ['Development', 'VIM']
date = '2023-15-12'
description = "Java'da bir sınıfı oluştururken, genellikle bir yapıcı(constructor) kullanılır. Ancak, bazı durumlarda static factory metodların kullanımı önemli avantajlar sağlayabilir.."
slug = 'static-factory-method-avantajları'
tags = ['Java', 'constructor', 'static factory methods']
title = "Java'da Static Factory Metodları ve Constructor'lar Arasındaki Fark"
+++

### Yapıcı (Constructor) Nedir?

Yapıcı(constructor), bir sınıfın nesnesinin oluşturulduğu anı temsil eden özel bir metoddur. Bu metodun adı sınıfın adıyla aynı olmak zorundadır ve nesne oluşturulurken çağrılır. Constructor, sınıfın özelliklerini başlatmak, nesnenin ilk durumunu belirlemek ve diğer başlangıç ayarlarını yapmak için kullanılır. Her bir nesne için ayrı ayrı çalışır ve nesnenin oluşturulma sürecini yönetir.

### Statik Fabrika Metodu(Static Factory Method) Nedir?

Statik fabrika metodu, bir sınıfın bir nesnesini oluşturmak için kullanılan özel bir metoddur. Bu metot, genellikle sınıfın bir örneği oluşturulmadan çağrılabilir. Yani, bu metotla bir nesne oluşturulduğunda, normal bir constructor çağrılmaz. Statik fabrika metotları, nesne oluşturma sürecini kontrol etmek, özel durumları ele almak, önbellekleme yapmak veya nesne havuzları oluşturmak gibi durumlar için kullanılabilir.

Java'da bir sınıfı oluştururken, genellikle bir yapıcı(constructor) kullanılır. Ancak, bazı durumlarda statik fabrika metotların kullanımı önemli avantajlar sağlayabilir.

### Constructor Kullanımı

Aşağıdaki örnekte, `Person` sınıfı için bir yapıcı(constructor) kullanılmıştır. Ancak, constructor'lar bazı durumlarda yetersiz veya esnek olmayabilir.

```Java
public class Person {
    private String name;
    private int age;

public Person(String name, int age) {
    this.name = name;
    this.age = age;
    }
}
```
### Statik Fabrika Metotları (Static Factory Methods)

Aşağıdaki örnekte, `createPerson` adlı bir static factory metodu kullanılmıştır. Bu örnek, yukarıdaki Person sınıfının bir statik fabrika metodu ile nasıl uygulanabileceğini gösteriyor.

```Java
public class Person {
    private String name;
    private int age;

    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public static Person createPerson(String name, int age) {
        return new Person(name, age);
    } 
}
```

Statik fabrika metotlarının kullanımının avantajlarına yukarıdaki örnekleri baz alarak değinelim.

#### Avantajlar

1. **İsimlendirme Özgürlüğü**: Bir yapıcı(constructor) tanımlandığında ilgili sınıf ile aynı ismi taşımak zorundadır ancak statik fabrika metotlarını tanımlarken adını dilediğiniz gibi seçebilirsiniz. Bu, kullanımı daha açık ve anlaşılır kılabilir.

```Java
Person john = Person.createPerson("John", 30);
```
2. **Önbellekleme (Caching)**: Her çağrıldığında yeni bir nesne oluşturmak yerine, aynı parametrelerle çağrıldığında önceden oluşturulmuş bir nesneyi döndürebilir ve bu sayede önbellekleme yapabilirsiniz.

```Java
   private static final Map<String, Person> personCache = new HashMap<>();

   public static Person createPerson(String name, int age) {
       String key = name + "_" + age;
       if (personCache.containsKey(key)) {
           return personCache.get(key);
       } 
       else {
           Person person = new Person(name, age);
           personCache.put(key, person);
           return person;
       }
   }
```

3. **Alt Tür Oluşturma (Subtype Instantiation)**: Yaratılan nesnenin alt türünü döndürebilirsiniz. Bu, kullanımı daha esnek kılar.
```Java
      public static Person createStudent(String name, int age, String studentId) {
          return new Student(name, age, studentId);
      }
```
4. **Eksik Parametre Kontrolü**: Constructor ile nesne oluştururken eksik parametreleri kontrol etmek nispeten zordur. Ancak statik fabrika metodu bu durumu daha iyi ele alabilir.
```Java
     public static Person createPersonWithOptionalParams(String name, int age, String address) {
         Person person = new Person(name, age);
         if (address != null) {
             person.setAddress(address);
         }
         return person;
     }
```
5. **Singleton Desenini Uygulama**: Statik fabrika metodunu kullanarak singleton desenini kolayca uygulayabilirsiniz.
```Java
   private static final Person INSTANCE = new Person("Default", 0);
    
   public static Person getInstance() {
       return INSTANCE;
   }
```

### Sonuç
Java'da constructor kullanımı her zaman uygun olmayabilir. İhtiyaca bağlı olarak static factory metodlarını kullanmak, kodunuzun daha esnek, okunabilir ve ölçeklenebilir olmasını sağlayabilir. Her durumda, kullanılacak yöntemi seçerken proje gereksinimlerini ve kodunuzu daha iyi anlamak önemlidir.
