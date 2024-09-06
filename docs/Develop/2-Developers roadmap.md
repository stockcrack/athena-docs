# Developer's roadmap to build a custom solution

## Understand which approach works well for a given use case
LLMs are very good at providing linguistic capabilities such as:

- extracting data from text  
- translation  
- intent detection  
- sentiment analysis  
- summarization  

On the other hand, LLMs are completely unable to reason in a logical, trustable, predictable or explainable manner. 
There are situations where it is key to rely on robust symbolic reasoning capabilities in order to: 

- deduce new facts from known facts  
- decide in accordance to company policies  

If you need to take business decisions that must be trusted from a risk and compliance perspective, it would be completely unreasonable to 
place a statistical bet on the fact that a linguistic parrot can find the right answer.

The right approach is to use the right technology for the right use cases. The Athena Owl Agent framework will make your life easy at integrating various approaches to build an hybrid AI solution.

## Choose the right ingredients

The following table shows various features that are used by three different applications built via the Athena Owl Agent framework. 
Depending on the capabilities we want to provide to the employees that will interact with the agent, we will leverage different features. 

|                      | Simple agent with RAG  | Insurance complaint handling     | Tax assistant       |
|----------------------|--------------|----------------------------------|---------------------|
| LLM                  | general Q&A          |  extract data, detect intention  |  extract data, detect intention, generate summary of decision outcome      |
| RAG                  | queries on company documents             |  queries on company policies            |                     |
| Fetch data (tool calling to data APIs)         |              |       fetch customer and claim data          | fetch tax payer and vehicle data                 |
| Decide based on company policies (tool calling to rule service)    |              |               determine next best action     | determine eligibility for a tax discount                 |
| Human in the loop, Open/Closed conversation  |              |                               | ask specific questions needed to decide                 |


## Create a solution folder using a template

- Install Python 3.11 if needed
- Run `owl-solution-create` script

## Create a Python virtual environment
```
cd $DEMO/ibu_backend/src
python3 -m venv .venv
source .venv/bin/activate
```

## Install dependencies
Next, we need to install the Python library dependencies and set the Python path.
```
pip install -r requirements.txt
cd .. 
source setpython.sh
```

## Declare the various elements of the solution
Edit yaml files (or use admin console)

## Add hooks to custom code
This is how we can declare a hook to override the default agent runner class
```
ibu_tax_agent:
  agent_id: ibu_tax_agent
  name: Tax Agent
  description: OpenAI based agent with tool to call tax reduction eligibility service
  runner_class_name: ibu.llm.agents.IBU_TaxAgent.IBU_TaxAgent                            # custom runner class
  class_name: athena.llm.agents.base_chain_agent.OwlAgent
  modelClassName: langchain_openai.ChatOpenAI
  modelName: gpt-3.5-turbo-0125
  prompt_ref: tax_first_intent_prompt
  temperature: 0
  top_k: 1
  top_p: 1
```

## Setup unit and integration tests
To run all the unit tests:
pytest -s tests/ut

To run the integration tests:
...

## Implement custom code
We create a file ibu.llm.agents.IBU_TaxAgent.IBU_TaxAgent.py to implement the IBU_TaxAgent class
```
from athena.llm.agents.agent_mgr import OwlAgentDefaultRunner, OwlAgent, get_agent_manager

class IBU_TaxAgent(OwlAgentDefaultRunner):
    # your custom code here
```


Implement an agentic workflow using Langgraph

## Run tests
### Exploratory testing

There are two possible ways to run the out-of-the-box frontend webapp:

- Build and run a Docker image for the frontend so that it will be started by `docker compose up -d` script
- Run the chatbot webapp locally using node (version >= 18.18) and yarn

If needed, install node: 
```
nvm install --lts
nvm use --lts
```

If needed, install yarn: `npm install --global yarn`

```
cd owl-agent-interface
yarn
yarn dev
```

### Unit testing
To run all the unit tests:
```
pytest -s tests/ut
```

### Integration testing
Before running the integration tests, we need to start the Docker containers:  
```
cd $DEMO/deployment/local
docker compose up -d
```

We can then run all the integration tests:
```
pytest -s tests/it
```
## Package

## Deliver solution