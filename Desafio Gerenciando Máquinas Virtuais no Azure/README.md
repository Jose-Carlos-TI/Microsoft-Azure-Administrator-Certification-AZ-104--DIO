
# Gerenciando Máquinas Virtuais no Azure

Como implementar e gerenciar máquinas virtuais  com Avaibility Set no Azure

## Passos para implantar uma VM no portal Azure:

1. Acessar o portal Azure: Efetuar login no portal Azure.
2. Navegar até Máquinas Virtuais: Pesquisar por "Máquinas Virtuais" e selecionar a opção em "Serviços".
3. Criar uma nova máquina virtual: Clicar em "Criar" e selecionar "Máquina Virtual do Azure".
4. Preencher os detalhes da VM:
* Detalhes do projeto: Selecionar a assinatura e criar ou escolher um grupo de recursos.
* Detalhes da instância: Definir nome da VM, região (que suporte Início Confiável), tipo de segurança (como Máquinas Virtuais de Início Confiável) e opções de segurança (Inicialização Segura, vTPM, Monitoramento de Integridade).
* Imagem: Escolher uma imagem Gen2 compatível com Início Confiável (Linux ou Windows).
* Tamanho: Selecionar um tamanho de VM que suporte Início Confiável.
* Conta de administrador e regras de porta: Definir as configurações de acesso e segurança da máquina virtual.
5. Revisar e criar: Verificar os detalhes da VM e clicar em "Criar".
6. Aguardar a implantação: O Azure provisionará a máquina virtual. 

## Como criar um Availability Set:

1. No Portal do Azure:
* Vá para "Máquinas Virtuais" e clique em "Criar" -> "Máquina Virtual". 
* Na seção "Detalhes da instância", selecione um "Grupo de Recursos" existente ou crie um novo. 
* Escolha um "Nome" e uma "Região" para sua VM. 
* Na seção "Disponibilidade", selecione "Conjunto de Disponibilidade" e crie um novo ou selecione um existente.

2. Usando o Azure CLI:
Use o comando az vm create com o parâmetro --availability-set para especificar o nome do Availability Set.

### Melhores práticas:

Para alta disponibilidade, é recomendável ter pelo menos duas VMs em um Availability Set. 
Se você precisar de uma proteção ainda maior, considere usar Zonas de Disponibilidade, que distribuem suas VMs em diferentes data centers dentro de uma região. 
Não é possível alterar o Availability Set de uma VM após a criação, então planeje com antecedência.

## Como criar um Auto Scaling Group

1. Criar um Conjunto de Dimensionamento de Máquinas Virtuais:
* Acesse o portal do Azure e procure por "Conjuntos de Dimensionamento de Máquinas Virtuais". 
* Clique em "Criar" e siga as instruções para configurar o conjunto, incluindo:
* Assinatura e Grupo de Recursos: Selecione a assinatura e o grupo de recursos onde o conjunto será implantado. 
* Nome do Conjunto: Escolha um nome para o seu conjunto de dimensionamento. 
* Localização: Selecione a região onde o conjunto será implantado. 
* Sistema Operacional e Imagem: Escolha o sistema operacional (Windows ou Linux) e a imagem da máquina virtual. 
* Tamanho da Máquina Virtual: Selecione o tamanho da máquina virtual que melhor atende às suas necessidades. 
* Configuração de Rede: Configure a rede virtual e a sub-rede onde as máquinas virtuais serão implantadas. 
* Escala: Defina o número inicial de instâncias, a política de atualização e o tipo de escalonamento (manual ou automático).

2. Configurar o Auto Scaling:
* Após a criação do conjunto, acesse-o no portal do Azure e vá para a seção "Escala". 
* Selecione "Dimensionamento automático" e configure as regras de escala, que podem ser baseadas em:
* Métricas de Desempenho: CPU, memória, etc. 
* Agendamento: Aumentar ou diminuir o número de VMs em horários específicos. 
* Eventos: Disparar ações de escala com base em eventos externos.

3. Testar o Dimensionamento Automático:
* Após configurar o escalonamento automático, é importante testá-lo para garantir que funciona como esperado. 
* Você pode simular o tráfego na aplicação ou usar ferramentas de teste de carga para verificar o comportamento do conjunto. 

### Considerações importantes:
#### Tipos de Escala:
* Escala Horizontal: Aumentar ou diminuir o número de instâncias (VMs). 
* Escala Vertical: Alterar o tamanho das VMs (CPU, memória, etc.).

#### Balanceamento de Carga:
* Conjuntos de dimensionamento são normalmente usados com um balanceador de carga para distribuir o tráfego entre as VMs.

#### Alta Disponibilidade:
* O uso de conjuntos de dimensionamento pode melhorar a alta disponibilidade da aplicação, pois permite que o Azure substitua VMs com falha por novas instâncias. 

#### Custos:
* O auto scaling pode ajudar a otimizar os custos, pois você paga apenas pelas máquinas virtuais que estão em uso.

## Desanexando o disco de uma máquina virtual e utilizar em outra

### Desanexando o disco:

1. Acesse o portal do Azure.
2. Navegue até a máquina virtual (VM) da qual você deseja desanexar o disco.
3. No menu à esquerda, em "Configurações", selecione "Discos".
4. Localize o disco de dados que deseja desanexar e clique no botão "Desanexar".
5. Confirme a operação e salve as alterações. 

### Anexando o disco a outra VM:

1. Crie uma nova VM ou selecione uma VM existente onde você deseja usar o disco desanexado.
2. No painel da VM, em "Configurações", selecione "Discos". 
3. Clique em "Anexar disco existente" e selecione o disco desanexado da etapa anterior. 
4. Configure as opções adicionais, como o LUN (Logical Unit Number), e salve as alterações. 
Observações:
* Certifique-se de que a VM de destino tenha o tamanho e as configurações adequadas para o disco que você está anexando. 
* Se o disco for um disco do sistema operacional, a VM de destino deve ser compatível com o sistema operacional do disco. 
* É possível desanexar discos de dados a quente (sem desligar a VM), mas é recomendável que o disco não esteja sendo usado ativamente no momento da desanexação. 
* O disco permanece no armazenamento do Azure mesmo após a desanexação, e você será cobrado por ele até que o exclua. 
* Você pode excluir discos não utilizados para evitar custos adicionais. 
