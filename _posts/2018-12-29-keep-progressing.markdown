---
layout: post
title:  "Keep progressing when you are no longer a novice in software engineering"
date:   2018-12-29 17:15:08 -0300
categories: career english
---

We have seen the word passionate on 11 out of 10 software engineering resumes.

There is some possibility that half of those resumes were not lying. I have been there myself. 

I had weeks on which I would pull multiple all nighters on college, and I asked myself how staying alive out of coffee and power naps could be so fun.

![I am committed to my art]({{ "/assets/committed_to_my_art.png" | absolute_url }})

[Evidence shows](https://80000hours.org/career-guide/job-satisfaction/) that engaging work is the top ingredient you need to look for on your search for the dream job. Nothing makes our work better than that feeling of flow which can hold our attention for hours.

However, as in any other activity, eventually interest wears out. You can't get the same amount of newness indefinetely from your hours of work or study. At this point, you'll need to find new creative ways to maintain a healthy pace on your software skills progression.

### Invest on your workflow ###

I'll start by the most underrated point.

The term **workflow** comprises all the whole pipeline of things you systematically do in order to move something from planned to done.

What is that exactly? Opening your issue tracker system, opening your terminal tabs on the right folders, setting up the tools to get the work done, interacting with your issue tracking system, then the whole *-> writing -> testing -> versioning -> CI -> opening a PR -> review* loop - all those make up a *part* of your workflow.

![Workflow Complexity]({{ "/assets/workflow_complexity.png" | absolute_url }})

Of course, there are much more to that. There are team-level, company-level and even personal-level workflows! Let alone the energy you invest daily just to be a functional human being in an office.

The problem of workflows is, they grow in complexity unexpectedly fast, sometimes they become even more dispendious than the intellectual job you were paid to do!

Yeah, although it is true that tedious workflows can be overcome by willpower, that's probably the worst thing you could ever spend your willpower with. Workflows should never be *in your way*, rather, they should help you *to get things done*.

As you are given ever more complex tasks, you should defend SIMPLICITY in the same proportion. This way you can avoid some motivation-draining monsters to appear.



Start by asking questions like this:

* How much energy do I need to put in order to start a new task?
* How well do I know my own tools (terminal, shell, IDE, version control, ...)?
* How much time I need to wait for tests to complete? And my CI build to finish?
* How can I minimize interruptions?

### Delegate wisely ###

This is related to the previous point.

It sucks we will never be a specialist in everything we need to do.

Deletating up

Sometimes it can be better this way.


### Self imposed breaks ###
### Stay playful ###

Hashtag never stop coding, hashtag never stop learning.












Quem gosta de passar uma noite todo mês revisando gastos e preenchendo planilhas? Bom seria se essas planilhas **se preenchessem sozinhas**, né? 

O que eu gostaria na verdade é que toda a minha vida financeira fosse **automatizada**. Mas vamos lá, um passo de cada vez.

Nesse post, vou mostrar como usar a API do Nubank para ir preenchendo um canal do meu Slack pessoal, que no futuro poderá ter reports diários do que anda acontecendo com o meu dinheiro.


### Passo 1: explorando a API do Nubank

Se você tem o Nubank, pode ver os dados do seu cartão no [https://app.nubank.com.br/](https://app.nubank.com.br/). Logando, você pode ver uma interface com o histórico de transações, que no caso, é o que queremos fazer scraping.

![Nubank WepApp]({{ "/assets/nubank_webapp.png" | absolute_url }})

A API por trás é restful e stateless, com auth per-request e boas práticas de hypermedia, o que vai facilitar bastante nosso trabalho. Quem já tentou fazer scraping sobre uma API stateful sabe o drama que é 😵

Já na tela de login, percebemos que a primeira chamada é para a rota:

{% highlight javascript %}
https://prod-s0-webapp-proxy.nubank.com.br/api/discovery
{% endhighlight %}

Que responde algo como:

{% highlight javascript %}
{
  "login": "...",
  "pusher_auth_channel": "...",
  ...
}
{% endhighlight %}

Cada rota aponta para o endereço de um proxy `https://prod-s0-webapp-proxy.nubank.com.br`, seguido de um URI. No caso de login, por exemplo, o endereço completo fica `https://prod-s0-webapp-proxy.nubank.com.br/api/proxy/AJxL5L2Tx4PB-W6VD1S2xp14EDQ.aHR0cHM6Ly9wcm9kLWdsb2JhbC1hdXRoLm51YmFuay5jb20uYnIvYXBpL3Rva2Vu`.

A primeira parte (antes do `.`) é um prefixo para o proxy vigente, e a segunda, é um endereço codado em base64: `https://prod-global-auth.nubank.com.br/api/token`.

Fazendo uma requisição POST com nosso usuário e senha para essa rota de token, o backend responde com o Token de acesso que vamos usar durante o scraping, e no body uma série de outras rotas - e entre elas, a que nos interessa neste post, `events`.

{% highlight javascript %}
{
  "access_token": "...",
  "_links": {
    "account": {
      "href": "..."
    },
    "events": {
      "href": "..."
    },
    ...
  },
  ...
}
{% endhighlight %}

Programando esse fluxo de API, temos como resultado nosso cliente "quebradiço", que eu subi neste repositório: [https://github.com/rhnasc/nubank_api_exporter](https://github.com/rhnasc/nubank_api_exporter). 

### Passo 2: notificando o Slack sobre últimas transações

Importando o cliente do passo anterior, conseguimos filtrar os eventos das últimas 24 horas com esta função:

{% highlight golang %}
func FilterEventsByTimeRange(events []*nubank.Event, from time.Time, to time.Time) []*nubank.Event {
	filteredEvents := []*nubank.Event{}

	for _, event := range events {
		onRange := event.Time.After(from) && event.Time.Before(to)

		if onRange {
			filteredEvents = append(filteredEvents, event)
		}
	}

	return filteredEvents
}
{% endhighlight %}
[nubank_slack_daily_notifier/filter.go](https://github.com/rhnasc/nubank_slack_daily_notifier/blob/master/filter.go)

Com o resultado desse filtro, notificarei o meu Slack com [esta package](github.com/ashwanthkumar/slack-go-webhook).

A package para o slack nada mais é que um wrapper sobre o protocolo de webhooks do Slack. Preciso então adicionar a integração no meu Slack chamada **Incoming WebHooks**.

![App Directory]({{ "/assets/app_directory.png" | absolute_url }})

O slack nos dará então uma URL de webhooks que poderemos postar num canal. Estarei postando em um chamado `#nubank`.

Ao rodar a aplicação, o resultado será este, mostrando no Slack uma transação que eu fiz nas últimas 24 horas:

![App Directory]({{ "/assets/automate_all_the_things.png" | absolute_url }})

BOA! 🤩

O código desse passo está [neste repo](https://github.com/rhnasc/nubank_slack_daily_notifier).

### Passo 3: rodando tudo em um AWS Lambda

Agora, teremos que subir o nosso código Go em um AWS lambda. Como o código roda pontualmente, essa parece a melhor opção de deployment.

Seu lambda deve se parecer com este aqui:

![Lambda]({{ "/assets/aws_lambda.png" | absolute_url }})

No lado esquerdo, estaremos usando o CloudWatch Events como trigger. O motivo disso é que podemos configurá-lo para ser disparado por uma configuração crontab. A que eu estou usando é `cron(0 0 ? * * *)`.

Para subir o código, podemos seguir [este tutorial](https://docs.aws.amazon.com/lambda/latest/dg/go-programming-model-handler-types.html). Uma vez que o nosso main funciona (passo anterior), precisamos apenas modificá-lo para este formato: 

{% highlight golang %}
package main

import (
  "github.com/aws/aws-lambda-go/lambda"

  ...
)

func HandleRequest() {
  // your old main goes here
}

func main() {
	lambda.Start(HandleRequest)
}

{% endhighlight %}

Diferente de outras linguagens de script, a Amazon não disponibiliza um editor online para você mexer no seu código. Você terá que compilar e subir apenas um executável. Então buildamos o código para um SO linux e zipamos para subir na dashboard do Lambda:

`GOOS=linux go build -o main && zip deployment.zip main`

Usando a dashboard, podemos criar um teste para o lambda que acabamos de configurar. Se tudo der certo, agora o nosso código vai rodar na nuvem, e receberemos novamente o registro da transação pelo canal do Slack :)

![Lambda]({{ "/assets/aws_lambda_success.png" | absolute_url }})

### Great Success

No Slack posso colocar controles interativos para classificar as compras de acordo com minhas categorias.

Também posso importar esses dados em um banco relacional, fazer consultas, cruzar com dados de outros bancos e criar planilhas e projeções que façam sentido para mim.

**Automate all the things :)**
