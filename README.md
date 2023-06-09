# Netflix-Sigma

## Auto Vetores e Auto Valores

Para uma matriz A não nula, temos autovetores e autovalores que são possíveis de serem calculados. Eles são características que nos permitem modificar e transladar pontos, modificando características da matriz original conforme essas matrizes oriundas são modificadas.

Com eles podemos decompor e recompor uma matriz da seguinte maneira: 

$$
Ax = x \lambda
$$

No exemplo, $A$ é nossa matriz original, $x$ é nossa matriz de auto vetores (colunas), e $\lambda$ é nossa matriz de autovalores. Nessa representação fica claro que uma matriz de autovetores multiplicando a matriz original é equivalente a mesma matriz de autovetores sendo multiplicada por uma matriz de autovalores.


Vamos supor que possuímos uma matriz de dois autovetores, um em cada coluna. Junto a eles, temos dois autovalores. Além disso, possuímos nossa matriz original ($A$). Esse sistema pode ser escrito na forma de uma multiplicação matricial, se assumirmos que nossos auto-vetores são vetores-coluna:

$$
A \begin{bmatrix} x_1 & x_2 \end{bmatrix} = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} 
$$

Multiplicando os dois lados da equação por $\begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1}$, ficamos com:

$$
A \begin{bmatrix} x_1 & x_2 \end{bmatrix}\begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1} = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1}
$$

Isso é necessário para isolarmos nossa matriz original, já que uma matriz vezes a sua inversa é igual a identidade, ou seja, descarta a necessedidade de continuarmos a levar em consideração ambas as matrizes na conta. Dessa maneira, ficamos com a seguinte equação:
$$
A = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \begin{bmatrix} x_1 & x_2 \end{bmatrix}^{-1}
$$

Para sistemas mais complexos, nos teríamos mais autovetores e mais autovalores, mas a ordem do processo continua a mesma. Em termos amplos:

$$
A = x \lambda x^{-1}
$$


## PCA (Principal Components Analysis)

Para chegarmos na utilidade do processo, vemos a matriz de covariância, que relaciona variáveis entre si. Cada variável é representada por uma coluna nessa matriz. Calculamos uma matriz de covariância da seguinte maneira: 
$$
C = A A^T
$$

Ao decompormos essa matriz de covariância e modificarmos os seus autovetores e autovalores, conseguimos reduzir as variáveis necessárias para a visualização de nossos dados. O processo é verdadeiro para matrizes quadradas, sendo chamado de PCA (Principal Components Analysis). 

Para englobar toda forma de matriz, temos o SVD (Singular Value Decomposition).


## SVD (Singular Value Decomposition)

Na decomposição SVD, usamos a formulação:

$
A = U \Sigma V^T,
$

onde:

* As colunas de $U$ são os auto-vetores de $A^T A$,
* As colunas de $V$ (e, portanto, as linhas de $V^T$) são auto-vetores de $A A^T$,
* $\Sigma$ é uma matriz onde $s_{i,i}$ é a raiz quadrada dos auto-valores de $A^T A$ ou de $A A^T$.

O SVD tem aplicações diferentes do PCA. O PCA busca reduzir o número de variáveis correlacionadas, facilitando a visualização de informações. O SVD é utilizado em diversas aplicações, mas a que nos interessa é a redução de ruído. 

Supondo que há uma imagem que possui pixeis esfumaçados ou esteja um pouco ganulada. Podemos representar esses pixeis em uma matriz, que chamaremos de $P$. Conseguimos calcular $U$, $S$ e $V^{T}$ de $P$.

$$
U, S, V^{T} = SVD(P) 
$$

No python, ficaria assim. 

```python 
from scipy.linalg import svd

u, s, vt = svd(P)
```

Com essas novas matrizes, conseguimos reduzir uma das suas dimensões. Reduzimos o número de autovetores de $U$ (colunas) e de $V^{T}$ (linhas), além de fazer o mesmo com o número de autovalores de $S$ (colunas, já que a matriz possui apenas uma linha). No python, fizemos isso usando uma função de comprimir, que recebe as matrizes em forma de np.array, junto a um valor que representa o tamanho das novas matrizes $K$, e devolve elas modificadas. 

```python
def comprimir (u, s, vt, K):
    #  Remove elementos de u, s e vt deixando somente K componentes restantes
    u_ = u[:,:K]
    s_ = s[:K]
    vt_ = vt[:K,:]

    return u_, s_, vt_
```

Mesmo com essa redução, as novas matrizes conseguem voltar a ser a matriz $P$, seguindo a regra abaixo: 

$$
P = U \Sigma V^T,
$$

A matriz $\Sigma$ é uma matriz identidade que possui os autovalores na diagonal, similar a matriz de autovalores do PCA. Um exemplo que ajuda a visualizar essa matriz está abaixo, com dois autovalores: 

$$
\begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}
$$

É importante ter em mente que quanto mais retiramos da matriz original, mais informações relevantes e não relevantes são levadas junto. Além disso, os autovalores e autovetores mais distantes da origem tem um impacto menor no produto final, então, geralmente, podem ser desconsiderados (quando estamos falando de redução de ruida). 


# Netflix - Projeto

O projeto da Netflix é uma versão de redução de ruído. Nele, adicionamos a uma matriz um review aleatório, um "barulho" nas medições. Nossa meta é sair da matriz modificada (com essa review errada) e chegar na matriz original.

A primeira etapa é criar uma matriz $A$ de usuários $\times$ filmes em componentes:

$
A = X Y Z,
$

onde:
* $A$ tem uma linha por usuário e uma coluna por filme,
* $X$ tem uma linha por usuário e uma coluna por perfil,
* $Y$ é quadrada e mapeia perfis para perfis,
* $Z$ tem uma linha por perfil e uma coluna por filme.

Essa decomposição nos lembra muito o SVD. As matrizes, inclusive, se correspondem:

$$
X = U
$$
$$
Y = \Sigma
$$
$$
Z = V^T
$$

Depois, temos que encontrar um valor de $K$ ótimo para comprimir as matrizes de autovetores e autovalores ($XYZ$). Vamos dizer que o número de filmes é representado pela variável $f$ e o número de usuários por $u$. Além disso, temos o número de autovetores da matriz $A$, ele será representado por $v$. Nossas matrizes teriam as seguintes dimensões: 

$$
X_{u \times v}
$$
$$
Y_{v \times v}
$$
$$
Z _{v \times f}
$$

Quando aplicamos nossa compressão, estamos substituíndo o valor de $v$. Assim, temos que:

$$
X_{u \times K}
$$
$$
Y_{K \times K}
$$
$$
Z _{K \times f}
$$ 

## Testes - Valores Gerais de $K$

A próxima etapa é escolher um valor a ser modificado. Fizemos isso de algumas maneiras:
- Valor Conhecido: escolhemos um valor conhecido pelo grupo e que se mantém o mesmo enquanto testamos os valores de $K$.
- Valor Desconhecido: um valor aleatório é substituido por outro valor aleatório a cada iteração.
- Valor Desconhecido Válido: um valor aleatório de um review real (não nulo) é substituido por outro valor aleatório a cada iteração.

Após isso, vamos incrementando esse K de 0 até o máximo possível. Vamos alimentando um CSV com os valores resultantes dos testes: diferença entre os valores, valor original, valor aleatório, valor de $K$ e valor estimado.

Finalmente, plotamos esses resultados em gráficos e os analisamos. 

## Teste de Stress

Fizemos 15 testes de stress para 100, 200 e 300 valores substituídos. O $K$ variava de 400 à 540, valor escolhido após a análise dos gráficos resultantes do testes gerais. 

O resultado desses testes é um gráfico que relaciona os 45 pontos resultantes, mostrando a média da diferença absoluta, e os valores de $K$ testados.

É possível também plotar cada um dos valores separadamente, já que os resultados são salvos em CSVs diferentes (um para cada).