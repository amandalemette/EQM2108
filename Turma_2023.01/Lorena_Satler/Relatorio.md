### EQM 2108 – MODELAGEM E SIMULAÇÃO DE PROCESSOS NA ENGENHARIA QUÍMICA

### TRABALHO FINAL DE DISCIPLINA

ALUNA: LORENA SATLER PENA

PROFESSORA: AMANDA LEMETTE

## TÍTULO: MODELAGEM DINÂMICA DE UM TROCADOR DE CALOR DE TUBO DUPLO

### 1. Objetivo
Validar um modelo dinâmico de um trocador de calor de duplo tubo para prever como as alterações das variáveis do sistema podem afetar as propriedades dos fluidos de saída.


### 2. Introdução

Nas indústrias de processo, os trocadores de calor são equipamentos essenciais projetados para implementar a troca de calor entre dois fluidos ou mais, sujeitos a diferentes temperaturas. Eles são classificados por seu design, número de fluidos e tipo de fluxo.

Em alguns tipos de trocadores de calor, os dois fluidos são separados por uma parede ou membrana e a transferência de calor ocorre tanto por convecção quanto por condução. Em outro tipo menos comum de trocador, os dois fluidos entram fisicamente em contato um com o outro à medida que ocorre a transferência de calor.

O trocador de calor de tubo duplo é o tipo mais simples de trocador de calor e pode operar com fluxo cocorrente (Figura 1) ou contracorrente (Figura 2). O projeto consiste em um único tubo pequeno (lado do tubo) dentro de um maior (lado do casco).

* Um trocador de calor cocorrente é mais usado quando deseja-se que os fluxos de saída deixem o trocador na mesma temperatura.

* Um trocador de calor em contracorrente permite uma transferência de energia mais eficiente.

![Captura de Tela 2023-06-25 às 17 55 00](https://github.com/amandalemette/EQM2108/assets/135286174/127ea6b1-c16b-4e11-bd58-18abb4aad3fc) 

### 3. Metodologia 

O modelo apresentado aplica equações diferenciais ordinárias (ODEs) para descrever o processo e fornecer os gráficos das temperaturas de saída versus tempo. 

O trocador de calor foi dividido em três segmentos diferenciais (Δz), em vez de tomar a derivada parcial ao longo do comprimento em relação à temperatura. Para cada unidade de Δz, e cada fluido naquela unidade do trocador de calor, foi necessária uma equação de aproximação. 

### 3.1. Balanço Energético - Modelagem com EDO's

### 3.1.1. Balanço de Energia do Fluido do Lado do Tubo.

*Taxa de acúmulo de energia térmica no fluido do lado do tubo = Taxa de entrada de energia - Taxa de saída de energia - Calor transferido do lado do casco*

* O termo à esquerda no balanço de energia é a quantidade de energia térmica que se acumula no fluido do lado do tubo e causa uma mudança em sua temperatura de saída.

* Os termos do lado direito do balanço de energia descrevem a energia térmica do fluido que entra, do fluido que sai e a quantidade de transferência de calor do fluido do lado do casco. No termo para transferência de calor do lado do casco, as temperaturas são as temperaturas das correntes de saída. 

O balanço de energia do lado do tubo pode ser descrito como:


$$
mc_{p,t}\frac{dT_{t,out}}{dt} = \rho c_{p,t} F_{t,in} T_{t,in} - \rho c_{p,t} F_{t,out} T_{t,out} - \frac{kA_i}{\Delta z} (T_{t,out} - T_{s,out})
$$ 

### 3.1.2. Balanço de Energia do Fluido do Lado do Casco.

*Taxa de acúmulo de energia térmica no fluido do lado do tubo = Taxa de entrada de energia - Taxa de saída de energia - Calor transferido para o fluido do lado do tubo - Taxa de perda de calor para o ambiente* 

* O termo à esquerda no balanço de energia compreende a quantidade de energia térmica que se acumula no fluido do lado do casco e causa uma mudança em sua temperatura de saída.
* Os termos à direita descrevem a energia térmica do fluido que entra e do fluido que sai, a transferência de calor para o fluido do lado do tubo e também o calor dissipado por convecção para o ambiente.

$$
mc_{r,s}\frac{dT_{s,out}}{dt} = \rho c_{p,s} F_{s,in} T_{s,in} - \rho c_{p,s} F_{s,out} T_{s,out} - \frac{kA_o}{\Delta z} (T_{s,out} - T_{t,out})-hA(T_s - T_∞)
$$

*onde,*


$m$ = massa do fluido $= V \rho = \rho A_{transversal} \Delta z $

$c_{p} $= capacidade calorífica do fluido à pressão constante

$T$ = temperatura

$t$ = tempo

$k$ = coeficiente de transferência de calor condutivo

$A$ = área da superfície do tubo que o fluido entra em contato

$\Delta z$ = comprimento do tubo

$\rho$ = densidade do fluido

$F$ = vazão volumétrica do fluido

$h$ = coeficiente de transferência de calor por convecção para o ar


*E os subscritos denotam:*

$t$ - fluido do lado do tubo

$out$ - saída

$in$ - entrada

$i$ - dentro

$s$ - fluido do lado do casco

$∞$ - ar

$o$- fora do tubo


### 4. Hipóteses

Como existem muitas variáveis independentes em um trocador de calor, é justificado considerar algumas hipóteses nesta modelagem, reduzindo o número de EDOs necessárias para definir as variáveis do processo. 

* Hipótese 1: Os fluidos possuem propriedades constantes e perfeita transferência de calor através do metal da tubulação. 

$$C_p = a+BT+cT^2+dT^3 = constante$$

* Hipótese 2: Serão desconsiderados os atrasos do sensor que mede a temperatura de saída do fluido do lado do tubo e o dispositivo de controle de vazão do fluido do lado do casco. 

* Hipótese 3: A resposta dinâmica do sistema do atuador para a válvula de controle é instantânea. 

* Hipótese 4: O trocador de calor está perfeitamente isolado, logo, perda de calor para o ambiente é desprezada e ocorre condução perfeita através do metal do fluido do lado do casco para o fluido do lado do tubo. Essa condução é descrita pelo coeficiente de transferência de calor, k, entre os dois fluidos.

$$ℎ𝐴(𝑇𝑠−𝑇∞)=0$$

* Hipótese 5: Em vez de tomar a derivada parcial ao longo do comprimento em relação à temperatura, o tubo foi dividido em segmentos diferenciais, Δz.  Idealmente, Δz é uma seção transversal infinitamente pequena do comprimento do trocador de calor, logo, podemos tomar a temperatura em Δz como sendo a temperatura de saída das correntes quente e fria para os Δ𝑧's, respectivamente.

* Hipótese 6: Assumir que através deste segmento diferencial (Δz), a temperatura do líquido que sai do segmento é a mesma que a temperatura do líquido dentro do segmento. 

A Figura 3 abaixo mostra um exemplo de simplificação onde o trocador de calor é dividido em três segmentos:

![Captura de Tela 2023-06-25 às 17 59 06](https://github.com/amandalemette/EQM2108/assets/135286174/d6ee24d1-1a71-4917-9a77-ceb457dd35a7)
Figura 3: Trocador de calor dividido em três segmentos diferenciais (Δz)

### 5. Estudo de caso

Resfriar rápida e eficientemente um fluido a 330 K em um trocador de calor de tubo duplo em contracorrente com água fria entrando a 250 K como fluido refrigerante. 

Criar um modelo para determinar quando esse processo entrará em equilírio. 

*Dados:*

$T_{air}$=296,15 K (Temp. do Ar)

$r_i$=0,1 m (Diâmetro interno do tubo interno)

$r_{oi}$=0,12 m (Diâmetro externo do tubo Interno)

$r_{oe}$=0,15 m (Diâmetro externo do tubo externo)

$Δz$=1 m (Incremento de comprimento)

$k$ = 450000 W/m2*K (coeficiente de transferência de calor entre os fluidos do lado do casco e do tubo)

*Informações sobre o fluido do lado do casco (água fria)*

$C_{ps}$ = 4185 J/kg*K (capacidade de calor do fluido do lado do casco)

$T_{0s}$ =250 K (temperatura de entrada do fluido do lado do casco)

$A_s$ = 0,02543 m (área da seção transversal onde o fluido do lado do casco está presente)

$ρ_s$=1000 kg/m^3 (densidade do fluido do lado do casco)

$F_s$=0,1 mˆ3/s (vazão volumétrica do fluido do lado do casco)

*Informações sobre o fluido do lado do Tubo (solução)*

$C_{pt}$ =1200 J/kg*K (capacidade de calor do fluido do lado do tubo)

$T_{0t}$ =330 K (temperatura de entrada do fluido do lado do tubo)

$A_t$ = 0,0314 m (área da seção transversal onde o fluido do lado do tubo está presente)

$ρ_t$ =1030 kg/m^3 (densidade do fluido do lado do tubo)

$F_t$ =0,2 m3/s (vazão volumétrica do fluido do lado do tubo)

### 6. Resultados

De acordo com gráfico gerado, é possível observar que a troca térmica é efetiva e acontece muito rapidamente, em t<0.0001s. Contudo, faz sentido físico.

Considerando o problema proposto, o perfil de temperatura de saída do fluido de resfriamento do lado do casco (Figura 4) converge de forma repentina e para um valor mais distante da temperatura inicial de entrada (T0s = 250K) em relação ao fluido do lado do tubo, porque possui menos massa por unidade de volume, entrando no estado estacionário em t=0.0001s com a temperatura de saída igual a 313K. Já o perfil de temperatura do fluido de interesse do lado do tubo (Figura 4) teve a variação mais amena, entrando no estado estacionário em t=0.004s com a temperatura de saída igual a 280K. 

![Captura de Tela 2023-06-26 às 10 53 05](https://github.com/amandalemette/EQM2108/assets/135286174/fad3fba2-b177-4586-8f00-d4e15191c82d)
Figura 4: Perfis de Temperatura no Casco e no Tubo Ft = 0.2m3/s, Fs = 0.1m3/s

Fazendo uma análise mais cuidadosa em relação à variação das vazões volumétricas do processo, ao fixarmos a vazão no tubo de acordo com a fornecida no problema, Ft = 0.2 m3/s, e aumentarmos a vazão do lado do casco para Fs = 0,5 m3/s, a temperatura de saída do lado do casco (Ts_out) diminui, fazendo com que as temperaturas saída no casco e no tubo entrem em estado estacionário quando Ts_out = Tt_out = 261K. Por outro lado, se diminuirmos a a vazão do lado do casco para Fs = 0,05 m3/s, Ts_out aumenta muito e extrapolando a temperatura de entrada do fluido quente. Se fixarmos a vazão do lado do casco fornecida no probelma proposto (Fs = 0.1 m3/s) e diminuirmos a vazão do lado do tubo pra Ft = 0,1 m3/s, ocorre troca térmica satisfatória, onde Ts_out = 303 K e Tt_out = 264 K. Contudo, se aumentarmos Ft = 1,0 m3/s, a troca térmica não é 
Ft = 1,0, não ocorre troca térmica efetiva, pois o fluido quente perde pouco calor e sai com Tt_out = 314K.

Ademais, como foi assuminda a mesma temperatura para os fluxos de saída e para o interior do segmento, a escolha do comprimento desses Δz ajuda a ditar a precisão desse modelo. 

### 7. Conclusões

Como os trocadores de calor têm uma grande variedade de aplicações, especialmente em processos químicos, ar condicionado e refrigeração, o controle do sistema é essencial para a otimização e a realização de previsões dos processos. 

O modelo proposto cumpre o objetivo do trabalho e tem sentido físico, uma vez que é capaz de resfriar o fluido de interese (T0t = 330 K e Tt_out = 280K), entretanto, se aplica para o comprimento do trocador de Δz = 1 e o número de divisões N = 3. Além disso, foi observado que a variação das vazões volumétricas de processo afetam diretamente a eficiencia da troca térmica e devem ser escolhidas de acordo com as temperaturas de saída de interesse. 

A modelagem dinâmica de um trocador de calor de tubo duplo permite que os engenheiros prevejam, por exemplo, como as alterações das variáveis do processo podem afetar as propriedades dos fluidos de saída e determinar quando o sistema entrará em estado estacionário.

Existem considerações e simplificações apropriadas para diminuir o número de EDO's e resolver os balanços diferenciais de energia, devido ao número notável de variáveis independentes a serem contabilizadas em um modelo de um trocador de calor, no entanto, a validade dessas suposições depende da precisão do modelo desejado.

### 8. Referências

1. Libre Texts Engineering - Acessado em 01 de junho de 2023: https://eng.libretexts.org/Bookshelves/Industrial_and_Systems_Engineering/Chemical_Process_Dynamics_and_Controls_(Woolf)/06%3A_Modeling_Case_Studies/6.06%3A_ODE_and_Excel_model_of_a_Heat_Exchanger


2. Aula 23 - Trocadores de Calor - Professor Washington Izarrabal (UFJF) - Acessado em 24 de junho de 2023: https://www.ufjf.br/washington_irrazabal/files/2014/05/Aula-23_Trocadores-de-Calor.pdf

