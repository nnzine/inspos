Lua's `math.random()` returns a random number from 0.0 to 1.0. You can also pass in an integer instead and get a nice round number, `math.random(10)`.

Like most systems, this is actually a "pseudo-random number generator". `pseudo` is greek for "lying".... because this is not REALLY random. I don't mean that in the philosophical free-will vs determinism way either... I mean we can actually predict with 100% accuracy what number will come up next by doing something called "seeding" the random number generator. Try this on your norns:

```lua
math.randomseed(42)
print("A random number from 1..100. I predict it will be... mmmm.... 4")
print("Random number:", math.random(100))
print("A random number from 1..100. I predict it will be... mmmm.... 33")
print("Random number:", math.random(100))
print("A random number from 1..100. I predict it will be... mmmm.... 70")
print("Random number:", math.random(100))
```

We can use this as a simple way to explore generative music! Let's make a new norns script; first we'll set up the engine and sequins.

```lua
s = require 'sequins'
engine.name = 'PolyPerc'
```

Now we build a sequence of four "random" frequencies:
```
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

This is pretty fun, but even better would be to have some other parts playing at the same time. Here we can generalize a bit, adding in a bass line:

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

function init()
  local lead_seq = mkseq(42, 4, 400, 800) -- First param is the 'seed'
  local bass_seq = mkseq(7,  4, 50,  200) -- Change the seed!
  run_seq(lead_seq, 0.25, 0.25)
  run_seq(bass_seq, 1, 4)
end
```

When you run this with the same seeds you will get the same tune. Change the lead part seed and only the "random" lead sounds change. Change the bass part seed and only the "random" bass sounds change. This way you can explore different lead parts and bass parts independently by changing these seeds!

You can add more parts, change the lengths and parameters of the existing parts, or even do other isolated "random" actions by setting another seed and generating some numbers for those actions.

Have fun!