# milang
shell redone for the new era


## Major differences
- variable scoping by default  
  - can be disabled  
- object oriented style  
- option control  
- variable & loop typing  

## Direct Coding
milang is just shell with abstractions  
actual code is simply written as if it was POSIX shell  
it is then compiled for the shell set by the `SHELL` keyword

## Objects
- shell  
  > the `SHELL` keyword should be used to select a shell  
  > however a shell is selected from `/etc/shells` automatically  
  > if it supports all the requested features
- func  
  - keywords
    - new  
      > used to create new functions
      > ```
      > func.new ex() {
      >  #...
      >}
      > ```
    - init
      > used to init variables
      > ```
      > func.init() {
      >   init.float: "float"
      >   # creates new float variable as $float
      >   # the init keyword should only be used inside init()
      > }
      > ```
    - import
      > used to import other functions  
      > `.mla` files inside `./functions` automatically imported
      > ```
      > func.import() {
      >   import.library: "curl"
      >   # import the curl milang library
      >   import.PATH: "./extras"
      >   # import all .mla files in ./extras
      > }
      > ```
    - main
      > the main function in a `.mla` file
      > ```
      > func.main() {
      >   ex()
      > }
      > ```
- [options](#Options)
- features
  > `options.FEATURES`  
  is used for features with ambiguous support
- negatives  
  > if a keyword fails a negative should be used to handle it  
  > otherwise compilation/execution will fail  
  > negatives look like this
  >```
  > options.FEATURES.float
  > # the above fails
  > !^ options.FEATURES.float.emu 
  > # code to run if above line returns 1
  >```


## Types
- std/string/var  
  > the standard variable type
- char  
  > supports only a single character
  > - used for character loops
- line  
  > same as std but read from file  
  > supports selecting with `[<>]`  
  > line 200 is `line[200]`
- int  
  > supports only a number  
  > maximum size depends on active shell
- float  
  > must be enabled  
  > see [Options](#Options)


## Options
milang uses option control for various features  
options are typically boolean; their states are automatically swapped  
by the `options.NAME` keyword   
- float support  
  > check for support with  
  > `options.FEATURE.float`  
  > emulation can be enabled via
  > ```
  > options.FEATURE.float
  > !!^ options.FEATURE.float.emu
  > ```
  > emulation uses [`psh-fractional`](g.transcendent.ink/psh-fractional) or `shmaths`  
  > **NOTICE: psh-fractional is buggy; please use `shmaths`**
- scoping
  > milang uses variable scoping by default  
  > this can however add cpu cycles and be annoying at times
  > state can be swapped with `options.SCOPE`
  >> **!! WARNING: This should only be done per loop or function  
  >> disabling scoping variable is very insecure**
- loop counters
  > loop counters can be useful for various tasks  
  > they can be enabled per loop via
  > ```
  > for.<type> in $var -- options.COUNTER
  > ```