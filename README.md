# Projeto PostgreSQL – AWS

**Recursos utilizados na aplicação:**

1. Placa CPU EMBFLEX ESP32;
2. Máquina virtual EC2 - aws;
3. Mqtt Broker instalado na EC2;
4. Técnica de link de broker IoT com DB;

### Banco de dados PostgreSQL:

Para a criação de um banco de dados PostgreSQL em cloud, deve-se primeiro ter uma conta em um cloud web servisse, nesse caso foi utilizado a AWS.
Como o PostgreSQL é um banco de dados relacional, foi utilizado o serviço RDS da AWS, especializado em banco de dados relacionais, dentro desse serviço são determinadas configurações como, nome do banco de dados, VPC, security groups que determina permissão e negação de acessos externos, pode ser configurado uma role IAM (conjunto de regras, grupos, usuários e politicas para a própria segurança do banco, segurança interna), memória do banco, acesso público e backup dos dados. Para esse banco em fase de testes, foram configurados:

- Storage: 20GiB;
- Security groups:
- Type | port range | Source
- SSH | 22 | 0.0.0.0/0
- PostgreSQL | 5432 | sg-046c41689d9e6969a - launch-wizard-1
- PostgreSQL | 5432 | MyIP
- Zona: North Virginia (pode ser usada qualquer uma);
- VPC: padrão;
- Public accessibility: sim;
- Class: db.t3.micro;
- vCPU: 2;
- RAM: 1GB;

![Alt](./images/aws-rds.png)

Para testar a aplicação foi instalado em uma máquina local o PostgreSQL. Para acesso, deve-se fornecer o endereço Endpoint do banco, gerado na própria instancia RDS, o nome do banco, usuário e senha definidos nas pré-configurações. Depois disso foram criadas as tabelas e colunas do banco;
Observação: o acesso só é liberado se você habilitou o ip da máquina no security groups;

## Acessando o banco e criando tabelas

- Primeiro instale o PostgreSQL e abra o SQL SHELL
- Faça o seu login

  ![Alt](./images/postgresql-login.png)

Em seguida digite os seguintes códigos para a criação do banco:

```js
CREATE DATABASE mqtt;
```

Ao entrar no banco digite:

```js
CREATE TABLE temperature(
id serial primary key,
temp numeric(4,2),
dh timestamp);
```

A tabela foi criada!

## Por que optar pelo PostgreSQL ao invés do MariaDB

Foi feito uma pesquisa entre PostgreSQL x MariaDB, foi constatado que, para esse tipo de aplicação IoT o PostgreSQL é melhor. Em um caso de dashboard usando algum software como por exemplo o Grafana, com algumas extensões, é possível fazer a compressão de alguns dados time series, deleção de dados automaticamente em um período entre outros recursos. É de fácil gerência e manipulação de dados.

O banco de dados PostgreSql vem se tornando cada vez mais fortes em ambientes corporativos e clouds. Suas transações são extremamente úteis para uma série de tarefas, como por exemplo, seu tipo de condificação padrão é UTF-8, então sinais como `"´~^ç"` serão aceitos na sua tabela, já no mariaDB não. Outro benefício são tipos de dados, no PostgreSQL você pode colocar diversos tipos de dados, como por exemplo o `boolean`, que armazena True ou False, já no MariaDB o dado é armazenado como tipo `tinyint(1)` e é mostrado 0 (False) ou 1(True).

Esses são alguns motivos dos quais o PostgreSQL é considerado melhor do que o MariaDB, é um banco com muito mais possibilidades e facilidade com a manipulação dos dados.

## Referências bibliográficas

<https://docs.aws.amazon.com/pt_br/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.PostgreSQL.html>    
<https://embarcados.com.br/webinar-monitorando-sensores-iot-do-esp32-ao-grafana/>  
<https://www.youtube.com/watch?v=m4fFVFf4ZcE>
