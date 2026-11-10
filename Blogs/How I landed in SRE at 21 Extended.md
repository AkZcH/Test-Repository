

Forty interns were sitting in one meeting room, staring at a large screen. Managers were calling out names one by one or in small groups, and the room had that particular mix of boredom and nerves that only shows up when everyone is waiting to learn where the next few months of their life will happen.

I had joined the company as a Product Engineering Intern. I expected to end up near the existing product teams, working with Java, Spring Boot, React, Python, MySQL, GitLab and Jira. Then the infrastructure allocation appeared on the screen. I looked at the infrastructure tab and saw a single name.

Mine.

I was the only intern allocated to infrastructure. I was now, in a very real sense, an SRE, and I was suddenly different from every other product engineering intern in that room.

The satisfaction I felt was not the "I worked harder than everyone" kind. It was quieter than that. It felt like all of the time I had poured into Linux, networking, Rust, Docker, Kubernetes, and distributed systems had finally led somewhere. Nobody had assigned me that path. I had declared it for myself, and now there was external proof that the direction had value.

But this story doesn't really begin in that room. It begins years earlier, with a completely ordinary first semester and no idea where I would end up.

---

## I Didn't Start Out Wanting to Become an SRE

My journey began like most CS students' do.

In my first semester, I learned HTML, CSS, and JavaScript. I didn't understand the landscape of software engineering at that point. Web development was simply the most visible doorway into programming, so I walked through it.

In the second semester, I moved into Python. In the third, I threw myself at React, Node.js, and a bit of Express. I was following the conventional application development path: frontend, backend, APIs, databases, languages.

Then I discovered machine learning.

My Python background made the transition easy. I built basic models, then moved toward deep learning, and later into RAG, LLMs, context windows, tokenization, embeddings, and attention mechanisms. It was a genuinely exciting time.

But something was quietly changing. I was learning a lot, and I was becoming more curious about what sat underneath the things I was building than about the things themselves.

## The Problem With Staying at the Application Layer

There was a point where I realized I was drifting away from the level of engineering that fascinated me.

Building an application is one thing. Understanding what happens beneath it is another. I started getting pulled toward operating systems, networking, memory, processes, concurrency, hardware interaction, and the mechanisms that make software actually run.

The trigger was Rust.

A friend of mine was working with it, and I found the language compelling because it brought me closer to low-level programming while still offering modern abstractions. It gave me a way to build things that interacted directly with the underlying system. I loved the idea of programming that could, as I liked to put it, flirt with the hardware and the network.

There was a second motivation, and I'll be honest about it. I noticed that many students were following similar paths: web development, JavaScript frameworks, Python, machine learning, Spring Boot, databases. None of those paths are bad. They are perfectly valid. But I wanted a differentiator. I wanted to understand the bottom layer instead of only consuming the abstractions stacked on top of it.

And Rust had a reputation. People kept describing it as hard to learn. For me, that made it more attractive, not less. If something was both difficult and fundamental, I wanted to find out whether I could understand it.

---

## Systems Programming Became the Turning Point

My first serious systems projects changed how I thought about software.

### TurboNet

One of them was TurboNet, a Rust-based user-space networking library and client-server framework. It offered asynchronous client and server primitives, connection multiplexing, and pluggable protocols for building high-throughput network services.

Under the hood, it used async/await, Tokio for asynchronous I/O, the `bytes` crate for buffer management, Serde for configuration and serialization, Criterion for benchmarking, Cargo for dependency management, and GitHub Actions for CI.

The tech list matters less than what the project taught me. Network performance isn't about throwing more CPU at a problem. Buffer management, memory allocation, scheduling, backpressure, and runtime behavior all change how a system performs.

- Zero-copy techniques and buffer reuse reduced allocation overhead.
- Bounded channels and coordinated read/write pauses prevented runaway memory growth.
- Tokio's runtime showed me the power of handling many connections concurrently, and also how a single blocking operation can stall an otherwise asynchronous system.

Benchmarking changed how I thought about optimization, too. Increasing batching could improve throughput while hurting latency. Optimizing one metric could damage another. Socket options like `TCP_NODELAY` and `SO_RCVBUF`, along with OS-level tuning, could shift real-world performance.

The important change was in the question I asked. I stopped asking:

> "Does my networking code work?"

and started asking:

> "What is happening to memory, scheduling, buffers, sockets, and the operating system while my networking code is running?"

That was a fundamentally different way of thinking, and I never fully went back.

---

## The Post-Quantum Cryptography Detour (and Why I Quit)

Another project pulled me into a very different corner of systems and security. I tried to build an encryption tool around post-quantum cryptography.

The motivation was straightforward. If sufficiently capable quantum computers become practical, algorithms like RSA and elliptic-curve cryptography face serious threats from Shor's algorithm. That makes post-quantum cryptography a fascinating area. I dug into NIST's work and experimented with Rust, WebAssembly, Kyber, Dilithium, and Tokio. I got as far as implementing encryption and signatures.

And then the project failed. I deliberately abandoned it.

The reason wasn't that the idea was impossible. I simply didn't have the mathematical and cryptographic foundation to keep building it responsibly. I had studied RSA, AES, SHA-256, Shor's algorithm, and some of the underlying math, but I could feel the gaps in my understanding, and I knew they were dangerous ones.

So I made a decision that became part of my engineering philosophy:

**I wasn't going to vibe-code cryptography I didn't understand.**

I left the project with the intention of returning once I had the foundation. That failure was not wasted effort. It taught me that some hard engineering problems require you to stop, rather than pretending that assembling code is the same as understanding a system.

---

## Going Deep Into Networking

Networking gradually became one of the pillars of my systems knowledge. I didn't try to become an academic expert in every topic. I focused on what would actually be useful for the kind of engineering I wanted to do.

I studied the OSI model, TCP/IP, UDP, WebSockets, ports, port forwarding, packet structure, headers and metadata, packet transfer, client-server architecture, DNS routing, DHCP, IPv4, IPv6, CIDR, subnetting, masking, load balancing, and network programming in C. One resource that helped enormously was _Computer Networking: A Top-Down Approach_.

But reading was never enough for me. I wanted to build.

So I built GreenLink, a WebSocket-based chat application with multi-tenancy, media support, and AI-powered semantic search. It became a practical playground where I could apply networking concepts and combine them with Docker and Kubernetes.

Linux became a big part of this. I didn't treat it as something to learn from documentation alone. I learned it through commands, experiments, small programs, and direct interaction with the operating system.

Some of the questions that nagged at me were basic but deep. For example:

**Why does a Docker container have its own IP address instead of just using a different port on the host?**

Questions like that forced me to understand network namespaces, virtual networking, routing, and isolation, the actual mechanisms beneath the container abstraction. That was exactly the kind of understanding I was hunting for.

---

## Going Deep Into Operating Systems

Networking pulled me naturally toward operating systems. I studied OS types, kernels, shells, terminals, hardware interaction, hypervisors, containers versus virtual machines, processes, threads, forking, child processes, zombie processes, starvation, concurrency, the dining philosophers problem, and the reader-writer problem.

Two major resources were _Operating Systems: Three Easy Pieces_ and Galvin's operating systems textbook.

Again, implementation mattered, so I built a shell in C. That single project made concepts like processes, system calls, command execution, and process creation stop being textbook abstractions. I understood how a shell relates to the operating system because I had built one.

Somewhere along the way, I noticed I could explain difficult OS and networking concepts to juniors without getting lost. That became my personal test for understanding. If I could learn something, implement it, explain it, and eventually write about it, I considered it truly understood.

---

## From Fundamentals to Systems Design

Once networking and operating systems felt comfortable, I moved into system design. I studied load balancers, scaling strategies, database replication, master-replica architectures, caching, rate limiting, CDNs, HTTP and HTTPS, authentication, authorization, sessions, cookies, storage, and distributed systems.

University coursework helped a lot here. I had already studied operating systems, networking, databases, distributed systems, and computer architecture, and those subjects gave me a foundation that made later material easier to connect. Computer Organization and Architecture gave me exposure to registers, processors, RAM, buses, ALUs, I/O, and assembly-level ideas. I'm not an expert in assembly or hardware, but I understood enough of the lower layers to connect the dots.

### The Books and Engineering Blogs

I didn't read every technical book cover to cover. I learned selectively.

_Designing Data-Intensive Applications_ was the one I spent the most time with. I also leaned on _Computer Systems: A Programmer's Perspective_, _Operating Systems: Three Easy Pieces_, _The Phoenix Project_, and _Grokking the System Design Interview_. The goal was never to collect book titles. It was to understand systems.

I also became a regular reader of engineering blogs from companies like Google, Netflix, Meta, Spotify, and Anthropic. Those posts showed me something university courses couldn't fully provide: how real engineering organizations think about large-scale systems. I read about things like Spinnaker, Temporal, large-scale traffic management, infrastructure automation, distributed architectures, and production engineering practices.

My reading method was essentially recursive. When I hit a concept I didn't understand, I stopped, researched it, and went down another level if that concept had its own unfamiliar dependency. Then I climbed back up to where I started.

### Documentation as an Engineering Habit

I kept detailed notes in Obsidian and Google Docs, with Obsidian becoming my main tool. I strongly believe that unstructured notes usually reflect unstructured thinking. Documentation became more than a record of what I'd learned. It became a way of organizing my mental model.

I documented system design concepts, networking, operating systems, distributed systems, experiments, project decisions, and technical discoveries. Eventually, I started enjoying the writing itself and began sharing publicly on Medium and LinkedIn. There was a practical side benefit as well: reading and writing kept me away from endless scrolling and helped me build patience and a longer attention span.

I wasn't learning because someone told me to. I genuinely enjoyed it.

---

## The Project Philosophy: Fewer, Deeper Projects

I never tried to build ten projects a month. I gradually came to believe that two or three serious projects teach more than ten shallow ones.

My projects became increasingly systems-oriented. I built a distributed write-ahead log, a key-value store, and a distributed chat application with AI integration. They gave me hands-on exposure to low-level programming and distributed systems.

Then I wanted something bigger. That became RavenMQ.

### RavenMQ: Where Systems Started Turning Into Infrastructure

RavenMQ began as an attempt to replicate some of the ideas behind BullMQ while going deeper into distributed task scheduling. It grew into an AI-powered distributed task scheduler that learned from historical execution patterns and dynamically reordered its queue.

The architecture was made up of independent services:

- A Next.js frontend
- A FastAPI API
- A Python and NumPy ML predictor
- A TypeScript scheduler
- A Python worker pool
- PostgreSQL

The scheduler used a priority queue, with PostgreSQL acting as the coordination layer. An LSTM predictor estimated job runtime and influenced task priority. Workers claimed jobs using PostgreSQL's `FOR UPDATE SKIP LOCKED`, so two workers never picked the same job. Lease-based recovery let jobs from failed workers be reclaimed. PostgreSQL advisory locks handled leader election. The workers were stateless, so more could be added without any internal worker state to manage.

This was the project where infrastructure concepts started showing up naturally inside the application architecture.

### Kubernetes Became More Than a Tool

I deployed RavenMQ on Kubernetes and used Horizontal Pod Autoscaling for the worker pool. Kubernetes could add worker pods when CPU utilization rose and remove them when it fell.

But the project also showed me where that model breaks down. CPU utilization isn't the same thing as workload demand. A task queue can have thousands of pending jobs while CPU stays low. In that case, queue depth is a much better scaling signal. That thought led me toward event-driven autoscaling and tools like KEDA.

The project exposed a long list of practical problems too:

- Polling latency
- Database connection pool exhaustion
- No queue sharding
- Retry storms
- No dead-letter queue
- Incomplete observability
- No distributed tracing
- General operational complexity

That was a crucial lesson. A project doesn't become production-like just because Kubernetes is involved. Putting something in a container is not the hard part. The hard part is answering one question:

**What happens when it fails?**

### RavenMQ Changed How I Thought About Production

RavenMQ also taught me that databases are far more powerful than people give them credit for. PostgreSQL was simultaneously the queue, the coordinator, the lock manager, and the event mechanism. Specialized distributed infrastructure isn't automatically better. Sometimes a simpler component you understand deeply is more operationally useful.

I also learned that consistency matters. For critical operations, duplicate execution can be far worse than a bit of extra latency. Transactions, row-level locks, and careful coordination provide guarantees that are hard to reproduce with loosely coordinated components.

And I learned a real lesson about observability. Metrics alone aren't enough. A system can expose queue depth and worker counts and still leave you unable to answer:

**Why did this particular job take ten seconds?**

Answering that takes tracing, correlation, and context.

My thinking moved from "Does it work?" to "Does it work when components fail?" and eventually to a scarier question: "Can I actually operate this thing?"

---

## The AI Side Never Disappeared

While I moved deeper into systems, I never abandoned AI. I wanted to stay AI-literate because I saw AI engineering becoming a core part of software engineering. I worked with RAG, LLMs, MCP, fine-tuning, embeddings, context, and tokenization.

In one project, I took an open-source model from Hugging Face, fine-tuned it, turned it into a RAG chatbot, containerized it, and deployed it on an Oracle VPS.

That combination fascinated me, because AI systems still need infrastructure. They need compute, networking, storage, deployment, observability, scaling, security, model serving, data pipelines, and reliability. AI infrastructure started to look like an interesting direction inside the broader infrastructure path.

---

## The Career Strategy Started Becoming Clear

At some point, I realized I didn't want my identity tied to a language or a framework. I don't want to be "the Python developer" or "the Java developer." I want to be an engineer who can use Python, Java, Rust, Go, or whatever fits the problem.

Languages are tools. Engineering reasoning is the transferable skill. I wanted to understand systems deeply enough that changing tools wouldn't reset my knowledge to zero.

### Why Docker and Kubernetes Entered the Picture

Fundamentals give you understanding. Tools give you the ability to actually interact with systems. I'd heard engineers constantly talking about Docker, Kubernetes, DevOps, and cloud technologies. There was some FOMO involved, I'll admit. But more importantly, I realized these tools are the practical interfaces through which infrastructure concepts are applied.

I didn't want to work only in isolated development environments. I wanted to understand what happens when software meets production, and to work around real systems where reliability actually matters. That pulled me toward infrastructure and SRE.

### Why SRE Specifically?

SRE appealed to me because it combines systems engineering, production engineering, reliability, and problem solving. The stakes were part of the attraction. A production system can suffer compute failures, storage failures, database problems, network failures, bad deployments, configuration mistakes, capacity problems, or unexpected load. Infrastructure sits underneath all of it, and I found that responsibility compelling.

At the same time, I knew I didn't yet know enough to operate large production systems. That gap would become painfully clear later.

---

## The Interview

In the company's initial interview process, I didn't just list technologies.

When I mentioned GreenLink and explained that I'd used Kafka for notifications and streaming, the interviewer asked about my experience with it. When I mentioned exploring cryptography, the interviewer asked for evidence. So I showed Crypten, a file-streaming encryption service I'd built with open-source cryptographic crates, using Argon2id and XChaCha20. Rather than saying "I know cryptography," I explained what I had built and walked through the underlying concepts and math.

I also talked about my AWS experience and systems engineering work. Then came the scenario question: what would I do if an entire application went down?

My answer back then was very different from how I'd answer today. I talked about taking the application offline, preserving data, using binlogs and S3, identifying bottlenecks, and using CloudWatch for monitoring.

It was an amateur answer, and that matters. I wasn't already an SRE. I was showing that I could reason about systems with the knowledge I had at that time.

## I Wasn't Hired as an SRE

I want to be clear about this part. The company hired me as a **Product Engineering Intern**. The expected stack was Java, Spring Boot, React, some Python, MySQL, GitHub and GitLab, Jira, HTML, and CSS. The normal expectation was to work on existing or upcoming products, depending on the team and pod.

I did not walk in with an SRE title. That came later, and it came because of a chain of small things.

---

## Getting Noticed Inside the Company

### The Database Details

One of the first things that caught my manager's attention was my database knowledge. I was comfortable with SQL details that many people gloss over. I could distinguish between `DROP`, `DELETE`, and `TRUNCATE`. I could reason about time-related SQL behavior. I could write and understand complex queries across schemas and databases. More importantly, I paid attention to small details.

My manager also noticed how much I read. I spent time on system design, engineering blogs, and technical documentation. I wasn't only interested in finishing assigned programming tasks. I wanted to understand the systems around them.

### The Hackathon

The company ran an internal hackathon for the product interns. There were roughly 40 interns, split into groups of four, and I became the group lead.

The problem was a key-management system for office workstations. When an employee joins, they need keys for workstation drawers, and the existing process involved the employee, administration, and security and facilities teams.

Instead of building only the happy path, we started with requirements gathering. We spent the whole first day understanding the problem before writing code. We identified edge cases:

- What happens when an employee leaves without returning a key?
- What happens when an employee changes workstations?
- What happens when a key is lost?
- How should fines work?
- What happens if someone tries to issue multiple drawers?
- How should the system handle empty workstations?

I designed the database schemas and data access layer, and took responsibility for architecture, documentation, business logic, and much of the presentation. We chose a Spring Boot microservices architecture and designed a CI/CD pipeline. I also kept our GitLab hygiene strict. Main stayed protected, a development branch served as the integration branch, and teammates created feature branches with a consistent naming convention.

The idea was simple:

**Keep main clean. Integrate through development. Resolve issues before they reach production.**

We built automated emails for request confirmations, approvals, and facilities team notifications. We added single sign-on and a floor-grid view. The facilities team responded positively, and the judges kept throwing in new edge cases during evaluation. Because we had already reasoned through the problem, we could answer calmly instead of panicking. The project was eventually considered for actual company use.

### The Infrastructure Director

The most important moment came during a session the Director of Infrastructure ran for the interns.

I already knew some of the infrastructure concepts being discussed, because I had read internal documentation beforehand. I understood things like database schemas, naming conventions, environments, infrastructure structure, Kubernetes, and the cloud vendors the company used.

The director noticed. He remembered me after the session and later approached me separately. There was even a personal coincidence: he turned out to be from my hometown.

That interaction made the infrastructure path feel realistic. Meanwhile, I had been pushing my own manager to consider moving me toward infrastructure. He initially pushed back. His reasoning was fair: I was good at programming, so maybe I should start in product engineering and move into DevOps or infrastructure later.

The final decision wasn't mine to make. But I had created enough evidence that infrastructure leadership knew exactly who I was.

---

## The Screen

Then came allocation day.

Forty interns in one room. Managers calling names. Eventually, the infrastructure allocation came up on the screen, and I saw my name. Only one intern was listed.

Me.

That was the moment everything became real. All the time spent with Linux, networking, Docker, Kubernetes, Rust, Go, distributed systems, system design, cloud, observability, and infrastructure projects had turned into something tangible. I wasn't just learning infrastructure anymore. I had been selected to work in it.

## Walking Onto the Infrastructure Floor

The next day, I was waiting with the other interns when the VP of SaaS Infrastructure contacted me and asked me to message another person. After waiting an hour or more, infrastructure leadership came to pick me up and introduced me to the engineering team.

The first impression was surprisingly warm. The engineers were deeply focused on their monitors, but they looked up and greeted me. I was given a temporary workstation and introduced around. The director who had spoken to the interns remembered me, shook my hand, and said:

**"You finally made it."**

It was a small sentence, but it carried the weight of the whole transition.

---

## My First Days in Infrastructure

### Learning Before Touching Anything

My first few days weren't about touching production. They were about learning. I went through internal documentation, studied the company's infrastructure, and started picking up the tools and concepts relevant to the environment.

One of the first practical technologies I learned was Boto3. I already knew Python, but now Python became a way to automate infrastructure tasks through AWS APIs. That was an important shift. Programming was no longer the product. It had become an instrument for operating systems.

### My First Automation

My first task was simple. There were logs stored in a file. I had to read them, classify them, and generate another file from the result.

Technically, it was basic Python file handling. I used what I knew and checked the documentation when needed. What mattered wasn't the complexity of the script. It was that a repetitive manual process could now be eliminated. It was an early lesson in infrastructure engineering:

Sometimes the best engineering work isn't building something impressive. Sometimes it's noticing that humans are repeatedly doing something a machine could do reliably.

### The Sandbox

After the initial learning period, I got a fresh server as a sandbox. I configured it myself, created my own SSH keys, connected using Ubuntu, and started experimenting. I also received controlled AWS credits that let me explore services like EC2, S3, DynamoDB, and Aurora, with restricted IAM permissions.

The environment was deliberately sandboxed, but it followed the company's conventions and policies. That distinction mattered. It was close enough to real infrastructure to teach me operational discipline, without handing an intern unrestricted access to production.

### Breaking Things on Purpose

With the sandbox, I started breaking things deliberately. I created Docker containers and scaled them. I used `kubectl`, created pods, crashed them, and then fixed them. I experimented with scripts that automatically recreated Kubernetes pods when containers failed, and I explored scaling behavior based on workload.

It was one of the safest ways to learn reliability. Failure isn't something you should only read about. In a controlled environment, you should cause it, observe what happens, recover, and document it.

### Production Is Not a Playground

One of the biggest differences I discovered was access control. In a sandbox, you can experiment. In production, you earn access.

Database access is restricted. Different permissions exist for different operations. Read access comes before write access. Sensitive access may require explicit requests and justification. Production, UAT, development, sandbox, and other internal environments each have their own role and protections. There are backups, failover mechanisms, migration processes, security policies, and multiple layers of control.

At first, this felt restrictive. Then I understood why. The point isn't to stop engineers from doing their jobs. The point is to reduce the blast radius of human mistakes. The goal isn't to assume engineers are incompetent. It's to assume humans make mistakes and design systems so that one mistake doesn't automatically become a catastrophe.

---

## The War Room

The moment that really humbled me came during one of my first knowledge-transfer sessions.

I was sitting with several engineers who each had around a decade of experience. They were whiteboarding infrastructure concepts and explaining how systems behave. What struck me wasn't that they knew more technologies. It was how they reasoned.

There was a flow to their thinking. They moved from symptoms to evidence, from evidence to possible causes, from causes to validation, and from validation to remediation, without ever losing the thread.

I remember thinking:

**"Do I even deserve to be here?"**

I was 21. I was an intern. They had years of production experience. I knew Docker. I knew Kubernetes. I knew Linux. I knew how to build systems. And suddenly I understood the difference between **knowing a technology and operating a system**.

Then another thought followed: I had done something to earn my place in that room. That's why I was there.

---

## Lessons That Changed How I Think

### The Biggest Myth About SRE I Had to Unlearn

Before joining, I assumed infrastructure and SRE would mean a lot of building: setting up servers, configuring Kubernetes clusters, deploying systems, designing new things constantly.

The reality was different. SRE is fundamentally about reliability. The question isn't just:

**"Can we build this?"**

It's:

**"Can this keep working when the system is under stress, when dependencies fail, when traffic spikes, when something changes, and when humans make mistakes?"**

A system that works for one user but collapses under a million isn't reliable. A system that works until one dependency fails isn't reliable. A system that looks healthy on a dashboard while users are hitting failures isn't healthy. And a huge portion of the work is maintaining what already exists, which was one of my biggest misconceptions.

### The Real SRE Skill Isn't Kubernetes

A person can say "I know Kubernetes." But what does that mean? Running three pods on a laptop is not the same as operating a production environment with hundreds of pods. Using `kubectl` is not the same as understanding the consequences of a production change. Knowing Docker is not the same as understanding container networking, isolation, resource limits, and failure modes. Knowing Prometheus is not the same as knowing which signals actually matter.

The real skill is reasoning. When something breaks, you ask:

- What changed, and when?
- What was the system doing before?
- What evidence do we have?
- What pattern does the failure follow?
- What is the blast radius?
- What should we touch, and what should we absolutely not touch?
- How do we recover?
- How do we prevent it from happening again?

That's SRE thinking.

### Root Cause Analysis Changed My Perspective

Infrastructure engineers don't approach an incident by panicking that something broke. They approach it analytically. Something happened. Now determine what happened, when, what changed, what the previous healthy state looked like, which dependency is involved, what the root cause is, and what the safest remediation is.

The objective isn't merely to restart something and move on. It's to understand why it failed and reduce the probability or impact of recurrence.

### Observability Isn't Just Monitoring

Before SRE, I knew Prometheus and Grafana. After entering infrastructure, I began to see observability differently. Monitoring tells you that something may be wrong. Observability helps you reason about what is happening inside the system.

Pattern recognition matters too. If the same incident keeps occurring, the pattern itself is evidence. And even when dashboards look healthy, users can still be having problems. A system can be technically "up" while being functionally degraded. That's why metrics, logs, traces, alerts, and contextual signals all matter.

### Communication Is Also an Infrastructure Skill

Infrastructure engineering isn't purely technical. The team communicates in concise categories such as decisions, challenges, and information. Messages have a purpose beyond status updates. They should make clear what happened, what matters, and what needs to happen next. This is especially important during incidents. A technically brilliant engineer who communicates poorly can make an incident worse.

### What Experienced Engineers Taught Me

The biggest lesson from senior engineers wasn't a technology. It was their attitude toward failure. Problems are normal in infrastructure. In fact, problems are part of why an infrastructure team exists. The response isn't panic. It's investigation.

The engineers I watched didn't jump to assuming they'd caused something or that the situation was catastrophic. They looked at evidence, reasoned, and took their time when necessary. They weren't afraid to say they didn't immediately know something. There was very little ego involved. That changed my idea of what a senior engineer looks like.

### I Became Less Confident (In a Good Way)

Ironically, entering SRE didn't make me more confident. It made me less confident, productively.

Before joining, I was a bit cocky. I knew Docker. I knew Kubernetes. I could write Linux commands, Python, and Bash scripts. I had built systems. I thought I knew infrastructure.

After joining, I realized how little I actually knew. And that realization was valuable. There is a huge difference between:

**knowing about Kubernetes** and **operating Kubernetes in production**.

Between **building a distributed system** and **keeping a distributed system reliable when people depend on it**.

Between **learning infrastructure** and **being responsible for infrastructure**.

### Being the Youngest Person in the Room

At 21, I'm currently the youngest person on the team. Some of my colleagues are in their late twenties or thirties with years of experience, and some have young kids. Yet the relationship isn't hierarchical in the way I expected. My manager is older than my father, but we talk casually. The team doesn't treat me like an outsider.

Internally, though, I sometimes still feel like one. That's the strange contradiction. Externally, I'm already part of the infrastructure team. Internally, I'm constantly aware of how much I have left to learn. Both things can be true at the same time.

### From "Does It Work?" to "Can I Operate It?"

My earlier projects taught me how to build. RavenMQ taught me how to think about failure. SRE is teaching me how to operate. The progression looks like this:

1. Does it work?
2. Does it still work when something fails?
3. Can I detect the failure?
4. Can I recover safely?
5. Can I prevent or reduce the impact of the same failure next time?

That progression may be the most important technical lesson of this entire journey.

---

## What I Would Do Differently

**I'd chase fewer trends.** Following trends isn't inherently bad, and engineers need to know where the industry is heading. AI adoption, for example, isn't a passing fad. It's a broad shift in how software gets built. But there is a difference between a fundamental shift and framework churn. A new JavaScript framework every few months doesn't deserve the same attention as distributed systems, databases, networking, operating systems, or AI infrastructure.

**I'd learn fundamentals before tools.** Many students learn tools before understanding what those tools abstract. You can learn Docker without understanding Linux processes and namespaces. You can learn Kubernetes without understanding containers. You can learn networking tools without understanding packets, ports, addressing, routing, and protocols. You can learn database tooling without understanding transactions, indexing, consistency, and storage. That creates shallow competence. Tools become much easier once the foundations are in place.

**I'd do more DSA, earlier.** Even though my path moved toward infrastructure, I don't think DSA should be ignored. Queues exist in message systems. Heaps power scheduling. Sorting is everywhere. Graphs appear in networking and dependency relationships. Hashing shows up in storage, caches, routing, and distributed systems. More importantly, algorithmic thinking trains you to reason about problems. A disciplined habit of solving a few problems consistently and documenting the reasoning would have paid off.

---

## Advice for Students Trying to Find Their Path

### Don't Try to Learn Everything at Once

Exploration is valuable, especially early in college. Try web development. Try Python. Try ML. Try systems, security, cloud, AI, databases. Discover what pulls you in. But eventually, choose a domain and go deeper.

That doesn't mean choosing one job title forever. SRE, DevOps, infrastructure, platform engineering, cloud engineering, and systems engineering overlap a lot. The important decision is the **domain**, not necessarily the exact title.

### Build Fewer, Better Projects

You don't need ten projects a year. Two or three serious ones are enough if they force you to face difficult engineering problems. A good project gives you something to explain:

- Why this architecture?
- Why this database?
- Why this communication mechanism?
- What happens when the service fails?
- How does it scale, and where are the bottlenecks?
- What assumptions did you make?
- What would you change at 10x scale?
- What broke, and what did you learn?

Those questions matter far more than a long list of repositories.

### Share What You Build

Building privately is useful. Documenting and sharing creates another layer of learning. I started writing on Medium and LinkedIn because writing forces clarity. If you can't explain something, you probably don't understand it deeply enough yet.

Publishing also creates a public trail of how you think. You don't need to become an influencer or show your face. A technical blog, a GitHub repository, a LinkedIn post, an X thread, a Reddit write-up, or even a personal knowledge base can demonstrate your reasoning.

### The Learning Method That Worked for Me

My learning was roughly 60% reading and 40% building. I read because it isn't realistic to implement every distributed systems concept from scratch on a laptop, and I built whenever I could turn an idea into something concrete. I also used AI extensively as a learning assistant, especially when I needed a concept explained differently or wanted to explore implementation paths. But I kept my own notes and my own reasoning. AI helped me understand concepts. It wasn't meant to replace understanding, which mattered most in hard areas like cryptography and distributed systems.

My loop eventually looked like this:

**Learn → question → investigate → implement → break → fix → document → explain → move deeper.**

If I hit something I didn't understand, I backtracked. If something interested me, I researched it. If I could build it, I built it. If it broke, I investigated why. If I understood it well enough, I wrote it down. That loop slowly moved me from application development toward systems engineering.

---

## The Larger Lesson: Explore, Discover, Commit, Fight

This story isn't really about becoming an SRE. SRE is simply where the trajectory led. The bigger lesson is that career paths aren't always visible at the beginning.

I started with HTML. Then JavaScript. Then Python. Then React. Then Node. Then machine learning, deep learning, LLMs. Then Rust. Then networking, operating systems, distributed systems. Then Docker, Kubernetes, cloud, and infrastructure. None of those decisions required me to know where I'd end up. I explored first, found something I genuinely cared about, and then committed.

That's how I think about it now, in four stages:

**Explore.** At 18 or 19, you may not know what you want. That's normal. Try things.

**Discover.** Eventually, something will hold your attention longer than the rest. Pay attention to that.

**Commit.** Once you find something worth pursuing, go deep. Don't keep jumping because another technology is trending.

**Fight.** When an opportunity appears, don't wait for someone to hand it to you. Ask. Push. Show your work. Make your case. Take the opportunity.

I pushed my manager toward infrastructure. I demonstrated my systems knowledge in the interview. I built projects that gave me evidence to show. I talked with the infrastructure director. I read internal documentation before I was part of the infrastructure team. Each individual action was small. Together, they changed my trajectory.

---

## Where I Am Now, and Where This Could Go

Right now, I'm an SRE in the Infrastructure Management System division. I'm still an intern, and my access is intentionally restricted. I mostly work in sandbox environments, write automation scripts, study internal documentation, learn the organization's systems, study system design, and keep pursuing certifications and technical learning.

I'm not pretending I'm responsible for the entire production infrastructure. I'm not. I'm learning how it works, and that is exactly where I should be.

I want to stay in this domain. The exact specialization isn't fixed. AI infrastructure is especially interesting because it combines two areas I've spent a lot of time in: AI engineering and systems infrastructure. But the broader direction is settled: systems, infrastructure, reliability, distributed computing, cloud, and production engineering. The tools will change. The underlying problems will keep coming back in new forms.

---

## The Real Meaning of "At 21"

Being 21 isn't what makes this story interesting. What's interesting is that I'm 21 and already sitting in an environment where the consequences of infrastructure decisions are real. But I'm also 21 and still learning the basics of what production infrastructure demands.

I don't want this post to say:

**"Look at me. I'm 21 and I know SRE."**

I want it to say:

**"I'm 21, I got into SRE, and the first thing I discovered was how much I still don't know."**

That is much closer to the truth.

---

## The Ending

Let me return to the war room.

Nine days into the job, I'm sitting among engineers with years of production experience. I still don't know everything. In fact, I now understand much more clearly how little I know. But I also know why I'm there.

I didn't get there because of some special advantage. I started where almost everyone starts. I explored. I got distracted. I chased things that turned out not to matter. I abandoned projects. I built things that broke. I learned fundamentals, then built deeper projects. I documented everything. I kept pushing toward the problems that genuinely interested me.

And when the opportunity appeared, I took it.

The infrastructure tab showed one name.

Mine.

That was when I realized that sometimes the career you're looking for isn't something you find fully formed. Sometimes you build your way toward it. And sometimes, when the opportunity finally appears, you have to be willing to raise your hand and say:

**"That's me."**

---
