# Tutorial - Bootstrap your AI app project

## Prerequisites
:information_source: You will need Docker and Docker Desktop on your machine.

## Create an 'Hello World' Agent App with Athena Owl Framework

- create a root for everything related to Athena
```
mkdir AthenaDecisionSystems
```
- clone athena-owl-core and athena-owl-demos github repositories
```
cd AthenaDecisionSystems
git clone git@github.com:AthenaDecisionSystems/athena-owl-core.git 
git clone git@github.com:AthenaDecisionSystems/athena-owl-demos.git
```
- create a folder that will be the placeholder for your first Athena project: 
```
mkdir my-athena-ai-app
```
:information_source: **Recommendation:** we recommend that you place the 'my-athena-ai-app' folder at the same level as the 'athena-owl-core' folder on your disk. This will make your life easier to reference files using relative paths.

- copy the skeleton app SkeletonAppHelloLLM into your app folder and rename it
```
cp -a athena-owl-demos/SkeletonApp-HelloLLM/. my-athena-ai-app/
```
- create a .env file in the app folder
Add the API keys for the various third party providers that you want to use. 
```
cd my-athena-ai-app
vi .env
```

In this demo, we use OpenAI and Tavily. So, the .env file should look like this:
```
OPENAI_API_KEY=<your key here>
TAVILY_API_KEY=<your key here>
```

- run the demo using DockerCompose:
First make sure Docker Desktop is started.
```
cd deployment/local
docker compose up -d
```
