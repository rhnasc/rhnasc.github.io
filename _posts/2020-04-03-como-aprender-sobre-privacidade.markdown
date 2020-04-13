---
layout: post
title:  "Como aprender sobre privacidade, sendo um engenheiro de software"
date:   2020-04-03 02:14:02 -0300
categories: automation declarative iac aws
---

> **Nota:** esta é uma reprodução originalmente publicada no Linkedin ([link](https://www.linkedin.com/pulse/como-aprender-sobre-privacidade-sendo-um-engenheiro-de-nascimento/))

![Photo by Ilyuza Mingazova]({{ "/assets/2020-04-03/window_blind.jpg" | absolute_url }})

Quer dizer que você quer entender mais sobre **privacidade** - mas, para muitos, esse termo pode parecer algo tão etéreo que nem fica claro por onde começar!

Eu te entendo muito bem. E vou te dar uma ajuda.

Privacidade, para início de conversa, é o **direito humano** à reserva da sua própria vida pessoal.

No meio digital, com um alto tráfego e volume de dados, junto à possibilidade de uma retenção de informação virtualmente *infinita*, vemos com razão a preocupação social crescente sobre o tema, e que se concretiza em novas leis para a proteção de dados pessoais no mundo inteiro.

No Brasil, vemos a **Lei Geral de Proteção de Dados (LGPD)** entrando em vigor - [talvez?](https://www.camara.leg.br/noticias/626827-proposta-adia-para-2022-a-vigencia-da-lei-geral-de-protecao-de-dados-pessoais/) - em Agosto. Já na Europa, a **General Data Protection Regulation (GDPR)** se encontra ativa desde 2018.

Tais leis enforçam uma série de direitos do próprio usuário sobre o seu dado coletado ou processado, além de uma série de processos e elementos organizacionais que devem estar presentes na sua empresa.

Mas antes de falar sobre essas leis, vamos começar abrindo um ponto de discussão...

### Passo 1: Abra o Netflix 📺

<iframe width="560" height="315" src="https://www.youtube.com/embed/iX8GxLP1FHo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

**The Great Hack** (2019) trata do escândalo protagonizado pelo Facebook e a Cambridge Analytica, quando dados pessoais coletados a partir da rede social foram usados por cientistas de dados para criar conteúdos direcionados para mais de 200 campanhas políticas ao redor do mundo ([fonte](https://www.nation.co.ke/news/politics/How-Cambridge-Analytica-influenced-Kenyan-poll/1064-4349034-le7xbuz/index.html)) - inclusive para a campanha presidencial dos EUA em 2016.

Como profissionais de tecnologia, assistir esse documentário nos deixa com uma série de reflexões, afinal, como empresas que possuem dados pessoais - virtualmente _qualquer_ empresa de software que tenha ao menos 1 cliente ou colaborador - podem ter certeza que respeitam a privacidade? E como transformar essa linha, aparentemente tênue, que separa o respeito a esse direito em algo mais concreto, bem definido?

No final desse filme, antes de olhar para as legislações, recomendo essas perguntas de auto reflexão:

1. Que direitos as pessoas têm sobre os dados que dizem respeito a elas mesmas?
2. Quais são as nossas responsabilidades enquanto retentores e processadores desses dados?
3. Como proteger ou anonimizar esses dados, e quando apagá-los, garantindo assim a privacidade das pessoas?

### Passo 2: Aprenda os termos do GDPR 📚

Felizmente, hoje temos uma *bottom-line* de privacidade, a GDPR. 🇪🇺

A GDPR, além de proteger dados de residentes no espaço econômico europeu, estabeleceu uma **linguagem universal** para se falar em proteção de dados, que impacta qualquer que seja seu mercado de atuação.

Ela é imperativa ao definir direitos do **titular** (que é quem o dado pessoal diz respeito), bases legais para a coleta e processamento, e uma **terminologia** muito bem definidos sobre dados. Você deve aprender o que é um **processador** ou **coletor de dados**. Qual a diferença entre as técnicas de **pseudonimização** e **anonimização**.

![Bandeira da União Europeia. Photo by Markus Spiske on Unsplash.]({{ "/assets/2020-04-03/eu.jpg" | absolute_url }})

Ora, mas estou no Brasil, não seria melhor eu começar a aprender logo a LGPD?

Em parte, mas os melhores materiais disponíveis ainda são sobre GDPR. Como a LGPD foi inspirada na sua prima da Europa, você não vai ter muitas dificuldades em compreender as diferenças a partir do seu aprendizado sobre GDPR.

Além disso, a comunidade de software livre, bem como *vendors*, se comunicam com base nos termos definidos na GDPR. Então faz sentido sim tê-la como "primeira língua" em proteção de dados.

Tá certo. Mas por onde começo?

Se você, assim como eu, não tem a menor fluência em textos jurídicos, baixar a GDPR (em qualquer uma das 24 línguas oficiais da UE) não vai te proporcionar um aprendizado interessante.

Nesse sentido, recomendo utilizar plataformas online de aprendizado, e a minha primeira indicação é [GDPR - The Big Picture](https://app.pluralsight.com/courses/gdpr-big-picture), disponível no *Pluralsight*.

### Passo 3: Exercite seus conhecimentos sobre cibersegurança 🎩

![Homem operando um computador. Photo by freestocks on Unsplash.]({{ "/assets/2020-04-03/man_in_computer.jpg" | absolute_url }})

Ok, na verdade, esse ponto pode demorar sua vida profissional inteira. 😅

A garantia de privacidade também passa por garantir a **confidencialidade** e **integridade** dos dados pessoais - ou seja, que ele não seja exposto de maneira indevida (vazamentos) ou mesmo alterado ou excluído por um invasor.

Para implementar bons padrões de mercado para proteger os dados confiados à sua empresa, a mesma precisa possuir uma **cultura de segurança cibernética**.

Infelizmente, uma grande dificuldade para implantar essa cultura é a falta de expertise na área - pois, a não ser que você tenha trabalhado em uma empresa especializada, não há grandes chances de que você considere pelo menos proficiente nessa área.

E a culpa disso certamente não é a falta de interesse. Talvez seja a impressão geral de que esse conhecimento é inacessível, ou muito difícil de adquirir.

Felizmente, essa imagem tem mudado de forma acelerada e mais pessoas sentem-se confortáveis em inserir segurança nas suas rotinas de estudo e nos seus times de engenharia, como indica o [2019 State of DevOps Report](https://puppet.com/resources/report/state-of-devops-report/).

Então vamos pôr a mão na massa? 👩‍💻 Aqui vão algumas dicas:

1. Pesquisar que ferramentas você pode colocar no seu *pipeline* de entrega que te forneçam informações sobre a segurança do seu software;
2. Participar da comunidade, mesmo que você não seja um *expert*: conferências e jogos como *Capture the Flag*;
3. Atualizar os softwares e dependências com os quais você trabalha (talvez o melhor *ROI* em segurança seja manter tudo sempre atualizado), mas não esquecer de tentar entender os *changelogs* de todos os security fixes 🤓;
4. Discutir os resultados de *penetration tests* de forma coletiva, revisitando os problemas através de *blameless postmortems*.

### That's it! 🔒

Muito obrigado por chegar até aqui!

Espero que esses passos te ajudem a se tornar, além de um profissional melhor informado, um cidadão digital que entende bem o conceito de **privacidade**.
