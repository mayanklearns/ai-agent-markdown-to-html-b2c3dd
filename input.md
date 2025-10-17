In this project, students will build an application that can build, deploy, update an application!

1. **Build**. The student:
   - receives & verifies a **request** containing an app brief
   - uses an **LLM-assisted generator** to build the app,
   - deployes to **GitHub Pages**,
   - then **pings an evaluation API** with repo details
2. **Evaluate**. The instructors:
   - run automated **static, dynamic (Playwright), and and LLM** checks
   - store and publish the results **after the deadline**
   - send a **second request** tailored to the student’s codebase
3. **Revise**. The student
   - verifies secret
   - **updates** the app based on the request
   - **re‑deploys** Pages
   - then **pings a second evaluation API** with repo metadata.