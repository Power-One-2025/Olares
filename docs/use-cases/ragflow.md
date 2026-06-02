---
outline: [2, 3]
description: Learn how to install and configure RAGFlow on Olares to build a powerful local Retrieval-Augmented Generation (RAG) engine.
head:
  - - meta
    - name: keywords
      content: Olares, RAGFlow, RAG, AI server, knowledge base, LLM
app_version: "1.0.23"
doc_version: "1.0"
doc_updated: "2026-06-02"
---

# Build your private knowledge base with RAGFlow

RAGFlow connects large language models (LLMs) with your personal or business documents. Establish a local knowledge base that intelligently interprets and answers questions based exactly on the files you provide, keeping your data entirely private on your Olares device.

## Learning objectives

In this guide, you will learn how to:

- Install RAGFlow from the Olares Market.
- Connect chat and embedding language models.
- Keep the models loaded for always-on responses.
- Create a dedicated knowledge base and upload documents.
- Configure an AI chat assistant to query your personal data.

## Install RAGFlow and required dependencies

Before you can install RAGFlow, you must first install Ollama and MySQL (V8.0.0 or later).

:::info
RAGFlow relies on multiple middleware components to run smoothly. While xxx and xxx are already pre-installed on your Olares system, Ollama and MySQL require manual installation.
:::

1. Open Market, and search for "Ollama".
2. Click **Get**, and then click **Install**. Wait for the installation to finish.

   ![Install Ollama](/images/manual/use-cases/ollama.png#bordered)

3. Search for "MySQL" and install it.

   ![Install MySQL](/images/manual/use-cases/mysql.png#bordered)

4. Search for "RAGFlow" and install it.

   ![Install RAGFlow](/images/manual/use-cases/ragflow.png#bordered)

## Install model apps and download models

RAGFlow requires a chat model for generating responses, and an embedding model for processing documents.

This guide uses "Qwen3.5 9B" as the chat model and "Nomic Embed v1.5" as the embedding model, and installs the corresponding model apps.

1. Search for "Qwen3.5 9B" and install it.

   ![Install Qwen3.5 9B](/images/manual/use-cases/qwen35-9b1.png#bordered)

2. Search for "Nomic Embed v1.5" and install it.

   ![Install Nomic Embed v1.5](/images/manual/use-cases/nomic-embed1.png#bordered)

3. Wait for all the installations to finish.
4. Open the Qwen3.5 9B app from the Launchpad and wait for the model download to complete. Note down the model name exactly as shown. For example, `qwen3.5:9b`.
5. Open the Nomic Embed v1.5 app from the Launchpad and wait for the model download to complete. Note down the model name exactly as shown. For example, `nomic-embed-text:v1.5`.

## Keep models loaded

By default, the local LLM unloads from memory after 5 minutes of inactivity, and the next reply has to wait for the model to reload. For an always-on agent, enable the keep-alive setting on the model app to keep it resident in memory.

1. Open Settings, and then go to **Applications** > **Qwen3.5 9B Q4_K_M (Ollama)** > **Manage environment variables**.
2. Find **KEEP_ALIVE**, and then click <i class="material-symbols-outlined">edit_square</i>.

   ![Enable KEEP_ALIVE for the model app](/images/manual/use-cases/keep-alive-enable.png#bordered){width=80%}

3. Set the value to **true**, and then click **Confirm**.
4. Click **Apply**.
5. Go to **Applications** > **Nomic Embed v1.5 (Ollama/CPU)** > **Manage environment variables**, and then use the same procedure to set its **KEEP_ALIVE** to **true**.

:::tip When to leave KEEP_ALIVE unset
Keeping the model loaded consumes VRAM continuously. If you only use the agent occasionally and don't mind the cold-start delay, leave **KEEP_ALIVE** unset.
:::

## Obtain model endpoints

1. In Settings, go to **Applications** > **Qwen3.5 9B Q4_K_M (Ollama)**.
2. In **Shared entrances**, select **Qwen3.5 9B Q4_K_M** to view the endpoint URL.

   ![Qwen3.5 9B shared entrance](/images/manual/use-cases/anythingllm-qwen359b-shared-entrance.png#bordered){width=80%}

3. Copy the shared endpoint URL. For example:

   ```plain
   http://bd5355000.shared.olares.com
   ```

4. Go to **Applications** > **Nomic Embed v1.5**.
5. In **Shared entrances**, select **Nomic Embed v1.5** to view the endpoint URL.

   ![Nomic Embed v1.5 shared entrance](/images/manual/use-cases/anythingllm-nomic-shared-entrance.png#bordered){width=80%}

6. Copy the shared endpoint URL. For example:

   ```plain
   http://8298761c0.shared.olares.com
   ```

## Obtain model context size

1. Open Control Hub from the Launchpad.
2. Go to **Browse**, expand the **System** namespace, and then select **ollamaqwen359bv2server-shared**.
3. Under **Deployments**, expand **ollama**, and then select the listed pod. For example, **ollama-6db9bdfb68-8psxs**.
4. In the **Containers** section on the right, click the terminal icon next to **ollama**.

   ![Open model app container terminal](/images/manual/use-cases/model-app-terminal.png#bordered){width=80%}

5. Enter the following command in the terminal:

   ```bash
   ollama ps
   ```

6. Note down the value in the **CONTEXT** column. For example, `131072`.

   ![Check model app context size](/images/manual/use-cases/qwen35-9b-context.png#bordered)

7. Go back to the **System** namespace, find **ollamanomicembedtextv2server-shared**, and the use the same procedure to obtain and note down its context value. For example, `2048`.

   ![Check model app context size](/images/manual/use-cases/nomic-embed-text-context.png#bordered)

## Create your workspace

1. Open RAGFlow from the Launchpad, and then click **Get started** on the welcome page to access the sign-in page.

   ![Sign in RAGFlow](/images/manual/use-cases/ragflow-signin.png#bordered)

2. Select **Sign up** to create your initial account.
3. Enter email, nickname, and password, and then click **Continue**. You got the message notification "Registered". 
4. Enter your new email and password, and then click **Sign in** to access the dashboard.

   ![RAGFlow dashboard](/images/manual/use-cases/ragflow-dashboard.png#bordered)

## Configure RAGFlow 

To process documents and generate conversational answers, RAGFlow requires an embedding model to map your documents into searchable data, and a chat model to generate the actual responses.

### Add a chat model

Connect the LLM that acts as the brain of your chat assistant.

1. Click the user profile menu in the upper-right corner, and then select **Model providers** from the left sidebar.
2. In the **Available models** panel, select **Ollama**.
3. In the popup window, configure the settings as follows:

   - **Model type**: Select **Chat**.
   - **Model name**: Enter the exact model name you noted down. In this example, it is `qwen3.5:9b`.
   - **Base Url**: Enter the shared endpoint URL you noted down. In this example, it is `http://bd5355000.shared.olares.com`.
   - **API-Key**: Enter any text. Do not leave it blank.
   - **Max tokens**: Enter the context size of the model you noted down. In this example, it is `131072`.

4. Click **Verify**.
5. Click **OK** to save the model settings.

### Add an embedding model

Connect the model that processes and chunks your uploaded documents.

1. Select **Add** again under your chosen provider.
2. In the popup window, configure the settings as follows:

   - **Model type**: Select **Embedding**.
   - **Model name**: Enter the exact model name you noted down. In this example, it is `nomic-embed-text:v1.5`.
   - **Base Url**: Enter the shared endpoint URL you noted down. In this example, it is `http://bd5355000.shared.olares.com`.
   - **API-Key**: Enter any text. Do not leave it blank.
   - **Max tokens**: Enter the context size of the model you noted down. In this example, it is `131072`.

3. Click **Verify**.
4. Click **OK** to save the model settings.

## Create a knowledge base

Establish the data source for your RAG engine by uploading files. RAGFlow uses specific parsing templates to optimize how it reads different file types.

1. Go to the **Knowledge Base** tab.
2. Select the option to create a knowledge base.
3. Enter a descriptive name for your knowledge base.
4. Select the embedding model you configured in the previous section.
5. Choose the parsing template that best fits your document type (for example, **Paper**, **Manual**, or **General**).
6. Select **Save**.
7. Open your new knowledge base from the list.
8. Select the upload option, and choose the target documents from your system.
9. Wait for the document parsing process to finish. The status turns green to indicate the chunks are ready.

## Create an AI chat assistant

Link your knowledge base to a chat interface to start asking questions about your documents.

1. Go to the **Chat** tab.
2. Select the option to create a new chat.
3. Enter a name for the chat, and select **Save**.
4. Open the chat you just created, and navigate to the chat settings.
5. In the **Knowledge Base** section, select the knowledge base you created in the previous step.
6. Scroll down to the **Cross-language search** section, and select the languages relevant to your documents.
7. In the **Model** section, select your configured chat model.
8. Adjust optional settings like temperature or system prompts as needed, and select **Save**.

## Query your data

Test your new assistant to ensure the model accurately interprets your uploaded content.

1. Go to your configured chat interface.
2. Enter a question related to the uploaded documents in the message box.
3. Send the message. The assistant retrieves relevant information from your knowledge base and generates an accurate response based on your files.

## Learn more

- [Build a local knowledge base with AnythingLLM](anythingllm.md)
- [Build a research notebook with Open Notebook](open-notebook.md)
