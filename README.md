# Frontend Voice Agent using React + LiveKit SDK

This is a template for [LiveKit Agents](https://docs.livekit.io/agents) that provides a simple voice interface using [Agents UI](https://livekit.io/ui) components and [LiveKit JavaScript SDK](https://github.com/livekit/client-sdk-js). It supports [voice](https://docs.livekit.io/agents/start/voice-ai), [transcriptions](https://docs.livekit.io/agents/build/text/), and [virtual avatars](https://docs.livekit.io/agents/integrations/avatar).

Also available for:
[Android](https://github.com/livekit-examples/agent-starter-android) • [Flutter](https://github.com/livekit-examples/agent-starter-flutter) • [Swift](https://github.com/livekit-examples/agent-starter-swift) • [React Native](https://github.com/livekit-examples/agent-starter-react-native)

<picture>
  <source srcset="./.github/assets/light theme2.png" media="(prefers-color-scheme: light)">
  <source srcset="./.github/assets/dark theme2.png" media="(prefers-color-scheme: dark)">
  <img src="./.github/assets/running app.png" alt="App screenshot">
</picture>

### Features:

- Real-time voice interaction with LiveKit Agents
- Camera video streaming support
- Screen sharing capabilities
- Audio visualization and level monitoring
- Virtual avatar integration
- Light/dark theme switching with system preference detection
- Customizable branding, colors, and UI text via configuration

This template is built with Next.js and is free for you to use or modify as you see fit.

### Project structure

This frontend uses the [Agents UI](https://livekit.io/ui) components for core UI elements like media controls, audio visualizers, chat transcripts, and providing session data. Shadcn installs components into `components/` folder so you can customize them like any other local component.

```
agent-frontend/
├── app/
│   ├── api/
├── components/
│   ├── agents-ui/     - Agents UI components
│   ├── ai-elements/   - AI Elements components
│   ├── app/           - App-specific components
│   ├── ui/            - Primitive shadcn/ui components
├── fonts/
├── hooks/
├── lib/
├── public/
└── package.json
```

Business logic lives within the `components/app` folder. It's here where the application's state and behavior is managed and the various Shadcn UI components are composed together.

| File                  | Description                                                                                                                                           |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `session-view.tsx`    | Initializes the application, and LiveKit session. Renders the view controller and session UI including chat transcript, media tiles, and control bar. |
| `view-controller.tsx` | Manages the transitions between the welcome and session views based on the LiveKit session state.                                                     |
| `welcome-view.tsx`    | Renders the welcome UI when the LiveKit session is not connected.                                                                                     |
| `chat-transcript.tsx` | Manages the chat transcript transitions.                                                                                                              |
| `tile-layout.tsx`     | Manages the layout and transition of media tiles in various application states.                                                                       |

### Component usage

Most Agents UI components require access to a LiveKit session object for access to values like agent state or audio tracks. A Session object can be created from a [TokenSource](/reference/client-sdk-js/variables/TokenSource.html), and provided by wrapping the component in an [AgentSessionProvider](/reference/components/shadcn/component/agent-session-provider).

See [`components/app/app.tsx`](./components/app/app.tsx) for an example of how this is done in this app.

### Customizing components

Agents UI components, like most Shadcn compopnents, take as many primitive attributes as possible. For example, the [AgentControlBar](/reference/components/shadcn/component/agent-control-bar/page.mdoc) component extends `HTMLAttributes<HTMLDivElement>`, so you can pass any props that a div supports. This makes it easy to extend the component with your own styles or functionality.

You can edit any Agents UI component's source code in the `components/agents-ui` directory. For style changes, we recommend passing in tailwind classes to override the default styles. Take a look at the source code to get a sense of how to override a component's default styles.

### Updating components

To update the Agents UI components to the latest publication, run the following command:

```bash
pnpm shadcn:install
```

> [!NOTE]
> The CLI will ask before overwriting any modified files so you can avoid losing any customizations you might have made.

### Installing components

```bash
pnpm dlx shadcn@latest add @agents-ui/{component-name-a} @agents-ui/{component-name-b}
```

## Getting started

> [!TIP]
> If you'd like to try this application without modification, you can deploy an instance in just a few clicks with [LiveKit Cloud Sandbox](https://cloud.livekit.io/projects/p_/sandbox/templates/agent-starter-react).

[![Open on LiveKit](https://img.shields.io/badge/Open%20on%20LiveKit%20Cloud-002CF2?style=for-the-badge&logo=external-link)](https://cloud.livekit.io/projects/p_/sandbox/templates/agent-starter-react)

Run the following command to automatically clone this template.

```bash
lk app create --template agent-frontend
```

Then run the app with:

```bash
pnpm install
pnpm dev
```

And open http://localhost:3000 in your browser.

You'll also need an agent to speak with. Try my developed real-time voice-enabled AI agent using LiveKit and Google Gemini API to communicate with users naturally. Also, RAG has been integrated to retrieve context from the knowledge base.

## Configuration

We tweaked agent-starter-react [https://github.com/livekit-examples/agent-starter-react] so that it works with our use case, which is a voice agent for WE-helphub.

#### Example: App configuration (`app-config.ts`)

```ts
export const APP_CONFIG_DEFAULTS: AppConfig = {
  companyName: 'LiveKit',
  pageTitle: 'Voice Agent powered by LiveKit and Gemini Live API',
  pageDescription: 'Your Advanced AI voice agent',

  supportsChatInput: true,
  supportsVideoInput: true,
  supportsScreenShare: true,
  isPreConnectBufferEnabled: true,

  logo: '/We_logo.png',
  accent: '#002cf2',
  logoDark: '/We_logo.png',
  accentDark: '#1fd5f9',
  startButtonText: 'Talk to we helphub agent',

  // agent dispatch configuration
  agentName: process.env.AGENT_NAME ?? undefined,

  // LiveKit Cloud Sandbox configuration
  sandboxId: undefined,
};
```

You can update these values in [`app-config.ts`](./app-config.ts) to customize branding, features, and UI text for your deployment as we do in this project

> [!NOTE]
> The `sandboxId` is for the LiveKit Cloud Sandbox environment.
> It is not used for local development.

#### Environment Variables

You'll also need to configure your LiveKit credentials in `.env.local` (copy `.env.example` if you don't have one):

```env
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret
LIVEKIT_URL=https://your-livekit-server-url

# Agent dispatch (https://docs.livekit.io/agents/server/agent-dispatch)
# Leave AGENT_NAME blank to enable automatic dispatch
# Provide an agent name to enable explicit dispatch
AGENT_NAME=
```

These are required for the voice agent functionality to work with your LiveKit project .
To utilize our version of the voice agent, please visit voice-agent-v7 repo [https://github.com/ahmedelsousi7/voice-agent-v7.git].
Note: (.env files) environment varibale-value must match in the two projects to connect the LiveKit agent(worker)
with the Web UI.

## Contributing

This work was customized from this template [https://github.com/livekit-examples/agent-starter-react]. It's open source.
