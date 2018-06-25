---
layout: post
title:  "Exportando minhas transações do Nubank"
date:   2018-06-25 02:14:02 -0300
categories: automation golang lamba portuguese
---

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

Cada rota aponta para um endereço de um proxy `https://prod-s0-webapp-proxy.nubank.com.br`, seguido de uma URL. No caso de login, por exemplo, `/api/proxy/AJxL5L2Tx4PB-W6VD1S2xp14EDQ.aHR0cHM6Ly9wcm9kLWdsb2JhbC1hdXRoLm51YmFuay5jb20uYnIvYXBpL3Rva2Vu`.

A primeira parte (antes do `.`) é um prefixo para o proxy vigente, e a segunda, é um endereço codado em base64: `https://prod-global-auth.nubank.com.br/api/token`.

Fazendo uma requisição POST com nosso usuário e senha para essa rota de token, o backend responde com o Token de acesso que vamos durante o scraping, e no body uma série de outras rotas - e entre elas, a que nos interessa neste post, `events`.

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

Programando esse fluxo de API temos como resultado nosso cliente "quebradiço", que eu subi neste repositório: [https://github.com/rhnasc/nubank_api_exporter](https://github.com/rhnasc/nubank_api_exporter). 

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

Diferente de outras linguagens de script, a Amazon não vai um editor online para você mexer no seu código. Você terá que compilar e subir apenas um executável. Então buildamos o código para um SO linux e zipamos para subir na dashboard do Lambda:

`GOOS=linux go build -o main && zip deployment.zip main`

Usando a dashboard, podemos criar um teste para o lambda que acabamos de configurar. Se tudo der certo, agora o nosso código vai ser rodado na nuvem, e receberemos novamente o registro da transação pelo canal do Slack :)

![Lambda]({{ "/assets/aws_lambda_success.png" | absolute_url }})

### Great Success

No Slack posso colocar controles interativos para classificar as compras de acordo com minhas categorias.

Também posso importar esses dados em um banco relacional, fazer consultas, cruzar com dados de outros bancos e criar planilhas e projeções que façam sentido para mim.

**Automate all the things :)**
