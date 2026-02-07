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
- **Ngrok** (para expor o webhook localmente).

### 1. Importação do Workflow
1. Abra o editor do N8N (geralmente em `http://localhost:5678`).
2. Crie um novo workflow.
3. No menu, selecione "Import from File".
4. Escolha o arquivo `workflow-chatbot-telegram.json` deste repositório.

### 2. Configuração de Ambiente Local (Ngrok)
Como o N8N está rodando em ambiente local (Docker), é necessário expor a porta para que o Telegram consiga enviar as mensagens (Webhook).

1. Execute o ngrok na porta do N8N:
   ngrok http 5678

2. Copie o endereço HTTPS gerado (ex: `https://xxxx-xx-xx.ngrok-free.app`).

3. Adicione este endereço na variável de ambiente `WEBHOOK_URL` dentro do arquivo `docker-compose.yml` (no serviço `n8n-editor`):
   
   environment:
     - WEBHOOK_URL=https://seu-endereco-ngrok.app

4. Reinicie o container do N8N para aplicar a configuração:
   docker-compose up -d

### 3. Configuração de Credenciais
Para que o bot funcione, é necessário configurar as credenciais no menu "Credentials" do N8N. Utilize os seguintes nomes para facilitar a identificação:

| Tipo de Credencial | Nome Sugerido | Variável Esperada (Conceito) | Descrição |
|--------------------|---------------|-------------------|-----------|
| **Telegram API** | `Telegram Credential` | `TELEGRAM_BOT_TOKEN` | Token gerado pelo @BotFather. |
| **OpenWeatherMap API** | `OpenWeather Credential` | `OPENWEATHER_API_KEY` | Chave de API da OpenWeather. |
| **Google Gemini API** | `Google Gemini Credential` | N/A | (Opcional) Para respostas criativas. |

> **Nota de Segurança:** As chaves não estão incluídas no arquivo JSON por questões de segurança. Você deve inserir suas próprias chaves ao configurar as credenciais no N8N.

### 4. Executando o Bot
1. Ative o workflow no N8N (chave "Active" no topo da tela).
2. Abra o bot no Telegram.
3. Envie uma mensagem no formato: `Cidade, UF` (Ex: `Curitiba, PR`).
4. O bot responderá com a temperatura e um comentário sobre o clima.

## 🛠️ Detalhes da Implementação

### Estrutura do Fluxo
1. **Trigger:** Recebe a mensagem do Telegram (via Webhook).
2. **Validação (IF):** Regex valida o formato `Texto, Texto` (ex: `São Paulo, SP`).
3. **Tratamento:** Normaliza o texto (remove acentos, converte para minúsculas e remove espaços extras).
4. **API:** Consulta a OpenWeatherMap.
5. **Decisão (IF):** Verifica se o código HTTP retornado é 200 (sucesso).
6. **IA + Fallback:**
   - Tenta gerar frase via **Google Gemini**.
   - Se falhar, o nó **Code** assume e gera a mensagem padrão (Fallback Determinístico).
7. **Resposta:** Envia a mensagem final formatada ao usuário via Telegram.

## 🐳 Docker
O ambiente pode ser reproduzido utilizando o arquivo `docker-compose.yml` incluído neste repositório.

---
Desenvolvido como parte do Desafio Fase 2 da Pós-Graduação.
