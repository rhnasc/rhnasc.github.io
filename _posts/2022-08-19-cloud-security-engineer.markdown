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

<!-- {% linkpreview "https://blog.appsecco.com/an-ssrf-privileged-aws-keys-and-the-capital-one-breach-4c3c2cded3af" %} -->
<div class="jekyll-linkpreview-wrapper">
  <div class="jekyll-linkpreview-wrapper-inner">
    <div class="jekyll-linkpreview-content">
      <div class="jekyll-linkpreview-image">
        <a href="https://blog.appsecco.com/an-ssrf-privileged-aws-keys-and-the-capital-one-breach-4c3c2cded3af" target="_blank">
          <img src="https://miro.medium.com/v2/resize:fit:1200/1*xl_cr_m4ZP9ppUoWcMq54g.jpeg">
        </a>
      </div>

      <div class="jekyll-linkpreview-body">
        <h2 class="jekyll-linkpreview-title">
          <a href="https://blog.appsecco.com/an-ssrf-privileged-aws-keys-and-the-capital-one-breach-4c3c2cded3af" target="_blank">An SSRF, privileged AWS keys and the Capital One breach</a>
        </h2>
        <div class="jekyll-linkpreview-description">This post attempts to explain the technical side of how the Capital One breach occurred, the impact of the breach and what you can do as a…</div>
      </div>
    </div>
    <div class="jekyll-linkpreview-footer">
      <a href="//blog.appsecco.com" target="_blank">blog.appsecco.com</a>
    </div>
  </div>
</div>

Conhecimento técnico então te entrega meios para atingir esse fim. Risco é, ao mesmo tempo, complexo, contextual e dinâmico, mas entendê-lo te torna capaz de priorizar entregas de maneira confiante, em uma cadência que realmente faz sentido para a sua organização. _Big win_!

## Exercite suas coding skills

Nada melhor para ilustrar isso que um exemplo!

_Least-privilege_ é daqueles problemas que parece que nunca podem ser resolvidos de forma satisfatória - gera fricção entre segurança e desenvolvimento, adiciona erros, desacelera. Bom... Netflix tornou open source exemplos de soluções que desafiam essa concepção.

Trata-se de uma automação que entende o uso histórico das aplicações e remove gradualmente aqueles privilégios que ela não mais usa!

<!-- {% linkpreview "https://netflixtechblog.com/introducing-aardvark-and-repokid-53b081bf3a7e" %} -->
<div class="jekyll-linkpreview-wrapper">
  <div class="jekyll-linkpreview-wrapper-inner">
    <div class="jekyll-linkpreview-content">
      <div class="jekyll-linkpreview-image">
        <a href="https://netflixtechblog.com/introducing-aardvark-and-repokid-53b081bf3a7e" target="_blank">
          <img src="https://miro.medium.com/v2/resize:fill:463:244/g:fp:0.64:0.33/1*JSOImdkFx4CZIsSLkh4H0A.png">
        </a>
      </div>

      <div class="jekyll-linkpreview-body">
        <h2 class="jekyll-linkpreview-title">
          <a href="https://netflixtechblog.com/introducing-aardvark-and-repokid-53b081bf3a7e" target="_blank">Introducing Aardvark and Repokid</a>
        </h2>
        <div class="jekyll-linkpreview-description">AWS Least Privilege for Distributed, High-Velocity Development</div>
      </div>
    </div>
    <div class="jekyll-linkpreview-footer">
      <a href="//netflixtechblog.com" target="_blank">netflixtechblog.com</a>
    </div>
  </div>
</div>

Outro exemplo, um pipeline de detecção e _threat hunting_ usando ferramentas de dados:

<!-- {% linkpreview "https://www.linkedin.com/pulse/socless-detection-team-netflix-alex-maestretti/" %} -->
<div class="jekyll-linkpreview-wrapper">
  <div class="jekyll-linkpreview-wrapper-inner">
    <div class="jekyll-linkpreview-content">
      <div class="jekyll-linkpreview-image">
        <a href="https://www.linkedin.com/pulse/socless-detection-team-netflix-alex-maestretti" target="_blank">
          <img src="https://media.licdn.com/dms/image/C5612AQFRR0cif_iFmQ/article-cover_image-shrink_720_1280/0/1524001793005?e=2147483647&amp;v=beta&amp;t=QmHs4dp2Ito3FDxtDUUTKQPrmz0GZl310Qc-gKp0GLY">
        </a>
      </div>

      <div class="jekyll-linkpreview-body">
        <h2 class="jekyll-linkpreview-title">
          <a href="https://www.linkedin.com/pulse/socless-detection-team-netflix-alex-maestretti" target="_blank">A SOCless Detection Team at Netflix</a>
        </h2>
        <div class="jekyll-linkpreview-description">I am excited to share that we are investing in additional detection capabilities as part of the SIRT mission. There are a number of existing detection efforts across Netflix security teams.</div>
      </div>
    </div>
    <div class="jekyll-linkpreview-footer">
      <a href="//www.linkedin.com" target="_blank">www.linkedin.com</a>
    </div>
  </div>
</div>

Essas soluções só são possíveis porque, o time de CloudSec do Netflix é um time de _coders_. Ser um desenvolvedor confiante te empodera, por exemplo, com a capacidade de desenvolver uma automação para o contexto do seu desafio, ou de adaptar uma solução existente em um _fork_.

Saiba usar bem uma ou duas linguagens de programação (Python, Golang), testes automatizados, IaC (Terraform, CDK, Pulumi), entenda pipelines de desenvolvimento, containers, pipelines de dados! Tudo isso te fará um profissional mais capaz. 😁

## Interesse por Baixo Nível

Uma coisa que não dá pra escapar trabalhando em CloudSec são discussões envolvendo elementos de baixo nível.
Principalmente nos primeiros anos de experiência, o baixo nível parece indomável - sempre um novo conceito, protocolo, comportamento, de baixo nível que aparece que você nem imaginava existir. O segredo aqui é manter um _learner mindset_ - separar aquele tempo de qualidade para ler e entender aquilo, sem se sentir desencorajado.

Isso é especialmente importante porque, hoje, várias das mais importantes features de segurança são entregues a nível de infraestrutura. Um eBPF que permite observabilidade de conexões, um service mesh baseado em Envoy que te permite construir identidade de serviços em produção.

<!-- {% linkpreview "https://spiffe.io/" %} -->
<div class="jekyll-linkpreview-wrapper">
  <div class="jekyll-linkpreview-wrapper-inner">
    <div class="jekyll-linkpreview-content">
      <div class="jekyll-linkpreview-image">
        <a href="https://spiffe.io/" target="_blank">
          <img src="https://spiffe.io/img/spiffe-turtle.svg">
        </a>
      </div>

      <div class="jekyll-linkpreview-body">
        <h2 class="jekyll-linkpreview-title">
          <a href="https://spiffe.io/" target="_blank">SPIFFE – Secure Production Identity Framework for Everyone</a>
        </h2>
        <div class="jekyll-linkpreview-description">SPIFFE and SPIRE provide strongly attested, cryptographic identities to workloads across a wide variety of platforms</div>
      </div>
    </div>
    <div class="jekyll-linkpreview-footer">
      <a href="//spiffe.io" target="_blank">spiffe.io</a>
    </div>
  </div>
</div>

O baixo nível demanda algum interesse, humildade, e experimentação constante. Mas vale o desafio!

## Conclusão

Esses três elementos para mim compõem o canivete-suíço para um bom desenvolvimento na disciplina de CloudSec, compõem as dimensões a serem procuradas em um candidato ou na construção de um time. Se você gostou da leitura, aqui vão links de outros artigos meus, obrigado!
