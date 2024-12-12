# Tutorial - Bootstrap your AI app project

## Prerequisites
:information_source: *You will need Docker and Docker Desktop on your machine.*

## Create an 'Hello World' Agent App with Athena Owl Framework

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

We are now ready to interact with our Hello World AI application.

Start a browser and point to <a href="http://localhost:3000" target="_blank">localhost:3000</a>

- we will use the OOTB chatbot to send queries that will be served by the pre-configured LLM. Click on the Chatbot tab in the top navigation bar. In the chat, enter the following message:
```
What is the capital of Italy?
```
The agent will use the common knowledge of an LLM like OpenAI to provide an answer. A reasonable answer should look like:
``` { .text .no-copy }
The capital of Italy is Rome.

```

- our agent has been configured to use Tavily to search the web for all queries that require fresh data that was not available when the LLM was trained.
```
As of today, what is the exchange rate between dollars and euros?
```
The agent will perform an API call to Tavily. A reasonable answer should look like:
``` { .text .no-copy }
SSS
```

- tool calling to retrieve customer data 

==Play with Swagger ui== 

==Make a direct call the the APIs using cURL== 

