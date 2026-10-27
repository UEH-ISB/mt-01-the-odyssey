# Session 03 — System Design: the 5-Block Architecture

**Lecture note** · companion to the slide deck [`buoi-3.html`](buoi-3.html) · course: *Mentoring for AI-aided Projects* · The Odyssey · 120 minutes

> **How to use this note.** Each heading below matches one slide of the deck. Click the slide link to open it (`←` `→` to navigate, `N` for speaker notes).
> Under each slide you will find the key points, a longer explanation, code where it helps, the key terms (English — Vietnamese), and a **self-check**: a command or question you run or answer yourself.
> Sections marked **Deep dive** go beyond the slides.

**Primary source:** Phoenix Agent Technical Book, Chapter 3 *"Thiết kế kiến trúc hệ thống"* (AI20K Cohort 3), downloaded copy in this repository: [`docs/org/chapter-03-thiet-ke-kien-truc.md`](../../org/chapter-03-thiet-ke-kien-truc.md). Where this note says "chapter 3", it means that file.
Other sources are listed in [References](#references).

---

## Contents

| # | Section | Slides |
|---|---------|--------|
| 1 | [5-Block Architecture](#section-1--5-block-architecture) | 3 – 6 |
| 2 | [Front-End](#section-2--front-end) | 7 – 10 |
| 3 | [Back-End](#section-3--back-end) | 11 – 14 |
| 4 | [Database](#section-4--database) | 15 – 17 |
| 5 | [AI Agent](#section-5--ai-agent) | 18 – 20 |
| 6 | [DevOps](#section-6--devops) | 21 |
| 7 | [Architecture Diagram](#section-7--architecture-diagram) | 22 – 23 |
| + | [Summary, Q&A, References](#summary-qa-references) | 24 |
| A | [Appendix A — Applying this to the MT-01 repository](#appendix-a--applying-this-to-the-mt-01-repository) | — |
| B | [Appendix B — Glossary (English — Vietnamese)](#appendix-b--glossary-english--vietnamese) | — |

### Learning objectives

By the end of the session, a mentor can:

1. Name the **five blocks** of an AI-agent application and state each block's responsibility in one sentence — first with the restaurant analogy, then with a technical definition.
2. Explain the **three architectural properties** (separation of concerns, scalability, technology flexibility) that justify the split.
3. Read a **FastAPI** file in three parts: imports → classes → functions.
4. Read a **LangGraph** graph in three parts: state → nodes → edges.
5. Tell apart **structured and unstructured data** and say when a relational database or a vector store is the right tool.
6. Read and critique an **architecture diagram**, and ask the question that matters: *"which file or service is this box?"*

---

## Opening

### Slide 1 — Cover · [`#cover`](buoi-3.html#cover)

**Key points**
- Session 02 produced a running skeleton. Session 03 asks: *what are its parts, which part talks to which, and why is it split this way?*
- The whole lecture fits in five words: **Front-End, Back-End, AI Agent, Database, DevOps**.

**Explanation.** Chapter 3 opens with the most important question before writing any agent logic: *how will the system be structured?* An architecture is like a building plan: it shows where each component lives, how components communicate and which way data flows. Chapter 3 is blunt about the stakes: a good design lets a team develop quickly, maintain easily and scale when needed; a poor one leads to tangled code, many bugs and rewrites from scratch.

### Slide 2 — Agenda · [`#agenda`](buoi-3.html#agenda)

Seven sections follow the path of a request: the three **processing blocks** (Front-End → Back-End → AI Agent), then the two **supporting blocks** (Database, DevOps), and finally the **diagram** that puts them together. The last slide gives the summary, Q&A and references.

---

## Section 1 — 5-Block Architecture

### Slide 3 — Five blocks, one system · [`#overview`](buoi-3.html#overview)

**Key points**
- Chapter 3 describes a **three-tier architecture**: Front-End, Back-End, AI Agent. This lecture adds the two supporting blocks that every real deployment needs: **Database** and **DevOps**.
- Every link is **two-way**: a request goes one way, a response comes back. Hence the double-headed arrows.
- **Rule:** the agent never talks to users directly. All traffic passes through the back-end.

**Explanation.** The three processing blocks handle a request in sequence. The user types in the **front-end**; the front-end sends an HTTP request to the **back-end**; the back-end checks who the user is and what they may do, then calls the **agent**; the agent calls the **LLM provider** and its tools; the answer travels back the same way. The **database** sits under the back-end and the agent: the back-end reads and writes records (users, conversations), the agent retrieves context (documents). **DevOps** sits under everything: it packages, tests, deploys, hosts and monitors every block.

Chapter 3 gives three technical reasons why the agent must sit behind the back-end:

1. **Access control.** The back-end decides who may use the system and what each role may do. An agent reachable from the browser would skip that check.
2. **Rate limiting.** LLM calls cost money and time. The back-end limits how often each user can call.
3. **Logging.** The back-end records every request. Without it, failures cannot be traced.

The LLM provider is drawn with a dashed border because it is **external**: a paid service the team does not own and can swap (OpenAI, Anthropic, Google…).

```mermaid
flowchart LR
    U([Users]) <--> FE[Front-End]
    FE <-->|HTTP request / JSON, stream| BE[Back-End]
    BE <-->|task / result| AG[AI Agent]
    AG <-->|prompt / tokens| LLM[(LLM provider<br/>external)]
    BE <-->|read / write| DB[(Database)]
    AG <-->|retrieve context| DB
    subgraph DevOps["DevOps — build · test · deploy · host · monitor"]
      FE
      BE
      AG
      DB
    end
```

**Key terms:** three-tier architecture — *kiến trúc ba tầng*; block / layer — *khối / tầng*; external service — *dịch vụ bên ngoài*; rate limiting — *giới hạn tần suất gọi*.

**Self-check.** Without looking: draw the five blocks and the arrows between them. Then answer: *why can the front-end not call the agent directly?* (Give at least the three reasons above.)

### Slide 4 — The restaurant analogy · [`#restaurant`](buoi-3.html#restaurant)

**Key points**

| Block | Restaurant | Why the analogy works |
|---|---|---|
| Front-End | Dining room, décor, **menu** | What the guest sees and touches |
| API | **Waiter** and the **order ticket** | Carries requests in a fixed format between room and kitchen |
| Back-End | **Kitchen** | Checks every ticket, cooks by fixed recipes, coordinates stations |
| AI Agent | **Culinary R&D lab** | Handles dishes with no recipe yet: try, taste, adjust, repeat |
| Database | **Cold storage & pantry** | Labelled shelves; keeps ingredients long-term; fetch what is needed |
| DevOps | **Business licence, premises, daily operations** | Register, rent the building, pass inspections, CCTV and logbook |

**The order flow (badges 1–6 on the slide).**
1. A guest orders from the menu → *the user types a question in the chat UI.*
2. The waiter writes a ticket in a fixed format and carries it to the kitchen → *the front-end sends a JSON request that must match the API schema.*
3. The kitchen checks the ticket and cooks by recipe → *the back-end validates, checks permissions and runs business logic.*
4. An unusual request? The kitchen asks the R&D lab → *the back-end invokes the agent for open-ended reasoning.*
5. Kitchen and lab fetch ingredients from storage → *the back-end reads records; the agent retrieves documents.*
6. The dish goes back through the waiter → *the response streams back to the user.*

**Explanation — two points to stress.**
- **The R&D lab is not the kitchen.** Fixed procedures (validation, permissions, calculations, workflows) belong in the back-end. Only genuinely open-ended problems belong in the agent. A common student mistake is calling an LLM for something an `if` statement does — slower, more expensive and less reliable.
- **DevOps is not a one-off.** Renting the building happens once; keeping the restaurant open every day — inspections before opening (CI), cameras and a logbook (monitoring) — never stops.

**Self-check.** For your own project, write one sentence per row: *"In our system, the waiter's ticket is …; the R&D lab handles …; the cold storage keeps …"*. If a row is empty, write "not yet".

### Slide 5 — From the restaurant to technical definitions · [`#definitions`](buoi-3.html#definitions)

| Block | Technical definition | Common tools / frameworks |
|---|---|---|
| **Front-End** | The **client-side** layer, running in the user's browser or device: renders the interface, captures input and calls back-end APIs. | React, Next.js, Streamlit, Gradio |
| **API** | The **contract** between blocks: which operations exist, what each accepts and what it returns. | HTTP, REST, JSON, OpenAPI |
| **Back-End** | The **server-side** layer: receives requests, authenticates and authorises users, enforces business rules, orchestrates the agent and data access. | FastAPI, Django, Express, Spring Boot |
| **AI Agent** | The **reasoning** component: uses an LLM to plan, call tools and iterate until a goal-directed task is solved. | LangGraph, LangChain, LlamaIndex |
| **Database** | The **persistent** storage layer: keeps data beyond a single request — structured records and embeddings. | SQLite, PostgreSQL, MySQL, ChromaDB, pgvector |
| **DevOps** | The **practices and infrastructure** to build, test, release, deploy, operate and monitor the system. | Docker, GitHub Actions, AWS (EC2, S3, RDS) |

**Explanation.** The API row is not a sixth block: it is the interface *between* blocks. It appears in the table because the front-end can only be as good as the API it calls, and the agent can only be reached through it.

The tools column lists common options, not recommendations. Choosing among them is the team's decision, and the reasons belong in an **Architecture Decision Record** (see [Deep dive: ADR](#deep-dive--architecture-decision-records-adr)).

**Self-check.** Cover the middle column and give each block's definition from memory. Then check: did you mention *where it runs* (client / server) and *what it is responsible for*?

### Slide 6 — Why five blocks? Three architectural properties · [`#properties`](buoi-3.html#properties)

Chapter 3 gives three reasons for splitting the system instead of putting everything in one file. Each has a formal definition from the software-engineering literature.

#### 1. Separation of concerns

> *Decomposing a system into distinct parts, each addressing a single concern, so that each part can be understood, developed and changed in isolation.* — after Dijkstra (1974), *On the role of scientific thought* (EWD447).

- **In practice:** front-end only presents; back-end only handles business logic and data; agent only reasons. Fixing a UI bug never requires reading agent code; optimising the agent never touches the UI.
- **Restaurant:** the chef does not take payments at the table.

#### 2. Scalability

> *The ability of a system to maintain acceptable performance as its load grows, by adding resources rather than redesigning.* — after Kleppmann (2017), *Designing Data-Intensive Applications*, ch. 1.

- **In practice:** each block scales independently. Chapter 3 points out that the agent is usually the **bottleneck**, because LLM inference takes seconds. You can run more agent instances without adding front-end or back-end resources; conversely, with 1,000 concurrent users you scale the front-end and back-end, not the agents.
- **Two directions of scaling:** *vertical* (a bigger machine) and *horizontal* (more machines behind a load balancer). Separated blocks make horizontal scaling possible per block.
- **Restaurant:** more guests → more tables and waiters, not a second R&D lab.

#### 3. Technology flexibility (loose coupling)

> *Blocks depend only on each other's interfaces, never on internal implementations, so one block can be replaced without changing the others.* — the practical consequence of Parnas (1972), *On the criteria to be used in decomposing systems into modules* (information hiding).

- **In practice:** React → Vue, Streamlit → Next.js, OpenAI → Anthropic, SQLite → PostgreSQL — the API contract stays the same, so other blocks do not change.
- **Restaurant:** redecorating the room does not change the recipes.

**Chapter 3's key point:** the three-tier split is *not* over-engineering — it is the baseline, even for small projects.

**Key terms:** separation of concerns — *tách biệt trách nhiệm*; scalability — *khả năng mở rộng*; bottleneck — *điểm nghẽn*; loose coupling — *liên kết lỏng*; information hiding — *che giấu thông tin*.

**Self-check.** Chapter 3, review question 1: *In a three-tier architecture, why does the AI agent not communicate directly with the front-end? Give at least three technical reasons and explain the benefit of each.*

---

## Section 2 — Front-End

### Slide 7 — Two interface patterns: chat and dashboard · [`#fe-patterns`](buoi-3.html#fe-patterns)

**Key points**
- **Chat interface:** conversational, turn-by-turn interaction in natural language. Key elements: history, message list, streamed answer, sources, input box. Best for people who *ask*.
- **Dashboard:** aggregated, at-a-glance monitoring and decision support. Key elements: KPI tiles, charts, tables, filters, status. Best for people who *watch* (managers, reviewers, operations).
- **UI vs UX:** UI (*user interface*) is what users **see** — layout, colour, typography, controls. UX (*user experience*) is what users **feel** — speed, clarity, confidence. A beautiful interface in which users get lost is good UI and bad UX.

**Explanation.** Chapter 3 calls the front-end the "storefront" of the application. For AI-agent apps it usually takes the two forms above, often both in the same product: a chat for end users and a dashboard showing statistics, conversation history and management features.

**Self-check.** Name one user group of your project that needs a chat and one that needs a dashboard. If one group needs neither, what *do* they need?

### Slide 8 — React and Next.js · [`#fe-frameworks`](buoi-3.html#fe-frameworks)

**React** (Meta, 2013) is a JavaScript **library** for building user interfaces from reusable **components**; when the state changes, React re-renders the affected parts of the view.

**Next.js** (Vercel, 2016) is a full-stack **framework** built on React that adds routing, server-side rendering, static generation and server-side code (route handlers, server actions).

| Aspect | React | Next.js |
|---|---|---|
| Type | UI library | Full-stack framework on React |
| Rendering | Client-side (CSR) by default | SSR, SSG, CSR and React Server Components in one app |
| Routing | Add a library (e.g. React Router) | Built in, file-based (App Router) |
| Server code | None — needs a separate back-end | Route handlers, server actions |
| SEO / first load | Weaker: page is built in the browser | Strong: HTML is pre-rendered on the server |
| Learning curve | Smaller, flexible | Larger, opinionated |
| Typical use cases | Single-page apps, internal tools, dashboards, chat widgets embedded in other sites | Public product sites, SEO-critical pages, full-stack AI chat apps, e-commerce |

**Rendering modes (chapter 3).**
- **Server-Side Rendering (SSR):** the HTML is built on the server and sent to the browser. Good for static content and SEO (blog, landing page, dashboards of historical data).
- **Client-Side Rendering (CSR):** the HTML is almost empty; JavaScript builds the content in the browser. Good for interactive apps — especially chat, which updates in real time without reloading.
- **Static Site Generation (SSG):** pages are built once at build time.
- Real AI-agent apps mix them: SSR for public pages, CSR for the chat. Next.js supports both in one app through the App Router.

**Why React/Next.js for AI agents (chapter 3).**
1. **Streaming response** — agents answer token by token; React handles streaming updates better than Streamlit.
2. **Full customisation** — not limited by the templates of Streamlit or Gradio.
3. **Production-ready** — the choice of most companies if the project continues.

**Chapter 3's tip:** if time is short, start with **Streamlit** for a quick prototype (a basic chat UI takes a few lines of Python), then migrate to React/Next.js for a polished production interface.

#### Deep dive — a minimal Streamlit chat

```python
# app.py — run: streamlit run app.py
import requests
import streamlit as st

st.title("AI Assistant")
if "history" not in st.session_state:
    st.session_state.history = []                       # list of {"role", "content"}

for msg in st.session_state.history:                    # redraw the conversation
    st.chat_message(msg["role"]).write(msg["content"])

if question := st.chat_input("Ask anything…"):
    st.session_state.history.append({"role": "user", "content": question})
    st.chat_message("user").write(question)
    r = requests.post("http://localhost:8000/api/v1/chat", json={"message": question}, timeout=60)
    answer = r.json().get("answer", "Something went wrong.") if r.ok else "Something went wrong."
    st.session_state.history.append({"role": "assistant", "content": answer})
    st.chat_message("assistant").write(answer)
```

Note that the UI only talks to the back-end's API — exactly the rule of slide 3. Swapping Streamlit for React later changes this file only.

**Key terms:** component — *thành phần giao diện*; rendering — *dựng trang*; server-side rendering — *dựng trang phía máy chủ*; client-side rendering — *dựng trang phía trình duyệt*.

**Self-check.** For your project, which is more important: SEO for public pages, or a fast interactive chat? Which framework does that suggest, and is a Streamlit prototype enough for the next two weeks? Write the answer down — it is a candidate for an ADR.

### Slide 9 — Anatomy of a standard chat interface · [`#fe-chat`](buoi-3.html#fe-chat)

Chapter 3 names three core components — **input box, message list, streaming display**. A modern assistant adds five more:

| # | Element | Requirement |
|---|---|---|
| 1 | Conversation history | Past sessions in a sidebar; start a new chat |
| 2 | Header | Assistant name, status, settings |
| 3 | User message | Right-aligned; `role = "user"` |
| 4 | Assistant message | Renders **Markdown**: code blocks, tables, bold/italic (chapter 3) |
| 5 | Sources / citations | Each claim links to the document it came from |
| 6 | Message actions | Copy, regenerate, rate (feedback) |
| 7 | Streaming display | Tokens appear as they are generated, with a typing indicator; **auto-scroll** to the newest message |
| 8 | Input box | Multi-line; **Enter** sends, **Shift+Enter** inserts a new line; optional file upload |

**Data flow.**

```mermaid
flowchart LR
    I[Input box] -->|POST /api/v1/chat| API[Back-end]
    API -->|SSE stream| S[Streaming display]
    S --> M[Message list]
```

Each message is the same small object — which is also what the database will store:

```json
{ "role": "assistant", "content": "…", "timestamp": "2026-10-26T10:02:00Z", "sources": ["report.pdf#p2"] }
```

**Why streaming matters.** An agent can take 10–15 seconds to finish. Showing tokens as they arrive tells the user the system is working; a blank screen for that long feels broken (chapter 3).

**Why sources matter.** An answer the user cannot verify is an answer the user should not rely on — especially in education, health or finance.

**Self-check.** Open any AI assistant you use. Find all eight elements. Which one is missing, and does its absence hurt?

### Slide 10 — Evaluating a front-end · [`#fe-evaluation`](buoi-3.html#fe-evaluation)

Chapter 3's minimal "good enough" list for a project front-end:
- the user can ask a question and get an answer;
- streaming displays correctly;
- error messages are friendly (no stack traces);
- the interface is responsive (works on mobile);
- there is an indicator while the agent is working (spinner or "typing…").

Chapter 3 also warns: don't spend too long on UI polish — *a simple, stable interface beats a beautiful, buggy one*. Within an agent project, front-end quality is not the main evaluation criterion; agent logic and back-end matter more.

The slide places those items in a more formal frame:

| Dimension | Requirement | Metric / reference target | How to measure |
|---|---|---|---|
| Functional correctness | Every user task completes end to end; streamed Markdown renders correctly | Task success rate on scripted scenarios | End-to-end tests (e.g. Playwright) |
| Performance | Fast first load; immediate response to input; visible first token | **Core Web Vitals** (Google, web.dev): LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 — "good" thresholds at the 75th percentile; plus time-to-first-token | Lighthouse, Chrome DevTools |
| Usability | Clear, consistent, recoverable; system status always visible | Nielsen's 10 usability heuristics; System Usability Scale (SUS, 0–100) | Heuristic review; test with ~5 users |
| Accessibility | Perceivable, operable, understandable, robust (POUR) | **WCAG 2.2 level AA**; normal-text contrast ≥ 4.5:1 | axe, Lighthouse, keyboard-only test |
| Responsiveness | Works across screen sizes and input types | No horizontal scrolling at the smallest target width | DevTools device mode; a real phone |
| Reliability | Loading states, retries, friendly errors | Zero raw errors or stack traces shown to users | Stop the back-end; throttle the network |
| Security | No secrets in client code; sanitise rendered Markdown (XSS); HTTPS only | Zero secrets in the bundle | Bundle scan; OWASP checklist |

**Important.** Only two rows carry *published* numeric thresholds (Core Web Vitals and WCAG contrast). For the others, the pass mark is a **team decision** for its own users. A mentor asks the team for their target; a mentor does not set it.

**Key terms:** usability — *tính dễ dùng*; accessibility — *khả năng tiếp cận*; responsive — *hiển thị thích ứng*; heuristic — *nguyên tắc kinh nghiệm*.

**Self-check.** Run Lighthouse on any web page you built (Chrome DevTools → Lighthouse → *Analyze page load*). Note the Performance and Accessibility scores and the top issue for each.

---

## Section 3 — Back-End

### Slide 11 — The back-end: the system's control centre · [`#be-overview`](buoi-3.html#be-overview)

**Definition.** The **back-end** is the server-side part of an application that exposes APIs, enforces security and business rules, coordinates data and services, and returns results to clients. Clients see only its API — never its internals.

Chapter 3 calls it the "transit station": it receives requests from the front-end, handles all business logic that is *not* AI reasoning (authentication, authorisation, data validation, sessions, chat history, rate limiting, logging, error handling), calls the agent when needed, stores data and returns the result.

**One request, top to bottom.**

| Step | What happens | Typical failure |
|---|---|---|
| 1 · Routing & validation | Match method + path; parse JSON into a schema | **422** Unprocessable Entity — body does not match the schema |
| 2 · Authentication | *Who are you?* — verify a token or session | **401** Unauthorized — unknown or missing identity |
| 2 · Authorisation | *Are you allowed?* — check the role or ownership | **403** Forbidden — known identity, not permitted |
| 3 · Business logic | Rules, workflows, decisions | 409 Conflict, 400 Bad Request |
| 4 · Data access | SQL / ORM queries, transactions | — |
| 5 · Agent client | Invoke the agent; timeouts; retries | 504 Gateway Timeout, 503 |
| 6 · Response | JSON body or SSE stream + status code | **200** OK / 201 Created / 204 No Content |

**Cross-cutting concerns** (they wrap *every* step):
- **Error handling** — catch failures, log the detail, return a safe message and an appropriate status.
- **Logging & monitoring** — request id, latency, status; who did what and when; metrics and alerts.

**401 vs 403** is the most common confusion in student projects. 401: *the server does not know who you are.* 403: *the server knows who you are, and the answer is no.* Role rules — "only reviewers may approve" — produce 403, and they must be enforced **on the server**: hiding a button in the UI is not access control, because anyone can call the API directly.

**Key terms:** authentication — *xác thực (bạn là ai)*; authorisation — *phân quyền (bạn được làm gì)*; business logic — *nghiệp vụ*; cross-cutting concern — *mối quan tâm xuyên suốt*; status code — *mã trạng thái*.

**Self-check.** For each request below, which status code should the back-end return? (a) Body is `{}` but `message` is required. (b) No login token. (c) A student tries to approve a report that only lecturers may approve. (d) The LLM provider is down.
*Answers: 422, 401, 403, 503 (or 500 with a safe message).*

### Slide 12 — APIs and REST · [`#be-api`](buoi-3.html#be-api)

**API — Application Programming Interface.** A documented **contract** that specifies how one software component can request services or data from another: the **operations** available, the **input** each accepts, and the **output and errors** it returns — independent of how it is implemented. A **web API** exposes that contract over HTTP.

**REST — Representational State Transfer** (Fielding, 2000). An architectural style for networked systems. Its constraints:

| Constraint | Meaning |
|---|---|
| Client–server | UI and data storage are separated |
| Stateless | Each request carries everything needed; the server keeps no session between requests |
| Cacheable | Responses say whether they may be cached |
| Uniform interface | Resources identified by URLs; manipulated through representations (JSON) with standard methods |
| Layered system | Clients cannot tell whether they talk to the server or an intermediary (proxy, load balancer) |

**HTTP methods** (RFC 9110): `GET` read · `POST` create · `PUT` replace · `PATCH` partially update · `DELETE` remove. `GET`, `PUT` and `DELETE` are *idempotent* — repeating them has the same effect as doing them once.

**Status code families:** `2xx` success (200 OK, 201 Created, 204 No Content) · `4xx` client error (400, 401, 403, 404, 409, 422, 429 Too Many Requests) · `5xx` server error (500, 502, 503, 504).

**Example endpoints for a chat application.**

| Endpoint | Prototype (Python / FastAPI) | Input | Output |
|---|---|---|---|
| `GET /health` | `async def health() -> dict` | — | `200 {"status": "ok"}` |
| `POST /api/v1/chat` | `async def chat(req: ChatRequest) -> ChatResponse` | body `{"message": "…", "session_id": "s1"}` | `200 {"answer": "…", "session_id": "s1", "sources": [...]}` · `422` |
| `POST /api/v1/chat/stream` | `async def chat_stream(req: ChatRequest) -> StreamingResponse` | body as above | `200 text/event-stream` |
| `GET /api/v1/conversations` | `async def list_conversations(limit: int = 20) -> list[Conversation]` | query `?limit=20` | `200 [{"id": 7, "title": "…"}]` |
| `GET /api/v1/conversations/{id}` | `async def get_conversation(id: int) -> Conversation` | path `id` | `200 {…}` · `404` |
| `PATCH /api/v1/conversations/{id}` | `async def rename_conversation(id: int, body: Rename) -> Conversation` | path `id`, body `{"title": "…"}` | `200 {…}` · `403` · `404` |
| `DELETE /api/v1/conversations/{id}` | `async def delete_conversation(id: int) -> None` | path `id` + token | `204` · `403` not owner · `404` |

Notice the three places input can arrive — **body**, **query string**, **path** — and that every endpoint documents its **error** codes as well as its success code. An API that documents only the happy path is half a contract.

**Versioning.** The `/api/v1/` prefix lets you introduce `/api/v2/` later without breaking existing clients.

**Self-check.** Design two endpoints for a feature of your own project (e.g. "submit a document for review"). Fill in the four columns above, including at least two error codes per endpoint.

### Slide 13 — FastAPI · [`#be-fastapi`](buoi-3.html#be-fastapi)

**Definition.** FastAPI is an open-source, high-performance Python web framework for building APIs from standard Python **type hints** (created by Sebastián Ramírez, 2018). It runs on the **ASGI** standard via **Starlette** and validates data with **Pydantic**.

**The stack:** Uvicorn (ASGI server that listens on the port) → Starlette (web toolkit: requests, responses, routing) → FastAPI (endpoints, dependency injection, OpenAPI docs) → Pydantic (data validation).

**Why chapter 3 chooses FastAPI.**

1. **Native async performance.** Built on Starlette with `async`/`await` from the start. An agent app spends most of its time *waiting*: on the LLM (seconds), on tools (hundreds of milliseconds). Async lets the server serve other requests while one waits, instead of blocking.
2. **Automatic API documentation.** FastAPI generates an **OpenAPI** schema (formerly Swagger) and serves interactive docs at `/docs` (Swagger UI) and `/redoc` (ReDoc). Docs update every time an endpoint changes — no manual writing.
3. **Type safety with Pydantic.** Every request and response is validated. Wrong data (e.g. a string where an integer is expected) gets an automatic **422** with a detailed message.
4. **Server-Sent Events (SSE).** The server can push data continuously — ideal for streaming LLM output and the "typing" effect.

Plus **dependency injection** (`Depends`): a declarative way to give an endpoint what it needs — the current user, a database session, settings. It is how authentication is attached to endpoints.

#### Deep dive — sync vs async, measured

```python
# async_demo.py — run: python async_demo.py
import asyncio, time

async def fake_llm(name: str) -> str:
    await asyncio.sleep(1)        # stands in for waiting on the network
    return name

async def main() -> None:
    t = time.perf_counter()
    for n in "ABC":
        await fake_llm(n)                                   # one after another
    print(f"sequential: {time.perf_counter() - t:.1f}s")

    t = time.perf_counter()
    await asyncio.gather(*(fake_llm(n) for n in "ABC"))     # waits overlap
    print(f"concurrent: {time.perf_counter() - t:.1f}s")

asyncio.run(main())
```

**Self-check.** Run it. Expected output:

```
sequential: 3.0s
concurrent: 1.0s
```

Same work, same machine — the difference is only whether the waits overlap. That is the timeline on the slide.

**A warning that follows from this.** Inside an `async def` endpoint, never call a *blocking* function (e.g. `time.sleep`, a synchronous HTTP client like `requests`, or a synchronous DB driver). It blocks the whole event loop and every other user waits. Use async libraries (`httpx.AsyncClient`, `agent.ainvoke`) — or declare the endpoint with plain `def`, which FastAPI runs in a thread pool.

### Slide 14 — Reading a FastAPI file: imports → classes → functions · [`#be-code`](buoi-3.html#be-code)

Every FastAPI module reads the same way:

| Part | What it contains | Question it answers |
|---|---|---|
| **0 · Imports** | `fastapi`, `pydantic`, project modules | *What does this file depend on?* |
| **1 · Classes** | Pydantic models (`BaseModel`) | *What shape must the data have, going in and out?* |
| **2 · Functions** | Endpoints decorated with `@app.get/post/…` | *What does each operation do?* |

**The two packages.**
- **`fastapi`** — the web framework. `FastAPI()` creates the application; decorators like `@app.post("/path")` bind a function to an HTTP method + path; `HTTPException(status_code, detail)` returns an error; `StreamingResponse` streams; `Depends` injects dependencies.
- **`pydantic`** — the data-validation library. A class inheriting `BaseModel` declares fields with Python types; `Field(...)` adds constraints (`...` = required, `min_length`, `max_length`, `description`). Validation happens *before* your function runs. Pydantic v2's core is written in Rust, which makes validation fast.

#### Deep dive — a complete, runnable demo

The slide's example, completed with a fake agent so it runs without an API key. Adapted from chapter 3.

```python
# demo_app.py — run: uvicorn demo_app:app --reload     (Python 3.10+)
# ── 0 · IMPORTS ───────────────────────────────────────────────
import asyncio
import logging
import uuid

from fastapi import FastAPI, HTTPException
from fastapi.responses import StreamingResponse
from pydantic import BaseModel, Field

logger = logging.getLogger("demo")
app = FastAPI(title="AI Agent API (demo)", version="0.1.0")


# ── 1 · CLASSES: request / response contracts ────────────────
class ChatRequest(BaseModel):
    """What the client must send."""
    message: str = Field(..., min_length=1, max_length=10_000, description="The user's question")
    session_id: str | None = Field(None, description="Conversation id, if continuing one")


class ChatResponse(BaseModel):
    """What the server promises to return."""
    answer: str
    session_id: str
    sources: list[str] = Field(default_factory=list)


async def fake_agent(question: str) -> str:
    await asyncio.sleep(0.5)                                  # stands in for the LLM call
    if "boom" in question:
        raise RuntimeError("DB password=hunter2 leaked in this internal error")
    return f"You asked: {question}"


# ── 2 · FUNCTIONS: endpoints ──────────────────────────────────
@app.post("/api/v1/chat", response_model=ChatResponse)
async def chat(req: ChatRequest) -> ChatResponse:
    """Send a question to the agent and receive the answer."""
    try:
        answer = await fake_agent(req.message)
    except Exception:
        logger.exception("chat failed")                       # full detail → server log
        raise HTTPException(status_code=500,
                            detail="Something went wrong. Please try again.")  # safe → user
    return ChatResponse(answer=answer, session_id=req.session_id or str(uuid.uuid4()))


@app.post("/api/v1/chat/stream")
async def chat_stream(req: ChatRequest) -> StreamingResponse:
    """Stream the answer word by word as Server-Sent Events."""
    async def generate():
        for word in f"You asked: {req.message}".split():
            await asyncio.sleep(0.2)
            yield f"data: {word}\n\n"                         # SSE frame: 'data: …' + blank line
    return StreamingResponse(generate(), media_type="text/event-stream")


@app.get("/health")
async def health() -> dict:
    """Liveness probe for load balancers and monitoring."""
    return {"status": "ok"}
```

**Self-check** (Git Bash; in PowerShell use `curl.exe`, or use the **Try it out** buttons at `http://localhost:8000/docs`):

```bash
pip install fastapi uvicorn
uvicorn demo_app:app --reload                                       # terminal 1

curl -s localhost:8000/health                                       # → {"status":"ok"}
curl -s -X POST localhost:8000/api/v1/chat -H "Content-Type: application/json" \
     -d '{"message": "hello"}'                                      # → 200 with an answer
curl -s -o /dev/null -w "%{http_code}\n" -X POST localhost:8000/api/v1/chat \
     -H "Content-Type: application/json" -d '{"message": ""}'      # → 422
curl -s -X POST localhost:8000/api/v1/chat -H "Content-Type: application/json" \
     -d '{"message": "boom"}'                                       # → safe message only
curl -N -X POST localhost:8000/api/v1/chat/stream -H "Content-Type: application/json" \
     -d '{"message": "stream me please"}'                           # → words arrive one by one
```

Pass criteria: the empty message returns **422**; the `boom` request returns the generic message to the client while the **password appears only in the server terminal**; the stream prints words with visible pauses.

#### Deep dive — error handling, development vs production

Chapter 3's pattern: classify errors, return a friendly message with the right status, and reveal details only in development.

```python
@app.post("/api/v1/chat")
async def chat(request: ChatRequest):
    try:
        response = await agent.run(request.message)
        return ChatResponse(answer=response.content,
                            session_id=request.session_id or str(uuid4()))
    except LLMRateLimitError:
        raise HTTPException(status_code=429,
                            detail="The agent is busy. Please try again in a few seconds.")
    except LLMAuthError:
        if settings.app_env == "development":
            raise HTTPException(status_code=500, detail="Invalid API key. Check your .env file.")
        raise HTTPException(status_code=500, detail="System configuration error. Please contact the admin.")
    except Exception as e:
        logger.error(f"Unexpected error: {e}")
        raise HTTPException(status_code=500, detail="An unexpected error occurred. Please try again.")
```

> **Chapter 3 warning:** never show a Python stack trace or an internal error message to end users. In production, log the details (file or monitoring system) and return a generic message. Database connection strings and API keys can leak through error messages.

The anti-pattern to look for in code review is `raise HTTPException(status_code=500, detail=str(e))`: it sends the raw internal message to the client.

**Key terms:** type hint — *gợi ý kiểu dữ liệu*; validation — *kiểm tra hợp lệ*; dependency injection — *tiêm phụ thuộc*; event loop — *vòng lặp sự kiện*; stack trace — *vết ngăn xếp lỗi*.

---

## Section 4 — Database

### Slide 15 — Structured, semi-structured and unstructured data · [`#db-data`](buoi-3.html#db-data)

**First question (chapter 3): does the app need a database at all?** Many students assume every app needs one; not so. For an agent app, a database is needed for specific use cases:

| Use case | Why | Store |
|---|---|---|
| Chat history | Users review past conversations — the most common case | Relational DB |
| Sessions / long-term memory | Short-term memory within a session can live in LangGraph's built-in memory; **cross-session** memory needs a DB | Relational DB / key-value |
| Documents for RAG | The agent answers from specific documents | **Vector store** |
| Analytics & monitoring | Users, conversations, popular questions, response time | Relational DB / logs |

> **YAGNI** ("You Aren't Gonna Need It"): if the agent only chats and stores nothing long-term, skip the database at first. Add it when you actually need it (chapter 3).

**Three shapes of data.**

| Type | Description | Examples | Typical store |
|---|---|---|---|
| **Structured** | Fixed schema: rows × typed columns | Users, orders, messages | Relational DB (SQL) |
| **Semi-structured** | Self-describing; tags/keys; fields may vary | JSON, XML, logs | Document DB (NoSQL, e.g. MongoDB) or JSON columns (PostgreSQL `JSONB`) |
| **Unstructured** | No predefined model | PDFs, text, images, audio, video | Object storage (S3) + **embeddings** in a vector store |

**SQL vs NoSQL in one line.** SQL databases enforce a schema and relationships with strong consistency (ACID); NoSQL databases (document, key-value, wide-column, graph) relax the schema for flexibility or horizontal scale. Most agent apps start with SQL for records and add a vector store for documents.

**The relational model** (Codd, 1970) stores data in **tables** (relations) of rows (tuples) and columns (attributes). A **primary key (PK)** identifies each row; a **foreign key (FK)** references a row in another table. **SQL** (Structured Query Language, standardised as ISO/IEC 9075) defines, queries and modifies the data. **ACID** transactions — Atomicity, Consistency, Isolation, Durability — keep it correct even when many users write at once.

**Three relational engines.**

| | SQLite | PostgreSQL | MySQL |
|---|---|---|---|
| Model | Embedded library; the whole DB is **one file** | Client–server, open source | Client–server, open source (Oracle) |
| Setup | Zero configuration; `import sqlite3` is built into Python | Install / container / managed service | Install / container / managed service |
| Concurrency | Many readers, limited concurrent **writes** | Strong (MVCC), many writers | Strong |
| Extras | Tiny, portable, easy to back up (copy the file) | JSONB, full-text search, extensions such as **pgvector** | Very widely hosted; fast reads |
| Best for | Development, tests, small single-user apps | Production default for many teams | Web applications, existing MySQL ecosystems |

**Chapter 3's recommendation:** SQLite for development, PostgreSQL for production, switching only the connection string:

```python
import os
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = os.getenv("DATABASE_URL", "sqlite:///./data/app.db")   # fallback for development
engine = create_engine(DATABASE_URL)          # e.g. postgresql+psycopg://user:pass@host/db in production
SessionLocal = sessionmaker(bind=engine)
```

SQLAlchemy (an **ORM** — object-relational mapper) hides most differences between engines, so the code stays nearly the same.

**Key terms:** schema — *lược đồ*; primary key — *khóa chính*; foreign key — *khóa ngoại*; transaction — *giao dịch*; ORM — *ánh xạ đối tượng–quan hệ*.

**Self-check.** List every piece of data your project stores. Label each one structured, semi-structured or unstructured. Which ones genuinely need to survive a server restart?

### Slide 16 — Entity-Relationship Diagrams (ERD) and SQL · [`#db-erd`](buoi-3.html#db-erd)

**Definition.** An **ERD** is a conceptual model of the data a system must store: its **entities** (things), their **attributes** (properties) and the **relationships** between them, including **cardinality** — one-to-one (1–1), one-to-many (1–N), many-to-many (N–M). Introduced by Peter Chen (1976).

**Why we need it.**
1. A shared language between business people and engineers.
2. It reveals missing data and rules *before* coding — a missing table costs a minute on paper and a migration in code.
3. It is the blueprint for tables, keys and constraints.
4. It avoids duplicated data (**normalisation**).

**Crow's-foot notation.** Two bars `||` = exactly one; circle + fork `o<` = zero or many; bar + fork `|<` = one or many.

**Example — a chat application.**

```mermaid
erDiagram
    USERS ||--o{ CONVERSATIONS : owns
    CONVERSATIONS ||--o{ MESSAGES : contains
    MESSAGES ||--o{ FEEDBACK : receives
    USERS ||--o{ FEEDBACK : gives
    USERS {
        int id PK
        string email
        string full_name
        string role
        datetime created_at
    }
    CONVERSATIONS {
        int id PK
        int user_id FK
        string title
        datetime created_at
    }
    MESSAGES {
        int id PK
        int conversation_id FK
        string role
        text content
        datetime created_at
    }
    FEEDBACK {
        int id PK
        int message_id FK
        int user_id FK
        int rating
        text comment
    }
```

Read it as sentences: *a user owns zero or many conversations; each conversation belongs to exactly one user. A conversation contains zero or many messages. A message receives zero or many feedback ratings; each rating is given by exactly one user.*

**SQL — the two sub-languages.**
- **DDL** (Data Definition Language): `CREATE`, `ALTER`, `DROP` — the structure.
- **DML** (Data Manipulation Language): `INSERT`, `SELECT`, `UPDATE`, `DELETE` — the data.

```sql
-- DDL
CREATE TABLE messages (
  id              INTEGER PRIMARY KEY,
  conversation_id INTEGER NOT NULL REFERENCES conversations(id),
  role            TEXT NOT NULL CHECK (role IN ('user', 'assistant')),
  content         TEXT NOT NULL,
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- DML
INSERT INTO messages (conversation_id, role, content) VALUES (42, 'user', 'Chính sách hoàn tiền thế nào?');

SELECT c.title, m.role, m.content
FROM messages AS m
JOIN conversations AS c ON c.id = m.conversation_id     -- follows the "contains" line
WHERE c.user_id = 7
ORDER BY m.created_at DESC
LIMIT 20;

UPDATE conversations SET title = 'Hỏi về hoàn tiền' WHERE id = 42;
DELETE FROM messages WHERE conversation_id = 42;
```

**Self-check — run a real database in five seconds** (Python's built-in `sqlite3`, no install):

```python
# erd_demo.py — run: python -X utf8 erd_demo.py
import sqlite3

db = sqlite3.connect(":memory:")
db.executescript("""
CREATE TABLE users (id INTEGER PRIMARY KEY, email TEXT UNIQUE NOT NULL, role TEXT NOT NULL);
CREATE TABLE conversations (id INTEGER PRIMARY KEY, user_id INTEGER NOT NULL REFERENCES users(id), title TEXT);
CREATE TABLE messages (id INTEGER PRIMARY KEY,
                       conversation_id INTEGER NOT NULL REFERENCES conversations(id),
                       role TEXT NOT NULL CHECK (role IN ('user', 'assistant')),
                       content TEXT NOT NULL);
INSERT INTO users VALUES (7, 'an@uni.edu', 'student');
INSERT INTO conversations VALUES (42, 7, 'Refunds');
INSERT INTO messages (conversation_id, role, content) VALUES
  (42, 'user', 'Chính sách hoàn tiền thế nào?'),
  (42, 'assistant', 'Khách được hoàn tiền trong 30 ngày.');
""")
for row in db.execute("""SELECT c.title, m.role, m.content FROM messages m
                         JOIN conversations c ON c.id = m.conversation_id
                         WHERE c.user_id = 7 ORDER BY m.id"""):
    print(row)
```

Expected output:

```
('Refunds', 'user', 'Chính sách hoàn tiền thế nào?')
('Refunds', 'assistant', 'Khách được hoàn tiền trong 30 ngày.')
```

Then try to break it: insert a message with `role = 'admin'`. Which constraint stops you, and what error line does Python print?

**Key terms:** entity — *thực thể*; attribute — *thuộc tính*; relationship — *quan hệ*; cardinality — *bản số (số lượng quan hệ)*; normalisation — *chuẩn hóa*.

### Slide 17 — Vector stores: searching by meaning · [`#db-vector`](buoi-3.html#db-vector)

**Definitions.**
- **Embedding** — a fixed-length numeric vector produced by an *embedding model*, such that texts with similar meaning lie close together in vector space.
- **Vector store** (vector database) — a database that stores embeddings together with their source text and metadata, **indexes** them for fast approximate nearest-neighbour search (e.g. **HNSW**, Malkov & Yashunin, 2020) and returns the **k nearest** vectors to a query vector.
- **Cosine similarity** — `cos(a, b) = a·b / (‖a‖‖b‖)`, from −1 (opposite) through 0 (unrelated) to 1 (same direction).

**Why not just SQL?** A relational query finds rows by *exact values* (`WHERE title = 'Refunds'`). A user's question is free text in their own words; we need to find passages by *meaning*. That is what embeddings and similarity search do.

**The two pipelines.**

```mermaid
flowchart LR
    subgraph Indexing["1 · Indexing — offline, once per document"]
      D[Documents] --> C[Chunking] --> E1[Embedding model] --> V[Vectors + metadata] --> VS[(Vector store<br/>HNSW index)]
    end
    subgraph Query["2 · Query — online, every question"]
      Q[Question] --> E2[Same embedding model] --> QV[Query vector] --> S[Similarity search<br/>top-k] --> K[Top-k chunks] --> L[LLM → grounded answer]
    end
    VS -.-> S
```

This is **RAG — Retrieval-Augmented Generation** (Lewis et al., 2020): retrieve relevant passages first, then let the LLM generate an answer grounded in them.

**Practical notes.**
- **Chunking** matters as much as the model. Typical chunks are a few hundred tokens with some overlap, split on natural boundaries (headings, paragraphs).
- **Use the same embedding model** for indexing and querying; vectors from different models are not comparable.
- **Store metadata** (source file, page, date, access level) with each chunk — for citations and for filtering by permission.

**The Vietnamese example from the slide.**

| Text | Embedding (illustrative) | cos(q, ·) |
|---|---|---|
| **q:** "Mua rồi không ưng, tôi lấy lại tiền được không?" | `[0.10, −0.45, 0.30, …]` | — |
| "Khách được hoàn tiền trong vòng 30 ngày kể từ ngày mua." | `[0.12, −0.48, 0.33, …]` | **0.91 — top-1** |
| "Phí giao hàng nội thành là 20.000 đ/đơn." | `[0.41, 0.18, −0.09, …]` | 0.32 |
| "Cửa hàng mở cửa từ 8:00 đến 22:00 mỗi ngày." | `[−0.27, 0.05, 0.61, …]` | 0.11 |

The question says *"lấy lại tiền"* (get my money back) and never uses the words *"hoàn tiền"* (refund). A keyword search would miss the refund policy; a semantic search ranks it first because the meanings are close. The numbers are illustrative: real models produce hundreds to thousands of dimensions. For Vietnamese, use a model trained on multilingual or Vietnamese data — for example multilingual-E5, BGE-M3 or OpenAI's `text-embedding-3` family. Some Vietnamese-specific models (PhoBERT-based) expect word-segmented input; check the model card.

**Common tools (chapter 3 + current practice).**

| Tool | Type | Notes |
|---|---|---|
| **ChromaDB** | Open source, local | Simple; good for development and small production |
| **pgvector** | PostgreSQL extension | One database for relational data *and* vectors |
| **Pinecone** | Managed cloud service | Scales easily; paid |
| **Weaviate** | Open source, self-hosted or cloud | Feature-rich |
| **FAISS** | Library (Meta) | In-process similarity search; no server |
| **Qdrant, Milvus** | Open source vector databases | Production-grade alternatives |

Chapter 3's ChromaDB example (LangChain integration):

```python
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings

vectorstore = Chroma(
    collection_name="documents",
    embedding_function=OpenAIEmbeddings(),
    persist_directory="./data/chroma",
)
results = vectorstore.similarity_search("chính sách hoàn tiền", k=3)   # top-3 passages
```

**Self-check — compute cosine similarity by hand** (no install, three dimensions only):

```python
import math

def cos(a, b):
    dot = sum(x * y for x, y in zip(a, b))
    return dot / (math.sqrt(sum(x * x for x in a)) * math.sqrt(sum(y * y for y in b)))

q  = [0.10, -0.45, 0.30]
d1 = [0.12, -0.48, 0.33]
d2 = [0.41, 0.18, -0.09]
print(round(cos(q, d1), 3), round(cos(q, d2), 3))
```

Expected output: `1.0 -0.267`. With only three dimensions the vectors that point the same way score almost exactly 1; real embeddings spread meaning over hundreds of dimensions, so scores like 0.91 vs 0.32 are typical.

**Key terms:** embedding — *véc-tơ nhúng*; vector store — *kho véc-tơ*; similarity search — *tìm kiếm tương đồng*; chunking — *chia đoạn*; retrieval-augmented generation — *sinh câu trả lời có truy xuất*.

---

## Section 5 — AI Agent

### Slide 18 — From deep learning to agent frameworks · [`#ag-foundations`](buoi-3.html#ag-foundations)

Four ideas, each adding what the previous one lacks.

| | Definition | Strengths | Limitations |
|---|---|---|---|
| **Deep learning model** | A multi-layer neural network that learns hierarchical representations directly from data by gradient-based optimisation (LeCun, Bengio & Hinton, 2015; Goodfellow et al., 2016). | Learns features automatically; state of the art in vision, speech and language. | Needs large data and compute; usually trained for one task; hard to interpret. |
| **Large language model (LLM)** | A Transformer-based deep learning model (Vaswani et al., 2017) with billions of parameters, pre-trained on large text corpora to predict the next token, and adapted to many tasks through prompting (Brown et al., 2020). | General language understanding and generation; few-shot learning from examples in the prompt. | Hallucinates; knowledge cut-off; no tools, no actions and no memory of its own; cost and latency per call. |
| **LangChain** | An open-source framework that composes LLM calls with prompts, retrievers, tools and memory into reusable pipelines (*chains*). | Large integration ecosystem; quick RAG and tool use; standard interfaces make providers swappable. | Chains are linear: loops, branches and long-running state are hard to express; abstraction overhead. |
| **LangGraph** | A low-level orchestration library that models an agent as a **stateful directed graph**: nodes are steps, edges are transitions (conditional and cyclic), and a shared state persists across steps. | Explicit loops and branches; persistence (checkpoints); human-in-the-loop; streaming; easier to debug. | More design up front; steeper learning curve; overkill for one-shot tasks. |

**What each step adds.** DL → LLM: *scale and generality*. LLM → LangChain: *tools, retrieval and composition*. LangChain → LangGraph: *control flow and state*.

**Chain vs state machine (chapter 3).**
- **Chain** — Input → A → B → C → Output. Always the same order; no branching, no loops, no conditions. Fits simple tasks: "translate this sentence", "summarise this paragraph".
- **State machine** — Input → State A → (condition?) → State B or C → (need more information?) → back to A → … → Output. Branches on conditions, loops, can pause for input. Fits real tasks: "answer the user; if more information is needed, ask; if a search is needed, use a tool; if the answer is not good enough, think again".

Most real agent applications need a state machine. The repeated *think → act → observe* cycle is the **agentic loop**.

**Note on versions.** These frameworks evolve quickly; learn their current API from the official documentation (`github.com/langchain-ai/langchain`, `github.com/langchain-ai/langgraph`) rather than from old tutorials.

**Self-check.** Chapter 3, review question 2: *Explain the difference between a LangChain chain (linear) and a LangGraph state machine. Give a concrete situation where a simple chain is not enough and a state machine is necessary. Draw or describe the diagram.*

### Slide 19 — LangGraph: agents as stateful directed graphs · [`#ag-graph`](buoi-3.html#ag-graph)

**Formal model.** An agent is a directed graph **G = (V, E)** over a shared **state S**.
- Each **node** `v ∈ V` is a function `S → ΔS`: it reads the state and returns an *update* (only the keys it changed).
- Each **edge** `(u, v) ∈ E` is a transition: **fixed** (`add_edge`) or **conditional** on the state (`add_conditional_edges`).
- **Reducers** define how an update is merged into S — overwrite by default, or e.g. append (`Annotated[list, operator.add]`).
- **Cycles** are allowed: the graph runs until a path reaches `END`. Always bound loops (a counter in the state, or LangGraph's recursion limit).

**The reasoning loop on the slide.**

```mermaid
flowchart TD
    START([START]) --> R[REASON<br/>LLM decides the next step]
    R --> D{tool call?}
    D -- yes --> A[ACT<br/>search · DB query · API]
    A --> O[OBSERVE<br/>add result to state]
    O --> R
    D -- no --> E[EVALUATE<br/>good enough?]
    E -- pass --> END([END])
    E -. fail: reflect, retry .-> R
```

**Recent techniques built on the graph** — vocabulary mentors will meet; most projects need only the first.

| Technique | Idea | Source |
|---|---|---|
| ReAct / tool calling | Interleave reasoning and actions: reason → act → observe → … | Yao et al. (2023) |
| Reflection | Critique the draft, then retry with the critique | Shinn et al. (2023), *Reflexion* |
| Plan-and-execute | A planner node writes a plan; worker nodes execute steps | — |
| Persistence | A **checkpointer** saves S after every step per *thread*: resume, replay, "time travel" | LangGraph docs |
| Human-in-the-loop | **Interrupt** the graph to ask a human before a risky action, then resume | LangGraph docs |
| Fan-out & subgraphs | Run branches in parallel; nest graphs; multi-agent teams | LangGraph docs |

**Chapter 3's key point:** LangGraph's power is *conditional flow and loops*. An agent does not just run A → B → C; it can go back for more information, branch by question type, and stop when the answer is good enough. That is the difference between a simple chatbot and a real agent.

**Self-check.** For your project's agent, which of the six techniques does it actually need, and why? If the answer is "none", say what the agent loop looks like without them.

### Slide 20 — Worked example: a routing agent · [`#ag-example`](buoi-3.html#ag-example)

The router example from chapter 3, read in three parts: **state → nodes → edges**.

```python
# Chapter 3 (shortened). llm and search_tool are defined elsewhere.
import operator
from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, START, END

# 1 · STATE — the data every node shares
class AgentState(TypedDict):
    question: str
    context: Annotated[list[str], operator.add]   # reducer: append, don't overwrite
    needs_search: bool
    answer: str

# 2 · NODES — each takes the state and returns an update
def router(state):                                # decide the path
    label = llm.invoke(f"Classify as simple|search: {state['question']}")
    return {"needs_search": "search" in label.content}

def search(state):                                # act: call a tool
    return {"context": [search_tool.invoke(state["question"])]}

def generate(state):                              # answer from collected context
    ctx = "\n".join(state["context"])
    return {"answer": llm.invoke(f"{ctx}\n\nQ: {state['question']}").content}

# 3 · EDGES — wire the nodes, then compile
g = StateGraph(AgentState)
g.add_node("router", router)
g.add_node("search", search)
g.add_node("generate", generate)
g.add_edge(START, "router")
g.add_conditional_edges("router", lambda s: "search" if s["needs_search"] else "generate")
g.add_edge("search", "generate")
g.add_edge("generate", END)
agent = g.compile()
```

**Line by line.**
- `TypedDict` declares the state's keys and types. `Annotated[list[str], operator.add]` attaches a **reducer**: when a node returns `{"context": [x]}`, LangGraph *appends* `x` to the existing list instead of replacing it.
- Every node is an ordinary function. It receives the **whole** state and returns a **small dict** with only the changed keys; LangGraph merges it.
- `add_edge(START, "router")` sets the entry point. `add_conditional_edges` takes a function of the state that returns the name of the next node. `compile()` validates the graph and returns a runnable object with `invoke`, `ainvoke` and `stream`.

**Two traced runs.**

| Run | Path | State changes | Calls |
|---|---|---|---|
| A · "Xin chào!" | router → generate → END | `needs_search = False`; `answer = …` | 2 LLM, 0 tools |
| B · "Thời tiết Hà Nội hôm nay?" | router → search → generate → END | `needs_search = True`; `context = ["Hà Nội: 27°C, mưa nhẹ…"]`; `answer = …` | 2 LLM, 1 tool |

Same graph, different paths — decided at run time by the state.

**Turning it into a loop.** Add an edge from `generate` (or a new `evaluate` node) back to `router`, and a counter in the state (`tool_calls`) that forces `END` after N iterations. Chapter 3's full state includes exactly such a field: `tool_calls: int  # number of tool calls (limit)`.

**Self-check — run the graph without an API key.** Replace the LLM and the tool with stubs:

```python
# graph_demo.py — run: python -X utf8 graph_demo.py     (pip install langgraph)
import operator
from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, START, END

class AgentState(TypedDict):
    question: str
    context: Annotated[list[str], operator.add]
    needs_search: bool
    answer: str

def router(state):
    return {"needs_search": "thời tiết" in state["question"].lower()}   # stub for the LLM

def search(state):
    return {"context": ["Hà Nội: 27°C, mưa nhẹ"]}                       # stub for the tool

def generate(state):
    ctx = "; ".join(state["context"]) or "no context"
    return {"answer": f"[{ctx}] → answer to: {state['question']}"}

g = StateGraph(AgentState)
g.add_node("router", router)
g.add_node("search", search)
g.add_node("generate", generate)
g.add_edge(START, "router")
g.add_conditional_edges("router", lambda s: "search" if s["needs_search"] else "generate")
g.add_edge("search", "generate")
g.add_edge("generate", END)
agent = g.compile()

for q in ["Xin chào!", "Thời tiết Hà Nội hôm nay?"]:
    print(agent.invoke({"question": q, "context": []})["answer"])
print(agent.get_graph().draw_mermaid())          # the graph as a Mermaid diagram
```

Expected first two lines:

```
[no context] → answer to: Xin chào!
[Hà Nội: 27°C, mưa nhẹ] → answer to: Thời tiết Hà Nội hôm nay?
```

The last line prints Mermaid text: paste it into any Mermaid viewer (or a GitHub Markdown file) to see the graph — an *agent flow diagram* generated from the code itself.

**Key terms:** state — *trạng thái*; node — *nút*; edge — *cạnh*; conditional edge — *cạnh có điều kiện*; reducer — *hàm gộp trạng thái*; agentic loop — *vòng lặp tác tử*; checkpoint — *điểm lưu*.

---

## Section 6 — DevOps

### Slide 21 — DevOps: build, ship and run the system · [`#devops`](buoi-3.html#devops)

**Definition.** **DevOps** is a set of cultural practices, processes and tools that unify software **development** (Dev) and IT **operations** (Ops), so that changes are delivered **quickly, safely and repeatably**, with continuous feedback from production (Kim et al., 2016; Forsgren et al., 2018).

**The lifecycle loop.** Dev: *plan → code → build → test*; Ops: *release → deploy → operate → monitor*; monitoring feeds the next plan, so the loop never ends.

**Core practices.**

| Practice | What it means | Typical tool |
|---|---|---|
| Continuous Integration (CI) | Every change is built and tested automatically on every push | GitHub Actions, GitLab CI |
| Continuous Delivery / Deployment (CD) | Tested changes are packaged and released automatically (to staging or production) | GitHub Actions, AWS CodeDeploy |
| Containers | The application and its dependencies are packed into one **image** that runs identically everywhere | Docker, Docker Compose |
| Infrastructure as Code (IaC) | Servers, networks and databases are defined in versioned files | Terraform, AWS CloudFormation |
| Observability | Logs, metrics and traces; alerts on failure | CloudWatch, Prometheus, Grafana |
| Secrets management | Keys and passwords live in environment variables or a secret store, never in Git | `.env` (local), AWS Secrets Manager |

**DORA metrics** (Forsgren, Humble & Kim, *Accelerate*, 2018) — the standard measures of delivery performance: **deployment frequency**, **lead time for changes**, **change failure rate**, **time to restore service**.

**The AWS figure — a multi-service system, one request at a time.**

| Service | Role in the figure |
|---|---|
| **Route 53** | DNS: turns `app.example.com` into an address |
| **VPC** | A private network isolating the servers and the database |
| **Application Load Balancer (ALB)** | Spreads incoming traffic across healthy servers |
| **EC2** (in an **Auto Scaling group**) | Virtual servers running the Docker image (FastAPI + agent); instances are added when load grows |
| **RDS** | Managed PostgreSQL: backups, patches, failover across availability zones (Multi-AZ) |
| **S3** | Object storage: uploaded files, documents, static front-end assets |
| **ECR** | Registry of container images |
| **CloudWatch** | Logs, metrics, alarms |
| **GitHub Actions** (outside AWS) | CI/CD: test → build image → push to ECR → deploy |
| **LLM API** (outside AWS) | External provider called by the agent |

How the five blocks map onto it: **back-end and agent** run on EC2; the **database** is RDS (records) and S3 (files); **DevOps** is everything that builds, deploys and watches them.

#### Deep dive — the minimum DevOps for a student project

A container image definition:

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY src/ src/
EXPOSE 8000
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

A CI workflow that runs the tests on every push:

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: "3.11" }
      - run: pip install -r requirements.txt
      - run: pytest -q
```

Chapter 3's deployment diagram puts the app container, a PostgreSQL container and a ChromaDB container in one Docker Compose file, with GitHub Actions deploying and the OpenAI API outside.

**Key terms:** continuous integration — *tích hợp liên tục*; continuous delivery — *chuyển giao liên tục*; container — *container (gói chạy độc lập)*; infrastructure as code — *hạ tầng dưới dạng mã*; observability — *khả năng quan sát*; load balancer — *bộ cân bằng tải*.

**Self-check.** Answer for your project: (1) What command builds your image? (2) What runs automatically on every push? (3) Where would you look first if the app stopped answering at 2 a.m.?

---

## Section 7 — Architecture Diagram

### Slide 22 — Architecture diagrams: one picture, one question · [`#diag-concepts`](buoi-3.html#diag-concepts)

**Definition.** An **architecture diagram** is a visual model of a system's components, their responsibilities, and the connections and data flows between them, **at a chosen level of abstraction**. Chapter 3 calls it the most important communication tool in a software project: it helps teammates understand the system, helps mentors evaluate the design, and helps the author think clearly.

**Why Mermaid (chapter 3).** Mermaid is a text-based diagram language. Instead of drawing in a GUI tool and exporting a PNG, you write the diagram as text, commit it with the code, and GitHub renders it automatically.
- **Version-control friendly** — diffs and merges like code.
- **Renders on GitHub** and in many Markdown editors.
- **Lives next to the code** it describes.
- **Easy to update** — edit text, no redrawing.

**The three diagram types you need (chapter 3).**

**1 · System overview** — *what exists, and what talks to what?*

```mermaid
graph TB
    subgraph Client
        Browser[Web Browser]
        Mobile[Mobile App]
    end
    subgraph "FastAPI Backend"
        API[API Gateway]
        Auth[Auth Module]
        SessionMgr[Session Manager]
        ChatHandler[Chat Handler]
    end
    subgraph "AI Agent (LangGraph)"
        AgentOrchestrator[Agent Orchestrator]
        Router[Intent Router]
        SearchTool[Web Search]
        DBTool[DB Query]
        MemoryMgr[Memory Manager]
    end
    subgraph "External Services"
        OpenAI[OpenAI API]
        Tavily[Tavily Search]
    end
    subgraph "Data Layer"
        SQLite[(SQLite DB)]
        Chroma[(ChromaDB)]
    end
    Browser --> API
    Mobile --> API
    API --> Auth
    API --> ChatHandler
    ChatHandler --> AgentOrchestrator
    AgentOrchestrator --> Router
    Router --> SearchTool
    Router --> DBTool
    SearchTool --> Tavily
    Router --> OpenAI
    AgentOrchestrator --> MemoryMgr
    MemoryMgr --> SQLite
    AgentOrchestrator --> Chroma
```

**2 · Agent flow** — *how does the agent decide and loop?* This is where the **agentic loop** becomes visible; it is the main difference between an agent and a simple chain.

```mermaid
graph TD
    START([User Question]) --> Router[Intent Router]
    Router -->|Greeting| Direct[Direct Response]
    Router -->|Factual Q| Retrieval[RAG Retrieval]
    Router -->|Current Info| Search[Web Search Tool]
    Retrieval --> Grade[Grade Documents]
    Grade -->|Relevant| Generate[Generate Answer]
    Grade -->|Not Relevant| Rewrite[Rewrite Query]
    Rewrite --> Retrieval
    Search --> Generate
    Direct --> Generate
    Generate --> Check[Hallucination Check]
    Check -->|Pass| Output([Final Answer])
    Check -->|Fail| Generate
```

**3 · Deployment** — *where does each part run?*

```mermaid
graph LR
    subgraph "Docker Compose"
        subgraph "App Container"
            FastAPI[FastAPI Server]
            Agent[LangGraph Agent]
        end
        subgraph "DB Container"
            PG[(PostgreSQL)]
        end
        subgraph "Vector Container"
            Chroma[(ChromaDB)]
        end
    end
    subgraph External
        GitHub[GitHub Actions CI/CD]
        OpenAI[OpenAI API]
    end
    Internet((Internet)) --> FastAPI
    FastAPI --> PG
    FastAPI --> Chroma
    Agent --> OpenAI
    GitHub -->|Deploy| FastAPI
```

**Levels of abstraction — the C4 model** (Simon Brown, c4model.com):

| Level | Shows | Audience |
|---|---|---|
| 1 · Context | The system as one box, its users and the external systems it talks to | Everyone |
| 2 · Containers | The deployable/runnable parts: web app, API, database, vector store | Technical team, mentors |
| 3 · Components | Modules inside one container: router, auth, services | Developers of that container |
| 4 · Code | Classes and functions | Usually generated, rarely drawn |

Keep each diagram at **one** level. Mixing "the whole system" and "this one class" in the same picture is the most common reason a diagram is unreadable.

**Five rules for a good diagram (chapter 3).**
1. **Clear names** — every node describes its function; never "Service A", "Module 1".
2. **Group with subgraphs** — Frontend, Backend, Agent, External, Data; the boundaries become obvious.
3. **Mark the direction of data** — clear arrows; for two-way communication, label what flows each way.
4. **Number sequential flows** — (1), (2), (3)… on edges so the reader follows the order.
5. **Keep it simple** — one idea per diagram; make several diagrams instead of one crowded one.

> **Chapter 3 warning:** a diagram is not decoration — it is technical documentation. The team refers to it while coding, debugging and onboarding. It must be updated when the architecture changes. A diagram that does not match the code is worthless — or worse, misleading.

### Slide 23 — A reference architecture for an AI-agent application · [`#diag-reference`](buoi-3.html#diag-reference)

The standard picture: chapter 3's overview with the two supporting blocks.

```mermaid
flowchart LR
    subgraph FE["1 · Front-End (React / Next.js · Streamlit)"]
        Chat[Chat UI]
        Dash[Dashboard]
        Login[Login]
    end
    subgraph BE["2 · Back-End (FastAPI)"]
        Router[API router + validation]
        Auth[Auth: 401 / 403]
        Svc[Services: business rules]
        Hist[Session & history]
        Err[Error handler]
        Log[Logging · rate limiting]
    end
    subgraph AG["3 · AI Agent (LangGraph)"]
        Orch[Graph orchestrator]
        IR[Intent router]
        Tools[Tools: search · DB · retriever]
        Mem[Memory]
        Eval[Evaluator]
    end
    subgraph EXT["External"]
        LLM[LLM provider]
        Web[Search / web APIs]
    end
    subgraph DATA["4 · Data"]
        PG[(PostgreSQL)]
        VS[(Vector store)]
        Cache[(Cache · Redis)]
        S3[(Object storage · S3)]
    end
    FE <-->|"1 HTTPS · JSON / 7 SSE stream"| BE
    BE <-->|"4 invoke / stream"| AG
    AG <-->|6| EXT
    BE <-->|"3 history / 8 save, log"| DATA
    AG <-->|"5 retrieve context"| VS
```

**The eight-step request flow.**
1. The user sends a message from the chat UI over HTTPS.
2. The back-end validates the request and checks **authentication and authorisation** — the Auth box is where permission rules belong.
3. It loads the conversation history from PostgreSQL.
4. It invokes the agent.
5. The agent retrieves relevant context from the vector store.
6. The agent calls the LLM and any tools.
7. The answer streams back through the back-end to the chat UI.
8. The back-end saves the new messages and writes logs.

Everything rests on the **DevOps** layer: Docker containers, CI/CD (GitHub Actions), cloud hosting (AWS EC2, RDS, S3), monitoring (CloudWatch) and secrets in environment variables.

**Three structural points to check in any team's diagram.**
- The front-end talks **only** to the back-end.
- The agent reaches the outside world (LLM, search, other APIs) **only through its tools**.
- Every box is **needed**: a small project may have no cache, no object storage and no evaluator. Each box should exist because the project needs it — and the reason belongs in an ADR.

**Self-check.** Chapter 3, end-of-chapter outcome: *you should have (1) an architecture diagram for your team's project, (2) at least 2–3 ADRs for important decisions, and (3) a clear understanding of how the components communicate.* Draw your project's overview diagram in Mermaid; for every box, write the file, folder or service that implements it — or "not yet".

### Deep dive — Architecture Decision Records (ADR)

An **ADR** is a short document recording an important architectural decision: the **context**, the **alternatives** considered, the **decision** and its **rationale**, plus the **consequences** (Nygard, 2011). Chapter 3 treats it as a thinking tool as much as documentation: if you cannot write the rationale, the decision is not ready.

**Why.** Teams make many decisions — SQLite or PostgreSQL, LangGraph or LangChain, streaming or not, ChromaDB or Pinecone. Without a record, nobody remembers *why* three months later, and nobody knows whether the original reason still holds.

**Template (chapter 3).**

```markdown
# ADR-001: [Decision title]

**Date:** YYYY-MM-DD
**Status:** Accepted / Deprecated / Superseded by ADR-XXX

## Context
The problem or situation that forces a decision.

## Alternatives
### Option 1: …
- Pros: …
- Cons: …
### Option 2: …
- Pros: …
- Cons: …

## Decision
Option N.

## Rationale
1. …
2. …

## Consequences
- What becomes easier, what becomes harder, what we must now watch.
```

**Chapter 3's example (shortened):** *ADR-002 — SQLite for development, PostgreSQL for production.* Context: the agent must store chat history for multi-turn conversation. Alternatives: SQLite everywhere (simple, doesn't scale); PostgreSQL everywhere (strong, heavy for development); SQLite in dev + PostgreSQL in prod. Decision: the third. Rationale: no local PostgreSQL install; SQLAlchemy hides the differences; only `DATABASE_URL` changes. Consequences: test on both; migrations must work on both.

**Four rules (chapter 3).**
1. Record **important** decisions only — framework, database, LLM provider, architecture pattern, deployment strategy; not indentation style.
2. Show the **trade-offs**. There is no perfect solution, only the best fit for the context.
3. **Update** when a decision changes: mark the old ADR *Superseded by ADR-XXX* and write a new one.
4. Keep it **short** — readable in 3–5 minutes.

**Self-check.** Chapter 3, review question 3: *Write an ADR for "Should the agent chat endpoint use streaming (SSE) or a non-streaming response?" Analyse at least two options with pros and cons, and give a reasoned decision.*

---

## Summary, Q&A, References

### Slide 24 — Seven ideas to take away · [`#summary`](buoi-3.html#summary)

1. **Five blocks**, each with one responsibility, connected by API contracts — the agent never faces users directly.
2. **Front-end:** chat for asking, dashboards for watching; judge it by correctness, speed, usability and accessibility.
3. **Back-end:** validate → authenticate → authorise → run logic → respond; async so the LLM never blocks the server.
4. **Database:** SQL for records (design it with an ERD), vector stores for meaning; add only what you need.
5. **Agent:** a stateful directed graph — nodes, edges, state — that can branch and loop, unlike a chain.
6. **DevOps:** build, test, ship and watch every block automatically — containers, CI/CD, cloud, monitoring.
7. **Diagrams** are documentation: one question per picture, kept in Git, always matching the code.

**Closing exercise (Q&A).** Draw your project's five blocks. For each box, say which file or service it is — or write "not yet".

### Review questions (from chapter 3)

1. In a three-tier architecture, why does the AI agent not communicate directly with the front-end but go through the back-end? Give at least three technical reasons and explain the benefit of each.
2. Explain the difference between a LangChain chain (linear) and a LangGraph state machine. Give a concrete situation where a simple chain is not enough and a state machine is necessary. Draw or describe the diagram.
3. Write an ADR for: "Should the agent chat endpoint use streaming (SSE) or non-streaming responses?" Analyse at least two options, list pros and cons, and give a reasoned decision.

### References

**In this repository**
- Phoenix Agent Technical Book, Chapter 3 — [`docs/org/chapter-03-thiet-ke-kien-truc.md`](../../org/chapter-03-thiet-ke-kien-truc.md)
- Course overview — [`docs/giao-trinh/00-README.md`](../00-README.md) · Project brief — [`docs/de-bai.md`](../../de-bai.md)

**Software architecture**
- Dijkstra, E. W. (1974). *On the role of scientific thought* (EWD447).
- Parnas, D. L. (1972). On the criteria to be used in decomposing systems into modules. *Communications of the ACM*, 15(12), 1053–1058.
- Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly.
- Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures*. PhD dissertation, University of California, Irvine.
- Nygard, M. (2011). *Documenting Architecture Decisions*. cognitect.com/blog.
- Brown, S. *The C4 model for visualising software architecture*. c4model.com.

**Data**
- Codd, E. F. (1970). A relational model of data for large shared data banks. *Communications of the ACM*, 13(6), 377–387.
- Chen, P. P. (1976). The entity-relationship model — toward a unified view of data. *ACM Transactions on Database Systems*, 1(1), 9–36.
- Malkov, Y. A., & Yashunin, D. A. (2020). Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs. *IEEE TPAMI*, 42(4), 824–836.
- Lewis, P. et al. (2020). Retrieval-augmented generation for knowledge-intensive NLP tasks. *NeurIPS*.

**AI and agents**
- LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. *Nature*, 521, 436–444.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press.
- Vaswani, A. et al. (2017). Attention is all you need. *NeurIPS*.
- Brown, T. et al. (2020). Language models are few-shot learners. *NeurIPS*.
- Yao, S. et al. (2023). ReAct: Synergizing reasoning and acting in language models. *ICLR*.
- Shinn, N. et al. (2023). Reflexion: Language agents with verbal reinforcement learning. *NeurIPS*.

**DevOps**
- Kim, G., Humble, J., Debois, P., & Willis, J. (2016). *The DevOps Handbook*. IT Revolution.
- Forsgren, N., Humble, J., & Kim, G. (2018). *Accelerate: The Science of Lean Software and DevOps*. IT Revolution.

**Official documentation**
- FastAPI — fastapi.tiangolo.com · Pydantic — docs.pydantic.dev · Starlette — starlette.io
- React — react.dev · Next.js — nextjs.org/docs · Streamlit — docs.streamlit.io
- LangChain — github.com/langchain-ai/langchain · LangGraph — github.com/langchain-ai/langgraph
- PostgreSQL — postgresql.org/docs · SQLite — sqlite.org/docs.html · pgvector — github.com/pgvector/pgvector · Chroma — docs.trychroma.com
- AWS — docs.aws.amazon.com · Docker — docs.docker.com · GitHub Actions — docs.github.com/actions
- HTTP Semantics — RFC 9110 · Core Web Vitals — web.dev/vitals · WCAG 2.2 — w3.org/TR/WCAG22 · Mermaid — mermaid.js.org

---

## Appendix A — Applying this to the MT-01 repository

The slides are deliberately general. This appendix maps the lecture onto this repository's template so mentors can check a team's work against real files. **Run each command yourself** and compare the output with the expectation; the template may have changed since this note was written.

| Block | Where it lives in the template | Check (run from the repo root) |
|---|---|---|
| Front-End | Not in the template yet | `ls` — no front-end folder is expected at this stage |
| Back-End | `src/main.py` (app, CORS, `/health`), `src/api/routes.py` (endpoints), `src/models/schemas.py` (Pydantic models), `src/config.py` | `grep -n "@router\|@app" src/main.py src/api/routes.py` |
| AI Agent | `src/agents/` — `graph.py`, `state.py`, `nodes/`, `tools/` | `grep -n "add_node\|add_edge\|add_conditional_edges" src/agents/graph.py` |
| Database | `DATABASE_URL` in `src/config.py`; drivers in `requirements.txt` | `grep -n "sqlalchemy\|chromadb" requirements.txt` |
| DevOps | `Dockerfile`, `docker-compose.yml`, `.github/workflows/` | `cat docker-compose.yml` · `ls .github/workflows` |
| Diagram | `ARCHITECTURE.md` (Mermaid) | `grep -n "subgraph" ARCHITECTURE.md` |

**Three places where the template differs from chapter 3 — worth discussing with teams.**

1. **Where the agent sits in the diagram.** Chapter 3 treats the agent as its own tier; check whether `ARCHITECTURE.md` draws it as a separate subgraph or inside the back-end. Either can be defended — but the diagram and the code layout should agree.
   `grep -n "subgraph" ARCHITECTURE.md`
2. **Error detail sent to the client.** Chapter 3 says never return internal error messages to users. Look for the anti-pattern from slide 14:
   `grep -n "detail=str(e)" src/api/routes.py`
3. **Database and vector store not yet wired in.** Check whether the drivers are still commented out:
   `grep -n "sqlalchemy\|chromadb" requirements.txt`

**Self-check against the running skeleton** (from Session 02 — start the server first, e.g. with `make run`):

```bash
curl -s localhost:8000/health                                        # expect a JSON status
curl -s -o /dev/null -w "%{http_code}\n" -X POST localhost:8000/api/v1/chat \
     -H "Content-Type: application/json" -d '{"message": ""}'       # expect 422 (min_length=1)
```

Then open `http://localhost:8000/docs` and find the request and response schemas of `/api/v1/chat`. They come straight from the Pydantic classes in `src/models/schemas.py`.

What this appendix intentionally does **not** contain: code for authentication or role checks (a later session's exercise), and a "correct" MT-01 architecture diagram — drawing it is the team's own deliverable.

---

## Appendix B — Glossary (English — Vietnamese)

| English | Tiếng Việt | Where |
|---|---|---|
| system design / architecture | thiết kế hệ thống / kiến trúc | §1 |
| three-tier architecture | kiến trúc ba tầng | §1 |
| front-end / back-end | phần giao diện (phía người dùng) / phần máy chủ | §1 |
| AI agent | tác tử AI | §1, §5 |
| separation of concerns | tách biệt trách nhiệm | §1 |
| scalability | khả năng mở rộng | §1 |
| bottleneck | điểm nghẽn | §1 |
| loose coupling | liên kết lỏng | §1 |
| user interface (UI) / user experience (UX) | giao diện người dùng / trải nghiệm người dùng | §2 |
| component | thành phần (giao diện) | §2 |
| server-side / client-side rendering | dựng trang phía máy chủ / phía trình duyệt | §2 |
| streaming response | phản hồi dạng luồng | §2, §3 |
| accessibility | khả năng tiếp cận | §2 |
| API (application programming interface) | giao diện lập trình ứng dụng | §3 |
| endpoint | điểm cuối API | §3 |
| request / response | yêu cầu / phản hồi | §3 |
| status code | mã trạng thái | §3 |
| authentication / authorisation | xác thực / phân quyền | §3 |
| business logic | nghiệp vụ | §3 |
| validation | kiểm tra hợp lệ | §3 |
| dependency injection | tiêm phụ thuộc | §3 |
| asynchronous (async/await) | bất đồng bộ | §3 |
| error handling | xử lý lỗi | §3 |
| logging / monitoring | ghi log / giám sát | §3, §6 |
| structured / unstructured data | dữ liệu có cấu trúc / phi cấu trúc | §4 |
| relational database | cơ sở dữ liệu quan hệ | §4 |
| primary key / foreign key | khóa chính / khóa ngoại | §4 |
| transaction | giao dịch | §4 |
| entity-relationship diagram (ERD) | sơ đồ thực thể – quan hệ | §4 |
| cardinality | bản số | §4 |
| normalisation | chuẩn hóa | §4 |
| embedding | véc-tơ nhúng | §4 |
| vector store | kho véc-tơ | §4 |
| similarity search | tìm kiếm tương đồng | §4 |
| retrieval-augmented generation (RAG) | sinh câu trả lời có truy xuất | §4 |
| deep learning | học sâu | §5 |
| large language model (LLM) | mô hình ngôn ngữ lớn | §5 |
| hallucination | ảo giác (bịa thông tin) | §5 |
| chain | chuỗi | §5 |
| state machine | máy trạng thái | §5 |
| directed graph | đồ thị có hướng | §5 |
| state / node / edge | trạng thái / nút / cạnh | §5 |
| conditional edge | cạnh có điều kiện | §5 |
| agentic loop | vòng lặp tác tử | §5 |
| human-in-the-loop | có con người tham gia | §5 |
| continuous integration / delivery (CI/CD) | tích hợp / chuyển giao liên tục | §6 |
| container | container (gói chạy độc lập) | §6 |
| infrastructure as code | hạ tầng dưới dạng mã | §6 |
| load balancer | bộ cân bằng tải | §6 |
| observability | khả năng quan sát | §6 |
| architecture diagram | sơ đồ kiến trúc | §7 |
| level of abstraction | mức trừu tượng | §7 |
| architecture decision record (ADR) | bản ghi quyết định kiến trúc | §7 |
