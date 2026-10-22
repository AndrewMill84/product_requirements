# Setup and daily use

## No skill or system prompt required
This is a documentation repository. `AGENTS.md` is the permanent working agreement; a session message selects the task. No installation, API keys or executable dependencies are required.

Open the extracted repository folder in your chosen tool. Explicitly ask it to read `AGENTS.md` on the first session. `CLAUDE.md` points to that file for Claude Code. For any other tool, use its supported project instruction mechanism to point to `AGENTS.md`, or include that instruction in each starting message. Do not assume every tool automatically loads the same filename.

## Create the repository
Use an approved company GitHub or GitLab account and create an empty private repository. If using the web interface, upload the contents of this folder to the repository root, including the instruction files. Alternatively, use a terminal from this folder:

```bash
git init
git add .
git commit -m "Add requirements workspace starter"
git branch -M main
git remote add origin YOUR_REPOSITORY_URL
git push -u origin main
```

Replace `YOUR_REPOSITORY_URL` with the URL shown by your hosting service. These are instructions, not actions performed by this package. Authentication and Git must be available for terminal use.

## Give access to application evidence
Open or attach the approved application checkout using your agent tool. Record its location and repository reference in `project-context.md`. A path in a Markdown file does not grant access. Do not copy application source into this repository simply to make a reference work.

## First capability
Copy `templates/capability/` to a named folder under `capabilities/`. Fill in its title and brief or ask the agent to help. Add a row to the capability index. Start with one bounded change.

## End and resume
Ask the agent to update the records and checkpoint. Inspect the diff before saving a commit. Push changes according to company policy. Before continuing on another machine, obtain the latest repository changes. Start a fresh session with the resume prompt. The agent should read the files instead of relying on prior chat memory.

Avoid simultaneous editing of the same capability without agreeing ownership. Review conflicting decision records rather than letting an automatic merge choose business behaviour.
