# Diagrama de Ambiente On-premises + Azure + Webapp

O ambiente foi cuidadosamente arquitetado para manter a conectividade, redundância, segurança e resiliência de dados.

## Equipamentos utilizados


- 2 Switch Cisco SG220-26 L2
- 2 Fortigate FG-80F (HA)
- 2 Switch Dell N1524 - 24 portas gigabit 4X SFP+ L3
- 2 Servidores Rack PowerEdge R250
- 1 Servidor Rack PowerEdge R760
- 4 FortiAP 234F

## Descrição do Projeto


### Diversificação de Links de Internet:
Implementamos uma abordagem estratégica ao adotar três links de internet distintos:
Dois links de fibra óptica fornecidos por provedores confiáveis, garantindo alta velocidade e baixa latência.
Um link via satélite da Starlink, oferecendo uma alternativa resiliente especialmente em áreas onde o acesso à fibra óptica é limitado.

### Arquitetura de Rede Redundante:
Para aumentar a confiabilidade, os três links foram conectados à VLAN 10, que está distribuída entre dois switches de camada 2 em um modelo de redundância. Isso assegura que em caso de falha em um dos switches, o tráfego possa ser instantaneamente roteado através do outro, garantindo a continuidade das operações sem interrupções significativas.

### Firewalls de Alta Disponibilidade (HA):
Para fortalecer a segurança da rede, integramos dois firewalls configurados em um ambiente de Alta Disponibilidade (HA). Essa configuração garante que, mesmo em caso de falha de hardware ou manutenção, a segurança da rede seja mantida, com o tráfego de dados continuando a ser filtrado e monitorado sem interrupções.

### Roteamento Avançado com Redundância nos Switches de Camada 3:
Após a inspeção pelo firewall, o tráfego é encaminhado para dois switches de camada 3 configurados com redundância. Essa configuração garante uma maior disponibilidade da rede, pois, em caso de falha em um dos switches, o outro assume automaticamente, evitando interrupções no fluxo de dados. A redundância dos switches de camada 3 é essencial para manter a continuidade operacional e garantir a confiabilidade dos serviços oferecidos pela nossa rede.

### Implementação de Clusters de Servidores ESXi:
Dos três servidores ESXi mencionados, dois estão configurados em um cluster de alta disponibilidade. Essa configuração permite a migração automatizada de máquinas virtuais entre os servidores em caso de falha, garantindo a continuidade dos serviços e minimizando o tempo de inatividade. O cluster de servidores ESXi oferece uma infraestrutura altamente resiliente para hospedar nossos serviços essenciais.

### Diversidade de Serviços e Aplicações:
Nos servidores ESXi em cluster, temos três máquinas virtuais essenciais para as operações diárias da nossa organização:

- Active Directory (AD): Responsável pela autenticação de usuários, controle de acesso e políticas de segurança na rede.
- File Server: Fornece armazenamento centralizado e compartilhamento de arquivos para facilitar a colaboração entre os usuários.
- Print Server: Gerencia as impressoras da rede, permitindo que os usuários imprimam documentos de forma conveniente e eficiente.

### Infraestrutura Adicional nos Servidores ESXi:
No terceiro servidor ESXi, hospedamos uma variedade de serviços e aplicativos adicionais para suportar as operações e a gestão da nossa rede:

- Servidor de Backup: Responsável pelo armazenamento e gerenciamento de cópias de segurança dos dados críticos da organização, garantindo a resiliência contra perda de dados.
- vCenter Server: Utilizado para centralizar e simplificar a administração da infraestrutura de virtualização, oferecendo recursos avançados de gerenciamento e monitoramento.
- Servidor de Monitoramento (Zabbix e Grafana): Utilizados para monitorar o desempenho e a integridade da rede, fornecendo insights valiosos para otimização e resolução proativa de problemas.
- Ambientes de Desenvolvimento, Homologação e Banco de Dados: Criados para suportar as atividades de desenvolvimento de software, testes e gerenciamento de bancos de dados, proporcionando um ambiente seguro e isolado para experimentação e validação.

### Implementação de Armazenamento Centralizado com Conexão iSCSI:
Para garantir uma gestão eficiente e escalável dos dados, implementamos um sistema de armazenamento conectado aos três servidores via iSCSI (Internet Small Computer System Interface). Esse armazenamento centralizado permite a consolidação dos arquivos do file server e das máquinas virtuais em um único local, simplificando a administração e proporcionando um alto desempenho de acesso aos dados.

### Expansão da Infraestrutura de Rede com Pontos de Acesso (APs):
Para atender às necessidades de conectividade sem fio em diferentes áreas da nossa organização, implantamos quatro Pontos de Acesso (APs):

- Dois Access Point`s FortiAp para o Armazém: Esses pontos de acesso garantem uma cobertura confiável e de alta velocidade para dispositivos móveis e equipamentos sem fio utilizados no ambiente de armazenagem, melhorando a comunicação e a produtividade dos colaboradores nessa área.
- Dois Access Point`s FortiAp para o Setor Administrativo: Os pontos de acesso dedicados ao setor administrativo oferecem uma conectividade estável e segura para os dispositivos dos colaboradores e visitantes que trabalham nas áreas administrativas, promovendo uma colaboração eficiente e facilitando o acesso aos recursos de rede.

É importante ressaltar que, com uma capacidade de 512 clientes por AP e uma demanda estimada de 270 usuários no armazém e 230 usuários no setor administrativo, nosso projeto de rede sem fio apresenta uma significativa sobra de capacidade. Isso significa que nossa infraestrutura está bem dimensionada para atender às necessidades atuais e futuras de conectividade, garantindo uma experiência de usuário satisfatória e evitando possíveis gargalos ou congestionamentos na rede.

## Autenticação

### Autenticação Centralizada e Segura com LDAP via FortiGate:
Para garantir um acesso à rede seguro e controlado, implementamos a autenticação centralizada utilizando o protocolo LDAP (Lightweight Directory Access Protocol) através do nosso firewall FortiGate. Este sistema permite a integração com o nosso servidor Active Directory (AD), permitindo aos usuários fazerem login na rede utilizando suas credenciais do AD. Isso simplifica o gerenciamento de contas de usuário e reforça a segurança da nossa rede, ao mesmo tempo em que proporciona uma experiência de login transparente para os usuários.

### Conexão Segura com Azure via VPN Site-to-Site:
Estabelecemos uma conexão VPN Site-to-Site com a plataforma Azure da Microsoft, criando um túnel seguro entre a nossa rede local e a infraestrutura de nuvem do Azure. Essa conexão permite uma comunicação segura e confiável entre os recursos locais e os serviços hospedados na nuvem do Azure, facilitando a expansão dos nossos sistemas e aplicativos para a nuvem de forma transparente e segura.

### Integração Híbrida com o Microsoft Azure utilizando o Microsoft Azure AD Connect Sync:
Para uma integração eficiente e simplificada dos nossos serviços locais com os recursos de nuvem do Azure, implementamos o Microsoft Azure AD Connect Sync. Esta ferramenta permite a sincronização bidirecional de identidades entre o nosso ambiente local, baseado no Active Directory, e o Azure Active Directory (Azure AD). Com isso, podemos alcançar uma verdadeira nuvem híbrida, onde os usuários, grupos e políticas de segurança são consistentes em ambos os ambientes, facilitando a gestão de identidades e o acesso aos recursos tanto localmente quanto na nuvem.

## Ambiente WEB APP em Nuvem
### Ambientes de Desenvolvimento e Produção no Azure:
No Microsoft Azure, criamos ambientes distintos para o desenvolvimento e produção dos nossos aplicativos, garantindo uma segregação adequada entre as fases de desenvolvimento e implantação. Em ambos os ambientes, utilizamos serviços gerenciados para oferecer uma infraestrutura confiável e escalável.

### Ambiente de Desenvolvimento (DevTest Labs): Nesse ambiente, realizamos testes e experimentos utilizando um conjunto de serviços específicos:
- App Service para Backend (dotnet8): Hospeda a lógica de negócios e a API do nosso aplicativo, construído na plataforma .NET Core 8.
- App Service para Frontend (Node.js): Oferece uma interface de usuário responsiva e interativa, desenvolvida utilizando Node.js e tecnologias web modernas.
- Banco de Dados de Testes: Fornece um ambiente isolado para testes de integração e validação dos nossos aplicativos, utilizando um banco de dados dedicado.

### Ambiente de Produção: Nesse ambiente, implantamos os nossos aplicativos para oferecer serviços aos usuários finais com alta disponibilidade e desempenho:
- App Service de Produção para Frontend (Node.js): Hospeda a versão estável e escalável do nosso frontend, garantindo uma experiência consistente para os usuários.
- App Service de Produção para Backend (dotnet8): Oferece um backend robusto e confiável para suportar as operações críticas do nosso aplicativo, construído com a última versão do framework .NET.
- Banco de Dados de Produção: Armazena os dados essenciais do nosso aplicativo em um ambiente seguro e altamente disponível, garantindo a integridade e a confiabilidade dos dados dos usuários.

### Armazenamento e Gerenciamento de Logs no Azure:
Para garantir uma análise eficaz e uma gestão proativa dos nossos aplicativos e infraestrutura, utilizamos serviços de armazenamento e monitoramento no Azure:
- Storage Account: Armazena imagens, logs de aplicativos e logs do servidor web, garantindo a disponibilidade e integridade dos dados.

- Azure Monitor, Application Insights e Log Analytics Workspace: Oferecem recursos avançados de monitoramento, diagnóstico e análise de desempenho, permitindo identificar e resolver problemas rapidamente, além de fornecer insights valiosos para otimização e melhoria contínua dos nossos serviços.

## Gerenciamento de Código e Automação de Deploy no Azure:
No Microsoft Azure, adotamos uma abordagem de integração contínua (CI) e entrega contínua (CD) para automatizar o processo de desenvolvimento, teste e implantação dos nossos aplicativos. Utilizamos uma variedade de serviços no Azure para facilitar esse fluxo de trabalho:

- Repositório de Código: Armazenamos o código-fonte dos nossos aplicativos em um repositório seguro no Azure, proporcionando uma colaboração eficiente e um controle de versão preciso para o nosso desenvolvimento de software.
- Azure Pipelines (PR): Utilizamos o Azure Pipelines para automatizar o processo de revisão de código e pull requests (PR), garantindo a qualidade e consistência do nosso código antes de ser mesclado no repositório principal.
- Azure Pipelines (CI): Configuramos pipelines de integração contínua (CI) no Azure Pipelines para automatizar a compilação, teste e análise do nosso código sempre que uma alteração é feita no repositório, garantindo uma rápida detecção de problemas e uma entrega de código confiável.
- Azure Pipelines (CD): Implementamos pipelines de entrega contínua (CD) no Azure Pipelines para automatizar o processo de implantação dos nossos aplicativos nos ambientes de teste e produção, garantindo uma implantação rápida, consistente e segura dos nossos aplicativos.
