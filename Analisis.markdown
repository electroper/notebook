---
jupyter:
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.8.8
  nbformat: 4
  nbformat_minor: 5
---

::: {#adc27445-5e89-4342-a142-cd4648a40bb8 .cell .code execution_count="1"}
``` python
%load_ext watermark
%watermark
```

::: {.output .stream .stdout}
    Last updated: 2022-02-03T20:35:48.641623-05:00

    Python implementation: CPython
    Python version       : 3.8.8
    IPython version      : 7.22.0

    Compiler    : GCC 7.3.0
    OS          : Linux
    Release     : 5.4.0-48-generic
    Machine     : x86_64
    Processor   : x86_64
    CPU cores   : 4
    Architecture: 64bit
:::
:::

::: {#48b52937-7187-4ec3-b82f-0ae29696dc29 .cell .code execution_count="2"}
``` python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.rcParams["figure.figsize"] = (10,6)
```
:::

::: {#8a0505ea-6a64-49c4-9f65-b99d1393f71c .cell .code execution_count="3"}
``` python
url="https://raw.githubusercontent.com/manugarri/curso_data_science/master/Secciones/Seccion4.Analisis_y_procesado_de_datos/Procesado_de_Datos/data/vehiculos_original.csv"
vehiculos = pd.read_csv(url)
vehiculos.head()
```

::: {.output .execute_result execution_count="3"}
```{=html}
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
      <th>make</th>
      <th>model</th>
      <th>year</th>
      <th>displ</th>
      <th>cylinders</th>
      <th>trany</th>
      <th>drive</th>
      <th>VClass</th>
      <th>fuelType</th>
      <th>comb08</th>
      <th>co2TailpipeGpm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AM General</td>
      <td>DJ Po Vehicle 2WD</td>
      <td>1984</td>
      <td>2.5</td>
      <td>4.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>17</td>
      <td>522.764706</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AM General</td>
      <td>DJ Po Vehicle 2WD</td>
      <td>1984</td>
      <td>2.5</td>
      <td>4.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>17</td>
      <td>522.764706</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AM General</td>
      <td>FJ8c Post Office</td>
      <td>1984</td>
      <td>4.2</td>
      <td>6.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>13</td>
      <td>683.615385</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AM General</td>
      <td>FJ8c Post Office</td>
      <td>1984</td>
      <td>4.2</td>
      <td>6.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>13</td>
      <td>683.615385</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AM General</td>
      <td>Post Office DJ5 2WD</td>
      <td>1985</td>
      <td>2.5</td>
      <td>4.0</td>
      <td>Automatic 3-spd</td>
      <td>Rear-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>16</td>
      <td>555.437500</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#c15ec8cb-0e8c-4a85-8221-8ba7f81be9d6 .cell .code execution_count="4"}
``` python
vehiculos=vehiculos.rename(columns={
                "cylinders":"cilindros",
                "trany":"transmision",
                "make":"fabricante",
                "model":"modelo",
                "displ":"desplazamiento", #volumen de desplazamiento del motor
                "drive":"traccion",
                "VClass":"clase",
                "fuelType":"combustible",
                "comb08":"consumo", #combined MPG for fuelType1
                "co2TailpipeGpm":"co2", # tailpipe CO2 in grams/mile
            })
```
:::

::: {#be1c366a-cc92-4c06-a53c-aae9ffb911b3 .cell .code execution_count="5"}
``` python
vehiculos
```

::: {.output .execute_result execution_count="5"}
```{=html}
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
      <th>fabricante</th>
      <th>modelo</th>
      <th>year</th>
      <th>desplazamiento</th>
      <th>cilindros</th>
      <th>transmision</th>
      <th>traccion</th>
      <th>clase</th>
      <th>combustible</th>
      <th>consumo</th>
      <th>co2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AM General</td>
      <td>DJ Po Vehicle 2WD</td>
      <td>1984</td>
      <td>2.5</td>
      <td>4.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>17</td>
      <td>522.764706</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AM General</td>
      <td>DJ Po Vehicle 2WD</td>
      <td>1984</td>
      <td>2.5</td>
      <td>4.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>17</td>
      <td>522.764706</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AM General</td>
      <td>FJ8c Post Office</td>
      <td>1984</td>
      <td>4.2</td>
      <td>6.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>13</td>
      <td>683.615385</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AM General</td>
      <td>FJ8c Post Office</td>
      <td>1984</td>
      <td>4.2</td>
      <td>6.0</td>
      <td>Automatic 3-spd</td>
      <td>2-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>13</td>
      <td>683.615385</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AM General</td>
      <td>Post Office DJ5 2WD</td>
      <td>1985</td>
      <td>2.5</td>
      <td>4.0</td>
      <td>Automatic 3-spd</td>
      <td>Rear-Wheel Drive</td>
      <td>Special Purpose Vehicle 2WD</td>
      <td>Regular</td>
      <td>16</td>
      <td>555.437500</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>38431</th>
      <td>smart</td>
      <td>fortwo electric drive coupe</td>
      <td>2011</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Automatic (A1)</td>
      <td>Rear-Wheel Drive</td>
      <td>Two Seaters</td>
      <td>Electricity</td>
      <td>87</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>38432</th>
      <td>smart</td>
      <td>fortwo electric drive coupe</td>
      <td>2013</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Automatic (A1)</td>
      <td>Rear-Wheel Drive</td>
      <td>Two Seaters</td>
      <td>Electricity</td>
      <td>107</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>38433</th>
      <td>smart</td>
      <td>fortwo electric drive coupe</td>
      <td>2014</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Automatic (A1)</td>
      <td>Rear-Wheel Drive</td>
      <td>Two Seaters</td>
      <td>Electricity</td>
      <td>107</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>38434</th>
      <td>smart</td>
      <td>fortwo electric drive coupe</td>
      <td>2015</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Automatic (A1)</td>
      <td>Rear-Wheel Drive</td>
      <td>Two Seaters</td>
      <td>Electricity</td>
      <td>107</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>38435</th>
      <td>smart</td>
      <td>fortwo electric drive coupe</td>
      <td>2016</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Automatic (A1)</td>
      <td>Rear-Wheel Drive</td>
      <td>Two Seaters</td>
      <td>Electricity</td>
      <td>107</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>38436 rows × 11 columns</p>
</div>
```
:::
:::

::: {#3404c70b-2573-4c1b-96cd-22489e1eb770 .cell .code execution_count="6"}
``` python
vehiculos.dtypes
```

::: {.output .execute_result execution_count="6"}
    fabricante         object
    modelo             object
    year                int64
    desplazamiento    float64
    cilindros         float64
    transmision        object
    traccion           object
    clase              object
    combustible        object
    consumo             int64
    co2               float64
    dtype: object
:::
:::

::: {#f59c2f93-b65f-4987-ace7-da686ceee9a8 .cell .markdown}
#### Descipcion de Entidades

fabricante frabricante+modelo fabricante+modelo+año fabricante+año
:::

::: {#44a6bdab-fc6d-4b06-b9cf-6c225ae79091 .cell .code execution_count="7"}
``` python
vehiculos.to_csv("../Analisis_Exploratorio_Datos/vehiculos.1.procesado_inicial.csv", index = False)
```
:::

::: {#d3f63b46-d847-4d8a-9449-7a310e5f91b5 .cell .code execution_count="8"}
``` python
vehiculos = pd.read_csv("../Analisis_Exploratorio_Datos/vehiculos.1.procesado_inicial.csv")
```
:::

::: {#2b5fbf1c-385a-4e29-b0f2-4e0424b8389f .cell .code execution_count="9"}
``` python
vehiculos.shape
```

::: {.output .execute_result execution_count="9"}
    (38436, 11)
:::
:::

::: {#072a657d-497b-4a55-8522-1a47674a9d3c .cell .markdown}
#### Duplicados
:::

::: {#83d62c4c-b5f6-400e-ba1f-cc0f0d1114c4 .cell .code execution_count="10"}
``` python
vehiculos['modelo_unico'] = vehiculos.fabricante.str.cat([vehiculos.modelo,vehiculos.year.apply(str)], sep='-')
```
:::

::: {#c107f8da-9725-4f15-bdc5-5949c0a7d17b .cell .code execution_count="11"}
``` python
vehiculos.modelo_unico.value_counts()
```

::: {.output .execute_result execution_count="11"}
    Jeep-Cherokee/Wagoneer-1985                24
    Ford-F150 Pickup 2WD-1984                  19
    Chevrolet-C10 Pickup 2WD-1984              19
    GMC-C15 Pickup 2WD-1984                    19
    Chevrolet-S10 Pickup 2WD-1984              18
                                               ..
    Mercedes-Benz-S550-2010                     1
    Porsche-Cayenne GTS-2014                    1
    Volvo-XC90 AWD-2013                         1
    Mercedes-Benz-ML350 Bluetec 4matic-2014     1
    Mercedes-Benz-SL600-1994                    1
    Name: modelo_unico, Length: 17448, dtype: int64
:::
:::

::: {#6eaa3358-ec10-4d40-865e-f2b8039e30c3 .cell .code execution_count="12"}
``` python
vehiculos[vehiculos.modelo_unico=='Chevrolet-S10 Pickup 2WD-1984'].head()
```

::: {.output .execute_result execution_count="12"}
```{=html}
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
      <th>fabricante</th>
      <th>modelo</th>
      <th>year</th>
      <th>desplazamiento</th>
      <th>cilindros</th>
      <th>transmision</th>
      <th>traccion</th>
      <th>clase</th>
      <th>combustible</th>
      <th>consumo</th>
      <th>co2</th>
      <th>modelo_unico</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7243</th>
      <td>Chevrolet</td>
      <td>S10 Pickup 2WD</td>
      <td>1984</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>Manual 4-spd</td>
      <td>2-Wheel Drive</td>
      <td>Small Pickup Trucks 2WD</td>
      <td>Regular</td>
      <td>24</td>
      <td>370.291667</td>
      <td>Chevrolet-S10 Pickup 2WD-1984</td>
    </tr>
    <tr>
      <th>7244</th>
      <td>Chevrolet</td>
      <td>S10 Pickup 2WD</td>
      <td>1984</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>Manual 5-spd</td>
      <td>2-Wheel Drive</td>
      <td>Small Pickup Trucks 2WD</td>
      <td>Regular</td>
      <td>26</td>
      <td>341.807692</td>
      <td>Chevrolet-S10 Pickup 2WD-1984</td>
    </tr>
    <tr>
      <th>7245</th>
      <td>Chevrolet</td>
      <td>S10 Pickup 2WD</td>
      <td>1984</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>Automatic 4-spd</td>
      <td>2-Wheel Drive</td>
      <td>Small Pickup Trucks 2WD</td>
      <td>Regular</td>
      <td>20</td>
      <td>444.350000</td>
      <td>Chevrolet-S10 Pickup 2WD-1984</td>
    </tr>
    <tr>
      <th>7246</th>
      <td>Chevrolet</td>
      <td>S10 Pickup 2WD</td>
      <td>1984</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>Manual 4-spd</td>
      <td>2-Wheel Drive</td>
      <td>Small Pickup Trucks 2WD</td>
      <td>Regular</td>
      <td>21</td>
      <td>423.190476</td>
      <td>Chevrolet-S10 Pickup 2WD-1984</td>
    </tr>
    <tr>
      <th>7247</th>
      <td>Chevrolet</td>
      <td>S10 Pickup 2WD</td>
      <td>1984</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>Manual 5-spd</td>
      <td>2-Wheel Drive</td>
      <td>Small Pickup Trucks 2WD</td>
      <td>Regular</td>
      <td>22</td>
      <td>403.954545</td>
      <td>Chevrolet-S10 Pickup 2WD-1984</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#6e520161-b12a-4db2-8e32-d41dd2087a4a .cell .code execution_count="14"}
``` python
vehiculos[vehiculos.duplicated()].shape
```

::: {.output .execute_result execution_count="14"}
    (1506, 12)
:::
:::

::: {#2d2ef8b5-be74-428b-b54e-763ba154e991 .cell .code execution_count="15"}
``` python
vehiculos=vehiculos.drop_duplicates() # eliminamos valores duplicados
vehiculos.shape
```

::: {.output .execute_result execution_count="15"}
    (36930, 12)
:::
:::

::: {#22b7848b-6753-40e0-919e-0d61c281ba50 .cell .code execution_count="16"}
``` python
del vehiculos['modelo_unico'] #eliminamos columna de modelo_unico
```
:::

::: {#250edc33-acc5-4e6e-8826-2a65fbf47604 .cell .code execution_count="17"}
``` python
n_records = len(vehiculos)
def valores_duplicados_col(df):
    for columna in df:
        n_por_valor = df[columna].value_counts()
        mas_comun = n_por_valor.iloc[0]
        menos_comun = n_por_valor.iloc[-1]
        print("{} | {}-{} | {}".format(
            df[columna].name,
            round(mas_comun / (1.0*n_records),3),
            round(menos_comun / (1.0*n_records),3),
            df[columna].dtype
        ))

valores_duplicados_col(vehiculos)
```

::: {.output .stream .stdout}
    fabricante | 0.1-0.0 | object
    modelo | 0.005-0.0 | object
    year | 0.038-0.007 | int64
    desplazamiento | 0.095-0.0 | float64
    cilindros | 0.38-0.0 | float64
    transmision | 0.287-0.0 | object
    traccion | 0.353-0.005 | object
    clase | 0.145-0.0 | object
    combustible | 0.652-0.0 | object
    consumo | 0.097-0.0 | int64
    co2 | 0.084-0.0 | float64
:::
:::

::: {#6677ba3a-e7a6-4b0b-8d21-353d0af8155c .cell .code execution_count="19"}
``` python
vehiculos.transmision.value_counts(normalize=True).plot.barh();
```

::: {.output .display_data}
![](vertopal_98ecb89fd44d49d2aaa1edf34b75230c/1a3b1076d5d9d3f83086198e2d991264af8c656a.png)
:::
:::

::: {#bce8b684-a5e3-46ff-a2ea-41837ae2808d .cell .code execution_count="21"}
``` python
vehiculos.cilindros.hist();
```

::: {.output .display_data}
![](vertopal_98ecb89fd44d49d2aaa1edf34b75230c/7e3f581e9685b2496ab4935d7525e48fd345a9e3.png)
:::
:::

::: {#cf4b53cd-5c87-4904-9158-50ccb9c04aae .cell .code execution_count="22"}
``` python
vehiculos.combustible.value_counts(normalize=True).plot.barh();
```

::: {.output .display_data}
![](vertopal_98ecb89fd44d49d2aaa1edf34b75230c/f50384724f4197901f5164ad94e5558b1abd51ed.png)
:::
:::

::: {#c03bd729-b630-445f-9b51-2288a89bdeb3 .cell .code execution_count="23"}
``` python
# la columna combustible si puede tener un problema al tener el 65% de los casos de gasolina regular
```
:::

::: {#beadd49e-ca32-438e-b96d-4945ff13a908 .cell .markdown}
#### Valores Inexistentes
:::

::: {#1f61f667-37f4-40e9-94a5-1872599c0356 .cell .code execution_count="25"}
``` python
n_records = len(vehiculos)
def valores_inexistentes_col(df):
    for columna in df:
        print("{} | {} | {}".format(
            df[columna].name, len(df[df[columna].isnull()]) / (1.0*n_records), df[columna].dtype
        ))

valores_inexistentes_col(vehiculos)
```

::: {.output .stream .stdout}
    fabricante | 0.0 | object
    modelo | 0.0 | object
    year | 0.0 | int64
    desplazamiento | 0.0037909558624424585 | float64
    cilindros | 0.003845112374763065 | float64
    transmision | 0.00029786081776333605 | object
    traccion | 0.02158137015976171 | object
    clase | 0.0 | object
    combustible | 0.0 | object
    consumo | 0.0 | int64
    co2 | 0.0 | float64
:::
:::

::: {#14281a33-c803-411b-b525-ee4c2fc134ba .cell .markdown}
#Vemos que campo traccion, cilindros y transmision tienen valores
inexistentes. Sin embargo son cantidades despreciables (maximo es la
variable traccion con un 3% inexistente)
:::

::: {#4c5d2d64-23ea-4670-9ce9-f1347edb281b .cell .markdown}
#### Valores Extremos (outliers)
:::

::: {#96f6fd75-ea0c-43e1-bcff-520ee607d72e .cell .code execution_count="26"}
``` python
from scipy import stats
import numpy as np

def outliers_col(df):
    for columna in df:
        if df[columna].dtype != np.object:
            n_outliers = len(df[np.abs(stats.zscore(df[columna])) > 3])    
            print("{} | {} | {}".format(
                df[columna].name,
                n_outliers,
                df[columna].dtype
        ))

outliers_col(vehiculos)
```

::: {.output .stream .stdout}
    year | 0 | int64
    desplazamiento | 0 | float64
    cilindros | 0 | float64
    consumo | 233 | int64
    co2 | 358 | float64
:::

::: {.output .stream .stderr}
    <ipython-input-26-5e5c37a4d9d6>:6: DeprecationWarning: `np.object` is a deprecated alias for the builtin `object`. To silence this warning, use `object` by itself. Doing this will not modify any behavior and is safe. 
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      if df[columna].dtype != np.object:
:::
:::

::: {#b94bcc38-1386-45da-b6c9-fc09ba87384b .cell .code execution_count="27"}
``` python
# Vemos que las variables de consumo y co2 tienen outliers. Podemos hacer un boxplot para visualizar esto mejor
```
:::

::: {#d82e874d-50ac-4be2-bc31-7d92776c1373 .cell .code execution_count="28"}
``` python
vehiculos.boxplot(column='consumo');
```

::: {.output .display_data}
![](vertopal_98ecb89fd44d49d2aaa1edf34b75230c/961b9f3b257a6f911eef26479075db17afc47561.png)
:::
:::

::: {#0c0b6aa1-3da1-479e-83eb-56b496695bdf .cell .code execution_count="29"}
``` python
vehiculos.boxplot(column='co2');
```

::: {.output .display_data}
![](vertopal_98ecb89fd44d49d2aaa1edf34b75230c/d4cb1420636ef9adebd94160d5e20c84fd468134.png)
:::
:::

::: {#960c0a32-25c5-4637-89c2-d910fb91c689 .cell .code execution_count="30"}
``` python
vehiculos.boxplot(column='cilindros');
```

::: {.output .display_data}
![](vertopal_98ecb89fd44d49d2aaa1edf34b75230c/b1627440556a101370117a1ec5ab053a09e0c3d9.png)
:::
:::

::: {#9e684b2d-38ed-41cb-bbe2-f694a371be06 .cell .code execution_count="31"}
``` python
vehiculos[vehiculos.co2==0].combustible.unique()
```

::: {.output .execute_result execution_count="31"}
    array(['Electricity'], dtype=object)
:::
:::

::: {#a8aae11a-a29d-43b4-8978-c7900c74e16a .cell .code execution_count="32"}
``` python
vehiculos.combustible.unique()
```

::: {.output .execute_result execution_count="32"}
    array(['Regular', 'Premium', 'Diesel', 'Premium and Electricity',
           'Premium or E85', 'Electricity', 'Premium Gas or Electricity',
           'Gasoline or E85', 'Gasoline or natural gas', 'CNG',
           'Regular Gas or Electricity', 'Midgrade',
           'Regular Gas and Electricity', 'Gasoline or propane'], dtype=object)
:::
:::

::: {#c2857da9-f00f-48ed-bd14-447260ecb45e .cell .code execution_count="33"}
``` python
vehiculos_no_electricos=vehiculos[vehiculos.co2>0]
```
:::

::: {#e2c7a4da-d40e-4b0a-8f91-97156f97fc32 .cell .code execution_count="34"}
``` python
outliers_col(vehiculos_no_electricos)
```

::: {.output .stream .stdout}
    year | 0 | int64
    desplazamiento | 0 | float64
    cilindros | 0 | float64
    consumo | 400 | int64
    co2 | 221 | float64
:::

::: {.output .stream .stderr}
    <ipython-input-26-5e5c37a4d9d6>:6: DeprecationWarning: `np.object` is a deprecated alias for the builtin `object`. To silence this warning, use `object` by itself. Doing this will not modify any behavior and is safe. 
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      if df[columna].dtype != np.object:
:::
:::

::: {#e97f8db7-a0c6-42e8-8ac0-c8cfd52f7d4a .cell .code execution_count="35"}
``` python
valores_inexistentes_col(vehiculos)
```

::: {.output .stream .stdout}
    fabricante | 0.0 | object
    modelo | 0.0 | object
    year | 0.0 | int64
    desplazamiento | 0.0037909558624424585 | float64
    cilindros | 0.003845112374763065 | float64
    transmision | 0.00029786081776333605 | object
    traccion | 0.02158137015976171 | object
    clase | 0.0 | object
    combustible | 0.0 | object
    consumo | 0.0 | int64
    co2 | 0.0 | float64
:::
:::

::: {#2e597fdc-df9a-4ac5-be38-cbe99ca18801 .cell .code execution_count="37"}
``` python
vehiculos_no_electricos.to_csv('../Analisis_Exploratorio_Datos/vehiculos.2.limpio_analisis.csv',index=False)
```
:::

::: {#dd5319c2-dff0-4012-a9d6-b5c81618de7e .cell .code}
``` python
```
:::
