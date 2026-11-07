# Nó-de-monitoramento-industrial

**Status:** em desenvolvimento (fase de estruturação)

Documentação: [Escopo e requisitos](escopo_e_requisitos.md)

Nó de monitoramento industrial baseado em ESP32 (ESP-IDF e FreeRTOS). Um projeto que visa a criação de um aparelho de monitoramento que mede temperatura, vibração e corrente no local que está instalado, e se comunicar com outros dispositivos via ModBus RTU/RS-485 para então transmitir os dados a tópicos MQTT que serão disponibilizados via um dashboard feito com NODE-RED para o usuário.

Também é prevista atualização remota (OTA) com rollback e proteções para operar em ambiente industrial.

## Diagrama de blocos

<img width="1102" height="1202" alt="diagrama_no_industrial_v1 drawio (1)" src="https://github.com/user-attachments/assets/ff1e06a5-55db-4a4c-b3e6-2985e28c73ce" />

Nesse diagrama é possível visualizar todas as partes relevantes para o funcionamento do dispositivo.
Linhas contínuas representam a alimentação e linhas tracejadas representam os barramentos de dados.
