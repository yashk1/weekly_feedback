# Weekly Project Feedback Tool — MVP Scope

## Purpose

Help every project team member contribute weekly feedback, discuss the most important topics in a retrospective, and follow through on agreed actions.

## People and organization

- Feedback is organized by project, with a separate weekly retrospective cycle for each project.
- Every project team member can contribute feedback and vote, not just project owners.
- Each project has a designated facilitator, usually the project owner.
- The facilitator manages the meeting stages and publishes its outcomes.

## Agreed feedback framework

Each cycle uses three categories:

- **Start:** things the team should begin doing.
- **Stop:** things that are not working and should stop.
- **Continue:** things that work and should continue.

## Full user flow

### 1. Create a weekly feedback cycle

The facilitator opens a cycle for a project and makes it available to the project's team members.

### 2. Collect private feedback

Every member can submit items under Start, Stop, or Continue.

Feedback is attributed by default. Each submission has a **Submit anonymously** checkbox, so a contributor can choose anonymity for individual items.

Before the retrospective, contributors can see only their own submissions. They cannot see other people's feedback, including when they have already submitted their own.

### 3. Reveal all feedback together

The designated facilitator starts the retrospective. All feedback becomes visible to the project team at the same time.

### 4. Cluster related feedback

The tool automatically suggests clusters of similar feedback. During the meeting, the team discusses and adjusts these groups before voting.

Clusters must stay within each Start, Stop, or Continue category. Related items from different categories are not combined into one cluster.

### 5. Vote on discussion topics

Each team member gets **three votes in total** across the retrospective's topics. A member can put multiple votes on the same topic.

Voting happens after clustering. When voting closes, the tool produces a prioritized list of discussion topics.

### 6. Discuss the prioritized topics

The team discusses topics in descending vote order, guided by the facilitator.

### 7. Upload meeting material

After discussion, upload an audio recording, video recording, or transcript. The tool transcribes audio or video when needed and processes an uploaded transcript directly.

The tool extracts proposed action items and decisions from the transcript.

The MVP does not include built-in recording or integrations with Zoom, Google Meet, or Microsoft Teams. Those can be considered later.

### 8. Review and publish outcomes

The facilitator reviews and corrects the proposed actions and decisions before publishing them. Actions have an owner, an optional due date, and an Open or Done status.

### 9. Follow up next week

The project retains past cycles, decisions, and action items. Previous actions appear in the next retrospective for follow-up.

## Default decisions made to finish the MVP scope

The user explicitly asked for the remaining important decisions to be made rather than continuing an extended questionnaire. The following are proposed MVP defaults, distinct from the requirements selected directly by the user.

| Decision | Reason | Alternative considered |
| --- | --- | --- |
| Contributors can edit their own feedback before reveal. Submissions close when the facilitator reveals feedback. | Gives members time to refine their input and gives the meeting a stable set of items. | Keeping submissions open throughout the meeting. |
| The facilitator edits suggested clusters while the whole team discusses them. | Keeps the meeting coordinated and simplifies the first version. | Letting everyone move items and rename clusters simultaneously. |
| The facilitator controls transitions between reveal, clustering, voting, and discussion. | Makes the current stage clear and accommodates the team's pace. | Automatic transitions based on a timer. |
| Individual votes and running totals stay hidden from other members until voting closes. Members can see their own allocations. | Reduces influence from early votes. | Showing live vote counts. |
| The facilitator reviews extracted actions and decisions before publishing them. | Creates clear responsibility for correcting extraction errors. | Allowing any member to finalize them or publishing automatically. |
| Anonymous authors stay hidden from teammates and the facilitator. | Makes the anonymity promise meaningful. | Revealing anonymous authors to the facilitator only. |
| Actions have an owner, optional due date, and Open or Done status. | Supports basic follow-through without building a full task manager. | A richer task workflow with more statuses and external syncing. |
| Previous actions appear in the next retrospective. | Helps the team check whether agreed changes happened. | Managing follow-up entirely in a separate tool. |

## Main interface

1. **Project list:** access projects and their feedback cycles.
2. **Retrospective board:** Start, Stop, and Continue columns, with collection, reveal, clustering, voting, and discussion stages.
3. **Outcomes view:** uploaded meeting material, extracted drafts for facilitator review, published decisions, and action items.

Past cycles remain accessible within each project.

## Example with people and a project

