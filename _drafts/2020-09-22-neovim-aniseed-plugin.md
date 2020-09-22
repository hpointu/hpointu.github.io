# Come closer my friend

Here you are, I was waiting for you. No, not you! You, just behind! I recognize you. No, don't say anything, I'll tell you.

Now, remember... Remember that time when you were a little kid, staring at the blackboard in the classroom, not listening to any word coming out of your teacher's mouth. Daydreaming, eyes fixed gazing in the direction of the board.
In your head, that day, you were convinced that you had superpowers. You knew you could lift that chalk only with your mind. You knew you could, you just didn't know how to do it yet. But it was just a matter of discovering The Trick. So you practiced, you stared at the chalk, focusing your mind, the hardest that you could. That shouldn't be that difficult, because it was hidden in you, that energy, you just needed to be able to cast it to the right place. And so you tried, tried really hard to move that little piece of chalk.
Unfortunately, that day, it didn't work. But it didn't matter, you'd try again another day. And an other day.

Now guess what, it never worked. The truth is you never had any superpowers. But that's ok, you came to peace with the idea a long time ago. Now life is a little bit different for you. You had a few jobs, working with computers. Writing code. The code you write is boring as hell.
You're using this old-fashionned editor called Vim. Well, actually, you're using Neovim, because it fits better in my story, but that's not important yet. Sometimes you wish your editor could do something more for you. Deep inside, you start to feel like you could unleash some incredible power, if only you knew how to start...

If only... Nah! You remember those candid thoughts you had as a child. How deceptive they were. But now you know better, there are no such things as superpowers. Tweaking your editor has nothing to do with magic anyways, all you need to do is learning this weird language called vimscript, or VimL, you're not even sure about the name, let alone writing any meaningful program with it. All you know is life is too short to bother. So you give up the idea.

Then the years go by...

But one day... One day, you hear about "Lisp". You hear "Lisp" here, you hear "Lisp" there, "Lisp" "Lisp" "Lisp" "Lisp" everywhere (you need to whisper it in your head to get the effect). So slowly, bit by bit, you start reading about it, you try to understand what this is about. Your curiosity is awake. You get interested in what other people are doing with it. Then you start taking it a bit more seriously, you read SICP, then your mind explodes. You give it a go, and you discover the REPL. That's it! You're that kid again. You believe in superpowers, once again.

# Description of the plugin
So here's what we're going to do. A thing that suggests synonyms

# Description of Aniseed / conjure and all the rest

# Let's get started

---- Draft plan
- first we won't bother thinking about packaging and stuf, we'll do all our evaluation using Conjure with aniseed
- ...
- Much later on, create the plugin/mlt.vim file

What draws me to lisps is the interactive way of working with your code. Olical did a nice post about that --TODO-HERE--.
And this is exactly what I'm going to try. I know I can rely on Conjure to eval the fennel code I'm writing through aniseed.
I'll get a mockup of the plugin in this way, evaluating my code in vim. I don't want to bother with details such as packagin, yet.
I want to be able to count on my runtime to provide me with the feedback I need, and build my program bit by bit, like this.

With just Conjure being installed, I can already start evaluating some fennel code on the fly.
[PHOTO] Just conjure, no aniseed

Issue, loading a module defined somewhere else.

Introducing aniseed macros, such as module and all.

And now it's magic.
