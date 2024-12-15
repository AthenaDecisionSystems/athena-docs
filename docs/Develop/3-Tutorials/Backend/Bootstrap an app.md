# Tutorial - Bootstrap your AI app project 

In this tutorial, we will guide you through all the steps needed to create a new AI app with the Athena Owl Framework.

We will:  
- create a new AI app project from a template  
- run the AI app so that you can understand the various ways to interact with the app  
- modify the AI app to add a new agent with its own prompt and a custom tool

## Prerequisites
:information_source: *You will need to be able to run Docker on your machine. The following instructions have been built using Docker Desktop but you can use alternative tools such as Colima or Rancher Desktop.*

## Create a new Agent App from a template

Create a working directory for everything related to Athena and move into it. From a terminal, you can do:
```
mkdir AthenaDecisionSystems
cd AthenaDecisionSystems
```

Clone athena-owl-core and athena-owl-demos github repositories
```
git clone https://github.com/AthenaDecisionSystems/athena-owl-core.git
git clone https://github.com/AthenaDecisionSystems/athena-owl-demos.git
```
Alternatively and if you have an SSH key installed for github, you can do this:
```
git clone git@github.com:AthenaDecisionSystems/athena-owl-core.git 
git clone git@github.com:AthenaDecisionSystems/athena-owl-demos.git
```
The athena-owl-core repository contains the code of the Athena backend (implemented using FastAPI) and frontend component (implemented as a single page application using React). As developers, we will see how we can build a custom app. We will be able to reuse some code, for instance Python functions from the backend.

Create a folder that will be the placeholder for your first Athena project: 
```
mkdir my-athena-ai-app
```
:information_source: **Recommendation:** *place the 'my-athena-ai-app' folder directly under the AthenaDecisionSystems folder (so at the same level as the 'athena-owl-core' folder) on your disk. We provide some scripts that rely on relative paths and they will be easier to use.*

Copy the content of the skeleton app SkeletonAppHelloLLM into your app folder

```
cp -a athena-owl-demos/SkeletonApp-HelloLLM/. my-athena-ai-app/
```

Create a .env file in the app folder
Add the API keys for the various third party providers that you want to use. 
```
cd my-athena-ai-app
vi .env
```

In this demo, we use OpenAI and Tavily. So, the .env file should look like this:
```
OPENAI_API_KEY=<your OpenAI key here>
TAVILY_API_KEY=<your Tavily key here>
```
Please insert your OpenAI and Tavily API keys in the .env file if you already have the keys. 
Otherwise, follow these links to create keys for <a href="https://platform.openai.com/docs/quickstart" target="_blank">OpenAI</a> and <a href="https://app.tavily.com/" target="_blank">Tavily</a>

We are now ready to run our application. First, make sure that Docker Desktop is started and then use this command to start the demo with Docker Compose:
```
cd deployment/local
docker compose up -d
```

This will pull the two Docker images that are used to run our application:

- athenadecisionsystems/athena-owl-backend is the backend component that will serve APIs

- athenadecisionsystems/athena-owl-frontend is the OOTB web application that can be used to interact with AI agents using a chatbot interface.

Once the images have been pulled, you can check that two containers are actually up and running:
```
docker ps
```
The output of the command should look like: 
``` { .text .no-copy }
CONTAINER ID   IMAGE                                             COMMAND                  CREATED          STATUS          PORTS                    NAMES
d1bee478ae99   athenadecisionsystems/athena-owl-backend:1.0.0    "uvicorn athena.main…"   11 seconds ago   Up 10 seconds   0.0.0.0:8002->8000/tcp   ibu-backend
35530e7bd690   athenadecisionsystems/athena-owl-frontend:1.0.0   "docker-entrypoint.s…"   6 days ago       Up 10 seconds   0.0.0.0:3000->3000/tcp   owl-frontend
```

## Run the default 'Hello LLM' Agent App
We are now ready to interact with our Hello World AI application.
Running our Agent App is a good way to understand how we can interact with it, either using an out-of-the-box chatbot provided as a webapp or by calling APIs.

Start a browser and point to <a href="http://localhost:3000" target="_blank">localhost:3000</a>

- we will use the OOTB chatbot to send queries that will be served by the pre-configured LLM. Click on the Chatbot tab in the top navigation bar. In the chat, enter the following message:
```
What is the capital of Italy?
```
The agent will use the common knowledge of an LLM like OpenAI to provide an answer. A reasonable answer should look like:
``` { .text .no-copy }
The capital of Italy is Rome.

```

- The agent has been configured with some tools and will use a specific tool to retrieve customer data based on their email address. If you enter the following query:
```
What are the details of peter@acme.com?
```
You will get the following response:
``` { .text .no-copy }
Here are the details for the client with the email "peter@acme.com":
    Email: peter@acme.com
    Date of Birth: December 14, 1994
    Income: $19,500 USD
    Country of Residence: United States
```
The response is returned by a custom function that mimicks a call to an enterprise data API.

- The agent can also use Tavily to search the web for all queries that require fresh data that was not available when the LLM was trained.
```
what is the weather today in London? 
please also provide your source in terms of web url
```
The agent will perform an API call to Tavily. A reasonable answer should look like:
``` { .text .no-copy }
The current weather in London is as follows:

    Temperature: 7.2°C (45.0°F)
    Condition: Light drizzle
    Wind: 5.4 mph (8.6 kph) from the northeast
    Humidity: 100%
    Pressure: 1029.0 mb
    Visibility: 6.0 km (3.0 miles)
    Feels Like: 5.6°C (42.0°F)

This information is sourced from WeatherAPI.
```

Play with the Athena Backend APIs using Swagger UI
Start a browser and point to <a href="http://localhost:8002/docs" target="_blank">localhost:8002/docs</a>
Use the generic_chat endpoint with the following JSON payload
``` json
{
  "locale": "en",
  "query": "what is the data of pierre@acme.fr?",
  "user_id": "123",
  "agent_id": "hello_world_agent_with_tools"
}
```
The response should look like this
``` json
{
  "messages": [
    {
      "content": "The data for the email address pierre@acme.fr is as follows:\n\n- **Date of Birth**: December 14, 1994\n- **Income**: 19,500 EUR\n- **Country of Residence**: France",
      "style_class": null
    }
  ],
  "closed_questions": null,
  "reenter_into": null,
  "status": 200,
  "error": "",
  "chat_history": [
    {
      "content": "what is the data of pierre@acme.fr?",
      "role": "human"
    },
    {
      "content": "The data for the email address pierre@acme.fr is as follows:\n\n- **Date of Birth**: December 14, 1994\n- **Income**: 19,500 EUR\n- **Country of Residence**: France",
      "role": "AI"
    }
  ],
  "user_id": "123",
  "agent_id": "hello_world_agent_with_tools",
  "thread_id": "8d3a7baf-b266-4f35-9a28-04edff0a5703"
}
```

If you want, you can also make direct calls to the Athena Backend APIs using cURL from a terminal.
```
curl -X 'POST' \
  'http://localhost:8002/api/v1/c/generic_chat' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "locale": "en",
  "query": "what is the data of pierre@acme.fr?",
  "user_id": "123",
  "agent_id": "hello_world_agent_with_tools"
}'
```

## Create your own Agent App

### Create a new prompt
Edit the prompts.yaml file under ...

### Create a new agent
Edit the agents.yaml file

### Add a new tool
Register a new function as a tool in the tool factory