# Projeto Auto Scaling com CLB e teste no Endpoint 

* [Objetivos](#Objetivos)
* [VPC e Subredes](#VPC-e-Subredes)
* [Security Groups](#Security-Groups)
* [Modelo de Execução](#Modelo-de-Execução)
* [Load Balancer](#Load-Balancer)
* [Auto Scaling](Auto-Scaling)
* [CloudWatch](CloudWatch)
---
## Objetivos

Configurar um Load Balancer com Auto Scaling que aumenta ou reduz instâncias EC2 automaticamente com base no número de requisições ao endpoint /teste.
---
## VPC e Subredes

Primeiro, vamos a criação da VPC, criamos uma VPC com duas subnets públicas, sem Nat Gateway por não se fazer necessário. E assim chegamos mo resultado final:

![VPC](/Images/VPC.png)
---
## Security Groups

Foi necessário a criação de dois grupos, um para EC2 e outro para o Load Balancer:

### Regras de entrada:

Load Balancer:

| Type  | Protocol | Port Range | Source Type |  Source   |
| ----- | -------- | ---------- | ----------- | --------- |
| HTTP  |   TCP    |    80      |  Anywhere   | 0.0.0.0/0 |

EC2:

|   Type     | Protocol | Port Range | Source Type |  Source   |
| ---------- |--------- | ---------- | ----------- | --------- |
|    HTTP    |   TCP    |    80      |   Custom    |  LB SG    |
|    SSH     |   TCP    |    22      |   Meu IP    |  IP do Usuário    |

## Modelo de Execução

Seguindo os requisitos foi habilitado o ip público da EC2 sua utilização. E o resultado foi como o da imagem:

![EC2](/Images/EC2.png)

Após, deve-se criar um [user_data](/user_data.sh) e associá-lo ao modelo de execução.
---

## Load Balancer

Como um dos requisitos, utilizamos o Classic Load Balancer para esse projeto:

![CLB](/Images/CLB.png)

O conectamos a VPC através das subnets públicas e também o ligamos ao security group destinado a ele.
---

## Auto Scaling

Assim chegamos a Criação do Auto Scaling:

![AutoSC](/Images/AutoSC.png)

* Selecionamos o Lauch Template criado nas etapas anteriores e também foram adicionadas as subredes criadas. Fazendo assim o link ao Load Balancer.

![AutoSC](/Images/AutoSC2.png)

* Como requisito do projeto foram o mínimo de 1 de capacidade e máximo de 3.
---
## CloudWatch

Aqui vamos a implemetação das regras:

![CW](/Images/CW.png)

* Aumentar quando houver mais requisições,
Métrica: RequestCount por instância > 10