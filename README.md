### Cmp Commons Log ###

Componente encarregado de interceptar as chamadas feitas pelas classes com anotações RestController e logar mensagens de tipo AUDIT / INFO / ERROR.
A implementação do componente foi baseada na programação orientado a aspectos para registrar o log cada vez que o um método endpoint é 
chamado. Também foi utilizado Annotation de tempo de execução para definir os parâmetros que vão ser considerados no log. 

O tipos de logs registrados são:

O Log [AUDIT] e registrado no início do método.
O Log [RESULT] e registrado no retorno do método.
O Log [ERROR] e registrado quando é lançado do método.

Em todos os tipos de Log vai considerar logar os parâmetros que estejam com a anotação @LogParam

E os métodos públicos que possuem suas classes com a anotação RestController serão registrados logs a cada ação ocorrida.

## Getting Started ##

Para configurar este componente num projeto que tenha a necessidade de registrar os Logs em um arquivo centralizado de logs, é necessário primeiramente adicionar estas dependências necessárias no arquivo pom.xml (cmp-commons-log e spring-boot-starter-aop).

Após as dependências estarem devidamente importadas, a classe Application do projeto em questão deve ter a referência a esta nova dependência na anotação ComponentScan. 

E por fim, as classes que possuem a necessidade de serem logadas deve ter a anotação do novo componente de Log (LogParam) para que os parâmetros sejam interceptados na ação do método e registrados no log.

### Requirements ##

Versão do JDK: 8
Utilização de uma IDE para desenvolvimento Java
O projeto que deverá registrar logs deve ser do tipo Maven e Spring Boot(dependências do Spring devidamente configuradas no pom.xml).

### Installing ##

Primeiro, deve-se adicionar estas dependências (cmp-commons-log e spring-boot-starter-aop) ao pom.xml 

Exemplo:

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>

<dependency>
  <groupId>br.com.itau.cmp</groupId>
  <artifactId>cmp-commos-log</artifactId>
  <version>0.0.1</version>
</dependency>

Agora deve-se adicionar a referência ao componente na classe SpringApplication(que possui o método main / run) 
Exemplo :

@SpringBootApplication
@ComponentScan("br.com.itau.cmp.commons.log.aspect")
public class DemoApplication {
  public static void main(String[] args){
    SpringApplication.run(DemoApplication.class, args);
  } 
}

E por fim deve adicionar a anotação LogParam na classe que irá registrar os logs para que parâmetros necessários a serem logados sejam interceptados pelo novo componente e gerados os logs corretamente.


Exemplo:

@RequestMapping(method = RequestMethod.GET)
public ResponseEntity<?> createTenant(@LogParam(key = "TenantCreationResouce", method = "name") @Valid @RequestBody TenantCreationResource resource){
...
Obs: Neste primeiro exemplo notamos que são passado 2 parâmetros pela anotação LogParam. O parâmetro key recebe o valor "TenantCreationResouce", que tem como finalidade de apenas registro no log, e o parametro method que recebe o valor de "name", que é o nome do atributo que consta na entidade TenantCreationResource informada no parâmetro do método, onde o componente se encarrega de fazer executar o método GET relacionado ao parâmetro informado.


@RequestMapping(method = RequestMethod.DELETE, value = "/{id}")
public ResponseEntity<?> deleteTenant(@LogParam(key = "tenantId") String tenantId){
...
Obs: Neste segundo exemplo notamos que apenas o parâmetro key foi chamado, onde este parâmetro (a String tenantId) será registrado no log e seu valor passado por parâmetro, interceptado e registrado no log.
