<p align="center">
  <img src="https://appclerk.dev/images/logo-main.png" alt="AppClerk Core Banner" width="300" />
</p>

# appclerk-react

React SDK for AppClerk - React/Next.js components and hooks for compliance infrastructure.

## Installation

```bash
npm install appclerk-core appclerk-react
# or
yarn add appclerk-core appclerk-react
# or
pnpm add appclerk-core appclerk-react
```

## Usage

### Setup Provider

```tsx
// app/layout.tsx (Next.js)
import { AppClerkProvider } from 'appclerk-react';

export default function RootLayout({ children }) {
  return (
    <AppClerkProvider
      apiKey={process.env.APPCLERK_API_KEY}
      projectId={process.env.APPCLERK_PROJECT_ID}
    >
      {children}
    </AppClerkProvider>
  );
}
```

### Components

#### Privacy Policy

```tsx
'use client';
import { PrivacyPolicy } from 'appclerk-react';

export function MyPrivacyPolicy() {
  return (
    <PrivacyPolicy
      renderMode="native"
      displayMode="inline"
    />
  );
}
```

#### Terms of Service

```tsx
'use client';
import { TermsOfService } from 'appclerk-react';

export function MyTerms() {
  return (
    <TermsOfService
      renderMode="native"
      displayMode="modal"
    />
  );
}
```

#### Compliance Badge

```tsx
'use client';
import { ComplianceBadge } from 'appclerk-react';

export function MyBadge() {
  return (
    <ComplianceBadge
      style="compact"
      showLink={true}
    />
  );
}
```

#### Compliance Widget

```tsx
'use client';
import { ComplianceWidget } from 'appclerk-react';

export function MyWidget() {
  return (
    <ComplianceWidget
      position="bottom-right"
      showBadge={true}
      showLinks={true}
    />
  );
}
```

### Hooks

#### useDocumentContent

```tsx
import { useDocumentContent } from 'appclerk-react';

function MyComponent() {
  const { data, loading, error, refetch } = useDocumentContent({
    documentType: 'privacy-policy',
  });

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return <div>{data?.content}</div>;
}
```

#### useComplianceStatus

```tsx
import { useComplianceStatus } from 'appclerk-react';

function MyComponent() {
  const { data, loading } = useComplianceStatus({
    refreshInterval: 60000, // Refresh every minute
  });

  if (loading) return <div>Loading...</div>;

  return <div>Score: {data?.score}%</div>;
}
```

## API Reference

### Components

- `<AppClerkProvider />` - Context provider (required)
- `<PrivacyPolicy />` - Privacy policy document viewer
- `<TermsOfService />` - Terms of service document viewer
- `<PrivacyPolicyModal />` - Privacy policy modal
- `<TermsModal />` - Terms modal
- `<ComplianceBadge />` - Compliance verification badge
- `<ComplianceWidget />` - Compliance widget with badge and links
- `<ComplianceStatus />` - Detailed compliance status display

### Hooks

- `useAppClerk()` - Access AppClerk instance
- `useDocumentContent()` - Fetch document markdown
- `useDocumentMetadata()` - Fetch document metadata
- `useComplianceStatus()` - Fetch compliance status

## Rendering Modes

### Native Mode (Default)

Renders markdown natively using `react-markdown`:

- Better performance
- Full styling control
- Native feel
- Better offline support

### Webview Mode

Loads hosted HTML in an iframe:

- Exact rendering match
- Zero styling needed
- Always up-to-date
- Simpler implementation

## License

MIT

Official documentation: https://appclerk.dev/docs/sdk/react-native
Appclerk Core: https://appclerk.dev/docs/sdk/core
