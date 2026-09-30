# Fuzzy_Football_Score ⚽🥅
Programa que aplica regras fuzzy para avaliar o desempenho de um jogador de futebol na partida

## 🧩 Elementos do Domínio 
O domínio é composto por quatro dimensões principais de desempenho, além de fatores disciplinares e contextuais:

1. **Dimensão Ofensiva** — Participação em Gols
Representa o poder de decisão ofensiva do jogador, medida pela soma de gols + assistências na partida.

2. **Dimensão de Construção** — Precisão de Passes
Reflete a qualidade técnica e a tomada de decisão do jogador com a bola. Medida em percentual (0–100%), indica a eficiência na distribuição de jogadas.

3. **Dimensão Defensiva** — Desarmes
Quantifica a contribuição defensiva do jogador por meio do número de desarmes / ações defensivas bem-sucedidas. É especialmente relevante para volantes, zagueiros e laterais, mas também valoriza a entrega de jogadores ofensivos que recompõem.

4. **Dimensão Física** — Duelos Ganhos
Mede a intensidade e a imposição física do jogador em disputas diretas (aéreas ou terrestres). Um número elevado de duelos ganhos indica dominância física e agressividade positiva no jogo.

5. **Fatores Disciplinares** — Cartões
Avaliam o comportamento disciplinar do jogador, penalizando a nota final conforme a gravidade:

  * 1 cartão amarelo → penalização leve (0.3)

  * 2 cartões amarelos (vermelho indireto) → penalização severa (1.5)

  * 1 cartão vermelho direto → penalização severa (1.5)

6. **Fator Contextual** — Minutagem
Registra o tempo efetivo jogado, contextualizando as demais estatísticas. Um jogador com poucos minutos pode ter números menores sem necessariamente ter tido mau desempenho.


## 📥 Variáveis de Entrada (Antecedentes)

participação	0. .5	Gols + assistências
passes	0. .100%	de Precisão
desarmes	0. .10	Ações defensivas
duelos ganhos	0. .15


## 📤 Variável de Saída (Consequente)

nota final	3.0. .10.0


## 🔺 Conjuntos Fuzzy


* **Participação em Gols**

  * baixa → triangular [0, 0, 1]

  * moderada → triangular [0, 1, 2]

  * alta → trapezoidal [1, 3, 5, 5]


* **Passes (%)**

  * baixa → trapezoidal [0, 0, 50, 70]

  * media → triangular [60, 75, 90]

  * excelente → trapezoidal [80, 90, 100, 100]


* **Desarmes**

  * poucos → trapezoidal [0, 0, 1, 3]

  * razoaveis → triangular [2, 4, 6]

  * muitos → trapezoidal [5, 8, 10, 10]


* **Duelos Ganhos**

  * poucos → trapezoidal [0, 0, 2, 5]

  * medios → triangular [4, 7, 10]

  * muitos → trapezoidal [8, 12, 15, 15]


* **Nota**

  * ruim → triangular [3.0, 3.0, 6.6]

  * boa → triangular [5.5, 6.6, 7.8]

  * ótima → triangular [6.6, 10.0, 10.0]


## 🧠 Base de Regras

**Nota ruim**

  * R1: participacao_gols.baixa E passes.baixa → nota.ruim

  * R2: desarmes.poucos E duelos.poucos E participacao_gols.baixa → nota.ruim

  * R3: passes.baixa E desarmes.poucos E participacao_gols.baixa → nota.ruim

**Nota boa**

  * R4: participacao_gols.baixa E passes.media E (desarmes.razoaveis OU duelos.medios) → nota.boa

  * R5: participacao_gols.baixa E passes.excelente → nota.boa

  * R6: participacao_gols.baixa E (desarmes.muitos OU duelos.muitos) → nota.boa

**Nota ótima**

  * R7: participacao_gols.moderada OU participacao_gols.alta → nota.ótima

  * R8: passes.excelente E desarmes.muitos → nota.ótima

## ⛔ Penalizações por Cartões

  *  1 cartão amarelo	-0.3
  *  2 cartões amarelos	-1.5 (vermelho indireto)
  *  1 cartão vermelho direto	-1.5

[[Link para o Colab]](https://colab.research.google.com/drive/15pRBJL9nr6iwzt8Bv6Hjg4pzY175yLJX?usp=sharing)
