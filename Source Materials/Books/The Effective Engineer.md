Tags:
- [[Career Skills]]
---
## Chapter 1: Leverage
* $Leverage = \frac{Impact Produced}{Time Invested}$
- Increase your leverage
	- Reduce time taken to complete an activity
	- Increase the output (value/impact) of a particular activity
	- Switch to higher leverage activities
- Ensure your energy is directed towards real leverage points, not just easy wins. Some high-leverage activities require consistent, high amounts of time and effort, but have massive impact

## Chapter 2: Optimise for Learning
- Growth mindset: believe you can improve your abilities through hard work
- Learning compounds over time
- What makes a work environment conducive for learning
	- Fast growth (promotions, core business metrics, overall company growth)
	- Training
	- Openness (information sharing, prioritisation, learning from failures)
	- Pace (iteration speed)
	- People
	- Autonomy
- Learning at work
	- Study core code from your company's best engineers
	- Write more code
	- Go through internal resources for technical / educational material
	- Master your core programming languages
	- Send code reviews to the harshest critics
	- Enroll in classes for targeted areas
	- Participate in design discussions
	- Work on a diversity of projects
	- Make sure you're on a team with some more senior engineers you can learn from
	- Don't fear code you don't know. Jump in anyway.
- Learning outside work
	- New languages and frameworks
	- Skills in high demand
	- Books
	- Discussion groups, talks, conferences, meetups, etc.
	- Build and maintain a strong network
	- Follow bloggers who teach
	- Write to teach
	- Side projects (_actively_ pursue what you're passionate about)
	
## Chapter 3: Prioritise Regularly
- Prioritise tasks with the highest impact per unit time
- The act of prioritisation is one of those high impact tasks, schedule it regularly
- Track to-dos in a single, easily accessible list
- Protect your Maker Time. Block off hours on your calendar
- Don't try to multi-task too many things at once
- If-Then plans: _if_ (trigger) _then_ I will (do something)
	- counters procrastination
	- e.g. If it's after lunch then I will xxx, If I have 20 minutes before my next activity, then I will (do some short task)

## Chapter 4: Invest in Iteration Speed
- identify your bottlenecks and shorten your worst bottlenecks
- deployment cycles: use continuous deployment with small changes each time
- development time: invest in mastering / building tools to save time coding, compiling, and testing
	- master your IDE
	- get familiar with Unix CLI commands
	- prefer keyboard over mouse
	- automate manual workflows (>2 times, automate)
	- test ideas on an interactive interpreter / REPL
- debugging: shorten the testing workflow by setting up scripts / tools
- non-engineering bottlenecks: seek approval / feedback early, communicate with collaborators to avoid getting blocked

## Chapter 5: Measure What You Want To Improve
- Importance of good metrics
	- Helps focus on the right things
	- Guard against regressions
	- Measure your own effectiveness over time (i.e. your leverage)
- Set up instrumentation to collect key metrics
- Memorise useful numbers (e.g. common latencies): Helps estimate metrics of a design before even building it
- Your best defense against data abuse is skepticism - does the conclusion from the data actually make sense? Is the data even correct?
- strategies to improve data integrity
	- log data liberally
	- build tools to iterate on data accuracy more easily
	- write end-to-end tests on the entire analytics pipeline
	- examine collected data and suspicious numbers ASAP
	- cross validate by calculating the same metric in multiple ways and comparing the results

## Chapter 6: Validate Your Ideas Early and Often
- The shorter each iteration/feedback cycle, the faster we can learn from our mistakes
- quick ways to get feedback on a new feature
	- Build an MVP instead
	- Run A/B experiments
	- Fake change: e.g. Asana tested a signup button's impact by making a fake one that only displayed a popup saying the feature wasn't ready yet. Only after ascertaining the impact did they implement it
- ways to get feedback on your own implementation
	- Commit and send code for review early and often
	- Bounce ideas off teammates
	- Design API first: makes all the interactions between parts clear
	- Share a design document before starting development
	- Schedule projects to run in parallel with teammates' related projects => shared context, easy feedback
	- Get buy-in for controversial features: surface your ideas and build prototypes
- In all decisions, build feedback loops - either direct feedback or metrics

## Chapter 7: Improve Your Project Estimation Skills
ways to improve estimation accuracy:
- Decompose project into granular tasks. Long estimates are more inaccurate
- Estimate based on how long the task will take, not how long you want them to take
- Estimate with a probability distribution
	- e.g. "50% chance this gets done in x days, 90% in y days"
- Let the person doing the task make the estimate
- Avoid the anchoring bias (chapter 6 in [[Never Split the Difference]])
- Estimate via multiple approaches
	- e.g. task-by-task, section-by-section, comparing against historical data, etc.
- timebox open-ended tasks (e.g. research)
- allow others to challenge estimates
- add in buffer time to prepare for the unknown

ways to deliver within the estimated time
- Set milestones and prioritise based on that
- Validate the riskiest parts first

miscellaneous tips
- Beware of rewrite projects. They are very easy to underestimate.
- Don't sprint in a marathon: don't rush yourself, overwork, and burn out

## Chapter 8: Balance Quality with Pragmatism
- Establish sustainable code review processes
- Develop good abstractions
	- easy to learn and use, even without documentation
	- hard to misuse
	- satisfies requirements
	- easy to extend
	- appropriate to audience
- automate testing
	- but don't get hung up on 100% code coverage
- promptly repay tech debt

## Chapter 9: Minimise Operational Burden
- easier operation => easier to scale
- Embrace Simplicity
	- the first solutions are usually very complex, distill them into more elegant, simple solutions
- Build Systems to Fail Fast
	- Bad changes should make the system fail immediately, so that they can be fixed immediately and the offending change can be easily found
- Automate mechanical tasks
	- caveat: trying to automate decision-making is significantly more costly
- Make batch processes idempotent
	- easy to retry
	- allows you to frequently schedule tasks that are only run occasionally (same as failing fast)
- Practice responding and recovering quickly
	- Disaster recovery testing
	- preparing scripts for possible situations

## Chapter 10: Invest in Your Team's Growth
- Interviewing
	- identify desired qualities with your team
	- review effectiveness of existing hiring processes
	- design interview questions with multiple levels of difficulty
	- control the interview pace, don't let the candidate get stuck or ramble on for too long. Give hints or move on to another question
	- quickly scan for red flags by rapid-firing short, fundamental questions
	- periodically shadow other interviewers
	- don't be afraid to use unconventional approaches
- Onboarding
	- codelabs
	- onboarding talks
	- mentorship
	- (real-life) starter tasks
- Share code ownership
	- review each other's code / design
	- rotate tasks across team
	- keep code quality high
	- document code and workflows
	- teach and mentor others
- Post-mortems
	- after some issue
	- how and why it happened
	- what can be done to prevent it happening again
- create a great engineering culture
