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