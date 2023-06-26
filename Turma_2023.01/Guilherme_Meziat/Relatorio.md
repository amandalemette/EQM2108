<style>
img
{
    display:block;
    float:none;
    margin-left:auto;
    margin-right:auto;
    width:60%;
}
</style>


# Separação de Frações Derivadas do Petróleo por Destilação em Batelada: um Estudo de Caso

A destilação é um processo que pode ser feito de diversas formas, explorando uma propriedade das substâncias a serem separadas: o coeficiente de partição.
Esse coeficiente depende majoritariamente da pressão de vapor (ou de saturação) de cada substância e das suas atividades químicas na temperatura de ebulição.
Nesse caso, o processo será realizado através de uma coluna de destilação fracionada, conforme esquema apresentado abaixo:

![image](https://github.com/amandalemette/EQM2108/assets/11985514/611bdd51-9f36-4020-ae57-ca61c46cb36c)

A ordenação de pratos dentro dessa figura será a ordem utilizada ao longo do relatório.

Esse tipo de coluna é utilizado para destilações mais complexas, como a separação de misturas com vários componentes ou com coeficientes de partição muito próximas (ou ambos).
É notável nesse esquema a falta de uma corrente de alimentação para a coluna, o que se deve ao fato do processo de destilação realizado ser em batelada.
Em colunas como essa, uma carga é inicialmente alimentada na coluna antes do começo de sua operação.
Em seguida, espera-se até que a pureza da substância de interesse mais leve chegue ao nível-objetivo que se inicie a produção.

Durante o ciclo de produção, haverá um momento em que a pureza da substância de interesse cairá abaixo do nível desejado, pois o equilíbrio Vapor-Líquido dentro da coluna começará a favorecer o segundo componente mais leve no produto de topo frente ao esgotamento do componente mais leve.
Nessa situação, a produção é interrompida e encerrada para a batelada em questão ou, se houver mais um componente de interesse na solução, performa-se um "corte intermediário", ou seja, a transferência da saída da coluna do tanque de produção para um tanque secundário, até que a composição desse outro componente se torne pura o bastante para o recomeço da produção.

Durante o corte intermediário, o componente mais leve termina de se esgotar e, então, o segundo componente mais leve adquire boa pureza no topo da coluna.
Porém, caso não seja esse o segundo componente de interesse, basta continuar o corte até que esse componente também se esgote e o equilíbrio na fase vapor favoreça o terceiro componente mais leve no topo.
Caso o componente de interesse seja algum outro mais pesado, basta esperar o esgotamento do terceiro componente mais leve e assim por diante.
Geralmente, o tanque secundário que recebe as faixas intermediárias da destilação é um tanque de reciclo para a próxima batelada.

Colunas de destilação em batelada ganharam popularidade pela sua capacidade de produzir substâncias de alta pureza, uma necessidade dos dias de hoje.
Outra vantagem desse método é a capacidade de gerar uma variedade de produtos a partir de uma mesma alimentação com apenas uma coluna.
Dito isso, sua simulação traz diversos desafios adicionais, quando comparada á simulação de uma destilação contínua.
Primeiramente, uma coluna de destilação fracionada em batelada opera constantemente em estado transiente.
Além disso, um ou mais componentes podem desaparecer por completo da coluna, dependendo da necessidade de cortes intermediários, o que torna a simulação extremamente sensível.

## Equacionamento

Uma das primeiras coisas a se perceber para o equacionamento de uma coluna de destilação em batelada é que não existe uma divisão entre a seção de esgotamento e a seção de retificação, visto que não há um prato de alimentação, pois não há alimentação contínua na coluna.
Na suposição de que o prato de alimentação é o reboiler, pode-se dizer que a coluna inteira é uma seção de retificação e que a seção de esgotamento não existe, ou vice-versa para a suposição de que a alimentação é feita no condensador.
Assim, a visualização dos balanços de massa e de energia se torna um pouco mais simples.

O equacionamento dessa coluna será feito levando em consideração algumas suposições, das quais as principais são:
1. Não há acúmulo explícito de calor em nenhum dos estágios de equilíbrio da coluna; as entalpias serão calculadas algébricamente a cada iteração
    1. Isso implica que todo o calor inserido na coluna pelo reboiler será utilizado apenas para evaporar a fase líquida presente, o que é razoável pois, na prática, a temperatura de ebulição no reboiler não varia muito e nem muito rapidamente.
2. Toda a fase de vapor que sobe do prato n ao prato n+1 condensa e transfere toda o seu calor para o líquido, evaporando-o.
    1. Essa suposição foi feita de tal forma que o ponto de orvalho não seja relevante na realização do equilíbrio líquido-vapor de cada prato, já que todo o vapor condensa. Na prática, sabe-se que isso não acontece, mas a aproximação é válida.
3. Os equilíbrios vapor-líquido entre cada prato foram considerados teóricos nessa simulação.
    1. A eficiência de Murphree ($\nu$) esse termo pode ser utilizado para demonstrar que as interações vapor-líquido em cada prato não chegam ao equilíbrio vapor-líquido teórico calculado, refletindo o que acontece com o processo na prática. A desconsideração desse termo ($\nu = 1$) afeta principalmente a velocidade de resolução da destilação, acelerando-a mais do que realisticamente possível (e.g. a estabilidade é alcançada mais rapidamente, os níveis de pureza permanecem por mais tempo, a produção aumenta).
4. O nível do tanque condensador é mantido constante.
    1. Isso significa que a vazão de refluxo $R$ irá variar com o tempo, de acordo com a vazão de destilado $D$ e de vapor do prato de topo $V_{n_T}$ o que tem diversas implicações nos equilíbrios dentro da coluna.
5. As composições iniciais de todos os pratos, do reboiler e do condensador são identicas e iguais à composição de alimentação da coluna.

### Balanço molar geral

Assim como em qualquer outro balanço molar, a fórmula geral é:

### $$({Acumulo}) = ({Entrada}) - ({Saida})$$

1. Etapa de purificação: durante a fase de purificação, as únicas variações existentes ocorrem nos pratos dentro da coluna.

Uma boa forma de entender o balanço molar dentro da coluna de destilação é, primeiramente, visualizar dois valores em cada prato para a entrada ao invés de um só, onde o primeiro valor diz respeito ao líquido que cai do prato acima e o segundo ao vapor que sobe do prato abaixo.
O mesmo pode ser feito para a saída, com dois valores existentes, um dizendo respeito ao líquido que cai do prato atual para o prato abaixo e outro, à quantidade evaporada do prato atual, que segue para o prato acima. 

### $$({Acúmulo prato}) = [({Entrada\space liq}) + ({Entrada\space vap})] - [({Saida\space liq}) + ({Saida\space vap})]$$

Em termos mais específicos e se utilizando da ordenação de pratos do esquema mostrado:

### $$\frac{dm_n}{dt} = \dot{m}\_n = L_{n+1} + V_{n-1} - L_n - V_n$$

Os pratos da coluna são também chamados de estágios de equilíbrio, pois é neles que as interações vapor-líquido acontecem, possibilitando a purificação através da exploração do coeficiente de partição. 
Para os pratos do topo e do fundo, antes do reboiler e do condensador, alguns termos mudam devido à nomenclatura utilizada, como se pode ver no esquema apresentado:

* Para o prato do fundo ($n = 1$): $V_{n-1} = V_B$

### $$\dot{m}\_1 = L_{2} + V_{B} - L_1 - V_1$$

* Para o prato do topo ($n = n_T$): $L_{n+1} = R$

### $$\dot{m}\_{n_T} = R + V_{n_T-1} - L_{n_T} - V_{n_T}$$

O reboiler, no fundo da coluna, e o tanque condensador, no topo, também são estágios de equilíbrio, porém o seu balanço é um pouco diferente:

* Para o reboiler: como não há nenhum estágio inferior ao reboiler, o valor de saída de líquido não existe, bem como não há um valor de vapor de entrada.

![image](https://github.com/amandalemette/EQM2108/assets/11985514/374b26b5-3506-4f9a-9830-8b984fed3008)

### $$\dot{m}_B = L_1 - V_B$$

* Para o condensador: de forma contrária ao reboiler, não há nenhum estágio superior ao condensador, logo os valores de saída de vapor e de entrada de líquido não existem. Além disso, como o nível dele é mantido constante:

![image](https://github.com/amandalemette/EQM2108/assets/11985514/081c8e3e-53df-4b56-8471-f54f32bb8185)

### $$\dot{m}\_D = V_{n_T}-R = 0 \longrightarrow V_{n_T} = R$$

2. Etapa de produção: durante a fase de produção, o produto é retirado pelo topo, logo o balanço de massa do condensador sofre uma alteração.

![image](https://github.com/amandalemette/EQM2108/assets/11985514/4fd3e1e6-8a3b-4333-a463-8aac39d1b3ec)

### $$\dot{m}\_D = V_{n_T}-R-D = 0 \longrightarrow V_{n_T} = R+D$$

Vale notar que, em certos casos, pode existir saída de produto pelo fundo da coluna também, sendo potencialmente vantajoso para a extração dos componentes mais pesados.

As vazões de saída de vapor de cada prato serão definidas mais a frente, durante o balanço energético do processo, a partir da suposição 1.
Já as vazões líquidas podem ser definidas a partir de uma fórmula empírica abaixo para as interações hidráulicas entre os pratos, onde:

* $W_L$ = comprimento do downcomer em polegadas
* $DCOL$ = diâmetro da coluna em polegadas
* $DENSA_n = \sum_{i=1} \rho_{i}\cdot x_{n,i}$ = densidade média do líquido no prato em $\frac{lb}{ft^3}$
* $MWA_n = \sum_{i=1} MM_{i}\cdot x_{n,i}$ = massa molar média do líquido no prato em unidades de massa atômica

### $$L_n = \frac{DENSA_n\cdot W_L\cdot 999\cdot\left(\frac{183.2\cdot m_n\cdot MWA_n}{DENSA_n\cdot DCOL^2}\right)^{1.5}}{MWA_n}$$
A fórmula acima pode ser utilizada também para o cálculo da vazão de refluxo, R.

### Balanço molar por componente

Para realizar o balanço molar para cada componente presente em cada prato, as quantidades de líquido e vapor trocadas entre os pratos precisam ser multiplicadas pela fração molar do componente em cada fase (y para a fase vapor, x para a fase líquida).
Dessa forma, o balanço diz respeito apenas ao número de mols trocados do componente em questão.
A partir da suposição 2, considera-se que esse balanço é feito puramente na parte líquida de cada prato. Sendo assim:

* Para os pratos intermediários:
  
### $$\frac{dm_n,i}{dt} = \dot{m}\_{n,i} = L_{n+1} x_{n+1,i} + V_{n-1} y_{n-1,i} - L_n x_{n,i} - V_n y_{n,i}$$
 
* * Para o prato do fundo ($n = 1$): $y_{n-1,i} = y_{B,i}$

### $$\dot{m}\_{1,i} = L_{2}x_{2,i} + V_{B}y_{B,i} - L_1x_{1,i} - V_1y_{1,i}$$

* * Para o prato do topo ($n = n_T$): $x_{n+1,i} = x_{D,i}$

### $$\dot{m}\_{n_T,i} = Rx_{D,i} + V_{n_T-1}y_{n_T-1,i} - L_{n_T}x_{n_T,i} - V_{n_T}y_{n_T,i}$$

* Para o reboiler:

### $$\dot{m}\_{B,i} = L_1x_{1,i} - V_By_{B,i}$$

* Para o condensador: Aqui, apesar do controle de nível no tanque, evidentemente ainda existe variação molar entre os componentes, caso contrário não seria possível performar a destilação. 

1. Etapa de purificação:

### $$\dot{m}\_{D,i} = V_{n_T} y_{n_T,i}-Rx_{D,i}$$

2. Etapa de produção:

### $$\dot{m}\_{D,i} = V_{n_T} y_{n_T,i}-(R+D)x_{D,i}$$

Dessa forma, as composições líquidas de cada prato sempre serão conhecidas no começo de cada iteração.

O cálculo das composições do vapor de cada prato requer a realização do equilíbrio vapor-líquido. No caso dessa simulação, esse equilíbrio é calculado através de um processo iterativo, onde as variáveis conhecidas são as composições líquidas e a pressão de operação e se quer encontrar a composição e temperatura de bolha.
Considerando que o vapor se comporta como um gás ideal com pressão de saturação definida a partir da equação de antoine, a solução se comporta como uma solução regular de Hildebrand e que os pratos são teóricos conforme suposição 3, as formulações das Leis de Raoult e de Dalton podem ser feitas da seguinte forma:

##### $$P_i = P_t\cdot y_i = P^{Sat}_i\cdot x_i\cdot\gamma_i$$
### $$y_i = \frac{P^{Sat}_i\cdot x_i\cdot\gamma_i}{P_t}$$

Onde:

### $$lnP^{Sat}_i = A_i - \frac{B_i}{T+C_i}
### $$ln\gamma_i = \frac{v_i\cdot(\delta_i - \bar{\delta})^2}{RT}\space ;\quad \bar{\delta} = \frac{\sum_{i=1} x_iv_i\delta_i}{\sum_{i=1} x_iv_i} $$

Nas equações acima, $A_i$, $B_i$ e $C_i$ são os parâmetros de antoine, $v_i$ é o volume molar e $\delta_i$ é a solubilidade do componente i. R é a constante dos gases ideais.

Assim, os seguintes passos devem ser seguidos:

1. Chutar uma temperatura de bolha inicial
2. Calcular o coeficiente de atividade de cada componente através do modelo de Hildebrand
3. Calcular as pressões de saturação de cada componente
4. Calcular e minimizar a variação entre a soma das pressões de saturação e a pressão total, $(\sum P^Sat_i) - P_t$
    1. Se essa variação der próxima o bastante de zero (conforme tolerância adotada arbitrariamente), encerrar processo
    2. Caso contrário, passar para a próxima iteração com novo T, definido a partir de $(\sum P^Sat_i) - P_t$ (foi utilizada a função fsolve da biblioteca scipy.optimize)

Finalizado esse processo, é possível calcular $y_i$ a partir da fórmula anteriormente demonstrada.
Assim, obtêm-se a temperatura e composição de bolha do líquido analisado, lembrando que esse é o cálculo para apenas um dos pratos.

### Balanço energético (ou entálpico)

Para o balanço energético, as quantidades de líquido e vapor trocadas entre os pratos devem ser multiplicados pela entalpia molar de cada fase($H^V$ para a fase vapor e $H^l$ para a fase líquida), de forma que o resultado é o calor total de saída e entrada em cada prato.
Relembrando da suposição 1 feita para o equacionamento da coluna, é mais conveniente iniciar esse balanço falando do reboiler:

* Para o reboiler: nesse balanço, demonstra-se que é possível calcular a saída de vapor do reboiler

##### $$\dot{m}\_B = Q_R + L_1H^L_{1} - V_BH^V_{B} = 0$$
##### $$Q_R + L_1H^L_{1} = V_BH^V_{B}$$
### $$V_B = \frac{Q_R + L_1H^L_{1}}{H^V_{B}}$$
 
* Para o prato do fundo ($n = 1$): $H^V_{n-1} = H^V_{B}$. Rearranjando o balanço, observa-se que $V_1H^V_{1}$ ainda depende diretamente de $Q_R$

##### $$\dot{H}\_1 = L_{2}H^L_{2} + V_{B}H^V_{B} - L_1H^L_{1} - V_1H^V_{1} = 0$$
##### $$L_{2}H^L_{2} + Q_R + L_1H^L_{1} - L_1H^L_{1} - V_1H^V_{1} = 0$$
##### $$L_{2}H^L_{2} + Q_R - V_1H^V_{1} = 0$$
##### $$L_{2}H^L_{2} + Q_R = V_1H^V_{1}$$
### $$V_1 = \frac{L_{2}H^L_{2} + Q_R}{H^V_{1}}$$

* Para o prato $n = 2$: Aqui, parcebe-se que é possível definir $Q_R = V_{n-1}H^V_{n-1} - L_nH^L_{n}$

##### $$\dot{H}\_2 = L_{3}H^L_{3} + V_{1}H^V_{1} - L_2H^L_{2} - V_2H^V_{2} = 0$$
##### $$L_{3}H^L_{3} + Q_R + L_2H^L_{2} - L_2H^L_{2} - V_2H^V_{2} = 0$$
##### $$L_{3}H^L_{3} + Q_R - V_2H^V_{2} = 0$$
##### $$L_{3}H^L_{3} + Q_R = V_2H^V_{2}$$
### $$V_2 = \frac{L_{3}H^L_{3} + Q_R}{H^V_{2}}$$

* Para os pratos intermediários: Seguindo a lógica apresentada anteriormente
  
##### $$\dot{H}\_n = L_{n+1}H^L_{n+1} + V_{n-1}H^V_{n-1} - L_nH^L_n - V_nH^V_n = 0$$
##### $$L_{n+1}H^L_{n+1} + Q_R - V_nH^V_n = 0$$
##### $$L_{n+1}H^L_{n+1} + Q_R = V_nH^V_n$$
### $$V_n = \frac{L_{n+1}H^L_{n+1} + Q_R}{H^V_n}$$

* Para o prato do topo ($n = n_T$): $H^L_{n+1} = H^L_{D}$

##### $$\dot{H}\_{n_T} = RH^L_{D} + V_{n_T-1}H^V_{n_T-1} - L_{n_T}H^L_{n_T} - V_{n_T}H^V{n_T} = 0$$
##### $$RH^L_{D} + Q_R - V_{n_T}H^V{n_T} = 0$$
### $$V_{n_T} = \frac{RH^L_{D} + Q_R}{H^V_{n_T}}$$

* Para o condensador: O balanço entálpico não é necessário para o condensador, pois não é preciso (e nem possível) calcular a vazão de vapor do condensador, visto que $V_D = 0$.

As entalpias molares de cada fase em cada prato podem ser calculadas através da média ponderada das entalpias de cada componente, de forma análoga ao cálculo da densidade, massa molar e solubilidade médias:

### $$H^L_n = \sum_{i=1} H^L_{i}\cdot x_{n,i}\quad //\quad H^V_n = \sum_{i=1} H^V_{i}\cdot y_{n,i}$$

Nessa simulação, o cálculo de $H^v_i$ é feito inicialmente, a partir da temperatura de bolha encontrada na resolução do equilíbrio vapor-líquido:

### $$H^v_i = \int_{T_0}^{T}c_{p,i}dT$$

Onde $c_p$ é uma aproximação cúbica:

### $$c_{p,i} = a_1 + a_2T + a_3T^2 + a_4T^4$$

Nessa fórmula, $T_0$ é considerada como sendo igual a zero.

Uma forma de relacionar as entalpias da fase vapor com a fase líquida é através do calor latente de vaporização, que pode ser expressado com base na pressão de saturação de antoine:
### $$\lambda = RT^2 \frac{B_i}{(T+C_i)^2} \longrightarrow H^L_i = H^V_i - \lambda$$

## O problema em questão

Como dito anteriormente, as colunas de destilação em batelada se tornaram comuns em operações de separação com altos requerimentos de pureza.
O problema atual a ser estudado é a separação de uma mistura ternária de frações muito similares derivadas do petróleo: o ciclohexano(1), o heptano(2) e o tolueno(3).
Algumas informações dessa mistura seguem abaixo:

$F = 30000\space mol$
$z_i = [0.4;0.4;0.2]$
$m_n = 30\space mol\qquad(m_{n,i} = [12, 12, 6]$
$m_D = 1000\space mol\qquad(m_B = m_n\cdot n_t - m_D)$ 
$Q_R = 200\space kcal\space min^{-1}$
$D_{max} = 1.8\space mol\space min^{-1}$ (apenas após o começo da produção)
$x_{1,min} = 0.97$
$DCOL = 0.75\space m$
$P_op = 1\space atm$

### Passo-a-passo

Com o resto das variáveis relevantes definidas na seção de equacionamento da coluna, é possível completar os balanços molar e energético. Como todas as equações abordadas são EDOs, uma abordagem recursiva deve ser tomada para a sua resolução, discretizando uma variação contínua de tempo de destilação.
Sendo assim, utilizando o solve_ivp da biblioteca scipy.integrate, cada iteração deve seguir conforme definido abaixo:

1. Definir composições de cada prato na fase líquida a partir dos dados de número de mols
2. Calcular $L_n$ e a vazão de refluxo $R$ pela fórmula de interações hidráulicas
3. Resolver equilíbrio VL para obter a composição e temperatura de bolha, $y_{n,i}$ e T
4. Calcular as entalpias molares por componente e por fase, $H^V_{n,i}$ e $H^L_{n,i}$
     1. Calcular as entalpias molares por fase $H^V_n$ e $H^L_n$
5. Calcular $V_n$ a partir do balanço de massa energético
6. Calcular $\dot{m}\_{n,i}$ e $\dot{m}\_{n}$
7. Encerrar iteração

Internamente, o scipy.integrate.solve_ivp define um tempo de integração e define as novas composições de cada prato, que são passadas para o começo da função recursiva novamente. O tempo de simulação foi de 10000 minutos, ou aproximadamente uma semana.

## Resultados e Discussão

O código executado ofereceu os seguintes resultados:
```
Ciclo de produção de ciclohexano iniciada: 372.52 min
Ciclo de produção de ciclohexano encerrada após 6276.85 min
Quantidade: 9212.41 mol // Pureza do ciclo atual: 98.68 %
```
Isso significa que a fase de produção durou por volta de quatro dias e meio, finalizando com uma solução de ciclohexano de 98.68%.
Uma das primeiras coisas a se notar é que, apesar de o requerimento mínimo ser de 97% de ciclohexano, a pureza obtida foi significativamente mais alta.
Isso se deve ao fato de a coluna estar em estado transitório constantemente, dificultando o controle da fração de saída de ciclohexano da coluna, que já estava na ascendente no momento em que a condição de pureza mínima foi atingida.
A implicação desse resultado é que opcionalmente pode-se diluir essa fração de ciclohexano até que se alcance o requisito mínimo de pureza, um processo trivial em uma indústria para aumento de rendimento.

### Gráficos

Do código executado, diversas informações puderam ser extraídas, como a quantidade de matéria dentro da coluna, em cada prato, no reboiler, além das frações molares de cada componente em cada prato, no líquido e no vapor. Abaixo segue uma coletânea com todos esses resultados:

![image](https://github.com/amandalemette/EQM2108/assets/11985514/b2df8978-214c-4bf8-9ded-bc12a6b25f3b)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/bcd0f2a6-6e60-4ec4-9785-a1ab73fa09be)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/0f393e9b-dced-4c6d-9812-11a53e4c74e5)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/0934f153-aa6e-4d9c-af77-9a592ecda127)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/081cb011-03ce-4acc-956c-d2d318e40534)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/fb0a7ca1-c4f4-4f65-b824-b64a4bc3e9b4)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/39e0a6a2-cdb8-4992-830b-ff9829d339ed)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/816351dd-d7c7-4aaf-a0f8-75b2f3fbb8c9)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/c9b615b8-3ab6-4a83-9dc1-98daf7e1302b)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/ce112e16-9416-4182-ad95-3eb8f479f0b7)

![image](https://github.com/amandalemette/EQM2108/assets/11985514/7a734f6d-b2de-495d-af3c-44eb2e79d81e)





