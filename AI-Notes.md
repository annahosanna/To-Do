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

After a linebreak, refine the specification. In this way a single line does not need to accurately describe the entire project. Repeat adding new lines with a linebreak to continue to refining the project.

At some point test the project. Build it and identify any build errors. Specify the text of the build error and ask Q to fix it. (for swift for instance; however it automatically did this process for Java)

Once the program builds review the functionality against what you expected, try to figure out missing feature, bugs and edge cases. Then ask Q to few
