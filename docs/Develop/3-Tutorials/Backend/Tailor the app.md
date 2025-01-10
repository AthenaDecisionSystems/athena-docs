# Tailor an Agent App to your needs

Let's have a look at the `docker-compose.yaml` file under `deployment/local` to understand how an application can be customized. The docker compose file defines a component called `ibu-backend` that uses a predefined Docker image provided by Athena Decision Systems and serves APIs to interact with the agent app.

When running an ibu-backend Docker container, it will be able to access 3 mounted volumes that play a crucial role to customize the application:
- `../../ibu_backend/src/config:/app/config`. This mounted volume is used to access various configuration files for our app, such a prompts.yaml, agents.yaml
- `../../ibu_backend/src/ibu:/app/ibu`. This mounted volume is used to access your custom Python code.
- `./data/file_content:/app/file_content`. This mounted volume is used to store the document database used for RAG.


```yaml
  # THE BACKEND
  ibu-backend:
    hostname: ibu-backend
    image: athenadecisionsystems/athena-owl-backend:1.0.0
    container_name: ibu-backend
    ports:
      - 8002:8000
    environment:
      CONFIG_FILE: /app/config/config.yaml
    env_file:
      - ../../.env    # path to the file containing the API keys of your various providers
    volumes:
      - ../../ibu_backend/src/config:/app/config  # <-- access the app configuration files
      - ../../ibu_backend/src/ibu:/app/ibu        # <-- access some custom Python code
      - ./data/file_content:/app/file_content     # <-- used for the document database and RAG
```


### Create a new prompt
All the prompts used by our application are specified in the `prompts.yaml` file located in the folder `ibu_backend/src/config`.


### Create a new agent
All the agents used by the application are specified in the `agents.yaml` file located in the folder `ibu_backend/src/config`

### Add a new tool
Register a new function as a tool in the tool factory