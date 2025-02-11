## Have you ever received a critical feedback? if so, what was that and how it affect you
When answering a question about receiving critical feedback, it's important to frame your response positively and show how you took the feedback constructively to grow professionally. Here's a structured way to answer:

---

**Answer:**

Yes, I have received critical feedback that has greatly influenced my approach to project management and teamwork.

For instance, I was working on a project with two other engineers. As we progressed, it became clear that we needed additional resources to meet our deadline. However, I believed that onboarding a new team member would take more time than completing the work ourselves, so I decided to push forward with the existing team. While we successfully finished the project on time, my manager gave me critical feedback afterward. They pointed out that for larger projects, it's essential to onboard additional team members to distribute the workload more effectively, rather than increasing the burden on a smaller team. This is because there's a limit to how much workload individuals can handle, and it can impact overall productivity and team morale.

Additionally, my manager emphasized the importance of involving junior engineers in such projects, as it provides them with valuable learning opportunities and helps them grow.

I took this feedback to heart and have since made a conscious effort to involve more team members in larger projects. I've found that distributing the workload not only ensures timely completion but also fosters a collaborative environment where everyone can contribute and learn. This experience has also improved my ability to mentor junior engineers, helping them develop their skills and become valuable assets to the team.

## did you ever had a diaagrement
Answering a question about a disagreement you've had at work can be a great opportunity to show your ability to handle conflicts professionally, collaborate effectively, and prioritize the project's success. Here's a structured way to present your answer:

---

**Answer:**

Yes, I have had disagreements at work, and one notable instance involved working with another team called AI Fabric. They provided LLM (large language model) endpoints for processing reports and summarizing data. They proposed designing their API in an asynchronous way but without a callback mechanism. This approach made the API function more like a synchronous one since they were queuing up requests without returning `429 Too Many Requests` status codes. They suggested using a long retry mechanism to handle this.

I believed that this design could lead to potential issues in our scalable distributed system, especially under burst traffic conditions. If the requests were queued without returning a `429` status, it could result in long wait times and inefficiencies in handling timeouts and retries. 

I respectfully pushed back on their proposal and recommended two possible solutions: either implement the API as a truly synchronous one with proper handling of `429` status codes so that we could manage burst traffic effectively, or introduce a callback mechanism in the asynchronous design to handle job status updates more robustly. By doing this, we could avoid missing results due to timeouts and efficiently retry later by scanning job status.

This disagreement was resolved through constructive discussions. The AI Fabric team understood the potential issues, and we collaboratively decided to implement the API with the callback mechanism. This approach ensured that our system remained robust and scalable, and it also provided us with a more reliable way to handle retries and manage burst traffic.

This experience taught me the importance of clear communication, collaboration, and being open to different perspectives to achieve the best solution for the project.
