# ⚡ Kilua Bot: Automação de Modelagem UML com IA e Low-Code

> Este repositório contém os artefatos de implementação do artigo acadêmico **"Automação da modelagem de software com Inteligência Artificial: Um agente low-code no n8n para geração de diagramas UML"**.

O **Kilua Bot** é um agente orquestrado no **n8n** que utiliza a API do **Google Gemini** para converter descrições em linguagem natural ou trechos de código em diagramas UML renderizados (PlantUML/Mermaid).

---

## 📋 Pré-requisitos

Para reproduzir este experimento localmente, você precisará de:

1.  **Docker** (Recomendado para rodar o n8n).
2.  **Ngrok** (Para expor o webhook localmente para o Telegram).
3.  **Conta no Telegram** (Para criar o bot e interagir).
4.  **Chave de API do Google Gemini** (Google AI Studio).

---

## ⚙️ Guia de Instalação e Execução

Siga os passos abaixo para levantar o ambiente de testes.

### 1. Executando o n8n via Docker
A maneira mais simples de rodar o n8n é utilizando a imagem oficial do Docker. Execute o seguinte comando no seu terminal:

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8n/n8n
```
Após iniciar, acesse http://localhost:5678 no seu navegador.

2. Configurando o Túnel (Ngrok)

Como o n8n está rodando no seu computador (localhost), o Telegram não consegue enviar mensagens para ele. O Ngrok cria um endereço público temporário.

    Instale o Ngrok e autentique-se.

    Rode o comando para expor a porta do n8n:
    ngrok http 5678

    Copie a URL HTTPS gerada (ex: https://a1b2-c3d4.ngrok-free.app). Você precisará dela no passo 4.

Criando o Bot no Telegram

    Abra o Telegram e procure por @BotFather.

    Envie o comando /newbot.

    Dê um nome e um username para seu bot.

    Guarde o Token HTTP API gerado (ex: 123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11).

4. Importando e Configurando o Workflow

    Baixe o arquivo .json localizado na pasta /workflows deste repositório.

    No painel do n8n, clique em Add Workflow > menu ... > Import from File.

    Configuração de Credenciais:

        No n8n, vá em "Credentials" e crie uma nova para Telegram API. Cole o Token obtido no passo 3.

        Crie uma credencial para Google Gemini API. Cole sua chave do AI Studio.

    Ajuste do Webhook:

        Abra o nó inicial (Telegram Trigger).

        Verifique se ele está usando a credencial correta.

        Importante: O n8n deve reconhecer a URL do Ngrok automaticamente se configurado via variáveis de ambiente, mas para testes rápidos, certifique-se de que o n8n está acessível externamente.

🚀 Guia de Uso

Uma vez que o workflow esteja ativo ("Active") no n8n, vá para o seu bot no Telegram e clique em Start.

### 🛠️ Comparativo das Engines

| Motor | Característica | Recomendação |
| :--- | :--- | :--- |
| **🥇 PlantUML** | **O "Tanque de Guerra"**. Ignora pequenos erros de sintaxe da IA. Feio, mas robusto. | **Padrão (Recomendado)** para fluxos complexos. |
| **🎨 Mermaid.js** | **O "Carro Esportivo"**. Bonito e moderno, mas quebra com qualquer erro de sintaxe (Strict Syntax). | Use para diagramas simples ou estética visual. |

### 💻 Comandos Disponíveis

| Comando | Função | Motor Recomendado |
| :--- | :--- | :--- |
| `/genClass` | Diagrama de Classes (Atributos e Métodos) | Mermaid ou PlantUML |
| `/genFlow` | Diagrama de Atividades (Fluxogramas) | PlantUML (Mais seguro) |
| `/genCase` | Casos de Uso (Atores e Ações) | PlantUML (Essencial) |
| `/menu` | Mostra o menu interativo | - |
| `/start` | Executa o primeiro contato | - |
| `/help` | Exibe este guia | - |

Como pedir (Engenharia de Prompt)

1. Modo Descritivo (Linguagem Natural) Você conta o que quer, e o Kilua projeta.

    /genCase Sistema de Delivery onde o Cliente faz pedido, o Restaurante aceita e o Entregador finaliza.

2. Modo "Engenharia Reversa" (Código Puro) Você cola seu código, e o Kilua documenta.

    /genClass class Produto: def __init__(self, preco): self.preco = preco ...

3. Forçando um Motor Específico Se quiser testar o visual do Mermaid explicitamente:

    /genFlow mermaid Usuário faz login -> Sistema valida senha
