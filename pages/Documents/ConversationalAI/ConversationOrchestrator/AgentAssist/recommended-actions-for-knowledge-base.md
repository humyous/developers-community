---
pagename: Recommended Actions for Knowledge Base
redirect_from:
  - maven-maven-assist-recommended-actions-for-knowledge-base.html
  - maven-ai-maven-assist-recommended-actions-for-knowledge-base.html
  - conversation-orchestrator-maven-assist-recommended-actions-for-knowledge-base.html
Keywords:
sitesection: Documents
categoryname: "Conversational AI"
documentname: Conversation Orchestrator
subfoldername: Agent Assist
permalink: conversation-orchestrator-agent-assist-recommended-actions-for-knowledge-base.html
indicator: messaging
---

Conversation Orchestrator can recommend [Knowledge Base](conversation-builder-knowledge-base.html) articles that match customer intents in the Recommended Actions widget. 

<img class="fancyimage" width="750" src="img/maven/RA-KB.png">


Follow along with this guide to set up the feature.

### Enable Recommended Actions Widget

Note: The Recommended Actions Widget is only available in the Old Agent Workspace. The new Agent Workspace displays Recommended Actions within the conversation view

You will first need to enable the feature in Conversational Cloud. Please contact LivePerson support or your LivePerson customer representative to enable this feature. 

### Create Knowledge Base and Article

Before Conversation Orchestrator can start recommending articles in the widget you need to [create a knowledge base](conversation-builder-knowledge-base.html#adding-a-knowledge-base) and then [create an article](conversation-builder-knowledge-base.html#adding-your-first-article). 

You may continue to add more content at any time after the setup, but it is suggested to create at least one article so you can verify that your setup was completed. 

Once you have created an article, go back to the main screen.

Click on the **User Icon** in the top right corner, and select **Settings**.

**Copy** the **API Access Key**, as you will need this in the next set of steps.

### Configure Conversation Orchestrator

1. Log in to Conversation Orchestrator with your Conversational Cloud credentials and then navigate to Agent Assist > Recommended Actions - Knowledge Base. 

2. Paste the API access key that you copied in the previous step in the API Access Key text field, and then click save. Once you save, you should expect to see the Knowledge base you created previously. 

### Test KB Article Suggestions

1. Start a new messaging conversation from the consumer side

2. Open Conversational Cloud and accept the conversation. This will open the conversation window

3. Check to see if Recommended Actions Widget is visible

    <img class="fancyimage" width="200" src="img/maven/image_30.png">

4. From the consumer side enter a message that is relevant to a KB article you want to be triggered. In this example the user is asking about how to cancel an order

    <img class="fancyimage" width="750" src="img/maven/image_31.png">

### Use Suggestion in a Conversation

Click the **Use Article** button to copy the text to the agent’s text input area. You may choose to edit the text before sending it to the consumer

<img class="fancyimage" width="750" src="img/maven/image_32.png">

### Train the Model

Click the **Vote up** or **Vote down** buttons to provide feedback on the suggestions. Voting up tells the model that it was a relevant suggestion, while voting down tells it that it’s not. The relevance score is calibrated against this feedback so that Conversation Orchestrator can continuously improve it’s recommendations and provide the most relevant content to agents.  

<img class="fancyimage" width="750" src="img/maven/image_33.png">
