# Contributing Guidelines
Anyone is welcome to collaborate on this project, but please adhere to the conventions in place.
<!--TOC_START-->
## Contents
- [Concepts](#concepts)
- [Naming Conventions](#naming-conventions)
- [Topic Structure](#topic-structure)
- [Modules](#modules)
- [Review Process](#review-process)
- [Weekly Process](#weekly-process)
	- [Roles](#roles)
		- [Courseware Admin](#courseware-admin)
		- [Project Admin](#project-admin)
		- [Courseware Developers & Maintainers (Internal)](#courseware-developers--maintainers-internal)
	- [Breakdown](#breakdown)
		- [Monday (am)](#monday-am)
		- [Tuesday (pm)](#tuesday-pm)
		- [Wednesday (pm)](#wednesday-pm)
		- [Thursday (pm)](#thursday-pm)
		- [Friday (pm)](#friday-pm)
- [Other Implementation Guidelines](#other-implementation-guidelines)
	- [One Line Per Sentence](#one-line-per-sentence)
	- [Pull Requests and Issues for 1 Module](#pull-requests-and-issues-for-1-module)

<!--TOC_END-->
## Concepts
All courseware on this project is in small and digestable modules.
These modules are then organised into topics.
Moving forward, we will be introducing courses and pathways into the project.
- **Topics**
    - This is a subject, which could be a programming language or tool. For example, Java, Jenkins, Kubernetes and Google Cloud Platform would all be considered subjects.
- **Modules**
    - Modules are the topics broken down into much smaller, digestible content and tasks.
- **Courses**
    - A course is a carefully ordered set of modules, from any topics, that are relevant for the course outcome.
- **Pathways**
    - Pathways are courses that have been put in order.
## Naming Conventions
Files, folders and packages should be in lowercase with exceptions to the `README.md` file.
Names with multiple words should be separated with a `-` e.g cloud-enabling-technologies
## Topic Structure
Any example files & projects can be stored in the module folder.
There shouldn't be any reason for binary files to be on this repository, other than images at this point.
If binary files are generated from examples, then be sure to include them in the `.gitignore`.
Images should not be uploaded, otherwise this project will be a very big download.
You can, of course, still use images, but source them from somewhere else on the internet (such as a public storage bucket, [imgur](https://imgur.com), Google Drive).


Here is an example layout of a topic:
```text
./topics/example-topic/
├── modules
│   ├── module-1
│   │   └── README.md - Include broken-down concepts and tasks here
│   └── module-2
│       └── README.md
└── README.md - Overview of the topic, you could maybe include installation guides here if they aren't to cumbersome
```
The main `README.md` for a topic must at least have a heading (`# [TOPIC_NAME]`) and an overview section (`## Overview`):
```markdown
# Topic Name
## Overview
Some simple explanation or statement about the topic here.
```
A topic must have at least 1 module, usually this would be an introduction module to get started.
## Modules
A module should be a very small deliverable piece of information with a relevant task.
The only required headings are `# Module Name`, `## Overview` and `## Tasks`.
You must include an `## Overview` section because of the automation in place.
The `## Tasks` heading must be included because every module to should have a related task to complete.

So it would looke something like this:
```markdown
# Module name

## Overview

## ...
### ...

## Tasks
```

Some modules will be very difficult to include a task because there might not be an easy way to implement what has been discussed in the module.
To alleviate this, tasks can include quizzes, Google Form questionnaires and links to videos where needed.
Quizzes can be made by using hidden drop downs for the answers.

## Review Process
Modules will go through 2 sets of reviewing before being released into the *master* branch.

The first review is for technical and implementation purposes, therefore they would be reviewed ideally by someone with some knowledge in the area:
- Are the tasks clear and solvable? (attempt the task)
- Are the statements and explained concepts correct throughout the module?
- Does it serve its purpose well, as a piece of material for learning?
- Does it make sense grammatically, are there any spelling errors etc.
- Is the formatting correct
    - Are the required headers there and used correctly
    - Are each sentence on a separate line
- Is the module not too big or expands beyond what should be covered
- Is the *pull request* without *merge conflicts*
- Is the *pull request* labeled with the correct **release label**

The second set of reviewing is more for grammatical purposes and general look of the material:
- Any spelling mistakes.
- Grammatical improvements to ensure content is readable and makes sense.
- Formatting improvements to improve readability - this could be things like heading sizes and line breaks.
- Are acronyms expanded to avoid confusion?
- 

## Weekly Process
A weekly process is being formed internally for this project to allow modules to be implemented on a weekly basis.
This process only really concerns internal contributors of the project.

### Roles
#### Courseware Admin
This person reviews pull requests from release candidates into the master branch.
The courseware admin has the main say in what issues and modules are focused on each week and has the most visibility of the available courseware on the project.
#### Project Admin
Project admins are responsible for managing the automation and overall direction for the project.
#### Courseware Developers & Maintainers (Internal)
Courseware developers and maintainers create courseware and pull requests for the current ongoing release.
### Breakdown
Throughout the week pull requests are reviewed and merged to the `dev` branch. No more pull requests will be merged after Wednesday PM.
#### Monday (am)
A project admin, courseware admin and available internal contributors have a meeting to discuss and establish he following:
- What upcoming training requirements are there for material
- What modules need to be created
- Prioritise the modules and issues that need to be implemented
- Create a milestone for the next version, which is typically just an increment of the revision
- Decide who (internally) is responsible for completing those modules by **Wednesday**
#### Tuesday (pm)
- Stand-up meeting to check internal progress
#### Wednesday (pm)
- Cut off for merging into the `dev` branch.
- Any modules not reviewed and merged into the `dev` branch are taken from the release.
- Release candidate branch and a pull request to the `master` branch is created with a name of `R-[VERSION]`, `R-0.0.3` for instance.
#### Thursday (pm)
- Stand-up meeting to check internal progress on release candidate reviews.
#### Friday (pm)
- Revert any incomplete reviews and merge the release candidate branch to `master`
- Rebase the `dev` branch with the release candidate branch
- Delete the release branch.
- Tag the master branch as the new version.
## Other Implementation Guidelines
### One Line Per Sentence 
Use 1 line per sentence.
This makes the suggestions feature when reviewing the module extremely useful.
If the sentence is over more than one line then this feature becomes quite redundant and more difficult to use.
### Pull Requests and Issues for 1 Module
Issues and pull requests are intended for 1 module at a time.
This makes the review and implementation processes much more maintainable.
