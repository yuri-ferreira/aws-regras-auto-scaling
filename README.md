# Projeto Auto Scaling com CLB e teste no Endpoint 

* [Objetivos](#Objetivos)
* [VPC e Subredes](#VPC-e-Subredes)
---
## Objetivos

Configurar um Load Balancer com Auto Scaling que aumenta ou reduz instâncias EC2 automaticamente com base no número de requisições ao endpoint /teste.
---
## VPC e Subredes

Primeiro, vamos a criação da VPC, criamos uma VPC com duas subnets públicas, sem Nat Gateway por não se fazer necessário.

![VPC](/Images/VPC.png)

E Por fim ao final da criação:

![VPC2](/Images/VPC2.png)