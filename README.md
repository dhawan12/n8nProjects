# n8nProjects

# n8n Workflow: Gmail Labels Categorizer

This workflow automates the categorization of Gmail emails by analyzing their content and applying relevant labels.

## Description

The workflow uses the Gmail Trigger to monitor for new emails.  It then employs an OpenAI Chat Model to classify the email text into predefined categories.  Finally, it uses the Gmail node to automatically apply the appropriate label to the email.

## Workflow Details

1.  **Gmail Trigger:** Triggers the workflow when a new email arrives in the specified Gmail account.
2.  **Agent Label:** Uses a Langchain Text Classifier with an OpenAI Chat Model to analyze the email text and categorize it.  The categories include:
    * Other
    * Mails clients
    * Newsletters
    * Services en ligne (outil, abonnement)
    * Sponsorship
    * Collaboration
3.  **Gmail Nodes (Sponsorship, Newsletter, Mail clients, Collaboration, Services en ligne):** These nodes apply the corresponding Gmail label based on the category determined by the Agent Label node.

## Setup Instructions

1.  Ensure you have n8n installed and a Gmail account.
2.  Import the `Gmail_Labels_Categorizer.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Gmail Trigger** and **Gmail** nodes with your Gmail OAuth2 credentials.  You'll need to authorize n8n to access your Gmail account.
    * Configure the **OpenAI Chat Model** node with your OpenAI API credentials.
4.  **Label IDs:**
    * In each of the Gmail nodes (Sponsorship, Newsletter, etc.), you'll need to replace the placeholder `labelIds` with the actual IDs of your Gmail labels.  You can find these IDs in your Gmail settings.

## Usage

Once configured, the workflow will automatically run whenever a new email arrives in your inbox, categorizing and labeling it.

# n8n Workflow: Jina Scrapper

This workflow creates an AI Agent Chatbot that can scrape content from the Jina.ai website and answer user questions based on the scraped data.

## Description

The workflow receives a user's chat message, uses an AI Agent to scrape the Jina.ai website, and then employs a Google Gemini Chat Model to generate a response based on the scraped content.

## Workflow Details

1.  **When chat message received:** A Langchain Chat Trigger that starts the workflow when a chat message is received.
2.  **Jina.ai Web Scraping Agent:** An AI Agent that uses a tool to scrape the Jina.ai website. The prompt instructs the agent to use the scraped data to answer the user's question.
3.  **Google Gemini Chat Model:** Uses the Gemini model to generate a user-friendly response based on the information retrieved by the scraping agent.
4.  **Jina ai Web Scraper Tool:** A Langchain Tool HTTP Request node that performs the web scraping of the Jina.ai website.
5.  **Window Buffer Memory:** A Langchain Memory Buffer Window node to store conversation history.

## Setup Instructions

1.  Ensure you have n8n installed.
2.  Import the `Jina_Scrapper.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Google Gemini Chat Model** node with your Google Gemini (PaLM) API credentials.

## Usage

Trigger the workflow by sending a chat message. The AI Agent will scrape the Jina.ai website and provide a relevant answer.

# n8n Workflow: MCPServer

This workflow sets up an MCP (Multi-Control Platform) server within n8n, enabling it to interact with various tools.

## Description

The workflow uses an MCP Trigger to receive requests and then routes them to different tools, including Gmail, a Calculator, and a Social Post Generator.

## Workflow Details

1.  **MCP Server Trigger:** Triggers the workflow when an MCP request is received at the `/mcp` path.
2.  **Gmail:** A Gmail Tool node that can send emails.
3.  **Calculator:** A Langchain Tool Calculator node that can perform calculations.
4.  **Social Post Generator:** A Langchain Tool Workflow node that triggers another n8n workflow (`GkFwLLhUQORb7o0Z`) to generate social media content.

## Setup Instructions

1.  Ensure you have n8n and the necessary tool workflows (like the Social Post Generator) installed.
2.  Import the `MCPServer.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Gmail** node with your Gmail OAuth2 credentials.
    * Configure the **MCP Client** node with your MCP Client (SSE) account credentials.
4.  **Workflow ID:**
    * In the **Social Post Generator** node, ensure that the `workflowId` parameter (`GkFwLLhUQORb7o0Z`) matches the ID of your social media content generation workflow.

## Usage

Send MCP requests to the `/mcp` path to utilize the available tools (Gmail, Calculator, Social Post Generator).

# n8n Workflow: MCP Agents

This workflow sets up an AI Agent that can interact with an MCP (Multi-Control Platform) client.

## Description

The workflow receives chat messages, uses a Google Gemini Chat Model to process them, and interacts with an MCP client to perform actions.

## Workflow Details

1.  **When chat message received:** A Langchain Chat Trigger that starts the workflow when a chat message is received.
2.  **AI Agent:** A Langchain Agent that uses the Google Gemini Chat Model and the MCP Client tool.
3.  **Google Gemini Chat Model:** Uses the Gemini model to process the chat message.
4.  **Window Buffer Memory:** A Langchain Memory Buffer Window node to store conversation history.
5.  **MCP Client:** A Langchain Tool MCP Client node to interact with the MCP client.

## Setup Instructions

1.  Ensure you have n8n and an MCP client set up.
2.  Import the `MCP_Agents.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Google Gemini Chat Model** node with your Google Gemini (PaLM) API credentials.
    * Configure the **MCP Client** node with your MCP Client (SSE) account credentials.

## Usage

Send chat messages to the workflow. The AI Agent will process the messages and interact with the MCP client as needed.

# n8n Workflow: MultiAgent Content Generation

This workflow generates content for different platforms (LinkedIn, Blog, X) based on a given topic and target audience.

## Description

The workflow takes a content topic and audience as input, retrieves relevant information using the Tavily search API, and then uses OpenAI Chat Models to generate content tailored for LinkedIn, a blog, and X (Twitter).  It also updates a Google Sheet with the generated content.

## Workflow Details

1.  **Google Sheets:** Reads input data (topic, audience) from a Google Sheet.
2.  **Edit Fields:** Sets the `query` and `audience` variables.
3.  **HTTP Request:** Uses the Tavily search API to retrieve information related to the topic.
4.  **Split Out:** Splits the results from the Tavily API.
5.  **Aggregate:** Aggregates the relevant data (title, raw content).
6.  **LinkedIn:** An AI Agent using an OpenAI Chat Model to generate a LinkedIn post.
7.  **Blog:** An AI Agent using an OpenAI Chat Model to generate a blog article.
8.  **X:** An AI Agent using an OpenAI Chat Model to generate a tweet for X.
9.  **OpenAI Chat Models (3 instances):** These nodes use the OpenAI `gpt-4o` model to generate the content for the respective platforms.
10. **Google Sheets (2nd instance):** Updates the original Google Sheet with the generated LinkedIn post, blog article, and X tweet.
11. **Vector Store Tool, Pinecone Vector Store, OpenAI Chat Model4, Embeddings OpenAI, Information Extractor:** Nodes related to Vector Store to analyze the person’s past posts and identify their tone, writing style, and favorite topics.

## Setup Instructions

1.  Ensure you have n8n, a Google account, an OpenAI account, a Tavily API key, and a Pinecone account.
2.  Import the `MultiAgent_Content_Generation.json` file into your n8n instance.
3.  **Credentials:**
    * Configure both **Google Sheets** nodes with your Google Sheets API credentials.  Ensure the sheet is shared appropriately.
    * Configure the **OpenAI Chat Model** nodes with your OpenAI API credentials.
    * In the **HTTP Request** node, replace `"TA CLE API"` with your actual Tavily API key.
    * Configure the **Pinecone Vector Store** node with your Pinecone credentials.
4.  **Google Sheet IDs:**
    * In both **Google Sheets** nodes, replace the placeholder spreadsheet IDs and sheet names with the actual IDs and names of your Google Sheet.

## Usage

The workflow is designed to be triggered by data in a Google Sheet.  When a row in the sheet contains a topic and audience, the workflow will generate content for LinkedIn, a blog, and X, and then update the sheet with the generated content.



Okay, I've analyzed the n8n workflow files you provided. Here's a breakdown of README content tailored to each workflow:

1. Gmail_Labels_Categorizer.json

Markdown

# n8n Workflow: Gmail Labels Categorizer

This workflow automates the categorization of Gmail emails by analyzing their content and applying relevant labels.

## Description

The workflow uses the Gmail Trigger to monitor for new emails.  It then employs an OpenAI Chat Model to classify the email text into predefined categories.  Finally, it uses the Gmail node to automatically apply the appropriate label to the email.

## Workflow Details

1.  **Gmail Trigger:** Triggers the workflow when a new email arrives in the specified Gmail account.
2.  **Agent Label:** Uses a Langchain Text Classifier with an OpenAI Chat Model to analyze the email text and categorize it.  The categories include:
    * Other
    * Mails clients
    * Newsletters
    * Services en ligne (outil, abonnement)
    * Sponsorship
    * Collaboration
3.  **Gmail Nodes (Sponsorship, Newsletter, Mail clients, Collaboration, Services en ligne):** These nodes apply the corresponding Gmail label based on the category determined by the Agent Label node.

## Setup Instructions

1.  Ensure you have n8n installed and a Gmail account.
2.  Import the `Gmail_Labels_Categorizer.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Gmail Trigger** and **Gmail** nodes with your Gmail OAuth2 credentials.  You'll need to authorize n8n to access your Gmail account.
    * Configure the **OpenAI Chat Model** node with your OpenAI API credentials.
4.  **Label IDs:**
    * In each of the Gmail nodes (Sponsorship, Newsletter, etc.), you'll need to replace the placeholder `labelIds` with the actual IDs of your Gmail labels.  You can find these IDs in your Gmail settings.

## Usage

Once configured, the workflow will automatically run whenever a new email arrives in your inbox, categorizing and labeling it.
2. Jina_Scrapper.json

Markdown

# n8n Workflow: Jina Scrapper

This workflow creates an AI Agent Chatbot that can scrape content from the Jina.ai website and answer user questions based on the scraped data.

## Description

The workflow receives a user's chat message, uses an AI Agent to scrape the Jina.ai website, and then employs a Google Gemini Chat Model to generate a response based on the scraped content.

## Workflow Details

1.  **When chat message received:** A Langchain Chat Trigger that starts the workflow when a chat message is received.
2.  **Jina.ai Web Scraping Agent:** An AI Agent that uses a tool to scrape the Jina.ai website. The prompt instructs the agent to use the scraped data to answer the user's question.
3.  **Google Gemini Chat Model:** Uses the Gemini model to generate a user-friendly response based on the information retrieved by the scraping agent.
4.  **Jina ai Web Scraper Tool:** A Langchain Tool HTTP Request node that performs the web scraping of the Jina.ai website.
5.  **Window Buffer Memory:** A Langchain Memory Buffer Window node to store conversation history.

## Setup Instructions

1.  Ensure you have n8n installed.
2.  Import the `Jina_Scrapper.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Google Gemini Chat Model** node with your Google Gemini (PaLM) API credentials.

## Usage

Trigger the workflow by sending a chat message. The AI Agent will scrape the Jina.ai website and provide a relevant answer.
3. MCPServer.json

Markdown

# n8n Workflow: MCPServer

This workflow sets up an MCP (Multi-Control Platform) server within n8n, enabling it to interact with various tools.

## Description

The workflow uses an MCP Trigger to receive requests and then routes them to different tools, including Gmail, a Calculator, and a Social Post Generator.

## Workflow Details

1.  **MCP Server Trigger:** Triggers the workflow when an MCP request is received at the `/mcp` path.
2.  **Gmail:** A Gmail Tool node that can send emails.
3.  **Calculator:** A Langchain Tool Calculator node that can perform calculations.
4.  **Social Post Generator:** A Langchain Tool Workflow node that triggers another n8n workflow (`GkFwLLhUQORb7o0Z`) to generate social media content.

## Setup Instructions

1.  Ensure you have n8n and the necessary tool workflows (like the Social Post Generator) installed.
2.  Import the `MCPServer.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Gmail** node with your Gmail OAuth2 credentials.
    * Configure the **MCP Client** node with your MCP Client (SSE) account credentials.
4.  **Workflow ID:**
    * In the **Social Post Generator** node, ensure that the `workflowId` parameter (`GkFwLLhUQORb7o0Z`) matches the ID of your social media content generation workflow.

## Usage

Send MCP requests to the `/mcp` path to utilize the available tools (Gmail, Calculator, Social Post Generator).
4. MCP_Agents.json

Markdown

# n8n Workflow: MCP Agents

This workflow sets up an AI Agent that can interact with an MCP (Multi-Control Platform) client.

## Description

The workflow receives chat messages, uses a Google Gemini Chat Model to process them, and interacts with an MCP client to perform actions.

## Workflow Details

1.  **When chat message received:** A Langchain Chat Trigger that starts the workflow when a chat message is received.
2.  **AI Agent:** A Langchain Agent that uses the Google Gemini Chat Model and the MCP Client tool.
3.  **Google Gemini Chat Model:** Uses the Gemini model to process the chat message.
4.  **Window Buffer Memory:** A Langchain Memory Buffer Window node to store conversation history.
5.  **MCP Client:** A Langchain Tool MCP Client node to interact with the MCP client.

## Setup Instructions

1.  Ensure you have n8n and an MCP client set up.
2.  Import the `MCP_Agents.json` file into your n8n instance.
3.  **Credentials:**
    * Configure the **Google Gemini Chat Model** node with your Google Gemini (PaLM) API credentials.
    * Configure the **MCP Client** node with your MCP Client (SSE) account credentials.

## Usage

Send chat messages to the workflow. The AI Agent will process the messages and interact with the MCP client as needed.
5. MultiAgent_Content_Generation.json

Markdown

# n8n Workflow: MultiAgent Content Generation

This workflow generates content for different platforms (LinkedIn, Blog, X) based on a given topic and target audience.

## Description

The workflow takes a content topic and audience as input, retrieves relevant information using the Tavily search API, and then uses OpenAI Chat Models to generate content tailored for LinkedIn, a blog, and X (Twitter).  It also updates a Google Sheet with the generated content.

## Workflow Details

1.  **Google Sheets:** Reads input data (topic, audience) from a Google Sheet.
2.  **Edit Fields:** Sets the `query` and `audience` variables.
3.  **HTTP Request:** Uses the Tavily search API to retrieve information related to the topic.
4.  **Split Out:** Splits the results from the Tavily API.
5.  **Aggregate:** Aggregates the relevant data (title, raw content).
6.  **LinkedIn:** An AI Agent using an OpenAI Chat Model to generate a LinkedIn post.
7.  **Blog:** An AI Agent using an OpenAI Chat Model to generate a blog article.
8.  **X:** An AI Agent using an OpenAI Chat Model to generate a tweet for X.
9.  **OpenAI Chat Models (3 instances):** These nodes use the OpenAI `gpt-4o` model to generate the content for the respective platforms.
10. **Google Sheets (2nd instance):** Updates the original Google Sheet with the generated LinkedIn post, blog article, and X tweet.
11. **Vector Store Tool, Pinecone Vector Store, OpenAI Chat Model4, Embeddings OpenAI, Information Extractor:** Nodes related to Vector Store to analyze the person’s past posts and identify their tone, writing style, and favorite topics.

## Setup Instructions

1.  Ensure you have n8n, a Google account, an OpenAI account, a Tavily API key, and a Pinecone account.
2.  Import the `MultiAgent_Content_Generation.json` file into your n8n instance.
3.  **Credentials:**
    * Configure both **Google Sheets** nodes with your Google Sheets API credentials.  Ensure the sheet is shared appropriately.
    * Configure the **OpenAI Chat Model** nodes with your OpenAI API credentials.
    * In the **HTTP Request** node, replace `"TA CLE API"` with your actual Tavily API key.
    * Configure the **Pinecone Vector Store** node with your Pinecone credentials.
4.  **Google Sheet IDs:**
    * In both **Google Sheets** nodes, replace the placeholder spreadsheet IDs and sheet names with the actual IDs and names of your Google Sheet.

## Usage

The workflow is designed to be triggered by data in a Google Sheet.  When a row in the sheet contains a topic and audience, the workflow will generate content for LinkedIn, a blog, and X, and then update the sheet with the generated content.


Important Notes:

Credentials: Pay very close attention to the credential setup in each workflow. Incorrect credentials will prevent the workflows from running.
Dependencies: Some workflows may rely on specific n8n nodes or other workflows. Make sure you have all necessary components installed.
Customization: These READMEs are based on the structure of your workflow files. You may need to further customize them with specific details about your use case or any modifications you've made.
Security: Be mindful of storing API keys and credentials securely. Avoid committing them directly to your repository. Consider using environment variables or secure configuration methods.
I hope this helps you create thorough and informative README files for your n8n workflows!
