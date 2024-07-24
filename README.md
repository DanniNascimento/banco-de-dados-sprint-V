# Praticando SQL Curso Proz Banco de dados Simples...

Lembre-se, cada desafio é uma chance de crescer. Não se desanime com os erros; eles são degraus no caminho do aprendizado. E acima de tudo, divirta-se! 
O aprendizado mais eficaz acontece quando nos engajamos e nos interessamos pelo que estamos fazendo.


Vamos para um desafio de banco de dados pela SPRINT V da PROZ...

#### Diagrama de Entidade-Relacionamento (ERD)
Para um sistema de login simples, precisamos de pelo menos duas tabelas principais: Usuarios e Logins.

```sql
Usuarios
usuario_id (PK)
nome
email
senha (em um sistema real, a senha deve ser armazenada de forma segura, usando hashing)
data_criacao

Logins
login_id (PK)
usuario_id (FK)
data_login
sucesso (booleano indicando se o login foi bem-sucedido)
```

#### Criação do Banco de Dados e Inserção de Dados de Teste
```sql
CREATE TABLE Usuarios (
    usuario_id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    senha VARCHAR(255) NOT NULL,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Logins (
    login_id SERIAL PRIMARY KEY,
    usuario_id INT REFERENCES Usuarios(usuario_id),
    data_login TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    sucesso BOOLEAN NOT NULL
);

-- Inserção de dados de teste na tabela Usuarios
INSERT INTO Usuarios (nome, email, senha) VALUES
('Alice', 'alice@example.com', 'alice123'),
('Bob', 'bob@example.com', 'bob123'),
('Charlie', 'charlie@example.com', 'charlie123');

INSERT INTO Logins (usuario_id, data_login, sucesso) VALUES
(1, '2024-07-20 08:00:00', TRUE),
(2, '2024-07-21 09:30:00', FALSE),
(3, '2024-07-22 10:45:00', TRUE),
(1, '2024-07-23 11:15:00', TRUE);
```

#### Criando tabela (vendas)
```sql
CREATE TABLE Vendas (
    VendaID INT PRIMARY KEY AUTO_INCREMENT,
    ClienteID INT,
    ProdutoID INT,
    Quantidade INT,
    DataVenda DATE,
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ClienteID)
);
```

#### Consultas Simples
Selecionar todos os usuários e seus detalhes de login:

```SELECT 
    u.usuario_id, 
    u.nome, 
    u.email, 
    l.login_id, 
    l.data_login, 
    l.sucesso
FROM 
    Usuarios u
JOIN 
    Logins l ON u.usuario_id = l.usuario_id
```

Contar o número de logins bem-sucedidos por usuário:
```sql
SELECT 
    u.usuario_id, 
    u.nome, 
    COUNT(l.login_id) AS logins_bem_sucedidos
FROM 
    Usuarios u
JOIN 
    Logins l ON u.usuario_id = l.usuario_id
WHERE 
    l.sucesso = TRUE
GROUP BY 
    u.usuario_id, u.nome;
```

Selecionar os usuários que nunca fizeram login com sucesso:
```sql
SELECT 
    u.usuario_id, 
    u.nome, 
    u.email
FROM 
    Usuarios u
LEFT JOIN 
    Logins l ON u.usuario_id = l.usuario_id AND l.sucesso = TRUE
WHERE 
    l.login_id IS NULL;
```

#### Consulta 1: Selecionar todos os usuários e seus detalhes de login
```
usuario_id	nome	email	login_id	data_login	sucesso
1	Alice	alice@example.com	1	2024-07-20 08:00:00	True
2	Bob	bob@example.com	2	2024-07-21 09:30:00	False
3	Charlie	charlie@example.com	3	2024-07-22 10:45:00	True
1	Alice	alice@example.com	4	2024-07-23 11:15:00	True

```

#### Consulta 2: Contar o número de logins bem-sucedidos por usuário

```
usuario_id	nome	logins_bem_sucedidos
1	Alice	2
3	Charlie	1

```
#### Consulta 3: Selecionar os usuários que nunca fizeram login com sucesso
```
usuario_id	nome	email
2	Bob	bob@example.com
```
