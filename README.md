# Laboratório de Análise de Ameaças com Wazuh (Mini SOC)

## 1. Resumo Executivo
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
