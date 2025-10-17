# 🌡️ Projeto: Monitoramento IoT - Vinheria Agnello

Este projeto tem como objetivo monitorar **temperatura**, **umidade** e **luminosidade** de um ambiente utilizando um **ESP32**, enviando os dados em tempo real via **MQTT** para um broker configurado.  

> 📡 Ideal para aplicações de IoT em ambientes controlados, como vinícolas, estufas e laboratórios.

---

## 🧰 Tecnologias e Bibliotecas

- [ESP32](https://www.espressif.com/en/products/socs/esp32)
- WiFi
- [PubSubClient](https://pubsubclient.knolleary.net/) — comunicação MQTT
- [DHT Sensor Library](https://github.com/adafruit/DHT-sensor-library)
- Sensor **DHT11/DHT22**
- Sensor **LDR** (Luminosidade)

---

## 🪛 Funcionalidades

- 📤 **Publicação automática de dados** de temperatura, umidade e luminosidade via MQTT.  
- 💡 **Controle remoto do LED onboard** do ESP32 através de comandos MQTT.  
- 🔁 Reconexão automática ao Wi-Fi e ao Broker em caso de queda de conexão.  
- 🧾 Envio de dados em **formato JSON** no tópico principal, além de tópicos separados para cada sensor.

---

## 📡 Estrutura MQTT

| Tópico                          | Direção     | Descrição                        |
|----------------------------------|-------------|------------------------------------|
| `/TEF/device011/cmd`            | Subscribe   | Recebe comandos (ex.: `on`, `off`) para o LED |
| `/TEF/device011/attrs`          | Publish     | Publica JSON com todos os dados |
| `/TEF/device011/attrs/temp`     | Publish     | Publica temperatura (°C) |
| `/TEF/device011/attrs/umid`     | Publish     | Publica umidade (%) |
| `/TEF/device011/attrs/luz`      | Publish     | Publica luminosidade (%) |

### 📝 Exemplo de Payload JSON:
```json
{
  "temp": 25.60,
  "umid": 58.30,
  "luz": 80,
  "led": "on"
}
```

---

## ⚙️ Configuração de Hardware

| Componente        | Pino ESP32  | Observações                    |
|--------------------|------------|--------------------------------|
| DHT11 / DHT22      | GPIO 4     | Definir tipo de sensor no código |
| LDR (sensor luz)   | GPIO 34    | Entrada analógica              |
| LED Onboard        | GPIO 2     | Controle remoto via MQTT       |

---

## 🌐 Configuração de Rede

No código, atualize as credenciais de Wi-Fi e Broker MQTT:

```cpp
const char* SSID        = "NOME_DA_REDE";
const char* PASSWORD    = "SENHA_DA_REDE";
const char* BROKER_MQTT = "IP_DO_BROKER";
const int   BROKER_PORT = 1883;
```

---

## 📥 Instalação e Uso

1. **Clonar o repositório:**
   ```bash
   git clone https://github.com/seu-usuario/nome-do-repositorio.git
   ```

2. **Abrir o código no [Arduino IDE](https://www.arduino.cc/en/software)** ou **[PlatformIO](https://platformio.org/)**.

3. **Instalar as bibliotecas necessárias:**
   - `WiFi.h` (inclusa no core do ESP32)
   - `PubSubClient`
   - `DHT sensor library`
   - `Adafruit Unified Sensor` (dependência)

4. **Selecionar a placa ESP32** no menu da IDE:
   - Placa: `ESP32 Dev Module`
   - Porta: correta do seu dispositivo

5. **Fazer upload do código** para o ESP32.

6. **Monitorar a saída serial**:
   - Baud rate: `115200`

---

## 🧠 Comandos MQTT para LED

- `on` → Acende o LED onboard (GPIO 2)  
- `off` → Apaga o LED onboard

> Enviar no tópico: `/TEF/device011/cmd`

---

## 🧪 Exemplo de Teste com MQTT

Você pode testar a comunicação usando ferramentas como:
- [MQTT Explorer](https://mqtt-explorer.com/)
- `mosquitto_pub` e `mosquitto_sub`
- Plataformas FIWARE ou brokers locais

### Publicar comando:
```bash
mosquitto_pub -h 54.227.182.115 -t "/TEF/device011/cmd" -m "on"
```

### Escutar dados:
```bash
mosquitto_sub -h 54.227.182.115 -t "/TEF/device011/attrs"
```

---

