---
layout: post
title:  "Do que é feito um bom Cloud Security Engineer"
date:   2022-08-19 03:00:00 -0300
categories: cloud security
---

<link rel="stylesheet" href="/assets/css/linkpreview.css">

Hoje, há poucas disciplinas em computação nas quais um colaborador conseguiria entregar mais impacto do que em _Cloud Security Engineering_ (ou, Engenharia de Segurança na Nuvem).

Isso porque, na era da informação, onde tudo é rapidamente escalável, trata-se do desafio de entregar também segurança com essa mesma velocidade, ainda mantendo as premissas de alta produtividade e experimentação que a nuvem pode implementar.

Existe, contudo, uma falsa resposta a esse desafio - bastaria eu instalar esse ou aquele _vendor_ na minha nuvem e ela estará segura, certo?

Na verdade, além de uma resposta errada, esse é um sintoma - a cada vendor que promete resolver esse desafio ponta-a-ponta, eu penso que CloudSec ainda é pouco entendida pelo mercado - ora, não há vendors que prometem resolver seu "Site Reliability Engineering", "Data Engineering", ou "Software Engineering" todo de uma vez, certo? Seu _bullshit detector_ apitaria no mesmo momento.

Para entender a disciplina para então construir ou compor um time páreo para o desafio, vale o exercício de tentar analisar quais dimensões tornam uma pessoa capaz de entregar bem dentro de um time de CloudSec. Aqui vai o meu palpite:

## Entenda Risco

> Risco = Probabilidade x Impacto

O conhecimento técnico em Cloud e em Sec (CloudSec🤪) é útil exatamente dentro da finalidade de se entender essas duas variáveis que compõem o risco.

Tomemos por exemplo o conhecido hack do CapitalOne (2019). O post abaixo fala em _SSRF_, _IMDS_ e _privileged AWS keys_. Se entendermos, por exemplo, a forma como um SSRF é introduzido e explorado, vamos entender mais do que apenas o mecanismo de ataque - entendemos melhor o potencial **risco** dessa vulnerabilidade nossa organização.

{% linkpreview "https://blog.appsecco.com/an-ssrf-privileged-aws-keys-and-the-capital-one-breach-4c3c2cded3af" %}

Conhecimento técnico então te entrega meios para atingir esse fim. Risco é, ao mesmo tempo, complexo, contextual e dinâmico, mas entendê-lo te torna capaz de priorizar entregas de maneira confiante, em uma cadência que realmente faz sentido para a sua organização. _Big win_!

## Exercite suas coding skills

Nada melhor para ilustrar isso que um exemplo!

_Least-privilege_ é daqueles problemas que parece que nunca podem ser resolvidos de forma satisfatória - gera fricção entre segurança e desenvolvimento, adiciona erros, desacelera. Bom... Netflix tornou open source exemplos de soluções que desafiam essa concepção.

Trata-se de uma automação que entende o uso histórico das aplicações e remove gradualmente aqueles privilégios que ela não mais usa!

{% linkpreview "https://netflixtechblog.com/introducing-aardvark-and-repokid-53b081bf3a7e" %}

Outro exemplo, um pipeline de detecção e _threat hunting_ usando ferramentas de dados:

{% linkpreview "https://www.linkedin.com/pulse/socless-detection-team-netflix-alex-maestretti/" %}

Essas soluções só são possíveis porque, o time de CloudSec do Netflix é um time de _coders_. Ser um desenvolvedor confiante te empodera, por exemplo, com a capacidade de desenvolver uma automação para o contexto do seu desafio, ou de adaptar uma solução existente em um _fork_.

Saiba usar bem uma ou duas linguagens de programação (Python, Golang), testes automatizados, IaC (Terraform, CDK, Pulumi), entenda pipelines de desenvolvimento, containers, pipelines de dados! Tudo isso te fará um profissional mais capaz. 😁

## Interesse por Baixo Nível

Uma coisa que não dá pra escapar trabalhando em CloudSec são discussões envolvendo elementos de baixo nível.
Principalmente nos primeiros anos de experiência, o baixo nível parece indomável - sempre um novo conceito, protocolo, comportamento, de baixo nível que aparece que você nem imaginava existir. O segredo aqui é manter um _learner mindset_ - separar aquele tempo de qualidade para ler e entender aquilo, sem se sentir desencorajado.

Isso é especialmente importante porque, hoje, várias das mais importantes features de segurança são entregues a nível de infraestrutura. Um eBPF que permite observabilidade de conexões, um service mesh baseado em Envoy que te permite construir identidade de serviços em produção.

{% linkpreview "https://spiffe.io/" %}

O baixo nível demanda algum interesse, humildade, e experimentação constante. Mas vale o desafio!

## Conclusão

Esses três elementos para mim compõem o canivete-suíço para um bom desenvolvimento na disciplina de CloudSec, compõem as dimensões a serem procuradas em um candidato ou na construção de um time. Se você gostou da leitura, aqui vão links de outros artigos meus, obrigado!
