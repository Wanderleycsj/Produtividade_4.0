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

A ideia do projeo é o RaspBerry receber o valor da produtividade daquele estante, analisar se o valor está dentro ou não da meta de produtividade e por fim indicar para arduino atraves da portal serial qual Led deve estar acesso. E por sua vez o arduino deve acender o Led indicado pelo RaspBerry. Para programar o Raspberry Foi utilizado o Node-Red. Como o sistema não foi implementado na pratica dentro do proprio sistema foi criado um codigo com o intuido de simular uma produção. Ou seja a cada XXXXXX ele adiciona um numero aleatorio na produção e esse valor zera quando a unidade de tempo utilizada muda. Para testes foi utilizado o minuto para facilitar a analise.

#### Código para obter o minuto atual 
~~~javascript
let dataAtual = new Date();
let hr = dataAtual.getMinutes();

msg.payload.hora = hr;

return msg;


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
