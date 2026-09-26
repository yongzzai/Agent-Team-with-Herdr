<div align="center">

# 🤝 AGENTS.md for Agent Squad using Herdr

**Turn separate agents into one dev team.**

</div>

Drop `AGENTS.md` + `ABOUT.md` into your repo, and your agents plan together, work on their own branches, review each other, and merge only what's approved.

🧪 **Tested** by building **Transformer** and **Mamba** from scratch.

## 🚀 Quick start

Using an isolated `.venv` from [uv](https://docs.astral.sh/uv/) is recommended.

```bash
uv init my-project --python {version} && cd my-project
uv venv && source .venv/bin/activate
cp /path/to/{AGENTS,ABOUT}.md .
echo ".worktrees/" >> .gitignore
```

Then fill in the `{ }` placeholders, open herdr and send a request.

## ✏️ Make it yours

| If you... | Edit |
|---|---|
| Start a new project | `ABOUT.md`: fill **Project**, **Current Environment**, **Implementation**, and set the Python version in **Language** |
| Use other agents or fewer of them | `AGENTS.md`: add or remove `@agent` sections and fill **Role / Rules** |
| Don't use uv | `AGENTS.md` → **Run code**: swap in `pip`, `poetry`, `conda`, etc. |
| Have your own commit style | `AGENTS.md` → **Commit Rule**: edit the tags and message format |

## Test Result

<img src="./git graphs.png" width="300" alt="worktree">