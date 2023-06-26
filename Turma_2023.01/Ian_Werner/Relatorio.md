Trabalho final da disciplina EQM2108
## Simulação Bioreator CSTB, batelada e semi-batelada
Aluno: Ian Monteiro Werner
Professora: Amanda Lemette

# Resumo
O presente trabalho teve como objetivo a Simulação de um biorreator em batelada e em semi-batelada de acordo com o exemplo 5.5.2 do livro "Chemical Process Modeliing and Computer Simulation". Esses resultados foram comparados com a simulaçã ofeita em um reator tipo CSTB.
Esse reator visa a produção de etanol a partir de glicose por meio da levedura Sacaromices Cerevisiae. A partir da discussão do próprio autor, é proposto simular em batelada e em semi-batelada a partir do equacionamento proposto pelo livro.


#Introdução

A simulação de um bioreator em semi batelada é uma ferramenta valiosa no campo da engenharia bioquímica e biotecnologia. Essa abordagem envolve a operação do bioreator em um modo misto, combinando características da operação em batelada e contínua.

No bioreator em semi batelada, o processo é dividido em fases distintas. Inicialmente, ocorre uma fase batelada, na qual o meio de cultura e os microrganismos são adicionados ao sistema e a reação bioquímica é iniciada. Essa fase permite um maior controle sobre as condições iniciais e o monitoramento das variáveis-chave, como temperatura, pH e concentração de nutrientes.

Após a fase batelada, o bioreator entra em uma fase de alimentação contínua, na qual um suplemento de nutrientes é adicionado periodicamente para manter as condições ideais para o crescimento dos microrganismos e a produção do produto desejado. Essa estratégia permite um maior controle da cinética da reação e a maximização da produção.

O modo de operação em (semi)batelada é o mais comumente utilizado na indústria de fermentação, visando manter a esterilidade, evitar inibição do produto e/ou substrato e garantir flexibilidade operacional para acompanhar as mudanças nas demandas do mercado. Esta seção discute a produção de etanol em um fermentador de levedura de panificação (baker's yeast) operado nos modos de batelada e semibatelada.

Em suma, a simulação de um bioreator em semi batelada é uma ferramenta poderosa para a engenharia bioquímica, permitindo o estudo e o aprimoramento de processos biotecnológicos com maior controle e eficiência. Essa abordagem contribui para o desenvolvimento de soluções sustentáveis e economicamente viáveis na produção de diversos produtos de interesse, desde medicamentos e alimentos até biocombustíveis e produtos químicos renováveis.

# Metodologia
Vamos simular a operação de um biorreator ideal do tipo CSTB que processa a reação seguinte:

$$
A \longrightarrow P
$$

Vamos considerar:
- reação isotérmica (temperatura constante)
- mistura é ideal 
- alimentação não possue biomassa
- a densidade da alimentação e saída possuem densidade constante e iguais
- vazão de alimentação é igual a vazão de saída
- o meio de cultura só possui um único tipo de biomassa crescendo com um único substrato
-reação de 1a ordem

Modelo:
Balanço de massa
$$
{\frac{d(pV)}{dt}} = Fp - Fp = 0
$$
para uma biorreator:

- Taxa de biomassa entrando do reator = Fxf
- Taxa de biomassa saindo do reator = Fx
- Taxa de geração de biomassa
- acumulo de biomassa no reator = d(Vx)/dt

$$
{\frac{d(Vx)}{dt}} = Fx_f - Fx + Vr_1
$$

sendo r1 a taxa de geração de células
 Dividindo os dois lados por V:
$$
{\frac{d(x)}{dt}} = {\frac{F}{V}}x_f - {\frac{F}{V}}x + r_1
$$
a razão F/V pode ser descrita como tempo espacial do reator D. No entanto, no ramo da engenharia bioquímica, F/V é referido como taxa de diluiçao, provavelmente relacionado a diluição da biomassa no biorreator. Ficando assim:

$$
{\frac{d(x)}{dt}} = D(x_f - x) + r_1
$$

Para o balanço de massa do substrato, faz-se com a taxa de consumo do substrato e manipulando tem-se:

$$
{\frac{d(S)}{dt}} = D(S_f - S) - r_2
$$

A taxa de 1a ordem fica da forma para reação do produto A

$$
r = k\cdot C_A
$$

Sabe-se que a taxa de crescimento de bactérias depende do meio que elas estão. Assim, estipula-se que a taxa de crescimento é função da concentração de subtrato no meio de cultura. Sendo o rendimento do produto P em relação ao reagente A:

$$
r_1 = \mu x
$$
Com rendimento constante: 
$$
Y = {\frac{r_1}{r_2}} = {\frac{massa\ de\ células\ formadas}{massa\ de\ substrato\ consumido}} 
$$

assim a relação entre taxas é:

$$
r_2 = {\frac{r_1}{Y}}
$$
Substituindo no balanço de massa:

$$
{\frac{d(x)}{dt}} = D(x_f - x) + \mu x
$$

$$
{\frac{d(S)}{dt}} = D(S_f - S) - {\frac{d(\mu x)}{dY}}
$$

Para esse caso em que a alimentação não contem biomassa (xf = 0):

$$
{\frac{d(x)}{dt}} = (\mu  - D)x
$$

$$
{\frac{d(S)}{dt}} = D(S_f - S) - {\frac{(\mu x)}{Y}}
$$
 
 Como a crescimento específico (u) não é constante e varia com a concentração de substrato, é necessário escolher um modelo para estimar essa correlação. O modelo escolhido para esse trabalho é o de Monod

$$
\mu = {\frac{\mu _mS}{K_m+S}}
$$

sendo \mu_m o crescimento específico máximo e K_m a concentração de substrato limitante quando o crescimento específico é igual a metade do cresciemnto específico máximo. Ambos são parâmetros obtidos experimentalmente e não tem interpretação física direta.  

No entanto o proposto pelo livro é o uso de um reator tipo semi batelada em que etanol é produzido a partir da Saccharomyces cerevisiae com glicose como substrato:
Pra um reator de semi-batelada tem-se a adição de reagente em um reator batelada ao longo do processo em que o volume da solução aumenta com o tempo.

$$
{\frac{d(V)}{dt}} = F
$$

Para função do rendimento:

$$
{\frac{d(x)}{dt}} = \mu x - {\frac{F}{V}}x
$$
E para o substrato S:

$$
{\frac{d(S)}{dt}} = -\sigma x - {\frac{F}{V}}(Sf-S)
$$

E produto P

$$
{\frac{d(P)}{dt}} =\pi x - {\frac{F}{V}}P
$$

Essas equações tem dois novos termos: taxa de consumo de substrato \sigma e taxa de formação de produto \pi

$$
\mu = {\frac{0.408S}{0.22 + S}}e^-0.028P
$$

$$
\sigma = {\frac{\mu}{0.1}}
$$

$$
\pi = {\frac{S}{0.44 + S}}e^-0.015P
$$


#Resultados e Discussão

Para o reator em batelada, observa-se que a reação estabiliza muito rápido em um reator batelada. Dessa forma, o livro sugere que seja em pregado um reator semi-batelada para aumentar a eficiência do processo.

#Conclusão

POr fim, a simulação de bioreatores nos diferentes modos de operação, como Contínuo em Estado Estacionário (CSTB), Batelada e Semibatelada, desempenha um papel fundamental no desenvolvimento e otimização de processos biotecnológicos.

A simulação em CSTB é útil para entender e prever o comportamento de sistemas de fermentação contínua, permitindo a análise das taxas de crescimento microbiano, consumo de substratos e produção de produtos finais. Isso é especialmente importante para processos industriais de longa duração, nos quais é necessário garantir a estabilidade e eficiência do processo ao longo do tempo.

Por sua vez, a simulação em batelada é valiosa para estudar processos em pequena escala, onde há a adição de uma quantidade fixa de inóculo e substrato ao sistema, seguida de um período de fermentação. Esse modo de operação permite uma análise detalhada das cinéticas de crescimento, produção e consumo, além de facilitar a manipulação de variáveis como temperatura, pH e concentração de nutrientes.

Já a simulação em semibatelada combina características dos modos batelada e contínuo, oferecendo maior flexibilidade operacional. Nesse modo, ocorre uma fase inicial de batelada, permitindo um controle preciso das condições iniciais e monitoramento das variáveis-chave. Em seguida, o bioreator transita para a fase de alimentação contínua, onde nutrientes são adicionados periodicamente para manter o crescimento microbiano e a produção de compostos desejados. Essa abordagem visa maximizar a produção e a eficiência do processo.

Cada modo de operação tem suas vantagens e desafios específicos, e a escolha adequada depende das características do processo e dos objetivos de produção. A simulação desempenha um papel crucial ao permitir a avaliação e a comparação desses modos, fornecendo insights valiosos sobre a eficiência, produtividade e viabilidade econômica de cada abordagem.

Em última análise, a simulação de bioreatores em CSTB, batelada e semibatelada impulsiona a pesquisa e o desenvolvimento de processos biotecnológicos, possibilitando a otimização das condições operacionais, a maximização da produção de compostos desejados e a redução de custos e impactos ambientais. Essa abordagem contribui para a busca contínua por soluções sustentáveis e eficientes na produção de produtos químicos, farmacêuticos, alimentos e biocombustíveis.

A simulação de bioreatores nos modos CSTB, batelada e semibatelada desempenha um papel crucial no desenvolvimento e otimização de processos biotecnológicos. A simulação em CSTB é útil para processos contínuos de longa duração, enquanto a simulação em batelada permite análise detalhada das cinéticas. A simulação em semibatelada combina as vantagens dos modos batelada e contínuo, maximizando a produção. Cada modo tem suas vantagens e desafios, e a simulação ajuda a avaliar e comparar essas abordagens. Essa simulação impulsiona a pesquisa e desenvolvimento de processos biotecnológicos, contribuindo para soluções sustentáveis e eficientes na produção de diversos produtos.
