# From Feature Tours to Guided Outcomes

## Redesigning onboarding and contextual guidance for StreetLight InSight

**Project type:** UX content strategy · Product onboarding · In-app guidance · Information architecture  
**Products:** StreetLight InSight Planning and Traffic Monitor  
**Primary platform:** WalkMe, supported by the Help Center and eLearning  
**Project period:** 2026; implementation summary through October/November 2026  
**Role:** Technical writing / UX content strategy / WalkMe content design *(customize to reflect your exact ownership and collaborators)*  
**Status:** Initial improvements implemented; further experience and measurement work proposed

> **Case study summary:** I reviewed how StreetLight introduces users to its transportation-analysis products and how guidance follows them through complex workflows. The work shifted the onboarding model away from long, feature-oriented tours and toward short, contextual guidance focused on a user's transportation question and the decisions required to answer it. The initial release included revised walkthroughs, a reorganized Help menu, more intentional What's New behavior, and task-oriented Traffic Monitor tours. This case study documents the research, content strategy, implementation examples, intended benefits, and next steps. **User and business outcomes described below are hypotheses unless explicitly identified as observed.**

*Screenshot to add: A representative StreetLight screen showing the revised Help menu or a contextual walkthrough balloon.*

---

## 1. Context: a capable product with a complex learning journey

StreetLight helps transportation professionals study travel behavior and traffic conditions to inform planning, operations, and project decisions. Two experiences were in scope:

- **InSight Planning:** Select an analysis, configure travel modes, dates, geographies, and metrics, then explore and share results.
- **Traffic Monitor:** Investigate conditions such as slowdowns, travel-time changes, queues, volume patterns, route performance, and potential closure impacts.

The product already had a substantial support ecosystem: WalkMe walkthroughs and tooltips, a Help Center, and self-paced and instructor-led training. That breadth was valuable, but it also created opportunities for overlapping explanations, premature resource recommendations, and instructions that described interface controls before explaining why those controls mattered.

The core challenge was not a lack of help content. It was **aligning each piece of guidance with the user's immediate goal and stage of work**.

### The user outcome we wanted to support

Move from:

**“Learn where everything is and what every control does.”**

To:

**“I have a transportation question. Help me choose an approach, configure it, understand the results, and use the findings.”**

For Traffic Monitor, the analogous outcome was completing a meaningful traffic investigation—not touring every module.

---

## 2. The problem and opportunity

The historical WalkMe export showed several recurring patterns:

| Pattern in existing guidance | Why it mattered |
| --- | --- |
| Tours narrated interface controls, such as clicking **Next**, opening tabs, or using map tools. | A user could complete the tour without knowing how to apply the product to a project. |
| Some introductory flows sent users to another page, tour, video, Help Center article, or training resource. | This could fragment the task before the user had encountered a need for that resource. |
| Several tours and entry points overlapped, especially around zones, analysis configuration, and Traffic Monitor modules. | Users faced repeated invitations; the content required more ongoing maintenance. |
| The Help menu mixed contextual help with broader reference and learning materials. | Users had to identify which resource would answer their current question. |
| What's New could take over the experience at login. | Release awareness competed with the task a user had come to perform. |
| A support link and related links occupied space near AI chat. | According to the project team's implementation notes, the placement obscured text reminding users to verify AI-generated answers. |

The initial content audit suggested that onboarding could become more effective without adding more instruction: **reduce unnecessary explanation, improve timing, and make a useful next action easier to identify**.

*Screenshot to add: Before-state WalkMe balloon or menu, showing control-by-control instructions or a premature Help Center/training link.*

---

## 3. Research and discovery

### Inputs reviewed

1. **Historical WalkMe element export** (`walkme-elements.txt`): An inventory of the existing guidance copy, including analysis creation, zones, visualizations, and Traffic Monitor tours.
2. **Updated WalkMe element export** (`walkme_all_elements_2026-10-27.txt`): A snapshot of revised text and element names, used to compare selected before/after examples.
3. **InSight Planning Help Center export** (`planning-help-center.md`): Existing procedural, conceptual, and reference documentation, including a quick-start guide, zone concepts, analysis types, visualizations, metrics, and data availability.
4. **Fundamentals eLearning export** (`elearning-content.txt`): The training curriculum and conceptual framework for transportation analysis, including choices about **how, where, when, and what** to measure.
5. **Project implementation summary and working plan**: The completed changes, desired content-channel roles, proposed onboarding checklists, and longer-term opportunities supplied by the project team.
6. **External UX research**: Krystal Higgins's publicly available discussion of *Better Onboarding*, plus Nielsen Norman Group research on contextual help, onboarding tutorials, progressive disclosure, and task-focused documentation.

### What the research contributed

**Krystal Higgins — guided interaction and ongoing onboarding.** Higgins presents onboarding as a journey beyond first login: users develop competence by interacting with the product along paths that reflect what they are trying to accomplish. This informed the decision to teach within an authentic workflow and to avoid treating a generic introduction as the main onboarding experience [1–3].

**Nielsen Norman Group — contextual help over disruptive instruction.** NN/g describes why automatic tutorials are often skipped, forgotten, or poorly timed, and recommends help that is easy to dismiss, easy to find again, and closely related to the current task [4].

**Progressive disclosure and usable documentation.** NN/g's guidance supports revealing essential information first and making deeper detail available when requested; help should be searchable and focused on what users are trying to do [5–6].

**The content audit — put detail in the right channel.** The Help Center already provided substantial depth, and eLearning covered conceptual foundations. WalkMe did not need to reproduce both. Its strongest role was helping users make decisions and proceed at the point of interaction.

### Research limitations

This was primarily a **content and heuristic review**, not a formal usability study. The WalkMe exports expose text and element types, but do not reliably establish targeting rules, publication status, step counts, completion logic, or actual first-run behavior. No customer interviews, observed task tests, analytics, or support-ticket measurements were supplied for this case study. The full text of Higgins's book was **not** reviewed; its influence is grounded in the publisher's description and Higgins's public writing/interviews, not a chapter-by-chapter audit.

---

## 4. The strategy: help users do the work

Three short principles clarified where guidance should live:

> **Product:** Help me do it.  
> **Help:** Help me recover or understand it.  
> **Training:** Help me master it.

| Channel | Primary job in the experience |
| --- | --- |
| **In-app guidance / WalkMe** | Define a goal; help users choose options; explain consequential settings; assist with a task or recovery at the moment of need. |
| **Contextual Help menu** | Offer relevant walkthroughs and a small set of useful links for the current page. |
| **Help Center** | Provide durable procedures, definitions, constraints, troubleshooting, methodology, and deeper reference material. |
| **eLearning** | Develop conceptual understanding, scenario-based practice, advanced workflows, and broader confidence. |
| **What's New** | Make meaningful product changes discoverable without requiring users to read release notes before working. |

This model led to five design rules:

1. **Start with the transportation question**, not an inventory of features.
2. **Explain decisions and consequences**, not every mouse click.
3. **Show help in context** and let users reopen it later.
4. **Use one clear next step**, rather than chaining unrelated tours and resources.
5. **Keep deeper learning optional**, so users can continue their current task.

---

## 5. What changed

### A. Planning: shorter walkthroughs focused on a transportation question

The analysis and visualization guidance was reframed around what users were trying to find out.

| Earlier emphasis | Revised emphasis |
| --- | --- |
| **Create New Analysis**: navigate tabs, select controls, and follow the walkthrough. | **Create an analysis**: begin with the question, select an appropriate analysis, and make settings match the question. |
| **Add and Create Zones**: enumerate drawing, uploading, gates, tables, and navigation. | **Select the places you want to study**: define relevant geography, select zones, and review before saving. |
| **Visualize analysis results**: explain map tools, filters, and chart controls. | **Find insights in your results**: choose meaningful metrics, examine location/time patterns, and use findings in a report. |

**Example — analysis selection:** The revised export includes: *“Start with a transportation question you want to answer. We'll help you choose an analysis that fits what you want to measure.”* The associated experience, **Create an analysis**, is positioned around choosing an analysis rather than simply touring the setup screens.

**Example — zones:** The revised introductory copy explains that zones define where StreetLight measures travel and invites the user to add the roads or areas for the study. The review step asks users to check their selected zones before saving.

**Example — visualizations:** The revised tour invites users to explore metrics, maps, and charts to identify patterns useful for a project or report. It asks questions such as **“What stands out in your results?”** and **“When do these patterns occur?”** rather than only describing chart controls.

**Why:** Users need a reason for each selection, especially when an analysis has many possible combinations of travel mode, time period, zones, and metrics.

**Expected user benefit:** Less cognitive overhead; clearer connection between setup decisions and a useful result.  
**Expected team benefit:** Shorter, more focused walkthroughs; fewer instructions that become obsolete when controls move.

*Screenshot to add: “Create an analysis” or “Need help setting up your analysis?” balloon on the Basic Info tab.*

*Screenshot to add: “Find insights in your results” guidance beside Visualization filters or charts.*

### B. Help menu: distinguish assistance from reference material

The Help menu was reorganized to separate relevant, contextual help from broader learning and support resources. The updated export contains sections named **Help with this page** and **Learning and support**.

The content strategy also called for reducing links that appeared before a user understood why they needed assistance—for example, generic Help Center or Training Portal recommendations embedded within an introductory walkthrough.

**Why:** The first question at a difficult moment is usually *“What do I do here?”* not *“Which documentation collection should I browse?”*

**Expected user benefit:** Faster discovery of the right kind of help with less interruption to the current workflow.  
**Expected team benefit:** Cleaner navigation and a clearer division of responsibility among WalkMe, Help Center, and training.

A later refinement proposed separating **Help with this page** (in-app tours/decision guidance) from **Related articles** (relevant Help Center links). That exact two-section arrangement should be treated as a **design proposal until its implementation is confirmed**.

*Screenshot to add: Current Help menu showing “Help with this page” and the learning/reference section(s).* 

### C. Traffic Monitor: task-based module guidance

The historical export included module **Quick Tours** that stepped through controls. In the revised export, several walkthroughs have titles that describe a transportation task:

| Module | Revised walkthrough name |
| --- | --- |
| Timeline | **Explore recent traffic conditions over time** |
| Speeds | **Investigate traffic slowdowns** |
| Queuing | **Investigate traffic queues** |
| Route Monitoring | **Monitor traffic on key routes** |
| Closure Impacts | **Evaluate the impacts of a lane closure** |

The copy also changed. For example, the revised Timeline guidance asks **“Where and when did traffic change?”** and directs users to move through the timeline to find noteworthy conditions. The Closure Impacts flow emphasizes the closure location, remaining capacity, timing, and potential traffic diversion—not simply the sequence of clicks.

The team reported making these tours shorter, goal-oriented, and contextually accessible from the appropriate module.

**Why:** Users often know which traffic issue they need to investigate before they know the names or controls of the modules.

**Expected user benefit:** A clearer starting point and a more direct path to analyzing a relevant traffic condition.  
**Expected team benefit:** A consistent content pattern across modules and more reusable guidance principles.

**Important qualification:** The October export still contains earlier or overlapping module-tour entries. The text confirms substantial rewrites, not that every legacy tour has been unpublished or that all old entry points are inactive.

*Screenshot to add: Traffic Monitor Help menu with a task-oriented tour title, plus one revised Timeline or Closure Impacts balloon.*

### D. What's New: announce the release without taking over the session

The full What's New experience was reorganized as a **carousel**, allowing release highlights to be browsed when users choose to engage. Rather than automatically opening the full experience, the team changed the introduction to a small balloon attached to the existing **What's New** button.

The updated export includes a Fall 2026 invitation titled **“Discover what's new for Fall 2026!”** with copy encouraging users to select **What's New** whenever they want to explore the release.

**Why:** A user may be aware of a new release but not ready to interrupt an active task to read all the details.

**Expected user benefit:** Control over when to engage with announcements; fewer forced interruptions.  
**Expected team benefit:** A more maintainable release-content pattern and a single, reusable entry point for announcements.

The **balloon-on-button behavior** is documented in the team's change summary; its autoplay/suppression configuration is not verifiable from a text-only export.

*Screenshot to add: Fall 2026 What's New notification balloon anchored to the button; optionally the carousel opened on demand.*

### E. Support: move links without hiding the AI verification reminder

The team moved the **Contact Support** link and related links out of a location where they obscured an AI-chat message telling users to verify AI-generated answers.

**Why:** Persistent support access is useful, but it should not cover more consequential guidance about the reliability and verification of AI-generated information.

**Expected user benefit:** A clearer, less obstructed message about checking AI responses; continued access to support.  
**Expected team benefit:** Improved content hierarchy and a more deliberate trust/safety message within the interface.

*Screenshot to add: The AI chat area after the links were relocated, showing the verification reminder unobstructed.*

---

## 6. How the work was implemented

The project combined analysis and hands-on content revision:

1. **Inventory and compare existing content.** Review the historical WalkMe export alongside Help Center and training material to identify duplicated or poorly timed instruction.
2. **Define a cross-channel model.** Decide which information belongs in-product, in Help, or in eLearning.
3. **Rewrite the highest-value touchpoints.** Start with analysis selection and configuration, zones, visualizations, and the major Traffic Monitor modules.
4. **Improve entry points.** Make walkthroughs available in the relevant context; separate contextual guidance from broader resources.
5. **Reduce proactive interruption.** Replace full What's New autoplay with a smaller on-demand invitation.
6. **Review the updated export.** Check that revised titles and text support goals, decisions, and interpretation rather than merely repeating UI labels.

A practical lesson from the work was that **shortening the copy alone was not enough**. The entry point, timing, and destination of a balloon often mattered as much as the wording.

### A representative before-and-after

| Historical WalkMe language | Revised WalkMe language | Design shift |
| --- | --- | --- |
| **Zones:** “Click Next to continue.” | **Zones:** “Check the zones you've selected, then click Save.” | From tour progression to completing an actual task. |
| **Timeline:** “Drag the slider to change the time of day.” | **Timeline:** “Move through the timeline to see how conditions changed throughout the day. Look for locations and times that stand out.” | From control instruction to analytical purpose. |
| **Visualizations:** “Use these controls to filter the metrics you want to visualize…” | **Visualizations:** “Identify the metrics that relate to your transportation question.” | From interface explanation to decision support. |
| **Create Analysis:** Broad tour invitation with links to more resources. | **Create an analysis:** Start with a transportation question and choose an analysis that fits it. | From content discovery to a goal-driven starting point. |

These examples compare **copy visible in the two exports**. They are not instrumented evidence of user behavior.

---

## 7. Results, expected impact, and what remains unmeasured

### What can be claimed from the available evidence

- A revised content strategy and channel model were articulated.
- The updated WalkMe export contains specific rewritten titles and guidance for Planning, zones, visualizations, and several Traffic Monitor modules.
- The project team reported implementing a reorganized Help menu, contextual walkthrough access, a What's New carousel with a button-anchored invitation, and relocation of support links.
- The work created a clearer distinction between **getting something done now** and **learning more later**.

### Expected user outcomes — not yet measured

- Less time spent in general-purpose tours.
- More users able to identify a relevant analysis or Traffic Monitor module.
- Fewer interruptions before and during the first meaningful task.
- Easier access to relevant reference material at the point of need.
- Greater confidence progressing from configuration to interpretation.

### Expected organizational outcomes — not yet measured

- More focused and maintainable WalkMe content.
- Fewer duplicated explanations across WalkMe, Help Center, and eLearning.
- Clearer user journeys that can be improved and measured over time.
- Potentially fewer avoidable support requests and improved product adoption.

**No quantitative uplift, task-completion improvement, training conversion rate, or support reduction is claimed here.** Those would require product analytics, support data, or user research.

### Suggested measurement plan

| Question | Possible measurement |
| --- | --- |
| Are users making it to their first meaningful outcome? | Analysis submission; result/visualization interaction; first Traffic Monitor investigation. |
| Is contextual help useful? | Tour starts by context; early exits; successful continuation of the underlying task. |
| Is Help easier to find? | Help-menu findability task; time to locate a relevant guide or article. |
| Did What's New become less disruptive? | Invitation dismissal; voluntary carousel opens; clicks to relevant release details. |
| Are improvements reducing confusion? | Recurring support topics; abandonment points; qualitative feedback. |
| Does training reinforce product learning? | Course referrals, starts/completions, and subsequent product activity, interpreted cautiously. |

*Screenshot or artifact to add later: An analytics dashboard or anonymized before/after usability-test result—only once actual measurements are available.*

---

## 8. Trade-offs and lessons learned

**Guidance must remain available without becoming compulsory.** A first-visit invitation may be useful, but it should be dismissible, should not compete with other popups, and should not force users through a tour.

**Not every question belongs in a walkthrough.** Detailed metric definitions, methodology, and edge cases are better maintained in the Help Center. Conceptual understanding and advanced scenarios are better suited to training.

**A menu can be organized by user need rather than content inventory.** “Help with this page” has a different purpose from browsing a full Help Center catalog.

**Copy and product behavior must be evaluated together.** A good balloon can still be disruptive if it appears at the wrong time, and a good tour can still be hard to use if it is difficult to reopen.

**A text export is only part of the evidence.** It captures content changes but not actual visual placement, targeting rules, suppression logic, or whether older entries are still published. Screenshots, WalkMe configuration checks, and testing should supplement the content audit.

---

## 9. Next steps and future opportunities

The following ideas were discussed in the project plan or subsequent design work. **They should not be presented as implemented unless separately confirmed.**

### Near-term refinements

- **Make Help Me Choose more question-driven.** Help users choose analysis types, zones, or metric options by answering questions about what they want to study, rather than reading long lists of definitions.
- **Consolidate legacy entry points and overlapping tours.** Audit older module overviews, zone tours, and tour-to-tour invitations; retire redundant published experiences.
- **Refine first-use invitations.** Limit simultaneous prompts, suppress repeat invitations appropriately, and preserve on-demand reopening.
- **Improve first-results guidance.** Help users choose an appropriate metric, recognize geographic and time patterns, and use findings in a report.
- **Connect eLearning to meaningful moments.** Recommend relevant courses after a first analysis or investigation, not as a prerequisite to using the product.

### Medium- and longer-term opportunities

- **First-outcome checklist:** A short, auto-completing sequence from choosing a transportation question through producing and using a finding; a parallel outcome-based checklist for Traffic Monitor.
- **Context-aware Help:** Personalize useful links and tours using the current page, task, workflow state, and available modules.
- **Native guidance:** Move proven, high-frequency tips into empty states, field help, defaults, and validation where feasible.
- **Returning-user onboarding:** Introduce relevant changes to established users without restarting a new-user tour.
- **Cross-session continuity:** Preserve onboarding progress and help users resume unfinished work.
- **Evaluation:** Run task-based usability tests and use telemetry to test the assumptions in this case study.

*Screenshot to add later: A first-outcome checklist or question-driven Help Me Choose experience, once implemented.*

---

## 10. Portfolio takeaway

This project was an exercise in **content design as product design**. The solution was not to produce more onboarding material but to reconsider **what users needed to know, when they needed to know it, and what they should be able to do next**.

By framing guidance around transportation questions, shortening walkthroughs, improving access to contextual help, and making release announcements optional, the work moved StreetLight toward a more integrated onboarding experience. The next measure of success is whether these changes demonstrably help users reach useful insights sooner and with fewer interruptions.

---

## Research references

The following are the external research sources that informed this work; they are **design rationale**, not independent validation of the specific StreetLight implementation.

1. Higgins, Krystal. *Better Onboarding: Effective Strategies for Guiding New Users.* A Book Apart, 2021. [Publisher description](https://abookapart.com/products/better-onboarding.html).
2. Higgins, Krystal. “[Introducing the book, Better Onboarding!](https://www.kryshiggins.com/introducing-the-book-better-onboarding/)” May 11, 2021. Describes guided interaction and onboarding beyond the first run.
3. “[Designing Better User Onboarding Experiences: An Interview with Krystal Higgins.](https://blog.adobe.com/en/publish/2022/01/06/krystal-higgins-better-user-onboarding)” Adobe, January 6, 2022. Discusses onboarding as a journey toward established use.
4. Laubheimer, Page. “[Onboarding Tutorials vs. Contextual Help.](https://www.nngroup.com/articles/onboarding-tutorials/)” Nielsen Norman Group, February 12, 2023. Research and recommendations for contextual, dismissible, retrievable help.
5. Kendrick, Alita. “[Help and Documentation (Usability Heuristic #10).](https://www.nngroup.com/articles/help-and-documentation/)” Nielsen Norman Group, December 13, 2020. Task-focused, searchable help and documentation.
6. Nielsen, Jakob. “[Progressive Disclosure.](https://www.nngroup.com/articles/progressive-disclosure/)” Nielsen Norman Group, December 3, 2006. Reveal essential options before secondary complexity.

**Source-access note:** The full text of *Better Onboarding* was not supplied or read. References to Higgins's framework are based on verified publisher material and publicly available author interviews and articles. No page-specific claims from the book are made.

### Internal project evidence

- `walkme-elements.txt` — historical WalkMe text export (baseline; export date not established here).
- `walkme_all_elements_2026-10-27.txt` — updated WalkMe text export, dated October 27, 2026.
- `planning-help-center.md` — InSight Planning Help Center content export.
- `elearning-content.txt` — Fundamentals eLearning content export.
- Project plan, 2611 WalkMe changelog, and implementation notes supplied by the project team.

**Evidence note:** These are source materials for copy and documentation analysis. Live targeting, publication status, UI layout, and the team's reported behavior changes should be validated with current product screenshots or WalkMe configuration when preparing a formal results presentation.

---

## Portfolio publishing checklist

Before sharing this case study publicly:

- [ ] Confirm permission to name StreetLight/Jacobs and show the interface, including internal WalkMe content.
- [ ] Replace the role summary with your exact responsibilities and name collaborators where appropriate.
- [ ] Add real before-and-after screenshots, redacting customer data, internal URLs, and sensitive account details.
- [ ] Verify which designs were deployed versus prototyped or still proposed.
- [ ] Add real outcome metrics or user quotes only when approved, available, and attributable.
- [ ] Optionally shorten this detailed version into a 600–900-word public portfolio narrative and retain this file as the extended case study.
