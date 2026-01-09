<!-- @format -->

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

#### Next.js (App Router)

Because `app/layout.tsx` is a Server Component, you should create a small **client wrapper** for the provider and use that from your layout:

```tsx
// app/appclerk-provider.tsx
"use client";

import { AppClerkProvider } from "appclerk-react";

export function AppClerkRootProvider({
	children,
}: {
	children: React.ReactNode;
}) {
	return (
		<AppClerkProvider
			apiKey={process.env.NEXT_PUBLIC_APPCLERK_API_KEY!}
			projectId={process.env.NEXT_PUBLIC_APPCLERK_PROJECT_ID!}
		>
			{children}
		</AppClerkProvider>
	);
}
```

```tsx
// app/layout.tsx
import type { Metadata } from "next";
import "./globals.css";
import { AppClerkRootProvider } from "./appclerk-provider";

export const metadata: Metadata = {
	/* ... */
};

export default function RootLayout({
	children,
}: {
	children: React.ReactNode;
}) {
	return (
		<html lang="en">
			<body>
				<AppClerkRootProvider>{children}</AppClerkRootProvider>
			</body>
		</html>
	);
}
```

#### Next.js (Pages Router)

```tsx
// pages/_app.tsx
import { AppClerkProvider } from "appclerk-react";

export default function App({ Component, pageProps }) {
	return (
		<AppClerkProvider
			apiKey={process.env.NEXT_PUBLIC_APPCLERK_API_KEY}
			projectId={process.env.NEXT_PUBLIC_APPCLERK_PROJECT_ID}
		>
			<Component {...pageProps} />
		</AppClerkProvider>
	);
}
```

#### Vite / Create React App

```tsx
// src/main.tsx or src/index.tsx
import { AppClerkProvider } from "appclerk-react";
import { createRoot } from "react-dom/client";
import App from "./App";

const root = createRoot(document.getElementById("root")!);

root.render(
	<AppClerkProvider
		apiKey={import.meta.env.VITE_APPCLERK_API_KEY}
		projectId={import.meta.env.VITE_APPCLERK_PROJECT_ID}
	>
		<App />
	</AppClerkProvider>
);
```

### Components

#### Privacy Policy

```tsx
"use client";
import { PrivacyPolicy } from "appclerk-react";

export function MyPrivacyPolicy() {
	return <PrivacyPolicy renderMode="native" displayMode="inline" />;
}
```

#### Terms of Service

```tsx
"use client";
import { TermsOfService } from "appclerk-react";

export function MyTerms() {
	return <TermsOfService renderMode="native" displayMode="modal" />;
}
```

#### Compliance Badge

```tsx
"use client";
import { ComplianceBadge } from "appclerk-react";

export function MyBadge() {
	return <ComplianceBadge style="compact" showLink={true} />;
}
```

#### Compliance Widget

```tsx
"use client";
import { ComplianceWidget } from "appclerk-react";

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
import { useDocumentContent } from "appclerk-react";

function MyComponent() {
	const { data, loading, error, refetch } = useDocumentContent({
		documentType: "privacy-policy",
	});

	if (loading) return <div>Loading...</div>;
	if (error) return <div>Error: {error.message}</div>;

	return <div>{data?.content}</div>;
}
```

#### useComplianceStatus

```tsx
import { useComplianceStatus } from "appclerk-react";

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

## Style Guide

AppClerk React SDK provides comprehensive styling options for all markdown elements. You can style every aspect of your legal documents using either CSS classes (`className` and `markdownClassNames`) or inline styles (`customStyles`).

### Styling Methods

#### 1. Container Styling

Use the `className` prop to style the document container:

```tsx
<TermsOfService className="max-w-4xl mx-auto p-6 bg-white rounded-lg shadow-lg" />
```

#### 2. CSS Classes (`markdownClassNames`)

Apply CSS classes to specific markdown elements using the `markdownClassNames` prop. This is ideal when using CSS frameworks like Tailwind CSS or when you have global styles:

```tsx
<TermsOfService
	markdownClassNames={{
		container: "max-w-4xl mx-auto",
		h1: "text-3xl font-bold text-gray-900 mb-4",
		h2: "text-2xl font-semibold text-gray-800 mt-8 mb-3",
		h3: "text-xl font-medium text-gray-700 mt-6 mb-2",
		p: "text-gray-700 mb-4 leading-relaxed",
		ul: "list-disc ml-6 mb-4 space-y-2",
		ol: "list-decimal ml-6 mb-4 space-y-2",
		li: "text-gray-700 mb-2",
		a: "text-blue-600 hover:text-blue-800 underline",
		strong: "font-semibold text-gray-900",
		em: "italic text-gray-700",
		code: "bg-gray-100 px-2 py-1 rounded text-sm font-mono",
		pre: "bg-gray-900 text-gray-100 p-4 rounded-lg overflow-x-auto",
		blockquote: "border-l-4 border-blue-500 pl-4 italic text-gray-600",
		hr: "my-8 border-gray-300",
		table: "w-full border-collapse mb-4",
		th: "bg-gray-100 p-3 text-left font-semibold border",
		td: "p-3 border",
	}}
/>
```

#### 3. Inline Styles (`customStyles`)

Apply inline styles using the `customStyles` prop. This gives you complete control over every element:

```tsx
<TermsOfService
	customStyles={{
		container: {
			maxWidth: "800px",
			margin: "0 auto",
			padding: "2rem",
			backgroundColor: "#ffffff",
			borderRadius: "8px",
			boxShadow: "0 2px 8px rgba(0,0,0,0.1)",
		},
		h1: {
			fontSize: "2.5rem",
			fontWeight: "bold",
			color: "#1a1a1a",
			marginTop: "2rem",
			marginBottom: "1rem",
			fontFamily: "Georgia, serif",
		},
		h2: {
			fontSize: "2rem",
			fontWeight: "600",
			color: "#2d2d2d",
			marginTop: "1.5rem",
			marginBottom: "0.75rem",
		},
		p: {
			fontSize: "1rem",
			lineHeight: "1.75",
			color: "#4a4a4a",
			marginBottom: "1rem",
			fontFamily: "system-ui, sans-serif",
		},
		ul: {
			marginBottom: "1rem",
			paddingLeft: "1.5rem",
			listStyleType: "disc",
		},
		ol: {
			marginBottom: "1rem",
			paddingLeft: "1.5rem",
			listStyleType: "decimal",
		},
		li: {
			marginBottom: "0.5rem",
			lineHeight: "1.6",
		},
		a: {
			color: "#2563eb",
			textDecoration: "underline",
		},
		strong: {
			fontWeight: "600",
			color: "#1a1a1a",
		},
		em: {
			fontStyle: "italic",
			color: "#4a4a4a",
		},
		code: {
			backgroundColor: "#f3f4f6",
			padding: "0.125rem 0.375rem",
			borderRadius: "0.25rem",
			fontSize: "0.875em",
			fontFamily: "monospace",
			color: "#1f2937",
		},
		pre: {
			backgroundColor: "#1f2937",
			color: "#f9fafb",
			padding: "1rem",
			borderRadius: "0.5rem",
			overflow: "auto",
			fontSize: "0.875rem",
		},
		blockquote: {
			borderLeft: "4px solid #3b82f6",
			paddingLeft: "1rem",
			marginLeft: 0,
			fontStyle: "italic",
			color: "#6b7280",
		},
		hr: {
			border: "none",
			borderTop: "1px solid #e5e7eb",
			margin: "2rem 0",
		},
		table: {
			width: "100%",
			borderCollapse: "collapse",
			marginBottom: "1rem",
		},
		th: {
			padding: "0.75rem",
			textAlign: "left",
			fontWeight: "600",
			border: "1px solid #e5e7eb",
			backgroundColor: "#f9fafb",
		},
		td: {
			padding: "0.75rem",
			border: "1px solid #e5e7eb",
		},
	}}
/>
```

### Combining Methods

You can combine `className`, `markdownClassNames`, and `customStyles` for maximum flexibility:

```tsx
<TermsOfService
	className="my-custom-container"
	markdownClassNames={{
		h1: "text-3xl font-bold",
		p: "text-gray-700",
	}}
	customStyles={{
		h1: {
			color: "#1a1a1a", // Overrides any conflicting CSS class
		},
		p: {
			lineHeight: "1.8", // Adds to CSS class styles
		},
	}}
/>
```

### Styleable Elements

The following markdown elements can be styled:

| Element           | `markdownClassNames` Key | `customStyles` Key | Description            |
| ----------------- | ------------------------ | ------------------ | ---------------------- |
| Container         | `container`              | `container`        | Outer wrapper div      |
| Markdown wrapper  | `markdown`               | `markdown`         | Inner markdown wrapper |
| Heading 1         | `h1`                     | `h1`               | Main headings          |
| Heading 2         | `h2`                     | `h2`               | Section headings       |
| Heading 3         | `h3`                     | `h3`               | Subsection headings    |
| Heading 4         | `h4`                     | `h4`               | Level 4 headings       |
| Heading 5         | `h5`                     | `h5`               | Level 5 headings       |
| Heading 6         | `h6`                     | `h6`               | Level 6 headings       |
| Paragraph         | `p`                      | `p`                | Text paragraphs        |
| Unordered list    | `ul`                     | `ul`               | Bullet lists           |
| Ordered list      | `ol`                     | `ol`               | Numbered lists         |
| List item         | `li`                     | `li`               | Individual list items  |
| Link              | `a`                      | `a`                | Hyperlinks             |
| Strong/Bold       | `strong`                 | `strong`           | Bold text              |
| Emphasis/Italic   | `em`                     | `em`               | Italic text            |
| Inline code       | `code`                   | `code`             | Inline code snippets   |
| Code block        | `pre`                    | `pre`              | Code blocks            |
| Blockquote        | `blockquote`             | `blockquote`       | Quoted text            |
| Horizontal rule   | `hr`                     | `hr`               | Dividers               |
| Table             | `table`                  | `table`            | Tables                 |
| Table head        | `thead`                  | `thead`            | Table header           |
| Table body        | `tbody`                  | `tbody`            | Table body             |
| Table row         | `tr`                     | `tr`               | Table rows             |
| Table header cell | `th`                     | `th`               | Header cells           |
| Table data cell   | `td`                     | `td`               | Data cells             |

### Common Styling Examples

#### Dark Mode

```tsx
<TermsOfService
	theme="dark"
	customStyles={{
		container: {
			backgroundColor: "#1f2937",
			color: "#f9fafb",
		},
		h1: { color: "#ffffff" },
		p: { color: "#d1d5db" },
		a: { color: "#60a5fa" },
	}}
/>
```

#### Minimalist Design

```tsx
<TermsOfService
	customStyles={{
		container: {
			maxWidth: "600px",
			margin: "0 auto",
			padding: "3rem 1rem",
		},
		h1: {
			fontSize: "1.75rem",
			fontWeight: "300",
			marginBottom: "2rem",
			letterSpacing: "-0.02em",
		},
		p: {
			fontSize: "0.9375rem",
			lineHeight: "1.7",
			marginBottom: "1.25rem",
		},
		ul: {
			paddingLeft: "1.25rem",
		},
		li: {
			marginBottom: "0.5rem",
		},
	}}
/>
```

#### Print-Friendly Styles

```tsx
<TermsOfService
	markdownClassNames={{
		container: "print:max-w-none print:p-0",
		h1: "print:text-2xl print:break-after-avoid",
		h2: "print:text-xl print:break-after-avoid",
		p: "print:text-sm print:leading-relaxed",
		a: "print:text-black print:no-underline",
	}}
/>
```

### Legacy Support

For backward compatibility, the following legacy style keys are still supported:

- `customStyles.heading` - Applies to all headings (h1-h6)
- `customStyles.link` - Alias for `customStyles.a`

These will be merged with element-specific styles, with element-specific styles taking precedence.

## License

MIT

Official documentation: https://appclerk.dev/docs/sdk/react

Appclerk Core: https://appclerk.dev/docs/sdk/core

Appclerk React Native/Expo: https://appclerk.dev/docs/sdk/react-native
