## Go and Skycoin Starter Guide

### 1. Introduction
This document contains notes information for new developers of skycoin and its products and also go. 

### 2. What is Skycoin

- Skycoin is a currency token hosted on an instance of skyledger.
- The platform as a whole is called skyledger, and is open source.
- Components include wallet, node, exchange, OTC (for direct purchase) etc.
- The infrastructure is replicated for each coin (unlike etherium) which wishes to launch using the platform.
- Each coin has a single master node.  If the master node goes down, transactions are delayed.  The master node is just a normal node with a flag set.
- There is no miner.
- Transaction fees are in coin hours, not the coin itself.
- The wallet and node are combined in one package.
- Github is used for all source and documentation.
- Travis manages all builds (and some deployments)
- The smallest allowed amount at present is 0.001
- The smallest allowed amount ever is 0.000001


### 3. Getting started with Go and Sky for Java Programmers

#### 3.1 Prerequisites

1. You have go installed with GOPATH set (see below)
2. You have ssh setup for your personal github account so you can clone github projects.
3. Coffee.
4. A shot of Whiskey.


#### 3.2 Directory Structures

- Go works best with a single workspace.  
- A workspace is defined as the dir pointed to by $GOPATH.
- The default $GOPATH is ~/go or c:\users\you\go
- The structure under $GOPATH is:
	- bin
	- pkg
	- src
- Dependencies are put in pkg. This means that different projects will end up using the same version of dependencies, and you could end up with the latest at any time.    This will comes a shock to anyone from a java background.  Take the shot of whiskey.  There is hope, go now supports project specific vendor directories.  TBD.
- Compiled programs are put in bin.  If you have two projects which have the same name, one will overwrite the other.  There is no concept of namespace for compiled output.  You can’t have two version of helloworld.exe in bin (but you can still run them locally via “go run helloworld.go”)
- Src dir is not really src, it’s where entire projects go.  E.g.
	- src/project1/main.go
	- src/project1/sompackage
	- src/project2/main.go
- If you want to have multiple workspaces, GOPATH can contain multiple paths.  I believe it will look for dependencies in order on these paths, so you may not get the dependency or exe you were expecting.


#### 3.3 Working with Sky Github projects

You must checkout your projects into $GOPATH/src/github.com/skycoin/xxx
This is because the imports are defined like this:  

```
import (
    "github.com/skycoin/services/scanner/scan"
    "github.com/skycoin/services/scanner/config"
)

```

To checkout a project, flow this recipe:

1. Fork the project to your local github repo using the “fork” button in the top right.  
1. ```cd $GOPATH```
2.  ```mkdir github.com```
3.  ```cd github.com```
4.  ```mkdir skycoin```
5.  ```cd skycoin```
6.  ```git clone https://github.com/YOURREP/services.git```

NOTE, when you checkout the skycoin project itself, you get skycoin/skycoin.

#### 3.4 IDEs

##### 3.5.1 Windows
Visual Studio Code has good go support, and handles multiple projects at the same time. It supports cross-package refactoring, autocompletion and in editor compliation error highlighting and debugging.  It doesn’t support cross package refactoring.

You can install go support for visual studio code from the marketplace: https://marketplace.visualstudio.com/items?itemName=lukehoban.Go

__Tips:__ 

- F12 = go do definition.
- CTR-K V shows markdown preview side by side for .md files.

__Troubleshooting:__

1. Make sure GOPATH is set in your environment variables
2. ```go get -v github.com/nsf/gocode```


##### 3.4.2 Mac

Visual Studio Code works on Mac as well, with few gotchas (e.g. different key combinations).

##### 3.4.3 Linux

Are you crazy?

#### 3.5 GO Commands

See: https://golang.org/cmd/go/ 

| Command | Example | Description |
|---------|---------|-------------|
| get | Go get ```github.com/dotcloud/docker/docker``` | Pull in a dependency to $GOPATH/pkg (will install its dependencies also) |
| run | Go run | Run the go app in your current directory |
| test | Go test | Test the go app in your current directory | 
| build | Go build | Build main and put exe in current directory | 
| install | Go install | Build main and put exe in $GOPATH/pkg | 


#### 3.6 GO Gotchas

1. Like C, Go has no object oriented features such as inheritance or polymorphism. It does support encapsulation and interfaces.
2. func hello() is private (to a package), func Hello() is public.
3. A package is a group of files with the same package name (not sure if they need to be in same dir)
4. Go doesnt have exceptions, but you can return multiple values from a function.
5. Go uses pointers like C.  “&” and “*” but does not support pointer arithmetic. Instead it uses slices, which are something like a substring.
6. Like groovy, go source code doesn't need statement delimiters (;)
7. Go’s built in log class is very very basic, e.g. no levels, cant direct to file based on conf file, can’t disable specific levels on different envs etc.
8. If you so something like this “result = someComplexFunction()”, it won’t compile because it will say result is not used. If it is not used because you haven’t written the code to do something with result yet, you can’t disable this compilation error, but you can do this: “_=result” to get it to run.


#### 3.7 Useful Links

1. [Go in Production - Peter Burgon](https://peter.bourgon.org/go-in-production/)
2. [Go by example](https://gobyexample.com/)


#### 3.8 Skycoin project

[Github Skycoin/skycoin] (https://github.com/skycoin/skycoin)

Can be run locally or in docker.
It contains both a node (i.e. copy of the entire blockchain), various apis and a wallet.


#### 3.9 Go configuration

It must be possible to set any go configuration item by:  

1. Command line
2. Env var
3. Config file.

In that override order.

In addition, it must be possible to set on the command line or env var the location of the configuration file, so that it can be placed outside of the source structure to allow updating of the app.

__See:__. 

https://github.com/spf13/viper
https://github.com/zpatrick/go-config
https://github.com/creamdog/gonfig
https://github.com/namsral/flag


### 4. General rules

1. Never hardcode configuration.  This includes anything environment specific, especially:
	2. Mail gatewal and database usernames/passwords.
	3. Any password.
	4. Ip addresses and urls of services.
	5. Ports.
	6. Tuning parameters.
7. Never use root to run applications.  This applies to Unix root, and to mysql database root.  Its good practice to use limited user accounts also on dev and test.  Running as root is bad and lazy.
8. Log all exceptions (or equivalent, such as null returned where it is not a valid response), unexpected conditions and suspected hack attempt.
9. Log as much information as is available to help root cause analysis.  Not passwords or private keys obviously.  __This includes:__
	10. Way to identify where the problem occured. Minimum is name of function.  For exceptions, it should be a full stack trace.
	11. All and any parameters which may help (except passwords and other secret data of course)
	12. A timestamp.
	13. The severity (critical, error, warning, info etc)
	14. Ideally in a parsable format which all log output uses.


### 5 Status API

Ideally, all services (processes) should have an API or CLI to check its status.  This can be used by:

1. Monitoring and alerting tools
2. Administrators
3. Statistics graphing tools (very useful for root cause analysis across multiple components)

Monitoring tools support basic parsing.  At the simplest level they will look for the word “OK” in the response.  A more sophisticated tool will look for each key value pair, and can alert based on values changing over a threshold, e.g. number of errors > X, or the number of transactions has not gone up in the last X minutes etc

The status check should exercise all relevant components including:

- Database connection (e.g. a simple select), 
- Exchange connections (read a price)
- Blockchain (call an api).

If the service exposes http connection then the status api should be via that. __Example:__ 

www.myservice.com/status

The results should be as easy to parse by a monitoring service as possible.  We recommend key value pairs, but json could also be returned if requested (e.g. via parameter or content type).

#### 5.1 Basic level

Service up/operational:

version=1.23.12b
status=OK

If there is a critical error:

version=1.23.12b
status=ERROR
error=​Could not connect to database: No route to sever


#### 5.2 Additional data

The statistical data can be an absolute value (e.g. total number of users registered), or the number registered since the server was started (in memory).  Chose the latter if the former is expensive and the environment is not going to ever be clustered.  

Example: 

version=1.23.12b.   
status=OK   
uptime=1234  <- seconds   
connections=12.  
errors=3.   
users=123.  
transactions=345.  
maxTransactionTime=43.  
averageTransactionTime=12


### 6 Skycoin concepts

#### 6.1 Input/Output

Like Bitcoin (and unlike Ethereum) all transactions are defined by one or more inputs and one or more outputs.
Inputs are links to a previous transactions output.  The outputs should add up to the total of the inputs.  Any difference is lost (in bitcoin the difference is taken as a fee).  Note, if the balance of an account a is 10, and you want to transfer 1 to account b, then there will b e two outputs, one for 1 to be, and one for 9 (your change, back to yourself).

__See:__ https://bitcoin.org/en/developer-guide#transactions

#### 6.2 Balances

__Confirmed__ - may be pending a transaction so may not be spendable.  
__Spendable__ - can be transferred out -check this one!
__Expected__ - ?


#### 6.3 Wallets

It is defined by a “.wlt” file on the client machine.  It contains the seed, public and private keys and addresses of the wallets accounts.  
A wallet can have multiple addresses, but they are all generated from the same seed (in sequence)


#### 6.4 Address

[__In need of an update__]

#### 6.5 Change accounts

[__In need of an update__]

#### 6.6 UTXOs

[__In need of an update__]

### 7 CLI API Notes

The api will automatically handle change accounts.

#### 7.1 Status

See if the local node/wallet is running 

__Example:__

```
$ cli status

Json response:

{
    "running": true,
    "num_of_blocks": 13908,
    "hash_of_last_block": "94f67d235d7b69cf694691ed0eaa6608a02defa783b12f69ace117d2e2b48f0b",
    "time_since_last_block": "5096s",
    "webrpc_address": "127.0.0.1:6430"
}
```

#### 7.2 CreateRawTransaction

This does not actually do anything with the blockchain, but creates a raw transaction encoding the required transaction to be sent with broadcastTransaction.

NOTE: the documentation is wrong, the units are not droplets, but whole SKY.

__Example:__

```
$ cli createRawTransaction -j -f 2018_02_05_520f.wlt -a 2LKtfjUzWN8T4McPdxxxxNifXkBW7jiCm  27t1ZMAT153q7sxxxxx1Je1DZBAbD4DiYeAw  1
```

- j = json output
- f xxx = local wallet file containing the “from” account. Usually found under $HOME/.skycoin/wallets
- a from account
- To account
- Number of sky

Json response: 

```
{
    "rawtx": "b7000000009675f7c642eb29d20ce417134a3b5db728dc6855ceebf89e14d570b4e0a5e5120100000052a80606f35b8be65c8b06bc3483cc5c6d480f908cc4f838cef2a5f7cfd0e0d57a2ded3c4b27746f07af23720124d3f906bb601135d9cb63413df3980041c02b01010000009ad491564b2312a7cf9f7046713937249f8d8e1bf2c419ab80b192372e8543cd0100000000a13d5c53a94d0421722ec3980992a46d80cc433340420f0000000000222d000000000000"
}

```

#### 7.3 broadcastTransaction

Send a transaction created with createRawTransaction.

Example: 

```
cli broadcastTransaction. 
b7000000009675f7c642eb29d20ce417134a3b5db728dc6855ceebf89e14d570b4e0a5e5120100000052a80606f35b8be65c8b06bc3483cc5c6d480f908cc4f838cef2a5f7cfd0e0d57a2ded3c4b27746f07af23720124d3f906bb601135d9cb63413df3980041c02b01010000009ad491564b2312a7cf9f7046713937249f8d8e1bf2c419ab80b192372e8543cd0100000000a13d5c53a94d0421722ec3980992a46d80cc433340420f0000000000222d000000000000

Response (no json option)

5df24095084bc3d2a202b157b203a31bc3415e059e7ec6842338abbdea29ccff

```

#### 7.4 Transaction

Get data about a submitted transaction. 

Example:

```
cli transaction 5df24095084bc3d2a202b157b203a31bc3415e059e7ec6842338abbdea29ccff

Json response.

{
    "transaction": {
        "status": {
            "confirmed": true,
            "unconfirmed": false,
            "height": 2,
            "block_seq": 13910,
            "unknown": false
        },
        "time": 1517850985,
        "txn": {
            "length": 183,
            "type": 0,
            "txid": "5df24095084bc3d2a202b157b203a31bc3415e059e7ec6842338abbdea29ccff",
            "inner_hash": "9675f7c642eb29d20ce417134a3b5db728dc6855ceebf89e14d570b4e0a5e512",
            "timestamp": 1517850985,
            "sigs": [
                "52a80606f35b8be65c8b06bc3483cc5c6d480f908cc4f838cef2a5f7cfd0e0d57a2ded3c4b27746f07af23720124d3f906bb601135d9cb63413df3980041c02b01"
            ],
            "inputs": [
                "9ad491564b2312a7cf9f7046713937249f8d8e1bf2c419ab80b192372e8543cd"
            ],
            "outputs": [
                {
                    "uxid": "33463ce9fcef2bb489b07c43c4b65f3dec6f214887365311ec48b291ae848880",
                    "dst": "27t1ZMAT153q7sXX2q1Je1DZBAbD4DiYeAw",
                    "coins": "1.000000",
                    "hours": 11554
                }
            ]
        }
    }
}

```

### 8 Git

#### 8.1 Feature branch + 3 way sync for master branch

1. Fork the repo within github
	2. in github.com, click on "fork" in top right. Select your github account as the destination
3. Clone the forked repo to your local machine    into the skycoin repo dir (so imports work)
	4. $ cd github.com/skycoin
	5. $ git clone https://github.com/YOUR-USERNAME/skycoin.net
6. Set the upstream back to the original repo (for later updates)
	7. $ cd skycoin.net            
	8. $ git remote add upstream https://github.com/skycoin/skycoin.net.git
	9. $ git remote -v        
	origin  git@github.com:nutmix/skycoin.net.git (fetch). 
	origin  git@github.com:nutmix/skycoin.net.git (push). 
	upstream        git@github.com:skycoin/skycoin.net.git (fetch). 
	upstream        git@github.com:skycoin/skycoin.net.git (push)
10. Branch locally, make changes
	11. $ git checkout -b my_branch    
	12. $ git add ...     
13. Run tests related to that project
	14. $ yarn lint                # for skycoin.net project
	15. $ yarn test                # for skycoin.net project - doesnt do anything yet    
16. Commit your changes    
	17. $ git commit -m ...            Note: you can include the issue number(s) in the commit message, and a keyworld. E.g. "fixes #39"  This will auto-close the issue when merged to master
18. Push this branch to the fork
	19. $ git push origin my_branch
20. Make a pull request from their fork's branch to the main repo within github.  This will run the tests.
21. If there have been changes to the upstream (main) repos master branch, get them
	22. $ git fetch --all            
	23. $ git checkout master                    
	24. $ git merge upstream/master                
	25. $ git push origin master                    
26. go back to step 4
