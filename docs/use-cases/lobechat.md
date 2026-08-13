---
outline: [2, 4] 
title: Build a local AI agent with LobeHub
description: Install LobeHub on Olares and connect it to local models to build self-hosted AI assistants, generate images with ComfyUI, and create specialized agents with custom skills.
head:
  - - meta
    - name: keywords
      content: Olares, LobeHub, LobeChat, self-hosted lobechat, AI agent, image generation, ComfyUI, lobechat on olares
app_version: "1.0.18"
doc_version: "2.2"
doc_updated: "2026-08-13"      
---

# Build your local AI agent with LobeHub

LobeHub (previously known as LobeChat) is an open-source platform for building secure, self-hosted AI agents and chat experiences. It connects to your local models, supports file handling and knowledge bases, and allows you to create specialized agents with custom skills.

This guide covers the installation, configuration, and practical usage of LobeHub to create your personalized AI agents and generate images with ComfyUI.

:::tip About the product name
LobeHub is the official platform name, but the application is currently listed as "LobeChat" in the Olares Market. We use both names in this guide to match exactly what you will see on your screen. The Market will be updated to reflect the new LobeHub branding in the future release.
:::

## Learning objectives

- Install LobeHub on Olares and connect it to your local model.
- Chat with Lobe AI for everyday tasks.
- Create specialized agents using the Agent Builder or custom settings.
- Generate images with ComfyUI.
<!--- Create an agent group to enable multiple agents to collaborate on complex workflows.-->

## Prerequisites

Before you begin, you need the following model:
| Model type | Model | How to get it |
| :--- | :--- | :--- |
| Chat | Qwen3.6-27B (llama.cpp) | Install from Market |
| Image generation | ComfyUI Share | Install from Market |

- LobeChat upgraded to the latest version (chart version xxx)
- ComfyUI upgrade to the latest version (chart version xxx). For more informaiton, see [How to migrate to the new ComfyUI after upgrading to Olares 1.12.6](../use-cases/comfyui-common-issues.md#how-to-migrate-to-the-new-comfyui-after-upgrading-to-olares-1-12-6).

<!--@include: ../reusables/ai-service-connections.md#use-different-model-->

## Install LobeHub

1. From the Olares Market, search for "LobeChat".

   ![Search for LobeChat from Market](/images/manual/use-cases/find-lobechat2.png#bordered)

2. Click **Get**, and then click **Install**. Wait for the installation to finish.

## Sign in to LobeHub

1. Open **LobeChat** from the Launchpad.
2. Enter your email address, and then follow the prompts on the page to create a LobeHub account and sign in.

   ![LobeHub home page](/images/manual/use-cases/lobehub-start1.png#bordered)

## Configure the connection

Connect LobeHub to your local model to make the chat interface work.

### Get model connection details

<!--@include: ../reusables/ai-service-connections.md#get-model-connection-details-->

### Configure the connection in LobeHub

1. From the left sidebar, go to **Settings** > **AI Service Provider** > **OpenAI**.

      ![Configure model connection in LobeHub](/images/manual/use-cases/lobehub-config-model.png#bordered)

2. Configure the following settings:

   - **API Key**: Enter any placeholder text such as `local`.
   - **API Proxy URL**: Enter the **Base URL** you copied from the Model Console. For example, `https://e46e044d.laresprime.olares.com/v1`.
   - **Use Responses API Specification**: Ensure this option is disabled.
   - **Use Client Request Mode**: Ensure this option is disabled.

      :::tip
      Do not enable the **Use Client Request Mode** option when running local models. This mode is designed for remote API calls and might cause connection errors.
      :::

3. In the **Model List** section, click **Fetch models** to pull the list of supported models. The model name `unsloth/Qwen3.6-27B-GGUF:Q4_K_M` appears in the list.

   ![Fetch model list and enable models](/images/manual/use-cases/lobehub-fetch-enable-model1.png#bordered)

4. Click <i class="material-symbols-outlined">toggle_off</i> to enable it.
5. In the **Connectivity Check** section, select the model you just enabled from the list, and then click **Check** to verify the connection. If the model is large, it might take a little longer to load.

   The button changes to **Check Passed**, indicating that the connection is established. 

   ![Connectivity check success](/images/manual/use-cases/lobehub-checkpass2.png#bordered)  

6. Click the home icon at the upper-left corner to return to the LobeHub home page.

   ![Return to home page](/images/manual/use-cases/lobehub-return-home.png#bordered){width=50%} 

## Use Lobe AI

Lobe AI is the official default agent from LobeHub. It is designed to help you accomplish a wide range of tasks without the need for complex setup, such as software development, learning support, creative writing, data analysis, and daily personal tasks.

If Lobe AI does not meet your specific workflow needs, you can build your own specialized agents. For more information, see [Create an agent](#create-an-agent).

1. From the left sidebar, click **Lobe AI**.
   
   ![Click Lobe AI](/images/manual/use-cases/lobe-ai.png#bordered) 

2. Under the chat window, click the model selector and select the local model.
3. Chat as you would with any standard conversational AI.

## Create an agent

Create your own specialized agents by using the conversational Agent Builder or by manually configuring the settings from scratch.

LobeHub allows you to create specialized assistants to handle specific tasks by leveraging various language models and combining them with skills.
- **Flexible model switching**: You can switch language models instantly within the same chat to achieve the best results. For example, if you are not satisfied with a response, you can select a different model from the list to leverage their unique strengths.
- **Skill extensions**: You can also install additional skills to extend and enhance the capabilities of your agent.
   To install skills, ensure that you select a model compatible with Function Calling. Look for <i class="material-symbols-outlined">brick</i> next to the model name, which indicates the model supports function calls.

### Create using Agent Builder

Agent Builder is LobeHub's built-in assistant that helps you create specialized agents through conversations. Describe your needs, and it will automatically generate a complete agent configuration, including role settings, system prompts, and skills.

1. On the home page, click **Create Agent** under the chat box.

   ![Create Agent button](/images/manual/use-cases/lobehub-create-agent1.png#bordered)

2. In the chat box, describe the specific task you want the agent to handle. For example,

   ```text
   I need an agent to review my daily work items and summarize them.
   The summary should focus on the overall purpose of the tasks and
   highlight specific action items.
   ```
3. Select the local language model.
4. Press **Enter**. The profile page of the new agent opens, and you can see the Agent Builder starts configuring your agent automatically.

   ![Agent builder](/images/manual/use-cases/lobehub-agent-builder1.png#bordered)

5. Use the chat interface on the lower right to interact with the Agent Builder. As you provide more details or refine your requirements, the Agent Builder automatically drafts and updates accordingly. 
6. When the creation is completed, click **Start Conversation** to use the agent.
7. Provide your text in the chat, and then you can get the refined results. For example:

   ```text
   - fix bug 405 on login
   - discuss with design on new dashboard
   - answer customer question about billing in email.
   - review pr112, ddl 11:00 am tmrw
   ```
   You get the output:

   ![Sample output by agent builder](/images/manual/use-cases/agent-builder-example1.png#bordered)  

8. If you are satisfied with the agent's performance, pin it for quick access:

   a. Return to the home page.
   
   b. Hover over the agent from the left sidebar, click <i class="material-symbols-outlined">more_horiz</i>, and then click **Pin**.

### Create a custom agent

If you have specific requirements and prefer to configure the agent entirely manually, create a custom agent.

Custom agents offer the highest level of personalization. You can set the agent's avatar, name, AI model, skills, and prompt to create a unique AI agent.

1. On the home page, click the robot icon in the upper left corner, and then select **Create Agent**.

   ![Create custom agent](/images/manual/use-cases/lobehub-create-custom-agent.png#bordered){width=50%} 

   The **Agent Profile** page opens.

   ![Custom agent profile](/images/manual/use-cases/lobehub-custom-agent-profile1.png#bordered)

2. Click the default robot avatar to select a new icon for your agent.
3. Enter the agent name. For example, `SEO Copywriter`.
4. Select the local model.
5. Click **+ Add Skill** to equip the agent with additional tools. For example, select **Web Browsing** for gathering SEO data.
6. Define role and behavior by filling out the structured markdown template to define exactly how the agent operates. For example,

   ```text
   #### Goal
   Write SEO-optimized blog posts based on the user-provided topic.
   #### Skills
   - Keyword research, deployment, and density optimization
   - Engaging headline generation
   - Markdown formatting
   #### Workflow
   1. Ask the user for a topic.
   2. Suggest target keywords, an H1 title, and an optimal meta description.
   3. Generate a structured outline designed for google's featured snippets.
   4. Generate a structured outline for approval.
   5. Write the full blog post once the outline is approved.
   #### Constraints
   - Use simple language and avoid technical jargon.
   - Focus on user values instead of listing product features.
   - Avoid using passive voice.
   - Target users with the second person "you"
   ```
7. Click **Start Conversation** to use it. For example, type the following request:

   ```text
   I want to rank for "local AI alternatives"
   ```
8. Review the proposal and output, and then iterate with it until you are satisfied with the results.

   ![Custom agent result sample](/images/manual/use-cases/lobehub-seo-sample1.png#bordered)

9. If you are satisfied with the agent's performance, pin it for quick access:

   a. Return to the home page.
   
   b. Hover over the agent from the left sidebar, click <i class="material-symbols-outlined">more_horiz</i>, and then click **Pin**.

<!--
## Manage agents

When you have many assistants and group chats, organizing them into groups is the most intuitive way to manage them. It keeps your assistant list clean and makes switching between them easier.

### Pin agents

Pin frequently used assistants to the top of the agent list for quicker access. 
1. On the LobeHub home page, find the assistant in the **Agent** section on the left sidebar.
2. Point to it, click <i class="material-symbols-outlined">more_horiz</i>, and then click **Pin**. The pinned assistants will stay at the top of the list for easy access.

### Categorize agents

create categories to group different agents for

1. On the LobeHub home page, point to **Agent** from the left sidebar, click <i class="material-symbols-outlined">more_horiz</i>, and then click **Add New Category**. A **New Category** section is created under **Agent**.
   ![Add New Category menu](/images/manual/use-cases/lobehub-new-category.png#bordered){width=45%} 

2. Point to **New Category**, click <i class="material-symbols-outlined">more_horiz</i>, and then click **Rename Category**. 

### Move to a group

If you have multiple groups, go to the assistant list or group menu and select "Manage Groups" to easily rename or reorder them.

## Create an agent team

For complex workflows, a single agent might not be enough. LobeHub allows you to create an agent team, where multiple specialized agents collaborate as members, execute tasks in parallel, and iterate on each other's work.

1. On the home page, click **Create Group** under the chat box.

   ![Create Group button](/images/manual/use-cases/lobehub-create-group.png#bordered){width=85%} 

2. In the chat box, describe the specific task you want the agent team to handle. For example,

   ```
   I need a team to research trending AI tech news and write a daily 
   newsletter. One agent should gather the facts, and another should
   format them into an engaging email draft.
   ```
3. Select the language model, and then press **Enter**.

   ![Create Group chat box](/images/manual/use-cases/lobehub-create-group-start.png#bordered){width=85%} 

   The **Group Profile** opens with a **Supervisor** created by default. Every agent team chat includes a built-in moderator responsible for: Understanding your needs and assigning discussion tasks, Coordinating the speaking order of assistants, Summarizing the discussion and extracting key conclusions, and Keeping the conversation organized and on-topic.
   
   Meanwhile, the Lobe AI starts designing the team automatically and lists the steps to complete the task.

   ![Agent group builder](/images/manual/use-cases/lobehub-agent-group-builder.png#bordered){width=85%} 

4. Communicate with Lobe AI to complete the steps:
   - Provide detailed for group settings and agent configurations.
   - Approve the requests to create individudal agent members.
   - Clarity your requirements when necessary.

   When the creation of the team agents is completed, the agents are displayed in Members on the left sidebar.

    ![Agent team member created](/images/manual/use-cases/agent-group-member-created.png#bordered){width=85%}

5. Click **Group Profile** and check the configurations of each agent on its tab. Make adjustments as needed. For example,
 
   - Group Settings:
      - Group name: AI Tech News Research & Newsletter Team
      - Group objectives or work modes: I need a team to research trending AI tech news and write a daily newsletter. This will be used as the shared prompt for team agents.

   - Configure the Supervisor, including the avatar, name, model, skill, and supervisor information to enable more precise workflow coordination.
      - Name: Supervisor
      - Model: Qwen2.5 7B
      - Skill: Web browsing
      - Description: I need a team to research trending AI tech news and write a daily newsletter. This will be used as the shared prompt for team agents.

6. Click **Start Conversation** to use it. For example, type `crawl this webpage https://news.ycombinator.com/ and draft a short, engaging newsletter for the latest three AI news`, and then 

   ![Agent team work result sample](/images/manual/use-cases/lobehub-team-result.png#bordered){width=85%} 

## Manage agent teams

### Add or remove members
 
1. In the team chat, from the left sidebar, point to **Memebers**, and then click the **Add Member** icon to bring additional assistants into the group chat.
2. From the left sidebar, point to an existing member, and then click the **Remove Member** icon to delete the member from the team chat.

### Delete agent teams

1. On the LobeHub home page, point to the target agent team, click <i class="material-symbols-outlined">more_horiz</i>, and then click **Delete**.
-->

## Generate images with ComfyUI

Connect ComfyUI to LobeHub, so you can generate images directly through chat.

### Configure the ComfyUI connection

1. Open Olares Settings, go to **Applications** > **ComfyUI** > **Entrances** > **ComfyUI**, and then copy the **Endpoint** URL. For example, `https://d9ce03380.laresprime.olares.com`.

   ![ComfyUI entrance in Settings](/images/manual/use-cases/comfyui-entrance.png#bordered){width=75%}

2. Open LobeChat, and then go to **Settings** > **AI Service Provider** > **ComfyUI**:

   a. Ensure the **ComfyUI** option is toggled on.

   b. In **ComfyUI Server URL**, paste the **Endpoint** URL you just copied.

   c. For **Authentication Type**, select **No Authentication**.

   ![ComfyUI settings in LobeHub](/images/manual/use-cases/lobehub-comfyui-config.png#bordered)   

6. Locate the **Model List** section, and then check that ComfyUI presets such as **FLUX.1 Schnell** appear.

   ![ComfyUI preset models](/images/manual/use-cases/lobehub-comfyui-preset-models.png#bordered)

   :::tip What are ComfyUI presets?
   The Model List shows ComfyUI presets. A preset is a ready-made image generation workflow in LobeChat.

   For example, **FLUX.1 Schnell** is the display name of the preset, and `comfyui/flux-schnell` is its preset ID. When you select it, LobeChat runs the fixed workflow behind the scenes.
   :::

7. Toggle on the preset you want to use, such as **FLUX.1 Schnell**.

### Generate an image

1. Return to the home page, select **Artwork** from the left sidebar to open the image generation page.

   ![Artwork in LobeHub](/images/manual/use-cases/lobehub-artwork-entry.png#bordered)

2. From the left panel, configure the generation options:

   a. Select a ComfyUI preset model. This guide uses **FLUX.1 Schnell**.

      ![Artwork model select in LobeHub](/images/manual/use-cases/lobehub-artwork-model-select.png#bordered)

   b. For **Aspect Ratio**, select **1:1**.

   c. For **Width** and **Height**, enter the values as needed.

   d. For **Steps**, keep the default value, or lower it for faster generation.

   e. For **Seed**, leave it empty for a random result. Enter a fixed value if you want to recreate a similar image later.

   f. For **Number of Images**, select **1**.

3. Send a prompt. For example:

   ```text
   A black puppy on the beach, sunset
   ```

4. If the **Use custom Fal API Key** popup appears, click **Close message**.
5. Wait for the image to finish generating.

   <!-- ![Generated image in LobeHub](/images/manual/use-cases/lobehub-comfyui-image.png#bordered) -->

:::tip
Image generation might take from a few seconds to a few minutes depending on the model and current load.
:::

### Required models for the FLUX.1 Schnell preset

Each ComfyUI preset needs its own model files placed in the correct directories under **Common** > **comfyui** > **model**. The following table uses the FLUX.1 Schnell preset as an example.

If you see errors such as `Model not found: flux1-schnell.safetensors` or `Failed to queue prompt` while using this preset, a required model file is usually missing or in the wrong directory.

| File | Directory | Download link |
| :--- | :--- | :--- |
| `flux1-schnell-fp8.safetensors` | Both `checkpoints/` and `diffusion_models` | [Download](https://huggingface.co/Comfy-Org/flux1-schnell/resolve/main/flux1-schnell-fp8.safetensors?download=true) |
| `t5xxl_fp16.safetensors` | Both `text_encoders/` and `clip/` | [Download](https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/t5xxl_fp16.safetensors) |
| `clip_l.safetensors` | `text_encoders/` | [Download](https://huggingface.co/comfyanonymous/flux_text_encoders/blob/main/clip_l.safetensors) |
| `ae.safetensors` | `vae/` | [Download](https://huggingface.co/Comfy-Org/Lumina_Image_2.0_Repackaged/resolve/main/split_files/vae/ae.safetensors) |

:::tip Download time
These model files vary in size. The main model is over 16 GB and the text encoder is over 9 GB. Depending on your network speed, downloading the larger files might take from several minutes to a few hours.
:::

### Keep generated images available

By default, generated images might not persist across Pod restarts or session cleanup. To keep images accessible from older conversations, configure S3-compatible object storage for LobeChat. This guide uses Cloudflare R2 as an example. You can also use other S3-compatible services such as AWS S3 or MinIO.

:::warning Use your own credentials
You must create and use your own Cloudflare R2 bucket and API token. Do not use credentials shared by others, including test credentials.
:::

#### Create a Cloudflare R2 bucket and API token

1. Sign up and log in to the [Cloudflare Dashboard](https://dash.cloudflare.com/).

2. From the left sidebar, go to **Storage & databases** > **R2 Object Storage** > **Create bucket**.

3. Click **Create bucket**, and then enter a bucket name such as `LobeHub`.

4. On the R2 overview page, copy the **Account ID**.

5. Go to **Manage R2 API Tokens** > **Create API Token**:

   a. Set the permission to **Object Read & Write** for the bucket.

   b. Save the **Access Key ID** and **Secret Access Key**. The secret is shown only once.

#### Configure the S3 endpoint

The R2 S3 endpoint uses the following format. Do not include the bucket name in the domain.

```text
https://<AccountID>.r2.cloudflarestorage.com
```

For example: `https://a1b2c3d4e5f6.r2.cloudflarestorage.com`

#### Update the LobeChat ConfigMap

1. Open Control Hub from the Launchpad, and then go to **Browse** > **{username}** > **lobechat-{username}** > **Configmaps** > **lobechat-config**.



find the LobeChat ConfigMap named `lobechat-config`.

2. Update the following fields:

   | Field | Value |
   | :--- | :--- |
   | `S3_ACCESS_KEY_ID` | The Access Key ID from R2 |
   | `S3_BUCKET` | Your bucket name, such as `LobeHub` |
   | `S3_ENDPOINT` | `https://<AccountID>.r2.cloudflarestorage.com` |
   | `S3_REGION` | `auto` |
   | `S3_SECRET_ACCESS_KEY` | The Secret Access Key from R2 |
   | `S3_SET_ACL` | `0` |

3. Save the ConfigMap, and then restart the LobeChat Pod to apply the changes.

#### Verify

Generate a new image, refresh the page or open a new conversation, and confirm that the image still loads.

## FAQs

### Why did the connection check fail when I connected to Ollama?

If you encounter the `Error requesting Ollama service` error, troubleshoot as follows and retry:

   ![Connectivity error](/images/manual/use-cases/lobehub-connection-error.png#bordered)
1. Check the Model Console to confirm that the Model shows **READY** and the Engine shows **RUNNING**.
2. Ensure the **Use Client Request Mode** option on the Ollama settings page is disabled.

   ![Disable the use client request mode option](/images/manual/use-cases/lobehub-disable-client-request-mode3.png#bordered)

### Why is the ComfyUI preset list empty?

1. Verify that the **ComfyUI Server URL** in LobeChat matches the **Endpoint** URL from Olares **Settings** > **Applications** > **ComfyUI** > **Entrances** > **ComfyUI**.
2. Open **ComfyUI Launcher** and confirm it shows **Running**.
3. In **ComfyUI Launcher**, go to **Models** > **Installed models**, and confirm the required models are installed.

### Why did image generation fail with "Failed to queue prompt"?

Generation failed:
- Model not found
- Failed to queue prompt
- 


This usually means a required model file is missing or in the wrong directory. See [Required models for the FLUX.1 Schnell preset](#required-models-for-the-flux1-schnell-preset) for the file list, correct paths, and download links.

### Why do generated images disappear after a while?

Without object storage, images might be lost when the LobeChat Pod restarts or the session is cleaned up. See [Keep generated images available](#keep-generated-images-available) for steps to configure Cloudflare R2 or another S3-compatible storage.

### How to download a missing model file from Hugging Face

The following steps use `flux1-schnell.safetensors` as an example. Use the same process to download other missing model files from their links in [Required models for the FLUX.1 Schnell preset](#required-models-for-the-flux1-schnell-preset).

1. Go to [Comfy-Org on Hugging Face](https://huggingface.co/Comfy-Org).
2. Locate **Models**, click the search icon next to it, and then enter `flux1-schnell` in the search field.

   ![Search in Comfy-Org on Hugging Face](/images/manual/use-cases/lobehub-comfy-org-search.png#bordered)

3. From the result list, click **Comfy-Org/flux1-schnell**.

   ![Search result for flux1-schnell](/images/manual/use-cases/lobehub-comfy-org-search-result.png#bordered)

4. Click the **Files and versions** tab, and then select **flux1-schnell-fp8.safetensors**.

   ![FLUX.1 Schnell FP8 file](/images/manual/use-cases/lobehub-comfy-org-fp8.png#bordered)

   :::tip How to choose between the two files
   `flux1-schnell-fp8.safetensors` is the FP8 quantized version. It is smaller, faster, and uses less memory, so it works for most setups.

   `flux1-schnell.safetensors` is the full precision version. It is larger and needs more memory, and might produce slightly better quality.
   :::

5. Click **Download**.
6. Open Olares Files, and then go to the directory shown in [Required models for the FLUX.1 Schnell preset](#required-models-for-the-flux1-schnell-preset). Place the downloaded file in that directory.

If the workflow still reports that `flux1-schnell.safetensors` is missing, rename the downloaded file from `flux1-schnell-fp8.safetensors` to `flux1-schnell.safetensors`.
