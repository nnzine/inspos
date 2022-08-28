Randomness! What even is it and how can we use it in music? The real and often unintuitive mean of `random` is `not predictable`. I don't mean this in a statistical sense, like "heads will come up about half the time", but instead "I cannot tell you with complete certainty if the coin-toss will be heads or tails".

## Randomness in Lua

A source of the un-predictable can be very useful, especially in art, and Lua provides a built-in way to generate random numbers. Lua's `math.random()` returns a random number from 0.0 to 1.0. You can also pass in an integer instead and get a nice round number, `math.random(10)` gives a number from 1 to 10. We can use this as the foundation for collaboration between human and machine. What should the new volume be? `math.random()`. Which of these 5 notes should we play next? `math.random(5)`. Your machine collaborator can contribute in ways that you can't predict.

Alas, this is a bit of a lie. It turns out that making a machine, which is built on predictability and logic and consistent repetition, making this machine generate random numbers isn't so easy. So Lua, like most computing systems, fakes it with a "pseudo-random number generator". `pseudo` is greek for "lying" or "fake".... because this is not REALLY random, and is instead "difficult but not impossible to predict". We can make it _completely_ predictable, actually, by doing something called "seeding" the random number generator with an unchanging value using `math.randomseed(...)`. Try this on your Norns Maiden REPL:

```lua
math.randomseed(42)
print("Random number:", math.random(100))
```

I bet you $100 USD that it will be `33`. Now try `math.random(100)` again. Double-or-nothing I bet it'll be `70`. Shall we go one more? OK, run `math.random(100)`.... and you'll get `43`. No, I am not a prognosticator -- the "seed" determines the sequence of results. If you use a different seed instead of `42` you will get a different sequence. If you run `math.randomseed(42)` again the sequence starts over with `33`.

## Practical Application: Melody Generator

We can use this as a powerful yet simple way to explore generative music! Let's make a new Norns script; first we'll set up the engine and sequins.

```lua
s = require 'sequins'
engine.name = 'PolyPerc'
```

Now we build a sequence of four "random" frequencies:

```lua
function init()
  math.randomseed(42) -- change this to explore!
  lead_notes = {}
  for n = 1,4 do
    table.insert(lead_notes, math.random(400) + 400)
  end
  lead_seq = s(lead_notes)

  clock.run(lead)
end
```

Finally, we continuously play these frequencies:

```lua
function lead()
  while true do
    engine.release(0.25)
    engine.hz(lead_seq())
    clock.sleep(0.25)
  end
end
```

Run this and you should get four repeating beeps looping. Now change the `42` to `43` and run it again -- you'll get a different "random" sequence. Now change it back to `42`.... and you're back to the first sequence! You have a whole lot of integers to choose from, so try out a bunch to see what different sounds you get.

## From Melody to Song

This is pretty fun, but even better would be to have some other parts playing at the same time. We'll give each part its own seed! Each seed gets a separate sequence of numbers. With a seed for each voice you can modify one seed and explore while the other voices remain unchanged. Let's generalize a bit, building some general functions and adding in a bass line:

```lua
-- Build a "random" sequence of frequencies
function mkseq(seed, count, low_freq, high_freq)
  math.randomseed(seed)
  local freqs = {}
  for n = 1,count do
    table.insert(freqs, math.random(high_freq - low_freq) + low_freq)
  end
  return s(freqs)
end

-- Endlessly play a sequence of frequencies
function run_seq(seq, sleep_time, release)
  clock.run(function()
    while true do
      engine.release(release)
      engine.hz(seq())
      clock.sleep(sleep_time)
    end
  end)
end

-- Here is where the script starts
-- Set up two sequences (voices) and get them going!
function init()
  local lead_seq = mkseq(42, 4, 400, 800) -- First param is the 'seed'
  local bass_seq = mkseq(7,  4, 50,  200) -- Change the seed!
  run_seq(lead_seq, 0.25, 0.25)
  run_seq(bass_seq, 1, 4)
end
```

When you run this with the same seeds you will get the same tune. Change the lead part seed and only the "random" lead sounds change. Change the bass part seed and only the "random" bass sounds change. This way you can explore different lead parts and bass parts independently by changing these seeds!

## Going Further

You can add more parts, change the lengths and parameters of the existing parts, or even do other isolated "random" actions by setting another seed and generating some numbers for those actions. Take a look at `lissadron` norns script as a direct example of some seeded-sound generation. Other scripts, like `less-concepts`, indirectly get at similar sequences through generative algorithms such as cellular automata.

Have fun!
