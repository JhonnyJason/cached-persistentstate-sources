# cached-peristentstate

# Background
One very convenient way to persistently store the internal state of a node service is to just write the state object as JSON.
For usecases where Databases donot make sense this is the way to go.

This is a small convenience package for this usecase, especially convenient to have some caveats solved.

- When the service is being killed on writing the state, the file might become corrupted -> Creating a backup
- Too many states, too much memory usage -> Caching of files and limiting the number of simultaneously held states or state-chunks.

*Note: This package is not made for maximum performance. Especially not to serve 10k+ clients having ther states to be loaded and saved in a few seconds. This is made for when 10k+ clients might be expected over a lifetime with states for each client larger than 100kb thus holding everything in-memory would be infeasible, but we still don't want to read from disk every time when a certain client is active.*

# Usage
Requirements
------------
- nodejs >10

Installation
------------
Current git version:

```
$ npm install git+https://github.com/JhonnyJason/cached-persistentstate-output.git
```

Npm Registry
```
$ npm install cached-persistentstate
```

Current Functionality
---------------------
The whole API works synchronously.

```coffee
import * as state from "cached-persistentstate"

################################################################################
# initialize with Options
defaultState = {
    serviceState: {
        key: null
        name: "myservice"
        knownClients: []
    }
}
basePath = "./my-state"
maxCacheSize = 128

options = {defaultState, basePath, maxCacheSize}

state.initialize(options)


################################################################################
# load State
stt_service = state.load("serviceState")
stt_service.key = createKey()


################################################################################
# save same state
state.save("serviceState")


################################################################################
# save new Object
clientState = {
    key: createKey()
    clientData: {}
}
state.save("client1", clientState)


################################################################################
# remove Object and stored files
state.remove("client1")

################################################################################
# inspect cache state
state.logCacheState()

```

## Options
The package should be initialized before using.
However the initialization could be skipped, when the defaults are sufficient and the directory `./state` exists.
If you donot pass in any options, the default options are taken.

DefaultOptions:
```coffee
{
    defaultState: null
    basePath: "./state"
    maxCacheSize: 64
}
```
Default Initialization
```coffee
state.initialize() # will create the ./state directory
```

Without default initialization `./state` will not be created.

*Note: it will error out when you provide a basePath where more than 1 directory needs to be created.*


## load(id)
- Will return the current Object in cache if it is available.
- Will try to read the Object from disk if it is not available in the cache.
- If reading the Object from disk also fails it will try to find it in the defaultState.
- If nothing works it will return an new empty Object `{}`

## save(id, obj)
- If we have an obj - it will write the new obj into the object cache.
- Will check if any changes have occured to the cached json string from the file.
- Will save the new json string to the file and copy it as a backup.

*Note: this also works without passing any obj to simply save the same object again if it changed in the meantime.*
*Note: if you change but donot save the obj and it is cut from the cache then - the changes will be lost.*

## remove(id)
Aaand it's gone, it's all gone!

*Note: It is your responsibility to forget and delete all refernces to the state object you have. The pacakge could only delete the ones it holds by itself.*


---

# Further steps

- ...


All sorts of inputs are welcome, thanks!

---

# License

## The Unlicense JhonnyJason style

- Information has no ownership.
- Information only has memory to reside in and relations to be meaningful.
- Information cannot be stolen. Only shared or destroyed.

And you wish it has been shared before it is destroyed.

The one claiming copyright or intellectual property either is really evil or probably has some insecurity issues which makes him blind to the fact that he also just connected information which was freely available to him.

The value is not in him who "created" the information the value is what is being done with the information.
So the restriction and friction of the informations' usage is exclusively reducing value overall.

The only preceived "value" gained due to restriction is actually very similar to the concept of blackmail (power gradient, control and dependency).

The real problems to solve are all in the "reward/credit" system and not the information distribution. Too much value is wasted because of not solving the right problem.

I can only contribute in that way - none of the information is "mine" everything I "learned" I actually also copied.
I only connect things to have something I feel is missing and share what I consider useful. So please use it without any second thought and please also share whatever could be useful for others. 

I also could give credits to all my sources - instead I use the freedom and moment of creativity which lives therein to declare my opinion on the situation. 

*Unity through Intelligence.*

We cannot subordinate us to the suboptimal dynamic we are spawned in, just because power is actually driving all things around us.
In the end a distributed network of intelligence where all information is transparently shared in the way that everyone has direct access to what he needs right now is more powerful than any brute power lever.

The same for our programs as for us.

It also is peaceful, helpful, friendly - decent. How it should be, because it's the most optimal solution for us human beings to learn, to connect to develop and evolve - not being excluded, let hanging and destroy oneself or others.

If we really manage to build an real AI which is far superior to us it will unify with this network of intelligence.
We never have to fear superior intelligence, because it's just the better engine connecting information to be most understandable/usable for the other part of the intelligence network.

The only thing to fear is a disconnected unit without a sufficient network of intelligence on its own, filled with fear, hate or hunger while being very powerful. That unit needs to learn and connect to develop and evolve then.

We can always just give information and hints :-) The unit needs to learn by and connect itself.

Have a nice day! :D