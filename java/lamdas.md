# Lambdas

## method references

Function<Person, Integer> f = person -> person.getAge();
Function<Person, Integer> f = Person::getAge ;

Consumer<String> printer = s -> System.out.println(s);
Consumer<String> printer = s -> System.out::println;


**Consumer**
* accept
* can be chained (andThen())

**Supplier** get

**Function** apply

**Predicate** test
* can be chained (and(), or())