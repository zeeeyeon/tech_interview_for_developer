[Spring] Annotation

## 어노테이션만 붙여도 코드가 실행되는 이유
<hr>

1. 어노테이션 붙인 클래스 작성 → 예: @Component, @Service, @Controller, @Autowired, @Transactional

2. 스프링 실행 → @ComponentScan이 작동 → 클래스패스를 스캔해 어노테이션 붙은 클래스를 찾음
```
// SpringBootApplication 안에 이미 포함됨
@ComponentScan(basePackages = "com.example")
```
3. 어노테이션이 붙은 클래스를 Bean으로 생성하여 ApplicationContext에 등록 → DI 컨테이너가 객체를 대신 생성하고 관리
```
// AbstractApplicationContext 내부에서
beanFactory.registerBeanDefinition(beanName, beanDefinition);
```
4. BeanPostProcessor가 작동 → 등록된 Bean 중 특정 어노테이션이 붙은 Bean을 감지
```
public PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName) {
    InjectionMetadata metadata = findAutowiringMetadata(beanName, bean.getClass(), pvs);
    metadata.inject(bean, beanName, pvs); // 실제 주입
    return pvs;
}
```
5. 어노테이션에 따라 필요한 동작을 삽입 → 리플렉션으로 필드에 값을 주입하거나, → 프록시로 감싸 메서드 실행 전/후 로직을 삽입
```
Field[] fields = bean.getClass().getDeclaredFields();
for (Field field : fields) {
    if (field.isAnnotationPresent(Autowired.class)) {
        // 주입할 객체를 찾아서 주입
        Object dependency = beanFactory.getBean(field.getType());
        field.setAccessible(true);
        field.set(bean, dependency);
    }
}
```

```
@Override
public Object postProcessAfterInitialization(Object bean, String beanName) {
    // 프록시 감싸기
    if (bean has @Transactional method) {
        return createProxy(bean); // CGLIB or JDK Proxy
    }
    return bean;
}
```
6. 결과적으로 어노테이션만 붙였는데도 → 코드가 “자동으로 실행된 것처럼” 보이게 됨


## Reflection
<hr>

- 런타임에 클래스, 메서드, 필드 등의 정보를 동적으로 조회하고 조작하는 기능
- 리플렉션으로 어노테이션 감지 예시 (이 방식으로 Spring은 모든 필드/메서드/생성자에 대해 어노테이션이 붙었는지 일일이 검사해서 처리)
- 필요시 전략적으로 사용 (초기 한 번만 사용 후, 캐싱하여 사용)
```
Field field = MyService.class.getDeclaredField("myRepository");

if (field.isAnnotationPresent(Autowired.class)) {
    System.out.println("Autowired 어노테이션이 붙었음");
}
```

```
public PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName) {
    InjectionMetadata metadata = findAutowiringMetadata(beanName, bean.getClass(), pvs);
    metadata.inject(bean, beanName, pvs); // 여기서 실제 주입
    return pvs;
}

public void inject(Object target, @Nullable String beanName, @Nullable PropertyValues pvs) throws Throwable {
    for (InjectedElement element : this.injectedElements) {
        element.inject(target, beanName, pvs); // 하나하나 리플렉션으로 주입
    }
}

protected void inject(Object target, @Nullable String requestingBeanName, @Nullable PropertyValues pvs) throws Throwable {
    Field field = (Field) this.member;
    Object value = beanFactory.resolveDependency(...); // 주입할 Bean 찾기
    field.setAccessible(true);  // private 필드 접근 허용
    field.set(target, value);   // 리플렉션으로 주입
}
```