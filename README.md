
# Tokens analysis

Simple example of how to use Python to get data from the Ethereum blockchain.

## Files
* **tokens.ipynb**: Jupyter notebook with the code used (Python)
* **tokens.csv** (input file): List of tokens to analyze
* **./datasets/balances.csv** (output file): Matrix with the balance of tokens of different addresses
* **./datasets/{TOKEN_NAME}.csv** (output file): Raw data of user transactions with the token

## Code
```python
from urllib2 import Request, urlopen, URLError
import pandas as pd
import numpy as np
import json
```

### Get data from Etherscan


```python
# Write your Etherscan API key here
APIKEY = ""

# Make sure you have this file in the same directory
tokens = pd.read_csv('tokens.csv', index_col=False, header=0)
```


```python
tokenTxs = {}

def getTxs(token):
    try:
        # Will look only for normal txs
        request = Request("http://api.etherscan.io/api?module=account&action=txlist&address={}&apikey={}".format(token["Address"], APIKEY))
        response = urlopen(request)
        txs = response.read()
        tokenTxs[token["Project"]] = pd.read_json(json.dumps(json.loads(txs)["result"]), orient='records')
    except URLError, e:
        print 'API error. Got an error code:', e

tokens.apply(getTxs, axis=1)

# Sample data
tokenTxs["Acade City"].head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>blockHash</th>
      <th>blockNumber</th>
      <th>confirmations</th>
      <th>contractAddress</th>
      <th>cumulativeGasUsed</th>
      <th>from</th>
      <th>gas</th>
      <th>gasPrice</th>
      <th>gasUsed</th>
      <th>hash</th>
      <th>input</th>
      <th>isError</th>
      <th>nonce</th>
      <th>timeStamp</th>
      <th>to</th>
      <th>transactionIndex</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0x5fd4ccfbc4fd08790bec564d3941bdd39a59fc242fc7...</td>
      <td>2541610</td>
      <td>798691</td>
      <td></td>
      <td>419262</td>
      <td>0x5f23acdd1e87112b5fe143509d74ded22b6e59b3</td>
      <td>400000</td>
      <td>21800903077</td>
      <td>87290</td>
      <td>0x92089c7ccbcb0e190e4d87e3158c0572e066f22ab53a...</td>
      <td>0x7d124a0200000000000000000000000008e50ae3e83f...</td>
      <td>0</td>
      <td>15</td>
      <td>2016-10-31 14:38:13</td>
      <td>0xac709fcb44a43c35f0da4e3163b117a17f3770f5</td>
      <td>1</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0xacf96f6ef0fcd8878ed90705f3b35a01d36f41a73ffb...</td>
      <td>2541693</td>
      <td>798608</td>
      <td></td>
      <td>205240</td>
      <td>0xb6f2af0b3551161fe95f18219a8d402fc4e0233b</td>
      <td>100000</td>
      <td>21000000000</td>
      <td>100000</td>
      <td>0xf64a321a017ddf84a0daa9aecdf3353c0a5b1369822f...</td>
      <td>0x</td>
      <td>1</td>
      <td>27</td>
      <td>2016-10-31 14:58:24</td>
      <td>0xac709fcb44a43c35f0da4e3163b117a17f3770f5</td>
      <td>5</td>
      <td>1.000000e+17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0x12aa0744a148380816466e7a8b7f7302bcd3971a998c...</td>
      <td>2541866</td>
      <td>798435</td>
      <td></td>
      <td>105000</td>
      <td>0x5d61433e4dbd2e6a44c62846a7ef3a1d4cd256b3</td>
      <td>21000</td>
      <td>21000000000</td>
      <td>21000</td>
      <td>0xdcdbed18849b1b7603c0fa5075a037cf48971ac9ce1c...</td>
      <td>0x</td>
      <td>1</td>
      <td>0</td>
      <td>2016-10-31 15:43:16</td>
      <td>0xac709fcb44a43c35f0da4e3163b117a17f3770f5</td>
      <td>4</td>
      <td>1.000000e+12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0x9ea42e81a52662618b847314d253bbb3fc7bef219f0d...</td>
      <td>2542668</td>
      <td>797633</td>
      <td></td>
      <td>121000</td>
      <td>0xcecafbdbbb5d5baf57844a6611e36fc781aad017</td>
      <td>100000</td>
      <td>21000000000</td>
      <td>100000</td>
      <td>0x934743bcc99b9e211ff607407740e5eebd3255283a33...</td>
      <td>0x</td>
      <td>1</td>
      <td>0</td>
      <td>2016-10-31 18:52:16</td>
      <td>0xac709fcb44a43c35f0da4e3163b117a17f3770f5</td>
      <td>1</td>
      <td>6.000000e+18</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0x6d29e46ae11e4c5cef7f0ea2580993fd6451ce5a5ec7...</td>
      <td>2543686</td>
      <td>796615</td>
      <td></td>
      <td>358785</td>
      <td>0x6cfabd40891abe610efd0cc0cfb8a2f2209ea68d</td>
      <td>100000</td>
      <td>26000000000</td>
      <td>100000</td>
      <td>0xb369fac72065e37a74712f8c4375f5d1deea9b735c01...</td>
      <td>0x</td>
      <td>1</td>
      <td>0</td>
      <td>2016-10-31 22:58:32</td>
      <td>0xac709fcb44a43c35f0da4e3163b117a17f3770f5</td>
      <td>10</td>
      <td>4.000000e+18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove duplicates
uniqueAddresses = np.array([])

for txs in tokenTxs.itervalues():
    uniqueAddresses = np.concatenate([uniqueAddresses, np.unique(txs["from"])])

print "Number of user accounts (with duplicates): {}".format(len(uniqueAddresses))
uniqueAddresses = np.unique(uniqueAddresses)
print "Number of user accounts (after removing duplicates): {}".format(len(uniqueAddresses))
```

    Number of user accounts (with duplicates): 17362
    Number of user accounts (after removing duplicates): 15286


### Get data from local node


```python
# Uncomment to install web3
#!pip install web3
```


```python
from web3 import Web3, KeepAliveRPCProvider, IPCProvider

# Note that you should create only one RPCProvider per
# process, as it recycles underlying TCP/IP network connections between
# your process and Ethereum node
#web3 = Web3(KeepAliveRPCProvider(host='localhost', port='8545'))

# or for an IPC based connection
web3 = Web3(IPCProvider())
```


```python
# ERC20 standar
abi = json.loads('[{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_value","type":"uint256"}],"name":"approve","outputs":[{"name":"success","type":"bool"}],"payable":false,"type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"_from","type":"address"},{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"success","type":"bool"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"balanceOf","outputs":[{"name":"balance","type":"uint256"}],"payable":false,"type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transfer","outputs":[{"name":"success","type":"bool"}],"payable":false,"type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"},{"name":"_spender","type":"address"}],"name":"allowance","outputs":[{"name":"remaining","type":"uint256"}],"payable":false,"type":"function"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_from","type":"address"},{"indexed":true,"name":"_to","type":"address"},{"indexed":false,"name":"_value","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"_owner","type":"address"},{"indexed":true,"name":"_spender","type":"address"},{"indexed":false,"name":"_value","type":"uint256"}],"name":"Approval","type":"event"}]')

balances = pd.DataFrame("", index=uniqueAddresses, columns=tokens["Project"].values)

def setBalances(token):
    tokenContract = web3.eth.contract(
        abi = abi,
        address = token["Address"]
    )

    for address in uniqueAddresses:
        try:
            balances.set_value(address, token["Project"], tokenContract.call().balanceOf(address))
        except URLError, e:
            print 'Web3 error. Got an error code:', e

tokens.apply(setBalances, axis=1)

# Sample data
balances.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Augur</th>
      <th>ICONOMI</th>
      <th>Golem</th>
      <th>Digix</th>
      <th>Pluton (Plutus)</th>
      <th>SingularDTV</th>
      <th>First Blood</th>
      <th>VSice (VDlice)</th>
      <th>Hacker Gold (Ether.camp)</th>
      <th>Maker DAO</th>
      <th>Chrono Bank</th>
      <th>Unicorns (Ethereum Fundation)</th>
      <th>Xaurum</th>
      <th>Acade City</th>
      <th>Swarm City</th>
      <th>Bitpark Coin</th>
      <th>Round</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0x00004aba4ac63de11447e4e17aca83f0abb1fc33</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0x000118f3bd5a727f663c85c671370760c7730927</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0x0001aebe0b48bbf1cee8df2c0dfd7c2031543859</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0x0001fe7648a2c144becdf9f17f0055315a519f86</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>134349877</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0x000313efbb302549f83e35e50bf0a4e3f0a639af</th>
      <td>0</td>
      <td>0</td>
      <td>2262127659574400000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Export


```python
# You need to have a ./dataset directory
# Raw data
for project, txs in tokenTxs.items():
    url = "./datasets/{}.csv".format(project)
    txs.to_csv(url, sep=',')

# Balances
balances.to_csv("./datasets/balances.csv", sep=',')
```
# R Data Analysis
El objetivo es estructurar la información extraída de la Blockchain y analizarla mejor.
La información a tratar procede de varios Tokens desarrollados en Ethereum y vamos a analizar cómo se relacionan en función de las cuentas que operan con ellas.

Primeramente, importamos los datos de los ficheros:
```
mydataaugur<-read.csv("/datasets/Augur.csv",header=TRUE)
mydatadigix<-read.csv("/datasets/Digix.csv",header=TRUE)
mydatagolem<-read.csv("/datasets/Golem.csv",header=TRUE)
```
Añadimos un nuevo campo a los datos de los ficheros con el nombre del Token
```
mydataaugur$token<-"Augur"
mydatadigix$token<-"Digix"
mydatagolem$token<-"Golem" 
```
Juntamos en un único data frame los datos de transacciones de tres tokens
```
mydata<-rbind(mydataaugur,mydatadigix,mydatagolem)
```
Damos formato al campo fecha de la transacción
```
mydata$ts<-as.POSIXct(as.character(mydata$timeStamp),origin="1970-01-01",tz="GMT") 
```
Añadimos un número secuencial para cada cuenta de origen en orden temporal
```
mydata2<-cbind(mydata, rank=ave(as.numeric(mydata$ts), mydata$from, FUN=rank))
```
Exportamos los datos a un fichero csv
```
write.csv(mydata2, file="../mydatatokens.csv")
```
Ponemos los valores del campo token como columnas cuyo valor será el número de transacciones por cuenta de origen
```
library(reshape2)
mydata3<-dcast(mydata2, from ~ token)

write.csv(mydata3, file="../mygraphtokens.csv")
```
Calculamos la correlación entre los tokens en función las cuentas que operan en ellas
```
> cor(mydata3)
```

      |   from    |   Augur   |   Digix  |    Golem    |
      --------------------------------------------------
from  |1.00000000 |0.012476091|0.02059404|-0.015118285 |
Augur  0.01247609  1.000000000 0.10363220 -0.002783285
Digix  0.02059404  0.103632200 1.00000000  0.031423973
Golem -0.01511829 -0.002783285 0.03142397  1.000000000


En el resultado se puede ver que no hay correlación lineal en la operativa de los Tokens al estar los valores cercanos a cero. El hecho de que alguien opere con un Token no indica que opere o no en otro Token.

Mejoramos el formato de la tabla anterior para reducirlo al cruce de Tokens.
```
mydata4<-cor(cbind(mydata3$Golem,mydata3$Digix,mydata3$Augur))
rownames(mydata4)<-c("Golem","Digix","Augur")
colnames(mydata4)<-c("Golem","Digix","Augur")

>mydata4
```
``
          Golem      Digix        Augur
Golem  1.000000000 0.03142397 -0.002783285
Digix  0.031423973 1.00000000  0.103632200
Augur -0.002783285 0.10363220  1.000000000
``
Pintamos un grafo de correlación entre Tokens (ver fichero cor.jpg)
```
library(qgraph)
Q<-qgraph(cor(mydata3))> 
```
