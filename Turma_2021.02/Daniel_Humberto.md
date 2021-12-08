EQM 2108 – MODELAGEM E SIMULAÇÃO DE PROCESSOS NA ENGENHARIA QUÍMICA
PROF: AMANDA EMETTE
ALUNO: DANIEL P. HUMBERTO
TRABALHO FINAL DE DISCIPLINA

Título: ESTIMAÇÃO DE PARÂMETROS CINÉTICOS DA DECOMPOSIÇÃO TÉRMICA DE REDE METALORGÂNICA UTILIZANDO OTIMIZAÇÃO POR ENXAME DE PARTÍCULAS

# 1.	Resumo

O presente trabalho propõe um modelo cinético para estimar a constante cinética, energia de ativação e ordem aparente de reação da decomposição térmica de uma rede organometálica, através da otimização por enxame de partículas (Particle Swarm Optmization). O banco de dados foi gerado experimentalmente com o ensaio de análise termogravimétrica até 600°C. A amostra sofre perda severa de massa entre 400 e 520°C.

# 2.	Introdução
  
Estruturas organometálicas (MOFs) são um tipo de material cristalino microporoso construído a partir de metais e ligantes orgânicos capazes de formar uma rede com área superficial e volume de poros elevados[1].  MOFs têm atraído atenção na catálise devido às suas estruturas especiais e propriedades funcionais. Este tipo de material possui amplo campo de aplicação, como separação de gases [2], detecção química [3], tratamento de água [4], biomedicina [5], luminescência [6] e catálise heterogênea [7]. No entanto, ao contrário das zeólitas, a maioria das MOFs não apresenta estabilidade térmica em condições de alta temperatura ou pressão, o que dificulta seu uso como catalisador. Apesar disso, MOFs podem servir como um material de sacrifício para preparar compostos à base de carbono, mais estáveis termicamente e em condições de serem aplicados nos processos catalíticos[8]. Após a exposição a temperaturas elevadas, sob atmosfera inerte, o ligante orgânico é transformado em uma matriz de carbono, na qual a fase metálica permanece encapsulada e dispersa[9].
A análise termogravimétrica é uma ferramenta bastante utilizada para caracterização de materiais, incluindo MOFs [10, 11]. Essa técnica monitora a mudança de peso do material em função da temperatura ao longo do tempo [12].
A taxa de reação de decomposição no ensaio de TGA pode ser descrita como a equação abaixo:

dX/dt=-k.X^n

onde X é a massa da amostra, t é o tempo, k é a constante cinética e n a ordem de reação. Esta expressão pode ser relacionada com outra:

X=m_0-m_0  .(w/w_0 )

onde m0 é a massa inicial, w0 é a perda de massa inicial e w é a perda de massa num tempo qualquer. Para o tempo final, w = w∞, que é a perda máxima ao final da análise. e X = 0. Então, a derivada de X com relação ao tempo é:

dX/dt=-m_0/w_∞ .X^n

Logo:

(d(w/w_∞ ))/dt=k.m_0^(n-1).(1-w/w_∞ ).n

Se f = w/w_∞ , então chegamos a:

df/dt=k_0.m_0^(n-1).(1-f)^n.exp⁡(-E_a/RT)

onde k segue a equação de Arrhenius em relação à temperatura.
A última equação possui informações que podem ser retiradas dos dados de TGA, como df/dt e f.

A otimização por enxame de partículas (PSO – Particle Swarm Optimiztion) é um método heurístico de pesquisa global baseada no comportamento natural das partículas de uma população, inspirada no comportamento coletivo de um bando de pássaros ou cardume de peixes. O algoritmo usa o modelo de busca para velocidade e posição, onde um número de partículas compõe uma população e a posição de cada partícula representa uma solução em potencial no espaço. Já a velocidade da partícula é usada para atualizar a posição e a função objetivo “fitness” é usada para avaliar a posição das partículas. No início da busca, velocidade e posição de cada partícula na população são inicializadas e, em seguida, ajustadas por iterações[13].
em 1995, foram propostas as equações de velocidade e posição para a técnica [14]:

v_(i,d)^(k+1)= v_(i,d)^k+ c_1.r_1  .(p_(i,d)^k- x_(i,d)^k )+  c_2.r_2  .(p_(global,d)^k- x_(i,d)^k )
x_(i,d)^(k+1)= x_(i,d)^k+v_(i,d)^(k+1)  

onde v e x são, respectivamente, velocidade e posição no espaço de busca da partícula i,  d é a direção de busca, k é o número de iteração, c1 e c2 são duas constantes positivas chamadas de parâmetro cognitivo e social, r1 e r2 são números aleatórios com distribuição uniforme no intervalo de 0 a 1, sempre diferentes para cada direção, partícula e iteração. pi é o melhor ponto encontrado pela partícula i e pglobal é o melhor valor encontrado por todo o enxame.
Portanto, o presente trabalho tem por objetivo, estimar a energia de ativação, a constante cinética e a ordem de reação da decomposição da MOF utilizando a técnica de PSO sobre os dados de TGA, gerados experimentalmente.

# 3. Metodologia

A análise experimental foi realizada no analisador térmico Shimadzu, modelo DTG-60, com rampa de aquecimento de 5°C/min até 600°C. A figura 1 mostra o gráfico construído durante o ensaio:

"https://github.com/amandalemette/EQM2108/blob/41cd50b072f687666f5945788efad9e6336ee0fd/Turma_2021.02/Imagens/Gr%C3%A1fico_TGA_Trabalho_Final_Daniel%20Humberto.jpg?raw=true"

Analisando a curva de DTG, em azul, nota-se que a decomposição do material é uma reação exotérmica, pois o pico da curva está para cima, com fluxo térmico positivo. Há dois picos e descontinuidades na curva, indicando reações diferentes ao longo do processo, indicando uma complexidade para a simulação dos dados experimentais. O gráfico foi construído com base nos dados obtidos da reação, que são tempo, temperatura, DTG e massa da amostra, sendo estas informações o banco de dados a ser usado no enxame de partículas.
Os dados coletados do ensaio de TGA possuem, originalmente, uma variação muito pequena no tempo (1 segundo) e há elementos ruidosos no conjunto. 
Utilizando o banco de dados sem tratá-lo, o gráfico de variação da massa com o tempo apresentaria descontinuidades e oscilações por haver muitos ruídos atrelados aos dados e isto não é interessante para o trabalho. Portanto, para suavizar a curva, escolheu-se trabalhar com um intervalo de tempo maior (minutos).
A curva de t x f mostra o comportamento da variação de massa ao longo do tempo:

"https://github.com/amandalemette/EQM2108/blob/c44bc2798835c1435568fe9db4a2ae83c5a84768/Turma_2021.02/Imagens/Gr%C3%A1fico_txf_Trabalho_Final_Daniel%20Humberto.jpg?raw=true"
