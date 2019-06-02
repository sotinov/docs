# Snippets
## Spring and factory beans
```java
@Configuration
public class TestConfig {
    Map<Class, Boolean> newInstcFlag = new HashMap<>();

    @Bean
    @Scope(value = "prototype")
    public Service service() {
        newInstcFlag.put(ServiceSub.class, true); //  return new instance
        return new Service() ;
    }

    private static <T> FactoryBean<T> mockBean(Class<T> cls, Map<Class, Boolean> m) {
        return new FactoryBean<T>() {
            T mockedObj;
            @Override public T getObject() {
                if(mockedObj == null || m.get(cls) == null || m.get(cls)) {
                    m.put(cls, false);
                    return mockedObj = Mockito.mock(cls);
                } else {
                    return mockedObj;
                }
            }
            @Override public Class<?> getObjectType() { return cls; }
            @Override public boolean  isSingleton  () { return false; }
        };
    }

    @Bean
    public FactoryBean<ServiceSub> serviceSubFactoryBean() {
        return mockBean(ServiceSub.class, newInstcFlag);
    }
}
```
