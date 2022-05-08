---
title: Variable Scope
author: Brock Wilcox <awwaiid@thelackthereof.org>
---

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

* parameters
* local
  * functions
  * blocks

[... to be continued ...]
