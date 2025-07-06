# TFsim-FP
Segundo trabalho prático da disciplina Arquitetura de Computadores II

TFSim é um simulador educacional que implementa uma arquitetura MIPS64 com suporte à execução fora de ordem e especulação de desvios, baseando-se nos princípios do algoritmo de Tomasulo. Desenvolvido com o uso de SystemC — uma linguagem de modelagem baseada em eventos discretos — o simulador fornece uma interface gráfica detalhada para acompanhar o comportamento interno daarquitetura em nível de ciclos.
Este trabalho tem como objetivo estender as funcionalidades do TFSim com a adição de suporte a novas instruções de ponto flutuante, especificamente FADDI, FADD, FSUB, FMUL e FDIV. Para validar o funcionamento correto dessas instruções, foram desenvolvidos dois trechos de código que exploram suas capacidades: a soma dos termos de uma progressão aritmética com valores em ponto flutuante e o cálculo do quadrado da diferença entre dois valores.

Essas novas instruções foram implementadas baseando-se nas instruções inteiras semelhantes ja existentes (como DADDI, DADD, DSUB, DMUL e DDIV), com atenção especial aos comentários presentes no código especificando onde novas instruções deveriam ser adicionadas e respeitando o uso dos registradores e suas dependências. 

Foram realizadas modificações nos arquivos de código-fonte para incluir os novos identificadores de instruções ao conjunto de instruções reconhecidas pelo simulador, respeitando a estrutura do programa, que dá suporte à execução dessas instruções, extensão das unidades de execução para conseguir realizar as operações, ajustes no despacho das instruções para garantir que elas fossem direcionadas às estações de reserva apropriadas (módulos corretos do SystemC) e configuração da latência das instruções (assim como na implementação das instruções já existentes, utilizou-se valores default, baseados nas latências utilizadas em [David A. Patterson 2012]).

Além disso, foi necessário alterar a conversão dos valores lidos como operandos de uma instrução (string) para que estes fossem armazanados como valores de ponto flutuante nos registradores F (float). Apesar do programa já oferecer algum suporte para adição de novas instruções, foi necessário fazer alterações para que fosse possivel armazenar valores PF nos registradores e também utilizar valores obtidos de operações que deveriam ser executadas anteriormente, aproveitando os valores disponibilizados pelas Estações de Reserva. Dessa forma, foram alteradas as cargas de valores que consideravam apenas inteiros (string to Int) para que pudessem suportar valores ponto flutuante (string to Float).

###Arquivos modificados:###
- issue_control.cpp e issue_control_rob.cpp: adição das novas instruções e especifica\ção de seu tipo, que define para onde ela deve ser enviada no fluxo de módulos de SystemC;
- main.cpp: adição das novas instruções, bem como seus tempos de latência, adição das novas instruções no menu de ajustes dos tempos de latência da interface e adição dos novos trechos de código criados para teste na área de benchmarks para que pudessem ser utilizados com facilidade na interface, além de modificação de alguns trechos de código para que o simulador pudesse ser executado em ambiente Windows;
- res_station.cpp, res_station_rob.cpp}: adição das instruções no trecho de código que cuida da execução das intruções no simulador, para que a lógica por trás das operações se mantesse correta;
- res_vector.cpp e res_vector_rob.cpp: adição das novas instruções e especificação do seu tipo, além de modificar o tipo dos valores resultantes de operações anteriores quando disponibilizados pelas Estações de Reserva, de inteiro para float.

###Experimentação###
Inicialmente, foram desenvolvidos testes bem simples para garantir o funcionamento correto das novas instruções de ponto flutuante implementadas, como a soma, subtração, divisão e multiplicação direta entre dois valores FP. Depois desses testes preliminares, foram desenvolvidos dois trechos de código em MIPS64, adicionados na mesma pasta que contém os benchmarks do simulador, para que pudessem ser utilizados como tais:

- Soma dos termos de uma PA: realiza a soma dos termos de uma progressão aritmética com razão decimal, utilizando FADDI, FADD e FMUL.
- Quadrado da diferença: calcula $(a - b)^2$ utilizando FADDI, FADD, FSUB e FMUL;

Para a realização desses testes, foi alterado o valor armazenado pelo registrador de ponto flutuante F0 para que ele armazanasse o valor 0.0, permitindo assim que ele pudesse ser utilizado de forma semelhante ao R0, que armazena 0 para operações com inteiros. 
