# 🎯 Mega-Sena Analyzer

**Analisador estatístico e gerador auditável de jogos da Mega-Sena (offline)**

> ⚠️ **Aviso importante**
> Este projeto **não prevê resultados**, **não aumenta a probabilidade matemática** de ganhar na Mega-Sena e **não descobre padrões ocultos**.
> Ele aplica **estatística descritiva, detecção de outliers e boas práticas de engenharia** — nada além disso.

---

## 📌 Objetivo do projeto

Este repositório contém um **motor estatístico explicável** para:

* Analisar jogos da Mega-Sena
* Classificar jogos como **típicos, limítrofes ou extremos** com base no histórico real
* Gerar jogos **estruturalmente razoáveis**, evitando vieses humanos óbvios
* Servir como **estudo de caso** de:

  * scoring estatístico
  * detecção de outliers
  * pipelines auditáveis
  * separação entre dados, regras e interpretação

A Mega-Sena aqui é apenas o **dataset didático**.

---

## 🧠 Filosofia

* **Dados reais > heurística**
* **Score contínuo > booleano**
* **Explicabilidade > “acerto”**
* **Auditabilidade > ilusão de controle**

Nada de:

* “número atrasado”
* regressão para prever sorteio
* redes neurais
* superstição estatística

---

## 🏗️ Arquitetura geral

```
dados históricos (JSON)
        ↓
extração de features
        ↓
estatística descritiva
        ↓
score de raridade (z-score médio)
        ↓
classificação por percentil (P80 / P95)
        ↓
interpretação humana
```

### Separação clara de responsabilidades

| Camada      | Função                                                |
| ----------- | ----------------------------------------------------- |
| Dados       | Histórico real dos sorteios                           |
| Features    | Soma, pares, amplitude, dezenas, unidades, sequências |
| Estatística | Média, desvio padrão, percentis                       |
| Score       | Raridade contínua                                     |
| Semântica   | Típico / Limítrofe / Extremo                          |
| UI          | HTML + JS offline                                     |

---

## 📂 Estrutura do repositório

```
.
├── README.md
├── mega_sena_analyzer_versao_final_html_js_offline.html # analisador HTML + JS (offline)
```

---

## 📊 Formato do histórico

O histórico é armazenado em JSON no formato:

```json
{
  "megasena": [
    {
      "concurso": "2954",
      "data": "20/12/2025",
      "numeros": ["01", "09", "37", "39", "42", "44"]
    }
  ]
}
```

## ⚙️ Funcionamento do score

### 1️⃣ Features extraídas por jogo

* soma total
* pares × ímpares
* amplitude (`max - min`)
* maior concentração por dezena
* maior concentração por unidade
* maior sequência consecutiva

### 2️⃣ Score de raridade

```text
score = média(|z-score(feature)|)
```

Interpretação:

* `~0.5` → muito típico
* `~1.0` → limítrofe
* `>1.3` → extremo

---

## 🏷️ Classificação semântica

A classificação **não usa thresholds arbitrários**.

Ela é baseada em **percentis do histórico real**:

* **Típico** → até P80
* **Limítrofe** → P80–P95
* **Extremo** → acima de P95

Isso garante:

* coerência estatística
* estabilidade
* auditabilidade

---

## 🧪 Sanity check (validação estrutural)

Ao carregar o histórico, o sistema verifica automaticamente:

```
~80%  → típico
~15%  → limítrofe
~5%   → extremo
```

Se isso não ocorrer:

* score está mal calibrado
* features estão enviesadas
* thresholds incorretos

---

## 🌐 Interface Web (offline)

O projeto inclui um **HTML + JavaScript 100% offline**, que permite:

* Analisar jogos informados pelo usuário
* Gerar jogos aleatórios
* Ver score e classe imediatamente
* Funcionar sem servidor, sem build, sem dependências

Basta abrir o html no navegador.

---

## 🚫 O que este projeto NÃO faz

* ❌ Não prevê sorteios
* ❌ Não aprende com novos jogos (evita feedback loop)
* ❌ Não aumenta probabilidade de acerto
* ❌ Não usa machine learning

Se alguém promete isso, está mentindo — este projeto não.

---

## 🧩 Casos de uso reais (fora loteria)

O mesmo desenho é usado em:

* credit scoring
* detecção de fraude
* anomaly detection
* validação de LLMs
* sistemas de risco e compliance

A Mega-Sena é apenas o exemplo mais neutro possível.

---

## 🛠️ Tecnologias

* HTML estático
* JavaScript (runtime estatístico)
* Zero dependências externas

---

## 📜 Licença

Uso educacional e experimental.
Sem garantias, explícitas ou implícitas.

---

## 🧠 Nota final

> **Loteria é aleatoriedade.**
> Estatística aqui serve para **entender o espaço**, não para dominá-lo.

Se você está procurando previsões, este projeto não é para você.
Se você está interessado em **engenharia estatística limpa e explicável**, seja bem-vindo.
