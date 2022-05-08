---
title: Variable Scope
author: Brock Wilcox <awwaiid@thelackthereof.org>
---

***META: This is some noise to play around with what some zine content could be like. It will very likely be decimated if not deleted!***

I love variable scope. I dream about it. I apply it to other parts of my life. When I walk through the park and see a branch I wonder about the scope of an individual leaf and of the trunk and the center of the earth and the sun. I think about all the people I know named Zach and how their names collide but I can still tell them apart.

Let's start at the top. My Highschool was fancy and had a programming class taught by Mrs. Denton (who says I can now call er "DD" since it was 25 years ago). She had a few mantras. The primary chant was "loops are fun!". I might be making this up, but the second most important "globals are evil!". Why? Because there can be only one, and anyone can in the program can change it any time, and you never know where it will be read from, and worst of worst two different parts of the program might try to use the same name for two completely different purposes.

Sidebar: I often anthropromophize, or at the very least agent-ize, bits of a program. Sometimes it is a single line of code that "does" things, sometimes a function, sometimes a file. It "wants" things, it "tries" things. Possibly this isn't helpful but also it means I get to see the program as a choir of voices singing one by one and all at once.

It's quite easy to get a global variable in Lua! It's the default. When you assign to a variable that doesn't have any other scope indicated, you get a global. You can read from this variable anywhere you want.

```lua
-- Global
message = "Greetings!"

function show_message()
  print(message) -- read from the global
end
```

Guess what happens when you read a variable that nothing has ever referenced before? It works fine! You get `nil`, but no error. That means that if you write "messege" instead of "message" you will get `nil` but no error. BEWARE!

OK next up -- function parameters. I guess folks say that the "argument" is what you call it when you invoke the function and "parameter" is what you call it from within the function, but I generally call them "params". An important point is that they don't have to match at all.

```lua
function show_message(msg)
  -- from here, "msg" is the parameter
  print("My message is:" .. msg)
end

msg = "fishes"
counter = 7
show_message(counter)
print("Global msg is", msg) -- This "msg" is the global
```

Here we have a variable (a global!) named `counter`. We pass that into `show_message`. Another way to say this is that we invoke `show_message` with `counter`. Same thing.

But from INSIDE of `show_message` we see this as a parameter named `msg`. This variable is "visible" from inside of the function, but not outside. Meaning if you try to do like `print(msg)` outside the function, `msg` refers to the global variable and not the function parameter.

That idea of what is "visible" from where -- that's scope. That's the key thing.

If you have a local variable (or parameter) with the same name as a global variable then we say that the local variable "shadows" the global one. That is to say, the local one wins and overshadows the outside one.

* parameters
* local
  * functions
  * blocks

Vocabulary:
* **Variable:** A named value
* **Parameter:**
* **Leak:** When a concept escapes from what you intended it to be contained to
* **Inside:** The stuff up until the end. End of a function, end of a loop, end of a condition block.
* **Outside:** The stuff ... not inside.
* **nil:** The default value of variables, even ones that you never previously mentioned
* **flow:** The order in which you program is executed
* **Perspective:** given a specific bit of code during the execution, you can think of what that can see when it comes to variables

----

***META: Alternate treatment***

All the scopes in one example. maybe this should be a fun diagram with arrows.

```lua
msg = "Hello humans" -- global

function say_greeting(msg) -- parameter
  local msg = msg .. ", said me" -- local
  if #msg > 20 then
    local msg = msg .. ", more than 20 chars"
    print("Your message is now", msg)
  else
    print("Your message is short", msg)
  end
end
```



[... to be continued ...]
