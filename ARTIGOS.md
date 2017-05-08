<p>Se você chegou aqui de paraquedas e não sabe o que é o Telegram, de onde ele vem e do que ele se alimenta, eu já expliquei tudo isso em outro artigo. Lá além de falar um pouco sobre o Telegram, eu explico como criar o seu Bot através do @BotFather, o pai dos Bots do Telegram, é nele que você vai conseguir o seu Token de acesso e definir o nome e username do seu Bot, aqui nós vamos partir direto pro código.
Nesse artigo eu também vou comparar algumas diferenças entre utilizar Javascript e Ruby para desenvolver o seu Bot então recomendo que leia o artigo I antes de prosseguir. O código mostrado nesse artigo é são trechos do código do XplainShell Bot. Ele foi desenvolvido por mim e nele você pode enviar mensagens com algum comando Shell e o Bot vai retornar uma explicação do que aquele comando faz. Vocês vão ver como ele funciona.
Vamos ao código! Lembrando que o projeto é Open-Source então se você gostar da ideia, saber um pouco de JavaScript e quiser contribuir é só dar uma olhada na aba Issues no repositório.</p>

<p>Nesse artigo eu também vou comparar algumas diferenças entre utilizar Javascript e Ruby para desenvolver o seu Bot então recomendo que leia o artigo I antes de prosseguir. O código mostrado nesse artigo é são trechos do código do XplainShell Bot. Ele foi desenvolvido por mim e nele você pode enviar mensagens com algum comando Shell e o Bot vai retornar uma explicação do que aquele comando faz. Vocês vão ver como ele funciona.</p>

<p>Vamos ao código! Lembrando que o projeto é Open-Source então se você gostar da ideia, saber um pouco de JavaScript e quiser contribuir é só dar uma olhada na aba Issues no repositório.</p>

```
const telegramBot = require('node-telegram-bot-api'),
    dotenv = require('dotenv').config(),
    request = require('superagent'),
    cheerio = require('cheerio'),
    token = process.env.TELEGRAM_API,
    bot = new telegramBot(token, {
        polling: true
    });
bot.on('message', (msg) => {
    let userID = msg.chat.id,
        messageUser = msg.text,
        url = 'https://explainshell.com/explain?cmd=' + messageUser;
    request.get(url, (err, res) => {
        if (err) throw err;
        let $ = cheerio.load(res.text),
            answer = $('.help-box').text(),
            errorMessage = "Sorry, this command is invalid or unknown. Try another command.";
        bot.sendMessage(userID, answer).catch((error) => {
            bot.sendMessage(userID, errorMessage);
        });
    });
});
```

<p>No começo eu importei 4 bibliotecas, foram essas libs que me auxiliariam a desenvolver o Bot funcional com apenas 24 linhas. A principal e indispensável é a node-telegram-bot-api, através do Token fornecido pelo Bot Father ela se comunica com a API oficial do Telegram e permite que a gente desenvolva o Bot utilizando Javascript.</p>

<p>A segunda foi a dotenv que permite que eu separe as minhas variáveis de ambiente em um arquivo .env e não leve essa informação para produção e também possa ignorá-la através de um .gitignore e não colocá-la no github. Caso você, assim como eu, hospede o seu Bot no Heroku você pode setar suas variáveis de ambiente através da dashboard da plataforma.</p>

<p>A terceira lib foi a superagent e eu escolhi ela ao invés da biblioteca request porque achei ela mais simples e a documentação dela também era mais direta ao ponto. Como se trata de uma simples chamada HTTP ambas as bibliotecas fazem perfeitamente o serviço, escolha a sua preferida.</p>

<p>A quarta e última lib, provavelmente a cereja do bolo desse Bot é o cheerio, que é uma implementação do core do Jquery, por isso a mesma sintaxe já tão conhecida. O que ele faz? Ele permite que você parseie e manipule o DOM de uma forma simples e rápida. Perfeita pro que foi feito no Bot.</p>

<p>Depois que passamos o Token para o Bot ele escuta se alguma mensagem é recebida e assim que uma mensagem é enviada pelo usuário, ele começa a trabalhar. Pegamos o ID do usuário, afinal precisamos saber a quem devemos mandar a resposta, não é? E também a mensagem que ele enviou e mandamos a mensagem na URL para fazermos a requisição nas linhas abaixo. Há outros exemplos desse tipo de implementação possíveis na doc. do Telegram API.</p>

<p>Beleza, faço a requisição pra URL que eu montei e depois de receber a response navego pelo DOM utilizando o Cheerio até chegar na resposta fornecida pelo site do XplainShell. Em caso de sucesso na resposta, eu encaminho a resposta que foi capturada do site para o usuário e caso não exista uma resposta para a mensagem enviada pelo usuário, encaminhamos uma mensagem de erro que convida o usuário a tentar novamente.</p>

<p>Esse é o meu Bot e ele está hospedado no Heroku que é uma plataforma de serviço em nuvem com suporte para diversas plataformas: Python, Ruby, PHP, Javascript. Por que o Heroku? Porque ele além de possuir um plano free tem um deploy muito fácil e rápido que fica ainda mais fácil quando você está utilizando Javascript. Você só precisa criar um arquivo chamado Procfile na pasta do seu projeto e fazer o deploy.</p>

> web: node xplainshell.js

<p>Esse é o meu Procfile, eu só estou dizendo para ele rodar o arquivo que você acabou de ver lá em cima e o Heroku cuida de todo o resto, além de todo o processo ser feito pelo terminal e ter uma sintaxe muito parecido com o uso do git, o que deixa você mais confortável em fazer o deploy do seu bot.</p>

<p>Você só precisa ter o Heroku instalado, assim como você precisa do node para rodar e testar o Bot localmente. Depois de fazer login na sua conta do Heroku pelo terminal é só você dar push da sua aplicação para o Heroku, você pode ver em detalhes esse processo na documentação.</p>

<p>Simples e muito útil! Quando eu migrei pro Linux e via a galera mandando rodar vários comandos no terminal sempre ficava com aquele pé atrás, acho que acontece com todo mundo. Hoje eu já estou usando uma distro mais hipster do Linux com facilidade mas a dois anos atrás esse Bot teria sido uma mão na roda pra mim. Se você conhece alguém que está nesse mesmo cenário, manda o Bot XplainShell para ele!</p>

<p>Se você chegou até aqui não esqueça de deixar o seu ❤ e/ou um comentário. I ❤ seu Feedback. Se você tem interesse por Bots ou gostou do meu texto você pode ler algum dos meus outros artigos, recomendo:</p>

- 1. Telegram Bot — Como desenvolver seu Bot utilizando Ruby e Heroku(https://medium.com/tableless/telegram-bot-como-fazer-um-bot-com-ruby-e-heroku-a2bc384ab18e)
- 2. Docker — Dockerhub, pull e push nas suas imagens(https://medium.com/tableless/docker-dockerhub-pull-e-push-nas-suas-imagens-57dffa0232ad)
- 3. TDD — Test Driven Development(https://medium.com/tableless/tdd-test-driven-development-71ad9a69d465)
- 4. PHP — Criando um CRUD com Laravel 5.4(https://medium.com/@victorhugorocha/php-criando-um-crud-b%C3%A1sico-com-laravel-5-4-f17afe11c66e)

**FONTE:** https://medium.com/@victorhugorocha/telegram-bot-como-fazer-um-bot-com-javascript-e-heroku-7e1d8b20e4a
