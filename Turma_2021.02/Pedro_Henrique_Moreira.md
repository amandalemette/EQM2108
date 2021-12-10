 Trabalho final da disciplina EQM2108
## Condition monitoring of hydraulic systems
Aluno: Pedro Henrique de Lima Ripper Moreira

Professora: Amanda Lemette

# Resumo
O presente trabalho teve como objetivo a otimização de um modelo de decisão de árvore de classificação aplicado a um banco de dados real obtido da literatura ou sites como o Kaggle. Para tal, foi feito uma avaliação qualitativa dos hiperparâmetros desse modelo na obtenção da melhor acurácia de resposta ao conjunto de dados de treino e teste para evitar problema de "overfitting". Os hiperparâmetros otimizados são max_depth = 9, min_samples_leaf = 2 e min_samples_split = 2. Por final, fez-se uma análise comparativa da acurácia de diversos modelos de classificação utilizando o método de validação cruzada, onde o ExtraTreesClassifier se destacou como tendo o melhor resultado de 0.981 de acurácia.

# Introdução

O monitoramento das condições operacionais de sistemas hidráulicos, como forma de reduzir tempo de inatividade e custo de manutenção, utilizando o método estatístico, vem -se destacando na indústria. Esse método se baseia na análise das condições de operação do sistema nos momentos de falhas, construindo-se um banco de dados histórico suficientemente grande para prever falhas futuras.


Nesse sentido, o modelo de previsão de falhas pode ser anexado a um algoritmo de aprendizagem supervisionada, como por exemplo, scatter-based supervised learning, ou rede neural, tal qual, feedforward artificial neural network, para detectar e classificar falhas futuras e evitá-las. 
 


# Metodologia

O conjunto de dados foi obtido na plataforma Kaggle em https://www.kaggle.com/jjacostupa/condition-monitoring-of-hydraulic-systems, onde aborda a avaliação da condição de um equipamento de teste hidráulico com base em dados de múltiplos sensores. Este equipamento de teste consiste em um circuito primário de trabalho e um circuito de filtração de resfriamento secundário, conectados através de um tanque de óleo. 

O sistema repete ciclicamente os ciclos de carga constante (duração de 60 segundos) e mede os valores do processo, como pressões, fluxo de volume e temperaturas, por meio de sensores que são apresentados na Tabela 1.

Tabela 1: Infomação dos sensores analizados.
| Sensor | PhysicalQuantity           | Unit  | Sampling rate |
|:------:|:--------------------------:|:-----:|:-------------:|
| PS1    | Pressure                   | bar   | 100 Hz        |
| PS2    | Pressure                   | bar   | 100 Hz        |
| PS3    | Pressure                   | bar   | 100 Hz        |
| PS4    | Pressure                   | bar   | 100 Hz        |
| PS5    | Pressure                   | bar   | 100 Hz        |
| PS6    | Pressure                   | bar   | 100 Hz        |
| EPS1   | MotorPower                 | W     | 100 Hz        |
| FS1    | VolumeFlow                 | l/min | 10 Hz         |
| FS2    | VolumeFlow                 | l/min | 10 Hz         |
| TS1    | Temperature                | °C    | 1 Hz          |
| TS2    | Temperature                | °C    | 1 Hz          |
| TS3    | Temperature                | °C    | 1 Hz          |
| TS4    | Temperature                | °C    | 1 Hz          |
| VS1    | Vibration                  | mm/s  | 1 Hz          |
| CE     | CoolingEfficiency(virtual) | %     | 1 Hz          |
| CP     | CoolingPower(virtual)      | kW    | 1 Hz          |


A condição de quatro componentes hidráulicos (resfriador, válvula, bomba e acumulador), assim como a bandeira de estabilidade, variam quantitativamente e suas respectivas classificações são as seguintes:
1) Condição do Resfriador / % <br> 
    3: Próximo de falha total <br>
    20: Eficiência Reduzida  <br>
    100: Eficiência Total <br><br>
2) Condição da Válvula / % <br>
    73: Próximo de falha total <br>
    80: Lag severo  <br>
    90: Lag pequeno  <br>
    100: Comportamento otimizado de troca <br><br>
3) Vazamento interno da bomba <br>
    0: Sem vazamento <br>
    1: Vazamento fraco <br>
    2: Vazamento severo<br><br>
4) Acumulador Hidráulico / bar <br>
    90: Próximo de falha total  <br>
    100: Pressão severamente reduzida <br> 
    115: Pressão levemente reduzida <br>
    130: Pressão otimizada <br><br>
5) Bandeira de Estabilidade <br>
    0: Condições estáveis <br>
    1: Condições estáticas podem não ter sido alcançadas ainda <br><br>

Nesse sentido, os dados de cada sensor foram tratados, calculando-se a média de todos os pontos das colunas, para todas as linhas, obtendo-se um resultado único para cada ciclo de 60 segundos de análise. Em seguida, dentre as classificações realizadas, a bandeira de estabilidade será destacada nesse arquivo, por se tratar de uma condição de operação que engloba a condição de operação dos quatro componentes hidráulicos destacados. A quantidade de dados foi dividida em 80% para treino e 20% para teste. O modelo utilizado para a classificação foi o DecisionTreeClassifier da biblioteca sklearn, e seus hiperparâmetros otimizados usando a biblioteca optuna.

Por fim, fez-se uma análise comparativa da acurácia de diversos modelos de classificação da biblioteca sklearn utilizando os métodos de validação cruzada:  KFold Cross-Validation (kfold = 10) e Leave-one-out Cross Validation (LOOCV).

# Resultados e Discussões
Inicialmente, construiu-se a matriz de correlação entre as variáveis de entrada e a bandeira de estabilidade para compreendermos melhor a influência de cada variável na classificação final.

<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/correlation_matrix_PHM.png?raw=true"  width=900 height=525 /><center>
<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/corr.png?raw=true"  width=900 height=525 /><center>

Em seguida aplicou-se o modelo DecisionTreeClassifier com os hiperparâmetros padrão da biblioteca sklearn, a fim de se observar a influência da sua otimização. A seguir, podemos ver a matriz de confusão para os dados de treino e teste com acurácia igual a 1.0 e 0.955. Podemos ver que esse modelo pode estar ocorrendo um problema de overfitting, devido a grande diferença entre sua acurácia de treinamento e treino e o valor unitário para treinamento.

<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/confusion_matrix_default_training_PHM.png?raw=true"  width=900 height=525 /><center>

<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/confusion_matrix_default_test_PHM.png?raw=true"  width=900 height=525 /><center>

Em seguida, utilizando a biblioteca optuna, os parâmetros e intervalos: max_depth(1, 15), min_samples_split(2, 5) e min_samples_leaf(2,5) foram otimizados, para que possamos avaliar esse possível overfitting. 

<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/optimization_PHM.png?raw=true"  width=900 height=525 /><center>

Os resultados com melhor valor foram determinados com max_depth = 9, min_samples_split = 2 e min_samples_leaf = 3, além da importância do hiperparâmetro max_depth ter a maior importância para o valor objetivo, com 0,975 de relevância.

<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/relevancia_PHM.png?raw=true"  width=900 height=525 /><center>

Em seguida, montou-se novamente a matriz de confusão do treino e teste, obtendo resultados de acurácia de 0,992 e 0,966, respectivamente. Esses valores indicam que o modelo com os hiperparâmetros padrão estavam sofrendo problema de overfitting devido a maior diferença entre acurácia de treino e teste quando comparados aos resultados dos parâmetros otimizados.  

<center><img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/confusion_matrix_otimizado_training_PHM.png?raw=true"  width=900 height=525 /> <img src="https://github.com/amandalemette/EQM2108/blob/2a19ba9f0ef02e0530489f2492546d768902314d/Turma_2021.02/Imagens/releconfusion_matrix_otimizado_test_PHMvancia_PHM.png?raw=true"  width=900 height=525 /><center>

Finalmente, utilizando o método de validação cruzada KFold Cross-Validation (kfold = 10) e Leave-one-out Cross Validation (LOOCV) os seguintes modelos com hiperparâmetros padrão foram avaliados, assim como seus respectivos resultados de acurácia de treino (ideal = LOOCV; cv = kfold10):

>RidgeClassifier: ideal=0.904, cv=0.900
>SGDClassifier: ideal=0.613, cv=0.584
>PassiveAggressiveClassifier: ideal=0.542, cv=0.559
>KNeighborsClassifier: ideal=0.939, cv=0.936
>DecisionTreeClassifier: ideal=0.964, cv=0.964
>ExtraTreeClassifier: ideal=0.966, cv=0.958
>LinearSVC: ideal=0.650, cv=0.591
>SVC: ideal=0.657, cv=0.657
>AdaBoostClassifier: ideal=0.946, cv=0.948
>BaggingClassifier: ideal=0.976, cv=0.976
>RandomForestClassifier: ideal=0.981, cv=0.979
>ExtraTreesClassifier: ideal=0.982, cv=0.980

Dessa forma, os modelos BaggingClassifier, RandomForestClassifier e ExtraTreesClassifier, apresentaram os melhores resultados de acurácia, destacando-se o ExtraTreesClassifier como o melhor dentre todos os modelos analisados. Poderia-se então escolher esse modelo para fazer a análise dos hiperparametros e averiguar problemas de overfitting para futuras análises. No entanto, esses três modelos, por se tratarem de modelos de ensemble, requerem um tempo computacional mais elevado do que o DecisionTreeClassifier, o que deverá ser levado em consideração no tipo de aplicação a ser usada.  

# Conclusões

Podemos concluir que o modelo foi eficaz em classificar as possíveis falhas presentes no equipamento hidráulico quando analisamos a bandeira de estabilidade. No entanto, os hiperparâmetros padrão da biblioteca sklearn apresentaram um problema de overfitting que foi corrigido com a otimização desses hiperparâmetros utilizando a biblioteca optuna. Nesse sentido, os valores otimizados foram de max_depth = 9, min_samples_split = 2 e min_samples_leaf = 2. Além disso, a validação cruzada foi capaz de enaltecer, dentre os modelos analisados, a eficiência dos modelos de ensemble BaggingClassifier, RandomForestClassifier e ExtraTreesClassifier. 

# Referências
- Nikolai Helwig, Eliseo Pignanelli, Andreas Schütze, ‘Condition Monitoring of a Complex Hydraulic System Using Multivariate Statistics’, in Proc. I2MTC-2015 - 2015 IEEE International Instrumentation and Measurement Technology Conference, paper PPS1-39, Pisa, Italy, May 11-14, 2015. doi: 10.1109/I2MTC.2015.7151267

- N. Helwig, A. Schütze, ‘Detecting and compensating sensor faults in a hydraulic condition monitoring system’, in Proc. SENSOR 2015 - 17th International Conference on Sensors and Measurement Technology, oral presentation D8.1, Nuremberg, Germany, May 19-21, 2015. doi: 10.5162/sensor2015/D8.1

- Tizian Schneider, Nikolai Helwig, Andreas Schütze, ‘Automatic feature extraction and selection for classification of cyclical time series data’, tm - Technisches Messen (2017), 84(3), 198–206. doi: 10.1515/teme-2016-0072
