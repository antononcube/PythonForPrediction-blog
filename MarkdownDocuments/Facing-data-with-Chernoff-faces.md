# Facing data with Chernoff faces

## Introduction

This blog post notebook proclaims the Python package
["ChernoffFace"](https://pypi.org/project/ChernoffFace/)
and outlines and exemplifies its function `chernoff_face` that generates
[Chernoff diagrams](https://en.wikipedia.org/wiki/Chernoff_face).

The design, implementation *strategy*, and unit tests closely resemble
the Wolfram Repository Function (WFR)
[`ChernoffFace`](https://resources.wolframcloud.com/FunctionRepository/resources/ChernoffFace),
\[AAf1\], and the original Mathematica package
[“ChernoffFaces.m”](https://github.com/antononcube/MathematicaForPrediction/blob/master/ChernoffFaces.m),
\[AAp1\].

------------------------------------------------------------------------

## Installation

To install from GitHub use the shell command:

    python -m pip install git+https://github.com/antononcube/Python-packages.git#egg=ChernoffFace\&subdirectory=ChernoffFace

To install from PyPI:

    python -m pip install ChernoffFace

------------------------------------------------------------------------

# Usage examples

## Setup


```python
from ChernoffFace import *
import numpy
import matplotlib.cm
```


## Random data


```python
# Generate data
numpy.random.seed(32)
data = numpy.random.rand(16, 12)
```


```python
# Make Chernoff faces
fig = chernoff_face(data=data,
                    titles=[str(x) for x in list(range(len(data)))],
                    color_mapper=matplotlib.cm.Pastel1)
```


    
![png](https://raw.githubusercontent.com/antononcube/PythonForPrediction-blog/main/MarkdownDocuments/Diagrams/Facing-data-with-Chernoff-faces/output_4_0.png)
    


## Employee attitude data

Get Employee attitude data


```python
dfData=load_employee_attitude_data_frame()
dfData.head()
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
      <th>Rating</th>
      <th>Complaints</th>
      <th>Privileges</th>
      <th>Learning</th>
      <th>Raises</th>
      <th>Critical</th>
      <th>Advancement</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>43</td>
      <td>51</td>
      <td>30</td>
      <td>39</td>
      <td>61</td>
      <td>92</td>
      <td>45</td>
    </tr>
    <tr>
      <th>1</th>
      <td>63</td>
      <td>64</td>
      <td>51</td>
      <td>54</td>
      <td>63</td>
      <td>73</td>
      <td>47</td>
    </tr>
    <tr>
      <th>2</th>
      <td>71</td>
      <td>70</td>
      <td>68</td>
      <td>69</td>
      <td>76</td>
      <td>86</td>
      <td>48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>61</td>
      <td>63</td>
      <td>45</td>
      <td>47</td>
      <td>54</td>
      <td>84</td>
      <td>35</td>
    </tr>
    <tr>
      <th>4</th>
      <td>81</td>
      <td>78</td>
      <td>56</td>
      <td>66</td>
      <td>71</td>
      <td>83</td>
      <td>47</td>
    </tr>
  </tbody>
</table>
</div>



Rescale the variables:


```python
dfData2 = variables_rescale(dfData)
dfData2.head()
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
      <th>Rating</th>
      <th>Complaints</th>
      <th>Privileges</th>
      <th>Learning</th>
      <th>Raises</th>
      <th>Critical</th>
      <th>Advancement</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.066667</td>
      <td>0.264151</td>
      <td>0.000000</td>
      <td>0.121951</td>
      <td>0.400000</td>
      <td>1.000000</td>
      <td>0.425532</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.511111</td>
      <td>0.509434</td>
      <td>0.396226</td>
      <td>0.487805</td>
      <td>0.444444</td>
      <td>0.558140</td>
      <td>0.468085</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.688889</td>
      <td>0.622642</td>
      <td>0.716981</td>
      <td>0.853659</td>
      <td>0.733333</td>
      <td>0.860465</td>
      <td>0.489362</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.466667</td>
      <td>0.490566</td>
      <td>0.283019</td>
      <td>0.317073</td>
      <td>0.244444</td>
      <td>0.813953</td>
      <td>0.212766</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.911111</td>
      <td>0.773585</td>
      <td>0.490566</td>
      <td>0.780488</td>
      <td>0.622222</td>
      <td>0.790698</td>
      <td>0.468085</td>
    </tr>
  </tbody>
</table>
</div>



Make the corresponding Chernoff faces:


```python
fig = chernoff_face(data=dfData2,
                    n_columns=5,
                    long_face=False,
                    color_mapper=matplotlib.cm.tab20b,
                    figsize=(8, 8), dpi=200)
```


    
![png](https://raw.githubusercontent.com/antononcube/PythonForPrediction-blog/main/MarkdownDocuments/Diagrams/Facing-data-with-Chernoff-faces/output_10_0.png)
    


## USA arrests data

Get USA arrests data:



```python
dfData=load_usa_arrests_data_frame()
dfData.head()
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
      <th>StateName</th>
      <th>Murder</th>
      <th>Assault</th>
      <th>UrbanPopulation</th>
      <th>Rape</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>13.2</td>
      <td>236</td>
      <td>58</td>
      <td>21.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>10.0</td>
      <td>263</td>
      <td>48</td>
      <td>44.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>8.1</td>
      <td>294</td>
      <td>80</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>8.8</td>
      <td>190</td>
      <td>50</td>
      <td>19.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>9.0</td>
      <td>276</td>
      <td>91</td>
      <td>40.6</td>
    </tr>
  </tbody>
</table>
</div>



Rescale the variables:


```python
dfData2 = variables_rescale(dfData)
dfData2.head()
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
      <th>StateName</th>
      <th>Murder</th>
      <th>Assault</th>
      <th>UrbanPopulation</th>
      <th>Rape</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>0.746988</td>
      <td>0.654110</td>
      <td>0.440678</td>
      <td>0.359173</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>0.554217</td>
      <td>0.746575</td>
      <td>0.271186</td>
      <td>0.961240</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>0.439759</td>
      <td>0.852740</td>
      <td>0.813559</td>
      <td>0.612403</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>0.481928</td>
      <td>0.496575</td>
      <td>0.305085</td>
      <td>0.315245</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>0.493976</td>
      <td>0.791096</td>
      <td>1.000000</td>
      <td>0.860465</td>
    </tr>
  </tbody>
</table>
</div>



Make the corresponding Chernoff faces using USA state names as titles:



```python
fig = chernoff_face(data=dfData2,
                    n_columns=5,
                    long_face=False,
                    color_mapper=matplotlib.cm.tab20c_r,
                    figsize=(12, 12), dpi=200)
```


    
![png](https://raw.githubusercontent.com/antononcube/PythonForPrediction-blog/main/MarkdownDocuments/Diagrams/Facing-data-with-Chernoff-faces/output_16_0.png)
    


------------------------------------------------------------------------

## References

### Articles

\[AA1\] Anton Antonov, [“Making Chernoff faces for data
visualization”](https://mathematicaforprediction.wordpress.com/2016/06/03/making-chernoff-faces-for-data-visualization),
(2016), [MathematicaForPrediction at
WordPress](https://mathematicaforprediction.wordpress.com).

### Functions and packages

\[AAf1\] Anton Antonov,
[`ChernoffFace`](https://resources.wolframcloud.com/FunctionRepository/resources/ChernoffFace),
(2019), [Wolfram Function
Repository](https://resources.wolframcloud.com/FunctionRepository).

\[AAp1\] Anton Antonov, [Chernoff faces implementation in
Mathematica](https://github.com/antononcube/MathematicaForPrediction/blob/master/ChernoffFaces.m),
(2016), [MathematicaForPrediction at
GitHub](https://github.com/antononcube/MathematicaForPrediction).
