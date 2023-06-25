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

O equacionamento dessa coluna será feito levando em consideração algumas suposições principais:
1. Não há acúmulo explícito de calor em nenhum dos estágios de equilíbrio da coluna; as entalpias serão calculadas algébricamente a cada iteração
    1. Isso implica que todo o calor inserido na coluna pelo reboiler será utilizado apenas para evaporar a fase líquida presente, o que é razoável pois, na prática, a temperatura de ebulição no reboiler não varia muito e nem muito rapidamente.
2. Toda a fase de vapor que sobe do prato n ao prato n+1 condensa e transfere toda o seu calor para o líquido, evaporando-o.
    1. Essa suposição foi feita de tal forma que o ponto de orvalho não seja relevante na realização do equilíbrio líquido-vapor de cada prato, já que todo o vapor condensa. Na prática, sabe-se que isso não acontece, mas a aproximação é válida.
3. Os estágios de equilíbrio foram considerados teóricos nessa simulação.
    1. Existe um termo de eficiência de cada estágio, que serve para os pratos da coluna, chamado de eficiência de Murphree ($\nu$). Na realização do equilíbrio vapor-líquido, esse termo pode ser utilizado para demonstrar que os pratos não chegam ao equilíbrio teórico calculado, refletindo o que acontece com o processo na prática. A desconsideração desse termo ($\nu = 1$) afeta principalmente a velocidade de resolução da destilação, acelerando-a mais do que realisticamente possível (e.g. a estabilidade é alcançada mais rapidamente, os níveis de pureza permanecem por mais tempo, a produção aumenta).
4. O nível do tanque condensador é mantido constante
    1. Isso significa que a vazão de refluxo $R$ irá variar com o tempo, de acordo com a vazão de destilado $D$ e de vapor do prato de topo $V_{n_T}$ o que tem diversas implicações nos equilíbrios dentro da coluna.

### Balanço molar geral

Assim como em qualquer outro balanço molar, a fórmula geral é:

### $$({Acumulo}) = ({Entrada}) - ({Saida})$$

1. Etapa de purificação: durante a fase de purificação, as únicas variações existentes ocorrem nos pratos dentro da coluna.

Uma boa forma de entender o balanço molar dentro da coluna de destilação é, primeiramente, visualizar dois valores em cada prato para a entrada ao invés de um só, onde o primeiro valor diz respeito ao líquido que cai do prato acima e o segundo ao vapor que sobe do prato abaixo.
O mesmo pode ser feito para a saída, com dois valores existentes, um dizendo respeito ao líquido que cai do prato atual para o prato abaixo e outro, à quantidade evaporada do prato atual, que segue para o prato acima. 

### $$({Acúmulo prato}) = [({Entrada\space liq}) + ({Entrada\space vap})] - [({Saida\space liq}) + ({Saida\space vap})]$$

Em termos mais específicos e se utilizando da ordenação de pratos do esquema mostrado:

### $$\frac{dm_n}{dt} = \dot{m}\_n = L_{n+1} + V_{n-1} - L_n - V_n$$

Esses pratos também podem ser chamados de estágios de equilíbrio, pois são neles que o equilíbrio Vapor-Líquido ocorre e o produto pode ser purificado através da exploração do seu coeficiente de partição.
Para os pratos do topo e do fundo, antes do reboiler e do condensador, alguns termos mudam devido à nomenclatura utilizada, como se pode ver no esquema apresentado:

* Para o prato do fundo ($n = 1$): $V_{n-1} = V_B$

### $$\dot{m}\_1 = L_{2} + V_{B} - L_1 - V_1$$

* Para o prato do topo ($n = n_T$): $L_{n+1} = R$

### $$\dot{m}\_{n_T} = R + V_{n_T-1} - L_{n_T} - V_{n_T}$$

O reboiler, no fundo da coluna, e o tanque condensador, no topo, também podem ser considerados estágios de equilíbrio, porém o seu balanço é um pouco diferente:

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

### Balanço molar por componente

Para realizar o balanço molar para cada componente presente em cada prato, as quantidades de líquido e vapor trocadas entre os pratos precisam ser multiplicadas pela fração molar do componente em cada fase (y para a fase vapor, x para a fase líquida).
Dessa forma, o balanço diz respeito apenas ao número de mols trocados do componente em questão. Sendo assim:

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
### $$V_{n_T} = \frac{RH^L_{D} + Q_R -}{H^V{n_T}}$$

* Para o condensador: O balanço entálpico não é necessário para o condensador, pois não é preciso (e nem possível) calcular a vazão de vapor do condensador, visto que $V_D = 0$.
