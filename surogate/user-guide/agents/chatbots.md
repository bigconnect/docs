# Chatbots

Chatbot is one of the most common AI scenarios. However, effectively understanding user queries and responding appropriately remains a challenge. Surogate's general-purpose chatbot agent is our attempt to tackle this longstanding issue.

This chatbot closely resembles the chatbot introduced in [Start an AI chat](../chat/start-ai-chat.md), but with a key difference - it introduces a reflective mechanism that allows it to improve the retrieval from the target knowledge bases by rewriting the user's query.

This document provides guides on creating such a chatbot using our chatbot template.

### Create a chatbot agent from template[​](https://ragflow.io/docs/dev/general_purpose_chatbot#create-a-chatbot-agent-from-template) <a href="#create-a-chatbot-agent-from-template" id="create-a-chatbot-agent-from-template"></a>

To create a general-purpose chatbot agent using our template:

1. Click the **Agent** tab in the middle top of the page to show the **Agent** page.
2. Click **+ Create agent** on the top right of the page to show the **agent template** page.
3. On the **agent template** page, hover over the card on **General-purpose chatbot** and click **Use this template**.\
   &#xNAN;_&#x59;ou are now directed to the **no-code workflow editor** page._

<figure><img src="../../../.gitbook/assets/52e7dc62-4bf5-4fbb-ab73-4a6e252065f0.jpg" alt=""><figcaption></figcaption></figure>

### Understand each component in the template[​](https://ragflow.io/docs/dev/general_purpose_chatbot#understand-each-component-in-the-template) <a href="#understand-each-component-in-the-template" id="understand-each-component-in-the-template"></a>

Here’s a breakdown of each component and its role and requirements in the chatbot template:

* **Begin**
  * Function: Sets an opening greeting for users.
  * Purpose: Establishes a welcoming atmosphere and prepares the user for interaction.
* **Interact**
  * Function: Serves as the interface between human and the bot.
  * Role: Acts as the downstream component of **Begin**.
* **Retrieval**
  * Function: Retrieves information from specified knowledge base(s).
  * Requirement: Must have `knowledgebases` set up to function.
* **Relevant**
  * Function: Assesses the relevance of the retrieved information from the **Retrieval** component to the user query.
  * Process:
    * If relevant, it directs the data to the **Generate** component for final response generation.
    * Otherwise, it triggers the **Rewrite** component to refine the user query and redo the retrival process.
* **Generate**
  * Function: Prompts the LLM to generate responses based on the retrieved information.
  * Note: The prompt settings allow you to control the way in which the LLM generates responses. Be sure to review the prompts and make necessary changes.
* **Rewrite**:
  * Function: Refines a user query when no relevant information from the knowledge base is retrieved.
  * Usage: Often used in conjunction with **Relevant** and **Retrieval** to create a reflective/feedback loop.

### Configure your chatbot agent[​](https://ragflow.io/docs/dev/general_purpose_chatbot#configure-your-chatbot-agent) <a href="#configure-your-chatbot-agent" id="configure-your-chatbot-agent"></a>

1.  Click **Begin** to set an opening greeting:\


    <figure><img src="https://github.com/user-attachments/assets/4416bc16-2a84-4f24-a19b-6dc8b1de0908" alt=""><figcaption></figcaption></figure>
2.  Click **Retrieval** to select the right knowledge base(s) and make any necessary adjustments:\


    <figure><img src="https://github.com/user-attachments/assets/5f694820-5651-45bc-afd6-cf580ca0228d" alt=""><figcaption></figcaption></figure>
3.  Click **Generate** to configure the LLM's summarization behavior:\
    3.1. Confirm the model.\
    3.2. Review the prompt settings. If there are variables, ensure they match the correct component IDs:\


    <figure><img src="https://github.com/user-attachments/assets/19e94ea7-7f62-4b73-b526-32fcfa62f1e9" alt=""><figcaption></figcaption></figure>
4. Click **Relevant** to review or change its settings:\
   &#xNAN;_&#x59;ou may retain the current settings, but feel free to experiment with changes to understand how the agent operates._&#x20;
5. Click **Rewrite** to select a different model for query rewriting or update the maximum loop times for query rewriting:\
   ![choose\_model](https://github.com/user-attachments/assets/2bac1d6c-c4f1-42ac-997b-102858c3f550) ![loop\_time](https://github.com/user-attachments/assets/09a4ce34-7aac-496f-aa59-d8aa33bf0b1f)



1. Update your workflow where you see necessary.
2. Click to **Save** to apply your changes.\
   &#xNAN;_&#x59;our agent appears as one of the agent cards on the **Agent** page._

### Test your chatbot agent[​](https://ragflow.io/docs/dev/general_purpose_chatbot#test-your-chatbot-agent) <a href="#test-your-chatbot-agent" id="test-your-chatbot-agent"></a>

1. Find your chatbot agent on the **Agent** page:

<figure><img src="../../../.gitbook/assets/6e6382c6-9a86-4190-9fdd-e363b7f64ba9.jpg" alt=""><figcaption></figcaption></figure>

2. Experiment with your questions to verify if this chatbot functions as intended:

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

