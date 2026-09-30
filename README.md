# techhelp.help

A plain-language technology help site for older adults. Ask a question the way you'd ask a
person: "how do I find my phone number," "how do I make the text bigger" and get a short,
numbered set of steps with no jargon.

**Live:** [techhelp.help](https://techhelp.help)

## Why

I volunteer at the Belmont Public Library's senior tech sessions, helping older adults with
one-on-one technology questions. The same problems came up over and over, and every help
article I tried to point people to had the same flaw: it assumed knowledge they didn't have
and used vocabulary they'd never seen. The instructions were correct and useless.

techhelp.help is an attempt to fix that specific failure, providing answers written for someone who
is not sure what a "menu bar" is and is worried about breaking something.

## Stack

| Layer | Choice |
|---|---|
| Framework | Next.js 15 (App Router) on Vercel |
| Language | TypeScript |
| UI | React 18, Tailwind CSS, Framer Motion |
| AI | Anthropic API via `@anthropic-ai/sdk`, with response streaming |
| Chat | Stream (`stream-chat`, `stream-chat-react`) for real-time chat and conversation persistence |
| Tooling | pnpm, ESLint |

## Design decisions

**Next.js server functions instead of a separate backend.** I learned Node.js and Postgres
first and planned to build my own API server with my own conversation storage. I dropped
both. The one hard requirement was keeping the Anthropic API key off the client, and
Next.js server-side API routes handle that on their own. Conversation storage and real-time
delivery are solved problems, so Stream handles them. A standalone server and a database
would have been deployment and maintenance work that bought nothing the product needed yet.

**Anthropic over the alternatives.** I compared Anthropic, OpenAI, and Perplexity on how
reliably each produced clear, ordered instructions for a non-technical reader, and on cost
per query. Anthropic gave the most consistently followable answers for this audience.

**Prompt design was the hard part of the project.** A confidently wrong instruction is worse
than no instruction for someone already nervous about their computer. The system
prompt constrains the model to a small number of steps, non-technical vocabulary, and getting to the
actual problem quickly rather than explaining background.

**Accessibility drove the interface.** Large default type, high contrast, generous tap
targets, and one clear action per screen. The layout is deliberately sparse so the target audience 
will not get lost. 

**Persistent steps alongside the chat.** This came from watching people use it, not from
planning. Chat alone didn't work: users scrolled back to re-read step 2, lost their place,
and gave up. The generated instructions now stay on screen next to the conversation.

## Build process

1. **Framing and stack selection (2 months).** Problem grounded in two years of volunteering.
   Compared frameworks and AI providers, and de-risked the framework choice by building and
   deploying a smaller practice site end-to-end first.
2. **User flows and MVP scope (1 month).** Wireframed in Balsamiq, paper-prototyped the user
   journey with physical cut-outs, and tested navigation with non-technical users before
   writing code. MVP: a home page with one clear call to action, the chat interface, and
   jargon-free answers. Account login and saved history deliberately deferred.
3. **Implementation (4 months).** Used Cursor to scaffold features against the MVP spec, then
   reviewed, debugged, and refactored the generated code. GitHub connected to Vercel for
   deployment on merge.
4. **Testing and release (3 months).** Tested every version manually and across 8 sessions
   with older adults outside my family, turning each round of feedback into a prioritized fix
   list before the next release. Launched on the techhelp.help domain.

Built solo over 10 months.

## Running locally

```bash
pnpm install
cp .env.example .env.local   # add your Anthropic and Stream keys
pnpm dev
```

Open http://localhost:3000.

## Author

Hugo Clavel — [techhelp.help](https://techhelp.help)