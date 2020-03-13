# How to execute Zenroom scripts

This section explains how to invoke the Zenroom to execute scripts in various situations: from commandline, from javascript and various other languages.

Javascript has its own way to call `zenroom_exec(...)` without losing all the context for execution.

Other languages are recommended to `exec()` the Zenroom in a separate process and gather its `stdout` and its return value (1 on error and 0 on success). 

## Commandline

From **command-line** the Zenroom is operated passing files as
arguments:
```
Usage: zenroom [-dh] [-c config] [-k KEYS] [-a DATA] [ SCRIPT.lua | - ]
```
The `-d` flag activates more verbose output for debugging.

The `SCRIPT.lua` can be the path to a script or a single dash (`-`) to instruct zenroom to process the script piped from `stdin`.

### Interactive console

Just executing `zenroom` will open an interactive console with limited functionalities, which is capable to parse finite instruction blocks on each line. To facilitate editing of lines is possible to prefix it with readline using the `rlwrap zenroom` command instead.

The content of the KEYS, DATA and SCRIPT files cannot exceed 500KB.

Try:
```sh
./zenroom-static      [enter]
print("Hello World!") [enter]
                      [Ctrl-D] to quit
```
Will print Zenroom's execution details on `stderr` and the "Hello World!" string on `stdout`.

### Hashbang executable script

Zenroom also supports being used as an "hashbang" interpreter at the beginning of executable scripts. Just use:
```sh
#!/usr/bin/env zenroom-static -
```
in the header, please note the final dash which is pretty important to tell zenroom to process the script from `stdin`.



## Javascript
### Update Dic 2018
For **javascript** :tada: now a npm module is available on https://www.npmjs.com/package/zenroom

***

From **javascript** the function `zenroom_exec()` is exposed with four arguments: four strings and one number from 1 to 3 indicating the verbosity of output on the console.

This is the prototype of the javascript function:
```js
const zenroom = (script_file, conf_file=null, keys_file=null,
	             data_file=null, verbosity=3)
```
Only the `script_file` argument is mandatory, other arguments can be omitted, then defaults are used. For example after a `make js` build we can invoke the zenroom in `src/zenroom.js` creating such a script:

```js
const fs = require('fs')
const zenroom_module = require(process.argv[2])

const zenroom = (script_file, conf_file=null, keys_file=null, 
	             data_file=null, verbosity=3) => {
  const enc = { encoding: 'utf8' }
  const script = fs.readFileSync(script_file, enc)
  const conf = (conf_file) ? fs.readFileSync(conf_file, env) : null
  const keys = (keys_file) ? fs.readFileSync(keys_file, env) : null
  const data = (data_file) ? fs.readFileSync(data_file, env) : null
  return zenroom_module.ccall('zenroom_exec', 'number',
                              ['string', 'string', 'string', 'string', 'number'],
                              [script, conf, keys, data, verbosity])
}
console.log("[JS] zenroom_exec %s %s", process.argv[2], process.argv[3])
zenroom(process.argv[3])
```
If the above script is saved in `zenroom_exec.js` then on the commandline it can be invoked with the path to `zenroom.js` as first parameter and the path to a script as second parameter:
```sh
nodejs src/zenroom.js zenroom_exec.js examples/hello.lua
```

The contents of the three strings cannot exceed 500KB in size.

### Data string

The `data` is a simple string, but can be also a json map used to pass multiple arguments and complex data structures. Its contents can be accessed by the `script` using the global variable `DATA` (all uppercase).

For example create a json file containing a map (this can be a string
passed from javascript)

```json
{
	"secret": "zen and the art of programming",
	"salt": "OU9Qxl3xfClMeiCz"
}
```

Then run `zenroon -a arguments.json` and pass the following script as
final argument, or pipe from stdin or passed as a string argument to
`zenroom_exec()` from javascript:

```lua
i = require "inspect"
json = require "json"
args = json.decode(DATA)
-- args is now a lua table containing values for each args.argname
i.print(args)
```

### Keys string

The `keys` is also a simple string passed from arguments, separated from DATA since its access permission from caller may be different due to specific privacy restrinctions that may be adopted on the variable. It is accessed by the `script` using the global variable `KEYS`.

So for instance if we want to encrypt a secret message for multiple
recipients who have provided us with their public keys, one would load
this example keyfile:

```json
{
    "keyring": {
        "public":"GoTdVYbTWEoZ4KtCeMghV7UcpJyhvny1QjVPf8K4oi1i",
        "secret":"9PSbkNgsbgPnX3hM19MHVMpp2mzvmHcXCcz6iV8r7RyZ"
    },
    "recipients": {
        "jaromil": "A1g6CwFobiMEq6uj4kPxfouLw1Vxk4utZ2W5z17dnNkv",
        "francesca": "CQ9DE4E5Ag2e71dUW2STYbwseLLnmY1F9pR85gLWkEC6",
        "jimb": "FNUdjaPchQsxSjzSbPsMNVPA2v1XUhNPazStSRmVcTwu",
        "mark": "9zxrLG7kwheF3FMa852r3bZ4NEmowYhxdTj3kVfoipPV",
        "paulus": "2LbUdFSu9mkvtVE6GuvtJxiFnUWBWdYjK2Snq4VhnzpB",
        "mayo": "5LrSGTwmnBFvm3MekxSxE9KWVENdSPtcmx3RZbktRiXc"
    }
}
```

And then with this code encrypt a message to all recipients:

```lua
secret="this is a secret that noone knows"
-- this should be a random string every time
nonce="eishai7Queequot7pooc3eiC7Ohthoh1"

json = require "json"
crypto = require "crypto"

keys = json.decode(KEYS)

res = {}

for name,pubkey in pairs(keys.recipients) do
   k = crypto.exchange_session_x25519(
	  crypto.decode_b58(keys.keyring.secret),
	  crypto.decode_b58(pubkey))
   enc = crypto.encrypt_norx(k,nonce,secret)
   -- insert in results
   res[name]=crypto.encode_b58(enc)
end
print(json.encode(res))
```

The above script  will print out the encrypted message for each recipient reorganised in a similar json structure:

```json
{
   "jaromil" : "Ha8185xZoiMiJhfquKRHvtT6vZPWifGaXmD4gxjyfHV9ASNaJF2Xq85NCmeyy4hWLGns4MTbPsRmZ2H7uJh9vEuWt",
   "mark" : "13nhCBWKbPAYyhJXD7aeHtiFKb89fycBnoKy2nosJdSqfS2vhhHqBvVKb2oasiga9P3UyaEJZQdyYRfiBBKEswdmQ",
   "francesca" : "7ro9u2ViXjp3AaLzvve4E4ebZNoBPLtxAja8wd8YNn51TD9LjMXNGsRvm85UQ2vmhdTeJuvcmBvz5WuFkdgh3kQxH",
   "mayo" : "FAjQSYXZdZz3KRuw1MX4aLSjky6kbpRdXdAzhx1YeQxu3JiGDD7GUFK2rhbUfD3i5cEc3tU1RBpoK5NCoWbf2reZc",
   "jimb" : "7gb5SLYieoFsP4jYfaPM46Vm4XUP2jbCUnkFRQfwNrnJfqaew5VpwqjbNbLJrqGsgJJ995bP2867nYLcn96wuMDMw",
   "paulus" : "8SGBpRjZ21pgYZhXmy7uWGNEEN7wnHkrWtHEKeh7uCJgsDKtoGZHPk29itCV6oRxPbbiWEuN9Sm83jeZ1vinwQyXM"
}
```
