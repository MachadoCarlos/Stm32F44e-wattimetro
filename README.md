# Wattímetro RF com STM32F446 

Este repositório contém o firmware e a documentação para um wattímetro de rádio frequência (RF) desenvolvido para medir a potência de saída de unidades eletrocirúrgicas (bisturis elétricos). O projeto é baseado no microcontrolador **STM32F446** e focado em metrologia de equipamentos biomédicos.

Como os wattímetro operam em altas frequências (tipicamente entre 300 kHz e 750 MHz) e com formas de onda complexas , este sistema utiliza amostragem de alta velocidade e processamento digital de sinais para calcular a potência real (Watts) entregue a uma carga de teste com precisão.

##  Arquitetura do Sistema

### Hardware
* **Microcontrolador:** STM32F446 (ARM Cortex-M4 @ 180MHz com FPU e DSP)
* **Aquisição de Tensão (V):** Circuito atenuador (divisor resistivo/capacitivo compensado para RF)
* **Aquisição de Corrente (I):** Transformador de corrente (TC) de banda larga toroidal
* **Carga de Teste:** Resistores de potência não-indutivos (ex: 300Ω, 500Ω) 
* **Interface de Comunicação:** USB/UART para envio de dados a um software de supervisão no PC

### Firmware / Software
* **Amostragem:** Utilização dos ADCs nativos do STM32 em modo *Dual Regular Simultaneous* acoplados ao **DMA** para garantir aquisição contínua de V e I em alta taxa de amostragem (MSPS), sem gargalos na CPU.
* **Processamento de Sinal (DSP):** Cálculo de RMS real de tensão e corrente, e a obtenção da potência ativa.
* **Filtragem:** Filtros digitais FIR/IIR aplicados via biblioteca CMSIS-DSP para atenuação de ruídos espúrios.
* **RTOS:** [Opcional] Estruturado com FreeRTOS para separação eficiente entre as tarefas de aquisição (prioridade crítica) e comunicação (prioridade baixa).

## 📂 Estrutura do Repositório

```text
├── Core/
│   ├── Inc/               # Headers principais (Main, HAL config, etc)
│   └── Src/               # Código fonte (Main, inicialização de periféricos)
├── Hardware/              # Esquema elétrico preliminar e layout da placa
├── Docs/                  # Documentação técnica, requisitos metrológicos e calibração
└── watimetro.ioc          # Arquivo de configuração do STM32CubeMX
