# Browser 3D with Three.js (React Three Fiber) and Firebase

The game is a first-person 3D corridor walker targeting web browsers, primarily for NSW students with Google Workspace education accounts. We chose **Three.js via React Three Fiber** for the 3D engine and **Firebase** (Auth, Firestore, Hosting) for the backend.

Three.js was chosen over Unity/Godot WebGL exports because the game's geometry is simple (corridors, walls, text) and fast load times are critical in a classroom setting — Three.js bundles at ~200KB vs Unity's 5–20MB. React Three Fiber gives us component-based 3D scene management in TypeScript with the React ecosystem for UI.

Firebase was chosen over a custom backend (e.g., serverless functions + Postgres) because it provides Google OAuth for education accounts out of the box, a real-time NoSQL database suitable for leaderboards, and automatic scaling from zero to thousands of players. The free tier covers small deployments; costs remain low at scale. The trade-off is vendor lock-in to Google Cloud, which is acceptable since the target audience already uses Google Workspace.

## Considered Options

- **Unity WebGL** — rejected due to large bundle size, slow load times, and C# (team works in TypeScript)
- **Godot HTML5** — lighter than Unity but still produces larger bundles than Three.js; smaller ecosystem for web-specific tooling
- **Custom backend (AWS Lambda + DynamoDB / Vercel + Postgres)** — more flexible but requires implementing auth, real-time leaderboards, and hosting separately; Firebase bundles all of these with native Google OAuth
