---
marp: true
theme: aifa
paginate: true
lang: en
title: High-Performance PHP with Hyperf
description: An introduction to Hyperf, Swoole and a real migration from Laravel
author: Luiz Schons
footer: 'High-Performance PHP with Hyperf · luizschons.com'
---

<!-- _class: cover -->
<!-- _paginate: false -->
<!-- _footer: '' -->

# High-Performance PHP with Hyperf

Accelerating microservice architecture with PHP

<br>

**Luiz Schons**

<!--
This talk merges two earlier versions: "Accelerating Microservice Architecture with PHP — an introduction to Hyperf and Distributed Systems" and "Efficient Migration: from Laravel to Hyperf".
Outline: first undo the fear of PHP, then understand runtimes and Swoole, then Hyperf in code and, finally, a real migration from Laravel to Hyperf.
Transition: "First, let me introduce myself."
-->

---

<div class="about">
<div>

# Luiz Schons

<p class="role">Senior Software Engineer @ <b>PicPay</b></p>

<div class="chips">
<span>Guarapuava, Paraná, Brazil</span>
<span>DevParaná</span>
<span>UTFPR</span>
<span>PHP</span>
</div>

<p class="site">luizschons.com</p>

</div>
<img class="photo" src="assets/luiz.jpg" alt="Luiz Schons">
</div>

<!--
Quick intro (30 seconds): who I am, where I work, where I'm from, DevParaná.
Transition: "Before we start, a disclaimer."
-->

---

## Disclaimer

<ol class="steps">
<li>I'm not an <b>expert</b> in anything</li>
<li>Analyze <b>your scenario</b></li>
<li>There's no <b>silver bullet</b></li>
<li>Study, and study…</li>
</ol>

<!--
1. I'm not an expert in anything: I'll show what I studied and lived.
2. Analyze the scenario you're in: what worked for us may not fit you.
3. There's no silver bullet: Hyperf doesn't solve everything.
4. Study, and study…
-->

---

## Common ground

<div class="cols3">
<div class="card accent">

### API
Application Programming Interface

</div>
<div class="card">

### Microservices

</div>
<div class="card">

### Distributed systems

</div>
</div>

<br>

<p class="muted">Let's draw a bit to understand it better</p>

<!--
Align the terms before moving on:
- API (Application Programming Interface).
- Microservices.
- Distributed systems.
In the original, I drew live here. Sketch (on a board or with your hands) how one API talks to another, and how several services form a distributed system.
-->

---

<!-- _class: divider -->

<div class="kicker">Part 1 · The urban legend</div>

# Who's afraid of PHP?

<!--
Let's understand this urban legend a bit.
-->

---

## Why the fear

<div class="cols">
<div class="card bad">

### Past
Bad practices · Security

</div>
<div class="card warn">

### Change
Many language changes

</div>
</div>

<br>

<div class="card">

### Performance
vs. Ruby, Python and Java

</div>

<!--
Why are people afraid of PHP?
1. A history of bad practices.
2. Security.
3. Many changes in the language.
4. Performance, compared with Ruby, Python and Java.
-->

---

<!-- _class: statement -->

> That has **changed**.

<!--
Pause. All of that is the past.
-->

---

## PHP today

<div class="chips accent">
<span>JIT · 7.x / 8.x</span>
<span>Strict Typing</span>
<span>Null Coalesce</span>
<span>Closures</span>
<span>Generators</span>
<span>Security</span>
<span>Composer</span>
<span>PHPUnit · Pest</span>
<span>Laravel · Symfony · Zend · Hyperf</span>
</div>

<br>

<p class="muted">And… different <b>runtimes</b>?</p>

<!--
1. Better performance (7.x and 8.x) with JIT.
2. Modern syntax and features: strict typing, null coalescing operator, anonymous functions, closures, generators and much more.
3. Improved security.
4. Easier development tools and integrations: Composer, PHPUnit, Pest.
5. Modern frameworks: Laravel, Symfony, Zend, Hyperf.
6. Different runtimes?????
Transition: "But what are runtimes?"
-->

---

<!-- _class: statement -->

> But what are **runtimes**?

<!--
Examples on the original slide: PHP-FPM, RoadRunner, Swoole.
-->

---

## Runtime

<div class="card big accent">

Where PHP code is **executed**

</div>

<br>

<div class="chips">
<span>Interpretation</span>
<span>Memory</span>
<span>I/O</span>
<span>Operating system</span>
</div>

<!--
Concept: a runtime is the environment or infrastructure where PHP code runs.
It handles interpreting and executing the script, manages memory, input/output (I/O) and other interactions with the operating system and the application's resources.
Examples: PHP-FPM, RoadRunner, Swoole.
-->

---

## Understanding Swoole

<div class="cols3">
<div class="card accent">

### Asynchronous
high performance

</div>
<div class="card accent">

### Coroutines

</div>
<div class="card accent">

### Scale
more throughput

</div>
</div>

<br>

<p class="muted">A PHP runtime</p>

<!--
Swoole is a PHP runtime that enables high-performance asynchronous programming, with coroutine support, significantly improving the throughput and scalability of web applications.
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
Compare the two models (diagrams from both original decks).
FPM: Nginx has a master process; each request goes to a worker, which calls PHP-FPM, which runs index.php, loads the autoloader, starts/loads the framework and hits the database. On every request.
Swoole: a master process with reactors, a manager, and workers that stay up, talk to each other and load the PHP files, which access the database.
Key point: with FPM the framework boots on every request; with Swoole the workers stay alive.
-->

---

## Synchronous

<svg class="network" viewBox="0 0 1776 520" font-family="Inter, sans-serif" font-size="32">
<g fill="#6b7280" font-weight="700"><text x="0" y="80">THREAD 1</text><text x="0" y="200">THREAD 2</text><text x="0" y="320">THREAD 3</text><text x="0" y="440">THREAD 4</text></g>
<g stroke="#e5e7eb" stroke-width="2"><path d="M220 70H1760M220 190H1760M220 310H1760M220 430H1760"/></g>
<rect x="240" y="290" width="500" height="40" rx="10" fill="#4f46e5"/>
<rect x="750" y="290" width="250" height="40" rx="10" fill="#818cf8"/>
<rect x="1010" y="290" width="740" height="40" rx="10" fill="#4f46e5"/>
<g font-size="28" text-anchor="middle"><text x="490" y="380">10 s</text><text x="875" y="380">5 s</text><text x="1380" y="380">15 s</text></g>
</svg>

<p class="muted">One task after another</p>

<!--
Synchronous model: tasks run on a single thread, one after another. 10 seconds, then 5, then 15: it only finishes when all of them finish in sequence.
-->

---

## Asynchronous, concurrent

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

<p class="muted">Tasks take turns while they wait</p>

<!--
Asynchronous concurrent: tasks interleave. While one waits (I/O), another moves forward. Times from the original slide: 2, 3, 3, 3 and 5 seconds, alternating between threads.
This is where Swoole's coroutines shine.
-->

---

## Asynchronous, parallel

<svg class="network" viewBox="0 0 1776 520" font-family="Inter, sans-serif" font-size="32">
<g fill="#6b7280" font-weight="700"><text x="0" y="80">THREAD 1</text><text x="0" y="200">THREAD 2</text><text x="0" y="320">THREAD 3</text><text x="0" y="440">THREAD 4</text></g>
<g stroke="#e5e7eb" stroke-width="2"><path d="M220 70H1760M220 190H1760M220 310H1760M220 430H1760"/></g>
<rect x="240" y="50" width="600" height="40" rx="10" fill="#4f46e5"/>
<rect x="240" y="170" width="600" height="40" rx="10" fill="#818cf8"/>
<rect x="240" y="290" width="600" height="40" rx="10" fill="#4f46e5"/>
<g font-size="28" text-anchor="middle"><text x="540" y="130">5 s</text><text x="540" y="250">5 s</text><text x="540" y="370">5 s</text></g>
</svg>

<p class="muted">At the same time, on different threads</p>

<!--
Asynchronous parallel: tasks run at the same time on different threads. Three 5-second tasks finish together.
-->

---

<!-- _class: statement -->

> Do I have to do **all of this by hand**?

<!--
Rhetorical question. No: that's what Hyperf is for.
-->

---

<!-- _class: divider -->

<div class="kicker">Part 2 · The framework</div>

# Hyperf

<!--
Hyperf = Hyperspeed + Flexibility. "The Way to PHP Microservice".
-->

---

## Hyperf = Hyperspeed + Flexibility

<div class="cols">
<div class="card accent">

### Modular architecture

</div>
<div class="card accent">

### Performance

</div>
<div class="card">

### Non-blocking I/O
with Swoole

</div>
<div class="card">

### Broad integrations

</div>
</div>

<br>

<p class="muted">"The Way to PHP Microservice"</p>

<!--
Hyperf's website calls it "The Way to PHP Microservice", and the name comes from Hyperspeed + Flexibility.
- Modular architecture.
- Performance.
- Non-blocking I/O with Swoole.
- Broad integrations.
-->

---

## A controller

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
A controller in Hyperf. It looks a lot like what we already know.
Comment from the original code: the related objects (RequestInterface and ResponseInterface) are injected automatically by the dependency injection container.
-->

---

## A model

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
The model: Laravel developers feel at home. $table, $fillable and $casts, just like Eloquent.
The original slide had the docblocks (@property id, name, gender, created_at, updated_at and a description of each attribute); here only the essentials remain.
-->

---

## Routes

<style scoped>pre { font-size: 32px; }</style>

```php
<?php
use Hyperf\HttpServer\Router\Router;

// Any of the following three definitions can achieve the same effect
Router::get('/hello-hyperf', 'App\Controller\IndexController::hello');
Router::get('/hello-hyperf', 'App\Controller\IndexController@hello');
Router::get('/hello-hyperf', [App\Controller\IndexController::class, 'hello']);
```

<p class="muted">Three ways, same effect</p>

<!--
Routes: any of the three definitions has the same effect. A string with ::, a string with @ (Laravel style) or an array with the class and method.
-->

---

## Hyperf × Laravel

<div class="cols3">
<div class="card">

### Performance
<span class="good">Hyperf ✓</span> · Laravel ✗

</div>
<div class="card">

### Community
Hyperf ✗ · <span class="good">Laravel ✓</span>

</div>
<div class="card">

### Learning curve
Hyperf ✓ · Laravel ✓

</div>
</div>

<!--
Quick comparison from the original deck:
- Performance: point for Hyperf.
- Community: point for Laravel.
- Learning curve: both do well; if you know Laravel you can learn Hyperf.
-->

---

## When to use each

<style scoped>
table { font-size: 27px; width: 100%; }
th, td { padding: 9px 18px; }
</style>

| Criterion | Laravel | Hyperf |
|---|---|---|
| Size | Small to medium | Medium to large |
| Architecture | MVC | Microservices, coroutines |
| Performance | Limited under high load | Exceptional |
| Scalability | Traditional web | Distributed systems |
| Ease of use | Vast docs and community | Needs async and Swoole |
| Microservices | May not be efficient | Native: gRPC, REST |
| Ecosystem | Large | Growing |
| Use cases | CMS, e-commerce, dashboards | High-load APIs, real time |

<!--
Table from the migration deck:
- Application size: Laravel, small to medium (blogs, CMS, mid-size e-commerce). Hyperf, medium to large (microservices, high-load APIs).
- Architecture: Laravel, MVC pattern, fast development and maintenance. Hyperf, microservice-oriented, with coroutines for high concurrency.
- Performance: Laravel fits traditional applications, limited under high load. Hyperf is exceptional, handling millions of requests per second.
- Scalability: Laravel fits most traditional web applications. Hyperf, highly scalable, ideal for distributed systems and high availability.
- Ease of development: Laravel, expressive syntax, vast documentation and an active community. Hyperf requires prior knowledge of asynchronous programming and Swoole.
- Microservice support: Laravel supports it, but may not be as efficient under high load. Hyperf has native support, gRPC, REST and other distributed architectures.
- Ecosystem: Laravel, large community and package ecosystem. Hyperf, growing community, focused on high performance.
- Use cases: Laravel for traditional web, CMS, mid-size e-commerce, blogs, admin dashboards. Hyperf for high-performance APIs, microservices, high concurrency and low latency, real time (chat, push).
-->

---

<!-- _class: divider -->

<div class="kicker">Part 3 · In practice</div>

# Migration: from Laravel to Hyperf

<!--
Software migration: a bit of what I've been through.
-->

---

## What was the scenario?

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
Let's understand this story a bit. What scenario were we in?
A front end calling three main services (each with its database: SQL 1, SQL 2, SQL 3 and a noSQL one), and those services calling a chain of other services (4 to 8). Lots of cross dependencies.
In the original deck I also showed real screens of the system and highlighted Service 3.
-->

---

## Why migrate?

<div class="cols">
<div class="card bad">

### Past
Poor past decisions

</div>
<div class="card bad">

### Fear
Coding on eggshells

</div>
<div class="card warn">

### Performance
not Laravel's fault

</div>
<div class="card warn">

### A micro that wasn't micro
Domains · tense deploys

</div>
</div>

<!--
Reasons:
1. Poor decisions in the past.
2. Coding on eggshells.
3. Performance (and it wasn't Laravel's fault).
4. A microservice that wasn't micro (mixed domains).
5. Deploys were tense.
-->

---

## Why Hyperf?

<div class="cols">
<div class="card accent">

### Team
Everyone knew PHP

</div>
<div class="card accent">

### Coroutines
Non-blocking

</div>
<div class="card">

### Challenge

</div>
<div class="card">

### Performance
Swoole

</div>
</div>

<!--
Reasons:
1. The whole team knew PHP.
2. Coroutines and a non-blocking system.
3. Challenge.
4. Performance (Swoole).
-->

---

## How we planned

<ol class="steps">
<li>Goal: <b>better DX</b></li>
<li>Understand the <b>domains</b></li>
<li>An <b>action plan</b></li>
</ol>

<p class="muted">…but it wasn't all roses</p>

<!--
1. We defined where we wanted to get: software with a better developer experience (DX).
2. We understood the application's domains better.
3. We created an action plan.
But it wasn't all roses…
-->

---

## First challenge: the database

<div class="flow">
<div class="wait">MongoDB</div><span>→</span>
<div class="ok">PostgreSQL</div>
</div>

<br>

<ol class="steps">
<li>Migrate the <b>database</b></li>
<li>Migrate the <b>framework</b></li>
</ol>

<!--
The first challenge began: migrating the database (on the original slide, MongoDB to PostgreSQL).
Procedure: 1st migrate the database. 2nd migrate the framework.
-->

---

<!-- _class: statement -->

> Second challenge: how do you define **domains**?

<!--
The second challenge began. A big service that needed to become several. But how do you define domains?
-->

---

## Elephant Migration

<div class="card big bad">

Anti-pattern: migrating **everything at once**

</div>

<br>

<p class="muted">"Architecture: just wing it"</p>

<!--
Elephant Migration AntiPattern: tentar migrar o elefante inteiro de uma vez.
Meme do deck original: o livro "Architecture: just wing it".
-->

---

## Two paths

<div class="cols">
<div class="card accent">

### Component-based decomposition
Refactoring · component extraction · incremental and controlled

</div>
<div class="card">

### Tactical forking
Service replicas · removing unwanted parts

</div>
</div>

<br>

<p class="muted">Software Architecture: The Hard Parts</p>

<!--
Tactical forking or component-based decomposition?
- Component-based decomposition: refactoring, component extraction, incremental and controlled.
- Tactical forking: service replicas and removing the unwanted parts.
Reference: the book "Software Architecture: The Hard Parts — Modern Trade-Off Analyses for Distributed Architectures" (Neal Ford, Mark Richards, Pramod Sadalage, Zhamak Dehghani), with its decision flowchart between the two approaches.
-->

---

## Third challenge: documentation

<div class="flow">
<div class="bad">Outdated documentation</div><span>→</span>
<div class="ok">Documentation + ADR</div>
</div>

<br>

<p class="muted">Architecture Decision Records</p>

<!--
We found a problem: outdated documentation.
The third challenge began. The answer: documentation + Architecture Decision Records (ADR), documenting the software and the decisions.
-->

---

## Back to the second challenge

<div class="cols3">
<div class="card bad">

### Responsibilities
too many for one "micro" service

</div>
<div class="card bad">

### Coupling
tight across domains

</div>
<div class="card bad">

### Deploys
hit production hard

</div>
</div>

<!--
Back to the second challenge. The service's problems:
1. Too many responsibilities for a single "microservice".
2. Very tight coupling between different domains of the application.
3. Deploys had a big impact on production.
-->

---

## One service → many, via messaging

<svg class="network" viewBox="0 0 1776 560" font-family="Inter, sans-serif" font-size="32" text-anchor="middle">
<rect x="60" y="170" width="320" height="220" rx="27" fill="#f59e0b"/>
<text x="220" y="290" fill="#fff" font-weight="800">Service</text>
<path d="M440 280H640" stroke="#4f46e5" stroke-width="10"/><path d="M640 250l50 30-50 30z" fill="#4f46e5"/>
<rect x="1060" y="240" width="300" height="80" rx="40" fill="#ff6600"/><text x="1210" y="291" fill="#fff" font-weight="800">RabbitMQ</text>
<g fill="#fde68a"><rect x="760" y="40" width="220" height="120" rx="20"/><rect x="1440" y="40" width="220" height="120" rx="20"/><rect x="760" y="400" width="220" height="120" rx="20"/><rect x="1440" y="400" width="220" height="120" rx="20"/></g>
<g font-size="26"><text x="870" y="110">Service</text><text x="1550" y="110">Service</text><text x="870" y="470">Service</text><text x="1550" y="470">Service</text></g>
<g stroke="#111827" stroke-width="3"><path d="M980 140L1100 240M1440 140L1320 240M980 420L1100 320M1440 420L1320 320"/></g>
</svg>

<!--
The solution: break the big service into smaller services that talk through messaging, with RabbitMQ in the middle.
-->

---

## Consumer with Hyperf AMQP

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
A consumer in Hyperf: an annotation (attribute) with exchange, routing key, queue and number of consumers. The consumeMessage method receives the data and returns Result::ACK.
-->

---

## Before × after

<svg class="network" viewBox="0 0 1776 600" font-family="Inter, sans-serif" font-size="26" text-anchor="middle">
<g fill="none" stroke="#c9ced8" stroke-width="2" stroke-dasharray="8 6">
<rect x="20" y="60" width="560" height="500" rx="20"/>
<rect x="640" y="60" width="1116" height="500" rx="20"/>
</g>
<text x="300" y="40" fill="#6b7280" font-weight="700">Before</text>
<text x="1198" y="40" fill="#4f46e5" font-weight="700">After</text>
<rect x="180" y="150" width="240" height="70" rx="14" fill="#f59e0b"/><text x="300" y="195" fill="#fff" font-weight="700">Service 3</text>
<rect x="180" y="380" width="240" height="70" rx="35" fill="#e8e7fd"/><text x="300" y="425">SQL DB 3</text>
<path d="M300 220v160" stroke="#9aa1ad" stroke-width="3"/>
<g fill="#4f46e5"><rect x="670" y="150" width="160" height="70" rx="14"/><rect x="850" y="150" width="160" height="70" rx="14"/><rect x="1030" y="150" width="160" height="70" rx="14"/><rect x="1210" y="150" width="160" height="70" rx="14"/><rect x="1390" y="150" width="160" height="70" rx="14"/><rect x="1570" y="150" width="160" height="70" rx="14"/></g>
<g fill="#fff"><text x="750" y="195">3.1</text><text x="930" y="195">3.2</text><text x="1110" y="195">3.3</text><text x="1290" y="195">3.4</text><text x="1470" y="195">3.5</text><text x="1650" y="195">3.6</text></g>
<rect x="1080" y="380" width="240" height="70" rx="35" fill="#e8e7fd"/><text x="1200" y="425">SQL DB 3</text>
<g stroke="#9aa1ad" stroke-width="2.5"><path d="M750 220L1150 380M930 220L1170 380M1110 220L1190 380M1290 220L1210 380M1470 220L1230 380M1650 220L1250 380"/></g>
</svg>

<!--
The result in the architecture diagram: Service 3, which did everything, became services 3.1 to 3.6, each with one responsibility.
-->

---

## The final procedure

<ol class="steps">
<li>Migrate the <b>database</b></li>
<li>Migrate the <b>framework</b></li>
<li>Understand the <b>domains</b> and split</li>
<li><b>Document</b> software and decisions</li>
<li><b>Tests</b></li>
</ol>

<!--
Final procedure:
1. Migrate the database.
2. Migrate the framework.
3. Understand the domains and split them.
4. Document (software and decisions).
5. Tests.
-->

---

## And the team?

<div class="cols">
<div class="card accent">

### Communication
aligned with other areas

</div>
<div class="card accent">

### Support
room to learn

</div>
<div class="card good">

### Fear → excitement

</div>
<div class="card good">

### Growth
personal and team

</div>
</div>

<!--
But how did the team react?
1. Internal communication well aligned with other areas of the company.
2. Technical support and room for continuous learning.
3. Fear turned into excitement.
4. Personal and team growth.
-->

---

<!-- _class: statement -->

> Worth it? There's still a lot to improve, but **the first step was taken**.

<!--
So, is it worth it? Hyperf × Laravel: it depends on the scenario (go back to the when-to-use table).
There's still a lot to improve, but the first step was taken.
-->

---

## Recommendations

<div class="cols3">
<div class="card">

### Grokking Algorithms
Aditya Bhargava

</div>
<div class="card accent">

### Software Architecture: The Hard Parts
Ford · Richards · Sadalage · Dehghani

</div>
<div class="card">

### OO and SOLID for Ninjas
Maurício Aniche

</div>
</div>

<!--
Recommended books:
- Grokking Algorithms (Aditya Y. Bhargava).
- Software Architecture: The Hard Parts (Neal Ford, Mark Richards, Pramod Sadalage, Zhamak Dehghani).
- Orientação a Objetos e SOLID para Ninjas (Maurício Aniche, Casa do Código; Portuguese only).
-->

---

<!-- _paginate: false -->
<!-- _footer: '' -->

<div class="qr-slide">
<div>

<p class="kicker-dark">Thank you!</p>

# Questions?

<div class="contacts">
<div><span class="contact-label">Site</span><span class="contact-value">luizschons.com</span></div>
<div><span class="contact-label">LinkedIn</span><span class="contact-value">linkedin.com/in/luiz-schons</span></div>
<div><span class="contact-label">GitHub</span><span class="contact-value">github.com/sschonss</span></div>
</div>

</div>
</div>

<!--
Open for questions. Keep this slide on screen during the Q&A.
-->
