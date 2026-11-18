# Oficina de Mecânico ERP (HenMaRaVi ERP)

Sistema completo de Gestão (ERP) desenvolvido em **Java 17 (Spring Boot 3 + JPA/Hibernate)** integrado ao **Microsoft SQL Server**, voltado para oficinas mecânicas (controle de clientes, funcionários, mecânicos, veículos, estoque de peças, compras de fornecedores, ordens de serviço, auditoria automática e administração de banco de dados).

---

## 🚀 Tecnologias Utilizadas
- **Linguagem:** Java 17
- **Framework Backend:** Spring Boot 3.2.5 (Spring MVC, Spring Data JPA, Spring Security 6, Spring JDBC)
- **Persistência de Dados:** Hibernate / JPA + JDBC Template (para chamada de Views, Procedures, Functions e Backup/Restore)
- **Banco de Dados:** Microsoft SQL Server (`HenmaraviDB`)
- **Frontend e UI:** HTML5, CSS3, Bootstrap 5.3, JavaScript, Thymeleaf + Layout Dialect

---

## ✅ Atendimento aos Requisitos Acadêmicos (Seções 4 e 5)

| Requisito | Implementação no Projeto |
| :--- | :--- |
| **Mínimo de 5 tabelas e relacionamento N x N** | **14 tabelas** no SQL Server (`pessoas`, `clientes`, `funcionarios`, `mecanicos`, `veiculos`, `pecas`, `servicos`, `compras`, `pecas_compras`, `ordens_servico`, `ordens_servico_pecas`, `ordens_servico_servicos`, `usuarios`, `logs_auditoria`) com **3 relacionamentos N x N** (`pecas_compras`, `ordens_servico_pecas`, `ordens_servico_servicos`). |
| **Login/Logout e 3 níveis de permissão** | Autenticação no banco (`usuarios`) com 3 perfis: **Administrador (`ADMIN`)** com controle total, **Operacional da Oficina (`ATENDENTE`)** com visão operacional de atendimento, e **Mecânico (`MECANICO`)** restrito à execução de O.S. e consultas técnicas (`/acesso-negado` para rotas restritas). |
| **4.1 Interfaces: Login, Gestão de Contas, Lembrete de Senha, Perfil e Logs** | Telas dedicadas em `/login`, `/usuarios` (criação e gestão de contas exclusiva do Admin), `/lembrete-senha`, `/perfil` (edição de perfil do usuário logado) e `/logs` (auditoria com motivo e responsável pelas exclusões). |
| **4.2 Pacote `entity` com Annotations JPA** | Todas as 14 classes no pacote `com.henmaravi.erp.entity` mapeadas com `@Entity`, `@Table`, `@Id`, `@Inheritance(JOINED)`, `@ManyToOne`, `@EmbeddedId` e `@MapsId`. |
| **5.2 Três Stored Procedures com Transação** | `sp_FinalizarOrdemServico`, `sp_ReajustarPrecoPecas` e `sp_RegistrarCompraRapida`, chamadas pela interface Java em `/admin-bd`. |
| **5.2 Três Consultas Complexas via Views** | `vw_ResumoOrdensServico`, `vw_DesempenhoMecanicos` e `vw_AnaliseEstoquePecas`, exibidas em tempo real em `/admin-bd`. |
| **5.2 Três Triggers disparados pelo Java** | `trg_AuditoriaStatusOS`, `trg_ValidarEAuditarItemOSPeca` e `trg_AuditoriaItemCompra`, validando estoque e gravando em `logs_auditoria`. |
| **5.2 Três Functions (Variável, Tabela e Variável Tabela)** | `fn_CalcularTotalOS` (Escalar), `fn_BuscarOSPorCliente` (`RETURNS TABLE`) e `fn_RelatorioComissaoPorStatus` (`RETURNS @TabelaComissao TABLE`), consultadas em `/admin-bd`. |
| **5.3 Backup e Restore do Banco de Dados (Bônus)** | Execução de `BACKUP DATABASE` e `RESTORE DATABASE` diretamente pela tela `/admin-bd` do sistema Java (`DatabaseAdminService.java`). |

> 📄 **Documentação Acadêmica Completa (DER, Dicionário de Dados, Explicação das Entidades e Roteiro do Vídeo):** consulte o arquivo [`DOCUMENTACAO_ACADEMICA.md`](./DOCUMENTACAO_ACADEMICA.md) na raiz deste repositório.

---

## ⚙️ Como Instalar e Rodar o Projeto

### 1. Configurando o Banco de Dados (SQL Server)
1. Abra o **SSMS (SQL Server Management Studio)** e conecte-se ao seu servidor `localhost`.
2. Crie o banco de dados (caso ainda não exista):
   ```sql
   CREATE DATABASE HenmaraviDB;
   GO
   ```
3. Abra e execute o arquivo **`database.sql`** (localizado na raiz do projeto). Ele criará todas as **tabelas**, fará a **injeção de dados iniciais (`INSERT`)**, e criará as **3 Views, 3 Functions, 3 Triggers e 3 Stored Procedures**.

### 2. Configurando as Credenciais (`application.yml`)
Verifique o arquivo `src/main/resources/application.yml`:
```yaml
  datasource:
    url: jdbc:sqlserver://localhost:1433;databaseName=HenmaraviDB;encrypt=true;trustServerCertificate=true
    username: sa
    password: 1234
```

### 3. Executando o Sistema
Execute a classe `HenmaraviApplication.java` na sua IDE ou rode no terminal:
```bash
mvn spring-boot:run
```
Acesse no navegador: **`http://localhost:8080`**

---

## 🔐 Credenciais de Acesso (3 Níveis de Permissão)

| Nível / Perfil | Usuário | Senha | Permissões e Visualização |
| :--- | :--- | :--- | :--- |
| **1. Administrador (`ADMIN`)** | `admin` | `123` | **Acesso Total:** Criação/gestão de contas (`/usuarios`), Folha de Funcionários (`/funcionarios`), Gestão de Mecânicos (`/mecanicos`), Gestão de Clientes, Veículos, Estoque e Serviços, Entrada de Estoque (`/compras`), Logs de Exclusão (`/logs`) e Central DBA & Backup (`/admin-bd`). |
| **2. Operacional da Oficina (`ATENDENTE`)** | `atendente` | `123` | **Visão Operacional:** Abertura e execução de Ordens de Serviço (`/ordens-servico`), Cadastro e consulta de Clientes (`/clientes`) e Veículos (`/veiculos`), Consulta de Estoque de Peças (`/estoque`) e Catálogo de Serviços (`/servicos`). Sem acesso a Funcionários, Compras ou Admin BD. |
| **3. Mecânico (`MECANICO`)** | `mecanico` | `123` | **Visão de Bancada:** Abertura e execução de Ordens de Serviço (`/ordens-servico`), consulta de Veículos (`/veiculos`) e Serviços (`/servicos`). Bloqueado para gestão de mecânicos, clientes, estoque, compras e administração. |

---

## 🛡️ Regras de Negócio e Diferenciais Implementados

- **Criação de Contas Exclusiva pelo Admin (`/usuarios`):** Não há auto-cadastro público; somente usuários com perfil `ADMIN` podem criar, editar, ativar/desativar e excluir contas do sistema.
- **Reaproveitamento Inteligente de `Pessoa` (Herança `JOINED`):** Caso uma pessoa já cadastrada no banco (ex.: um `Mecanico`) passe a atuar também como `Cliente` ou `Funcionario` (ou vice-versa), o sistema reutiliza o `codigo` (`PK`) existente na tabela `pessoas` e insere apenas a chave estrangeira na tabela filha correspondente, preservando a restrição `UNIQUE` do CPF. Ao excluir apenas um dos papéis, o registro base em `pessoas` é mantido enquanto houver vínculo em outra tabela filha.
- **Auditoria Obrigatória de Exclusões (`/logs`):** Toda exclusão no sistema exige um **motivo obrigatório** e captura automaticamente o **usuário logado** (`SecurityContextHolder`), registrando os detalhes na tabela `logs_auditoria`.
- **Estorno Automático de Estoque em Compras (`/compras`):** Ao remover um item de uma nota de entrada ou excluir uma Compra inteira, a quantidade comprada é automaticamente subtraída de `pecas.estoque` e registrada na aba de **Logs e Exclusões (`/logs`)**.
- **Tratamento Amigável de Erros (`GlobalExceptionHandler`) e Cascata Opcional:** Exceções de integridade referencial (`FK`) e unicidade (`UNIQUE`) do SQL Server são traduzidas para mensagens claras em português informando exatamente quais registros estão vinculados, com opção de **forçar exclusão removendo vínculos** no modal de confirmação.
- **Testes de Integração Automatizados:** A suíte [`CrudAndRulesIntegrationTest.java`](./src/test/java/com/henmaravi/erp/CrudAndRulesIntegrationTest.java) valida automaticamente o fluxo de edição, exclusão, reaproveitamento de `Pessoa`, estorno de estoque e geração de logs.
