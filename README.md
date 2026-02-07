# Desafio Fase 2 - Bot de Clima no Telegram com N8N

Este projeto consiste em um chatbot inteligente para Telegram, desenvolvido em N8N, capaz de informar a temperatura atual de qualquer cidade brasileira utilizando a API OpenWeatherMap.

O projeto inclui funcionalidades avançadas como validação de entrada, tratamento de erros e enriquecimento da resposta utilizando Inteligência Artificial (Google Gemini) com fallback determinístico.

## 📋 Funcionalidades

- **Consulta de Clima:** Integração via API OpenWeatherMap.
- **Validação de Entrada:** Verifica se o usuário enviou o formato correto (Cidade, UF).
- **Tratamento de Erros:** Respostas amigáveis caso a cidade não seja encontrada.
- **Inteligência Artificial (Opcional):** Uso do Google Gemini para gerar mensagens criativas sobre o clima.
- **Fallback de Segurança:** Caso a IA falhe, o sistema garante a entrega da temperatura via código determinístico.

## 🚀 Como Executar

### Pré-requisitos
- Docker e Docker Compose instalados.
- N8N (versão recomendada 1.122.4 ou superior).
- Token de Bot do Telegram (@BotFather).
- Chave de API da OpenWeatherMap.
- (Opcional) Chave de API do Google Gemini.

### 1. Importação do Workflow
1. Abra o editor do N8N.
2. Crie um novo workflow.
3. No menu, selecione "Import from File".
4. Escolha o arquivo `workflow-chatbot-telegram.json` deste repositório.

### 2. Configuração de Credenciais
Para que o bot funcione, é necessário configurar as credenciais no menu "Credentials" do N8N:

| Tipo de Credencial | Nome Sugerido | Variável Esperada (Conceito) | Descrição |
|--------------------|---------------|-------------------|-----------|
| **Telegram API** | `Telegram Credential` | `TELEGRAM_BOT_TOKEN` | Token gerado pelo @BotFather. |
| **OpenWeatherMap API** | `OpenWeather Credential` | `OPENWEATHER_API_KEY` | Chave de API da OpenWeather. |
| **Google Gemini API** | `Google Gemini Credential` | N/A | (Opcional) Para respostas criativas. |

> **Nota de Segurança:** As chaves não estão incluídas no arquivo JSON por questões de segurança.

### 3. Executando o Bot
1. Ative o workflow no N8N (switch "Active").
2. Abra o bot no Telegram.
3. Envie uma mensagem no formato: `Cidade, UF` (Ex: `Curitiba, PR`).
4. O bot responderá com a temperatura e um comentário sobre o clima.

## 🛠️ Detalhes da Implementação

### Estrutura do Fluxo
1. **Trigger:** Recebe a mensagem do Telegram.
2. **Validação (IF):** Regex valida o formato `Texto, Texto`.
3. **Tratamento:** Normaliza o texto (remove acentos, minúsculas).
4. **API:** Consulta a OpenWeatherMap.
5. **Decisão (IF):** Verifica se o código HTTP é 200.
6. **IA + Fallback:**
   - Tenta gerar frase via **Google Gemini**.
   - Se falhar, o nó **Code** assume e gera a mensagem padrão.
7. **Resposta:** Envia a mensagem final ao usuário.

## 🐳 Docker
O ambiente pode ser reproduzido utilizando o arquivo `docker-compose.yml` incluído neste repositório.

---
Desenvolvido como parte do Desafio Fase 2 da Pós-Graduação.