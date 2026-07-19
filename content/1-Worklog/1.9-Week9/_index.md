---
title: "Week 9 Worklog"
date: 2026-05-04
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
### Week 9 Objectives:
- Shift focus from learning AWS to applying it to the AI Image Upscaling Service project.
- Examine the Next.js Frontend and Backend API, and finalize the API contract.
- Plan the integration of image uploading, upscale configuration, and result display.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Monday | Read the Frontend README, understand the stack (Next.js 14 App Router, TypeScript, Tailwind CSS, react-dropzone, lucide-react). Survey the structure of src/app, components, hooks, context, lib, and types. | 29/06/2026 | 29/06/2026 | <https://github.com/vuong20031591-hub/upscale-FE> |
| Tuesday | Identify the component groups to be implemented/completed: upload, config, result, feedback, and ui. Take notes on the design system: primary violet, CTA cyan, success/error/warning. | 30/06/2026 | 30/06/2026 | <https://github.com/vuong20031591-hub/upscale-BE> |
| Wednesday | Review API integration: GET /health/ready, GET /health/config, POST /upscale/ai, POST /upscale/standard. Design the request/response and error schemas. | 01/07/2026 | 01/07/2026 |  |
| Thursday | Map out the user flow: select image, preview, select mode, submit for processing, view results, and download image. Write high-level test cases for upload/config/result/error handling. | 02/07/2026 | 02/07/2026 |  |
| Friday |  Synthesize the API contract and the FE/BE integration checklist. Prepare the implementation plan for the upload UI and processing configurations. | 03/07/2026 | 03/07/2026 |  |

### Week 9 Achievements:
- Mastered the Frontend architecture of the AI Image Upscaling Service project.
- Clearly identified the required integration endpoints and the core user flows.
- Completed the draft API contract and high-level test cases for the implementation phase.

### Lessons Learned / Knowledge Gained:
- Learned how to apply cloud foundational knowledge and deployment mindsets to real-world FE/BE projects.
- Gained further experience with Next.js, TypeScript, Tailwind CSS, API clients, file uploading, and asynchronous state handling.
- Enhanced skills in Frontend-Backend integration, testing, debugging, and writing handoff documentation.