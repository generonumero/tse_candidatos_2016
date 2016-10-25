
# A doação de partidos para seus cadidatos

Baixamos a [prestação de contas dos candidatos em 2016](http://www.tse.jus.br/hotSites/pesquisas-eleitorais/prestacao_contas_anos/2016.html) e seguimos com a seguinte pergunta: quanto os partidos políticos doaram para seus candidatos? É possível identificar uma diferença entre os valores doado para homens e para mulheres? Quais são os partidos que mais se destacam e porquê? Quais foram os candidados que mais se destacaram e porquê? Essas são algumas das perguntas que a nossa anaálise procurou responder.

## Sexo invisível
A base de prestação de contas dos candidatos não possui a coluna "Sexo". Ou seja, apenas com essa base não é possível fazer as análises. Contudo, a base possui o nome e o número sequencial dos candidatos, algo que usaremos para importar os dados de sexo da base de candidaturas.

## Python: Pandas
Escolhemos o Pandas para fazer essa análise. Trata-se de um dos melhores módulos Python para análise de dados. O Pandas permite manipular os arquivos do Tribunal Superior Eleitoral com facilidade. Como são muitos arquivos e muito grandes, não é possível abri-los em ferramentas mais convencionais, como o Libre Office Calc.

Vamos começar, então, importando o Pandas para o nosso script. Se você não tem o Pandas instalado, basta digitar "pip install Pandas" no seu terminal.


```python
# -*- coding: utf-8
import pandas as pd
```

Agora vamos usar a função "read_csv" do Pandas para abrir o arquivo "receitas_candidatos_2016_brasil.txt". Esse arquivo possui o consolidado da prestação de contas de todos os candidatos nas eleições de 2016. É um arquivo de quase 1Gb. Temos alguns parâmetros:

- delimiter: ";" (os arquivos brasileiros possuem o delimitador ";" em vez da ",")
- decimal: "," (preciamos informar que no formato numérico brasileiro a casa decimal é separada por vírgula)
- encoding: "latin1" (o padrão brasileiro de encoding de caracteres usado pelo governo equivale ao "latin1" em Python)
- low_memory=False (o Pandas tenta adivinhar qual é o tipo de dado em cada coluna para economizar memória - número, texto etc - como uma das colunas possui mais de um tipo de dado, precisamos dizer ao Pandas que ele vai precisar usar mais memória)


```python
base = pd.read_csv(
    'data/receitas_candidatos_2016_brasil.txt', 
    delimiter=";", 
    decimal=",", 
    encoding="latin1", 
    low_memory=False)
```

Guardamos o arquivo na variável base e agora vamos exibir as 30 primeiras e 30 últimas linhas.


```python
base

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Cód. Eleição</th>
      <th>Desc. Eleição</th>
      <th>Data e hora</th>
      <th>CNPJ Prestador Conta</th>
      <th>Sequencial Candidato</th>
      <th>UF</th>
      <th>Sigla da UE</th>
      <th>Nome da UE</th>
      <th>Sigla  Partido</th>
      <th>Numero candidato</th>
      <th>...</th>
      <th>Valor receita</th>
      <th>Tipo receita</th>
      <th>Fonte recurso</th>
      <th>Especie recurso</th>
      <th>Descricao da receita</th>
      <th>CPF/CNPJ do doador originário</th>
      <th>Nome do doador originário</th>
      <th>Tipo doador originário</th>
      <th>Setor econômico do doador originário</th>
      <th>Nome do doador originário (Receita Federal)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>2</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>3</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>4</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>5</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>6</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>5000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>7</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>8</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>9</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>2000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>10</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25552898000131</td>
      <td>160000007179</td>
      <td>PR</td>
      <td>75353</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>12000</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>11</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>5000.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>12</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>2000.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>DOACAO ESTIMAVEL COMITÊ - RUA PROFESSORA CÉLIA...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>13</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>2800.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>14</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>3000.0</td>
      <td>Recursos de partido político</td>
      <td>Fundo Partidario</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>15</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>10000.0</td>
      <td>Recursos de partido político</td>
      <td>Fundo Partidario</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>16</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>540.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>BANDEIRA PARA CAMPANHA</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>17</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>SERVIÇOS ADMINISTRATIVOS</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>18</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>SERVIÇOS ADMINISTRATIVOS</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>19</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>200.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>GRAVAÇÃO DE AUDIOS PARA RADIOS</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>20</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>100.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>GRAVAÇÃO DE AUDIO PARA DIVULGACAO EM CARRO DE ...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>21</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>300.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>PRODUÇÃO DE JINGLES</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>22</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>800.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>PUBLICIDADE POR CARRO DE SOM</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>23</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>24</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25490411000133</td>
      <td>250000029177</td>
      <td>SP</td>
      <td>62391</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>15</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>25</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25746582000180</td>
      <td>250000072494</td>
      <td>SP</td>
      <td>62278</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>45111</td>
      <td>...</td>
      <td>300.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>26</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25746582000180</td>
      <td>250000072494</td>
      <td>SP</td>
      <td>62278</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>45111</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>PAS/AUTOMOVEL GM/CORSA WIND 2000/2000 - BRANCA...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>27</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25746582000180</td>
      <td>250000072494</td>
      <td>SP</td>
      <td>62278</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>45111</td>
      <td>...</td>
      <td>30.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>28</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25746582000180</td>
      <td>250000072494</td>
      <td>SP</td>
      <td>62278</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>45111</td>
      <td>...</td>
      <td>215.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>29</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25746582000180</td>
      <td>250000072494</td>
      <td>SP</td>
      <td>62278</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>45111</td>
      <td>...</td>
      <td>90.0</td>
      <td>Recursos de outros candidatos</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>5000 SANTINHOS 7X0 CM - PREFEITO / VICE VEREAD...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
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
      <th>1867360</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867361</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867362</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867363</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>3000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>CESSÃO DE SERVIÇOS JURIDICOS PERÍODO 16/08/201...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867364</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867365</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867366</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867367</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867368</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867369</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867370</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867371</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867372</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>1500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>CESSÃO DE CARRO DE SOM PERÍODO 16/08/2016 À 01...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867373</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25379727000152</td>
      <td>210000004025</td>
      <td>RS</td>
      <td>85898</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>13777</td>
      <td>...</td>
      <td>10000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867374</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>26064156000120</td>
      <td>260000006834</td>
      <td>SE</td>
      <td>31070</td>
      <td>ARAUÁ</td>
      <td>PROS</td>
      <td>90123</td>
      <td>...</td>
      <td>450.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867375</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>26064156000120</td>
      <td>260000006834</td>
      <td>SE</td>
      <td>31070</td>
      <td>ARAUÁ</td>
      <td>PROS</td>
      <td>90123</td>
      <td>...</td>
      <td>200.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>PRESTAÇÃO SERVIÇOS GRATUITA DE TÉCNICOS DE ASS...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867376</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25359813000101</td>
      <td>130000003834</td>
      <td>MG</td>
      <td>42323</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>43666</td>
      <td>...</td>
      <td>4950.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Transferência eletrônica</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867377</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25359813000101</td>
      <td>130000003834</td>
      <td>MG</td>
      <td>42323</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>43666</td>
      <td>...</td>
      <td>1000.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>005/2016 CONTRATO DE CESSÃO GRATUITA DE VEÍCUL...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867378</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25359813000101</td>
      <td>130000003834</td>
      <td>MG</td>
      <td>42323</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>43666</td>
      <td>...</td>
      <td>100.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>003/2016 CONTRATO DE PRESTAÇÃO GRATUITA DE SER...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867379</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25359813000101</td>
      <td>130000003834</td>
      <td>MG</td>
      <td>42323</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>43666</td>
      <td>...</td>
      <td>100.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>002/2016 CONTRATO DE PRESTAÇÃO GRATUITA DE SER...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867380</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25359813000101</td>
      <td>130000003834</td>
      <td>MG</td>
      <td>42323</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>43666</td>
      <td>...</td>
      <td>200.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>001/2016 CONTRATO DE PRESTAÇÃO GRATUITA DE SER...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867381</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>320.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867382</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>800.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>CESSÃO DE MAO DE OBRA (MOTORISTA DIARIA)</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867383</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>650.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867384</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>883.8</td>
      <td>Recursos de pessoas físicas</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>VEICULO UTILIZADO NO PERIODO DA CAMPANHA ELEIT...</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867385</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>70.0</td>
      <td>Recursos de partido político</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>SERVICO DE CONTABILIDADE</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867386</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>70.0</td>
      <td>Recursos de partido político</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>SERVIÇO DE CONTABILIDADE</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867387</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25536205000117</td>
      <td>170000009990</td>
      <td>PE</td>
      <td>24210</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>25555</td>
      <td>...</td>
      <td>50.0</td>
      <td>Recursos de partido político</td>
      <td>Outros Recursos</td>
      <td>Estimado</td>
      <td>SANTINHOS</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867388</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25652723000104</td>
      <td>160000008858</td>
      <td>PR</td>
      <td>74870</td>
      <td>CAPANEMA</td>
      <td>PMDB</td>
      <td>15123</td>
      <td>...</td>
      <td>500.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
    <tr>
      <th>1867389</th>
      <td>220</td>
      <td>Eleições Municipais 2016</td>
      <td>24/10/201621:30:15</td>
      <td>25652723000104</td>
      <td>160000008858</td>
      <td>PR</td>
      <td>74870</td>
      <td>CAPANEMA</td>
      <td>PMDB</td>
      <td>15123</td>
      <td>...</td>
      <td>150.0</td>
      <td>Recursos próprios</td>
      <td>Outros Recursos</td>
      <td>Depósito em espécie</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
      <td>#NULO</td>
    </tr>
  </tbody>
</table>
<p>1867390 rows × 35 columns</p>
</div>



Essa tabela tem muitas colunas que não precisamos usar na nossa análise. Vamos recuperar os nomes das colunas e depois vamos fazer um recorte para gerar uma base apenas com as colunas que queremos. Para recuperar as colunas de uma base, digite:


```python
base.columns
```




    Index([u'Cód. Eleição', u'Desc. Eleição', u'Data e hora',
           u'CNPJ Prestador Conta', u'Sequencial Candidato', u'UF', u'Sigla da UE',
           u'Nome da UE', u'Sigla  Partido', u'Numero candidato', u'Cargo',
           u'Nome candidato', u'CPF do candidato', u'CPF do vice/suplente',
           u'Numero Recibo Eleitoral', u'Numero do documento',
           u'CPF/CNPJ do doador', u'Nome do doador',
           u'Nome do doador (Receita Federal)', u'Sigla UE doador',
           u'Número partido doador', u'Número candidato doador',
           u'Cod setor econômico do doador', u'Setor econômico do doador',
           u'Data da receita', u'Valor receita', u'Tipo receita', u'Fonte recurso',
           u'Especie recurso', u'Descricao da receita',
           u'CPF/CNPJ do doador originário', u'Nome do doador originário',
           u'Tipo doador originário', u'Setor econômico do doador originário',
           u'Nome do doador originário (Receita Federal)'],
          dtype='object')



## Fatiando a base

Agora, vamos criar uma nova variável chamada "base_cortada" e vamos criar um novo DataFrame - o nome que o Pandas dá para as bases de dados que você manipula por meio do módulo - com as seguintes colunas:

- Sequencial Candidato (essa é a chave que vai nos ajudar a buscar os dados de gênero depois)
- Nome Candidato (nome para referência mais rápida)
- UF (estado do candidato)
- Nome da UE (cidade)
- Sigla Partido
- Cargo (Prefeito ou Vereador)
- Nome do doador (nome do diretório ou comissão que doou)
- Número partido doador (número da sigla registrado junto ao TSE)
- Número candidato doador (número do candidato doador, se aplicável)
- Valor receita (valor da doação)
- Tipo de receita (categoria que nos ajuda a identificar se a doação foi de pessoa física ou de um partido, por exemplo)
- Fonte receita (se aplicável)
- Especie recurso (depósito em espécie? transferência eletrônica? etc)


```python
base_cortada = pd.DataFrame(base, columns=[
                    u"Sequencial Candidato", 
                    u"Nome Candidato", 
                    u"UF", 
                    u"Nome da UE", 
                    u"Sigla  Partido",
                    u"Cargo",
                    u"Nome do doador",
                    u"Número partido doador", 
                    u"Número candidato doador", 
                    u"Valor receita", 
                    u"Tipo receita", 
                    u"Fonte receita", 
                    u"Especie recurso"
                ])
```

Vamos ver como ficou a nossa nova base cortada:


```python
base_cortada
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Sequencial Candidato</th>
      <th>Nome Candidato</th>
      <th>UF</th>
      <th>Nome da UE</th>
      <th>Sigla  Partido</th>
      <th>Cargo</th>
      <th>Nome do doador</th>
      <th>Número partido doador</th>
      <th>Número candidato doador</th>
      <th>Valor receita</th>
      <th>Tipo receita</th>
      <th>Fonte receita</th>
      <th>Especie recurso</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>LUIZ HENRIQUE TABORDA DOMINGOS</td>
      <td>12</td>
      <td>12000</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>ALESSANDRA EDUARDA PORNER BROERING</td>
      <td>12</td>
      <td>12000</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>2</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>WLADIMIR MAZZOLLA MORAIS</td>
      <td>12</td>
      <td>12000</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>3</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>RICARDO PERCEQUILO</td>
      <td>12</td>
      <td>12000</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>4</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>IVO ORLANDO PETRIS</td>
      <td>12</td>
      <td>12000</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>5</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>LUIZ CELSO BRANCO</td>
      <td>12</td>
      <td>12000</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>6</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>OSVALDIR BENATO</td>
      <td>12</td>
      <td>12000</td>
      <td>5000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>7</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>GIOVANE MACHADO</td>
      <td>12</td>
      <td>12000</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>8</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>GIOVANE MACHADO</td>
      <td>12</td>
      <td>12000</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>9</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>GIOVANE MACHADO</td>
      <td>12</td>
      <td>12000</td>
      <td>2000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>10</th>
      <td>160000007179</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CURITIBA</td>
      <td>PDT</td>
      <td>Vereador</td>
      <td>GIOVANE MACHADO</td>
      <td>12</td>
      <td>12000</td>
      <td>500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>11</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>JOSE APARECIDO CRISTO</td>
      <td>15</td>
      <td>15</td>
      <td>5000.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>12</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>JOSE APARECIDO CRISTO</td>
      <td>15</td>
      <td>15</td>
      <td>2000.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>13</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>JOSE APARECIDO CRISTO</td>
      <td>15</td>
      <td>15</td>
      <td>2800.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>14</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>Direção Municipal/Comissão Provisória</td>
      <td>15</td>
      <td>15</td>
      <td>3000.0</td>
      <td>Recursos de partido político</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>15</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>Direção Municipal/Comissão Provisória</td>
      <td>15</td>
      <td>15</td>
      <td>10000.0</td>
      <td>Recursos de partido político</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>16</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>MONICA CRISTINA VIANNA DA SILVA</td>
      <td>15</td>
      <td>15</td>
      <td>540.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>17</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>LUCAS DE CAMARGO</td>
      <td>15</td>
      <td>15</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>18</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>LUCIANA PROVAZI DAS NEVES</td>
      <td>15</td>
      <td>15</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>19</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>PAULO HENRRIQUE PEREIRA</td>
      <td>15</td>
      <td>15</td>
      <td>200.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>20</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>PAULO HENRRIQUE PEREIRA</td>
      <td>15</td>
      <td>15</td>
      <td>100.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>21</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>LUIZ LYRIO TALLARICO JUNIOR</td>
      <td>15</td>
      <td>15</td>
      <td>300.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>22</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>LUCIANO DE LIMA E SILVA</td>
      <td>15</td>
      <td>15</td>
      <td>800.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>23</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>SILVAN RENOSTO</td>
      <td>15</td>
      <td>15</td>
      <td>1000.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>24</th>
      <td>250000029177</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BOITUVA</td>
      <td>PMDB</td>
      <td>Prefeito</td>
      <td>SILVAN RENOSTO</td>
      <td>15</td>
      <td>15</td>
      <td>500.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>25</th>
      <td>250000072494</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>Vereador</td>
      <td>OCIMAR RODRIGUES VIEIRA</td>
      <td>45</td>
      <td>45111</td>
      <td>300.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>26</th>
      <td>250000072494</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>Vereador</td>
      <td>OCIMAR RODRIGUES VIEIRA</td>
      <td>45</td>
      <td>45111</td>
      <td>1000.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>27</th>
      <td>250000072494</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>Vereador</td>
      <td>ELIANA PALMIERI</td>
      <td>45</td>
      <td>45111</td>
      <td>30.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>28</th>
      <td>250000072494</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>Vereador</td>
      <td>ELIANA PALMIERI</td>
      <td>45</td>
      <td>45111</td>
      <td>215.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>29</th>
      <td>250000072494</td>
      <td>NaN</td>
      <td>SP</td>
      <td>BILAC</td>
      <td>PSDB</td>
      <td>Vereador</td>
      <td>ELEIÇÃO 2016 VITOR OSMAR BOTINI PREFEITO</td>
      <td>45</td>
      <td>45111</td>
      <td>90.0</td>
      <td>Recursos de outros candidatos</td>
      <td>NaN</td>
      <td>Estimado</td>
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
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1867360</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>ELISE DOS SANTOS VARGAS</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867361</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>MARCIA SALETE DA SILVA</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867362</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>CARMEM HELOISA DA SILVA</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867363</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>JUCEILA LOURDES DALL AGNOL DE LACERDA</td>
      <td>13</td>
      <td>13777</td>
      <td>3000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867364</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>FABULO NASCIMENTO DA ROSA</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867365</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>FABULO NASCIMENTO DA ROSA</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867366</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>JULIO CEZAR DA SILVA SANTOS</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867367</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>JULIO CEZAR DA SILVA SANTOS</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867368</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>MATHEUS FELIPE TEIXEIRA MACHADO</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867369</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>ELIANA MACHADO NASCIMENTO</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867370</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>JERUSA FONFONKA MACHADO</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867371</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>ROGERIO DA SILVA AMBIEDA</td>
      <td>13</td>
      <td>13777</td>
      <td>1000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867372</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>LUIZ ANTONIO VIEIRA INDA</td>
      <td>13</td>
      <td>13777</td>
      <td>1500.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867373</th>
      <td>210000004025</td>
      <td>NaN</td>
      <td>RS</td>
      <td>CANOAS</td>
      <td>PT</td>
      <td>Vereador</td>
      <td>LUCIANA NOBREGA VIEIRA</td>
      <td>13</td>
      <td>13777</td>
      <td>10000.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>1867374</th>
      <td>260000006834</td>
      <td>NaN</td>
      <td>SE</td>
      <td>ARAUÁ</td>
      <td>PROS</td>
      <td>Vereador</td>
      <td>VALTER SANTOS MEDEIROS</td>
      <td>90</td>
      <td>90123</td>
      <td>450.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867375</th>
      <td>260000006834</td>
      <td>NaN</td>
      <td>SE</td>
      <td>ARAUÁ</td>
      <td>PROS</td>
      <td>Vereador</td>
      <td>JOSE VANDERLEY DA CRUZ</td>
      <td>90</td>
      <td>90123</td>
      <td>200.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867376</th>
      <td>130000003834</td>
      <td>NaN</td>
      <td>MG</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>Vereador</td>
      <td>VAGNER BENEVIDES TEMPONI</td>
      <td>43</td>
      <td>43666</td>
      <td>4950.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Transferência eletrônica</td>
    </tr>
    <tr>
      <th>1867377</th>
      <td>130000003834</td>
      <td>NaN</td>
      <td>MG</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>Vereador</td>
      <td>VAGNER BENEVIDES TEMPONI</td>
      <td>43</td>
      <td>43666</td>
      <td>1000.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867378</th>
      <td>130000003834</td>
      <td>NaN</td>
      <td>MG</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>Vereador</td>
      <td>NILSON PEREIRA PINTO</td>
      <td>43</td>
      <td>43666</td>
      <td>100.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867379</th>
      <td>130000003834</td>
      <td>NaN</td>
      <td>MG</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>Vereador</td>
      <td>FRANK ROBERT ALVES FERREIRA</td>
      <td>43</td>
      <td>43666</td>
      <td>100.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867380</th>
      <td>130000003834</td>
      <td>NaN</td>
      <td>MG</td>
      <td>JOSÉ RAYDAN</td>
      <td>PV</td>
      <td>Vereador</td>
      <td>JULIANA ALVES DA SILVA</td>
      <td>43</td>
      <td>43666</td>
      <td>200.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867381</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>JOSÉ KAIO FELIPE NERY</td>
      <td>25</td>
      <td>25555</td>
      <td>320.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867382</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>AMANDA ELIZABETTE NERY CAMPOS</td>
      <td>25</td>
      <td>25555</td>
      <td>800.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867383</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>AMANDA ELIZABETTE NERY CAMPOS</td>
      <td>25</td>
      <td>25555</td>
      <td>650.0</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867384</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>MARTA LOURDES DA SILVA</td>
      <td>25</td>
      <td>25555</td>
      <td>883.8</td>
      <td>Recursos de pessoas físicas</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867385</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>Direção Municipal/Comissão Provisória</td>
      <td>25</td>
      <td>25555</td>
      <td>70.0</td>
      <td>Recursos de partido político</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867386</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>Direção Municipal/Comissão Provisória</td>
      <td>25</td>
      <td>25555</td>
      <td>70.0</td>
      <td>Recursos de partido político</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867387</th>
      <td>170000009990</td>
      <td>NaN</td>
      <td>PE</td>
      <td>GLÓRIA DO GOITÁ</td>
      <td>DEM</td>
      <td>Vereador</td>
      <td>Direção Municipal/Comissão Provisória</td>
      <td>25</td>
      <td>25555</td>
      <td>50.0</td>
      <td>Recursos de partido político</td>
      <td>NaN</td>
      <td>Estimado</td>
    </tr>
    <tr>
      <th>1867388</th>
      <td>160000008858</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CAPANEMA</td>
      <td>PMDB</td>
      <td>Vereador</td>
      <td>ROQUE OSMAR POMPERMAIER</td>
      <td>15</td>
      <td>15123</td>
      <td>500.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
    <tr>
      <th>1867389</th>
      <td>160000008858</td>
      <td>NaN</td>
      <td>PR</td>
      <td>CAPANEMA</td>
      <td>PMDB</td>
      <td>Vereador</td>
      <td>ROQUE OSMAR POMPERMAIER</td>
      <td>15</td>
      <td>15123</td>
      <td>150.0</td>
      <td>Recursos próprios</td>
      <td>NaN</td>
      <td>Depósito em espécie</td>
    </tr>
  </tbody>
</table>
<p>1867390 rows × 13 columns</p>
</div>



Agora vamos salvar a base para um arquivo CSV usando a função "to_csv" do Pandas:


```python
base_cortada.to_csv("receitas_GN.csv", encoding="utf-8")
```

## Buscando o sexo

Agora que temos a base que precisamos para fazer as análises, precisamos buscar o sexo de todos os candidatos. 

Vamos usar a coluna "Sequencial Candidato" na base que acabamos de criar e vamos compará-la com uma coluna de mesmo nome na base "consulta_cand_2016_brasil.txt".

Essa base foi criada unindo [todos os arquivos TXT do TSE](http://agencia.tse.jus.br/estatistica/sead/odsele/consulta_cand/consulta_cand_2016.zip) com um simples comando no terminal:

```bash
cat *.txt > consulta_cand_2016_brasil.csv
```

Você pode renomear depois para .txt (como abaixo) ou deixar .csv mesmo. Agora basta criarmos um novo DataFrame com a base que acabamos de definir:


```python
candidatos = pd.read_csv(
    'data/consulta_cand_2016_brasil.txt', 
    delimiter=";", 
    decimal=",", 
    encoding="latin1",
    header=None,
    low_memory=False)
```


```python
candidatos.columns
```




    Int64Index([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
                17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33,
                34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45],
               dtype='int64')



Como percebemos acima, o arquivo de candidaturas não possui cabeçalho. Foi por isso que um dos parâmetros na hora de importar o arquivo foi "``header=None``". 

Com isso, o Pandas nomeou em sequência as colunas do 0 até o 45, totalizando 46 colunas.

Para a consulta de sexo dos candidados vamos renomear as colunas 10, 11 e 30. Assim, criaremos uma nova base apenas com os nomes, o número sequencial e o sexo dos candidatos. Vamos usar essa base para acrescentar a informação de sexo na nossa tabela de doações:


```python
candidatos.rename(columns={10: "Nome Candidato", 11:"Sequencial Candidato", 30:"Sexo"}, inplace=True)
```


```python
candidatos_sexo = pd.DataFrame(candidatos, columns=["Sequencial Candidato", "Nome Candidato", "Sexo"])
```


```python
candidatos_sexo
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Sequencial Candidato</th>
      <th>Nome Candidato</th>
      <th>Sexo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10000000924</td>
      <td>MARIA DAS GRAÇAS LIMA FERREIRA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10000000792</td>
      <td>MARIA AUXILIADORA DIAS</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10000000941</td>
      <td>MAMED DANKAR NETO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10000003498</td>
      <td>JUCIANE DA SILVA PEIXOTO</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10000000953</td>
      <td>FRANCISCO RODRIGUES DOS SANTOS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10000000965</td>
      <td>DANIEL RIBEIRO DE MOURA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10000000889</td>
      <td>CLERTON GASPAR DE SOUZA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10000000893</td>
      <td>ANDRÉ LUIZ MELHORANÇA FILHO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10000000897</td>
      <td>MARIA MARCELA MESSIAS DE MELO</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10000002682</td>
      <td>DOMINGOS SAVIO MOREIRA DE ANDRADE</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10000000906</td>
      <td>JOAREZ GOMES BEZERRA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10000000910</td>
      <td>ELISANGELA MARIA GOMES DA SILVA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>12</th>
      <td>10000000922</td>
      <td>MARCUS ALEXANDRE MÉDICI AGUIAR VIANA DA SILVA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>13</th>
      <td>10000000933</td>
      <td>ANTONIO LIRA DE MORAIS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>14</th>
      <td>10000001061</td>
      <td>ABÍLIO RODRIGUES BARBOSA NETO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>15</th>
      <td>10000003504</td>
      <td>DAMIAO DA SILVA NASCIMENTO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>16</th>
      <td>10000001071</td>
      <td>LAEZIO FERREIRA DE ALMEIDA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>17</th>
      <td>10000001362</td>
      <td>PATRÍCIA BATISTA SCHILLING</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>18</th>
      <td>10000001078</td>
      <td>MARTA REGINA MARQUES DA SILVA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>19</th>
      <td>10000001079</td>
      <td>JOSÉ BONFIM DE OLIVEIRA AMORIM</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>20</th>
      <td>10000001083</td>
      <td>SEBASTIÃO BENICIO DE SOUZA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>21</th>
      <td>10000001091</td>
      <td>EDINEIA CANDIDA BERTO</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>22</th>
      <td>10000001097</td>
      <td>VALDECIR DE LIMA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>23</th>
      <td>10000001100</td>
      <td>MARCIANO BEZERRA DA SILVA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>24</th>
      <td>10000001101</td>
      <td>VILSON DOS SANTOS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>25</th>
      <td>10000001102</td>
      <td>ROZENO DA SILVA MELO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>26</th>
      <td>10000003261</td>
      <td>LEIDA KULINA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>27</th>
      <td>10000003262</td>
      <td>SALOMÃO JOSÉ DIAS MADJA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>28</th>
      <td>10000003465</td>
      <td>MANOEL NAGUILO GOMES DE AZEVEDO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>29</th>
      <td>10000001129</td>
      <td>MARIA DE NAZARÉ MARQUES DA SILVA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>497096</th>
      <td>270000008880</td>
      <td>TOMAZ DA SILVA XAVIER</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497097</th>
      <td>270000007598</td>
      <td>JOSE AMERICO AQUINO DE SOUSA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497098</th>
      <td>270000007599</td>
      <td>JAVAN QUIXABEIRA DE JESUS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497099</th>
      <td>270000007613</td>
      <td>JOSE PAULINO LIMA DOS SANTOS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497100</th>
      <td>270000009393</td>
      <td>FRANCO BARBOSA DE SOUSA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497101</th>
      <td>270000009401</td>
      <td>JEANE DA SILVA SOUSA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497102</th>
      <td>270000005800</td>
      <td>LUZIANE ALVES RIBEIRO</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497103</th>
      <td>270000005805</td>
      <td>ANTONIO CARLOS DE OLIVEIRA COSTA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497104</th>
      <td>270000005499</td>
      <td>ARTUR DIAS BENTO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497105</th>
      <td>270000005320</td>
      <td>ROSINALVA PEREIRA GOMES DE SOUSA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497106</th>
      <td>270000005351</td>
      <td>GENIVALDO NERES FREITAS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497107</th>
      <td>270000006334</td>
      <td>ROGERIO TAVARES RODRIGUES DA COSTA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497108</th>
      <td>270000006343</td>
      <td>MAURO JOSE RODRIGUES FRAGOSO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497109</th>
      <td>270000009296</td>
      <td>NILZA RODRIGUES DE SOUZA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497110</th>
      <td>270000009043</td>
      <td>RAIMUNDO NONATO GOMES DOS SANTOS</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497111</th>
      <td>270000007451</td>
      <td>ELVI LEAO COSTA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497112</th>
      <td>270000008044</td>
      <td>ANTONIA OZELIA DA SILVA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497113</th>
      <td>270000008063</td>
      <td>MAYANE RODRIGUES BEZERRA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497114</th>
      <td>270000005075</td>
      <td>DORECINO BARBOSA CIRQUEIRA</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497115</th>
      <td>270000004675</td>
      <td>FRANCIANE CARNEIRO DE SOUZA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497116</th>
      <td>270000001987</td>
      <td>FRANCISCA AUDEIDE RODRIGUES DO NASCIMENTO</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497117</th>
      <td>270000005891</td>
      <td>NATANAEL DOS SANTOS MACEDO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497118</th>
      <td>270000006906</td>
      <td>JOÃO ALBERTO COELHO MACHADO</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497119</th>
      <td>270000006909</td>
      <td>MARCOS AURÉLIO SUWATE XERENTE</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497120</th>
      <td>270000006924</td>
      <td>SUELENE LUSTOSA MATOS</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497121</th>
      <td>270000006927</td>
      <td>MARIA DO SOCORRO LUSTOSA DE SOUSA</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497122</th>
      <td>270000006933</td>
      <td>NEURAN RIBEIRO GUIMARÃES</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497123</th>
      <td>270000008097</td>
      <td>PATRICIA PIMENTEL HENRIQUE</td>
      <td>FEMININO</td>
    </tr>
    <tr>
      <th>497124</th>
      <td>270000007944</td>
      <td>GILSON ALVES ZIELINSKI</td>
      <td>MASCULINO</td>
    </tr>
    <tr>
      <th>497125</th>
      <td>270000010291</td>
      <td>JOAOZINEI FRANCISCO DA ROCHA</td>
      <td>MASCULINO</td>
    </tr>
  </tbody>
</table>
<p>497126 rows × 3 columns</p>
</div>



Vamos salvar uma cópia desse arquivo no formato CSV. Assim você pode importar para o seu processador de planilhas favorito.


```python
candidatos_sexo.to_csv("consulta_cand_2016_sexo.csv", encoding="utf-8")
```

## Unindo tabelas pelo número sequencial

Vamos unir as duas tabelas usando a coluna que elas possuem em comum: ``Sequencial Candidato``. À esquerda, vamos usar a nossa base cortada, à direita a base de candidatos com o sexo. O ponto de união das duas bases será a coluna ``Sequencial Candidato``.

O Pandas já faz o trabalho pra gente: onde o número sequencial na tabela com os sexos for X, por exemplo, ele vai acrescentar o valor do sexo automaticamente. Funciona como um VLOOKUP/PROCV ou index/match:


```python
left = base_cortada
right = candidatos_sexo

merge = pd.merge(left,right, on="Sequencial Candidato")
```

## Dividindo entre prefeitos e vereadores

Os valores de doação para prefeitos e vereadores são muito diferentes.

Vamos criar uma base apenas com os cargos de prefeito e outra apenas com vereadores:


```python
merge_filtrado = merge[merge['Tipo receita'] == u"Recursos de partido político"]
prefeitos = merge_filtrado[merge_filtrado['Cargo'] == u"Prefeito"]
vereadores = merge_filtrado[merge_filtrado['Cargo'] == u"Vereador"]
```

Agora geramos duas tabelas dinâmicas com os valores agregados.

# Quem recebeu mais: homens ou mulheres?

Nesse caso, queremos saber quanto os partidos doaram para mulheres e homens em cada localidade e quantas mulheres e homens estavam concorrendo em cada cidade.


```python
tabela_final_prefeitos = prefeitos.pivot_table(index=[u'Número partido doador',
                         u'UF', 
                         u'Nome da UE'],
                  columns=[u'Sexo'],
                  values=['Valor receita', "Sequencial Candidato"], 
                  aggfunc=[sum, pd.Series.nunique])

tabela_final_vereadores = vereadores.pivot_table(index=[u'Número partido doador',
                         u'UF', 
                         u'Nome da UE'],
                  columns=[u'Sexo'],
                  values=['Valor receita', "Sequencial Candidato"], 
                  aggfunc=[sum, pd.Series.nunique])
```


```python
tabela_final_prefeitos
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">sum</th>
      <th colspan="4" halign="left">nunique</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th colspan="2" halign="left">Valor receita</th>
      <th colspan="2" halign="left">Sequencial Candidato</th>
      <th colspan="2" halign="left">Valor receita</th>
      <th colspan="2" halign="left">Sequencial Candidato</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>Sexo</th>
      <th>FEMININO</th>
      <th>MASCULINO</th>
      <th>FEMININO</th>
      <th>MASCULINO</th>
      <th>FEMININO</th>
      <th>MASCULINO</th>
      <th>FEMININO</th>
      <th>MASCULINO</th>
    </tr>
    <tr>
      <th>Número partido doador</th>
      <th>UF</th>
      <th>Nome da UE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="30" valign="top">10</th>
      <th rowspan="2" valign="top">AC</th>
      <th>ACRELÂNDIA</th>
      <td>55394.00</td>
      <td>NaN</td>
      <td>6.000001e+10</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>SENADOR GUIOMARD</th>
      <td>NaN</td>
      <td>38058.00</td>
      <td>NaN</td>
      <td>9.000002e+10</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>AM</th>
      <th>MANAUS</th>
      <td>NaN</td>
      <td>1060000.00</td>
      <td>NaN</td>
      <td>1.200000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>AP</th>
      <th>MACAPÁ</th>
      <td>905000.00</td>
      <td>NaN</td>
      <td>1.500000e+11</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">BA</th>
      <th>AMÉLIA RODRIGUES</th>
      <td>NaN</td>
      <td>6478.50</td>
      <td>NaN</td>
      <td>2.500000e+11</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>LAJEDO DO TABOCAL</th>
      <td>NaN</td>
      <td>20350.00</td>
      <td>NaN</td>
      <td>1.500000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CE</th>
      <th>FORTALEZA</th>
      <td>NaN</td>
      <td>777000.00</td>
      <td>NaN</td>
      <td>2.400000e+11</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">GO</th>
      <th>MONTE ALEGRE DE GOIÁS</th>
      <td>NaN</td>
      <td>5000.00</td>
      <td>NaN</td>
      <td>9.000000e+10</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>NOVO GAMA</th>
      <td>NaN</td>
      <td>145000.00</td>
      <td>NaN</td>
      <td>4.500001e+11</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>PORANGATU</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>9.000000e+10</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>RUBIATABA</th>
      <td>NaN</td>
      <td>30000.00</td>
      <td>NaN</td>
      <td>1.800000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>SÃO SIMÃO</th>
      <td>NaN</td>
      <td>5000.00</td>
      <td>NaN</td>
      <td>9.000001e+10</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="14" valign="top">MA</th>
      <th>ARAIOSES</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>BURITICUPU</th>
      <td>NaN</td>
      <td>10594.00</td>
      <td>NaN</td>
      <td>1.300000e+12</td>
      <td>NaN</td>
      <td>13.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CAXIAS</th>
      <td>NaN</td>
      <td>215000.00</td>
      <td>NaN</td>
      <td>3.000000e+11</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CHAPADINHA</th>
      <td>38000.00</td>
      <td>NaN</td>
      <td>2.000000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>GODOFREDO VIANA</th>
      <td>2300.00</td>
      <td>NaN</td>
      <td>1.300000e+12</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>GOVERNADOR NEWTON BELLO</th>
      <td>NaN</td>
      <td>20000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>LAGO DA PEDRA</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>NINA RODRIGUES</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>PAÇO DO LUMIAR</th>
      <td>NaN</td>
      <td>20000.00</td>
      <td>NaN</td>
      <td>2.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>PIRAPEMAS</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>PRIMEIRA CRUZ</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>RAPOSA</th>
      <td>30000.00</td>
      <td>NaN</td>
      <td>2.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>SÃO VICENTE FERRER</th>
      <td>NaN</td>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>TIMBIRAS</th>
      <td>NaN</td>
      <td>20000.00</td>
      <td>NaN</td>
      <td>2.000000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">MG</th>
      <th>ABRE CAMPO</th>
      <td>NaN</td>
      <td>5000.00</td>
      <td>NaN</td>
      <td>1.300000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>BAMBUÍ</th>
      <td>NaN</td>
      <td>403.45</td>
      <td>NaN</td>
      <td>1.300001e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>BANDEIRA</th>
      <td>NaN</td>
      <td>52.00</td>
      <td>NaN</td>
      <td>1.300000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CACHOEIRA DE MINAS</th>
      <td>NaN</td>
      <td>104.00</td>
      <td>NaN</td>
      <td>1.300000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <th>...</th>
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
      <th rowspan="30" valign="top">90</th>
      <th rowspan="2" valign="top">GO</th>
      <th>SÍTIO D'ABADIA</th>
      <td>NaN</td>
      <td>1240.46</td>
      <td>NaN</td>
      <td>6.300001e+11</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>ÁGUA FRIA DE GOIÁS</th>
      <td>NaN</td>
      <td>21152.01</td>
      <td>NaN</td>
      <td>3.600000e+11</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">MG</th>
      <th>ANDRADAS</th>
      <td>10980.02</td>
      <td>NaN</td>
      <td>1.170001e+12</td>
      <td>NaN</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>BELO HORIZONTE</th>
      <td>NaN</td>
      <td>120000.00</td>
      <td>NaN</td>
      <td>2.600002e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>DIVINÓPOLIS</th>
      <td>NaN</td>
      <td>65000.00</td>
      <td>NaN</td>
      <td>2.600002e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>LUISBURGO</th>
      <td>NaN</td>
      <td>5000.00</td>
      <td>NaN</td>
      <td>1.300001e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>PARAOPEBA</th>
      <td>NaN</td>
      <td>17969.68</td>
      <td>NaN</td>
      <td>9.100002e+11</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>RAUL SOARES</th>
      <td>5000.00</td>
      <td>NaN</td>
      <td>1.300000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>SANTA MARIA DE ITABIRA</th>
      <td>NaN</td>
      <td>3000.00</td>
      <td>NaN</td>
      <td>3.900002e+11</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>PE</th>
      <th>RIO FORMOSO</th>
      <td>NaN</td>
      <td>47200.00</td>
      <td>NaN</td>
      <td>1.700000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">PI</th>
      <th>CRISTINO CASTRO</th>
      <td>10000.00</td>
      <td>NaN</td>
      <td>1.800000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>SÃO GONÇALO DO GURGUÉIA</th>
      <td>NaN</td>
      <td>689.50</td>
      <td>NaN</td>
      <td>3.600000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>SÃO JOÃO DO PIAUÍ</th>
      <td>NaN</td>
      <td>20000.00</td>
      <td>NaN</td>
      <td>1.800000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="13" valign="top">PR</th>
      <th>CAFELÂNDIA</th>
      <td>NaN</td>
      <td>768.00</td>
      <td>NaN</td>
      <td>1.600000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CAMBIRA</th>
      <td>NaN</td>
      <td>331.85</td>
      <td>NaN</td>
      <td>3.200000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CATANDUVAS</th>
      <td>NaN</td>
      <td>55.41</td>
      <td>NaN</td>
      <td>1.600000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>CRUZEIRO DO OESTE</th>
      <td>456.60</td>
      <td>NaN</td>
      <td>3.200000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>FOZ DO IGUAÇU</th>
      <td>NaN</td>
      <td>60000.00</td>
      <td>NaN</td>
      <td>1.600000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>IMBAÚ</th>
      <td>NaN</td>
      <td>454.80</td>
      <td>NaN</td>
      <td>3.200000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>MARECHAL CÂNDIDO RONDON</th>
      <td>NaN</td>
      <td>400.50</td>
      <td>NaN</td>
      <td>3.200000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>REBOUÇAS</th>
      <td>NaN</td>
      <td>8900.00</td>
      <td>NaN</td>
      <td>4.480000e+12</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>RIBEIRÃO DO PINHAL</th>
      <td>NaN</td>
      <td>345.00</td>
      <td>NaN</td>
      <td>1.600000e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>RONDON</th>
      <td>NaN</td>
      <td>1775.00</td>
      <td>NaN</td>
      <td>3.200000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>TELÊMACO BORBA</th>
      <td>NaN</td>
      <td>6469.30</td>
      <td>NaN</td>
      <td>8.000002e+11</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>VERA CRUZ DO OESTE</th>
      <td>NaN</td>
      <td>1595.00</td>
      <td>NaN</td>
      <td>6.400000e+11</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>VERÊ</th>
      <td>NaN</td>
      <td>3741.25</td>
      <td>NaN</td>
      <td>4.800000e+11</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>RO</th>
      <th>OURO PRETO DO OESTE</th>
      <td>35000.00</td>
      <td>NaN</td>
      <td>6.600000e+11</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>RS</th>
      <th>PELOTAS</th>
      <td>NaN</td>
      <td>1751.00</td>
      <td>NaN</td>
      <td>6.300001e+11</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">SP</th>
      <th>JARINU</th>
      <td>2150.00</td>
      <td>NaN</td>
      <td>2.500001e+11</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>MOCOCA</th>
      <td>NaN</td>
      <td>3508.00</td>
      <td>NaN</td>
      <td>1.000000e+12</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>5606 rows × 8 columns</p>
</div>




```python
tabela_final_prefeitos.to_csv("tabela_final_prefeitos.csv", encoding="utf-8")
tabela_final_vereadores.to_csv("tabela_final_vereadores.csv", encoding="utf-8")
```

# Qual foi o valor máximo doado de uma vez só?

Também queremos saber quais foram os valores máximos doados por cada partido em cada um dos cargos.

Geramos outra tabela dinâmica, assim:


```python
maximos_prefeitos = prefeitos.pivot_table(index=[u'Número partido doador'],
                            values=['Valor receita'],
                            columns=["Sexo"],
                            aggfunc=max)

maximos_vereadores = vereadores.pivot_table(index=[u'Número partido doador'],
                            values=['Valor receita'],
                            columns=["Sexo"],
                            aggfunc=max)
```


```python
maximos_prefeitos.to_csv("maximos_prefeitos.csv", encoding = "utf-8")
maximos_vereadores.to_csv("maximos_vereadores.csv", encoding = "utf-8")
```


```python
maximos_vereadores
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="2" halign="left">Valor receita</th>
    </tr>
    <tr>
      <th>Sexo</th>
      <th>FEMININO</th>
      <th>MASCULINO</th>
    </tr>
    <tr>
      <th>Número partido doador</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>30000.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>90000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>22500.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>200000.00</td>
      <td>200000.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>150000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>50000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4071.94</td>
      <td>20000.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>22000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2204.00</td>
      <td>9000.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>15000.00</td>
      <td>300000.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>12000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>100.00</td>
      <td>2400.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>133000.00</td>
      <td>300000.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>50000.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>300000.00</td>
      <td>1000000.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>8000.00</td>
      <td>200000.0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>19950.00</td>
      <td>25000.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>10000.00</td>
      <td>21000.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>25000.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>9000.00</td>
      <td>47500.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>10000.00</td>
      <td>59000.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>8530.00</td>
      <td>21600.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>40000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>43</th>
      <td>50000.00</td>
      <td>25000.0</td>
    </tr>
    <tr>
      <th>44</th>
      <td>12000.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>100000.00</td>
      <td>100000.0</td>
    </tr>
    <tr>
      <th>50</th>
      <td>9600.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>51</th>
      <td>20000.00</td>
      <td>139000.0</td>
    </tr>
    <tr>
      <th>54</th>
      <td>13600.00</td>
      <td>40000.0</td>
    </tr>
    <tr>
      <th>55</th>
      <td>200000.00</td>
      <td>200000.0</td>
    </tr>
    <tr>
      <th>65</th>
      <td>26818.75</td>
      <td>38000.0</td>
    </tr>
    <tr>
      <th>70</th>
      <td>4000.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>77</th>
      <td>15000.00</td>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>90</th>
      <td>10000.00</td>
      <td>80000.0</td>
    </tr>
  </tbody>
</table>
</div>



Ainda é possível fazer muitas análises com essa base! Estamos só começando.

Se você gostaria de fazer algum comentário ou sugestão, entre em contato com a gente no contato@generonumero.media

