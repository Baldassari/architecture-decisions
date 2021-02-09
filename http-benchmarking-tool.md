# HTTP Benchmarking Tool

* Status: proposed
* Date: 2021-02-03

## Contexto
Necessitamos realizar testes de carga/stress em serviços expostos através de REST APIs em Azure, para garantir que a solução esteja disponível e com bons indicadores de desempenho.

## Factores de Decisão

* A ferramenta deve informar o tempo:
    * de resposta dos pedidos;
    * médio global de respostas;
    * máximo global de respostas;
    * minimo global de respostas.
* A ferramenta deve possibilitar a configuração de envio de pedidos em paralelo;
* A ferramenta pode possibilitar a configuração do tempo predendido para o teste de carga. 

## Opções Consideradas

### Opção 1 - Autocannon - NodeJS
https://github.com/mcollina/autocannon
O Autocannon nos fornece uma tabela clara sobre os tempos, bem como, a probabilidade do tempo de resposta para o cenário em específico.
Também permite criar um cenário de encademento, pois, quando há a necessidade de logica predecedente ou posdecendete esta assim o permite fazer.
Não é clara a sua definição sobre como configurar a utilização em multithreading pois a tecnologia onde foi desenvolvida tem como principio o single-threading. 

#### Exemplo
```
Running 10s test @ https://dws-payment-gateway-ms-watst.azurewebsites.net/api/dws/merchants/benchmark/paymentMethod/qrcode/da01e071-3471-430f-bc86-7a599f01ef62/image
20 connections
1 workers

┌─────────┬────────┬─────────┬─────────┬─────────┬───────────┬────────────┬─────────┐
│ Stat    │ 2.5%   │ 50%     │ 97.5%   │ 99%     │ Avg       │ Stdev      │ Max     │
├─────────┼────────┼─────────┼─────────┼─────────┼───────────┼────────────┼─────────┤
│ Latency │ 506 ms │ 1892 ms │ 4307 ms │ 4967 ms │ 1906.8 ms │ 1171.19 ms │ 4967 ms │
└─────────┴────────┴─────────┴─────────┴─────────┴───────────┴────────────┴─────────┘
┌───────────┬─────┬──────┬────────┬────────┬────────┬─────────┬─────────┐
│ Stat      │ 1%  │ 2.5% │ 50%    │ 97.5%  │ Avg    │ Stdev   │ Min     │
├───────────┼─────┼──────┼────────┼────────┼────────┼─────────┼─────────┤
│ Req/Sec   │ 0   │ 0    │ 10     │ 16     │ 9.81   │ 5.75    │ 2       │
├───────────┼─────┼──────┼────────┼────────┼────────┼─────────┼─────────┤
│ Bytes/Sec │ 0 B │ 0 B  │ 118 kB │ 189 kB │ 115 kB │ 67.7 kB │ 23.6 kB │
└───────────┴─────┴──────┴────────┴────────┴────────┴─────────┴─────────┘

Req/Bytes counts sampled once per second.

98 requests in 10.08s, 1.15 MB read
```
### Opção 2 - Apache JMeter - Java
O Apache JMeter dispoe de uma interface gráfica com diversas funcionalidades para relização de testes. Permite produzir output em diversos formatos. Multithreading by design, signfica que permite executar testes em concorrência. Possibilita ter request predecendentes e posdecentes.
#### Exemplo resultado
```
Sample# StartTime       Thread Name Label           Same Time   Status      Bytes   Sent Bytes  Latency Connect Time
6813	12:21:16.058	TST 1-87	Generate QRCode	2694	    Success	    252	    365	    2694	0
6814	12:21:18.506	TST 1-47	Generate QRCode	260	        Warning	    2556	0	    0	    0
```
### Opção 3 - wrk - C/C++ e LUA

## Pros e contras das opções

### Opção 1 - Autocannon - NodeJS
* Boa, porque nos trás a probabilidade do tempo de resposta em caso de stress/carga.
* Boa, porque o CLI é simples e directo.
* Boa, porque é possiver executar testes programaticamente.
* Boa, porque está sobre licenciamento MIT.
* Mau, porque se houver falha em pedidos não é possivel visualizar o request/response

### Opção 2 - Apache JMeter - Java
* Boa, porque está sobre licenciamento Apache 2.0
* Boa, porque é multithreading by design
* Boa, porque dispoe de uma interface gráfica o que permite a curva de aprendizagem/utilização ser menor

### Opção 3 - wrk - C/C++ e LUA
* Boa, porque altamente recomendado e utilizado por diversos projectos.
* Boa, porque está sobre licenciamento Apache 2.0
* Mau, porque apenas é compatível com OS Linux com arquitectura GNU.
