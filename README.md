# 🦅 Projeto Falcon-8

<div align="center">

## 📹 Demonstração em Vídeo

[![Assista à Demonstração do Falcon-8](https://img.youtube.com/vi/-gr2yq_SHBE/maxresdefault.jpg)](https://youtu.be/-gr2yq_SHBE)

### 👆 Clique na imagem para assistir à demonstração completa

---

</div>

## 📌 Visão Geral

O **Falcon-8** é um processador didático de **arquitetura RISC de 16 bits**, inspirado na filosofia RISC-V, projetado para processamento de alto desempenho em **sistemas embarcados de tempo real**. A aplicação central é o controle dos sistemas de segurança do veículo autônomo *Dolphin Autonomous*.

---

## ⚙️ Especificações Técnicas

| Parâmetro | Valor |
|---|---|
| Arquitetura | RISC 16-bit |
| Palavra de Dados | 8 bits |
| Tamanho da Instrução | 16 bits (fixo) |
| Espaço de Endereçamento | 8 bits (256 bytes) |
| Registradores | 8 (R0–R7) |
| Implementações | Monociclo e Pipeline 3 estágios |
| Velocidade de Reação | 140–300× superior à humana |

---

## 🧠 Módulos Funcionais

### 🔴 Sistema de Frenagem Inteligente (SFI)
Monitora continuamente a **distância frontal** e a **velocidade do veículo** para detectar situações de risco e acionar os freios de forma proporcional:

- `Distância < Segurança` **E** `Velocidade > Limite` → **Emergência** (freio total)
- `Distância < Segurança` **E** `Velocidade ≤ Limite` → **Alerta** (freio normal)
- Caso contrário → **Normal** (sem acionamento)

### 🟡 Controle de Velocidade Adaptativa (CVA)
Ajusta o torque do motor via **PWM** com base no tráfego à frente:

- Distância **< 10m** → `Vel_Alvo = Vel_Atual ÷ 2` (instrução `SRL`)
- Distância **> 20m** → `Vel_Alvo = Vel_Atual × 2` (instrução `SLLI`), máx. 120 km/h
- Zona intermediária → `Vel_Alvo = Vel_Frente`

> Fórmula PWM: `PWM = (Vel_Alvo × 255) / 120`

---

## 🗺️ Mapa de Memória (I/O Mapeado)

### Entradas — Sensores (`0x10` – `0x15`)
| Endereço | Descrição |
|---|---|
| `0x10` | Distância frontal (m) |
| `0x11` | Velocidade atual (km/h) |
| `0x12` | Limite de segurança (m) |
| `0x13` | Velocidade máxima permitida (km/h) |
| `0x14` | Velocidade do veículo à frente (km/h) |
| `0x15` | Distância do veículo à frente (m) |

### Saídas — Atuadores (`0x20` – `0x23`)
| Endereço | Descrição |
|---|---|
| `0x20` | Comando de freio (`0`: OFF, `1`: Normal, `2`: Emergência) |
| `0x21` | Status do sistema (`0`: Normal, `1`: Alerta, `2`: Emergência) |
| `0x22` | Comando do acelerador (PWM `0–255`) |
| `0x23` | Velocidade-alvo calculada pelo CVA |

---

## 🧪 Casos de Teste Validados

### SFI
| Caso | Cenário | Entrada (dist, vel) | Saída (freio, status) |
|---|---|---|---|
| 1 | Emergência — Pedestre | 2m, 60km/h | 2, 2 |
| 2 | Operação Normal | 50m, 40km/h | 0, 0 |
| 3 | Alerta — Semáforo | 2m, 20km/h | 1, 1 |

### CVA
| Caso | Cenário | Vel. Alvo | PWM | Tolerância |
|---|---|---|---|---|
| 6 | Redução (8m) | 30 km/h | 60 | [50–80] |
| 7 | Aumento (25m) | 100 km/h | 200 | [200–220] |
| 8 | Manutenção (15m) | 55 km/h | 110 | [110–125] |
| 9 | Limite de segurança | 120 km/h | 255 | 255 (Fixo) |

---

## 🛠️ Arquivos do Projeto

| Arquivo | Descrição |
|---|---|
| `falcon8_assembler_v3.html` | Montador interativo Falcon-8 — escreva Assembly e gere a ROM |

---

## 📖 Banco de Registradores

| Registrador | Nome | Uso |
|---|---|---|
| R0 | `zero` | Constante zero (hardwired) |
| R1 | `t0` | Temporários / endereços |
| R2 | `t1` | Valores de sensores |
| R3 | `t2` | Constantes e limites |
| R4 | `t3` | Comparações de decisão |
| R5 | `s0` | Valores preservados |
| R6 | `s1` | Resultados / atuadores |
| R7 | `ra` | Endereço de retorno (JAL) |

---

<div align="center">

**Falcon-8** · Arquitetura RISC 16-bit para Sistemas Automotivos Autônomos

</div>
