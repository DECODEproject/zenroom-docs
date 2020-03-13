# Make ❤️ with Zenroom and Javascript


![Make ❤️ with Zenroom and Javascript](https://www.dyne.org/wp-content/uploads/2019/11/cropped-zenroom_only-logo-1000x675.png ':size=100x68')

### How to use Zenroom within the JS ecosystem

This article is part of a series of blog posts about interacting with Zenroom inside the Javascript messy world. This is the first entry and at the end of the article you should be able to run a fancy hello world within nodejs.

- [Part one Zenroom in node.js (this post)](https://www.dyne.org/using-zenroom-with-javascript-nodejs-part1/)
- [Part two Zenroom in the browser](https://www.dyne.org/make-with-zenroom-and-javascript-part-2/)
- [Part three Zenroom in React](https://www.dyne.org/make-with-zenroom-and-javascript-part-3/)
- Part four Zenroom in commonjs and ES6
### Zenroom

If you landed on this page accidentally maybe you heard of Javascript but not about Zenroom. This later as says on the official [website](https://zenroom.org) is


> a tiny and portable virtual machine that integrates in any application to authenticate and restrict access to data and execute human-readable smart contracts.
> 
> <cite><a href="https://www.zenroom.org">https://www.zenroom.org</a> The zenroom guru</cite>


Zenroom is a dyne.org project since 2017 and is part of the&nbsp;[DECODE project](https://decodeproject.eu/)&nbsp;about data-ownership and&nbsp;[technological sovereignty](https://www.youtube.com/watch?v=RvBRbwBm_nQ).


If you are landed here on purpose, as the time of writing this, we just released the 1.0.0 version of Zenroom 🎉 Hopefully first of many to come [TRL9](https://en.wikipedia.org/wiki/Technology_readiness_level) releases.

### 📑 Some RTFM and resources

So first things first, let’s start by where to look for good information about Zenroom (docs that are continuously under enhancement and update).

- [https://dev.zenroom.org](https://dev.zenroom.org) this is the main source of technical documentation
- [https://zenroom.org](https://zenroom.org) here you find more informative documentation and all the products related to the main project
- [https://dev.zenroom.org/demo](https://dev.zenroom.org/demo) a very useful playground to try online your scripts
- 
### 🌐 How a VM could live in a browser?

So basically Zenroom is a virtual machine that is mostly written in C and high-level languages and has no access to I/O and no access to networking, this is the reason that makes it so portable.


In the past years we got a huge effort from nice projects to transpile native code to Javascript, we are talking about projects like [emscripten](https://emscripten.org/), [WebAssembly](https://webassembly.org/) and [asm.js](http://asmjs.org/)


This is exactly what we used to create a WASM (WebAssembly) build by using the Emscripten toolkit, that behaves in a good manner with the JS world.

### 💻 Let’s get our hands dirty

So let’s start by our first hello world example in node.js I’m familiar with yarn so I’ll use that but if you prefer you can use `npm `


`$ mkdir zenroom-nodejs-test$ cd zenroom-nodejs-test $ yarn init $ yarn add zenroom`


The previous commands create a folder and a js project and will add zenroom javascript wrapper as a dependency. The wrapper is a very simple utility around the pure emscripten build.


Next create a `index.js` with the following content

```javascript
const zenroom = require('zenroom').default
zenroom.script('print("Hello World!")').zenroom.exec()
```

So the first line will import the `zenroom` module the second one executes a very simple Hello World! zenroom script.


**NB. The documentation of the JS wrapper API is available** [**here**](https://github.com/DECODEproject/Zenroom/tree/master/bindings/javascript) **.**


Now we need to run the file we just created by simply run


`$ node index.js`


🎉🎉🎉


You’ll find the asciimation here: [https://asciinema.org/a/274518](https://asciinema.org/a/274518)

### Let’s complicate it a bit! Let’s run Zencode!

Now that we saw that everything is let’s procede with some sofistication, let’s run a Natural Language Smart Contract called Zencode.


So create a new file called `keygen.js` and put the following code:


```javascript
const zenroom = require('zenroom')

const keygen_contract = `rule check version 1.0.0
Scenario 'simple': Create the keypair`
Given that I am known as 'Puria'
When I create the keypair
Then print my data`

zenroom.script(keygen_contract).zencode_exec()
```



Et voila the result is something like (I prettified for the purpose of readability)


```json
{
  "zenroom": {
    "curve": "goldilocks",
    "encoding": "url64",
    "version": "1.0.0+a7fab75",
    "scenario": "simple"
  },
  "Puria": {
    "keypair": {
      "public_key": "u64:BBqhjaIXr6vPMVhQKSU1vau5lUJDXwGBul0OwZYarNnUhbG2W6bMY-uo2dH-W4ymjx-vU_3agTQm2N1F25xq8o74DutvNW3ZX8GHROa5zIi7TIDoXy-_5sSyKBeVnGZ9IrFkoo9R2cbtREjOE6hgZ-Q",
      "private_key": "u64:IT-cZZQf1-yzXF6GSrvQGScRHGbZeh8_LGFMIGCKrxKZtbk3RJbWXLlBlOfJ3oAWgaaYa5mc9iM"
    }
  }
}
```


**NB. The documentation of zencode is available** [**here**](https://dev.zenroom.org/zencode/)


In the next article we will see how to run the same examples within a browser.



