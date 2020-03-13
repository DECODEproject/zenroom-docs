<p align="center">
  <a href="https://www.dyne.org">
    <img alt="restroom" src="https://www.oldbookillustrations.com/wp-content/high-res/1845/three-heads-768.jpg" width="150" />
  </a>
</p>

<h1 align="center">
  restroom</br>
  <sub>Easy REST API builder executing Zencode</sub>
</h1>

<p align="center">
  <a href="https://travis-ci.com/dyne/restroom">
    <img src="https://travis-ci.com/dyne/restroom.svg?branch=master" alt="Build Status">
  </a>
  <a href="https://dyne.org">
    <img src="https://img.shields.io/badge/%3C%2F%3E%20with%20%E2%9D%A4%20by-Dyne.org-blue.svg" alt="Dyne.org">
  </a>
</p>


<h4 align="center">
  <a href="#-install">💾 Install</a>
  <span> • </span>
  <a href="#-quick-start">🎮 Quick start</a>
  <span> • </span>
  <a href="#-configuration">🔧 Configuration</a>
  <span> • </span>
  <a href="#-acknowledgements">😍 Acknowledgements</a>
  <span> • </span>
  <a href="#-links">🌐 Links</a>
  <span> • </span>
  <a href="#-contributing">👤 Contributing</a>
  <span> • </span>
  <a href="#-license">💼 License</a>
</h4>

<br><br>

🚧 Zenroom is a software part of the [DECODE project](https://decodeproject.eu) about data-ownership and [technological sovereignty](https://www.youtube.com/watch?v=RvBRbwBm_nQ). Our effort is that of improving people's awareness of how their data is processed by algorithms, as well facilitate the work of developers to create along [privacy by design principles](https://decodeproject.eu/publications/privacy-design-strategies-decode-architecture) using algorithms that can be deployed in any situation without any change.


<details id="toc">
 <summary><strong>🚩 Table of Contents</strong> (click to expand)</summary>

* [Install](#-install)
* [Quick start](#-quick-start)
* [Configuration](#-configuration)
* [Acknowledgements](#-acknowledgements)
* [Links](#-links)
* [Contributing](#-contributing)
* [License](#-license)
</details>

***
## 💾 Install

```bash 
git clone https://github.com/dyne/restroom
cd restroom
yarn install
``` 

**[back to 🔝](#toc)**
***
## 🎮 Quick start

### 🖧 SERVER

To start using restroom you need to run the restroom server.
Some configuration are needed first, look [here](#-configuration) to
define your ENVIRONMENT VARIABLES before running the server.

Then run

```bash
yarn start
```

now all your zencode smart contracts are available as GET and POST
with the name of the file without the `.zen` extension
(eg. `random.zen` will be served as `/api/random` endpoint)


### 📖 OPENAPI

When the server is run, the configured folder is loaded and parsed and a
openapi (ex swagger) definition is serverd on the index page.

Each time you hit the `/` index page, the openapi definitions are updated,
this means that when you add new contracts into the `SMART_CONTRACTS` folder they  automatically appear into the swagger definition and are testable via the GUI.


### 💻 TEST CLIENT

The client is not necessary since you can use the tool you want to make
http GET/POST calls but is a handy tool to test the endpoints.

By running `./restroom-cli --help` you'll get the extensive documentation.

#### SIMPLE EXAMPLE
`./restroom-cli get random`


#### COMPLEX EXAMPLE
`./restroom-cli get somezencode -c ./conf -d ./data.json -k ./keys.json`


**[back to 🔝](#toc)**

***
## 🔧 Configuration

Restroom follows the [Twelve factor app principles](https://12factor.net/) hence all the configuration are via *Environment variables*.

### `SMART_CONTRACTS`
This is the entry point directory to place your zencodes to run.
NB. As per now all the contracts must end with the `.zen` suffix and the result of the contract should be a valid `JSON`

### `PORT`
The port to attach the server. **default: 3000**

### `HOST`
The host to bind the server **default: 0.0.0.0**

### Example `.env`
```
SMART_CONTRACTS=/home/src/restroom/zencode
PORT=8000
HOST=127.0.0.1
```

**[back to 🔝](#toc)**

***
## 😍 Acknowledgements

Copyright © 2019 by [Dyne.org](https://www.dyne.org) foundation, Amsterdam

Designed, written and maintained by Puria Nafisi Azizi.

Special thanks to Jaromil for his special contributions and for the idea.

Image in README courtesy of [Old book illustrations](https://www.oldbookillustrations.com/illustrations/three-heads/)


**[back to 🔝](#toc)**
***
## 🌐 Links

[https://zenroom.org/](https://zenroom.org/)

[https://dev.zenroom.org/](https://dev.zenroom.org/)

[https://dyne.org/](https://dyne.org/)


**[back to 🔝](#toc)**
***
## 👤 Contributing

Please first take a look at the [Dyne.org - Contributor License Agreement](CONTRIBUTING.md) then

1.  🔀 [FORK IT](../../fork)
2.  Create your feature branch `git checkout -b feature/branch`
3.  Commit your changes `git commit -am 'Add some fooBar'`
4.  Push to the branch `git push origin feature/branch`
5.  Create a new Pull Request
6.  🙏 Thank you


**[back to 🔝](#toc)**
***
## 💼 License
    restroom - Easy REST API builder executing Zencode
    Copyright (c) 2020 Dyne.org foundation, Amsterdam

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU Affero General Public License as
    published by the Free Software Foundation, either version 3 of the
    License, or (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU Affero General Public License for more details.

    You should have received a copy of the GNU Affero General Public License
    along with this program.  If not, see <http://www.gnu.org/licenses/>.

**[back to 🔝](#toc)**
