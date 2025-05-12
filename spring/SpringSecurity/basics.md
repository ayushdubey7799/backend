
# Spring Security: Architecture, Components, and Configuration

- **Filter Chain:** Spring Security inserts a `DelegatingFilterProxy` into the web application’s filter chain, which delegates to a Spring bean named **springSecurityFilterChain**. This bean is a `FilterChainProxy` that wraps one or more **SecurityFilterChain** instances. Each `SecurityFilterChain` holds a list of security `Filter` beans (CSRF, authentication, authorization filters, etc.) and a `RequestMatcher` for URL patterns. All HTTP requests pass through this filter chain in a fixed order (as defined by Spring Security).

- `FilterChainProxy` (behind `DelegatingFilterProxy`) is Spring Security’s core filter that orchestrates security processing. It is a Spring bean that delegates each request to the first matching `SecurityFilterChain`. Only the filters in that matching chain are applied to the request. For example, if multiple `SecurityFilterChain` beans are defined (each with different path matchers), the proxy tests each in order and invokes the first one whose matcher matches the request.

- The **filter execution order** is predefined to ensure correct semantics. For example, Spring Security’s common filters run in this sequence: `CsrfFilter`, `LogoutFilter`, `UsernamePasswordAuthenticationFilter`, `BasicAuthenticationFilter`, `DefaultLoginPageGeneratingFilter`, `DefaultLogoutPageGeneratingFilter`, `FilterSecurityInterceptor`, etc. (See the official reference for the complete ordering.) Spring Security automatically handles ordering so developers rarely need to specify it.

- **SecurityContextHolder / SecurityContext:** Spring stores the current `Authentication` (the security principal and authorities) in a `SecurityContext`, which by default is held in a `ThreadLocal` (managed by `SecurityContextHolder`). The `SecurityContextPersistenceFilter` (one of the first filters) loads the `SecurityContext` (typically from the HTTP session) at the start of request processing, and saves/clears it at the end. In particular, `FilterChainProxy` ensures the context is cleared out after each request to prevent ThreadLocal leaks.

- **AuthenticationManager / ProviderManager / AuthenticationProvider:** Authentication processing is performed by an `AuthenticationManager`. The common implementation is `ProviderManager`, which holds a list of `AuthenticationProvider`s. Each `AuthenticationProvider` knows how to authenticate a specific type of credentials (for example, `DaoAuthenticationProvider` handles username/password, `JwtAuthenticationProvider` handles JWTs). When a filter creates an `Authentication` token (e.g. `UsernamePasswordAuthenticationToken`), it is passed to the `ProviderManager`, which delegates to each `AuthenticationProvider` in turn. On success, the resulting authenticated `Authentication` is returned and set into the `SecurityContextHolder` by the filter.

- **Key authentication classes:** `UserDetailsService` is the core interface for loading user data (username, password, authorities) from a datastore. A `DaoAuthenticationProvider` uses a `UserDetailsService` and a `PasswordEncoder` to authenticate username/password submissions. On successful authentication, a `DaoAuthenticationProvider` returns a `UsernamePasswordAuthenticationToken` whose principal is the loaded `UserDetails`, and its authorities (roles) are those granted to the user. Authorities (represented by `GrantedAuthority`) are typically roles/scopes and are loaded into the `Authentication` by the `UserDetailsService`. For example, `ROLE_USER` or `ROLE_ADMIN` might be granted, which are then checked by authorization filters.

- **AuthenticationEntryPoint / ExceptionTranslationFilter:** When a request requires authentication (or access) but the user is not authenticated, Spring triggers the configured `AuthenticationEntryPoint`. In practice, an `ExceptionTranslationFilter` catches `AuthenticationException` or `AccessDeniedException`. For an unauthenticated user, it first **clears** the `SecurityContextHolder`, then invokes the `AuthenticationEntryPoint` (for example, redirecting to a login page, or sending an HTTP 401 with `WWW-Authenticate`). This is how Spring switches from normal request processing into the login challenge.

## Configuration Styles

### Java Configuration (Annotation-based)

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(Customizer.withDefaults())
            .httpBasic(Customizer.withDefaults())
            .formLogin(Customizer.withDefaults())
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
        return http.build();
    }
}
```

This setup auto-registers the `springSecurityFilterChain` and secures all URLs, enabling form login and HTTP Basic by default.

### XML Namespace Configuration

```xml
<http>
  <intercept-url pattern="/**" access="hasRole('USER')" />
  <form-login />
  <logout />
</http>
<authentication-manager>
  <authentication-provider>
    <user-service>
      <user name="admin" password="{noop}pass" authorities="ROLE_USER"/>
    </user-service>
  </authentication-provider>
</authentication-manager>
```

### FilterChain configuration

Regardless of style, all configurations result in a `FilterChainProxy` bean with the filters defined in the given order.

## Authentication Flows

### Form Login

Spring Security:
1. Reads credentials from the login form
2. Authenticates via `AuthenticationManager`
3. On success, stores in `SecurityContextHolder` and redirects
4. On failure, redirects to the login page

### HTTP Basic

1. Sends `WWW-Authenticate: Basic` on 401
2. Client resends with `Authorization: Basic <credentials>`
3. Authenticated by `BasicAuthenticationFilter`

### JWT / Bearer Token (Resource Server)

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://idp.example.com
```

```java
http.oauth2ResourceServer(rs -> rs.jwt());
```

### OAuth2 Login (Authorization Code)

- Starts with redirect to provider’s login
- Gets code, exchanges for token
- Loads user profile from provider
- Sets user into `SecurityContextHolder`

## Security Attack Protections

- **CSRF:** Enabled by default. Requires CSRF token on state-changing requests.
- **XSS:** Headers like `X-Content-Type-Options: nosniff`, `X-XSS-Protection: 0`
- **Clickjacking:** Header `X-Frame-Options: DENY`
- **Session Fixation:** Automatically handled by changing session ID on login
- **Brute Force:** Not implemented out-of-the-box; must be custom coded

## Examples

### Java DSL

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .csrf(Customizer.withDefaults())
        .httpBasic(Customizer.withDefaults())
        .formLogin(Customizer.withDefaults())
        .authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
    return http.build();
}
```

### XML Example

```xml
<http>
  <intercept-url pattern="/**" access="hasRole('USER')" />
  <form-login />
  <logout />
</http>
```

### Add Custom Filter in Java

```java
http.addFilterBefore(myFilter, UsernamePasswordAuthenticationFilter.class);
```
