## This document is my thoughts on AI and how it has affected me

### How I feel about AI
Using Amazon/AWS Q I was able to develop an iOS app without prior knowledge of Swift or iOS API's

Learning to use Q chat took a while. Perhaps this is true of any similar technology.
By learning Q I mean that it would generate code which could be significantly different based on just a few words in the functional specification.
For instance adjusting the functional specification to be more specific resulted resulted in using an old version of Swift.
Thus instructions had to be added to the functional specification to use a current version of swift.

I have tried both specifying the requirements as a single very very long sentance (If there was a period it had to be on the same line), and through reviewing the results emitted by Q, the sentance was revised until the revise the correct output was generate.

Q is quite smart with debugging. For instance if it generates source code it can automatically compile it, build it, review build errors, fix the relevant source code, and try again. In this case I was using Java.

Another way to use Q if via a Here Doc like: q chat -a <EOF whatever EOF
In this way multiple sentances could be used. The first line is a general best effort to specify a program. There could be several sentances without a linebreak. The length of each sentance is shorter and the length of the line is likely to wrap only a few times.

After a linebreak, refine the specification. In this way a single line does not need to accurately describe the entire project. Repeat adding new requirements with a linebreak to continue to refining the project. For instance a statement might be 'persist all user data entered', and on a newline enter 'don't persist field 1'

Its important that words in a sentance are not ambigious. Try to use specific words that do not need context to understand.

At some point test the project. Build it and identify any build errors. Specify the text of the build error and ask Q to fix it. (for swift for instance; however it automatically did this process for Java) After the major defintion, but before the refinement is when I performed the inital build and worked through any build errors.

Once the program builds review the functionality against what you expected, try to figure out missing features, bugs and edge cases. Then specify the functionality you would like refined or added. Things to look for in simple mobile apps might be: Is data persisted? Does the program let every element in an array be deleted when there must be at least one? Is it reimplemenmting functionality already provided by the OS? If a list is very large does it cause logic or UI issues (to big to display on screen)? Does the application function as expected when it is in the background and then brought to the foreground (background apps are suspended so clock might display the wrong time). Its also possible with many changes dead code sections and unused variables may occur. (but your compiler should notify you). And finally the code and structure may or may not follow best practices. Particularly with regard to security. Assume that if you have not explicitly stated how to implement something in a secure way, that it was not implemented with security or data validation in mind.

Just to be safe always commit to a remote repository frequently. If something goes really wrong the whole project may be removed, in which case a local git repository will not do any good.

### Most of all
It is so easy to generate lots of code that the joy of creating, creativity and problem solving was not preseant. :(
Instead it was an exercise is knowing what I wanted and figuring out how to ask for it (while that was problem solving it wasn't really creativity).
I did still have to have knowledge of data structures "create a many to one map using thing as a key" And if this was a much more involved project I would have specified the details of an object. Thus some amount of thought and experience needs to go into creating the specification. Which is to say you should be able to look at what is being generated and know if its what you intended, and when asking for something you already know what the outcome should be. Furthermore you still need to know libraries, and system provided functionality so that you do not reinvent the wheel.

So I was left feeling uninspired when in fact I should have felt that I could quickly create every piece of software on my idea list. (Most of this work via Q ultimately leveraged Claude 3.7/4.0) Furthermore I should have felt like there were even more projects I could dream up since I was no longer limited by the programing languages and platforms I was familiar with. Perhaps thats still to come and then I will feel like it is a PITA when I have to write code.

Generally the first line of my query would start out: `Create a new project, writien in ___ , named ___ . Do not use stubs.`

In a CI/CD pipeline, the software may build fine, but I definely would want to run this through software which catches security issues. (Because AI software tracks context, I wouldn't be surpised if there was already software to identify security issues in complex data flows)

So before you become uninspired think of all of the problems you would like to solve for which another solution does not exist (or it does but there is some barrier to entry)

At some point I would like to update this document with information about MCP, n8n, langchain, agents, and vector databases. Currently I feel like there are those who know it in theory, and those who have actually implemented it - but no one to walk though in plain english how to get from the theory to the implementation. (I understand things best when I can see it in code)

AI is good at:
Loweriing the barrier to entry: Easy to create ideas. Easy to re-implement something that you cannot obtain because it cost money or because it isn't available. (but you still have to know how to get from point a to point b, and be willing to know or discover the pieces in between)

In my opinion what does this mean for entry level software developers and computer science in general:
While fewer people will be needed to turn out code, those that use AI will still need to have a good knowledge of how all of the parts of their project should work, or else they will not be able to create detailed requirements, or troubleshoot issues beyond what the AI understands.
I'm concerned there will be a decrease in technology inovation and the drive to invent the newest best algorithm or whatever. The brain trust for new ideas and new solutions will be smaller. I would guess the number of CS PhD's would decrease and thus so would thesis ideas.

What does this mean for MBA (or just business roles in general):
Those that define how the business will function and how they will make money will be much closer to defining the requirements which are fed into the AI to create. Perhaps it would be benifial to Major in business and minor is CS visa versa.
This will increase business efficency using existing processes and ideas. (but is unlikely to generate new ideas which would revolutionize the business) 

Example: More AI agents and more context sources will be required. For instance if an AI were to create a new operating system it should have information about what users are missing from their current operating system - in fact it will probably need context about what users like too so that functionality is not lost) I think often we build on (large complex) ideas, and take for granted the value being built upon. I think that people talk about explicit technical debt (what needs to be fixed), without regard for enumerating everything that works and how it was implemented. If you were trying to explain it to AI you would not only need to express what needs to be fixed but also everything that works correctly and even the rules for how it works. (api call best practices)

So I implemented an MCP server as follow:
REST api wrapped MCP logic
The calls to the rest endpoint can be wrapped as "tools" and registered with llm
then the message can be passed to the llm and it will return the call the to the tool or an error
but that would be a huge number of tokens so a vector database can be used to determine most likely answers (vector databases measure similartiy), the other thing vector databases can do is provide suplemental related information (if you need a vector database try postgres + pgvector + pgvectorscale)

so instead of having to send a huge amount of data to be turned into tokens (4/3 the number of words) only a small amount as determined by the vector database needs to be sent
Larger chucks help more context in a vector database but to make sure no content is lost chunks should overlap by 25% to be safe.

After aa few days I realized now instead of prioritizing project and figuring out what I want to experiment with, I can just check if someone has already written the app I need and if not I'll write it myself in a very short period of time. There is no longer a piece of software I cannot make if I do not like what is out there. No more wishing for features so that it would fit my use case. Certainly for complex problems it is not possible, but for simple problems it works.

So far
I wanted a stopwatch app with special timing and alerting abilities (inherited time, integration with apple watch, meeting notes, send to slack)
I wanted an app that could save workflows in a standard way (bpmn) but I wanted to extend it so the optionally I could click on a task and take me to the relevant documentation/web page for that task. While it doesn't have a beatiful UI, and it doesn't have pretty line routing, It does what I need. (start, end, gateway, task, lines with arrows, plus clickable)(UI needs to be cleaned up, short cut keys, and mouse clicking needs to be intuitive, selection speed/application more responsiveness, less precise mouse clicks, editing more intuitive, adding new objects easier.

Writing a query for an llm is interesting. It could spit out a different answer every time. You need some kind of 'guard rails' which first ask the llm to create something and then another query to ask the llm to validate it. (you would kind of think the first query would have returned a valid answer) Thus it is important to notice what the llm sometimes gets wrong and what it always gets right (one way or another) I have even had llm spit out code and it could not figure out what was wrong until a human pointed out that the area of focus was much larger than the control, and that the problem was not with the control.) 


