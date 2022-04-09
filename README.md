# eive-java-101-exam-3

1) No Maven o arquivo `pom.xml` é o principal arquivo de configuração de dependências. Sabendo disso leia as afirmativas abaixo.
   1) A dependências podem ser adicionadas diretamente na pasta lib do projeto e este após compilar continua reconhecendo.
   2) A dependências seguem um padrão de organização, sempre devem ser adicionadas na sessão `<dependencies>` do arquivo.
   3) Sempre que uma nova dependência é adicionada no projeto o `Mavem` identifica e baixa de forma automática sem que o desenvolvedor precise se preocupar.

   Assinale a alternativa que corresponde apenas as informações verdadeiras.
   - [ ] iii, apenas
   - [ ] i e ii
   - [ ] i e iii 
   - [ ] ii e iii
   - [X] i, ii e iii

2) No `Maven` existem alguns comandos que são conhecidos como `goal`. Sabendo disso leia as afirmativas abaixo.
   1) O comando `mvn test` tem como finalidade executar os testes do projeto
   2) O comando `mvn deploy production` é usado quando precisa gerar o artefado para o ambiente de produção
   3) `mvn install` é o goal que realiza a instalação do projeto no diretório local

   Assinale a alternativa que corresponde apenas as informações verdadeiras.
   - [ ] i, apenas
   - [ ] ii apenas
   - [ ] i e ii apenas
   - [X] i e iii apenas
   - [ ] ii e iii, apenas
 
3) Supondo que existe um banco de dados onde neste banco de dados existe uma tabela com o nome `Produto` e nesta tabela temos dois registros: 
   ```
   ID = 1
   NOME = TELEVISÃO
   DESCRICAO = TV 52"

   ID = 2
   NOME = RÁRIO
   DESCRICAO = RÁDIO AM/FM
   ```

    Dado o seguinte código, qual o resultado é impresso no console?

   ```java
   Statement stmt = connection.createStatement();
   stmt.execute("Select id, nome, descricao From Produto");
   ResultSet rst = stmt.getResultSet();

   while(rst.next()){
      System.out.print(rst.getInt("id"));
      System.out.print(rst.getString("nome"));
      System.out.print(rst.getString("descricao"));
      System.out.println("");
   }
   connection.close();
   ```

   ```
   1TELEVISÃOTV 52"
   2RÁRIORÁDIO AM/FM
   ```

4) Veja o comando abaixo, como ele poderia ser adaptado para imprimir o código gerado na inclusão do registro?
   ```java
   Statement stmt = connection.createStatement();
   stmt.execute("INSERT INTO Produto VALUES ('Cadeira', 'Cadeira 4 pernas')");
   ```

   ```java
   Statement stmt = connection.createStatement();
   stmt.execute("INSERT INTO Produto VALUES ('Cadeira', 'Cadeira 4 pernas')",
       Statement.RETURN_GENERATED_KEYS);

   ResultSet rst = stmt.getGeneratedKeys();
   rst.next();
   System.out.println(rst.getInt("ID"));
   ```

5) O que é um pool de conexão e qual sua vantagem? Explique.
   * De forma prática, o pool de conexões é um cache de conexões ao banco de dados. Esta prática é realizada para que seja possível aproveitar conexões para requisições posteriores.
   * A vantagem que temos ao utilizar dessa prática é controle de acesso ao banco garantindo sempre um bom desempenho e um uso adequado dos recursos

6) De um exemplo de uso de pool de conexão usando a biblioteca c3p0
   ```java
   import java.sql.Connection;
   import java.sql.SQLException;
   import javax.sql.DataSource;
   import com.mchange.v2.c3p0.ComboPooledDataSource;

   class ConnectionFactory {
       private DataSource dataSource;

       public ConnectionFactory() {
           ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();

           comboPooledDataSource.setJdbcUrl("JdbcUrl");
           comboPooledDataSource.setUser("User");
           comboPooledDataSource.setPassword("Password");

           this.dataSource = comboPooledDataSource;
       }

       public Connection getConnection() throws SQLException {
           return this.dataSource.getConnection();
       }
   }

   public class Main {
       public static void main(String[] args) {
           ConnectionFactory connectionFactory = new ConnectionFactory();
           try {
               Connection connection = connectionFactory.getConnection();
               // utilizar a conexão
               connection.close();
           } catch (SQLException e) {
               e.printStackTrace();
           }
       }
   }
   ```

7) Quais as desvantagens de usar o JDBC em uma aplicação?
   * Código é mais complexo e muito verboso
   * Alto acoplamento com o banco de dados

8) Explique de forma sucinta como funciona o ciclo de vida de uma aplicação JPA
   * Uma entidade JPA é criada no estado transiente, assim o JPA não gerencia suas modificações.
   * Se uma entidade no estado transiente é persistida ou é retornada do banco de dados, então o JPA passa a gerenciá-la, liberando acesso a funcionalidades de incluir, atualizar ou até mesmo remover.
      * Podemos sincronizar com o banco a informação ou até mesmo salvar definitivamente.
   * Quando limpamos ou fechamos a conexão com a entidade, estamos dizendo para o JPA que não deve mais gerenciar esta entidade, não será propagada as alterações e afins da mesma.
      * É possível retornar uma entidade que não está sendo gerenciada para que ela seja novamente gerenciada pelo JPA

9) Sobre JPA explique a diferença entre o uso da importação de `javax.persistence.Entity` e `org.hibernate.annotations.Entity`
   * A classe Entity do hibernate é uma implementação da especificação da Entity do Javax.
   * Utilizando a Entity do hibernate o código fica acoplado com o hibernate, já utilizando da Entity do javax temos a possibilidade de trocar o hibernate por outro framework sem grandes alterações.

10) Dado a estrutura de uma tabela `Carros`, escreva como seria o mapeamento em JAVA usando o JPA 
    ```SQL
    Create Table Cores(
      ID_COR INT auto_increment,
      NOME VARCHAR(50),
      PRIMARY KEY(ID_COR)
    );

    Create Table Carros(
      ID_CARRO INT auto_increment,
      MARCA VARCHAR(50),      
      MODELO VARCHAR(50),
      PLACA VARCHAR(10),
      ID_COR INT,
      PRIMARY KEY(ID_CARRO)
    );

    ALTER TABLE Carros ADD FOREIGN KEY(ID_COR) REFERENCES Cores (ID_COR);
    ```

    ```java
    //Imports
    
    @Entity
    @Table(name = "Carros")
    public class Car {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        @Column(name = "id_carro")
        private Integer id;

        @Column(name = "marca")
        private String brand;

        @Column(name = "modelo")
        private String model;
        
        @Column(name = "placa")
        private String plate;
        
        @ManyToOne
        @Column(name = "id_cor")
        private Color color;

        //getters and setters
    }
    ```

11) Sobre o ciclo de vida de uma Entidade, explique cada um dos estados `TRANSIENT`, `MANAGED` e `DETACHED`.
    * `TRANSIENT` Estado da entidade onde o JPA ainda não gerencia, é necessário persistir a entidade para manipulação no JPA
    * `MANAGED` Estado da entidade controlado pela JPA, onde inserções, atualizações e remoções são registradas na classe.
    * `DETACHED` Estado da entidade não controlado pela JPA, qualquer alteração realizada na entidade neste estado não efetará o banco de dados.

12) Qual a finalidade do método `merge()` no JPA. Escreva um exemplo simples para mostrar seu uso.
    * O método merge() é utilizado para possibilitar que uma entidade no estado `Detached` retorne ao estado `Managed`
    ```java
    EntityManagerFactory factory = Persistence.createEntityManagerFactory("persistenceUnit");
    EntityManager em = factory.createEntityManager();

    Color color = new Color("Vermelho");

    em.persist(color);
    em.close();

    color = em.merge(color);
    color.setName("Red");
    ```

13) Usando a sintaxe JPQL como poderia ser adaptado o código abaixo para listar todos os dados da tabela `Carros`
    ```java
    public Lista<Carro> findAll(){
        return null;
    }
    ```

    ```java
    public Lista<Carro> findAll(){
        String consulta = "SELECT c FROM Carro c";
        return em.createQuery(consulta, Carro.class).getResultList();
    }
    ```

14) Usando a sintaxe JPQL avalie o código abaixo e sinalize quais pontos estão errados no código
    ```java
    public Lista<Pessoa> findById(Long id) {
        String consulta = "Select * from Pessoas P Where P.ID_PESSOA = ?";
        entityManager.createStatement(consulta);
        return entityManager.createQuery(consulta, Pessoa.class)
           .setParameter(1, 1)
           .getResultList(); 
    }
    ```
    * Lista possívelmente está escrita errada, deve ser List.
    * findById remete a um unico objeto, não há necessidade de retornar uma lista.
    * Informado o `*` ao invés do alias P.
    * Não utilizamos `?` como indicador de parametros.
    * A especificação EntityManager não possui o método createStatement.
    * Apesar de funcionar `setParameter` por posição é recomendado utilizar o nome do parametro.
    * `getResultList` neste cenário considerando que não irá retornar uma lista pode ser alterado para `getSingleResult`.

15) Qual a função do `mappedBy` e onde ele deve ser usado?
    * Indicar que um relacionamento já possui mapeamento em outra entidade, conhecemos isso como relacionamento bidirecional.
    * Deve ser usado em um relacionamento bidirecional no qual não queremos gerar a tabela.

16) Escreva um exemplo de como fazer um `JOIN` no JPQL?
    ```java
    public Cor findCorPorCarro(){
        String consulta = "SELECT Cor " +
                          "  FROM Carro" +
                          "  JOIN Carro.Cor Cor" +
                          " WHERE Carro.id_carro = :id_carro;
        return em.createQuery(consulta, Carro.class).getResultList();
    }
    ```

17) Qual a função do `select new` no JPA?
    * Criar um retorno especifico para consulta sem ser uma entidade
    * Um conceito muito utilizado para retornar arquivos VO

18) Considerando que no seu projeto tem a seguinte estrutura de pacotes:
    ```
    br.com.curso.modelo
         Carro.java
         Cores.java
    br.com.curso.dao
         CarroDao.java
         CoresDao.java
    ```

    Escreva um método que exemplifique uma consulta usando `select new` do JPA, descreva se precisa de novas classes e se for preciso onde cada uma delas deve ser organizada no projeto seguindo as boas práticas.
    ```
    Criar uma classe VO para receber o retorno da consulta
    
    br.com.curso.vo.relatorioCarroPorMarca.java
    ```
    ```java
    public List<relatorioCarroPorMarca> relatorioCarroPorMarca(String marca) {
        String consulta = "SELECT new br.com.curso.vo.relatorioCarroPorMarca( "+
                          "         Carro.marca, "+
                          "         Carro.modelo, "+
                          "         Cor.nome "+
                          "       ) "+
                          "  FROM Carro "+
                          "  JOIN Carro.Cor Cor "+
                          "  WHERE Carro.marca = :marca";
        return em.createQuery(consulta, relatorioCarroPorMarca.class)
            .setParameter("marca",marca)
            .getResultList();
    }
    ```
19) Sobre a configuração de conexão com banco de dados no Spring leia as afirmativas abaixo:
    1) Para conexão com o banco de dados é necessário apenas adicionar as dependências no arquivo `pom.xml`
    2) As configurações de acesso ao banco de dados devem ficar no arquivo `application.propriedades`
    3) A definição em `spring.jpa.hibernate.ddl-auto=update` é usada para dar atualizar as informações do banco de dados

    Assinale apenas as alternativas que correspondem a afirmações verdadeiras

    - [ ] i, apenas
    - [ ] i e ii, apenas
    - [X] ii e iii, apenas
    - [ ] i, ii e iii
    - [ ] Nenhuma das afirmativas

20) Escreva um exemplo de implementação e uso de um `CrudRepository` para a classe da entidade `Cores.java`.
    ```java
    package br.com.curso.repository;

    import org.springframework.data.repository.CrudRepository;
    import org.springframework.stereotype.Repository;

    import br.com.curso.entity.Cor;

    @Repository
    public interface CorRepository extends CrudRepository<Cor, Integer> {

    }
    ```
    ```java
    package br.com.curso.service;

    import org.springframework.stereotype.Service;

    import br.com.curso.entity.Cor;
    import br.com.curso.repository.CorRepository;

    @Service
    public class CorService {

        private final CorRepository corRepository;

        public CrudCorService(CorRepository corRepository) {
            this.corRepository = corRepository;
        }

        private void salvar(Cor cor) {
            corRepository.save(cor);
        }

        private Iterable<Cor> listar() {
            return corRepository.findAll();
        }

        private void remover(Cor cor) {
            corRepository.deleteById(cor.getId());
        }
    }
    ```

21) Explique como funciona as `Derived Query` e de exemplos de métodos com o uso das opções: `E` , `OU` e `MAIOR QUE`.

22) Explique qual a aplicabilidade ideal para os Repository `CrudRepository`, `PaginAndSortingRepository` e `JpaRepository`. Escreva a forma de como declarar a assinatura de cada um deles.

23) Dado o código abaixo:
    ```SQL
       Create Table Cores(
          ID_COR INT auto_increment,
          NOME VARCHAR(50),
          PRIMARY KEY(ID_COR)
       );

       Create Table Carros(
          ID_CARRO INT auto_increment,
          MARCA VARCHAR(50),      
          MODELO VARCHAR(50),
          PLACA VARCHAR(10),
          ID_COR INT,
          PRIMARY KEY(ID_CARRO)
       );

       ALTER TABLE Carros ADD FOREIGN KEY(ID_COR) REFERENCES Cores (ID_COR);
    ```

    Escreva um exemplo de método que use uma `projeção` que exiba os dados dos carros com cor azul, os campos exibidos devem ser:
       - `Marca`
       - `Modelo`
       - `Placa`
       - `Nome da cor`

24) Explique para que servem as anotações `@Repository`, `@Service`, `@Controller`, `@SpringBootApplication`

25) Escreva uma classe que contenha um método REST que retorna uma lista fixa de Carros, utilize o padrão DTO.

26) Adapte seu código da atividade anterior para um modelo que usa o padrão `Repository` e retorne uma lista de Carros. 

27) Qual a finalidade do `Bean Validation`, explique e de um exemplo do seu uso em código com a annotation `@Valid`

28) Explique para que é usado a annotation `@ExceptionHandler` e de um exemplo de método que utiliza ela.
