# Affinity Colocation

In many cases it is beneficial to colocate different entries if they are often accessed together. In this way, multi-entry queries are executed on one node (where the objects are stored). This concept is known as _affinity colocation_.

Entries are assigned to partitions by the affinity function. The objects that have the same affinity keys go to the same partitions. This allows you to design your data model in such a way that related entries are stored together. "Related" here refers to the objects that are in a parent-child relationship or objects that are often queried together.

For example, let’s say you have `Person` and `Company` objects, and each person has the `companyId` field that indicates the company the person works for. By specifying the `Person.companyId` and `Company.ID` as affinity keys, you ensure that all the persons working for the same company are stored on the same node, where the company object is stored as well. Queries that request persons working for a specific company are processed on a single node.

You can also colocate a computation task with the data.&#x20;

### Configuring Affinity Key

If you do not specify the affinity key explicitly, the cache key is used as the default affinity key. If you create your caches as SQL tables using SQL statements, the PRIMARY KEY is the default affinity key.

If you want to colocate data from two caches by a different field, you have to use a complex object as the key. That object usually contains a field that uniquely identifies the object in that cache and a field that you want to use for colocation.

There are several ways to configure a custom affinity field within the custom key, which are described below.

The following example illustrates how you can colocate the person objects with the company objects using a custom key class and the `@AffinityKeyMapped` annotation.

```java
public class AffinityCollocationExample {

    static class Person {
        private int id;
        private String companyId;
        private String name;

        public Person(int id, String companyId, String name) {
            this.id = id;
            this.companyId = companyId;
            this.name = name;
        }

        public int getId() {
            return id;
        }
    }

    static class PersonKey {
        private int id;

        @AffinityKeyMapped
        private String companyId;

        public PersonKey(int id, String companyId) {
            this.id = id;
            this.companyId = companyId;
        }
    }

    static class Company {
        private String id;
        private String name;

        public Company(String id, String name) {
            this.id = id;
            this.name = name;
        }

        public String getId() {
            return id;
        }
    }

    public void configureAffinityKeyWithAnnotation() {
        CacheConfiguration<PersonKey, Person> personCfg = new CacheConfiguration<PersonKey, Person>("persons");
        personCfg.setBackups(1);

        CacheConfiguration<String, Company> companyCfg = new CacheConfiguration<>("companies");
        companyCfg.setBackups(1);

        try (Ignite ignite = Ignition.start()) {
            IgniteCache<PersonKey, Person> personCache = ignite.getOrCreateCache(personCfg);
            IgniteCache<String, Company> companyCache = ignite.getOrCreateCache(companyCfg);

            Company c1 = new Company("company1", "My company");
            Person p1 = new Person(1, c1.getId(), "John");

            // Both the p1 and c1 objects will be cached on the same node
            personCache.put(new PersonKey(p1.getId(), c1.getId()), p1);
            companyCache.put("company1", c1);

            // Get the person object
            p1 = personCache.get(new PersonKey(1, "company1"));
        }
    }

}
```

You can also configure the affinity key field in the cache configuration by using the `CacheKeyConfiguration` class.
