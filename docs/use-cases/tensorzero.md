---
outline: [2, 3]
description: Set up TensorZero on Olares to manage AI models, route inferences, evaluate prompts, and connect clients via a unified OpenAI-compatible endpoint.
head:
  - - meta
    - name: keywords
      content: Olares, TensorZero, LLMOps, AI gateway, observability, evaluation, MCP, Ollama, self-hosted
app_version: "1.0.5"
doc_version: "1.0"
doc_updated: "2026-05-06"
---

# Manage AI connections with TensorZero

TensorZero is a comprehensive LLMOps platform that operates as an AI gateway, an observability dashboard, and an evaluation tool. It unifies APIs from multiple model providers and logs every inference to a built-in database, allowing you to monitor latency, evaluate prompt variants, and optimize your workflows.

Unlike basic proxies, TensorZero operates on a strict whitelist model. It requires you to explicitly declare every model and function in a configuration file, and mandates specific prefixes for client routing.

## Learning objectives

In this guide, you will learn how to:
- Understand TensorZero's configuration.
- Add an Ollama model and function.
- Add an embedding model.
- Test connections.
- Connect client applications.
- Access TensorZero's built-in MCP server.

## Prerequisites

- Ensure Ollama is installed with at least one chat model downloaded (e.g., qwen3.5:9b) and one embedding model downloaded (e.g., nomic-embed-text).

## Install TensorZero

1. Open Market, and search for "TensorZero".

    ![Search for TensorZero from Market](/images/manual/use-cases/tensorzero.png#bordered)

2. Click **Get**, and then click **Install**. Wait for the installation to finish.

## Understand the configuration requirements

TensorZero does not provide a graphical interface for configuring models. You manage all settings by editing the configuration file in Control Hub.

Before proceeding, review the following rules to avoid syntax errors and connection failures:
- Strict whitelist: TensorZero rejects direct requests to upstream provider model names like `gpt-4o` and `qwen3.5`. You must explicitly define an alias for every model in the configuration file.
- Strict prefixes: When connecting third-party clients, you must prepend your model aliases with specific prefixes, such as `tensorzero::model_name::alias`.
- TOML syntax: The configuration file uses TOML format. You must maintain at least one empty line between different sections. Removing empty lines causes syntax errors and crashes the application.

## Configure a chat model and function

In TensorZero, a model configuration defines the backend AI engine and endpoint, while a function defines the client-facing use case and routing logic. You must define a model and then link it to a function variant to process client requests. This example connects a local Ollama instance.

1. Open Settings, go to **Applications** > **Ollama** > **Shared entrances** > **Ollama API**, and then copy the endpoint URL. For example, `http://d54536a50.shared.olares.com`.
2. Open Control Hub, go to **tensorzero-{username}** > **Configmaps** > **gateway-startups**, and then click <i class="material-symbols-outlined">edit_square</i> to edit the `tensorzero.toml` file.

    ![Edit config file in Control Hub](/images/manual/use-cases/tensorzero-ctrl-hub.png#bordered)

3. In the YAML editor, scroll down to the end, and then add the following snippet. Replace the `api_base` with your copied Ollama URL and append /v1. Replace the `model_name` with the exact downloaded model name in your Ollama instance.

    This configuration example registers your Ollama model under an internal alias (`qwen3_5_9b`) and creates a client‑facing function (`my_function_name`) that routes requests to that model.

    ```python
    # models

    [models.qwen3_5_9b]

    routing = ["ollama"]

    [models.qwen3_5_9b.providers.ollama]

    type = "openai"

    api_base = "<ollama-shared-entrance>/v1"

    model_name = "qwen3.5:9b"

    api_key_location = "none"

    # functions

    [functions.my_function_name]

    type = "chat"

    [functions.my_function_name.variants.my_variant_name]

    type = "chat_completion"

    model = "qwen3_5_9b"
    ```

    :::warning
    Ensure you leave an empty line between `[models.qwen_local.providers.ollama]` and `[functions.my_chat_function]`. Do not use dots or colons in your alias names (e.g., use qwen_local, not qwen3.5:9b).
    :::

    ![Connect to Ollama](/images/manual/use-cases/tensorzero-config-ollama.png#bordered)

4. Click **Confirm**.
5. Go to **Deployments** > **tensorzero**, and then click **Restart**. Wait about 30 seconds for the gateway to apply the new configuration.

    ![TensorZero pod restart](/images/manual/use-cases/tensorzero-pod-restart.png#bordered)

## Configure an embedding model

Many client applications require embedding models for Retrieval-Augmented Generation (RAG) or memory features. TensorZero treats embedding models separately from chat models. You must define a dedicated embedding model. Do not use a chat function for embeddings.

1. Open the **gateway-startups** ConfigMap again and edit `tensorzero.toml`.
2. Append the following snippet to define an embedding model. This configuration registers your Ollama embedding model under the alias `nomic_embed`.

    ```python
    # embedding_models

    [embedding_models.nomic_embed]

    routing = ["ollama"]

    [embedding_models.nomic_embed.providers.ollama]

    type = "openai"

    api_base = "<ollama-shared-entrance>/v1"

    model_name = "nomic-embed-text"

    api_key_location = "none"
    ```

    ![Connect to embedding model](/images/manual/use-cases/tensorzero-config-embedding.png#bordered)    

    :::tip
    Make sure `model_name` matches the embedding model you have already pulled in Ollama (e.g., `nomic-embed-text`).
    :::

3. Click **Confirm**, and then restart the TensorZero container.

## Verify the connection

Use the built-in Playground to ensure your configured function routes correctly to the Ollama backend.

If you have never used TensorZero before, you must manually create at least one test case (a "Datapoint") before the Playground chat interface can load.

1. Open TensorZero from the Launchpad.
2. Select **Datasets** from the left sidebar.
3. Click **New Datapoint**, and configure the test case details. For example, to create a basic geography test:

    - **Dataset**: Specify the dataset name. For example, `Baseline tests`.
    - **Function**: Select the function you configured earlier. For example, `my_function_name`.
    - **Input**: Select **+ User Message**, click **+ Text**, and then enter a test prompt. For example, `What is the capital of Spain?`.
    - **Output**: Select **+ Text**, and then enter the exact response you expect the model to generate. For example, `Madrid`.
    - **Tags** and **Metadata**: (Optional) Enter any labels to help filter this test later. For example, add a tag with **Key** set to `type` and **Value** set to `QA`.

    ![Create a new datapoint](/images/manual/use-cases/tensorzero-new-datapoint.png#bordered)      

4. Click **Create Datapoint**.
5. Select **Playground** from the left sidebar.
6. Select your function, the dataset you just created, and the variant. The chat interface appears. If you receive a normal reply, the configuration is successful.

    ![Verify connection](/images/manual/use-cases/tensorzero-playground.png#bordered)   

## Obtain the TensorZero endpoint

Client applications like OpenCode require the TensorZero gateway URL to send API requests to your routed models.

1. Open Settings, go to **Applications** > **TensorZero** > **Entrances** > **TensorZero**.

    ![TensorZero endpoint addres](/images/manual/use-cases/tensorzero-endpoint.png#bordered){width=70%} 

2. Copy the endpoint URL. For example, `https://ea581361.laresprime.olares.com`. For OpenAI‑compatible clients, you must append `/openai/v1` to this URL.

## Route models to client applications

Configure your third-party applications to use TensorZero as a custom OpenAI-compatible provider.

TensorZero exposes an OpenAI-compatible endpoint. To connect third-party applications, you must construct the correct model name using specific prefixes.

### Determine your model name string

Clients require a strict string format depending on the resource you want to call:

| Resource type | TOML definition | Required string format | Example |
| :--- | :--- | :--- | :--- |
| **Function** | `[functions.xxx]` | `tensorzero::function_name::<alias>` | `tensorzero::function_name::my_chat_function` |
| **Model** | `[models.xxx]` | `tensorzero::model_name::<alias>` | `tensorzero::model_name::qwen_local` |
| **Embedding** | `[embedding_models.xxx]` | `tensorzero::embedding_model_name::<alias>` | `tensorzero::embedding_model_name::nomic_embed` |

### Connect your clients

<Tabs>
<template #OpenCode>

1. In OpenCode, click <i class="material-symbols-outlined">settings</i> in the bottom-left corner.
   ![Open OpenCode settings](/images/manual/use-cases/opencode-settings.png#bordered)

2. Select **Providers**, then scroll down and select **Connect** next to **Custom provider**.
   ![Select custom provider](/images/manual/use-cases/opencode-custom-provider.png#bordered)

3. Enter the following details, and then click **Submit**.
   - **Provider ID**: A unique identifier for the model provider. For example, `olares-ollama-tensorzero`.
   - **Display name**: The name shown in the provider list. For example, `Olares TensorZero`.
    - **Base URL**: Paste your TensorZero URL ending with `/openai/v1`. For example, `https://ea581361.laresprime.olares.com/openai/v1`.
    - **API key**: Enter any text. TensorZero ignores this by default, but OpenCode requires a value.
    - **Models**:
        - **Model ID**: Enter the exact function string, `tensorzero::function_name::my_function_name`.
        - **Display Name**: Enter a friendly name, such as `TensorZero Qwen`.

    ![TensorZero config in OpenCode](/images/manual/use-cases/tensorzero-opencode.png#bordered){width=70%}     

4. Refresh OpenCode, and then go to **Settings** > **Models** > **Olares TensorZero**.
5. Verify the model you added is enabled.

    ![TensorZero enabled in OpenCode](/images/manual/use-cases/tensorzero-opencode-enable.png#bordered)   

6. Start a new session, and select the TensorZero-managed model to begin a chat.

    ![TensorZero chat in OpenCode](/images/manual/use-cases/tensorzero-opencode-chat.png#bordered)

7. Open TensorZero, and check the observability data. For example, on the **Inferences** page, each request you send appears as a log entry, which confirms that TensorZero routes the traffic successfully.

    ![TensorZero Inferences page](/images/manual/use-cases/tensorzero-inferences.png#bordered)

8. Select an entry to view the details.

    ![TensorZero Inferences entry details](/images/manual/use-cases/tensorzero-inferences-details.png#bordered)
</template>
<template #AgentZero>

1. Open Agent Zero, and then go to **Settings** > **Agent Settings** > **Chat Model**.
2. Configure the following details:

    - **Chat model provider**: Select **Other OpenAI compatible**.
    - **Chat model name**: Enter `tensorzero::function_name::my_function_name`.
    - **Chat model API base URL**: Enter your TensorZero URL ending with /openai/v1. For example, `https://ea581361.laresprime.olares.com/openai/v1`.
    - **API key**: Enter any text. 

    ![TensorZero config in AgentZero](/images/manual/use-cases/tensorzero-agentzero.png#bordered)

3. Click **Save**.
4. To configure memory embeddings, go to the **Embedding Model** section, and configure:

    - **Embedding model provider**: Select **Other OpenAI compatible**.
    - **Embedding model name**: Enter `tensorzero::embedding_model_name::nomic_embed`.
    - **API key**: Enter any text.
    - **Embedding model API base URL**: Enter your TensorZero URL ending with /openai/v1. For example, `https://ea581361.laresprime.olares.com/openai/v1`

    ![TensorZero config in AgentZero, embedding model config](/images/manual/use-cases/tensorzero-agentzero-embed.png#bordered)    

5. Click **Save**.
5. Start a new chat. 

    ![TensorZero chat in AgentZero](/images/manual/use-cases/tensorzero-agentzero-chat.png#bordered)

6. Open TensorZero, and check the observability data. For example, on the **Inferences** page, each request you send appears as a log entry, which confirms that TensorZero routes the traffic successfully. 

    ![TensorZero inferences page](/images/manual/use-cases/tensorzero-agentzero-inferences.png#bordered)

6. Select an entry to view the details.

    ![TensorZero chat in AgentZero inference details](/images/manual/use-cases/tensorzero-agentzero-inference-details.png#bordered)

</template>
<template #OpenNotebook>

pending environment
</template>
</Tabs>

## Access the built-in MCP server

TensorZero includes a built-in Model Context Protocol (MCP) server located at the `/mcp` endpoint. This allows MCP-compatible clients like OpenCode to query your observability data directly.

For example, an agent can look up the average latency of `my_chat_function` or retrieve the raw input and output of previous inferences. 

The following exmple demonstrates how to configure an MCP client for OpenCode.

1. Open Files, and then go to **Data** > **opencode** > **.config** > **opencode**.
2. Double-click `opencode.json`, and then click <i class="material-symbols-outlined">edit_square</i>.
3. Add the following MCP configuration block:

    ```json
    {
    "mcp": {
        "tensorzero": {
        "type": "remote",
        "url": "<tensorzero-endpoint>/mcp",
        "enabled": true
        }
    }
    }
    ```
4. Click <i class="material-symbols-outlined">save</i>.
5. Restart OpenCode to apply the changes. in the upper right corner, on the **MCP** tab, you can see **tensorzero** is enabled.

    ![TensorZero MCP enabled in OpenCode](/images/manual/use-cases/tensorzero-mcp.png#bordered){width=50%}

6. Instruct your AI agent directly in the chat to explicitly use the TensorZero MCP tool. For example, `Use the TensorZero MCP tool to analyze the latest inference logs`.

    ![OpenCode MCP use](/images/manual/use-cases/tensorzero-mcp-opencode.png#bordered)






