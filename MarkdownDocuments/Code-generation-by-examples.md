# Code Generation by DSL Examples

---

## Introduction

This document ([notebook](https://github.com/antononcube/PythonForPrediction-blog/blob/main/Notebooks/Jupyter/Code-generation-by-examples.ipynb)) discusses the Python package ["DSLExamples"](https://pypi.org/project/DSLExamples/), [AAp1], which is a "data package" with examples of DSL commands translations to programming code. The DSL examples are suitable for [LLM few-shot training](https://www.prompthub.us/blog/the-few-shot-prompting-guide).

The function `llm_example_function` provided by ["LLMFunctionObjects"](https://pypi.org/project/LLMFunctionObjects/), [AAp2], can be effectively used to create translation functions utilizing those examples.
The utilization of such LLM-translation functions is exemplified below. A table with the results of correctness and speed experiments is also shown.

The presentation ["Robust LLM pipelines (Mathematica, Python, Raku)"](https://youtu.be/QOsVTCQZq_s), [AAv1], discusses the use of LLM-example functions in more general settings:

- [Short introduction](https://youtu.be/QOsVTCQZq_s?t=89)
- [Detailed explanations](https://www.youtube.com/watch?v=QOsVTCQZq_s&t=2840s)


Similar translations -- with much less computational resources -- are achieved with grammar-based DSL translators; see the Raku package ["DSL::Translators"](https://github.com/antononcube/Raku-DSL-Translators), [AAp2].
The Raku package ["LLM::Resources"](https://github.com/antononcube/Raku-LLM-Resources), [AAp4], has LLM-graphs for code generation that utilize the DSL examples of this package.

---

## Usage examples

Get all examples:

```python
from DSLExamples import *
from DataTypeSystem import * 
import pandas as pd

t = dsl_examples()
deduce_type(t)
```

```
# Assoc(Atom(<class 'str'>), Assoc(Atom(<class 'str'>), Assoc(Atom(<class 'str'>), Atom(<class 'str'>), 20), 7), 4)
```

Tabulate all translation languages and available workflow examples:

```python
rows = [
    {"language": lang, "workflow": workflow}
    for lang, workflows in dsl_examples().items()
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


Note that for `dsl_examples` the language to translate from is specified. Currently, the package has DSL examples for Bulgarian, English, Portuguese, and Russian (being from-languages.)  

Get the examples for Latent Semantic Analysis (**LSA**) **Mon**adic pipeline segments in Python:

```python
pd.DataFrame([{"command": k, "code": v} for k, v in dsl_examples('Python', 'LSAMon').items()]) 
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


Make an LLM example function for translation of LSA workflow building commands:

```python
from LLMFunctionObjects import *;
from LLMPrompts import *

conf = llm_configuration('ChatGPT', model = "gpt-5.5")

llm_pipeline_segment = llm_example_function(dsl_examples('WL', 'LSAMon'), llm_evaluator=conf)
```

Run the LLM function over a list of DSL commands: 

```python
commands =[
"use the dataset aAbstracts",
"make the document-term matrix without stemming",
"extract 42 topics using the method non-negative matrix factorization",
"show the topics"
]

gen_code = "⟹\n".join([llm_pipeline_segment(x).replace('Output:','').strip() for x in commands])
print(gen_code)
```

```
# LSAMonUnit[aAbstracts]⟹
# LSAMonMakeDocumentTermMatrix["StemmingRules" -> None]⟹
# LSAMonExtractTopics["NumberOfTopics" -> 42, Method -> "NNMF"]⟹
# LSAMonEchoTopics[]

```

Same workflow specified in Bulgarian:

```python
llm_pipeline_segment_bg = llm_example_function(dsl_examples(lang='WL', workflow='LSAMon', from_lang='Bulgarian'), llm_evaluator=conf)

commands = [
"използавай данните aAbstracts",
"направи документ-терм матрицата без да използаваш стъблата на думите",
"намери 40 теми ползвайки методата не-отрицателна матрична факторизация",
"покажи темите"
]

gen_code = "⟹\n".join([llm_pipeline_segment_bg(x).replace('Output:','').strip() for x in commands])
print(gen_code)
```

```
# LSAMonUnit[aAbstracts]⟹
# LSAMonMakeDocumentTermMatrix["StemmingRules" -> None]⟹
# LSAMonExtractTopics[40, Method -> "NNMF"]⟹
# LSAMonEchoTopicsTable[]

```

----

## Correctness and speed of different models 

The following performance table -- for both correctness and speed -- was derived with the following code:

```python
# LLM access configuration
conf = llm_configuration(<Provider>, model = <model>)

# Main testing function
llm_pipeline_segment = llm_example_function(dsl_examples('WL', 'LSAMon'), llm_evaluator=conf)

# Secondary testing function (for ≈ 1/4th of the models)
llm_pipeline_segment = llm_example_function(dsl_examples('WL', 'LSAMon'),  llm_evaluator = conf, prompts = 'Do the generation by example as quickly as possible. Do not overthink it.')
```

A few observations:

- For a few of the models the secondary function produced different results, but for most not difference in results and timings were observed.
- For some of the models that produced a correct results multiple executions were made and occasionally different (and wrong) code was generated.
- The tests are for line-by-line generation; en-bloc tests can produce different results.
  - Hopefully, fast and correct.   
- Of course, a more extensive benchmark study can be staged for different combinations of models, programming languages, natural languages, and workflows.
    - But that would require more planning, time, and resources.


<table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>Provider</th>
      <th>Model</th>
      <th>Approx. Time, s</th>
      <th>Approx. Time, m</th>
      <th>InWords</th>
      <th>Success</th>
      <th>Comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ollama</td>
      <td>gemma4:26b</td>
      <td>240</td>
      <td>4</td>
      <td>Very slow</td>
      <td>Correct</td>
      <td></td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>gemma4:26b</td>
      <td>190</td>
      <td>3.2</td>
      <td>Very slow</td>
      <td>Correct</td>
      <td>max_tokens = 500</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>gemma4:26b</td>
      <td>240</td>
      <td>4</td>
      <td>Very slow</td>
      <td>Wrong</td>
      <td>with the do-not-overthink-it prompt</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>gemma3:12b</td>
      <td>240</td>
      <td>4</td>
      <td>Very slow</td>
      <td>Wrong</td>
      <td>generates lots of redundant code and explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>qwen3-coder:30b</td>
      <td>25</td>
      <td>0.5</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>gpt-oss:20b</td>
      <td>90</td>
      <td>1.5</td>
      <td>Slow</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>qwen3.5:9b</td>
      <td>630</td>
      <td>10.5</td>
      <td>Very, very slow</td>
      <td>Correct</td>
      <td>Correct code followed by a Markdown table</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>qwen3.6:35b</td>
      <td>480</td>
      <td>8</td>
      <td>Very, very slow</td>
      <td>Correct</td>
      <td></td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>qwen3.6:35b</td>
      <td>300</td>
      <td>5</td>
      <td>Slow</td>
      <td>Correct</td>
      <td>with the do-not-overthink prompt</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>llama3.2:latest</td>
      <td>25</td>
      <td>0.5</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>gemma3:1b</td>
      <td>15</td>
      <td>0.4</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td></td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>gemma3:4b</td>
      <td>60</td>
      <td>1</td>
      <td>Slowish</td>
      <td>Wrong</td>
      <td>generates a bunch of code</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>deepseek-r1:latest</td>
      <td>250</td>
      <td>4.5</td>
      <td>Slow</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations and JSON structures</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>lfm2</td>
      <td>40</td>
      <td>0.67</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>lfm2</td>
      <td>2400</td>
      <td>40</td>
      <td>Very, very slow</td>
      <td>Interrupted</td>
      <td>with the do-not-overthink-it prompt</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>granite4.1:3b</td>
      <td>12</td>
      <td>0.2</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>granite4.1:8b</td>
      <td>60</td>
      <td>1</td>
      <td>Slowish</td>
      <td>Wrong</td>
      <td>generates a bunch of explanations</td>
    </tr>
    <tr>
      <td>Ollama</td>
      <td>medgemma1.5:4b</td>
      <td>1640</td>
      <td>24</td>
      <td>Very, very slow</td>
      <td>Interrupted</td>
      <td></td>
    </tr>
    <tr>
      <td>ChatGPT</td>
      <td>gpt-4.1-mini</td>
      <td>4</td>
      <td>0.1</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td></td>
    </tr>
    <tr>
      <td>ChatGPT</td>
      <td>gpt-4.1</td>
      <td>5</td>
      <td>0.1</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td></td>
    </tr>
    <tr>
      <td>ChatGPT</td>
      <td>gpt-5.3-chat-latest</td>
      <td>13</td>
      <td>0.2</td>
      <td>Fast</td>
      <td>Correct</td>
      <td></td>
    </tr>
    <tr>
      <td>ChatGPT</td>
      <td>gpt-5.4-mini</td>
      <td>4</td>
      <td>0.1</td>
      <td>Fast</td>
      <td>Wrong</td>
      <td></td>
    </tr>
    <tr>
      <td>ChatGPT</td>
      <td>gpt-5.5</td>
      <td>45</td>
      <td>0.75</td>
      <td>Slowish</td>
      <td>Correct</td>
      <td></td>
    </tr>
    <tr>
      <td>Gemini</td>
      <td>gemini-3.5-flash</td>
      <td>40</td>
      <td>0.67</td>
      <td>Slowish</td>
      <td>Correct</td>
      <td></td>
    </tr>
  </tbody>
</table>

Testing for speed and correctness helps to make more adequate designs for agentic translation architectures like the ones described in the article ["Day 6 – Robust code generation combining grammars and LLMs"](https://raku-advent.blog/2025/12/06/day-6-robust-code-generation-combining-grammars-and-llms/), [AA1, AAn1].

---

## Implementation details

There are several ways to organize the DSL examples with respect to the from-languages:

| Type                                                                                   | Comment                                                | Currently used                      | 
|----------------------------------------------------------------------------------------|--------------------------------------------------------|-------------------------------------|
| Have a separate file for each from-langauge                                            | Convenient editing and refinement                      | Yes                                 |
| One file of all examples; from-langauge is a key for each workflow                     | Can be produces with the separate files                | No                                  |
| Keep English-only DSL examples and use dictionaries of command translations to English | Does not train the LLM directly with the from-language | Dictionaries are kept for reference |


This [Jupyter notebook](https://github.com/antononcube/Raku-DSL-Examples/blob/main/docs/DSL-examples-dev.ipynb) has a workflow for the translation of the English DSL examples into other languages.

---

## References

## Articles, blog posts

[AA1] Anton Antonov, ["Day 6 – Robust code generation combining grammars and LLMs"](https://raku-advent.blog/2025/12/06/day-6-robust-code-generation-combining-grammars-and-llms/), (2025), [Raku Advent Calendar](https://raku-advent.blog).

### Notebooks

[AAn1] Anton Antonov, ["Robust code generation combining grammars and LLMs"](https://community.wolfram.com/groups/-/m/t/3588794), (2025), [Wolfram Community](https://community.wolfram.com).

### Packages

[AAp1] Anton Antonov, [DSLExamples, Python package](https://github.com/antononcube/Python-DSLExamples/), (2026), [GitHub/antononcube](https://github.com/antononcube). ([PyPI.org](https://pypi.org/project/DSLExamples).)

[AAp2] Anton Antonov, [LLMFunctionObjects, Python package](https://github.com/antononcube/Python-packages/tree/main/LLMFunctionObjects), (2023-2026), [GitHub/antononcube](https://github.com/antononcube). ([PyPI.org](https://pypi.org/project/LLMFunctionObjects).)

[AAp3] Anton Antonov, [DSL::Translators, Raku package](https://github.com/antononcube/Raku-DSL-Translators), (2020-2026), [GitHub/antononcube](https://github.com/antononcube).

[AAp4] Anton Antonov, [LLM::Resources, Raku package](https://github.com/antononcube/Raku-LLM-Resources), (2026), [GitHub/antononcube](https://github.com/antononcube).

### Videos

[AAv1] Anton Antonov, ["Robust LLM pipelines (Mathematica, Python, Raku)"](https://youtu.be/QOsVTCQZq_s), (2024), [YouTube/AAA4prediction](https://www.youtube.com/@AAA4prediction).