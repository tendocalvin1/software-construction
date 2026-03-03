# software-construction
Here’s the **straight-up, no BS engineering answer** to both questions.

---

## **1) How Netflix Uses Microservices in Its Operations**

Netflix didn’t adopt microservices because it was cool — it *had to* if it wanted to stay alive as a global streaming business. They run **thousands of independent, small services**, each responsible for a specific domain. That’s the real definition of microservices: decomposing a huge platform into *loosely coupled, independently deployable units*. ([Design Gurus][1])

Here are the core ways Netflix uses microservices:

**🔹 Independent Functionality per Service**
Netflix splits major components — like **user accounts, billing, recommendations, playback/streaming logic, content catalog, and analytics** — into separate services. Each one can be built, deployed, scaled, or fixed without touching the rest of the system. ([Design Gurus][1])

**🔹 Scalability Where It Matters**
Different parts of Netflix have wildly different load patterns. For example:

* *Playback* is extremely heavy during peak hours
* *Recommendations* may be CPU-intensive but not high bandwidth

Microservices lets Netflix scale just the parts under heavy demand instead of the entire system at huge cost. ([Design Gurus][1])

**🔹 Fault Isolation and Resilience**
If the recommendation service fails, users can still **search and play videos**. Netflix uses patterns like **circuit breakers** (e.g., Hystrix-like tools) and Chaos Engineering to ensure faults in one service don’t cascade across the platform. ([Howik][2])

**🔹 Independent Deployment Velocity**
Engineering teams own their microservices end-to-end. This autonomy means:

* They can deploy updates without packaging the whole platform
* There's *no massive integration bottleneck*
* They can deploy hourly if needed

Different teams don’t step on each other’s toes — and Netflix deploys *thousands of times* per day because of this. ([Design Gurus][1])

**🔹 Cloud First and Distributed**
Behind the scenes, Netflix runs on AWS with tools for service discovery (Eureka), API routing (Zuul/Envoy), orchestration (Conductor), observability, and fault tolerance — all glue that makes microservices manageable at massive scale. ([TechAhead][3])

**In one line:** *Netflix uses microservices to scale globally, isolate failures, and release stuff without downtime — and they do it with hundreds to thousands of independent services talking to each other.* ([Design Gurus][1])

---

## **2) Two Companies That Used Microservices and Later Returned to a Monolith (and Why)**

This trend isn’t widely advertised in tech blogs, but it’s very real in the engineering trenches — *microservices are not a silver bullet*. Several notable cases have pivoted back from microservices to monoliths or more consolidated architectures.

Here are two concrete examples backed by industry studies and case analyses:

---

### **📌 Amazon Prime Video Monitoring Service**

Amazon Prime Video *actually rolled back* a portion of its distributed microservices (specifically their **monitoring service**) into a monolith.

**Why they changed back:**

* **Cost explosions:** The microservices plus serverless orchestration (Step Functions + Lambda + S3) generated huge cloud bills. When they consolidated back into a monolith, they reported **~90% lower infrastructure costs**.
* **Scalability complexity:** The distributed version struggled to scale effectively for very high throughput.
* **Performance:** Inter-service communication and orchestration added significant latency overhead. ([Medium][4])

**Lesson:** if the distributed overhead (cost + latency + orchestration) outweighs the benefits, a simpler monolith can be cheaper and faster. ([Medium][4])

---

### **📌 InVision**

InVision originally migrated from a monolith to microservices as they grew. But over time, they *undid a lot of the microservices split* and merged services back into a monolithic codebase.

**Why they changed back:**

* **Operational complexity:** Dozens of microservices meant many repos, languages, build pipelines, monitoring systems — overhead that outstripped the benefit for their team size and use case.
* **Cognitive and coordination load:** Smaller teams had more systems to maintain, which *slowed them down*, not sped them up — the exact opposite of one core microservices promise.
* **People + tech misalignment:** Microservices started as a solution to collaboration problems but became another source of friction. ([bennadel.com][5])

**Lesson:** misalignment between architecture and team structure kills velocity. Simple beats distributed when you don’t actually need scale. ([bennadel.com][5])

---

### **(Additional pattern — not asked but worth knowing)**

**Istio control plane** (open-source service mesh) *also* moved many of its distributed control plane components into a **monolithic control plane binary**, mainly because:

* The microservices variants were too complex,
* Hard to debug/manage,
* Didn’t deliver enough operational value compared to the overhead. ([IEEE Computer Society][6])

This reinforces that microservices always trade **infrastructure complexity + operational cost** for **modularity + scale** — and sometimes the cost isn’t worth it. ([IEEE Computer Society][6])

---

## **Brutal Reality Check**

* **Netflix pulled off microservices because they *had to* for scale and invested heavily in tooling.** Most companies do not need that level of complexity. ([Design Gurus][1])
* **Microservices are not “always better.”** They introduce distributed systems issues (latency, debugging pain, operational cost, team coordination headaches). ([scisimple.com][7])
* Reverting **doesn’t make you a failure — it means you overloaded the engineering cost bucket compared to business value.** ([svitla.com][8])

---

If you want, I can break this down into architectural decision frameworks or how to decide when *you* should choose microservices vs monolith as an engineer/CTO — brutally practical, not academic.

[1]: https://www.designgurus.io/answers/detail/does-netflix-use-microservices?utm_source=chatgpt.com "Does Netflix use microservices?"
[2]: https://howik.com/netflix-microservices-architecture?utm_source=chatgpt.com "Netflix Microservices Architecture: The Inside Story - Howik"
[3]: https://www.techaheadcorp.com/blog/design-of-microservices-architecture-at-netflix/?utm_source=chatgpt.com "How Does Netflix Work? Microservices Architecture Explained"
[4]: https://medium.com/%40princevermasrcc/when-monoliths-triumph-case-studies-of-reversing-microservices-adoption-6f0bb3fd055e?utm_source=chatgpt.com "Case Studies of Reversing Microservices Adoption"
[5]: https://www.bennadel.com/blog/3944-why-ive-been-merging-microservices-back-into-the-monolith-at-invision.htm?utm_source=chatgpt.com "Why I've Been Merging Microservices Back Into The ..."
[6]: https://www.computer.org/csdl/magazine/so/2021/05/09520758/1wiMujJmZ3i?utm_source=chatgpt.com "The Monolith Strikes Back: Why Istio Migrated From ..."
[7]: https://scisimple.com/en/articles/2025-10-03-the-shift-from-microservices-to-monoliths-a-deep-dive--a37vzn8?utm_source=chatgpt.com "The Shift from Microservices to Monoliths: A Deep Dive - Simple Science"
[8]: https://svitla.com/blog/monolith-vs-microservices-architecture/?utm_source=chatgpt.com "Microservices vs Monolithic Architecture ᐈ a Full Guide"
