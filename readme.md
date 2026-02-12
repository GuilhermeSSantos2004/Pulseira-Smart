# 🔵 QPulse — Pulseira Inteligente de Segurança Infantil

> Sistema vestível que monitora a distância entre pais e filhos em tempo real, enviando alertas automáticos quando a criança se afasta além do limite seguro.

---

## 📌 Sobre o Projeto

O **QPulse** é uma pulseira inteligente projetada para crianças, pensada para ambientes como praias, parques, shoppings e eventos. A pulseira se comunica com o celular dos pais e dispara notificações progressivas conforme a criança se afasta, ajudando a prevenir situações de perda ou desaparecimento.

O projeto foi desenvolvido como protótipo funcional utilizando **ESP32** e simulado no **Wokwi**.

---

## 🎯 Problema

Todos os anos milhares de crianças se perdem temporariamente em locais públicos. Pais frequentemente perdem o contato visual com os filhos em ambientes movimentados, e quando percebem, a criança já está longe. Soluções existentes como GPS são caras e dependem de planos de dados. O QPulse oferece uma alternativa acessível baseada em proximidade.

---

## 💡 Como Funciona

O sistema opera com dois dispositivos que se comunicam entre si:

**Pulseira (na criança):**
- Monitora a distância em relação ao dispositivo dos pais
- Possui botão de emergência SOS (pressionar 3 segundos)
- Indica o status por LED colorido
- Exibe nível de bateria

**App/Dispositivo dos Pais:**
- Recebe notificações automáticas quando a criança se afasta
- Mostra a distância em tempo real com barra visual
- Exibe a localização aproximada da criança
- Emite alarmes sonoros progressivos

### Níveis de Alerta

| Distância | Status | LED | Notificação ao Pai |
|-----------|--------|-----|---------------------|
| 0 – 30% (até ~150m) | **Seguro** | 🟢 Verde | Nenhuma |
| 30 – 60% (~150m a 300m) | **Alerta** | 🟡 Amarelo | Vibração + notificação: *"Seu filho está se afastando"* |
| Acima de 60% (>300m) | **Crítico** | 🔴 Vermelho | Alarme sonoro + notificação urgente: *"Seu filho está muito longe!"* |
| Botão SOS | **Emergência** | 🔴 Piscante | Alarme máximo + localização GPS enviada |

---

## 🔔 Sistema de Notificações

Quando a criança ultrapassa a zona segura, o pai recebe alertas de forma progressiva:

1. **Zona de Alerta (amarela):** dois bipes curtos + LED amarelo na pulseira. O dispositivo do pai exibe a mensagem de alerta e a distância atual.

2. **Zona Crítica (vermelha):** alarme rápido e intermitente + LED vermelho piscante. O dispositivo do pai mostra a localização aproximada da criança e emite som contínuo até o pai confirmar.

3. **SOS ativado pela criança:** a criança pressiona o botão por 3 segundos. O dispositivo do pai recebe imediatamente as coordenadas GPS, endereço aproximado e alarme máximo.

---

## 🛠️ Componentes do Protótipo

| Componente | Função | Pino ESP32 |
|------------|--------|------------|
| ESP32 DevKit V1 | Microcontrolador principal | — |
| Display OLED SSD1306 128x64 | Interface visual (pulseira + pai) | SDA: 21, SCL: 22 |
| LED Vermelho | Indicador de alerta crítico | GPIO 25 |
| LED Verde | Indicador de zona segura | GPIO 26 |
| LED Azul | Indicador de inicialização | GPIO 27 |
| Buzzer | Alarmes sonoros | GPIO 23 |
| Botão (vermelho) | SOS de emergência | GPIO 15 |
| Potenciômetro | Simula a distância criança–pai | GPIO 34 |
| 3x Resistores 220Ω | Proteção dos LEDs | — |

---

## 📐 Diagrama do Circuito

```
                    ┌─────────────────┐
                    │     ESP32       │
                    │                 │
   OLED SDA ◄──────┤ GPIO 21         │
   OLED SCL ◄──────┤ GPIO 22         │
                    │                 │
   LED Verm ◄──220Ω┤ GPIO 25         │
   LED Verde◄──220Ω┤ GPIO 26         │
   LED Azul ◄──220Ω┤ GPIO 27         │
                    │                 │
   Buzzer   ◄──────┤ GPIO 23         │
   Botão SOS◄──────┤ GPIO 15 (PULLUP)│
   Potenciô.◄──────┤ GPIO 34 (ADC)   │
                    └─────────────────┘
```

---

## 🚀 Como Executar

### No Wokwi (simulação online)

1. Acesse [wokwi.com](https://wokwi.com)
2. Crie um novo projeto **ESP32**
3. Cole o conteúdo do arquivo `sketch.ino`
4. Substitua o `diagram.json` pelo arquivo fornecido
5. Clique em **Play**
6. Use o **potenciômetro** para simular a criança se afastando
7. Pressione o **botão vermelho** por 3 segundos para acionar o SOS

### No hardware real

1. Monte o circuito conforme o diagrama
2. Instale as bibliotecas no Arduino IDE:
   - `Adafruit SSD1306` (v2.5.7)
   - `Adafruit GFX Library` (v1.11.3)
3. Selecione a placa **ESP32 Dev Module**
4. Faça o upload do código
5. Abra o Monitor Serial (115200 baud) para acompanhar os logs

---

## 📱 Fluxo de Uso Real (Visão do Produto)

```
Pai coloca a pulseira na criança
            │
            ▼
Pulseira sincroniza com app do pai via Bluetooth
            │
            ▼
Criança brinca normalmente ──► LED Verde ──► Pai tranquilo
            │
            ▼
Criança começa a se afastar
            │
            ├── 150m ──► LED Amarelo ──► Pai recebe: "João está se afastando"
            │
            ├── 300m ──► LED Vermelho ──► Pai recebe: "ALERTA! João está longe!"
            │                              + localização no mapa
            │
            └── Criança aperta SOS ──► Pai recebe: coordenadas GPS
                                        + alarme máximo
                                        + endereço aproximado
```

---

## 📂 Estrutura dos Arquivos

```
QPulse/
├── sketch.ino          # Código principal do ESP32
├── diagram.json        # Diagrama de componentes do Wokwi
└── README.md           # Este arquivo
```

---

## 🔮 Melhorias Futuras

- **Bluetooth Low Energy (BLE):** comunicação real entre pulseira e celular do pai, medindo distância pelo RSSI do sinal
- **App mobile (Flutter/React Native):** interface no celular dos pais com mapa, histórico e configuração de zonas seguras
- **GPS real (módulo NEO-6M):** localização precisa da criança via satélite
- **Notificações push:** alertas no celular mesmo com app em segundo plano
- **Cerca virtual (Geofencing):** definir áreas seguras no mapa e alertar ao sair delas
- **Múltiplas pulseiras:** monitorar mais de uma criança simultaneamente
- **Sensor de remoção:** alertar se a pulseira for retirada do pulso
- **Bateria recarregável:** com indicador real de carga e alerta de bateria fraca
- **Resistência à água:** encapsulamento IP67 para uso em praias e piscinas

---

## 👥 Equipe

Projeto desenvolvido como protótipo acadêmico de IoT para segurança infantil.