# Sinalização de Produtividade
Atividade final - Tópicos em Indústria 4.0

No ambiente empresarial movimentado de hoje, onde a eficiência é fundamental, surge a ideia de um sistema de LED que usa luzes vermelhas, verdes e amarelas para mostrar o quão produtivo está cada posto de trabalho. Essa abordagem não apenas monitora, mas também melhora o desempenho, dando uma visão rápida do status operacional e incentivando a eficiência na organização. Ao combinar tecnologia e gestão de maneira inteligente, este sistema se torna uma ferramenta valiosa para impulsionar a excelência operacional e atingir metas organizacionais com mais eficácia. 

## Metodologia

### Materias Utilizados
* RaspBerry Pi
* Arduino Uno
* Módulo Semáforo Led
* Jumpers(Conexão Led - Arduino)
* Fonte(ALimentação RaspBerry)
* Cabo USB 2.0 - A/B (Comunicação Serial Arduino - RaspBerry)

###

### Funcionamento 

A ideia do projeto é o RaspBerry receber o valor da produtividade daquele estante, analisar se o valor está dentro ou não da meta de produtividade e por fim indicar para Arduíno através da portal serial qual Led deve estar acesso. E por sua vez o Arduíno deve acender o Led indicado pelo RaspBerry. Para programar o Raspberry Foi utilizado o Node-Red. Como o sistema não foi implementado na pratica dentro do próprio sistema foi criado um código com o intuito de simular uma produção. Ou seja a cada tempo determinado ele adiciona um numero aleatório na produção e esse valor zera quando a unidade de tempo utilizada muda. Para testes foi utilizado o minuto para facilitar a analise.

#### Código para obter o minuto atual 
~~~javascript
let dataAtual = new Date();
let hr = dataAtual.getMinutes();

msg.payload.hora = hr;

return msg;
~~~

#### Código para gerar o valor da produção
~~~javascript
PRODUÇÃO

let dataAtual = new Date();
let hrnew = dataAtual.getMinutes();
let numeroAleatorio = Math.floor(Math.random() * 10);

msg.payload.hrnew = hrnew;

if(msg.payload.hrnew == msg.payload.hr){
    msg.payload.producao = msg.payload.producao + numeroAleatorio;
} else{
    msg.payload.producao = 0 
    msg.payload.hr = msg.payload.hrnew
}
~~~

Em relação ao código de calculo da produção foi colocado em Loop para sempre incrementar o valor final. E por fim esse valor da produção pasa por um ultimo código onde separa cada faixa para cada cor de Led. Ele identifica em qual faixa o valor está e manda essa informação para o Arduíno através da comunicação serial.

#### Código para determinar qual Led ira acender
~~~javascript
if(msg.payload.producao == 0){
    msg.payload.led = 'r'
    msg.payload.aux  = 'r'

} else if(msg.payload.producao >= 1 && msg.payload.producao < 10){
    
    if(msg.payload.aux == 'r'){
        msg.payload.led = ''
    } else if(msg.payload.aux != 'r'){
        msg.payload.led = 'r'
    }
    msg.payload.aux = 'r'

}
else if(msg.payload.producao >= 10 && msg.payload.producao < 25){
    if(msg.payload.aux == 'y'){
        msg.payload.led = ''
    } else if(msg.payload.aux != 'y'){
        msg.payload.led = 'y'
    }
    msg.payload.aux = 'y'
}
else if(msg.payload.producao >= 25){
    if(msg.payload.aux == 'g'){
        msg.payload.led = ''
    } else if(msg.payload.aux != 'g'){
        msg.payload.led = 'g'
    }
    msg.payload.aux = 'g'
}
~~~

Ja no arduino ele recebe qual led 
