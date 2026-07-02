# Homework 19: Build Your Virtual Agile Team

## Product

I am building **CampusCart**, a web and mobile marketplace that helps college students buy, sell, and schedule pickup for dorm supplies, textbooks, electronics, and daily essentials on campus.

## Part 1: Team Map

```text
CampusCart Agile Team

Business and Product Direction
+-- Maya Chen - Product Owner
    +-- owns product vision, backlog priority, user stories, acceptance criteria
    +-- works closely with stakeholders, students, campus store partners, and analytics
    +-- final go-to person for product scope and priority questions

Agile Process and Team Health
+-- Jordan Lee - Scrum Master
    +-- facilitates ceremonies, removes blockers, protects sprint focus
    +-- coaches the team on Agile habits and continuous improvement
    +-- go-to person for process problems, blocked work, and team communication issues

Design and User Experience
+-- Priya Shah - Product Designer
    +-- owns user flows, wireframes, Figma designs, usability feedback
    +-- joins refinement before sprint planning so stories are ready
    +-- go-to person for UX, visual assets, and accessibility questions

Engineering
+-- Sofia Martinez - Frontend Engineer
|   +-- owns React screens, component quality, and frontend performance
|   +-- collaborates closely with Priya and Marcus
+-- Ethan Brooks - Frontend Engineer
|   +-- owns mobile responsive behavior, form validation, and UI state
|   +-- helps QA reproduce browser-specific issues
+-- Steven Zhao - Backend Engineer
|   +-- owns listing APIs, order workflow, database schema, and integration logic
|   +-- go-to person for backend bugs and API contracts
+-- Marcus Reed - Full Stack Engineer
|   +-- connects frontend flows to backend APIs and handles cross-layer features
|   +-- helps resolve frontend/backend contract disagreements
+-- Owen Patel - DevOps / Platform Engineer
    +-- owns CI/CD, AWS infrastructure, monitoring, and deployment pipeline
    +-- go-to person for Jenkins, Docker, EC2, logs, and production release issues

Quality
+-- Nia Johnson - QA Engineer
    +-- owns test strategy, regression test plans, exploratory testing, and bug triage
    +-- helps engineers define test cases before coding starts
    +-- go-to person for release confidence, but the whole team owns quality
```

In this team, nobody reports directly to the Scrum Master or Product Owner like a traditional hierarchy. The Product Owner owns product decisions, the Scrum Master owns process health, and the engineers own technical delivery together. The closest collaboration pairs are Product Owner plus Designer for discovery, Designer plus Frontend for user experience, Backend plus Full Stack for APIs, QA plus Engineers for quality, and DevOps plus Engineers for releases.

## Part 2: A Day in the Life

I am Steven, a backend engineer on the CampusCart team. My day usually starts at 9:30 AM with Daily Standup on Zoom. Jordan, our Scrum Master, opens the meeting and keeps it moving. We each answer what we finished yesterday, what we plan to do today, and whether anything is blocking us. I say that yesterday I finished the listing search endpoint, today I am adding order reservation logic, and I need a final decision from Maya about whether sellers can cancel an order after a buyer has scheduled pickup.

Maya answers right away because she owns product priority and acceptance criteria. She says the first version should allow cancellation only before pickup is confirmed, because students need predictable pickup windows. Priya adds that she already has the updated Figma flow, and Sofia says she will adjust the frontend confirmation modal after standup. The conversation starts to get a little detailed, so Jordan parks it and asks Maya, Priya, Sofia, and me to stay for five minutes after standup.

After standup, we do that quick follow-up. Maya clarifies the rule, Priya links the Figma frame in Jira, and I update the acceptance criteria on the story. I write a short comment in Jira so QA can test the same rule later: "Seller can cancel before pickup confirmation, but not after buyer confirms pickup." Nia adds a test case to the regression checklist.

Around 10:00 AM, I pull the latest `dev` branch from GitHub and start coding. I update the backend service to check order status before allowing cancellation. I also update the API response so the frontend gets a clear error message. Before I open a pull request, I run unit tests locally and add one new test for the cancellation rule.

At 11:30 AM, Marcus pings me in Slack. He is wiring Sofia's frontend screen to my endpoint and notices that my API returns `409 Conflict`, but the old frontend expected `400 Bad Request`. We jump into a short call. We decide `409` is the right status because the request is valid but conflicts with the current order state. I update the API contract in Confluence, Marcus updates the frontend handling, and we both leave notes in the Jira ticket.

After lunch, Nia finds a bug in the staging environment. If a buyer clicks confirm twice very quickly, the backend can create two pickup confirmation events. She files a bug in Jira with steps to reproduce, a screenshot, and the test account she used. Because this touches data consistency, Jordan flags it in Slack and asks if it threatens the sprint goal. I take the bug because it is in my area. I add idempotency logic to the confirmation endpoint, pair with Nia to reproduce and verify it, and then open a pull request.

At 3:00 PM, we have Backlog Refinement. Maya brings upcoming stories for saved searches and campus pickup reminders. Priya shows early wireframes, Owen asks whether the reminder service will need a scheduled job, and I point out that we should write a technical design doc before building notifications because it may involve email, push notifications, and rate limits. The team agrees to split the feature into smaller stories before Sprint Planning.

Later in the afternoon, Owen messages the team that Jenkins failed on the `dev` branch because a Docker build step is timing out. Since deployment and infrastructure are his area, he takes the lead, but Marcus and I help by checking whether any recent dependency change made the image slower to build. Owen finds a stale Docker cache issue on the EC2 worker, clears it, reruns the pipeline, and posts the green build link in Slack.

Before I end the day, I review Sofia's pull request because it depends on my API. I leave one comment about an edge case, approve after she fixes it, and move my own Jira ticket to "Ready for QA." I update my notes in Confluence so the team has the final API behavior documented. By the end of the day, the cancellation flow is code complete, the duplicate confirmation bug is fixed, and the pipeline is green again. It feels like a normal Agile day: a little planning, a little building, a little surprise, and a lot of communication.

## Part 3: Interview Q&A

### 1. What product are you building, and who is it for?

My team is building CampusCart, a campus marketplace for college students. The goal is to help students buy and sell dorm supplies, textbooks, electronics, and daily essentials with safe campus pickup scheduling.

### 2. How many people are on your team and why?

My team has nine people. That size feels right because the product needs product ownership, design, backend, frontend, QA, and deployment support, but it is still small enough that we can communicate quickly without too much process overhead.

### 3. Who is on your team, and what does each person do?

My team has Maya Chen as Product Owner, Jordan Lee as Scrum Master, Priya Shah as Product Designer, Sofia Martinez and Ethan Brooks as Frontend Engineers, me, Steven Zhao, as Backend Engineer, Marcus Reed as Full Stack Engineer, Nia Johnson as QA Engineer, and Owen Patel as DevOps Engineer. Each person has a clear responsibility, but we still collaborate across roles when a sprint goal needs help.

### 4. Who is your Product Owner and what do they do day to day?

Our Product Owner is Maya Chen. Day to day, she talks with stakeholders, reviews user feedback, writes and prioritizes user stories, clarifies acceptance criteria, and makes sure the team is building the most valuable work first.

### 5. Who is your Scrum Master and how are they different from a project manager?

Our Scrum Master is Jordan Lee. He is different from a traditional project manager because he does not assign tasks or command the team. He facilitates ceremonies, removes blockers, protects the team from random scope changes, and helps us improve how we work.

### 6. How many engineers do you have and how are they split?

We have five engineers. Sofia and Ethan focus on frontend, I focus on backend, Marcus works full stack, and Owen handles infrastructure and deployment. That split works because our product has a lot of user-facing flows, but it also needs reliable APIs and a stable release pipeline.

### 7. Do you have a QA engineer, and does only QA own testing?

We do have a QA engineer, Nia Johnson, but testing is not only her job. Nia owns test strategy and regression planning, but every engineer is responsible for writing tests, checking edge cases, and not throwing unfinished work over the wall.

### 8. Who handles deployments and infrastructure?

Owen Patel handles deployments and infrastructure. He owns Jenkins, Docker, AWS, monitoring, and production release health. Engineers still help when their code breaks the build, but Owen is the go-to person for pipeline and infrastructure problems.

### 9. Do you have a designer, and where do they fit in the sprint cycle?

Yes, our designer is Priya Shah. She works ahead of the sprint during discovery and refinement, so designs are ready before Sprint Planning. During the sprint, she answers UX questions, reviews implementation, and helps adjust designs when we find constraints.

### 10. How long are your sprints and why?

Our sprints are two weeks long. One week feels too rushed for design, development, testing, and review, while three or four weeks would make feedback slower. Two weeks gives us a good rhythm and still lets us react quickly.

### 11. What happens during Sprint Planning?

During Sprint Planning, Maya explains the sprint goal and the highest priority backlog items. The team asks questions, checks acceptance criteria, estimates work, discusses dependencies, and pulls in only the amount of work we believe we can finish. By the end, we have a sprint goal and a Sprint Backlog.

### 12. What does your Daily Standup look like?

Our Daily Standup is a 15-minute meeting every morning. Jordan runs it and keeps it focused. Each person says what they did yesterday, what they plan to do today, and whether they are blocked. If a discussion gets too detailed, we take it offline after standup.

### 13. What is a Sprint Review and who attends yours?

A Sprint Review is where we show completed work and collect feedback. Our team attends, and we also invite stakeholders like campus store partners, student support, and sometimes a few student beta users. It is not just a demo; it is a feedback session.

### 14. What is a Retrospective and what does your team actually do in it?

A Retrospective is where the team talks about how the sprint went and how we can improve. We discuss what went well, what was painful, and what action item we want to try next sprint. We keep it practical, so we leave with one or two improvements instead of a long wish list.

### 15. What is Backlog Refinement and when does your team do it?

Backlog Refinement is when we prepare future work before Sprint Planning. We usually do it once a week. Maya brings upcoming stories, Priya adds design context, engineers ask technical questions, QA adds test concerns, and we split or clarify stories until they are ready.

### 16. What is your team's Definition of Done?

Our Definition of Done means the code is implemented, reviewed, tested, merged, documented if needed, and verified in staging. For user-facing work, it also needs to match the accepted design and meet the acceptance criteria. A ticket is not done just because the code compiles.

### 17. You found a critical bug two days before release. What do you do and who do you tell?

If I find a critical bug two days before release, I immediately tell Jordan, Maya, Nia, and Owen. We triage the severity, decide whether it blocks release, create or update the Jira bug, and focus the right engineers on a fix. If the risk is too high, Maya makes the product call to delay or reduce scope.

### 18. A stakeholder wants to add a feature mid-sprint. How does your team handle it?

If a stakeholder wants to add a feature mid-sprint, we do not automatically accept it. Maya evaluates the value and urgency, Jordan protects the sprint goal, and the team discusses tradeoffs. If it is truly urgent, something else comes out of the sprint; otherwise it goes into the Product Backlog.

### 19. You are blocked on a ticket and cannot finish before the sprint ends. Who do you talk to?

If I am blocked, I first raise it in standup and talk to Jordan so the blocker is visible. I also talk to the person who can actually unblock me, like Maya for requirements, Priya for design, Owen for infrastructure, or another engineer for technical help. I would rather surface the risk early than surprise the team at the end.

### 20. Two engineers disagree on a technical approach. How is that resolved?

If two engineers disagree, we first compare the tradeoffs with facts: complexity, maintainability, performance, risk, and timeline. If it is still unclear, Marcus or I would write a short technical design note, and we would ask the team for feedback. For bigger decisions, the senior technical owner makes the final call after hearing both sides.

### 21. The designer is out sick and you need assets to move forward. What do you do?

If Priya is out sick, I check Figma and Confluence first to see whether the assets or design rules already exist. If they do not, I talk to Maya and Jordan about whether the ticket can move forward with a placeholder or whether we should switch to another ready story. I would not invent final design decisions without design review.

### 22. Your deployment pipeline breaks and blocks the whole team. Who owns that problem?

Owen owns the deployment pipeline, so he leads the investigation. But if my code caused the build failure, I stay with him until it is fixed. The team treats a broken pipeline as a team-level blocker because nobody can ship safely until it is green.

### 23. What is a Product Backlog and who owns it?

The Product Backlog is the ordered list of everything we might build for CampusCart, including features, bugs, tech debt, and research tasks. Maya owns it because she is responsible for product value and priority, but the whole team contributes input.

### 24. What is a Sprint Backlog and how does it get created?

The Sprint Backlog is the set of work the team commits to for the current sprint. It gets created during Sprint Planning when the team pulls items from the Product Backlog based on priority, capacity, dependencies, and the sprint goal.

### 25. When does your team write a technical design doc and who approves it?

We write a technical design doc when a feature affects architecture, data model, integration behavior, security, or long-term maintainability. The engineer leading the work writes it, other engineers review it, Owen reviews infrastructure impact, and Maya confirms it still supports the product requirement.

### 26. How does your team file and track bugs?

We file bugs in Jira with severity, steps to reproduce, expected behavior, actual behavior, screenshots or logs, environment, and owner. Nia usually helps triage bugs, but anyone can file one. Critical bugs get discussed immediately instead of waiting for the next meeting.

### 27. Where does your team write things down, and what tools do you use?

We use Jira for stories, bugs, sprint boards, and backlog tracking. We use Confluence for technical design docs, API decisions, and team process notes. We use Figma for designs, GitHub for code reviews, Jenkins for CI/CD, Slack for daily communication, and Zoom for ceremonies.

### 28. How does your team decide what goes into the next sprint?

Maya proposes the highest priority work based on user value, stakeholder needs, bugs, and roadmap goals. The team checks whether the work is clear and realistic, then pulls in what fits our capacity. We try to choose work that supports one clear sprint goal instead of a random list of tickets.

### 29. What happens if your team consistently does not finish sprint work?

If we consistently do not finish sprint work, we treat it as a planning and process problem, not as a reason to blame people. In retro, we look at whether stories are too large, estimates are unrealistic, blockers are hidden, or too much unplanned work is entering the sprint. Then we reduce capacity or improve refinement.

### 30. How does a brand new feature go from idea to production?

A new feature starts as an idea from a stakeholder, user feedback, or team insight. Maya turns it into a backlog item, Priya explores the user flow, the team refines and estimates it, engineers build it during a sprint, QA verifies it, stakeholders review it, Jenkins deploys it, and Owen monitors production after release.

### 31. If someone asked you in an interview to describe your team, what would you say in 60 seconds?

I would say my team is a nine-person Agile team building CampusCart, a campus marketplace for students. We have a Product Owner who owns priority, a Scrum Master who protects the process, a designer who works ahead of the sprint, five engineers split across frontend, backend, full stack, and DevOps, plus a QA engineer who leads testing strategy. We work in two-week sprints, use Jira, Confluence, Figma, GitHub, Jenkins, Slack, and AWS, and we treat quality as a team responsibility. When problems come up, we escalate early, keep the sprint goal visible, and make tradeoffs transparently instead of hiding risk.
