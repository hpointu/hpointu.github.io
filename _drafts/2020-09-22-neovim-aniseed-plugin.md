---
layout: post
title:  "Neovim plugins with Fennel and Aniseed"
categories: [blog, dev]
comments: true
---

Here you are, I was waiting for you. No, not you! You, just behind! I recognize you. No, don't say anything, I'll tell you.

# This is your story

Now, remember... Remember that time when you were a little kid, staring at the blackboard in the classroom, not listening to any word coming out of your teacher's mouth. Daydreaming, eyes fixed gazing in the direction of the board.
In your head, that day, you were convinced that you had superpowers. You knew you could lift that chalk only with your mind. You knew you could, you just didn't know how to do it yet. But it was just a matter of discovering The Trick. So you practiced, you stared at the chalk, focusing your mind, the hardest that you could. That shouldn't be that difficult, because it was hidden in you, that energy, you just needed to be able to cast it to the right place. And so you tried, tried really hard to move that little piece of chalk.
Unfortunately, that day, it didn't work. But it didn't matter, you'd try again another day. And an other day.

Now guess what, it never worked. The truth is you never had any superpowers. But that's ok, you came to peace with the idea a long time ago. Now life is a little bit different for you. You had a few jobs, working with computers. Writing code. The code you write is boring as hell.
You're using this old-fashionned editor called Vim. Sometimes you wish your editor could do something more for you. Deep inside, you start to feel like you could unleash some incredible power, if only you knew how to start...

If only... Nah! You remember those candid thoughts you had as a child. How deceptive they were. But now you know better, there are no such things as superpowers. Tweaking your editor has nothing to do with magic anyways, all you need to do is learning this weird language called vimscript, or VimL, you're not even sure about the name, let alone writing any meaningful program with it. All you know is life is too short to bother. So you give up the idea.

Then the years go by...

But one day... One day, you hear about "[Lisp](TODO)". You hear "Lisp" here, you hear "Lisp" there, "Lisp" "Lisp" "Lisp" "Lisp" everywhere (you need to whisper it in your head to get the effect).
So slowly, bit by bit, you start reading about it, you try to understand what this is about. Your curiosity is awake.
You get interested in what other people are doing with it.
Then you start taking it a bit more seriously, you read [SICP](TODO), then your mind explodes.
You give it a go, and you discover the [REPL](TODO). That's it! You're that kid again. You believe in superpowers, once again.

So you and your good old friend Vim are starting to feel alive again, you search around for the best tooling you can find to help you in your Lisp quest.
You notice there are a few awesome people who went through the trouble of writing awesome tools with the awesome VimL. Deep inside you envy their courage, their dedication.
Along the way, you have a vague remembering of something called "[Neovim](TODO)". You decide to give it a go. Wow, so you can actually write neovim plugins in any language you want, since it exposes its API [through RPC](TODO). Interesting. And, what's that?! It also has a [Lua](TODO) runtime! This could come in handy.

But your child dreams are a long way behind you. You don't get over-excited. Until... something unexpected happens...

You stumble across (??) something called "[Conjure](TODO)", written by the awesome [Oliver Caldwell](TODO).
He's talking about a language, [Fennel](TODO), that is a lisp which compiles to Lua. And he's written a [whole suite](TODO) of tooling that makes writing fennel code for neovim a breeze. At least it appears so.
You could definitely use that to write the tools you always wanted in your editor!

But there's still an issue. All this stuff is way too clever for your little brain. It's hard. So you decide to investigate, step by step, and before writing the tools you always wanted, you'll write one that nobody will ever want. Why? Because it's easier. Even you won't use it, but it'll teach you one thing or two.

This is how, you and me, we embark on this journey. Don't mind me, I'm just this little voice whose going to mock your incompetence along the way. But that's fine because I'm in the exact same situation as you. As you might guess, I've never written any vim plugin before. Nor Fennel code.
So don't trust me too much...


# Description of the plugin
Writing blog posts is long and cumbersome. We should automate it. Ok, maybe not entirely, but what we could do is having our editor suggest a list of synonyms for any word we'd like. What a cool idea.

So here's what we're going to do: a plugin that suggests synonyms.

We'll keep it very very simple, remember, we are still learning.

# Let's get started

One of the thing we like the most about Lisp is the interactive way of working with our code. Oliver Caldwell did [a nice post](TODO) about that.
And this is exactly what we are going to try. We know we can rely on Conjure to eval our fennel code.
We will get a mockup of the plugin in this way, evaluating our code in vim. We don't want to bother with details such as packaging, yet.
We want to be able to count on our runtime to provide us with the feedback we need, and build our program bit by bit, and just like this.

Conjure is here to help you doing exactly that. But Conjure is not a small thing. It is built around [Aniseed](TODO), also made by Oliver Caldwell.
Aniseed is this nice piece of software that will make our life much easier to write our plugin in Fennel. But I reckon that this is a lot take in at once, if like me you have no previous experience with this type of stuff. 

So, we'll try to deconstruct it. We'll make the assumumption that Conjure is installed and well configured (although it's working out of the box), but we'll look at things only as Conjure users. Although we'll assume most of the time than we know nothing, I still suggest you take a little detour through the [Conjure School](TODO), at least to get a feel for what Conjure can do for its users.

# Getting familiar Fennel inside Neovim

With just Conjure being installed, we can already start evaluating some fennel code on the fly.
Let's verify that by writing the simplest piece of code we can think of in a new `.fnl` file:

![simple fennel](/assets/fennel/raw_fennel.png)

I just pressed `\ee` (see [Conjure doc](TODO) to learn the basics of using Conjure) on the form to eval, and hop! It appears to be working like a charm. Look, it even prints our text in vim's message buffer at the bottom. We're getting closer to hacking vim now!

Cool, so now we're going to write a fennel function. And we don't want to change a winning team, so let's try to evaluate that :

```lisp
(fn hello [] (print "Hello you!"))
```

Now trying to evaluate that gives us this output from Conjure (using `\er` as a variant of `\ee` to make sure I'm evaluating the whole form. Again, the Conjure doc is quite good to see all key mappings):

```lisp
; eval (root-form): (fn hello [] (print "Hello you!"))
#<function: 0x416b2d18>
```

Ok... It looks like this has been compiled correctly, and it's returning me a function. What about we call the return value on the spot by surrounding our function definition with parenthesis?
(Notice the `root-form` being sent to conjure has an extra set of parenthesis, meaning we're calling the function we've just defined)

```lisp
; eval (root-form): ((fn hello [] (print "Hello you!")))
; (out) Hello you!
nil
```

Great so this is definitely working! We can see the expected output in the conjure logs, and we also saw "Hello you!" within the Vim's message buffer. And of course, my function is returning `nil` because it's just returning with the return value of `print`.

Now we'll make it accept an argument, and try to call it with an aribitrary value.

```lisp
(fn hello [name]
  (print (.. "Hello " name "!")))

(hello "Zozo")
```

Reading the Fennel documentation teaches us that the `..` function is for concatening strings.
If we try to eval, using `\eb`, our whole buffer, we get something like that in conjure's log:

```lisp
; eval (buf): /home/freddy/Dev/nvim/raw/test.fnl
; (out) Hello Zozo!
nil
```

Although if we try to eval the definition of the function and the call to that function separately, something weird happens:

```lisp
; eval (current-form): (fn hello [name] (print (.. "Hello " name "!")...
#<function: 0x40c7e9e8>
; --------------------------------------------------------------------------------
; eval (current-form): (hello "Zozo")
; Compile error in /home/freddy/Dev/nvim/raw/test.fnl:2
;   unknown global in strict mode: hello
; 
; * Try looking to see if there's a typo.
; * Try using the _G table instead, eg. _G.hello if you really want a global.
; * Try moving this code to somewhere that hello is in scope.
; * Try binding hello as a local in the scope of this code.
```

Let's try to analyze what's happening: `unknown global in strict mode: hello`. It's as if our `hello` function doesn't belong to the scope where we invoke it.

Well, it kind of makes sense: when we evaluated our code, Conjure sent it to Fennel, who compiled our function and yielded it in one "compilation unit". But that compilation unit didn't persist anywhere. Then we're trying to refer to that function in an other compilation unit, without providing any mean to fennel to know what we want. Remember, we're evaluating stuff on the fly, our code has no "location" from the fennel compiler perspective.

It seems that using this bare-minimal method of evaluating sporadically one form or another, we can't go very far.

# Ahead of Time (AOT) compilation

The easiest solution that I can think of is to work at a file level, and when we're happy with the functions we're writing in a particular fnl file, we can use the fennel compiler ourselves on the entire file. This will give us a `.lua` file that we can place in our vim path, and load it as a normal lua file.
This is what Fennel is calling [Ahead of Time compilation](TODO).
That way, our code will have a location for fennel to use and refer to. We can then rely on lua's `require` function to load our code.
One obvious benefit from this approach is that there won't be any overhead to loading our code over what we'd be doing with normal Lua plugins.
Although this is a perfectly valid solution for distributing our code, we're looking for something a bit more interactive, leveraging the quick feedback loop that conjure provides for us.
If possible, we'd like to avoid such a slow feedback loop, so we'll try to do better.

# Leveraging Lua's module system

The AOT compilation described above defines clear compilation unit, the scope of the file we're compiling.
This is a bit coarse, and compared to evaluating the whole buffer at once, like we did earlier with `\eb`, the only additional feature we are getting from this is the ability to load code from another compilation unit.
This is already not bad, but we want to do better. We want to be able to persist the result of our evaluations in such a way that we can eval bits and bobs here and there, interactively, to incrementally build our system.

What would be helpful to us is to have some kind of register somewhere, in which we could add/replace some variables or function definitions.

Of course, there is the Vim global scope itself. We can declare global vaiables in vim and store some values in there. But as far as I'm aware, we can't store function definitions there.

An other option is the way Lua is looking up for existing code.
I won't go into too much details there, mainly because I don't know them myself, but naively put, Lua "modules" are simply associative tables that are loaded when some code call `require`.
The calling code is then responsible for keeping a ref to that table, usually in a local var, and search functions to call within that map. Surely, because Fennel compiles to Lua, it must be easy to use that mechanism in our plugin.

Nothing new here, we already saw that with the AOT techinque, we can require code, blablabla. You're right. But! Two things are of particular importance to us.

- The first one is that `require` effectively maintains a global table. This table is  called `package.loaded`, and is essentially a mapping between a Lua module path, and the module content of that path (an other associative table).

- The second one is that tables are kept as mutable references.
What this means is while the reference itself can be constant, the underlying table can change.
So every client code looking up for a func or a var in that table might get a totally different thing depending on the time of the day! Although this feature might seem dangerous to someone caring a little bit for immutable data, this is something we can put to our benefit.

The first point gives us an answer to the fact that our code needs a place to live within the runtime.

The second point gives us an answer to the need of redefining stuff on the fly.

With these ingredients, we can stay within our runtime, and yet be able to interact with previously defined pieces of code (functions or values) without having to ever restart anything.

But there is still an issue: whenever we want to evaluate some code, we need to make sure it contains all the necessary information to perform the right lookups to our global table.
Either to register itself to the right place, or to retrieve things that have been defined some time earlier.

So, we need some kind of abstract concept that would wrap every piece of code we evaluate inside a context, a namespace of some sort. Defining things should then be done within that context, as well as referring to other things should then be relative to that context.

Well... Guess what. Aniseed macros define exactly such an abstraction.

Everytime we evaluate something with Aniseed within Conjure, it is wraped in a "module". There is a `module` macro, that allows us to define a context per file. Aniseed also provide a few macros to define new things, `def` or `defn` for example, which have the effect of registering our definition within the context of the module we're writing.

There is no need to dig more in details about the way Aniseed does that, for our purpose. All we need to know is we got our backs covered to address the few organisational issues we saw while trying to define some code.

Aniseed provides much more than these module-related macros.
Its `core` module provides a set of functions inspired by Clojure, such as `map`, `mapcat`, `assoc`, `get-in`, etc.
There is a `string` module for a few String related utility functions,
a way to run some tests with `test`, but probably one of the most useful is a very small `nvim` module, that bridges to a lua-friendly neovim api. More on that later.

# Using Aniseed and project (non-)setup

Now that we understand why we really want to use Aniseed to write our Fennel plugin, let's see how we can use it.

Let's start by creating our project directory structure.

We'll call our plugin "More like that". Because it will suggest more words, like that one.

```bash
mkdir more-like-that && cd more-like-that
mkdir -p fnl/more-like-that
```

The `fnl/more-like-that/` directory will hold our code.
Let's create our first file in there:

```bash
touch fln/more-like-that/main.fnl
```

Now let's start editing it with our favourite editor (Neovim with Conjure installed being the only choice, thank you).

If you remember our plan from the begining, we don't want to bother with packaging, yet.
So we're going to take a shortcut, and install Aniseed directly in our vim runtimepath. Go to [Aniseed](https://github.com/Olical/aniseed) repository and follow the installation steps.

I'm using vim-plug, so I'm adding that to my vim config, as instructed on the Readme for Aniseed.

```
Plug 'Olical/aniseed', { 'tag': 'v3.7.0' }
Plug 'bakpakin/fennel.vim'
```

When we'll want to share our plugins to the world, we'll need something more definitive. That is, shipping our own version of aniseed along with our plugin, so it is self contained as well as making sure our aniseed version doesn't conflict with any other plugin.
But let's not bother with that yet.

All we want for now, is being able to build our plugin code by just evaluating some forms on the fly. Just because I want to. You'll see, you'll like it as well.

So here we are, inside our `main.fnl` file, and we're going to write this :

```lisp
(module more-like-that.main)

(defn hello [name]
  (print (.. "Hello, " name "!")))
```

Let's pause for a bit. Here, we're using two Aniseed macros, `module` and the `defn`. `module` defines a context in which further definitions will live. `defn` defines a function within that context.

We can eval the whole buffer to let the runtime know about our module and our function, with `\eb`.

Now that our vim knows about our function, we can evaluate this form on its own, within that module : 

```lisp
(hello "you")
```

Just pressing `\ee` on it, and we shall see our previously defined function being called and our message being printed.

```lisp
; eval (current-form): (hello "you")
; (out) Hello, you!
nil
```

The way this works is that Conjure scans our buffer to find a module definition, and evaluates the form within the context of that module definition.

It seems that we're having all the tools we need at our disposal. I don't know you, but I'm pretty excited by this.
Take a moment to realize: With nothing more than Conjure installed in our Vim, and a copy of Aniseed somewhere in our vim path, we can build a software inside the Vim runtime, with an incredibly tight feedback loop, without ever having to reload files or to restart anything.
All we need is to eval some forms with Conjure.
And in fact, putting Aniseed in our runtimepath isn't striclty necessary in the previous example, since we're not requiring any of its functions.
We're just using 2 macros that are always available in Conjure's Aniseed client.
I'm in love.

# Interacting with vim

So, now that we're starting to understand the tools we're using, let's actually think abour our plugin.

We're going to have some kind of synonyms provider. We'll abstract that as a function that takes a word as a parameter, and return a list of new words or expressions, to suggest to the user.

The provider for such synonyms is to be decided yet, for now what is itching my brain is not that part, but the way we'll display our suggestions.

I simply have no idea of how to do that with vim.

This is a pretty dumb function... Whatever word you give to it, it returns the same list of words.

But that's enough for us, we'll use that as our suggestion list to display to the user. Don't forget to evaluate this function so it's part of our runtime.

The kind of UI we want is similar to vim completion popup, I suppose. That is, a little popup list that opens next to the cursor location, let the user select something, and confirm the choice to replace the targeted word.

After reading a bit of the documentation in vim `:h complete` and `:h completefunc`, it seems to me that while this is probably perfecty suitable for our need, we won't have much room to experiment, as the `completefunc` and `omnifunc` features in vim are pretty much made for text completion, triggered in insert mode, with a very specific way to register a very specific function.

We'll have a bit more fun and we'll use floating windows instead.

Let's try to define a very simple function, to open a window in vim. It will take a position relative to the vim's main window (a column number and a row number) as well as a size (width and height):

```lisp
(defn open-window [col row width height] ...)
```

Now I'll let you read the documentation for `:h nvim_open_win`, and `:h nvim_create_buf`, since we'll probably need some text to but inside our window.

TODO: elaborate. nvim.* ? Aniseed.
Elaborate also the idea behing buffer/window.

Our first version will look something like that:

```lisp
(defn make-window
  [col row width height]
  (let [buf (nvim.create_buf false true) ;; a non-listed, scratch buffer
       ;; defining a few options for my window:
        opts {:style "minimal"
              :relative "editor"
              :width width
              :height height
              :row row
              :col col}]
    ;; "wipe" seem to mean we get rid of the buffer when it's hidden,
    ;; as in, for real. No remain. I like that.
    (nvim.buf_set_option buf "bufhidden" "wipe")
    ;; This is the call to actually open the window
    (nvim.open_win buf true opts)))
```

I'm still not too sure about the options I chose. I'm still discovering all that, so I picked things that
seemed to make sense from the documentation.

Now you can evaluate this function, using Conjure, and try to call it.

This is me trying different positions and size for my window:

<script src="https://asciinema.org/a/q3K122jNJgxBPMVLmBWSqXEMO.js" id="asciicast-q3K122jNJgxBPMVLmBWSqXEMO"></script>

Here, you can start to get a feel of what it's like to be able to evaluate code on the fly, and see the result immediately.


TODO: start populating the content of the window
...

So we'll setup this simple function, that will serve as a stub that we're going to use to try out a few different ways of suggesting words to the user :

```lisp
(defn get-synonyms [word]
  ["bloke" "thesaurus" "spider" "gregorian"])
```
