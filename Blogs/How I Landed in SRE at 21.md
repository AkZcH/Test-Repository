- [x] Posted on Portfolio

There's a moment I keep coming back to.

About 40 interns sat in a meeting room, waiting for team allocations. A big screen at the front showed the assignments, and managers called names one by one. I had joined as a Product Engineering Intern, so I expected to land somewhere in the Java, Spring Boot, React, and MySQL world.

Then I clicked over to the infrastructure tab and saw my name. It was the only name there.

I didn't feel like I had beaten everyone. It felt more like years of quiet, scattered, sometimes pointless-looking effort had finally added up to something. Linux, networking, Rust, Docker, Kubernetes, distributed systems: all of it suddenly had a destination.

But the story doesn't start there. It starts with a very ordinary CS student who had no idea SRE existed.

## I Didn't Set Out to Become an SRE

My first semester was HTML, CSS, and JavaScript. Second semester, Python. Third semester, React, Node, and Express. I was walking the standard path: frontend, backend, APIs, databases.

Then I found machine learning. Python made the jump easy, and I moved from basic models to deep learning, then to RAG, LLMs, embeddings, tokenization, and attention. I learned a lot.

But something kept nagging at me. I was getting more curious about what sat underneath the applications I was building than about the applications themselves.

## The Rust Detour That Changed Everything

A friend was working in Rust, and I got curious. Rust let me program close to the machine while still giving me modern abstractions. It was the first time I could build things that touched the operating system and the network directly.

There was also a less noble motivation: I wanted a differentiator. Lots of students were following the same path of web, Python, ML, and Spring Boot. Nothing wrong with that path, but I wanted to understand the bottom layer instead of only consuming the abstractions on top of it. People also kept saying Rust was hard to learn, which made it more appealing, not less.

My first serious systems project was TurboNet, an async networking library and client-server framework in Rust, built on Tokio. It taught me that network performance isn't about throwing more CPU at a problem. Buffer reuse, zero-copy techniques, backpressure, and scheduling all matter. Batching can improve throughput while hurting latency. Socket options like `TCP_NODELAY` and `SO_RCVBUF` change real behavior.

My question shifted from "does my networking code work?" to "what is happening to memory, buffers, sockets, and the OS while it runs?" That shift is the real starting point of this whole story.

## The Project I Deliberately Abandoned

Around then I tried to build an encryption tool around post-quantum cryptography, using Kyber, Dilithium, Rust, and WebAssembly. I got as far as working encryption and signatures.

Then I stopped, on purpose.

I knew about RSA, AES, SHA-256, and Shor's algorithm, but I didn't have the mathematical foundation to keep building responsibly. So I made a rule for myself: **I'm not going to vibe-code cryptography I don't understand.**

That project "failed," but it taught me something I still use. Sometimes the right engineering move is to stop, rather than pretend that assembling code is the same as understanding a system.

## Going Down the Stack

Networking came next. I worked through the OSI model, TCP/IP, UDP, DNS, DHCP, CIDR, subnetting, load balancing, and network programming in C, with _Computer Networking: A Top-Down Approach_ as my main guide. Reading wasn't enough, so I built GreenLink, a WebSocket chat app with multi-tenancy, media, and AI-powered semantic search.

The questions that pulled me in were the basic ones. Why does a Docker container get its own IP instead of just using a different port on the host? Chasing that led me into network namespaces, virtual networking, and routing. That's the kind of understanding I was after.

Networking pushed me into operating systems: processes, threads, forking, zombies, concurrency, containers versus VMs. I used _Operating Systems: Three Easy Pieces_ and Galvin, and I built a shell in C, which turned processes and system calls from textbook vocabulary into things I had actually wrestled with.

That became my personal test for understanding something: can I learn it, build it, explain it, and write about it? If yes, I probably get it.

## How I Learned

I didn't read every book cover to cover. I read selectively: _Designing Data-Intensive Applications_, _Computer Systems: A Programmer's Perspective_, _The Phoenix Project_, and a lot of system design material. I also read engineering blogs from Google, Netflix, Meta, Spotify, and others, which taught me how real organizations think about large systems, something coursework can't fully cover.

My method was recursive. If I hit a concept I didn't know, I stopped, went down a level, learned it, and came back up.

I kept detailed notes in Obsidian, because unstructured notes usually mean unstructured thinking. Eventually I started writing publicly on Medium and LinkedIn. Writing forces clarity, and it also kept me off endless scrolling.

Roughly, my split was 60% reading and 40% building. I used AI heavily as a learning assistant, but never as a replacement for understanding.

## Fewer, Deeper Projects

I never believed in building ten projects a month. Two or three serious ones teach more than ten shallow ones. Mine got progressively more systems-flavored: a distributed write-ahead log, a key-value store, a distributed chat app with AI.

Then came RavenMQ, the project that changed how I think.

RavenMQ was an AI-powered distributed task scheduler. An LSTM predicted job runtimes and influenced queue priority. Workers claimed jobs with PostgreSQL's `FOR UPDATE SKIP LOCKED`, leases let the system recover jobs from dead workers, and advisory locks handled leader election. The whole thing ran on Kubernetes with a Horizontal Pod Autoscaler on the worker pool.

The most valuable part was discovering where it fell short. CPU utilization isn't the same as demand: a queue can hold thousands of pending jobs while CPU sits idle, which is a case for scaling on queue depth with something like KEDA. I also ran into polling latency, connection pool exhaustion, retry storms, no dead-letter queue, and no distributed tracing.

A project doesn't become production-grade just because Kubernetes is involved. Putting something in a container is easy. The hard question is: **what happens when it fails?**

RavenMQ moved my thinking from "does it work?" to "does it work when components fail?" and eventually to "can I actually operate this thing?"

## Why SRE

I never dropped AI. I fine-tuned an open-source model, wrapped it in a RAG chatbot, containerized it, and deployed it on a VPS. That taught me that AI systems still need compute, networking, storage, observability, and reliability. It also makes AI infrastructure a direction I find genuinely interesting.

I also decided early that I didn't want my identity tied to a language or framework. I don't want to be "the Python guy" or "the Java guy." Languages are tools. Reasoning about systems is the transferable skill.

Docker and Kubernetes entered the picture because fundamentals give you understanding, but tools give you the ability to touch real systems. And SRE pulled me in because of the stakes. Production systems fail through compute, storage, networking, bad deploys, config mistakes, and unexpected load, and infrastructure sits underneath all of it. I found that responsibility fascinating, even though I didn't yet know how little I knew about carrying it.

## The Interview

In the interview, I didn't just list technologies. When I mentioned GreenLink and Kafka, the interviewer probed. When I mentioned cryptography, they asked for proof, so I showed Crypten, a file-streaming encryption service I'd built with Argon2id and XChaCha20, and walked through the concepts behind it.

Then came a scenario question: what would you do if the entire application went down? My answer involved taking the app offline, preserving data with binlogs and S3, hunting for bottlenecks, and watching CloudWatch.

Looking back, it was an amateur answer. And that's the point. I wasn't an SRE yet. I was showing that I could reason about systems with the knowledge I had.

## Getting Noticed

I was hired as a Product Engineering Intern, not an SRE. The path to infrastructure was not handed to me.

First, my manager noticed my database fundamentals: the difference between `DROP`, `DELETE`, and `TRUNCATE`, time-related SQL quirks, complex cross-schema queries. He also noticed that I read constantly and cared about the systems around my tasks.

Then came an internal hackathon. About 40 interns in teams of four, and I led ours. The problem was a key-management system for office workstation drawers. Instead of coding the happy path, we spent day one on requirements and edge cases. What if someone leaves without returning a key? Changes desks? Loses a key? Gets multiple drawers issued?

We built it as Spring Boot microservices with a CI/CD pipeline. I owned the schemas, data layer, architecture, and documentation, and kept GitLab clean: protected `main`, a `development` integration branch, and consistently named feature branches. Keep main clean, integrate through development, resolve issues before they reach production. The judges kept throwing new edge cases at us, and because we'd done the thinking upfront, we could reason through them instead of panicking. The project was considered for real company use.

The other turning point was a session with the Director of Infrastructure. I had already read internal documentation, so I understood the schemas, naming conventions, environments, Kubernetes setup, and cloud vendors well enough to follow along. He noticed, remembered me, and later approached me separately. (Coincidentally, he's from my hometown.)

Meanwhile I had been pushing my own manager to consider me for infrastructure. He pushed back, reasonably: I was good at programming, so why not start in product engineering and move over later? The final call wasn't mine. But I had created enough evidence that the right people knew who I was.

Then came the screen, and my name.

## Walking Onto the Infrastructure Floor

The next day, after about an hour of waiting, the infrastructure leadership came to get me. The engineers were deep in their monitors, but they looked up and greeted me. The director shook my hand and said, "You finally made it."

That one sentence carried the whole transition.

## The First Days

The first few days weren't about production. They were about learning: internal docs, the company's infrastructure, and new tools. One of the first was Boto3, which turned my Python skills into a way to automate infrastructure through AWS APIs. Programming stopped being the product and became an instrument for operating systems.

My first task was small: read logs from a file, classify them, and generate an output file. Basic Python. But it showed me something about infrastructure work: sometimes the best contribution isn't building something impressive. It's noticing that humans keep doing something a machine could do reliably.

Then I got a sandbox: a fresh server I configured myself, with my own SSH keys, plus controlled AWS credits and restricted IAM permissions for EC2, S3, DynamoDB, and Aurora. It followed company conventions but kept me far from production.

So I started breaking things on purpose. I created containers, scaled them, made pods with `kubectl`, crashed them, and fixed them. I wrote scripts to recreate pods when containers failed and played with scaling behavior. You shouldn't only read about failure. In a safe environment, cause it, observe it, recover, and document it.

I also learned the other side of that: production is not a playground. You earn access. Read comes before write, sensitive access needs justification, and environments like production, UAT, dev, and sandbox exist for different purposes. Backups, failover, migration processes, and layers of approval all exist for one reason: to shrink the blast radius of human mistakes. The point isn't to distrust engineers. It's to assume everyone makes mistakes and design so one mistake doesn't become a catastrophe.

## The Room That Humbled Me

In one of my first knowledge-transfer sessions, I sat with engineers who had around a decade of experience each, whiteboarding how systems behave.

What struck me wasn't that they knew more tools. It was how they reasoned. They moved from symptoms to evidence, from evidence to hypotheses, from hypotheses to validation, and from validation to remediation, without losing the thread.

I remember thinking: _do I even deserve to be here?_ I was 21 and an intern. I knew Docker, Kubernetes, and Linux. I had built systems. And suddenly I understood the gap between **knowing a technology and operating a system**.

Then a second thought followed: I had earned this seat. That's why I was there.

## What I Had Wrong About SRE

I expected SRE to be mostly building: standing up clusters, deploying new systems, designing constantly. The reality is different. SRE is about reliability. The question isn't "can we build this?" It's "will this keep working under stress, when dependencies fail, when traffic spikes, when something changes, and when humans slip?"

A system that works for one user but collapses at a million isn't reliable. A system that looks green on a dashboard while users suffer isn't healthy. And a huge share of the job is maintaining what already exists.

The real skill also isn't Kubernetes. Running three pods on a laptop is not operating hundreds in production. Knowing Prometheus isn't the same as knowing which signals matter. The real skill is reasoning. When something breaks:

- What changed, and when?
- What was the system doing before?
- What evidence do we have?
- What's the blast radius?
- What should we touch, and what should we absolutely not touch?
- How do we recover, and how do we stop it recurring?

Incidents aren't met with panic. They're met with investigation and root cause analysis. Restarting something and moving on is not the goal. Understanding why it failed is.

This also changed how I see observability. Monitoring tells you something may be wrong. Observability helps you reason about what's happening inside. A system can be "up" and still functionally degraded, which is why metrics, logs, traces, and context all matter. And communication is part of the job: clear, purposeful messages about what happened, what matters, and what's next. A brilliant engineer who communicates badly can make an incident worse.

## Less Confident, in a Good Way

Before joining, I was a little cocky. I knew Docker, Kubernetes, Linux, Python, and Bash, and I'd built systems. I thought I knew infrastructure.

Joining made me less confident, and that was productive. There's a huge difference between knowing about Kubernetes and operating it in production. Between building a distributed system and keeping one reliable while people depend on it. Between learning infrastructure and being responsible for it.

The senior engineers taught me the most through their attitude, not their tools. Problems are normal; they're why the team exists. They looked at evidence, took their time, and had no ego about saying "I don't know yet."

I'm now the youngest person on the team, working with people in their late twenties and thirties, and a manager older than my father. Yet we talk casually. Externally I'm part of the team. Internally I still feel like the newcomer. Both are true.

## What I'd Tell Students

**Explore, then commit.** At 18 or 19 you don't know what you want. Try web, Python, ML, systems, security, cloud, AI. When something holds your attention longer than the rest, notice it and go deep. The domain matters more than the exact title, since SRE, DevOps, platform, cloud, and systems engineering overlap heavily.

**Fundamentals before tools.** Learn Linux processes and namespaces before Docker. Learn containers before Kubernetes. Learn packets, ports, and routing before networking tools. Otherwise you get shallow competence.

**Build fewer, better projects.** A good project gives you things to explain: why this architecture, why this database, what happens when a service dies, where are the bottlenecks, what would you change at 10x scale, what broke, what did you learn. Those answers beat a long repo list.

**Share your work.** Writing forces clarity. You don't need to be an influencer. A blog, a GitHub repo, or a public knowledge base shows how you think.

**Don't skip DSA.** Queues, heaps, graphs, and hashing show up all over infrastructure, and algorithmic thinking trains you to reason. I wish I had practiced consistently earlier.

**Don't chase framework churn.** Pay attention to real shifts like AI, but a new JavaScript framework every few months doesn't deserve the same attention as databases, networking, or operating systems.

**Then fight for the opportunity.** I pushed my manager. I showed evidence in the interview. I read internal docs before I belonged to the team. I spoke up around the director. Each action was small. Together they changed my trajectory.

## Where I Am Now

I want to be clear about this: I'm an SRE intern in the Infrastructure Management System division, and my access is intentionally restricted. I work mostly in sandboxes, write automation scripts, study internal documentation and system design, and keep working through certifications. I am not responsible for the company's production infrastructure. I'm learning how it works, which is exactly where I should be.

Where this goes next isn't fixed. AI infrastructure appeals to me because it sits where two things I've invested in meet: AI engineering and systems. But the broader direction is settled: systems, infrastructure, reliability, distributed computing, production engineering. The tools will change. The underlying problems will keep coming back in new forms.

## Back to the Room

I'm sitting among engineers with years of production experience. I don't know everything. I understand far better now how much I don't know.

But I also know why I'm here. I had no special advantage. I started where almost everyone starts. I explored, got distracted, chased things that didn't matter, abandoned projects, and built things that broke. I learned fundamentals, built deeper, wrote everything down, and kept moving toward the problems that pulled at me.

When the opportunity showed up, I took it.

The infrastructure tab had one name on it. Mine.

The career you're looking for often doesn't show up fully formed. You build your way toward it. And when the moment finally arrives, you have to be willing to raise your hand and say: **"That's me."**

---

I can put this into a Markdown file if you want to publish it on Medium or your site, or tighten it into a shorter LinkedIn version.