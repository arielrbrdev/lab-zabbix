# 🛡️ Monitoramento Distribuído e Segurança: Zabbix Proxy e Automação PSK

## 💡 Sobre o Projeto

Este laboratório focou na implementação de uma solução de **monitoramento distribuído** utilizando a arquitetura de Proxy do Zabbix. O objetivo principal foi garantir que a coleta de métricas de hosts remotos fosse escalável e, sobretudo, **segura**.

O projeto demonstrou a proficiência nas seguintes áreas:
* **Escalabilidade:** Implementação do **Zabbix Proxy** para consolidar o tráfego de agentes remotos.
* **Segurança:** Configuração de criptografia **TLS com Chaves Pré-Compartilhadas (PSK)** para proteger a comunicação entre os componentes.
* **DevSecOps/Automação:** Desenvolvimento de um **Script BASH** para automatizar a instalação e configuração do Zabbix Agent.

---

## ⚙️ Tecnologias e Arquitetura

| Categoria | Tecnologia/Ferramenta | Função no Projeto |
| :--- | :--- | :--- |
| **Monitoramento** | **Zabbix Proxy** | Recebe dados dos agentes remotos e repassa ao Servidor Principal. |
| **Segurança** | **TLS com PSK (Pre-Shared Keys)** | Utilizado para proteger a comunicação na rede de monitoramento. |
| **Automação** | **Script BASH** | Automatiza a instalação, configuração e habilitação do `zabbix-agent`. |
| **Ambiente** | **Máquinas Virtuais (VMs)** | Simulação do Host Remoto, Zabbix Proxy e Servidor Principal. |

### Topologia Implementada

A arquitetura seguiu um fluxo de monitoramento distribuído:

> Host Remoto (Agente Zabbix) → Rede Remota (Zabbix Proxy) → Servidor Principal (Zabbix Server, Frontend, BD). 

---

## 🚀 Detalhes da Execução

### 1. Configuração de Criptografia Segura

A comunicação foi securitizada usando criptografia PSK:

* **Mecanismo:** Chaves pré-compartilhadas (PSK) foram configuradas e utilizadas para a comunicação.
* **Identidade PSK:** A identidade **`Proxy-Remoto-PSK`** foi configurada no *frontend* Zabbix para autenticar os hosts.
* **Validação:** A coleta de métricas foi validada na interface web, embora tenha sido observado um erro de identificação TLS em um dos hosts (`DM-002`), necessitando de *troubleshooting*.

### 2. Automação do Agente (Script BASH)

Um *script* BASH foi desenvolvido para agilizar o *deployment* do agente, solicitando o IP de destino e o `Hostname` e reescrevendo o arquivo `zabbix_agentd.conf` automaticamente.

```bash
# Trecho do Script: Coleta de Inputs
Read -p "Digite o IP do Servidor ou Proxy Zabbix: "ZABBIX_ENDPOINT_IP
Read -p "Digite um nome único para este agente: " AGENT_HOSTNAME

# Trecho do Script: Configuração do Hostname
Sed -i "s/^Hostname=Zabbix server/Hostname=\${AGENT_HOSTNAME}/" "\$CONFIG_FILE"
