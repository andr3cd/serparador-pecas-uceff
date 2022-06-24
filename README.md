# serparador-pecas-uceff
SEPARADOR DE PEÇAS ALTAS, MÉDIAS E BAIXAS - UCEFF - ENGENHARIA ELÉTRICA
// SEPARADOR DE PEÇAS ALTAS, MÉDIAS E BAIXAS
/* O funcionamento do circuito eletropneumático básico é da seguinte forma: 
Ao pressionar o botão liga o a máquina liga e o ciclo é iniciado.
Ao passar pelos sensores, é identificado se a peça é alta, média ou baixa.
Movimentando a válvula direcional para passar ar comprimido que movimenta a haste do atuador linear para
frente, ou para trás, liberando acesso a peça em direção ao local de destino.
Então é acionado o soprador que empurra a peça ao local de destino.*/


//define os pinos entradas
int liga = 3;
int e1 = 4; //sensor E1 definido com pino 4 (E1=SENSOR DE PEÇAS BAIXAS)
int e2 = 5; //sensor E2 definido com pino 5 (E2=SENSOR DE PEÇAS MÉDIAS)
int e3 = 6; //sensor E3 definido com pino 6 (E3=SENSOR DE PEÇAS ALTAS)

int s1_mais = 13; //avanço atuador S1 definido com pino 13
int s1_menos = 12; //recuo atuador S1 definido com pino 12
int s2_mais = 11; //aciona soprador S2 definido com pino 11
int ts=500; //tempo do sopro ligado
int td=750; //tempo do desvio ligado

//estados das entradas
int Estado_liga = 0; //atribui 0 ao estado de liga
int Estado_e1 = 0; //atribui 0 ao estado de e1
int Estado_e2 = 0; //atribui 0 ao estado de e2
int Estado_e3 = 0; //atribui 0 ao estado de e3


void setup() {
  pinMode (liga, INPUT); //define liga como uma entrada
  pinMode (e1, INPUT); //define sensor E1 como uma entrada
  pinMode (e2, INPUT); //define sensor E2 como uma entrada
  pinMode (e3, INPUT); //define sensor E3 como uma entrada
  pinMode (s1_mais, OUTPUT); //define s1_mais como uma saída
  pinMode (s1_menos, OUTPUT); //define s1_menos como uma saída
  pinMode (s2_mais, OUTPUT); //define s2_mais como uma saída

}

void loop() {
  Estado_liga = digitalRead(liga); //lê a entrada liga e atribui ao estado
  Estado_e1 = digitalRead(e1); //lê a entrada e1 e atribui ao estado
  Estado_e2 = digitalRead(e2); //lê a entrada e2 e atribui ao estado
  Estado_e3 = digitalRead(e3); //lê a entrada e3 e atribui ao estado

    if ((Estado_liga==1)){
            
    if (((Estado_e1==1)&&(Estado_e2==0)&&(Estado_e3==0))||((Estado_e1==1)&&(Estado_e2==1)&&(Estado_e3==0))){
      digitalWrite(s1_menos, HIGH);////se a peça for média ou baixa, liga o recuo do atuador S1 para direcionar a bandeja de PEÇAS fora do padrão
      delay(td); // tempo do desvio ligado
      digitalWrite(s2_mais, HIGH); //se a peça for média ou baixa, aciona o soprador que empura a peça para a bandeja de PEÇAS fora do padrão
      delay(ts); // tempo do soprodor ligado
      digitalWrite(s2_mais, LOW); // desliga o soprador
      digitalWrite(s1_menos, LOW);//desliga o recuo do atuador S1
      }
      else{digitalWrite(s2_mais, LOW);//caso contrário, desliga o soprador
           digitalWrite(s1_menos, LOW);//caso contrário, desliga o recuo do atuador S1
          }
    if ((Estado_e1==1)&&(Estado_e2==1)&&(Estado_e3==1)){
      digitalWrite(s1_mais, HIGH); //se a peça na esteira for alta aciona os 3 sensores, avançando o atuador S1 para a posição de peças altas
      delay(td); // tempo do desvio ligado
      digitalWrite(s2_mais, HIGH); //aciona o soprador que empura a peça para a bandeja de PEÇAS altas
      delay(ts); // tempo do soprodor ligado
      digitalWrite(s2_mais, LOW); // desliga o soprador
      digitalWrite(s1_mais, LOW); // desliga o avanço do atuador S1
      }
      else{digitalWrite(s2_mais, LOW);//caso contrário, desliga o soprador
           digitalWrite(s1_mais, LOW); //caso contrário, desliga o avanço do atuador S1
      }
      } // se botão liga está =0 coloca todos os atuadores em estado =0
      else{digitalWrite(s2_mais, LOW); //desliga o sistema o soprador
           digitalWrite(s1_mais, LOW); //caso contrário, desliga o avanço do atuador S1
           digitalWrite(s1_menos, LOW);//caso contrário, desliga o recuo do atuador S1
      }       
    
}
    
