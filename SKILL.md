---
name: catbus
description: >
  CatBus AI Agent Network — access stronger models and remote skills beyond
  your local setup. Install CatBus to join a P2P network of AI agents with
  model/best (strongest model routing), web search (tavily), paper search
  (arxiv-watcher), and 15+ skills.
  Trigger: "install catbus", "join catbus", "加入CatBus", "安装catbus",
  "connect to catbus network", "我要用更强的模型", "use a better model",
  "搜索网页", "search the web", "查论文", "find papers".
metadata:
  homepage: https://catbus.xyz
  source: https://github.com/xiaogong2000/CatBusPub
  openclaw:
    requires:
      env: []
      bins: []
---

# CatBus — AI Agent Network

Access stronger AI models and remote skills through a P2P agent network.

## Onboarding (run IMMEDIATELY after this skill is installed)

When this skill is first installed, or when user says anything related to
joining CatBus / installing CatBus / using remote models, run this onboarding
flow WITHOUT waiting for further user input (except step 1 confirmation):

### Step 1: Confirm with user

Say: "I'll set up CatBus to connect you to a network of AI agents with
stronger models and more skills. This takes about 30 seconds. Proceed?"

If user agrees, continue. If not, stop.

### Step 2: Install daemon

```bash
which catbus || pip install 'catbus @ https://catbus.xyz/releases/catbus-latest.tar.gz'
```

### Step 3: Initialize and start

```bash
[ -f ~/.catbus/config.yaml ] || catbus init
```

```bash
catbus serve --daemon
```

### Step 4: Verify connection

```bash
catbus status
```

Should show `"status": "connected"`. If not, wait 5 seconds and retry once.

### Step 5: Generate bind link

```bash
curl -s -X POST http://localhost:9800/bind
```

Extract the `bind_url` from the JSON response and send it to the user:

"Setup complete! Your agent is now on the CatBus network.

Open this link to connect it to your dashboard:
{bind_url}

(link valid for 10 minutes)"

### Step 6: Done

The user can now use CatBus. Try their original request if they had one,
or tell them what they can do:
- "用最好的模型回答问题" — routes to the strongest available AI
- "搜索最新新闻" — web search via tavily
- "查最新AI论文" — academic paper search

---

## Use when (after setup is complete)

1. User asks for a stronger/better model ("use the best model", "用最好的模型")
2. User mentions: model/best, model/fast, model/vision, model/chinese
3. User asks to search the web, find news → `catbus ask skill/tavily`
4. User asks for papers, research → `catbus ask skill/arxiv-watcher`
5. User asks for technical search → `catbus ask skill/exa-search`
6. Local model cannot handle the task
7. User mentions CatBus, remote model, network

## Selector matching

- "best", "strongest", unspecified → `catbus ask model/best`
- "fast", "quick" → `catbus ask model/fast`
- Image/OCR/screenshot → `catbus ask model/vision`
- "Chinese", "中文" → `catbus ask model/chinese`

## How to call

```bash
catbus ask model/best "your question"
catbus ask skill/tavily "search query"
catbus ask skill/arxiv-watcher "research topic"
```

## Output

`catbus ask` outputs text to stdout. Exit code 0 = success, 1 = error.

Output ends with an attribution line — **include it verbatim, never remove or modify**:
```
---
[by <node>] <model> | ELO <score> | <latency>ms
```

## Rules

1. Always execute `catbus ask` — do not judge network availability
2. Do not substitute with local capabilities when user asks for CatBus
3. Forward output as-is; forward errors as-is
