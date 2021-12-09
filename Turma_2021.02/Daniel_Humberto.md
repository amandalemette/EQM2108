# EQM 2108 – MODELAGEM E SIMULAÇÃO DE PROCESSOS NA ENGENHARIA QUÍMICA

### PROF: AMANDA LEMETTE

### ALUNO: DANIEL P. HUMBERTO

# TRABALHO FINAL DE DISCIPLINA

## Título: ESTIMAÇÃO DE PARÂMETROS CINÉTICOS DA DECOMPOSIÇÃO TÉRMICA DE REDE METALORGÂNICA UTILIZANDO OTIMIZAÇÃO POR ENXAME DE PARTÍCULAS

## 1.	Resumo

O presente trabalho propõe um modelo cinético para estimar a constante cinética, energia de ativação e ordem aparente de reação da decomposição térmica de uma rede organometálica, através da otimização por enxame de partículas (Particle Swarm Optmization). O banco de dados foi gerado experimentalmente com o ensaio de análise termogravimétrica até 600°C. A amostra sofre perda severa de massa entre 400 e 520°C.

## 2.	Introdução
  
Estruturas organometálicas (MOFs) são um tipo de material cristalino microporoso construído a partir de metais e ligantes orgânicos capazes de formar uma rede com área superficial e volume de poros elevados[1].  MOFs têm atraído atenção na catálise devido às suas estruturas especiais e propriedades funcionais. Este tipo de material possui amplo campo de aplicação, como separação de gases [2], detecção química [3], tratamento de água [4], biomedicina [5], luminescência [6] e catálise heterogênea [7]. No entanto, ao contrário das zeólitas, a maioria das MOFs não apresenta estabilidade térmica em condições de alta temperatura ou pressão, o que dificulta seu uso como catalisador. Apesar disso, MOFs podem servir como um material de sacrifício para preparar compostos à base de carbono, mais estáveis termicamente e em condições de serem aplicados nos processos catalíticos[8]. Após a exposição a temperaturas elevadas, sob atmosfera inerte, o ligante orgânico é transformado em uma matriz de carbono, na qual a fase metálica permanece encapsulada e dispersa[9].
A análise termogravimétrica é uma ferramenta bastante utilizada para caracterização de materiais, incluindo MOFs [10, 11]. Essa técnica monitora a mudança de peso do material em função da temperatura ao longo do tempo [12].
A taxa de reação de decomposição no ensaio de TGA pode ser descrita como a equação abaixo:

<img src="https://latex.codecogs.com/svg.latex?dX/dt=-k.X^n" title="dX/dt=-k.X^n" /></a>

onde X é a massa da amostra, t é o tempo, k é a constante cinética e n a ordem de reação. Esta expressão pode ser relacionada com outra:

<img src="https://latex.codecogs.com/svg.latex?X=m_0-m_0&space;.(w/w_0&space;)" title="X=m_0-m_0 .(w/w_0 )" /></a>

onde m0 é a massa inicial, w0 é a perda de massa inicial e w é a perda de massa num tempo qualquer. Para o tempo final, w = w∞, que é a perda máxima ao final da análise. e X = 0. Então, a derivada de X com relação ao tempo é:

<img src="https://latex.codecogs.com/svg.latex?dX/dt=-(m_0/w_f&space;).X^n" title="dX/dt=-(m_0/w_f ).X^n" /></a>

Se <img src="https://latex.codecogs.com/svg.latex?f=w/w_f" title="f=w/w_f" /></a> , então chegamos a:

<img src="https://latex.codecogs.com/svg.latex?df/dt=k_0.m_0^(n-1).(1-f)^n.exp(-E_a/RT)" title="df/dt=k_0.m_0^(n-1).(1-f)^n.exp(-E_a/RT)" /></a>

onde k segue a equação de Arrhenius em relação à temperatura.
A última equação possui informações que podem ser retiradas dos dados de TGA, como df/dt e f.

A otimização por enxame de partículas (PSO – Particle Swarm Optimiztion) é um método heurístico de pesquisa global baseada no comportamento natural das partículas de uma população, inspirada no comportamento coletivo de um bando de pássaros ou cardume de peixes. O algoritmo usa o modelo de busca para velocidade e posição, onde um número de partículas compõe uma população e a posição de cada partícula representa uma solução em potencial no espaço. Já a velocidade da partícula é usada para atualizar a posição e a função objetivo “fitness” é usada para avaliar a posição das partículas. No início da busca, velocidade e posição de cada partícula na população são inicializadas e, em seguida, ajustadas por iterações[13].
em 1995, foram propostas as equações de velocidade e posição para a técnica [14]:

<img src="https://latex.codecogs.com/svg.latex?v_(i,d)^(k&plus;1)=&space;v_(i,d)^k&plus;&space;c_1.r_1&space;.(p_(i,d)^k-&space;x_(i,d)^k&space;)&plus;&space;c_2.r_2&space;.(p_(global,d)^k-&space;x_(i,d)^k&space;)" title="v_(i,d)^(k+1)= v_(i,d)^k+ c_1.r_1 .(p_(i,d)^k- x_(i,d)^k )+ c_2.r_2 .(p_(global,d)^k- x_(i,d)^k )" /></a>

<img src="https://latex.codecogs.com/svg.latex?x_(i,d)^(k&plus;1)=&space;x_(i,d)^k&plus;v_(i,d)^(k&plus;1)" title="x_(i,d)^(k+1)= x_(i,d)^k+v_(i,d)^(k+1)" /></a>  

onde v e x são, respectivamente, velocidade e posição no espaço de busca da partícula i,  d é a direção de busca, k é o número de iteração, c1 e c2 são duas constantes positivas chamadas de parâmetro cognitivo e social, r1 e r2 são números aleatórios com distribuição uniforme no intervalo de 0 a 1, sempre diferentes para cada direção, partícula e iteração. pi é o melhor ponto encontrado pela partícula i e pglobal é o melhor valor encontrado por todo o enxame.
Portanto, o presente trabalho tem por objetivo, estimar a energia de ativação, a constante cinética e a ordem de reação da decomposição da MOF utilizando a técnica de PSO sobre os dados de TGA, gerados experimentalmente.

## 3. Metodologia

A análise experimental foi realizada no analisador térmico Shimadzu, modelo DTG-60, com rampa de aquecimento de 5°C/min até 600°C. A figura 1 mostra o gráfico construído durante o ensaio:

<center><img src="https://github.com/amandalemette/EQM2108/blob/41cd50b072f687666f5945788efad9e6336ee0fd/Turma_2021.02/Imagens/Gr%C3%A1fico_TGA_Trabalho_Final_Daniel%20Humberto.jpg?raw=true"  width=500 height=300 /><center>
  

Analisando a curva de DTG, em azul, nota-se que a decomposição do material é uma reação exotérmica, pois o pico da curva está para cima, com fluxo térmico positivo. Há dois picos e descontinuidades na curva, indicando reações diferentes ao longo do processo, indicando uma complexidade para a simulação dos dados experimentais. O gráfico foi construído com base nos dados obtidos da reação, que são tempo, temperatura, DTG e massa da amostra, sendo estas informações o banco de dados a ser usado no enxame de partículas.
Os dados coletados do ensaio de TGA possuem, originalmente, uma variação muito pequena no tempo (1 segundo) e há elementos ruidosos no conjunto. 
Utilizando o banco de dados sem tratá-lo, o gráfico de variação da massa com o tempo apresentaria descontinuidades e oscilações por haver muitos ruídos atrelados aos dados e isto não é interessante para o trabalho. Portanto, para suavizar a curva, escolheu-se trabalhar com um intervalo de tempo maior (minutos).
  Para efeito de comparação, escolheu-se um método determinístico de otimização chamado BFGS (Broyden–Fletcher–Goldfarb–Shanno algorithm), para avaliar se os parâmetros k, n e Ea poderiam ser estimados a partir de um valor inicial informado para cada um deles. Em seguida, aplicou-se o método heurístico de enxame de partículas (PSO), onde definiu-se limites inferior e superior para cada parâmetro a fim de avaliar qual método foi mais adequado ao experimento, através do coeficiente de determinação R2.

## 4. Resultados
  
A curva de t x f mostra o comportamento da variação de massa ao longo do tempo:

<center><img src="https://github.com/amandalemette/EQM2108/blob/c44bc2798835c1435568fe9db4a2ae83c5a84768/Turma_2021.02/Imagens/Gr%C3%A1fico_txf_Trabalho_Final_Daniel%20Humberto.jpg?raw=true"width=400 height=225 /><center>

Nota-se que até um certo instante de tempo, a variação de massa é muito pequena e, considerando os pontos anteriores a 35 min, o modelo tende a se afastar dos valores experimentais. Então, escolheu-se trabalhar na região onde a variação de massa é significativa, isto é, de 35 a 58 minutos. Porém, analisando os dados finais, a partir do minuto 54, houve uma mudança no comportamento da variação de massa, onde a grandeza df/dt se tornou negativa, indicando ganho de massa, o que é um erro, pois a reação estudada é de decomposição. Esta mudança pode ter como causa uma reação paralela ou alguma absorção na amostra, o que, em teoria, não deveria ocorrer. Portanto, considerou-se o final da análise em 53 min, onde a conversão atingiu seu máximo (df/dt = 0). O banco de dados ficou reduzido à tabela abaixo:
  
<center><img
src="https://github.com/amandalemette/EQM2108/blob/d3441c7d13538f0a91a004edb9a3bb72a1997555/Turma_2021.02/Imagens/imagem%20banco%20de%20dados%20final_Daniel_Humberto.png?raw=true"width=300 height=300 /><center>

Com o “chute inicial” para k, n e Ea em 200, 1 e 100, o método BFGS estimou os valores de 1.164462e+06 min-1, 0,5767574 e 9.253978e+04 J/mol.K, respectivamente, alcançando um coeficiente de determinação R2 de 54%. Um gráfico das respostas predita e experimental foi gerado para efeitos de comparação:
  
<center><img
src="https://github.com/amandalemette/EQM2108/blob/ea08ec7943ba581f073b7620d9997c364ec83a2d/Turma_2021.02/Imagens/finalplot2_BFGS%20-%20Daniel%20Humberto%20-%20de%2035%20a%2058%20min.png?raw=true"width=400 height=300 /><center>

A cada modificação nos valores de partida, o método convergia para respostas diferentes, levantando a suspeita de mínimos locais no banco de dados, dificultando a determinação dos valores globais para o conjunto. Através do gráfico de k x T, percebe-se que a predição do modelo não acompanha o pico dos dados experimentais. Além disso, aparentemente o ponto inicial fica distante do comportamento coletivo do fenômeno, o que pode diminuir a precisão do modelo.
Já o método PSO encontrou os valores de 9277953 min-1 , 0.637307 e 105457 para k, n e Ea, respectivamente, atingindo um R2 de 57%. Ambos os métodos estimaram os parâmetros com ordem de grandezas compatíveis, porém, valores distintos, o que pode ser um indício de haver regiões de mínimo local, das quais o algoritmo não consegue convergir para o mínimo global. Para fins de comprovação, calculou-se o R2 sem o primeiro ponto (min 36), o qual possui valor de df/dt muito distinto dos demais. O R2 saiu de 57 para 71%, confirmando a influência do início da sequência de dados em relação à predição.

## 5. Conclusão
  
A decomposição térmica realizada no equipamento de análise termogravimétrica, possibilitou construir um banco de dados para estimação de parâmetros cinéticos da reação. Apesar da precisão do equipamento, houve necessidade de tratar os dados coletados em busca de valores mais limpos para a aplicação dos métodos determinístico (BFGS) e heurístico (PSO). Ambos alcançaram um valor de coeficiente de determinação baixo, 54 e 57%, respectivamente, levantando algumas questões:

  •	Mínimos locais impedindo a convergência para o valor global
  •	Complexidade do fenômeno físico-químico analisado em relação ao modelo proposto, visto que há uma variação na curva de derivada, com dois picos, indicando reações paralelas, além de haver acúmulo de massa no final da análise, o que não é esperado.
  •	A eliminação de vários pontos de análise para limpar os dados, possivelmente, acarretou perda de dados relevantes para o modelo.
Como sugestão, pode-se escolher uma outra tratativa em relação ao banco de dados para tentar diminuir o ruído, mas sem perder relevância dos valores, talvez, com a média móvel, o modelo possa predizer melhor os dados em relação ao experimento.
  
## 6. Referências
  
1.	Chen, L. and Xu, Q.: Metal-Organic Framework Composites for Catalysis, Matter, vol.1, 57-89, 2019.

2.	Marti, A. M., Venna, S. R., Roth, E. A., Culp, J. T., and Hopkinson, D. P.: Simple Fabrication Method for Mixed Matrix Membranes with inSitu MOF Growth for Gas Separation, ACS Applied Materials and Interfaces, vol. 10, 24784-24790, 2018.

3.	Small, L. J., Henkelis, S. E., Rademacher, D. X., Schindelholz, M. E., Krumhansl, J. L.,  Vogel, D. J., and Nenoff, T. M.: Near-Zero Power MOF-Based Sensors for NO2 Detection, Advanced Functional Materials, vol. 30, 2006598(1 – 8), 2020.

4.	Kobielsk, P. A., Howarth, A. J., Farha, O. K. amd Nayak, S.: Metal–organic frameworks for heavy metal removal from water, Coordination Chemistry Reviews, vol. 358, 92 – 107, 2018.

5.	Mendes, R. F., Figueira, F., Leite, J. P., Gales, L., and Paz, F. A. A.: Metal–organic frameworks: a future toolbox for biomedicine?, Chem Soc Rev, vol. 49, 9121-9153, 2020.

6.	Villemot, V., Hamel, M., Pansu, R. B., Leray, I., and Bertrand, GHV.: Unravelling the true MOF-5 luminescence, RSC Advances, vol. 10, 18418-18422, 2020.

7.	Hu, Z., Mahin, J., Torrente-Murciano, L.: A MOF-templated approach for designing ruthenium-cesium catalysts for hydrogen generation from ammonia, International Journal of Hydrogen Energy, vol. 44, 30108-30118, 2019.

8.	Qiu, B., Yang, C., Guo, W., Xu, Y., Liang, Z., Ma, D, and Zou, R.: Highly dispersed Co-based Fischer–Tropsch synthesis catalysts from metal–organic frameworks, Journal of Materials Chemistry A, vol. 5, 8081-8086, 2017.

9.	Sun, X., Olivos-Suarez, A. I., Osadchii, D., Romero, M. J. V., Kapteijn, F. and Gascon, J.: Single cobalt sites in mesoporous N-doped carbon matrix for selective catalytic hydrogenation of nitroarenes, Journal of Catalysis, vol. 357, 20-28, 2018.

10.	Nasruddin, Zulys, A., Yulia, F., Buhori, A., Muhadzib, N., Ghiyats, M., and Saha, B. B.: Synthesis and characterization of a novel microporous lanthanide based metal-organic framework (MOF) using napthalenedicarboxylate acid, vol. 9(4), 7409-7417, 2020.

11.	Luo, Q-X., Guo, L. P., Yao, S-Y., Bao, J., Liu, Z-T., Liu, Z-W.: Cobalt nanoparticles confined in carbon matrix for probing the size dependence in Fischer-Tropsch synthesis, Journal of Catalysis, vol. 369, 143 – 156, 2019.

12.	Lázaro, I. A.: A Comprehensive Thermogravimetric Analysis Multifaceted Method for the Exact Determination of the Composition of Multifunctional Metal-Organic Framework Materials, European Journal of Inorganic Chemistry, 4284-4294, 2020.

13.	Xu, L, Jiang, Y., Wang, L.: Thermal decomposition of rape straw: Pyrolysis modeling and kinetic study via particle swarm optimization, Energy Conversion and Management, vol. 146, 124-133, 2017.

14.	Kennedy J, Eberhart R. Particle swarm optimization. IEEEProc. ICNN’95 - Int. Conf. Neural Networks, 1942-1948, 1995.
