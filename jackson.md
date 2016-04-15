* To serialize hibenate proxied values:
1. Add this dependency:
        <dependency>
            <groupId>com.fasterxml.jackson.datatype</groupId>
            <artifactId>jackson-datatype-hibernate4</artifactId>
            <version>2.6.4</version>
        </dependency>

2. When instantiating ObjectMapper:
        ObjectMapper mapper = new ObjectMapper();
        mapper.registerModule(new Hibernate4Module());

* To support filters:
1. Add @JsonFilter('filterId') to the class you want to serialize
2.
  a. When using ObjectMapper apply the filter
  b. When within Spring Rest controllers:
        MappingJacksonValue mappingJacksonValue = new MappingJacksonValue(instance);
        FilterProvider filters = new SimpleFilterProvider()
                .addFilter(filter, SimpleBeanPropertyFilter.filterOutAllExcept(properties));
        mappingJacksonValue.setFilters(filters);

        return mappingJacksonValue;

