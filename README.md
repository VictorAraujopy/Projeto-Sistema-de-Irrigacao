# 🌿 FarmTech Solutions - Sistema de Irrigação Inteligente (Fase 2)

## 1. Visão Geral do Projeto

Este repositório documenta a Fase 2 do projeto de **Sistema de Irrigação Inteligente**, desenvolvido pela FarmTech Solutions. O sistema utiliza um microcontrolador ESP32, simulado na plataforma **Wokwi.com**, para otimizar o uso da água na agricultura.

O objetivo é monitorar em tempo real fatores críticos do solo — **Umidade**, **pH** e a presença de **Nutrientes (NPK)** — para acionar de forma autônoma um sistema de irrigação, garantindo a saúde da lavoura e a economia de recursos hídricos.

**➡️ Link para o Projeto no Wokwi:** https://wokwi.com/projects/444184023624515585
---

## 2. 🔌 Componentes e Hardware Simulado

Devido às limitações da plataforma de simulação, utilizamos componentes análogos para representar sensores agrícolas, conforme a tabela abaixo:

| Componente Real | Simulação no Wokwi | Pino (GPIO) | Função no Projeto |
| :--- | :--- | :--- | :--- |
| **Sensor de Umidade do Solo** | Sensor **DHT22** | `15` | Mede a umidade em tempo real (%). |
| **Sensor de pH do Solo** | Sensor **LDR** (Resistor Dependente de Luz) | `34` (ADC) | Simula a escala de pH através de um valor analógico (0-4095). |
| **Alerta de Nutrientes (N, P, K)** | **3x Botões** | `21`, `22`, `23` | Simulam um **alerta de deficiência**. Pressionar um botão indica que o nutriente correspondente está em falta. |
| **Bomba D'água** | **Módulo Relé** | `4` | Atua como um interruptor para ligar ou desligar a irrigação. |

---

## 3. 🧠 Lógica de Controle Inteligente

O sistema foi otimizado para a cultura de **Alface (Lactuca sativa)**, que prospera em solo consistentemente úmido e com pH próximo da neutralidade.

A nossa lógica de decisão é conservadora e inteligente, garantindo que a irrigação só ocorra sob condições ideais para não prejudicar o solo ou a planta.

> **Regra Principal:** A irrigação (Relé) será acionada **SE E SOMENTE SE** todas as condições a seguir forem verdadeiras simultaneamente:

1.  **Solo Seco:** A umidade lida pelo DHT22 for **inferior a 60%**.
2.  **pH Ideal:** O nível de pH simulado pelo LDR estiver na faixa ideal, entre **1500 e 2500**.
3.  **Solo Saudável:** **NENHUM** botão de alerta de deficiência de nutriente (N, P ou K) estiver pressionado.

Se qualquer uma dessas condições não for atendida, a irrigação é interrompida ou permanece desligada.

---

## 4. 🏗️ Estrutura do Código (`sketch.ino`)

O código em C++/Arduino foi estruturado para ser legível e modular:

* **Definições (`#define`):** No topo do arquivo, todos os pinos dos componentes são mapeados para nomes fáceis de entender (ex: `BOMBA_PIN`), facilitando a manutenção.
* **`setup()`:** Prepara o ambiente, iniciando a comunicação Serial, o sensor DHT e configurando os pinos (`pinMode`) como entrada (`INPUT_PULLUP` para os botões) ou saída (`OUTPUT` para o relé).
* **`controlarBomba(bool ligar)`:** Uma função auxiliar que abstrai o controle do relé. Ela recebe `true` para ligar a bomba ou `false` para desligar, além de imprimir o status da ação no monitor.
* **`loop()`:** O coração do sistema. A cada 2 segundos, ele executa o ciclo:
    1.  **Lê** os dados de todos os sensores (DHT, LDR, Botões).
    2.  **Exibe** os dados brutos no Monitor Serial para depuração.
    3.  **Decide**, com base na lógica de controle (Seção 3), se a bomba deve ser ligada ou desligada.
    4.  **Age**, chamando a função `controlarBomba()` para executar a decisão.

---

## 5. 📈 Diagrama de Conexão do Circuito

A imagem abaixo ilustra as conexões físicas entre o ESP32 e os componentes no ambiente de simulação Wokwi.

<img width-="1062" height="657" alt="image" src="https://github.com/user-attachments/assets/b6ba827b-dcdc-4715-bdb4-8e80bf9121ac" />

---

## 6. 🚀 Como Executar e Testar o Projeto

1.  Acesse o **link do projeto** na seção 1.
2.  No Wokwi, abra a aba **`Library Manager`** e confirme que a biblioteca **"DHT sensor library by Adafruit"** está instalada.
3.  Clique no botão verde **`▶️ Start Simulation`**.
4.  Para testar a lógica, crie o **"cenário perfeito"** para a irrigação:
    * Ajuste a umidade do **DHT22** para um valor **< 60%**.
    * Ajuste a luz do **LDR** até o valor de pH no Monitor Serial ficar **entre 1500 e 2500**.
    * **Não pressione nenhum botão**.
    * ✅ **Resultado esperado:** A bomba (relé) deve ligar.
5.  Agora, **pressione qualquer um dos botões** (N, P ou K).
    * ❌ **Resultado esperado:** A bomba deve desligar, provando que o sistema de segurança está funcionando.

---

## 7. 👥 Equipe (FarmTech Solutions)

* **Victor Araujo Ferreira da Silva** - Engenheiro de softwere 
* **Filipe Marques Previato** - Engenheiro de softwere 2
* **Pedro Zanon Castro Santana** - Engenheiro de Hardwere
* **Jonathan Gomes Ribeiro Franco** - tester/videomaker
