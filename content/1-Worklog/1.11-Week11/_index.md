---
title: "Week 11 Worklog"
date: 2026-04-26
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
### Week 11 Objectives:

- Integrate the FE with the Backend's image processing endpoints.
- Display upscale results, support downloads, and implement workflow resetting.
- Optimize the user experience during image processing.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Monday | Create a FormData object containing the image file, mode, and scale factor. Integrate the POST /upscale/standard endpoint. | 13/07/2026 | 13/07/2026 |  |
| Tuesday | Integrate the POST /upscale/ai endpoint. Handle successful responses, server errors, endpoint errors, and timeouts. | 14/07/2026 | 14/07/2026 |  |
| Wednesday | Add loading/pending states while the Backend is processing images. Disable the submit button to prevent duplicate requests. | 15/07/2026 | 15/07/2026 |  |
| Thursday | Build the Result Display: source image, upscaled image, mode/scale details, and a download button. Add reset/retry options to reprocess images. | 16/07/2026 | 16/07/2026 |  |
| Friday | Optimize Toast/Alert notifications for success/error events and test responsiveness on desktop/mobile. Compile a list of bugs and a remediation checklist prior to the final week. | 17/07/2026 | 17/07/2026 |  |

### Week 11 Achievements:
- The upload - API call - result retrieval - download flow is fully functional at a basic level.
- Mastered integration debugging via the Network tab, status codes, response schemas, and Backend logs.
- The Result Display and error feedback have become clearer and more intuitive.

### Lessons Learned / Knowledge Gained:
- Learned how to apply cloud foundational knowledge and deployment mindsets to real-world FE/BE projects.
- Gained further experience with Next.js, TypeScript, Tailwind CSS, API clients, file uploading, and asynchronous state handling.
- Enhanced skills in Frontend-Backend integration, testing, debugging, and writing handoff documentation.
