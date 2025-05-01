# Login e controle de acesso
# TEORIA

## Ideia Geral
### Login
- Usuário envia as credenciais *usuário/senha* → servidor valida → gera um token → envia de volta para o cliente.
  
![image](https://github.com/user-attachments/assets/8e6bc348-db2f-4926-8f3b-2057bb38c467)

### Acesso a um recurso protegido
- Cliente envia **token** junto com o pedido → servidor valida o token → libera ou bloqueia o acesso.
  
![image](https://github.com/user-attachments/assets/101da7ba-2c70-4718-b882-c64d4e3b12e1)

### TOKEN
- Um token é uma string que representa a identidade do usuário de forma segura
- Em vez do cliente enviar usuário/senha toda vez, ele envia esse token para provar que está autenticado.
#### Como o Token é gerado
- Depois que o usuário realiza o login e é validado, o servidor gera o token;
- Existem vários tipos, mas no Spring Boot geralmente é utilizado o **JWT** (*Json web Token*)
### JWT
JWT é divido em três partes:
- **HEADER:** Informações sobre o tipo de token e o algoritimo de assinatura
- **PAYLOAD:** Informações(claims) sobre o usuário, como ID, roles, tempo de expiração, etc..
- **SIGNATURE:** Assinatura digital para garantir que o token não foi auterado.

#### Exemplo de token(ficticio)

```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9. 
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ. 
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
## OAuth 2.0

### Visão geral do OAuth2

![image](https://github.com/user-attachments/assets/f56ccbb5-45bd-40a4-ac36-a5ddf4c007cc)

- É um protocolo de autorização (não autenticação pura)
- Permite que aplicações acessem recursos de usuários em outros servidores sem expor as credenciais (usuário/senha)
- Separa autenticação e autorização entre dois servidores diferentes, são eles:
  - **Authorization Server:** Quuem autentica o usuário (verifica que ele é emite tokens
  - **Resource Server:** Quem protege os dados e exige tokenbs validos para liberar acesso

### Fluxo geral no OAuth 2.0 (simplificado)
1. O usuário quer acessar um recurso protegido numa aplicação (ex: Google Drive).
2. A aplicação redireciona o usuário para o Authorization Server (ex: servidor do Google) para login.
3. O Authorization Server autentica o usuário e, se tudo der certo, emite um Access Token.
4. O cliente (aplicação) usa o Access Token para acessar o Resource Server (o recurso desejado).
5. O Resource Server valida o token e retorna o recurso se tudo estiver OK.

# PRÁTICA

## Modelo de dados User-Role

![image](https://github.com/user-attachments/assets/b95f18af-726f-4b1a-b378-6a75a866ffb6)

## Dependências Spring Security

```Bash
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```

## Liberando provisoriamente os endpoints
```Bash
@Configuration
public class SecurityConfig {

	@Bean
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		http.csrf(csrf -> csrf.disable());
		http.authorizeHttpRequests(auth -> auth.anyRequest().permitAll());
		return http.build();
	}
}
```

## Referência

 - [JWT](https://jwt.io/)
 - [Códigos de status de respostas HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status)
 - [OAuth 2.0](https://oauth.net/2/)
 - [BCryptPasswordEncoder](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/crypto/bcrypt/BCryptPasswordEncoder.html)
