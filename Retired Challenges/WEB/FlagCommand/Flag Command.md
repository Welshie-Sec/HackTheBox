# Exec Summary
Flag Command is a very easy web challenge that only requires looking at source files and looking at what request you make.

# Stats
* Difficulty: Very Easy
* Time to complete: Very Short
* Tools Used: Burpsuite Community, Browser

# Challenge Description
Embark on the "Dimensional Escape Quest" where you wake up in a mysterious forest maze that's not quite of this world. Navigate singing squirrels, mischievous nymphs, and grumpy wizards in a whimsical labyrinth that may lead to otherworldly surprises. Will you conquer the enchanted maze or find yourself lost in a different dimension of magical challenges? The journey unfolds in this mystical escape!

# Solution

So pulling up the challenge, we are met with all the flavor text and then appears to wait for commands. If you happy path it you will notice that a specific route gets the farthest before the game needs to be reset. Pulling up the source for the page, we see three .js files.

In the HTTP Request section of main, we see how the flag is produced and how the game plays out to a point.


![main_js_flag](https://github.com/user-attachments/assets/c7e8988f-0b13-448d-ba09-4218f62c16a6)


So following this point and getting to step 4, the next step or the win condition doesn't seem to appear. I was doing this in burpsuite and noticed that it caught a particular request to the options section of the API.

![api_options](https://github.com/user-attachments/assets/13c2d18b-7659-475c-b6f6-56cfa8e2e500)


So at different stages of the game it shows different options...and a secret string. Inputing that string after doing the START input will lead to the flag output for this challenge. Originally I thought that it had to be done once you reach the last stage, but no it doesn't seem to require it.

![flag](https://github.com/user-attachments/assets/ac89e719-76b8-4a36-8321-a9e1e53ab873)



