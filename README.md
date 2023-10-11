**1 – Introdução.**

Neste documento iremos falar sobre o nosso projeto final, que será um Colorímetro com LDR, que nada mais é do que um instrumento que irá realizar algumas análises de algumas substâncias químicas presentes na água.

Iremos abordar algumas características dele, como o funcionamento, qual a finalidade, falaremos também sobre o aspecto construtivo dele, componentes utilizados, como faremos as leituras, etc...

**2 – Componentes Utilizados.**

Para a montagem deste equipamento será utilizado alguns recursos do MKL05Z32, como o periférico ADC, que será responsável por converter o sinal analógico recebido pelo sensor LDR para um sinal digital para que ele possa fazer o processamento desse sinal, o PWM para poder controlar a intensidade do LED, etc..

Além dos periféricos do MKL05Z32 iremos utilizar alguns outros componentes para que o nosso equipamento funcione da maneira que desejamos, que são:

-   4x botões (push boton)
    
-   1x sensor LDR
    
-   1X LED RGB
    
-   1X display LCD 16x2
    
-   1x CI SMD AD8544
    
-   Transistores BC848
    
-   Resistores e capacitores a definir
    
-   1x gabinete
    
-   1x câmara escura (poço)
  
Com o avanço do projeto, caso tenha tempo hábil, deve ser adicionada uma memória EEPROM externa para salvar dados, o componente deve ser o 24LC512 que tem endereço configurável e comunicação I2C. Acrescentando mais funcionalidades e comunicação serial para descarregar dados em um terminal externo.

**3 – Funcionamento do Equipamento.**

O nosso equipamento foi projetado para fazer análises de algumas substâncias químicas presentes na água, como por exemplo a quantidade de cloro ou ferro. Com isso é possível verificar se a água que está sendo analisada está dentro dos padrões aceitáveis, bastante usado em estação de tratamento de água.

O equipamento irá realizar as leituras através de um feixe de luz emitido por um LED, esse feixe de luz irá variar a resistência do LDR que será medida pela queda de tensão que ele causa no circuito, esse sinal será convertido para um sinal digital através do conversor ADC do MKL05Z32 onde será feito o processamento desse sinal e será mostrado os resultados na tela, que será um display LCD.

O sensor LDR e o LED ficarão nas extremidades de um poço escuro com 180° entre eles, fazendo com que o LED fique de frente para o LDR, ficando um vão entre os dois onde será colocado uma cubeta de vidro com a amostra de agua dentro, o feixe de luz do LED ira passar por essa amostra de agua até chegar no sensor LDR, dependendo da coloração que a amostra estiver esse feixe de luz deverá chegar até o sensor com uma intensidade de luz mais fraca, e através dessa diferença da intensidade de luz o equipamento irá fazer os cálculos para definir a quantidade de uma determinada substancia química presente na água.

Para fazer a análise, será feito a coleta de uma amostra de água e nessa amostra será colocado um reagente químico, como por exemplo um reagente de cloro que tem como por exemplo a cor vermelha, quando esse reagente entrar em contato com a água irá acontecer uma reação química na água mudando a coloração da água para vermelho de acordo com a quantidade de cloro presente na água, quanto maior a quantidade de cloro, mais forte ficara essa cor vermelha, e é através dessa coloração a luz verde emitida pelo LED será absorvida pela amostra e então o LDR verá uma irradiância menor de luz. Nesse exemplo usamos o cloro, mas para fazer a análise de ferro usaríamos um reagente de ferro podendo ser de uma coloração diferente do cloro, mas o princípio da reação seria o mesmo.

Para medirmos essa diferença de coloração será primeiro feito a calibração do equipamento com um padrão branco (zero), que é água ultrapura que não possui nenhum tipo de interferente, esse padrão terá 100% de transmitância, através dessa transmitância que será feito os cálculos para fazer as análises. Ao fazer calibração do equipamento irá ser inserido no equipamento a amostra da água com a reação feita, o feixe de luz que passar por essa amostra chegara com uma intensidade de luz menor do que a intensidade de luz no padrão branco, por conta da coloração da amostra, fazendo com que a transmitância seja menor que a do padrão branco, e é essa diferença de transmitância que o equipamento irá utilizar para fazer os cálculos para determinar a quantidade de tal substância química presente na água.

O equipamento terá três menus para cada cor do LED RGB, porque o LED que será acionado para fazer as leituras irá depender da análise a ser feita, por exemplo para analisar o cloro será necessário fazer a análise com o LED verde, então essa análise devera ser feita no menu do LED verde, e ainda terá um quarto menu que é onde será feito o ajuste da intensidade dos LEDs através do controle de PWM.

**4 – Aspectos Construtivos.**

A ideia é fazer o equipamento em um gabinete que irá conter um espaço interno para colocar a placa, no lado externo ficara os botões que serão utilizados para fazer a navegação no equipamento, serão quatro botões onde um será o de OK, um de CANCELA, um CIMA e um BAIXO, ele também terá um espaço para deixar o display a mostra e um espaço para colocar o poço.

O poço será um poço escuro com uma tampa para que não entre luz externa a não ser a própria luz do LED, ele será redondo com um vão no meio para inserir a cubeta e nas laterais dele ficarão o LED e o sensor LDR, um de frente para o outro.

Na construção da placa ainda não temos um layout definido, mas sabemos que vamos fazer um circuito para o MKL05Z32, para poder utilizar os periféricos dele e fazer o controle equipamento, e para a leitura do LDR usaremos o periférico de ADC do MKL05Z32, onde será usada uma ponte de Wheatstone para melhor percepção da variação da resistência que estará ligada a um bloco de ampOp diferencial com ganho a definir.

Uma das dificuldades que podemos encontrar na construção do equipamento seja fazer com que as leituras do sensor LDR fiquem estáveis, para que o equipamento tenha mais precisão nos resultados, onde ainda será pensado em como fazer para estabilizar as leituras, se será feito através de filtros, ou achar uma intensidade do LED onde as leituras fiquem mais estáveis, os tentar fazer algo no firmware para ele fazer várias leituras e tirar uma média dessas leituras.

**5 - Conclusão**

A ideia principal do projeto é fazer as leituras de amostras de água para verificar determinada substancia química presente na água, caso sobre tempo pensamos em colocar algumas funcionalidades a mais no equipamento, como por exemplo fazer a análise da água e armazenar o resultado dessa analise dentro de uma memória externa, na qual pretendemos utilizar o CI SMD 24LC512, e ainda fazer com que o usuário consiga transferir esses resultados para o seu computador através da comunicação I2C. Pensamos também em adicionar a função de data e hora no equipamento para que junto com os resultados das analises possa ser salvo também a data e o horário em que foi feito a análise, para que seja mais fácil para o cliente comparar os resultados.

Conforme foi dito anteriormente essas funcionalidades só serão adicionadas no equipamento caso haja tempo hábil para fazê-las, mas a ideia principal é fazer somente as leituras das amostras.
