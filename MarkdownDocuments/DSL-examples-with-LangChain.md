# DSL examples with LangChain

Anton Antonov  
[PythonForPrediction at WordPress](https://pythonforprediction.wordpress.com)  
February 2026

----

## Introduction

This notebook demonstrates the usage of the Python data package ["DSLExamples"](https://pypi.org/project/DSLExamples), [AAp1], with examples of Domain Specific Language (DSL) commands translations to programming code.

The provided DSL examples are suitable for [LLM few-shot training](https://www.prompthub.us/blog/the-few-shot-prompting-guide). [LangChain](https://www.langchain.com) can be used to create translation pipelines utilizing those examples. The utilization of such LLM-translation pipelines is exemplified below.

The Python package closely follows the Raku package  ["DSL::Examples"](https://raku.land/zef:antononcube/DSL::Examples), [AAp2], and Wolfram Language paclet ["DSLExamples"](https://resources.wolframcloud.com/PacletRepository/resources/AntonAntonov/DSLExamples), [AAp3], and has (or should have) the same DSL examples data.


**Remark:** Similar translations -- with much less computational resources -- are achieved with grammar-based DSL translators; see ["DSL::Translators"](https://github.com/antononcube/Raku-DSL-Translators), [AAp4].


---

## Setup


Load the packages used below:

```python
from DSLExamples import dsl_examples, dsl_workflow_separators
from langchain_core.output_parsers import StrOutputParser
from langchain_ollama import ChatOllama

import pandas as pd
import os
```

---

## Retrieval

Get all examples and retrieve specific language/workflow slices.


```python
all_examples = dsl_examples()
python_lsa = dsl_examples("Python", "LSAMon")
separators = dsl_workflow_separators("WL", "LSAMon")

list(all_examples.keys()), list(python_lsa.keys())[:5]

```

```
# (['WL', 'Python', 'R', 'Raku'],
  ['load the package',
   'use the documents aDocs',
   'use dfTemp',
   'make the document-term matrix',
   'make the document-term matrix with automatic stop words'])
```

---

## Tabulate Languages and Workflows


```python
rows = [
    {"language": lang, "workflow": workflow}
    for lang, workflows in all_examples.items()
    for workflow in workflows.keys()
]

pd.DataFrame(rows).sort_values(["language", "workflow"]).reset_index(drop=True)

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>language</th>
      <th>workflow</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Python</td>
      <td>LSAMon</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Python</td>
      <td>QRMon</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Python</td>
      <td>SMRMon</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Python</td>
      <td>pandas</td>
    </tr>
    <tr>
      <th>4</th>
      <td>R</td>
      <td>DataReshaping</td>
    </tr>
    <tr>
      <th>5</th>
      <td>R</td>
      <td>LSAMon</td>
    </tr>
    <tr>
      <th>6</th>
      <td>R</td>
      <td>QRMon</td>
    </tr>
    <tr>
      <th>7</th>
      <td>R</td>
      <td>SMRMon</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Raku</td>
      <td>DataReshaping</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Raku</td>
      <td>SMRMon</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Raku</td>
      <td>TriesWithFrequencies</td>
    </tr>
    <tr>
      <th>11</th>
      <td>WL</td>
      <td>ClCon</td>
    </tr>
    <tr>
      <th>12</th>
      <td>WL</td>
      <td>DataReshaping</td>
    </tr>
    <tr>
      <th>13</th>
      <td>WL</td>
      <td>LSAMon</td>
    </tr>
    <tr>
      <th>14</th>
      <td>WL</td>
      <td>QRMon</td>
    </tr>
    <tr>
      <th>15</th>
      <td>WL</td>
      <td>SMRMon</td>
    </tr>
    <tr>
      <th>16</th>
      <td>WL</td>
      <td>Tabular</td>
    </tr>
    <tr>
      <th>17</th>
      <td>WL</td>
      <td>TriesWithFrequencies</td>
    </tr>
  </tbody>
</table>
</div>


---

## Python LSA Examples


```python
pd.DataFrame([{"command": k, "code": v} for k, v in python_lsa.items()])

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>command</th>
      <th>code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>load the package</td>
      <td>from LatentSemanticAnalyzer import *</td>
    </tr>
    <tr>
      <th>1</th>
      <td>use the documents aDocs</td>
      <td>LatentSemanticAnalyzer(aDocs)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>use dfTemp</td>
      <td>LatentSemanticAnalyzer(dfTemp)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>make the document-term matrix</td>
      <td>make_document_term_matrix()</td>
    </tr>
    <tr>
      <th>4</th>
      <td>make the document-term matrix with automatic s...</td>
      <td>make_document_term_matrix[stemming_rules=None,...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>make the document-term matrix without stemming</td>
      <td>make_document_term_matrix[stemming_rules=False...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>apply term weight functions</td>
      <td>apply_term_weight_functions()</td>
    </tr>
    <tr>
      <th>7</th>
      <td>apply term weight functions: global IDF, local...</td>
      <td>apply_term_weight_functions(global_weight_func...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>extract 30 topics using the method SVD</td>
      <td>extract_topics(number_of_topics=24, method='SVD')</td>
    </tr>
    <tr>
      <th>9</th>
      <td>extract 24 topics using the method NNMF, max s...</td>
      <td>extract_topics(number_of_topics=24, min_number...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Echo topics table</td>
      <td>echo_topics_interpretation(wide_form=True)</td>
    </tr>
    <tr>
      <th>11</th>
      <td>show the topics</td>
      <td>echo_topics_interpretation(wide_form=True)</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Echo topics table with 10 terms per topic</td>
      <td>echo_topics_interpretation(number_of_terms=10,...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>find the statistical thesaurus for the words n...</td>
      <td>echo_statistical_thesaurus(terms=stemmerObj.st...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>show statistical thesaurus for king, castle, p...</td>
      <td>echo_statistical_thesaurus(terms=stemmerObj.st...</td>
    </tr>
  </tbody>
</table>
</div>


---

## LangChain few-shot prompt


Build a few-shot prompt from the DSL examples, then run it over commands.


```python
from langchain_core.prompts import FewShotPromptTemplate, PromptTemplate

# Use a small subset of examples as few-shot demonstrations
example_pairs = list(python_lsa.items())[:5]
examples = [
    {"command": cmd, "code": code}
    for cmd, code in example_pairs
]

example_prompt = PromptTemplate(
    input_variables=["command", "code"],
    template="Command: {command}\nCode: {code}"
)

few_shot_prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    prefix=(
        "You translate DSL commands into Python code that builds an LSA pipeline."
        "Follow the style of the examples."
    ),
    suffix="Command: {command}\nCode:",
    input_variables=["command"],
)

print(few_shot_prompt.format(command="show the topics"))

```

```
# You translate DSL commands into Python code that builds an LSA pipeline.Follow the style of the examples.
# 
# Command: load the package
# Code: from LatentSemanticAnalyzer import *
# 
# Command: use the documents aDocs
# Code: LatentSemanticAnalyzer(aDocs)
# 
# Command: use dfTemp
# Code: LatentSemanticAnalyzer(dfTemp)
# 
# Command: make the document-term matrix
# Code: make_document_term_matrix()
# 
# Command: make the document-term matrix with automatic stop words
# Code: make_document_term_matrix[stemming_rules=None,stopWords=True)
# 
# Command: show the topics
# Code:

```

---

## Translation With Ollama Model

Run the few-shot prompt against a local Ollama model. 

```python
llm = ChatOllama(model=os.getenv("OLLAMA_MODEL", "gemma3:12b"))

commands = [
    "use the dataset aAbstracts",
    "make the document-term matrix without stemming",
    "extract 40 topics using the method non-negative matrix factorization",
    "show the topics",
]

llm = ChatOllama(model="gemma3:12b")

chain = few_shot_prompt | llm | StrOutputParser()

sep = dsl_workflow_separators('Python', 'LSAMon')

result = []
for command in commands:
    result.append(chain.invoke({"command": command}))

print(sep.join([x.strip() for x in result]))

```

```
# LatentSemanticAnalyzer(aAbstracts)
# .make_document_term_matrix(stemming_rules=None)
# .extract_topics(40, method='non-negative_matrix_factorization')
# .show_topics()

```

---

## Simulated Translation With a Fake LLM

For testing purposes it might be useful to use a fake LLM so the notebook runs without setup and API keys.

```python
try:
    from langchain_community.llms.fake import FakeListLLM
except Exception:
    from langchain_core.language_models.fake import FakeListLLM

commands = [
    "use the dataset aAbstracts",
    "make the document-term matrix without stemming",
    "extract 40 topics using the method non-negative matrix factorization",
    "show the topics",
]

# Fake responses to demonstrate the flow
fake_responses = [
    "lsamon = lsamon_use_dataset(\"aAbstracts\")",
    "lsamon = lsamon_make_document_term_matrix(stemming=False)",
    "lsamon = lsamon_extract_topics(method=\"NMF\", n_topics=40)",
    "lsamon_show_topics(lsamon)",
]

llm = FakeListLLM(responses=fake_responses)

# Create a simple chain by piping the prompt into the LLM
chain = few_shot_prompt | llm

for command in commands:
    result = chain.invoke({"command": command})
    print("Command:", command)
    print("Code:", result)
    print("-")

```

```
# Command: use the dataset aAbstracts
# Code: lsamon = lsamon_use_dataset("aAbstracts")
# -
# Command: make the document-term matrix without stemming
# Code: lsamon = lsamon_make_document_term_matrix(stemming=False)
# -
# Command: extract 40 topics using the method non-negative matrix factorization
# Code: lsamon = lsamon_extract_topics(method="NMF", n_topics=40)
# -
# Command: show the topics
# Code: lsamon_show_topics(lsamon)
# -

```

---

## References

[AAp1] Anton Antonov, [DSLExamples, Python package](https://github.com/antononcube/Python-DSLExamples), (2026), [GitHub/antononcube](https://github.com/antononcube).

[AAp2] Anton Antonov, [DSL::Examples, Raku package](https://github.com/antononcube/Raku-DSL-Examples), (2025-2026), [GitHub/antononcube](https://github.com/antononcube).

[AAp3] Anton Antonov [DSLExamples, Wolfram Language paclet](https://resources.wolframcloud.com/PacletRepository/resources/AntonAntonov/DSLExamples/), (2025-2026), [Wolfram Language Paclet Repository](https://resources.wolframcloud.com/publishers/resources?PublisherID=AntonAntonov).

[AAp4] Anton Antonov, [DSL::Translators, Raku package](https://github.com/antononcube/Raku-DSL-Translators), (2020-2024), [GitHub/antononcube](https://github.com/antononcube).