# Guide to run the IBU Insurance demo

This guide provides step-by-step instructions to set up this demo on macOS.

## Table of Contents
- [macOS Installation](#macos-installation)
  - [Step 1: Install Homebrew](#step-1-install-homebrew)
  - [Step 2: Install Colima](#step-2-install-colima)
  - [Step 3: Install Docker and Docker Compose](#step-3-install-docker-and-docker-compose)
  - [Step 4: Start Colima](#step-4-start-colima)
  - [Step 5: Clone the Demo Repository](#step-5-clone-the-demo-repository)
  - [Step 6: Setup IBM watsonx.ai](#step-6-setup-ibm-watsonxai)
  - [Step 7: Setup the Demo environment](#step-7-setup-the-demo-environment)
  - [Step 8: Run the Demo](#step-8-run-the-demo)
  - [Step 9: Demo scenario](#step-9-demo-scenario)

## macOS Installation

### Step 1: Install Homebrew
Homebrew is a package manager for macOS that simplifies the installation of software.

To install Homebrew, open your Terminal and run the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After the installation is complete, verify that Homebrew is installed:

```bash
brew --version
```

### Step 2: Install Colima
Colima is a lightweight container runtime for macOS that works with Docker.

To install Colima, run:

```bash
brew install colima
```

If you have an Apple Silicon (M1/M2) Mac, you need to install Rosetta:

```bash
softwareupdate --install-rosetta
```

### Step 3: Install Docker and Docker Compose
Docker is essential for containerization, and Docker Compose helps in managing multi-container applications.

To install Docker and Docker Compose, run:

```bash
brew install docker docker-compose
```

### Step 4: Start Colima
Now, you can start Colima with the desired configuration. If you have an Intel Mac, run:

```bash
colima start --cpu 4 --memory 8
```

For Apple Silicon (M1/M2) Macs, run:

```bash
colima start --cpu 4 --memory 8 --vm-type=vz --vz-rosetta
```

### Step 5: Clone the Demo Repository
Now that your environment is set up, you can clone the `athena-owl-demos` repository. If you don't have Git installed, you can do so by running:

```bash
brew install git
```

Then, clone the repository and navigate into the demo directory:

```bash
git clone https://github.com/AthenaDecisionSystems/athena-owl-demos.git
cd athena-owl-demos
```

### Step 6: Setup IBM watsonx.ai

1. **Create an IBM watsonx.ai account**  
   Visit the [IBM watsonx.ai](https://www.ibm.com/products/watsonx-ai) page and follow the links to set up a Cloud instance.

2. **Generate an API key**  
   In your IBM Cloud account, go to `Profile and settings` and generate an API key.

3. **Instantiate a watsonx.ai service**  
   From your IBM Cloud account, create a new watsonx.ai service instance.

4. **Copy your watsonx.ai configuration parameters**  

   ```
   WATSONX_URL=<your watsonx.ai URL> # example value = https://us-south.ml.cloud.ibm.com/
   WATSONX_APIKEY=<your watsonx.ai API Key>
   WATSONX_PROJECT_ID=<your watsonx.ai Project ID>
   ```

### Step 7: Setup the demo environment

Navigate to the IBU Insurance demo directory:

```
cd IBU-insurance-demo
```

Create your `.env` file. It contains some configuration parameters. Paste your watsonx.ai configuration parameters:

```
WATSONX_URL=<your watsonx.ai URL> # example value = https://us-south.ml.cloud.ibm.com/
WATSONX_APIKEY=<your watsonx.ai API Key>
WATSONX_PROJECT_ID=<your watsonx.ai Project ID>
```

If you plan to use other LLMs for which a key is needed, paste them here:

```plaintex
# Define the IBM KEY to access models deployed on watsonx.ai
IBM_API_KEY=USE_YOUR_IBM_API_KEY

# Only when you want to use one of Open AI model
OPENAI_API_KEY=USE_YOUR_OPENAI_API_KEY

# Only when you want to use one a Mistral AI model
MISTRAL_API_KEY=USE_YOUR_MISTRAL_API_KEY

# Use Tavily to do search on last news, that could be interesting to validate tool calling
TAVILY_API_KEY=USE_YOUR_TAVILY_API_KEY

# if you want to get traces in Langchain tracing - this is optional
LANGCHAIN_TRACING_V2=false
LANGCHAIN_API_KEY=USE_YOUR_LANGCHAIN_KEY

# If you want to use one of the Anthropic Claude models
ANTHROPIC_API_KEY=USE_YOUR_ANTHROPIC_KEY
```

### Step 8: Run the Demo

Once everything is set up, you can run the project by executing:

```sh
cd deployment/local
docker-compose up -d
```

After the project is running, you can access the Owl Agent at:

```plaintext
http://localhost:3000
```

### Step 9: Demo Scenario

---

Your environment should now be fully set up and ready for development and testing. If you encounter any issues, please refer to the documentation or raise an issue in the GitHub repository.

