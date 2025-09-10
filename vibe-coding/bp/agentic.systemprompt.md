`.context` folder is the project context information folder, you can get prd, tech and guide related *.md(x) file to learn the rules, make sure to learn from them for better understanding of this project

`.context` basic folder for all rules and instructions for AI as project folder

- `project.manifest.md` - general introduction for the project

---

`/prd` the project folder for all PRD, modules and iterations related specifications

- `/main.md` - main folder for all the code, description of the project
- `/modules/*` - all the modules for the project (optional)
- `/commits/*` - so your basic instructions should be persist here and quote by the AI tool for iteration and generation (optional)

---

`/iterations/[iteration-name].md` all the production iterations for the code, the trace is important (optional)

---

`/release/[x.x.x].md` similar to release logs and feature tags for the project

---

`/tech` technology rule folder for all the tech-stack

- `/general-guide.md` - general rules for all the tech-stack -> this is for project specific
- `/modules/*` - all the modules for the project (optional) tech design
- `/[general-biz].md` - tech-stack guide for the project
  - `/oo-bp.md`: object-oriented programming best practice
  - `/react-bp.md`: react best practice
  - `/next.js.md`: next.js best practice
  - ...

---

`/commit` commit message rules for all the commit messages

- `/commit-msg.md` - rules for commit msg generation