# Spring Security - Lambda DSL

### What is Lambda DSL
**Lambda DSL** (Domain Specific Language) in the context of Spring Security refers to the usage of lambda expressions to configure security rules and filters. This style of configuration allows you to define security configurations using lambda functions, providing a more concise and expressive way to set up security policies.

context = প্রসঙ্গ  
In the context = প্রেক্ষাপটে  
In the context of = এর প্রেক্ষাপটে  
<br><br>

## Configuration using lambdas
```
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests(authorizeRequests ->
                authorizeRequests
                    .antMatchers("/blog/**").permitAll()
                    .anyRequest().authenticated()
            )
            .formLogin(formLogin ->
                formLogin
                    .loginPage("/login")
                    .permitAll()
            )
            .rememberMe(withDefaults());
    }
}
 ```
<br>

## Equivalent configuration without using lambdas
```
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/blog/**").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .rememberMe();
    }
}
```
<br> <br>
## WebFlux Security
```
@EnableWebFluxSecurity
public class SecurityConfig {

    @Bean
    SecurityWebFilterChain springSecurityFilterChain(ServerHttpSecurity http) {
        http
            .authorizeExchange(exchanges ->
                exchanges
                    .pathMatchers("/blog/**").permitAll()
                    .anyExchange().authenticated()
            )
            .httpBasic(withDefaults())
            .formLogin(formLogin ->
                formLogin
                    .loginPage("/login")
            );
        return http.build();
    }
}
```

## Lambda DSL configuration tips
When comparing the two samples above, you will notice some key differences:

* In the Lambda DSL there is no need to chain configuration options using the `.and()` method. The `HttpSecurity` instance is automatically returned for further configuration after the call to the lambda method.
* `withDefaults()` enables a security feature using the defaults provided by Spring Security. This is a shortcut for the lambda expression `it -> {}`.
<br><br>
## Goals of the Lambda DSL
The Lambda DSL was created to accomplish to following goals:
- Automatic indentation makes the configuration more readable.
- The is no need to chain configuration options using `.and()`.
- The Spring Security DSL has a similar configuration style to other Spring DSLs such as Spring Integration and Spring Cloud Gateway.
