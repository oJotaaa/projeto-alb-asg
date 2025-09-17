# Projeto: Escalabilidade e Disponibilidade na Nuvem

Este projeto resume os conceitos de Auto Scaling, Load Balancer e as regras de escalabilidade que garantem a performance e a resiliência de uma aplicação.

## 1. Load Balancer 🚦

### O que é?
O Load Balancer atua como um 'gerenciador de tráfego' para uma aplicação. Sua função principal é distribuir as solicitações dos usuários de forma equilibrada entre os vários servidores disponíveis. Isso evita que um único servidor fique sobrecarregado enquanto outros ficam ociosos, garantindo que a aplicação se mantenha rápida e disponível para todos."

![Arquitetura de um Load Balancer](https://miro.medium.com/v2/resize:fit:1200/0*25wxhPyXLvQ-8rpr.jpg)

### Passo a passo de exemplo (Conceitual)
1.  **Recebimento:** O usuário tenta acessar o site `meusite.com`. A requisição chega primeiro no Load Balancer.
2.  **Verificação:** O Load Balancer verifica quais servidores estão "saudáveis" e disponíveis para receber trabalho.
3.  **Distribuição:** Ele encaminha a requisição para o servidor com a menor carga no momento.
4.  **Resposta:** O servidor processa a requisição e responde de volta para o usuário através do Load Balancer.

## 2. Auto Scaling 🤖

### O que é?
O Auto Scaling é um serviço que escalona a quantidade de recursos computacionais (como servidores) automaticamente, de acordo com as necessidades da aplicação. Ele opera com base em regras pré-definidas. Por exemplo, uma regra pode estipular que, se a carga de CPU de um servidor ultrapassar 70% por mais de 5 minutos, uma nova instância seja criada para ajudar a diminuir o sobrecarregamento.

![Fluxo de Auto Scaling](https://docs.aws.amazon.com/images/autoscaling/ec2/userguide/images/asg-basic-arch.png)


### Regras de Scaling (Scale Out e Scale In)

As regras de scaling são as condições que disparam a ação de adicionar ou remover servidores. Elas são baseadas em métricas de monitoramento e garantem que a aplicação tenha sempre a capacidade necessária, sem desperdício.

* **Regra de Scale Out (Adicionar Servidor):** Esta regra é acionada quando a aplicação precisa de *mais* poder computacional.
    * **Condição:** A média de uso da CPU de todos os servidores ultrapassa um limite (ex: 75%).
    * **Período:** Essa condição precisa se manter por um tempo determinado (ex: 5 minutos) para evitar reações a picos de uso muito curtos.
    * **Ação:** Um ou mais servidores novos são criados e adicionados ao Load Balancer.

* **Regra de Scale In (Remover Servidor):** Esta regra é acionada quando a aplicação tem capacidade sobrando, permitindo *economizar custos*.
    * **Condição:** A média de uso da CPU cai abaixo de um limite (ex: 25%).
    * **Período:** Essa condição precisa se manter por um tempo determinado (ex: 10 minutos) para garantir que a baixa demanda é estável.
    * **Ação:** Um dos servidores ociosos é removido do Load Balancer e desligado.

### Passo a passo de exemplo (Conceitual)
1.  **Monitoramento:** O serviço de Auto Scaling monitora constantemente a métrica definida (ex: uso de CPU).
2.  **Alarme (Scale Out):** A média de CPU de todos os servidores ultrapassa 75% por 5 minutos. Um alarme é disparado.
3.  **Ação (Scale Out):** O Auto Scaling automaticamente cria um novo servidor, o instala e o adiciona ao grupo do Load Balancer.
4.  **Alarme (Scale In):** Horas depois, o tráfego diminui e a média de CPU cai para menos de 25%. Outro alarme é disparado.
5.  **Ação (Scale In):** O Auto Scaling remove um dos servidores extras para economizar recursos.

### 3. O que aprendi 🧠

Aprendi que o Load Balancer e o Auto Scaling não são serviços que trabalham sozinhos; eles são peças de um sistema maior que trabalham em perfeita sinergia para criar uma aplicação resiliente, escalável e econômica.

O **Load Balancer** atua na linha de frente, como um gerente de tráfego, garantindo que o trabalho seja distribuído de forma justa entre todos os servidores disponíveis. Ele garante a saúde individual de cada instância.

Enquanto isso, o **Auto Scaling** age como um gerente de recursos, observando a saúde do *grupo inteiro* de servidores. Quando ele percebe que todos os servidores, mesmo com o trabalho bem distribuído pelo Load Balancer, estão sobrecarregados, ele entra em ação.

Seguindo as **regras de scaling**, ele cria novas instâncias para aumentar a capacidade total. O Load Balancer imediatamente reconhece esses novos servidores e passa a incluí-los na distribuição de tráfego, aliviando a pressão do sistema. O processo inverso ocorre quando o tráfego diminui, garantindo que não haja desperdício de recursos. Juntos, eles garantem que a aplicação sempre tenha o tamanho certo para a demanda do momento.
