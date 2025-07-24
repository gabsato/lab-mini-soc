# Laboratório de Análise de Ameaças com Wazuh (Mini SOC)

## 1. Resumo do projeto
Este projeto consistiu na criação de um laboratório de cibersegurança (Mini SOC) totalmente funcional, utilizando ferramentas open-source para simular um ambiente corporativo. O objetivo foi implementar uma solução de monitoramento contínuo com o Wazuh (SIEM/EDR), configurar agentes em diferentes sistemas operacionais (Windows e Linux), simular uma ameaça interna e analisar os alertas gerados. O projeto abrangeu desde a arquitetura da infraestrutura virtual até à investigação de eventos de segurança, passando pela resolução metódica de desafios de conectividade e configuração.

## 2. Objetivo do Projeto
O objetivo principal foi desenvolver competências práticas e demonstráveis em operações de segurança (SecOps), alinhadas com as funções de um analista de segurança da informação. Os objetivos específicos foram:
- Implementar um SIEM: Instalar e configurar o Wazuh como solução centralizada de monitoramento.
- Gerir endpoints: Integrar agentes Wazuh em sistemas Windows e Linux, simulando uma rede heterogênea.
- Detectar ameaças: Simular uma tentativa de escalonamento de privilégios e validar a capacidade de deteção do SIEM.
- Analisar eventos: Investigar os alertas gerados, utilizando a interface do Wazuh para filtrar e correlacionar dados.
- Documentar processos: Criar um relatório técnico detalhado, documentando todas as fases, desafios e conclusões do projeto.

## 3. Arquitetura do Laboratório
A infraestrutura foi construída sobre o Oracle VirtualBox, utilizando uma rede segmentada para simular um ambiente seguro e controlado.
- *Software de Virtualização: Oracle VM VirtualBox 7.x*
- *Componentes Virtuais:*
  - Servidor Wazuh: Imagem OVA oficial do Wazuh 4.x.
  - Endpoint Windows: Windows 10 Pro (Máquina de Avaliação).
  - Endpoint Linux: Ubuntu Desktop 24.04 LTS.
  - Estação de Ataque: Kali Linux.
- *Configuração de Rede:*
Foi utilizada uma rede do tipo "Host-Only Adapter" (192.168.56.0/24) para permitir a comunicação entre todas as máquinas virtuais e o computador hospedeiro, de forma isolada e segura.
Para os endpoints (Windows e Ubuntu) que necessitavam de acesso à internet para a instalação dos agentes, foi configurado um segundo adaptador de rede do tipo NAT, demonstrando uma configuração de rede mais complexa e realista.

### *Diagrama da Rede:*
![Diagrama da rede do laboratório](https://github.com/gabsato/lab-mini-soc/blob/main/assets/diagrama-da-rede.png?raw=true)


## 4. Ferramentas e Metodologias
Para a construção e validação deste laboratório, foram utilizadas ferramentas padrão da indústria e aplicadas metodologias fundamentais de Operações de Segurança (SecOps).

***Ferramentas***
- *Wazuh (SIEM / EDR):* Utilizado como o cérebro do laboratório, atuando como uma solução integrada de SIEM para a centralização, correlação e análise de logs, e como um EDR (Endpoint Detection and Response) através dos seus agentes, que forneceram monitoramento de segurança contínuo nos endpoints.
- *Oracle VM VirtualBox:* Usado para a criação de toda a infraestrutura virtual, permitindo a simulação de múltiplos sistemas operacionais e a segmentação de redes em um ambiente controlado e isolado.
- *Nmap (Network Mapper):* Usado como a principal ferramenta de reconhecimento de rede para simular a fase inicial de um ataque. As suas capacidades de varredura de portas foram utilizadas para testar a visibilidade dos endpoints e a capacidade de deteção do SIEM.
- *Kali Linux:* Utilizado como a plataforma ofensiva (Red Team), servindo como base para o lançamento das atividades de reconhecimento com o Nmap contra os alvos na rede do laboratório.
- *PowerShell & Bash:* Empregados como as interfaces de linha de comando para administração e automação nos endpoints Windows e Linux, respectivamente, sendo cruciais para a instalação e configuração dos agentes Wazuh.

***Metodologias***
- *Troubleshooting Metódico:* Aplicação de um processo lógico de diagnóstico para resolver falhas técnicas. O processo incluiu a verificação de status de serviços (systemctl), análise de conectividade de rede e reconfiguração de adaptadores virtuais para solucionar erros como ERR_CONNECTION_REFUSED e falhas de resolução de DNS.
- *Análise de eventos de segurança:* Utilização da plataforma SIEM (Wazuh Discover) para filtrar, pesquisar e analisar eventos, distinguindo entre logs operacionais rotineiros e alertas de segurança acionáveis. O processo culminou na identificação de um alerta de Nível 10, referente a uma tentativa de escalonamento de privilégios (sudo failure).
- *Simulação de ameaças (Red Team vs. Blue Team):* Emulação de táticas adversárias (Red Team) para testar as capacidades de deteção da infraestrutura. A análise defensiva subsequente dos alertas gerados (Blue Team) validou a eficácia da pipeline de monitoramento, desde a coleta do log no endpoint até à sua visualização no dashboard.
- Arquitetura de segurança em camadas (Defense in Depth): Desenho e implementação de uma rede segmentada (Host-Only + NAT) como uma medida de segurança fundamental. Esta abordagem isolou o ambiente de laboratório e controlou os fluxos de comunicação, demonstrando a aplicação prática do conceito de defesa em profundidade.

## 5. Execução e Desafios
A execução do projeto seguiu um plano estruturado, mas encontrou desafios realistas que exigiram a aplicação de conhecimentos de troubleshooting.
### 5.1. Instalação e Configuração
A fase inicial consistiu na instalação do VirtualBox e na criação das máquinas virtuais, seguida pela importação do servidor Wazuh e instalação dos sistemas operacionais Windows e Ubuntu.
### 5.2. Desafios de Conectividade e Resolução
Durante a configuração, surgiram dois desafios principais que simulam problemas comuns em ambientes de TI:
1. Erro "ERR_CONNECTION_REFUSED": Ao tentar acessar ao dashboard, o navegador recusou a conexão e ao investigar no terminal do servidor, com o comando systemctl status, revelou que os serviços do Wazuh (indexer, dashboard) não estavam totalmente ativos. A solução foi reiniciar os serviços na ordem correta, garantindo que as dependências fossem satisfeitas.
2. Erro "name could not be resolved": Durante a instalação dos agentes, as VMs não conseguiam baixar os pacotes da internet. O diagnóstico revelou que a rede "Host-Only" não providencia acesso à internet. A solução foi implementar uma arquitetura de rede com dois adaptadores para as VMs de endpoint, um "Host-Only" para a comunicação interna com o SIEM e um "NAT" para o acesso externo.
### 5.3  Simulação de ameaça e detecção
A validação final do laboratório foi feita através da simulação de uma ameaça. A primeira tentativa, um scan de portas com Nmap, foi executada com sucesso, mas não gerou alertas, indicando que a configuração padrão do agente não era sensível a este tipo de ruído.
Para garantir a validação da pipeline de alertas, uma segunda simulação mais assertiva foi realizada: uma tentativa de escalonamento de privilégios no endpoint Linux, forçando falhas de autenticação com o comando sudo.

## 6. Análise de Resultados e Evidências
A simulação de falha de sudo foi imediatamente detectada pelo agente Wazuh no Ubuntu e reportada ao servidor, gerando um alerta de alta severidade.
![Three failed attempts to run sudo](https://github.com/gabsato/lab-mini-soc/blob/main/assets/wazuh-alert-rule-10.png?raw=true)

### Análise do alerta
- rule.id: 5503, 5504: Correspondem às regras de falha de autenticação e falhas múltiplas.
- rule.level: 10: Classificado como um evento de alta severidade (Crítico), exigindo atenção imediata de um analista.
- rule.description: Three failed attempts to run sudo: Descreve claramente a natureza da ameaça.
- agent.name: zinoproxy-VirtualBox: Identifica com precisão o endpoint onde a atividade suspeita ocorreu.

## 7. Conceitos de segurança aplicados
- SIEM (Security Information and Event Management): O Wazuh foi usado como um SIEM para agregar, analisar e correlacionar dados de log de múltiplas fontes em tempo real.
- EDR (Endpoint Detection and Response): O agente Wazuh atuou como um EDR, monitorando ativamente os endpoints em busca de atividades maliciosas e fornecendo capacidade de resposta.
- Princípio do menor privilégio: A tentativa de usar sudo e a sua detecção reforçam a importância de monitorar o uso de privilégios elevados no sistema.
- Segmentação de rede: O uso de uma rede Host-Only demonstra o conceito de segmentar um ambiente para controlar o tráfego e aumentar a segurança.

## Conclusões e aprendizados
Este projeto foi uma jornada prática e aprofundada pelo ciclo de vida de uma operação de segurança. Os desafios encontrados, longe de serem obstáculos, foram as melhores oportunidades de aprendizado, forçando uma investigação metódica de problemas de rede, serviços e configuração, competências essenciais para qualquer profissional de cibersegurança.
A conclusão é que a implementação de um SIEM como o Wazuh é uma ferramenta poderosa para ganhar visibilidade sobre o ambiente e detectar ameaças, mas o seu verdadeiro valor é desbloqueado pela capacidade do analista de configurar, manter e, principalmente, investigar os alertas gerados.
