```python
%load_ext watermark
%watermark
```

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
    
    


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.rcParams["figure.figsize"] = (10,6)
```


```python
url="https://raw.githubusercontent.com/manugarri/curso_data_science/master/Secciones/Seccion4.Analisis_y_procesado_de_datos/Procesado_de_Datos/data/vehiculos_original.csv"
vehiculos = pd.read_csv(url)
vehiculos.head()
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




```python
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


```python
vehiculos
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




```python
vehiculos.dtypes
```




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



#### Descipcion de Entidades

fabricante
frabricante+modelo
fabricante+modelo+año
fabricante+año


```python
vehiculos.to_csv("../Analisis_Exploratorio_Datos/vehiculos.1.procesado_inicial.csv", index = False)
```


```python
vehiculos = pd.read_csv("../Analisis_Exploratorio_Datos/vehiculos.1.procesado_inicial.csv")
```


```python
vehiculos.shape
```




    (38436, 11)



#### Duplicados


```python
vehiculos['modelo_unico'] = vehiculos.fabricante.str.cat([vehiculos.modelo,vehiculos.year.apply(str)], sep='-')
```


```python
vehiculos.modelo_unico.value_counts()
```




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




```python
vehiculos[vehiculos.modelo_unico=='Chevrolet-S10 Pickup 2WD-1984'].head()
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




```python
vehiculos[vehiculos.duplicated()].shape
```




    (1506, 12)




```python
vehiculos=vehiculos.drop_duplicates() # eliminamos valores duplicados
vehiculos.shape
```




    (36930, 12)




```python
del vehiculos['modelo_unico'] #eliminamos columna de modelo_unico
```


```python
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
    


```python
vehiculos.transmision.value_counts(normalize=True).plot.barh();
```


    
![png](output_18_0.png)
    



```python
vehiculos.cilindros.hist();
```


    
![png](output_19_0.png)
    



```python
vehiculos.combustible.value_counts(normalize=True).plot.barh();
```


    
![png](output_20_0.png)
    



```python
# la columna combustible si puede tener un problema al tener el 65% de los casos de gasolina regular
```

#### Valores Inexistentes


```python
n_records = len(vehiculos)
def valores_inexistentes_col(df):
    for columna in df:
        print("{} | {} | {}".format(
            df[columna].name, len(df[df[columna].isnull()]) / (1.0*n_records), df[columna].dtype
        ))

valores_inexistentes_col(vehiculos)

```

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
    

#Vemos que campo traccion, cilindros y transmision tienen valores inexistentes. Sin embargo son cantidades despreciables (maximo es la variable traccion con un 3% inexistente)

#### Valores Extremos (outliers)


```python
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

    year | 0 | int64
    desplazamiento | 0 | float64
    cilindros | 0 | float64
    consumo | 233 | int64
    co2 | 358 | float64
    

    <ipython-input-26-5e5c37a4d9d6>:6: DeprecationWarning: `np.object` is a deprecated alias for the builtin `object`. To silence this warning, use `object` by itself. Doing this will not modify any behavior and is safe. 
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      if df[columna].dtype != np.object:
    


```python
# Vemos que las variables de consumo y co2 tienen outliers. Podemos hacer un boxplot para visualizar esto mejor
```


```python
vehiculos.boxplot(column='consumo');
```


    
![png](output_28_0.png)
    



```python
vehiculos.boxplot(column='co2');
```


    
![png](output_29_0.png)
    



```python
vehiculos.boxplot(column='cilindros');
```


    
![png](output_30_0.png)
    



```python
vehiculos[vehiculos.co2==0].combustible.unique()
```




    array(['Electricity'], dtype=object)




```python
vehiculos.combustible.unique()
```




    array(['Regular', 'Premium', 'Diesel', 'Premium and Electricity',
           'Premium or E85', 'Electricity', 'Premium Gas or Electricity',
           'Gasoline or E85', 'Gasoline or natural gas', 'CNG',
           'Regular Gas or Electricity', 'Midgrade',
           'Regular Gas and Electricity', 'Gasoline or propane'], dtype=object)




```python
vehiculos_no_electricos=vehiculos[vehiculos.co2>0]
```


```python
outliers_col(vehiculos_no_electricos)
```

    year | 0 | int64
    desplazamiento | 0 | float64
    cilindros | 0 | float64
    consumo | 400 | int64
    co2 | 221 | float64
    

    <ipython-input-26-5e5c37a4d9d6>:6: DeprecationWarning: `np.object` is a deprecated alias for the builtin `object`. To silence this warning, use `object` by itself. Doing this will not modify any behavior and is safe. 
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      if df[columna].dtype != np.object:
    


```python
valores_inexistentes_col(vehiculos)
```

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
    


```python
vehiculos_no_electricos.to_csv('../Analisis_Exploratorio_Datos/vehiculos.2.limpio_analisis.csv',index=False)
```


```python

```
