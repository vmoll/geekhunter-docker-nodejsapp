# geekhunter-docker-nodejsapp

Exemplo de uma aplicação criada para ser distribuída utilizando Docker.

# Develop, Ship e Run

Veja um exemplo de conteúdo de um Dockerfile:

```FROM node:10-alpine
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
WORKDIR /home/node/app
# Install app dependencies
COPY package*.json ./
USER node
RUN npm install
COPY --chown=node:node . .
EXPOSE 8080
CMD [ "node", "app.js" ]
```

Se olharmos para o arquivo app.js que define uma aplicação básica em Node.js e express teremos:

```
const express = require('express');
const app = express();
app.get('/', function (req, res) {
res.send('Geek Hunter!');
});
app.listen(3000, function () {
	console.log('Servidor Geek Hunter rodando na porta 3000!');
}); 
```

Agora que já definimos nossa aplicação em app.js e já temos nosso Dockerfile, vamos criar uma imagem do Docker para poder rodar nossa aplicação e realizar o deploy na imagem que será carregada no container.


## Vamos criar uma imagem executando o comando:

```
$ docker build -t vmoll/nodejs-image-demo .
```

A opção -t serve para informarmos uma tag para a imagem que estamos criando.
Após a criação será possível rodar a aplicação, utilizando o seguinte comando:

```
$ docker run -it -p 3000:3000 -h instance-hostname --rm --name nodejs-image-demo vmoll/nodejs-image-demo:latest
```


No comando que acabamos de executar informamos -p 3000:3000, responsável por realizar o bind ou vinculação da porta local para uma porta externa do serviço que será disponibilizado na máquina que está hospedando a aplicação que acabamos de criar e realizar o deploy.

Veja como a aplicação está funcional e operante, acessando http://localhost:3000, e receberá a mensagem “Hello Geeks!”:

***Observação importante:*** criei aqui uma aplicação bem simples, apenas para demonstrar que é possível realizar o deploy na imagem Docker que criamos e nosso foco principal é a criação e execução de containers usando Docker. Caso queria criar uma aplicação completa, pode utilizar algum gerador como o express-generator. Para isso execute o seguintes comandos:

```
$ npm install express-generator -g
```

```
$ express minhaAplicacao
```

```
$ cd minhaAplicacao
```

```
$ npm install
```

```
$ npm start
```

Após o que abra uma janela do browser em http://localhost:3000

