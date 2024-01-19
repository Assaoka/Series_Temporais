<h1 align = "center"> Anotações </h1>

## Definição:
&emsp;&emsp; Uma série temporal é uma sequência de dados que ocorrem em ordem sucessiva durante algum período de tempo. Se um fenômeno ocorre ao longo do tempo, podemos coletar dados sobre esse fenômeno em intervalos regulares e para construir uma série temporal.

&emsp;&emsp; A análise de séries temporais se preocupa em entender padrões e tendências em dados passados, utilizando técnicas como decomposição, suavização e análise de tendência e sazonalidade. O que permite criar modelos que descrevem o comportamento passado e, possivelmente, prever o comportamento futuro.


## Passeio Aleatório:
&emsp;&emsp; Um passeio aleatório é frequentemente usado como um modelo simplificado para valores como preço de um ativo financeiro. De acordo com esse modelo, o preço de um ativo é a soma de todas as variações aleatórias passadas e que essas variações são independentes entre si. Isso significa que as variações não estão correlacionadas e não podem ser previstas.

&emsp;&emsp; Na prática, a maioria das séries temporais são uma combinação de um passeio aleatório (ruído) com algum padrão que pode ser modelado.


## Componentes de uma Série Temporal:
- `Tendência:` Descreve a direção geral que a série está seguindo (crescente, decrescente ou constante). Pode ser linear ou não linear. Geralmente, a tendência é modelada com médias móveis, suavização exponencial ou regressão linear.

- `Sazonalidade:` Descreve o padrão de repetição de um ciclo de tempo fixo (diário, semanal, mensal, anual). Por exemplo, a variação da temperatura ao longo do ano (estações).

- `Ruído:` Como já mencionado, o ruído é uma variação irregular e imprevisível que não pode ser atribuída a tendência ou sazonalidade.

![Componentes de uma Série Temporal](./[2]%20Transformação%20e%20Decomposição/Gráficos/Decomposição.png)
Fonte da Imagem: [[2] Previsão de Dados Pluviometricos com Modelos Clássicos](./)


## Estacionariedade:
&emsp;&emsp; Uma série temporal é estacionária se suas propriedades estatísticas (média, variância e autocorrelação) não mudam com o tempo. A maioria dos modelos de séries temporais assume que a série é estacionária. Se a série não for estacionária, ela deve passar por uma transformação para que se aproxime de uma série estacionária.
~~~python
import matplotlib.pyplot as plt

def visualizar_estacionaridade (série):
	plt.plot(série, label = 'Série Real')
	plt.plot(série.rolling(12).mean(), label = 'Média Móvel')
	plt.legend(loc = 'best')
	plt.show()
~~~

~~~python
import statsmodels.tsa.stattools as stattools

def teste_KPSS(série):
    kpss = stattools.kpss(série)
    print(f'Estatística de Teste = {kpss[0]}')
    print(f'p-valor = {kpss[1]}')
    print(f'Valores Críticos:')
    for chave, valor in kpss[3].items():
        print(f'    {chave}: {valor}')
    print(f'Resultado: {"Série Estacionária" if (kpss[0] < kpss[3]["5%"]) else "Série Não Estacionária"}')
~~~

## Normalidade:
&emsp;&emsp; Uma série temporal é normal se seus valores seguirem uma distribuição normal. A maioria dos modelos de séries temporais assume que a série é normal. Se a série não for normal, ela deve passar por uma transformação para que se aproxime de uma série normal.
~~~python
import seaborn as sns
import scipy.stats as stats
import matplotlib.pyplot as plt

def visualizar_normalidade(resíduo):
    stats.probplot(resíduo, dist = 'norm', plot = plt)
    plt.title('Normal QQ Plot - Resíduos')
    plt.show()

    sns.histplot(resíduo, kde = True)
    plt.title('Histograma - Resíduos')
    plt.show()
~~~

~~~python
import scipy.stats as stats

def teste_shapiro(série):
    e, p = stats.shapiro(série)
    print(f'Estatística de Teste = {e}')
    print(f'p-valor = {p}')
    print(f'Resultado: {"É Normal" if (p > 0.05) else "Não é Normal"}')
~~~


## Modelos Clássicos de Séries Temporais:
- `Modelo Autorregressivo (AR - AutoRegressive):` Um modelo AR explica o valor de uma série temporal como uma função linear dos valores anteriores.

- `Modelo de Média Móvel (MA - Moving Average):` Um modelo MA explica o valor de uma série temporal em termos de erros a partir da média móvel.

- `Modelo Autorregressivo de Média Móvel (ARMA - AutoRegressive Moving Average):` Um modelo ARMA combina os dois modelos anteriores.

- `Modelo Autorregressivo Integrado de Média Móvel (ARIMA - AutoRegressive Integrated Moving Average):` Um modelo ARIMA é semelhante ao modelo ARMA, mas é aplicado a uma série temporal que foi diferenciada pelo menos uma vez para torná-la estacionária. ARIMA (p, d, q):
  - **p:** número de termos autorregressivos.
  - **d:** número de vezes que a série temporal deve ser diferenciada.
  - **q:** número de termos de média móvel.

- `Modelo Sazonal Autorregressivo Integrado de Média Móvel (SARIMA - Seasonal AutoRegressive Integrated Moving Average):` Um modelo SARIMA é semelhante ao modelo ARIMA, com um componente sazonal (que permite modelar padrões repetitivos). SARIMA (p, d, q) (P, D, Q, s):
  - **P:** número de termos autorregressivos sazonais.
  - **D:** número de vezes que a série temporal sazonal deve ser diferenciada.
  - **Q:** número de termos de média móvel sazonais.
  - **s:** intervalo de tempo que o padrão se repete.
