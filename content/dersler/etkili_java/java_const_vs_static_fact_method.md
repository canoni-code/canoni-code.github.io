
# Java'da Static Factory Metodları ve Constructor'lar Arasındaki Fark

Java'da bir sınıfı oluştururken, genellikle bir yapıcı(constructor) kullanılır. Ancak, bazı durumlarda static factory metodların kullanımı önemli avantajlar sağlayabilir.

## Constructor Kullanımı
Aşağıdaki örnekte, `Person` sınıfı için bir yapıcı(constructor) kullanılmıştır. Ancak, constructor'lar bazı durumlarda yetersiz veya esnek olmayabilir.

    public class Person {
        private String name;
        private int age;
    
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }
    }




## Statik Fabrika Metodları (Static Factory Methods)
Aşağıdaki örnekte, `createPerson` adlı bir static factory metodu kullanılmıştır. Bu örnek, yukarıdaki Person sınıfının bir static factory metod ile nasıl uygulanabileceğini gösteriyor.

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
Static factory method kullanımının avantajlarına yukarıdaki örnekleri baz alarak değinelim.


### Avantajlar

1.  **İsimlendirme Özgürlüğü**: Bir yapıcı(constructor) tanımlandığında ilgili sınıf ile aynı ismi taşımak zorundadır ancak static factory metodu tanımlarken adını dilediğiniz gibi seçebilirsiniz. Bu, kullanımı daha açık ve anlaşılır kılabilir.

    `Person john = Person.createPerson("John", 30);`


2.  **Önbellekleme (Caching)**: Her çağrıldığında yeni bir nesne oluşturmak yerine, aynı parametrelerle çağrıldığında önceden oluşturulmuş bir nesneyi döndürebilir ve bu sayede önbellekleme yapabilirsiniz.


       private static final Map<String, Person> personCache = new HashMap<>();
        public static Person createPerson(String name, int age) {
            String key = name + "_" + age;
            if (personCache.containsKey(key)) {
                return personCache.get(key);
            } else {
                Person person = new Person(name, age);
                personCache.put(key, person);
                return person;
            }
        }

3.  **Alt Tür Oluşturma (Subtype Instantiation)**: Yaratılan nesnenin alt türünü döndürebilirsiniz. Bu, kullanımı daha esnek kılar.


    public static Person createStudent(String name, int age, String studentId) {
        return new Student(name, age, studentId);
    }

4.  **Eksik Parametre Kontrolü**: Constructor ile nesne oluştururken eksik parametreleri kontrol etmek zordur. Ancak static factory metodu bu durumu daha iyi ele alabilir.


    public static Person createPersonWithOptionalParams(String name, int age, String address) {
        Person person = new Person(name, age);
        if (address != null) {
            person.setAddress(address);
        }
        return person;
    }

5.  **Singleton Desenini Uygulama**: Static factory metodunu kullanarak singleton desenini kolayca uygulayabilirsiniz.


    private static final Person INSTANCE = new Person("Default", 0);

    public static Person getInstance() {
        return INSTANCE;
    }


## Sonuç

Java'da constructor kullanımı her zaman uygun olmayabilir. İhtiyaca bağlı olarak static factory metodlarını kullanmak, kodunuzun daha esnek, okunabilir ve ölçeklenebilir olmasını sağlayabilir. Her durumda, kullanılacak yöntemi seçerken proje gereksinimlerini ve kodunuzu daha iyi anlamak önemlidir.