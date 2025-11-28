# Datasets de Discurso de Ódio Processados

Este diretório contém versões padronizadas e limpas dos datasets de discurso de ódio em português, geradas a partir dos dados brutos (`01-raw`). O processamento foi realizado pelo notebook `02-clean_files.ipynb`.

## 📂 Arquivos Disponíveis

* **`fortuna.csv`**: Dataset Fortuna (Binário).
* **`hatebrxplain.csv`**: Dataset HateBR.
* **`offcombr3.csv`**: Dataset OffComBR-3.
* **`olidbr.csv`**: Dataset OLID-BR (Treino + Teste concatenados).
* **`toldbr.csv`**: Dataset ToLD-BR.
* **`tupy.csv`**: Dataset TuPy (Versão binária).
* **`hurtlex_PT_conservatives.csv`**: Léxico HurtLex (apenas termos "conservative").

## 📝 Formato dos Dados

Todos os arquivos CSV de datasets (exceto HurtLex) seguem estritamente o seguinte esquema:

| Coluna     | Tipo    | Descrição                                                                 |
| :--------- | :------ | :------------------------------------------------------------------------ |
| `text`     | String  | O texto do comentário/tweet processado. (Sempre entre aspas duplas `"`)   |
| `is_toxic` | Boolean | Rótulo unificado: `True` (1) para tóxico/ódio/ofensivo, `False` (0) caso contrário. |

> **Nota de Formatação:** Os arquivos foram salvos com `QUOTE_NONNUMERIC`. Strings estão entre aspas, valores numéricos/booleanos não.

## ⚙️ Processo de Limpeza e Normalização

As seguintes transformações foram aplicadas a todos os textos:

1.  **Menções de Usuário:**
    * Qualquer menção (`@usuario`) foi substituída pelo token padronizado **`@user`**.
    * A palavra "USER" isolada também foi normalizada para **`@user`**.
    * *Regex:* `(?<!\w)@\w+` e `\bUSER\b`.

2.  **URLs e Links:**
    * Links HTTP/HTTPS e `www` foram substituídos pelo token **`<url>`**.
    * A palavra "link" no final de frases (ou sequências de "link" no final) foi substituída por **`<url>`**.
    * *Regex:* `(?:https?://|www\.)\S+` e `(?i)\blink\b(?=(?:\s*link\b)*\s*$)`.

3.  **Retweets:**
    * Abreviações `RT` ou `rt` isoladas foram normalizadas para **`<RT>`**.

4.  **Formatação Geral:**
    * Quebras de linha (`\n`) foram substituídas por espaços simples.
    * Registros duplicados (baseados no texto processado) foram removidos.
    * Registros com rótulos vazios (`NaN`) foram removidos.

## 🏷️ Lógica de Rótulos (is_toxic)

A coluna `is_toxic` unifica diferentes nomenclaturas dos datasets originais:

* **Fortuna:** `hatespeech_comb` (1 = True).
* **HateBRXplain:** `offensive_label` (1 = True).
* **OffComBR:** `@@class` ('yes' = True).
* **OLID-BR:** `is_offensive` ('OFF' = True).
* **TuPy:** `hate` (1 = True).
* **ToLD-BR:** Considerado tóxico (`True`) se houver marcação positiva em **qualquer** uma das colunas: *homophobia, obscene, insult, racism, misogyny, xenophobia*.