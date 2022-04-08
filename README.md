# eive-java-101-exam-3

1) No Maven o arquivo `pom.xml` é o principal arquivo de configuração de dependencias. Sabendo disso leia as afirmativas abaixo.
   1) A dependências podem ser adicionadas diretamente na pasta lib do projeto e este após compilar continua reconhecendo.
   2) A dependências seguem um padrão de organização, sempre devem ser adicionadas na sessão `<dependencies>` do arquivo.
   3) Sempre que uma nova depenência é adicionada no projeto o `Mavem` identifica e baixa de forma altomatica sem que o desenvolvedor precise se preocupar.

   Assinale a alternativa que corresponte apenas as informações verdadeiras.
   - [ ] III, apenas
   - [ ] I e II
   - [ ] I e III 
   - [ ] II e III
   - [ ] I, II e III

2) No `Maven` existem alguns comandos que são conhecidos como `goal`, com base no seu conhecimento, leia as afirmativas abaixo e assinale a alternativa correta
   1) O comando `mvn test` tem como finalidade executar os testes do projeto
   2) O comando `mvn deploy production` é usado quando precisa gerar o artefado para o ambiente de produção
   3) `mvn install` é o goal que realiza a instalação do projeto no diretório local

   - [ ] I, apenas
   - [ ] II apenas
   - [ ] I e II apenas
   - [ ] I e III apenas
   - [ ] II e III, apenas
 
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

4) Veja o comando abaixo, como ele poderia ser adaptado para imprimir o código gerado na inclusão do registro?
   ```java
   Statement stmt = connection.createStatement();
   stmt.execute("INSERT INTO Produto VALUES ('Cadeira', 'Cadeira 4 pernas')");  
   ```

5) Oque é um pool de conexão e qual sua vantagem? Explique.

6) De um exemplo de uso de pool de conexão usando a biblioteca c3p0

7) Quais as desvantagens de usar o JDBC em uma aplicação?

8) Explique de forma suscinta como funciona o cliclo de vida de uma aplicação JPA

9) Sobre JPA explique a diferença entre o uso da importação de `javax.persistence.Entity` e `org.hibernate.annotations.Entity`

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

11) Sobre o ciclo de vida de uma Entidade, Explique cada um dos estados `TRANSIENT`, `MANAGED` e `DETACHED`.

12) Qual a finalidade do método `merge()` no JPA. Escreva um exemplo simples para mostrar seu uso.

13) Usando a sintaxe JPQL como poderia ser adaptado o código abaixo para listar todos os dados da tabela `Carros`
    ```java
    public Lista<Carro> findAll(){
        return null;
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

15) Qual a função do `mappedBy` e onde ele deve ser usado?

16) Escreva um exemplo de como fazer um `JOIN` no JPQL?

17) Qual a função do `select new` no JPA?

18) Considerando que no seu projeto tem a seguinte estrutura de pacotes:
    ```
    br.com.curso.modelo
         Carro.java
         Cores.java
    br.com.curso.dao
         CarroDao.java
         CoresDao.java
    ```

    Escreva um método que exemplifique uma consulta usando `select new` do JPA, descreva se precisa de novas classes e se for preciso onde cada uma delas deve ser organizada no projeto seguindo as boas praticas.


19) Sobre a configuração de conexão com banco de dados no Spring leia as afirmativas abaixo:
    1) Para conexão com o banco de dados é necessário apenas adicionar as dependências no arquivo `pom.xml`
    2) As configurações de acesso ao banco de dados devem ficar no arquivo `application.propriedades`
    3) A definição em `spring.jpa.hibernate.ddl-auto=update` é usada para dar atualizar as informações do banco de dados

    Assinale apenas as alternativas que correspondem a afirmações verdadeiras

    - [ ] I, apenas
    - [ ] I e II, apenas
    - [ ] II e III, apenas
    - [ ] I, II e III
    - [ ] nenhuma das afirmativas

20) Escreva um exemplo de implementação e uso de um `CrudRepository` para a classe da entidade `Cores.java`.

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

27) Qual a finalidade do `Bean Validation`, Explique e de um exemplo do seu uso em código com a annotation `@Valid`

28) Explique para que é usado a annotation `@ExceptionHandler` e de um exemplo de método que utiliza ela.
