# Escopo e requisitos - nó de monitoramento industrial com ESP32

## Escopo

O projeto consiste na criação de um dispositivo baseado em ESP32 capaz de coletar dados de corrente, temperatura e vibrações no ambiente em que estiver instalado, além de dados enviados por outros equipamentos via Modbus RTU/RS-485, e publicá-los por MQTT, utilizando um broker no local, e uma central com dashboard utilizando Node-RED para exibição ao operador.

O aparelho deve operar de maneira confiável no ambiente industrial, tolerando interferência na comunicação cabeada e sem fio, picos de energia, além de condições físicas adversas, como poeira, temperatura e umidade. Meios de se realizar atualização remota e rollback também são necessários.

## Requisitos funcionais

### Aquisição de dados

- O dispositivo deve medir a corrente elétrica passando por um fio, com uma margem de erro de no máximo 5%.
- O dispositivo deve medir a média de vibração no ambiente, sendo requisitada a variação relativa em RMS.
- O dispositivo deve medir a temperatura do local, com margem de erro de 2 graus Celsius a mais ou a menos.
- Esses dados devem ser adquiridos em um ciclo de aquisição não superior a 3 segundos de duração.

### Comunicação

- O dispositivo deve se comunicar com aparelhos escravos Modbus RTU/RS-485 a uma taxa de baud de 115200 com 8 bits de dados e paridade par (pode ser modificado se necessário), com o microcontrolador ESP32 agindo como mestre.
- O limite de escravos Modbus não deve ser inferior a 5.
- Para os dispositivos conectados por Modbus, deve-se repetir a mensagem 3 vezes em caso de timeout, antes que o escravo seja marcado como offline.
- O dispositivo deve enviar os dados a tópicos via MQTT com conexão a uma rede Wi-Fi.
- O dispositivo deve implementar o QoS 1.
- Os nomes dos tópicos devem poder ser alterados através de provisionamento Wi-Fi.
- Uma cópia do histórico dos tópicos deve ser feita disponível em JSON no computador ao clique de um botão no dashboard.

### Confiabilidade

- O dispositivo deve ser capaz de tolerar instabilidades de tensão, inversões de polaridade e uma fonte capaz de providenciar de 9 a 36V.
- O dispositivo deve tolerar poeira e vibrações, sendo requisitado um gabinete IP54 ou superior.
- O dispositivo deve tolerar temperaturas que vão de +85ºC a -40ºC.
- Caso tenha perda de conexão Wi-Fi, o dispositivo deve tentar a reconexão em intervalos de 20 segundos por tempo indeterminado.
- É necessário um watchdog para lidar com casos de travamento, e o dispositivo deve reiniciar em até 15 segundos.
- Em caso de queda de rede, o dispositivo deve reter as últimas 100 leituras na memória NVS flash.
- Em caso de queda de energia, o dispositivo deve continuar funcionando por no mínimo mais 5 minutos, por meio de uma bateria.
- Em caso de queda de energia, o dispositivo também deve notificar os operadores que está sem energia.

### Atualização remota

- O dispositivo deve permitir a atualização de firmware pela rede (OTA), iniciada pelo operador por meio de um botão no dashboard.
- O firmware deve ser baixado por HTTPS e só considerado válido após o dispositivo reconectar ao broker MQTT.
- Caso o novo firmware falhe ou trave durante a inicialização, o dispositivo deve retornar automaticamente à versão anterior (rollback) e notificar o operador.
