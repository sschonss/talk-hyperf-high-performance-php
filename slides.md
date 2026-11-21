---
marp: true
theme: aifa
paginate: true
lang: pt-BR
title: High-Performance PHP com Hyperf
description: Uma introdução ao Hyperf, ao Swoole e a uma migração real do Laravel
author: Luiz Schons
footer: 'High-Performance PHP com Hyperf · luizschons.com'
---

<!-- _class: cover -->
<!-- _paginate: false -->
<!-- _footer: '' -->

# High-Performance PHP com Hyperf

Acelerando arquitetura de microservices com PHP

<br>

**Luiz Schons**

<!--
Palestra que junta duas versões antigas: "Acelerando Arquitetura de Microservices com PHP — uma introdução ao Hyperf e Sistemas Distribuídos" e "Migração Eficiente: do Laravel para o Hyperf".
Roteiro: primeiro desfazer o medo do PHP, depois entender runtimes e Swoole, depois o Hyperf no código e, por fim, uma migração real do Laravel para o Hyperf.
Transição: "Antes, deixa eu me apresentar."
-->

---

<div class="about">
<div>

# Luiz Schons

<p class="role">Senior Software Engineer @ <b>PicPay</b></p>

<div class="chips">
<span>Guarapuava, Paraná, Brasil</span>
<span>DevParaná</span>
<span>UTFPR</span>
<span>PHP</span>
</div>

<p class="site">luizschons.com</p>

</div>
<img class="photo" src="assets/luiz.jpg" alt="Luiz Schons">
</div>

<!--
Apresentação rápida (30 segundos): quem sou, onde trabalho, de onde venho, DevParaná.
Transição: "Antes de começar, um aviso."
-->

---

## Disclaimer

<ol class="steps">
<li>Não sou <b>especialista</b> em nada</li>
<li>Analise o <b>seu cenário</b></li>
<li>Não existe <b>bala de prata</b></li>
<li>Estude e estude…</li>
</ol>

<!--
1. Não sou especialista em nada: vou mostrar o que eu estudei e vivi.
2. Analise o cenário em que você está: o que funcionou para nós pode não servir para você.
3. Não existe bala de prata: Hyperf não resolve tudo.
4. Estude e estude…
-->

---

## Nivelamento

<div class="cols3">
<div class="card accent">

### API
Application Programming Interface

</div>
<div class="card">

### Microservices

</div>
<div class="card">

### Sistemas distribuídos

</div>
</div>

<br>

<p class="muted">Vamos desenhar um pouco para entender melhor</p>

<!--
Nivelar os termos antes de seguir:
- API (Application Programming Interface).
- Microservices.
- Sistemas distribuídos.
No original, aqui eu desenhava ao vivo para explicar. Desenhar no quadro (ou com a mão) como uma API conversa com outra, e como vários serviços formam um sistema distribuído.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 1 · A lenda urbana</div>

# Quem tem medo do PHP?

<!--
Vamos entender um pouco dessa lenda urbana.
-->

---

## Os motivos do medo

<div class="cols">
<div class="card bad">

### Passado
Más práticas · Segurança

</div>
<div class="card warn">

### Mudança
Muitas mudanças na linguagem

</div>
</div>

<br>

<div class="card">

### Desempenho
vs. Ruby, Python e Java

</div>

<!--
Por que as pessoas têm medo do PHP?
1. Histórico de más práticas.
2. Segurança.
3. Muitas mudanças na linguagem.
4. Desempenho, comparado com Ruby, Python e Java.
-->

---

<!-- _class: statement -->

> Isso **mudou**.

<!--
Pausa. Tudo isso é passado.
-->

---

## O PHP de hoje

<div class="chips accent">
<span>JIT · 7.x / 8.x</span>
<span>Strict Typing</span>
<span>Null Coalesce</span>
<span>Closures</span>
<span>Generators</span>
<span>Segurança</span>
<span>Composer</span>
<span>PHPUnit · Pest</span>
<span>Laravel · Symfony · Zend · Hyperf</span>
</div>

<br>

<p class="muted">E… diferentes <b>runtimes</b>?</p>

<!--
1. Desempenho melhorado (7.x e 8.x) com o uso do JIT.
2. Sintaxe e recursos modernos: strict typing, operador null coalesce, funções anônimas, closures, generators e muito mais.
3. Segurança aprimorada.
4. Ferramentas de desenvolvimento e integrações facilitadas: Composer, PHPUnit, Pest.
5. Frameworks modernos: Laravel, Symfony, Zend, Hyperf.
6. Diferentes runtimes?????
Transição: "Mas o que são runtimes?"
-->

---

<!-- _class: statement -->

> Mas o que são **runtimes**?

<!--
Exemplos na tela original: PHP-FPM, RoadRunner, Swoole.
-->

---

## Runtime

<div class="card big accent">

Onde o código PHP é **executado**

</div>

<br>

<div class="chips">
<span>Interpretação</span>
<span>Memória</span>
<span>I/O</span>
<span>Sistema operacional</span>
</div>

<!--
Conceito: runtime refere-se ao ambiente ou infraestrutura onde o código PHP é executado.
Ele trata da interpretação e execução do script, gerencia a memória, os processos de entrada/saída (I/O) e outras interações com o sistema operacional e os recursos da aplicação.
Exemplos: PHP-FPM, RoadRunner, Swoole.
-->

---

## Entendendo o Swoole

<div class="cols3">
<div class="card accent">

### Assíncrono
alta performance

</div>
<div class="card accent">

### Coroutines

</div>
<div class="card accent">

### Escala
mais processamento

</div>
</div>

<br>

<p class="muted">Uma runtime PHP</p>

<!--
Swoole é uma runtime PHP que permite a programação assíncrona de alta performance, com suporte a coroutines, melhorando significativamente a capacidade de processamento e a escalabilidade de aplicações web.
-->

---

## FPM × Swoole

<svg class="network" viewBox="0 0 1776 700" font-family="Inter, sans-serif" font-size="30">
<g fill="none" stroke="#c9ced8" stroke-width="2.5">
<rect x="20" y="20" width="840" height="660" rx="27" fill="#f6f7f9"/>
<rect x="916" y="20" width="840" height="660" rx="27" fill="#f6f7f9"/>
</g>
<text x="440" y="80" text-anchor="middle" font-weight="800" font-size="36">Nginx + PHP-FPM</text>
<text x="1336" y="80" text-anchor="middle" font-weight="800" font-size="36" fill="#4f46e5">Swoole</text>
<g font-size="26" text-anchor="middle">
<rect x="70" y="120" width="740" height="64" rx="14" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="440" y="162">Master Process</text>
<g id="w"><rect x="70" y="240" width="220" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="180" y="277">Worker</text>
<rect x="70" y="330" width="220" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="180" y="367">PHP-FPM</text>
<rect x="70" y="420" width="220" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="180" y="457">index.php</text></g>
<use href="#w" x="260"/><use href="#w" x="520"/>
<rect x="70" y="510" width="220" height="56" rx="12" fill="#fdecd3" stroke="none"/><text x="180" y="547">autoload</text>
<rect x="330" y="510" width="480" height="56" rx="12" fill="#fdecd3"/><text x="570" y="547">start / load framework</text>
<rect x="70" y="600" width="220" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="180" y="637">Database</text>
<rect x="966" y="120" width="740" height="110" rx="14" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="1336" y="152" font-size="22" fill="#6b7280">Master Process</text>
<rect x="996" y="165" width="210" height="50" rx="10" fill="#e8e7fd"/><text x="1101" y="199">Reactor</text>
<rect x="1231" y="165" width="210" height="50" rx="10" fill="#e8e7fd"/><text x="1336" y="199">Reactor</text>
<rect x="1466" y="165" width="210" height="50" rx="10" fill="#e8e7fd"/><text x="1571" y="199">Reactor</text>
<rect x="1226" y="270" width="220" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="1336" y="307">Manager</text>
<rect x="976" y="370" width="200" height="56" rx="12" fill="#4f46e5"/><text x="1076" y="407" fill="#fff">Worker</text>
<rect x="1236" y="370" width="200" height="56" rx="12" fill="#4f46e5"/><text x="1336" y="407" fill="#fff">Worker</text>
<rect x="1496" y="370" width="200" height="56" rx="12" fill="#4f46e5"/><text x="1596" y="407" fill="#fff">Worker</text>
<rect x="1196" y="480" width="280" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="1336" y="517">PHP Files</text>
<rect x="1226" y="600" width="220" height="56" rx="12" fill="#fff" stroke="#c9ced8" stroke-width="2"/><text x="1336" y="637">Database</text>
</g>
<g stroke="#9aa1ad" stroke-width="2.5" fill="none">
<path d="M180 184v56M440 184v56M700 184v56M180 296v34M180 386v34M180 476v34M180 566v34M290 538h40"/>
<path d="M1336 230v40M1336 326v44M1300 326L1076 370M1372 326L1596 370M1176 398h60M1436 398h60M1076 426L1290 480M1336 426v54M1596 426L1382 480M1336 536v64"/>
</g>
</svg>

<!--
Comparar os dois modelos (diagramas dos dois decks originais).
FPM: o Nginx tem um master process; cada requisição vai para um worker, que chama o PHP-FPM, que executa o index.php, carrega o autoload, sobe/carrega o framework e vai ao banco. Isso a cada requisição.
Swoole: um master process com reactors, um manager e workers que ficam de pé, conversam entre si e carregam os arquivos PHP, que acessam o banco.
Destaque: no FPM o framework sobe a cada requisição; no Swoole os workers ficam vivos.
-->

---

## Síncrono

<svg class="network" viewBox="0 0 1776 520" font-family="Inter, sans-serif" font-size="32">
<g fill="#6b7280" font-weight="700"><text x="0" y="80">THREAD 1</text><text x="0" y="200">THREAD 2</text><text x="0" y="320">THREAD 3</text><text x="0" y="440">THREAD 4</text></g>
<g stroke="#e5e7eb" stroke-width="2"><path d="M220 70H1760M220 190H1760M220 310H1760M220 430H1760"/></g>
<rect x="240" y="290" width="500" height="40" rx="10" fill="#4f46e5"/>
<rect x="750" y="290" width="250" height="40" rx="10" fill="#818cf8"/>
<rect x="1010" y="290" width="740" height="40" rx="10" fill="#4f46e5"/>
<g font-size="28" text-anchor="middle"><text x="490" y="380">10 s</text><text x="875" y="380">5 s</text><text x="1380" y="380">15 s</text></g>
</svg>

<p class="muted">Uma tarefa depois da outra</p>

<!--
Modelo síncrono: as tarefas rodam numa thread só, uma depois da outra. 10 segundos, depois 5, depois 15: só termina quando todas terminam em sequência.
-->

---

## Assíncrono concorrente

<svg class="network" viewBox="0 0 1776 520" font-family="Inter, sans-serif" font-size="32">
<g fill="#6b7280" font-weight="700"><text x="0" y="80">THREAD 1</text><text x="0" y="200">THREAD 2</text><text x="0" y="320">THREAD 3</text><text x="0" y="440">THREAD 4</text></g>
<g stroke="#e5e7eb" stroke-width="2"><path d="M220 70H1760M220 190H1760M220 310H1760M220 430H1760"/></g>
<rect x="240" y="290" width="240" height="40" rx="10" fill="#4f46e5"/>
<rect x="500" y="170" width="360" height="40" rx="10" fill="#818cf8"/>
<rect x="880" y="290" width="360" height="40" rx="10" fill="#4f46e5"/>
<rect x="1260" y="170" width="360" height="40" rx="10" fill="#818cf8"/>
<rect x="1260" y="290" width="490" height="40" rx="10" fill="#c7d2fe"/>
<g font-size="28" text-anchor="middle"><text x="360" y="380">2 s</text><text x="680" y="150">3 s</text><text x="1060" y="380">3 s</text><text x="1440" y="150">3 s</text><text x="1505" y="380">5 s</text></g>
</svg>

<p class="muted">Tarefas se alternam enquanto esperam</p>

<!--
Assíncrono concorrente: as tarefas se intercalam. Enquanto uma espera (I/O), outra avança. Tempos do slide original: 2, 3, 3, 3 e 5 segundos, alternando entre as threads.
É aqui que as coroutines do Swoole brilham.
-->

---

## Assíncrono paralelo

<svg class="network" viewBox="0 0 1776 520" font-family="Inter, sans-serif" font-size="32">
<g fill="#6b7280" font-weight="700"><text x="0" y="80">THREAD 1</text><text x="0" y="200">THREAD 2</text><text x="0" y="320">THREAD 3</text><text x="0" y="440">THREAD 4</text></g>
<g stroke="#e5e7eb" stroke-width="2"><path d="M220 70H1760M220 190H1760M220 310H1760M220 430H1760"/></g>
<rect x="240" y="50" width="600" height="40" rx="10" fill="#4f46e5"/>
<rect x="240" y="170" width="600" height="40" rx="10" fill="#818cf8"/>
<rect x="240" y="290" width="600" height="40" rx="10" fill="#4f46e5"/>
<g font-size="28" text-anchor="middle"><text x="540" y="130">5 s</text><text x="540" y="250">5 s</text><text x="540" y="370">5 s</text></g>
</svg>

<p class="muted">Ao mesmo tempo, em threads diferentes</p>

<!--
Assíncrono paralelo: as tarefas rodam ao mesmo tempo em threads diferentes. Três tarefas de 5 segundos terminam juntas.
-->

---

<!-- _class: statement -->

> Mas eu tenho que fazer **tudo isso na mão**?

<!--
Pergunta retórica. Não: é para isso que existe o Hyperf.
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 2 · O framework</div>

# Hyperf

<!--
Hyperf = Hyperspeed + Flexibility. "The Way to PHP Microservice".
-->

---

## Hyperf = Hyperspeed + Flexibility

<div class="cols">
<div class="card accent">

### Arquitetura modular

</div>
<div class="card accent">

### Performance

</div>
<div class="card">

### I/O não bloqueante
com Swoole

</div>
<div class="card">

### Extensa integração

</div>
</div>

<br>

<p class="muted">"The Way to PHP Microservice"</p>

<!--
O site do Hyperf se apresenta como "The Way to PHP Microservice", e o nome vem de Hyperspeed + Flexibility.
- Arquitetura modular.
- Performance.
- I/O não bloqueante com Swoole.
- Extensa integração.
-->

---

## Um controller

<style scoped>pre { font-size: 27px; }</style>

```php
<?php

declare(strict_types=1);

namespace App\Controller;

use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Contract\ResponseInterface;

class IndexController
{
    public function index(RequestInterface $request, ResponseInterface $response)
    {
        $target = $request->input('target', 'World');
        return 'Hello ' . $target;
    }
}
```

<!--
Um controller no Hyperf. Parece muito com o que a gente já conhece.
Comentário do código original: os objetos relacionados (RequestInterface e ResponseInterface) são injetados automaticamente pelo container de injeção de dependência.
-->

---

## Um model

<style scoped>pre { font-size: 30px; }</style>

```php
<?php

declare(strict_types=1);

namespace App\Model;

use Hyperf\DbConnection\Model\Model;

class User extends Model
{
    protected ?string $table = 'user';

    protected array $fillable = ['id', 'name', 'gender', 'created_at', 'updated_at'];

    protected array $casts = ['id' => 'integer', 'gender' => 'integer'];
}
```

<!--
O model: quem vem do Laravel se sente em casa. $table, $fillable e $casts, como no Eloquent.
O slide original tinha os docblocks (@property id, name, gender, created_at, updated_at e a descrição de cada atributo); aqui ficou só o essencial.
-->

---

## Rotas

<style scoped>pre { font-size: 32px; }</style>

```php
<?php
use Hyperf\HttpServer\Router\Router;

// Any of the following three definitions can achieve the same effect
Router::get('/hello-hyperf', 'App\Controller\IndexController::hello');
Router::get('/hello-hyperf', 'App\Controller\IndexController@hello');
Router::get('/hello-hyperf', [App\Controller\IndexController::class, 'hello']);
```

<p class="muted">Três formas, mesmo efeito</p>

<!--
Rotas: qualquer uma das três definições tem o mesmo efeito. String com ::, string com @ (estilo Laravel) ou array com a classe e o método.
-->

---

## Hyperf × Laravel

<div class="cols3">
<div class="card">

### Performance
<span class="good">Hyperf ✓</span> · Laravel ✗

</div>
<div class="card">

### Comunidade
Hyperf ✗ · <span class="good">Laravel ✓</span>

</div>
<div class="card">

### Curva de aprendizado
Hyperf ✓ · Laravel ✓

</div>
</div>

<!--
Comparação rápida do deck original:
- Performance: ponto para o Hyperf.
- Comunidade: ponto para o Laravel.
- Curva de aprendizado: os dois vão bem; quem sabe Laravel aprende Hyperf.
-->

---

## Quando usar cada um

<style scoped>
table { font-size: 27px; width: 100%; }
th, td { padding: 9px 18px; }
</style>

| Critério | Laravel | Hyperf |
|---|---|---|
| Tamanho | Pequeno a médio | Médio a grande |
| Arquitetura | MVC | Microservices, corrotinas |
| Desempenho | Limitado em alta carga | Excepcional |
| Escalabilidade | Web tradicional | Sistemas distribuídos |
| Facilidade | Vasta doc e comunidade | Requer assíncrono e Swoole |
| Microservices | Pode não ser eficiente | Nativo: gRPC, REST |
| Ecossistema | Grande | Em crescimento |
| Casos de uso | CMS, e-commerce, dashboards | APIs de alta carga, tempo real |

<!--
Tabela do deck de migração:
- Tamanho da aplicação: Laravel, pequeno a médio porte (blogs, CMS, e-commerce médio). Hyperf, médio a grande porte (microserviços, APIs de alta carga).
- Arquitetura: Laravel, padrão MVC, desenvolvimento rápido e manutenção. Hyperf, orientado a microserviços, com corrotinas para alta concorrência.
- Desempenho: Laravel adequado para aplicações tradicionais, limitado em alta carga. Hyperf excepcional, lidando com milhões de requisições por segundo.
- Escalabilidade: Laravel adequado para a maioria das aplicações web tradicionais. Hyperf, alta escalabilidade, ideal para sistemas distribuídos e alta disponibilidade.
- Facilidade de desenvolvimento: Laravel, sintaxe expressiva, vasta documentação e comunidade ativa. Hyperf requer conhecimento prévio de programação assíncrona e do Swoole.
- Suporte a microserviços: Laravel suporta, mas pode não ser tão eficiente em alta carga. Hyperf tem suporte nativo, gRPC, REST e outras arquiteturas distribuídas.
- Ecossistema: Laravel, grande comunidade e ecossistema de pacotes. Hyperf, comunidade em crescimento, foco em alta performance.
- Casos de uso: Laravel para web tradicional, CMS, e-commerce médio, blogs, dashboards administrativos. Hyperf para APIs de alta performance, microserviços, alta concorrência e baixa latência, tempo real (chat, push).
-->

---

<!-- _class: divider -->

<div class="kicker">Parte 3 · Na prática</div>

# Migração: do Laravel para o Hyperf

<!--
Migração de software: um pouco do que eu já passei.
-->

---

## Qual era o cenário?

<svg class="network" viewBox="0 0 1776 660" font-family="Inter, sans-serif" font-size="26" text-anchor="middle">
<g fill="none" stroke="#c9ced8" stroke-width="2" stroke-dasharray="8 6">
<rect x="800" y="10" width="520" height="100" rx="18"/>
<rect x="620" y="150" width="1140" height="260" rx="18"/>
<rect x="20" y="200" width="520" height="450" rx="18"/>
</g>
<g fill="#6b7280" font-size="22"><text x="1060" y="40">Front End</text><text x="1190" y="180">Main</text><text x="280" y="230">Other Services</text></g>
<g fill="#fff" stroke="#c9ced8" stroke-width="2">
<rect x="960" y="50" width="200" height="50" rx="10"/>
<rect x="660" y="200" width="200" height="50" rx="10"/><rect x="940" y="200" width="200" height="50" rx="10"/><rect x="1300" y="200" width="200" height="50" rx="10"/>
<rect x="80" y="260" width="200" height="50" rx="10"/><rect x="80" y="360" width="200" height="50" rx="10"/><rect x="300" y="440" width="200" height="50" rx="10"/><rect x="200" y="520" width="200" height="50" rx="10"/><rect x="250" y="590" width="200" height="50" rx="10"/>
</g>
<g fill="#e8e7fd"><rect x="660" y="320" width="200" height="60" rx="30"/><rect x="900" y="320" width="200" height="60" rx="30"/><rect x="1240" y="320" width="200" height="60" rx="30"/><rect x="1480" y="320" width="220" height="60" rx="30"/></g>
<text x="1060" y="83">Front End</text>
<text x="760" y="233">Service 1</text><text x="1040" y="233">Service 2</text><text x="1400" y="233">Service 3</text>
<text x="180" y="293">Service 4</text><text x="180" y="393">Service 5</text><text x="400" y="473">Service 6</text><text x="300" y="553">Service 7</text><text x="350" y="623">Service 8</text>
<g font-size="22"><text x="760" y="358">SQL DB 1</text><text x="1000" y="358">SQL DB 2</text><text x="1340" y="358">SQL DB 3</text><text x="1590" y="358">noSQL DB</text></g>
<g fill="none" stroke="#9aa1ad" stroke-width="2.5">
<path d="M1060 100L760 200M1060 100L1040 200M1060 100L1400 200M760 250v70M1040 250L1000 320M1400 250L1340 320M1400 250L1590 320"/>
<path d="M180 310v50M280 385L300 465M400 490L300 520M300 570L350 590" />
<path d="M660 230Q560 260 280 285M940 230Q600 300 280 385M1300 240Q700 420 500 465M1300 250Q700 520 400 545M1500 230Q1600 640 450 615" stroke="#ef4444" stroke-dasharray="6 6"/>
</g>
</svg>

<!--
Vamos entender um pouco dessa história. Qual era o cenário em que a gente estava?
Um front end chamando três serviços principais (cada um com seu banco: SQL 1, SQL 2, SQL 3 e um noSQL), e esses serviços chamando uma cadeia de outros serviços (4 a 8). Muitas dependências cruzadas.
No deck original eu também mostrava telas reais do sistema e destacava o Service 3.
-->

---

## Por que migrar?

<div class="cols">
<div class="card bad">

### Passado
Decisões não assertivas

</div>
<div class="card bad">

### Medo
Programar pisando em ovos

</div>
<div class="card warn">

### Performance
não era culpa do Laravel

</div>
<div class="card warn">

### Micro que não era micro
Domínios · deploy tenso

</div>
</div>

<!--
Motivos:
1. Decisões não assertivas no passado.
2. Programar pisando em ovos.
3. Performance (e não era culpa do Laravel).
4. Micro-serviço que não era micro (domínios misturados).
5. O deploy era tenso.
-->

---

## Por que Hyperf?

<div class="cols">
<div class="card accent">

### Time
Todo mundo sabia PHP

</div>
<div class="card accent">

### Coroutines
Non-blocking

</div>
<div class="card">

### Desafio

</div>
<div class="card">

### Performance
Swoole

</div>
</div>

<!--
Motivos:
1. Toda a equipe tinha conhecimento em PHP.
2. Coroutine e sistema non-blocking.
3. Desafio.
4. Performance (Swoole).
-->

---

## Como planejamos

<ol class="steps">
<li>Onde chegar: <b>melhor DX</b></li>
<li>Entender os <b>domínios</b></li>
<li>Um <b>plano de ação</b></li>
</ol>

<p class="muted">…mas nem tudo são flores</p>

<!--
1. Definimos onde queríamos chegar: um software com uma experiência de desenvolvimento melhor (DX).
2. Entendemos melhor os domínios da aplicação.
3. Criamos um plano de ação.
Mas nem tudo são flores…
-->

---

## Primeiro desafio: o banco

<div class="flow">
<div class="wait">MongoDB</div><span>→</span>
<div class="ok">PostgreSQL</div>
</div>

<br>

<ol class="steps">
<li>Migrar o <b>banco de dados</b></li>
<li>Migrar o <b>framework</b></li>
</ol>

<!--
Começou o primeiro desafio: migrar o banco de dados (no slide original, MongoDB para PostgreSQL).
Procedimento: 1º migrar o banco de dados. 2º migrar o framework.
-->

---

<!-- _class: statement -->

> Segundo desafio: como definir **domínios**?

<!--
Começou o segundo desafio. Um serviço grande que precisava virar vários. Mas como definir domínios?
-->

---

## Elephant Migration

<div class="card big bad">

Anti-pattern: migrar **tudo de uma vez**

</div>

<br>

<p class="muted">"Arquitetura: vai fazendo aí"</p>

<!--
Elephant Migration AntiPattern: tentar migrar o elefante inteiro de uma vez.
Meme do deck original: o livro "Arquitetura: vai fazendo aí".
-->

---

## Dois caminhos

<div class="cols">
<div class="card accent">

### Decomposição baseada em componentes
Refatoração · extração de componentes · incremental e controlada

</div>
<div class="card">

### Bifurcação tática
Réplicas dos serviços · remoção de partes indesejadas

</div>
</div>

<br>

<p class="muted">Arquitetura de Software: as Partes Difíceis</p>

<!--
Bifurcação tática ou decomposição baseada em componentes?
- Decomposição baseada em componentes: refatoração, extração de componentes, incremental e controlada.
- Bifurcação tática: réplicas dos serviços e remoção das partes indesejadas.
Referência: livro "Arquitetura de Software: as Partes Difíceis — Análises Modernas de Trade-off para Arquiteturas Distribuídas" (Neal Ford, Mark Richards, Pramod Sadalage, Zhamak Dehghani), com o fluxograma de decisão entre as duas abordagens.
-->

---

## Terceiro desafio: documentação

<div class="flow">
<div class="bad">Documentação desatualizada</div><span>→</span>
<div class="ok">Documentação + ADR</div>
</div>

<br>

<p class="muted">Architecture Decision Records</p>

<!--
Encontramos um problema: documentação desatualizada.
Começou o terceiro desafio. A resposta: documentação + Architecture Decision Records (ADR), documentando o software e as decisões.
-->

---

## Voltando ao segundo desafio

<div class="cols3">
<div class="card bad">

### Responsabilidades
demais para um "micro" serviço

</div>
<div class="card bad">

### Acoplamento
forte entre domínios

</div>
<div class="card bad">

### Deploy
impactava produção

</div>
</div>

<!--
Voltando ao segundo desafio. Os problemas do serviço:
1. Muitas responsabilidades para um "micro-serviço" só.
2. Acoplamento muito forte entre diferentes domínios da aplicação.
3. Deploy impactava muito no ambiente de produção.
-->

---

## Um serviço → vários, via mensageria

<svg class="network" viewBox="0 0 1776 560" font-family="Inter, sans-serif" font-size="32" text-anchor="middle">
<rect x="60" y="170" width="320" height="220" rx="27" fill="#f59e0b"/>
<text x="220" y="290" fill="#fff" font-weight="800">Serviço</text>
<path d="M440 280H640" stroke="#4f46e5" stroke-width="10"/><path d="M640 250l50 30-50 30z" fill="#4f46e5"/>
<rect x="1060" y="240" width="300" height="80" rx="40" fill="#ff6600"/><text x="1210" y="291" fill="#fff" font-weight="800">RabbitMQ</text>
<g fill="#fde68a"><rect x="760" y="40" width="220" height="120" rx="20"/><rect x="1440" y="40" width="220" height="120" rx="20"/><rect x="760" y="400" width="220" height="120" rx="20"/><rect x="1440" y="400" width="220" height="120" rx="20"/></g>
<g font-size="26"><text x="870" y="110">Serviço</text><text x="1550" y="110">Serviço</text><text x="870" y="470">Serviço</text><text x="1550" y="470">Serviço</text></g>
<g stroke="#111827" stroke-width="3"><path d="M980 140L1100 240M1440 140L1320 240M980 420L1100 320M1440 420L1320 320"/></g>
</svg>

<!--
A solução: quebrar o serviço grande em serviços menores que se comunicam por mensageria, com RabbitMQ no meio.
-->

---

## Consumer com Hyperf AMQP

<style scoped>pre { font-size: 25px; } h2 { margin-bottom: 12px; }</style>

```php
<?php

declare(strict_types=1);

namespace App\Amqp\Consumers;

use Hyperf\Amqp\Annotation\Consumer;
use Hyperf\Amqp\Message\ConsumerMessage;
use Hyperf\Amqp\Result;
use PhpAmqpLib\Message\AMQPMessage;

#[Consumer(exchange: 'hyperf', routingKey: 'hyperf', queue: 'hyperf', nums: 1)]
class DemoConsumer extends ConsumerMessage
{
    public function consumeMessage($data, AMQPMessage $message): Result
    {
        print_r($data);
        return Result::ACK;
    }
}
```

<!--
Um consumer no Hyperf: uma annotation (atributo) com exchange, routing key, fila e número de consumidores. O método consumeMessage recebe os dados e devolve Result::ACK.
-->

---

## Antes × depois

<svg class="network" viewBox="0 0 1776 600" font-family="Inter, sans-serif" font-size="26" text-anchor="middle">
<g fill="none" stroke="#c9ced8" stroke-width="2" stroke-dasharray="8 6">
<rect x="20" y="60" width="560" height="500" rx="20"/>
<rect x="640" y="60" width="1116" height="500" rx="20"/>
</g>
<text x="300" y="40" fill="#6b7280" font-weight="700">Antes</text>
<text x="1198" y="40" fill="#4f46e5" font-weight="700">Depois</text>
<rect x="180" y="150" width="240" height="70" rx="14" fill="#f59e0b"/><text x="300" y="195" fill="#fff" font-weight="700">Service 3</text>
<rect x="180" y="380" width="240" height="70" rx="35" fill="#e8e7fd"/><text x="300" y="425">SQL DB 3</text>
<path d="M300 220v160" stroke="#9aa1ad" stroke-width="3"/>
<g fill="#4f46e5"><rect x="670" y="150" width="160" height="70" rx="14"/><rect x="850" y="150" width="160" height="70" rx="14"/><rect x="1030" y="150" width="160" height="70" rx="14"/><rect x="1210" y="150" width="160" height="70" rx="14"/><rect x="1390" y="150" width="160" height="70" rx="14"/><rect x="1570" y="150" width="160" height="70" rx="14"/></g>
<g fill="#fff"><text x="750" y="195">3.1</text><text x="930" y="195">3.2</text><text x="1110" y="195">3.3</text><text x="1290" y="195">3.4</text><text x="1470" y="195">3.5</text><text x="1650" y="195">3.6</text></g>
<rect x="1080" y="380" width="240" height="70" rx="35" fill="#e8e7fd"/><text x="1200" y="425">SQL DB 3</text>
<g stroke="#9aa1ad" stroke-width="2.5"><path d="M750 220L1150 380M930 220L1170 380M1110 220L1190 380M1290 220L1210 380M1470 220L1230 380M1650 220L1250 380"/></g>
</svg>

<!--
O resultado no diagrama de arquitetura: o Service 3, que fazia tudo, virou os serviços 3.1 a 3.6, cada um com uma responsabilidade.
-->

---

## O procedimento final

<ol class="steps">
<li>Migrar o <b>banco de dados</b></li>
<li>Migrar o <b>framework</b></li>
<li>Entender os <b>domínios</b> e separar</li>
<li><b>Documentar</b> software e decisões</li>
<li><b>Testes</b></li>
</ol>

<!--
Procedimento final:
1. Migrar o banco de dados.
2. Migrar o framework.
3. Entender os domínios e separar.
4. Documentar (software e decisões).
5. Testes.
-->

---

## E a equipe?

<div class="cols">
<div class="card accent">

### Comunicação
alinhada com outros setores

</div>
<div class="card accent">

### Suporte
espaço para aprender

</div>
<div class="card good">

### Medo → animação

</div>
<div class="card good">

### Evolução
pessoal e do time

</div>
</div>

<!--
Mas como a equipe reagiu?
1. Comunicação interna bem alinhada com outros setores da empresa.
2. Suporte técnico e espaço para aprendizado contínuo.
3. O medo virou animação.
4. Evolução pessoal e do time.
-->

---

<!-- _class: statement -->

> Vale a pena? Ainda tem muita coisa a evoluir, mas **o primeiro passo foi dado**.

<!--
Mas e aí, vale a pena? Hyperf × Laravel: depende do cenário (voltar à tabela de quando usar cada um).
Ainda tem muita coisa a evoluir, mas o primeiro passo foi dado.
-->

---

## Recomendações

<div class="cols3">
<div class="card">

### Entendendo Algoritmos
Aditya Bhargava

</div>
<div class="card accent">

### Arquitetura de Software: as Partes Difíceis
Ford · Richards · Sadalage · Dehghani

</div>
<div class="card">

### OO e SOLID para Ninjas
Maurício Aniche

</div>
</div>

<!--
Livros recomendados:
- Entendendo Algoritmos (Aditya Y. Bhargava).
- Arquitetura de Software: as Partes Difíceis (Neal Ford, Mark Richards, Pramod Sadalage, Zhamak Dehghani).
- Orientação a Objetos e SOLID para Ninjas (Maurício Aniche, Casa do Código).
-->

---

<!-- _paginate: false -->
<!-- _footer: '' -->

<div class="qr-slide">
<div>

<p class="kicker-dark">Obrigado!</p>

# Perguntas?

<div class="contacts">
<div><span class="contact-label">Site</span><span class="contact-value">luizschons.com</span></div>
<div><span class="contact-label">LinkedIn</span><span class="contact-value">linkedin.com/in/luiz-schons</span></div>
<div><span class="contact-label">GitHub</span><span class="contact-value">github.com/sschonss</span></div>
</div>

</div>
</div>

<!--
Abrir para perguntas. Deixar este slide na tela durante o Q&A.
-->
