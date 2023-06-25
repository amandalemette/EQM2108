## EQM 2108 – MODELAGEM E SIMULAÇÃO DE PROCESSOS NA ENGENHARIA QUÍMICA

### PROFESSORA: AMANDA LEMETTE

### ALUNA: LORENA SATLER PENA

### TRABALHO FINAL DE DISCIPLINA

## TÍTULO: MODELAGEM DINÂMICA DE UM TROCADOR DE CALOR DE TUBO DUPLO

### 1. OBJETIVO
Criar um modelo dinâmico  de um trocador de calor de duplo tubo para prever como as alterações das variáveis do sistema podem afetar as propriedades de saída.


### 2. INTRODUÇÃO

Nas indústrias de processo, os trocadores de calor são equipamentos essenciais projetados para implementar a troca de calor entre dois fluidos ou mais, sujeitos a diferentes temperaturas. Eles são classificados por seu design, número de fluidos e tipo de fluxo.

Em alguns tipos de trocadores de calor, os dois fluidos são separados por uma parede ou membrana e a transferência de calor ocorre tanto por convecção quanto por condução. Em outro tipo menos comum de trocador, os dois fluidos entram fisicamente em contato um com o outro à medida que ocorre a transferência de calor.

O trocador de calor de tubo duplo é o tipo mais simples de trocador de calor e pode operar com fluxo cocorrente (Figura 1) ou contracorrente (Figura 2). O projeto consiste em um único tubo pequeno (lado do tubo) dentro de um maior (lado do casco).

* Um trocador de calor cocorrente é mais usado quando deseja-se que os fluxos de saída deixem o trocador na mesma temperatura.

* Um trocador de calor em contracorrente permite uma transferência de energia mais eficiente.

![Captura de Tela 2023-06-25 às 17 55 00](https://github.com/amandalemette/EQM2108/assets/135286174/127ea6b1-c16b-4e11-bd58-18abb4aad3fc)

### 3. Modelagem com EDO's

O modelo apresentado usa equações diferenciais ordinárias (ODEs) para descrever o processo e fornecer os gráficos das temperaturas de saída versus tempo. 

Para cada unidade, Δz, e cada fluido naquela unidade do trocador de calor, é necessária uma equação de aproximação. 

### 3.1. Balanço Energético

### 3.1.1. Balanço de Energia do Fluido do Lado do Tubo.

*Taxa de acúmulo de energia térmica no fluido do lado do tubo = Taxa de entrada de energia - Taxa de saída de energia - Calor transferido do lado do casco*

* O termo à esquerda no balanço de energia da equação abaixo é a quantidade de energia
térmica que se acumula no fluido do lado do tubo e causa uma mudança em sua temperatura de saída.

* Os termos do lado direito do balanço de energia descrevem a energia térmica do fluido que entra, do fluido que sai e a quantidade de transferência de calor do fluido do lado do casco. No termo para transferência de calor do lado do casco, as temperaturas são as temperaturas das correntes de saída.

As temperaturas de saída mudarão conforme você estiver operando em co-corrente ou contra-corrente. O balanço de energia é escrito como:


$$
mc_{p,t}\frac{dT_{t,out}}{dt} = \rho c_{p,t} F_{t,in} T_{t,in} - \rho c_{p,t} F_{t,out} T_{t,out} - \frac{kA_i}{\Delta z} (T_{t,out} - T_{s,out})
$$

### 3.1.2. Balanço de Energia do Fluido do Lado do Casco.

*Taxa de acúmulo de energia térmica no fluido do lado do tubo = Taxa de entrada de energia - Taxa de saída de energia - Calor transferido para o fluido do lado do tubo - Taxa de perda de calor para o ambiente* 

* O termo mais à esquerda no balanço de energia acima é a quantidade de energia térmica que se acumula no fluido do lado do casco e causa uma mudança em sua temperatura de saída.
* Os termos do lado direito do balanço de energia acima descrevem a energia térmica do fluido que entra e do fluido que sai, a transferência de calor para o fluido do lado do tubo e também o calor perdido por convecção para o ambiente.

$$
mc_{r,s}\frac{dT_{s,out}}{dt} = \rho c_{p,s} F_{s,in} T_{s,in} - \rho c_{p,s} F_{s,out} T_{s,out} - \frac{kA_o}{\Delta z} (T_{s,out} - T_{s,out})-hA(T_s - T_∞)
$$

*onde,*

$m$ = massa do fluido $= V \rho = \rho A_{transversal} \Delta z $

$c_{p} $= capacidade calorífica do fluido à pressão constante

$T$ = Temperatura

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


4. Hipóteses

Existem considerações e simplificações que você pode fazer para resolver os balanços diferenciais de energia. A validade dessas suposições depende da precisão de um modelo que você precisa.

Como existem muitas variáveis independentes em um trocador de calor, é preciso considerar algumas hipóteses nesta modelagem e reduzir o número de EDOs necessárias para definir as variáveis do processo. 

* Hipótese 1: Os fluidos possuem propriedades constantes e perfeita transferência de calor através do metal da tubulação. 

$$C_p = a+BT+cT^2+dT^3 = constante$$


* Hipótese 2: A temperatura de saída do fluido do lado do tubo é monitorada por um sensor de temperatura, e a vazão do fluido do lado do casco é controlada por um dispositivo de controle de fluxo acionado. Ambos sensores estão calibrados com 100% de precisão. 

* Hipótese 3: O trocador de calor está bem isolado, logo, perda de calor para o ambiente é desprezada.

$$ℎ𝐴(𝑇𝑠−𝑇∞)=0$$

* Hipótese 4: Serão desconsiderados os atrasos do sensor que mede a temperatura de saída do fluido do lado do tubo e o dispositivo de controle de vazão do fluido do lado do casco. 

* Hipótese 5: A resposta dinâmica do sistema do atuador para a válvula de controle é rápida. 

* Hipótese 6: O trocador de calor está perfeitamente isolado e ocorre condução perfeita através do metal do fluido do lado do casco para o fluido do lado do tubo, e essa condução é descrita pelo coeficiente de transferência de calor, k, entre os dois fluidos.

* Hipótese 7: Em vez de tomar a derivada parcial ao longo do comprimento em
relação à temperatura, o tubo foi dividido em segmentos diferenciais, Δz.  Idealmente, Δz é uma seção transversal infinitamente pequena do comprimento do trocador de calor, logo, podemos tomar a temperatura em Δz como sendo a temperatura de saída das correntes quente e fria para os Δ𝑧's, respectivamente.

* Hipótese 8: Vamos assumir que através deste segmento diferencial, a temperatura do líquido que sai do segmento é a mesma que a temperatura do líquido dentro do segmento. Como estamos assumindo a mesma temperatura para os fluxos de saída e para o interior do segmento, a escolha do comprimento desses Δz ajuda a ditar a precisão da solução.

A figura abaixo mostra um exemplo de simplificação onde o trocador de calor é dividido em três segmentos:

![Captura de Tela 2023-06-25 às 17 59 06](https://github.com/amandalemette/EQM2108/assets/135286174/d6ee24d1-1a71-4917-9a77-ceb457dd35a7)

### 6. Estudo de caso

Resfriar um fluido a 330 K em um trocador de calor de tubo duplo em contracorrente com água fria entrando a 250 K como fluido refrigerante.

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

$F_s$=0,1 m3/s (vazão volumétrica do fluido do lado do casco)

Informações sobre o fluido do lado do Tubo (solução)

$C_{pt}$ =1200 J/kg*K (capacidade de calor do fluido do lado do tubo)

$T_{0t}$ =330 K (temperatura de entrada do fluido do lado do tubo)

$A_t$ = 0,0314 m (área da seção transversal onde o fluido do lado do tubo está presente)

$ρ_t$ =1030 kg/m^3 (densidade do fluido do lado do tubo)

$F_t$ =0,2 m3/s (vazão volumétrica do fluido do lado do tubo)

###8. Discussões 

A modelagem dinâmica de um trocador de calor permite que os engenheiros tenham mais controle no futuro, crie um modelo para determinar quando esse processo entrará em estado estacionário e faça um gráfico dos perfis de temperatura dos fluxos de saída.



