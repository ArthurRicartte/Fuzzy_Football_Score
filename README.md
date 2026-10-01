# ⚽🥅 Fuzzy_Soccer_Score - Sistema Baseado em Conhecimento para Avaliação de Jogadores de Futebol

Segundo miniprojeto da disciplina de Sistemas Baseados em Conhecimento (SBC).  
Desenvolvido por: `Arthur Ricartte` e `Felipe Rodrigues`

Sistema de avaliação de desempenho de jogadores de futebol baseado em **lógica fuzzy (Mamdani)**, com penalizações por cartões amarelos e vermelhos, inspirado em métricas de apps de nota (ex.: Sofascore).

---

## 🧩 Elementos do Domínio 

O domínio é composto por quatro dimensões principais de desempenho, além de fatores disciplinares e contextuais:

1. **Dimensão Ofensiva — Participação em Gols**  
   Representa o poder de decisão ofensiva do jogador, medida pela soma de gols + assistências na partida.

2. **Dimensão de Construção — Precisão de Passes**  
   Reflete a qualidade técnica e a tomada de decisão do jogador com a bola. Medida em percentual (0–100%), indica a eficiência na distribuição de jogadas.

3. **Dimensão Defensiva — Desarmes**  
   Quantifica a contribuição defensiva do jogador por meio do número de desarmes / ações defensivas bem-sucedidas. É especialmente relevante para volantes, zagueiros e laterais, mas também valoriza a entrega de jogadores ofensivos que recompõem.

4. **Dimensão Física — Duelos Ganhos**  
   Mede a intensidade e a imposição física do jogador em disputas diretas (aéreas ou terrestres). Um número elevado de duelos ganhos indica dominância física e agressividade positiva no jogo.

5. **Fatores Disciplinares — Cartões**  
   Avaliam o comportamento disciplinar do jogador, penalizando a nota final conforme a gravidade:
   * **1 cartão amarelo** → penalização leve (`-0.3`)
   * **2 cartões amarelos (vermelho indireto)** → penalização severa (`-1.5`)
   * **1 cartão vermelho direto** → penalização severa (`-1.5`)

6. **Fator Contextual — Minutagem**  
   Registra o tempo efetivo jogado, contextualizando as demais estatísticas. Um jogador com poucos minutos pode ter números menores sem necessariamente ter tido mau desempenho.

---

## 📥 Variáveis de Entrada (Antecedentes)

* **participacao_gols**: `0` a `5` (Gols + assistências)
* **passes**: `0` a `100%` (Precisão de passes)
* **desarmes**: `0` a `10` (Ações defensivas)
* **duelos**: `0` a `15` (Duelos ganhos)

---

## 📤 Variável de Saída (Consequente)

* **nota**: `3.0` a `10.0` (Nota base do desempenho)

---

## 🔺 Conjuntos Fuzzy

* **Participação em Gols**
  * `baixa` → triangular `[0, 0, 1]`
  * `moderada` → triangular `[0, 1, 2]`
  * `alta` → trapezoidal `[1, 2, 5, 5]`

* **Passes (%)**
  * `baixa` → trapezoidal `[0, 0, 50, 70]`
  * `media` → triangular `[60, 75, 90]`
  * `excelente` → trapezoidal `[80, 90, 100, 100]`

* **Desarmes**
  * `poucos` → trapezoidal `[0, 0, 1, 3]`
  * `razoaveis` → triangular `[2, 4, 6]`
  * `muitos` → trapezoidal `[5, 8, 10, 10]`

* **Duelos Ganhos**
  * `poucos` → trapezoidal `[0, 0, 2, 5]`
  * `medios` → triangular `[4, 7, 10]`
  * `muitos` → trapezoidal `[8, 12, 15, 15]`

* **Nota**
  * `ruim` → triangular `[3.0, 3.0, 6.6]`
  * `boa` → triangular `[5.5, 6.6, 7.8]`
  * `ótima` → triangular `[6.6, 10.0, 10.0]`

---

## 🧠 Base de Regras

### 🔴 Nota Ruim
* **R1:** `participacao_gols.baixa` **E** `passes.baixa` $\rightarrow$ `nota.ruim`
* **R2:** `desarmes.poucos` **E** `duelos.poucos` **E** `participacao_gols.baixa` $\rightarrow$ `nota.ruim`
* **R3:** `passes.baixa` **E** `desarmes.poucos` **E** `participacao_gols.baixa` $\rightarrow$ `nota.ruim`

### 🟡 Nota Boa
* **R4:** `participacao_gols.baixa` **E** `passes.media` **E** (`desarmes.razoaveis` **OU** `duelos.medios`) $\rightarrow$ `nota.boa`
* **R5:** `participacao_gols.baixa` **E** `passes.excelente` $\rightarrow$ `nota.boa`
* **R6:** `participacao_gols.baixa` **E** (`desarmes.muitos` **OU** `duelos.muitos`) $\rightarrow$ `nota.boa`

### 🟢 Nota Ótima
* **R7:** `participacao_gols.moderada` **OU** `participacao_gols.alta` $\rightarrow$ `nota.ótima`
* **R8:** `passes.excelente` **E** `desarmes.muitos` $\rightarrow$ `nota.ótima`

---

## ⛔ Penalizações por Cartões

* **1 cartão amarelo:** `-0.3`
* **2 cartões amarelos:** `-1.5` *(vermelho indireto)*
* **1 cartão vermelho direto:** `-1.5`

---

🔗 **[Acesse nosso projeto no Google Colab](https://colab.research.google.com/github/ArthurRicartte/Fuzzy_Soccer_Score/blob/main/FuzzySoccerScore.ipynb)**
