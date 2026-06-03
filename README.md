# Oficina de Mecânico ERP

Sistema completo de Gestão (ERP) desenvolvido para oficinas mecânicas, focado no controle de estoque de peças, ordem de serviço e emissão de faturamento financeiro.

## 🚀 Tecnologias Utilizadas
- **Linguagem:** Java 17
- **Framework Backend:** Spring Boot 3
- **Persistência de Dados:** Spring Data JPA / Hibernate
- **Banco de Dados:** Microsoft SQL Server
- **Frontend e UI:** HTML5, CSS3, Bootstrap 5, Javascript Vanilla, Thymeleaf
- **Arquitetura:** MVC (Model-View-Controller)

---

## ⚙️ Como Instalar e Rodar o Projeto em Outro Computador

Para rodar este projeto em um novo computador, siga as instruções detalhadas abaixo:

### 1. Pré-Requisitos
Você precisará ter instalado na sua máquina:
- **Java 17 (JDK 17)** ou superior.
- **Maven** (Opcional se usar uma IDE).
- **SQL Server** e **SSMS** (SQL Server Management Studio).
- **IDE Java** (Eclipse, IntelliJ IDEA ou VS Code).

### 2. Configurando o Banco de Dados (SQL Server)
1. Abra o seu SSMS (SQL Server Management Studio).
2. Conecte-se ao seu servidor local (localhost).
3. Crie o banco de dados vazio rodando o seguinte comando:
   ```sql
   CREATE DATABASE HenmaraviDB;
   ```
4. Em seguida, pegue o arquivo **`database.sql`** (que está na raiz deste projeto) e rode ele dentro do SSMS. Ele já contém a criação de todas as tabelas (CREATE TABLE) e a injeção dos 10 registros de teste (INSERT) para deixar o sistema totalmente populado e pronto para uso.

### 3. Configurando as Credenciais (Application.yml)
1. Abra este projeto na sua IDE.
2. Navegue até o arquivo `src/main/resources/application.yml`.
3. Verifique as credenciais do seu banco de dados na seção `datasource`:
   ```yaml
   username: sa
   password: YourStrong!Password #
   ```
   *(Atenção: O Hibernate já está configurado com `ddl-auto: update`, mas como você executou o `database.sql` no passo anterior, ele não tentará recriar as tabelas).*
- Professor, nós sabemos que o Spring Boot pode criar as tabelas sozinho porque deixamos o ddl-auto: update ligado. Porém, como pedimos para o senhor rodar o arquivo database.sql primeiro (para injetar os dados de teste), quando o sistema ligar, o Hibernate vai ver que as tabelas já existem e não vai tentar recriá-las nem causar conflito."

### 4. Executando o Sistema
1. Na sua IDE, localize a classe principal: `HenmaraviErpApplication.java`.
2. Clique com o botão direito e escolha **Run (Executar)**.
3. Se estiver usando o terminal, digite: `mvn spring-boot:run`.
4. Aguarde a mensagem "Started" no console.
5. Abra o navegador e acesse a URL oficial do sistema:
   👉 **`http://localhost:8080`**

### 5. Dados de Login (Usuário de Teste)
Para acessar o sistema via tela de Autenticação (Login), utilize a credencial padrão configurada no Spring Security:
- **Usuário:** admin
- **Senha:** 123

---

## 📦 Módulos Principais do ERP
1. **Autenticação:** Segurança aplicada, bloqueando rotas sem login.
2. **Dashboard de Bordo:** Painel visual com cards financeiros somando totais de faturamento e chamados abertos.
3. **Módulo de Pessoas:** Herança implementada no banco (`Pessoa` -> `Cliente`, `Funcionário`, `Mecânico`).
4. **Módulo de Veículos:** Vinculação de automóveis a clientes da base.
5. **Estoque e Catálogo:** Gestão do estoque físico de Peças e precificação de Mão de Obra (Serviços).
6. **Módulo de Compras (Entrada):** Formulário dinâmico para dar entrada em nota fiscal e alimentar o estoque físico automaticamente.
7. **Ordem de Serviço (O.S.):** Coração do ERP. Tela robusta com cabeçalho de cliente/carro e adição iterativa (Modais) de Peças e Serviços, abatendo estoque simultaneamente.
