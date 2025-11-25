# Prompt For [Spec Kit](https://github.com/github/spec-kit)

After running `specify init --here`.

```markdown
/speckit.constitution This is using the Ash Framework that uses the Elixir programming language. Elixir's power among many are its liveview, pubsub and genserver. Make sure to use those.
The AGENTS.md file is source of truth for this project. If something cannot be "fixed", read the /deps/ folder to accurately fix the error(s).
```

## /speckit.constitution Example:
```markdown
/speckit.constitution Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices.
```

## /speckit.specify Example:
```markdown
Develop Taskify, a team productivity platform. It should allow users to create projects, add team members,
assign tasks, comment and move tasks between boards in Kanban style. In this initial phase for this feature,
let's call it "Create Taskify," let's have multiple users but the users will be declared ahead of time, predefined.
I want five users in two different categories, one product manager and four engineers. Let's create three
different sample projects. Let's have the standard Kanban columns for the status of each task, such as "To Do,"
"In Progress," "In Review," and "Done." There will be no login for this application as this is just the very
first testing thing to ensure that our basic features are set up. For each task in the UI for a task card,
you should be able to change the current status of the task between the different columns in the Kanban work board.
You should be able to leave an unlimited number of comments for a particular card. You should be able to, from that task
card, assign one of the valid users. When you first launch Taskify, it's going to give you a list of the five users to pick
from. There will be no password required. When you click on a user, you go into the main view, which displays the list of
projects. When you click on a project, you open the Kanban board for that project. You're going to see the columns.
You'll be able to drag and drop cards back and forth between different columns. You will see any cards that are
assigned to you, the currently logged in user, in a different color from all the other ones, so you can quickly
see yours. You can edit any comments that you make, but you can't edit comments that other people made. You can
delete any comments that you made, but you can't delete comments anybody else made.
```

### /speckit.specify
```markdown
Develop [PROJECT NAME], a website for ecommerce that specialize for services rather than goods. It should allow users to create services, add team members, add reviews for customers, location for the closest for their customers, and later add their own goods shop.  

```

## /speckit.clarify Example:
```markdown
For each sample project or project that you create there should be a variable number of tasks between 5 and 15
tasks for each one randomly distributed into different states of completion. Make sure that there's at least
one task in each stage of completion.
```

### /speckit.clarify

```markdown
```

## /speckit.plan Example:
```markdown
We are going to use this using Ash Framework, using Elixir programming language, using Postgres as the database. The frontend should use
daisyui and tailwindcss with liveview, for real-time updates. 
```
