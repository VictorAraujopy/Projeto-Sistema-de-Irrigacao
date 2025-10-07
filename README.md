# 🌿 FarmTech Solutions - Sistema de Irrigação Inteligente (Fase 2)

## 1. Introdução ao Projeto

Este repositório contém o código-fonte e a documentação do projeto de **Sistema de Irrigação Inteligente** desenvolvido pela equipe da FarmTech Solutions, utilizando o ESP32 na plataforma de simulação **Wokwi.com**.

O objetivo principal é simular o monitoramento em tempo real de fatores críticos da agricultura — **Umidade do Solo**, **pH do Solo**, e níveis de **Nutrientes NPK** — para acionar um sistema de irrigação automatizado (Relé/Bomba d'água).

**Link do Projeto Wokwi:** [https://wokwi.com/projects/444184023624515585](https://wokwi.com/projects/444184023624515585)

## 2. Componentes e Simulações Didáticas

Devido às limitações de hardware de simulação, foram utilizadas as seguintes substituições didáticas, conforme a especificação do projeto:

| Componente Real | Simulação Wokwi | Pino (GPIO) | Tipo de Sinal | Função |
| :--- | :--- | :--- | :--- | :--- |
| **Sensor de Umidade do Solo** | Sensor **DHT22** (Umidade do Ar) | `15` | Digital | Mede a umidade do solo (%). |
| **Sensor de pH do Solo** | Sensor **LDR** (Intensidade de Luz) | `34` | Analógico (ADC) | Simula a escala contínua de pH (0-4095). |
| **Nutrientes (N, P, K)** | **3 Botões** (Verdes) | `21`, `22`, `23` | Digital (INPUT_PULLUP) | Nível "Tudo ou Nada" (Pressionado = Presente). |
| **Bomba D'água** | **Módulo Relé** (Atuador) | `4` | Saída Digital | Liga/Desliga a irrigação. |

---

## 3. Lógica de Controle de Irrigação

O sistema foi otimizado para a cultura de **[INSIRA A CULTURA ESCOLHIDA AQUI, EX: CULTURA DO MILHO]**.

Nossa lógica de decisão para acionar a bomba d'água é baseada em uma combinação das condições ideais, priorizando a economia de água.

A irrigação (Relé) será acionada somente se **TODAS** as seguintes condições forem verdadeiras:

1.  **UMIDADE BAIXA:** A umidade do solo lida pelo DHT22 for **abaixo de [INSIRA A UMIDADE MÍNIMA, EX: 45.0] %**.
2.  **pH ADEQUADO:** O nível de pH (LDR) estiver **dentro da faixa ideal** para a cultura, entre **[INSIRA O VALOR MÍNIMO LDR, EX: 1500]** e **[INSIRA O VALOR MÁXIMO LDR, EX: 2500]**.
3.  **NUTRIENTES:** O Nutriente **[INSIRA UM NUTRIENTE CHAVE, EX: Fósforo (P)]** estiver em nível adequado (Botão P **Pressionado**).

*( **Nota para a Entrega:** Adapte o código C/C++ (bloco `void loop()`) para refletir esta lógica exatamente, substituindo os valores e a regra de controle. )*

---

## 4. Estrutura do Código (Pessoa 2 - Desenvolvedor de Software)

O código C/C++ (`sketch.ino`) é organizado em funções claras para monitoramento e controle:

* **`lerUmidade()`:** Utiliza a biblioteca DHT para obter o percentual de umidade.
* **`lerNivelPH_LDR()`:** Lê o valor analógico (0-4095) do GPIO 34 (LDR).
* **`lerBotao(pino)`:** Retorna `true` se o botão de nutriente for pressionado (`LOW` devido ao PULLUP).
* **`controlarBomba(bool ligar)`:** Gerencia o estado do Relé (GPIO 4), ligando (`HIGH`) ou desligando (`LOW`) a bomba.
* **Monitor Serial:** Todos os dados lidos (Umidade, pH, Status NPK) e as ações de controle são exibidas no Monitor Serial para fins de debug e validação da equipe.

---

## 5. Diagrama de Conexão

Segue o diagrama do circuito montado no Wokwi, que ilustra as conexões entre o ESP32 e os sensores/atuadores:

**[INSERIR IMAGEM DO CIRCUITO AQUI]**
*(Recomendação: Substitua esta linha pela imagem do circuito Wokwi.com conforme o entregável da Pessoa 1.)*

---

## 6. Opcionais Implementados (Ir Além)

* **[Deixe esta seção vazia ou remova se nenhum opcional for implementado.]**
* **Opcional 1 (Integração Python/API):** *[Descreva aqui o que foi feito, ex: O script Python (`api_clima.py`) foi utilizado para consultar a previsão de chuva da OpenWeather. Este dado é inserido manualmente no código C++ via variável.]*
* **Opcional 2 (Análise em R):** *[Descreva aqui a análise estatística implementada, ex: Um script R (`analise_umidade.R`) foi usado para calcular a média histórica de umidade, e o resultado influencia a faixa de acionamento da bomba.]*

---

## 7. Como Executar e Testar

1.  Acesse o link do projeto Wokwi.
2.  Verifique se a biblioteca **DHT sensor library** (Adafruit) está instalada no **Library Manager**.
3.  Inicie a simulação (botão **Play**).
4.  Abra o **Monitor Serial** e observe o fluxo de dados.
5.  **Validação:** Interaja com os *sliders* do **LDR** e do **DHT22**, e clique nos **Botões N, P e K** para validar que o **Relé** só é acionado quando as condições da Lógica de Controle (Seção 3) são satisfeitas.