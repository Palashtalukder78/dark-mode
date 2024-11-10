STEP 1: Edit TailwindCSS config file

```javascript
// tailwind.config.ts
/\*_ @type {import('tailwindcss').Config} _/;
module.exports = {
  darkMode: "class",
  // ...
};
```

STEP 2: next-themes

Command: npm i next-themes
First, create a providers.tsx file inside your app folder like this:

```javascript
// app/providers.jsx
"use client";

import { ThemeProvider } from "next-themes";

export function Providers({ children }) {
  return (
    <ThemeProvider attribute="class" defaultTheme="system" enableSystem>
      {children}
    </ThemeProvider>
  );
}
```

STEP 3: add the <Providers> component to your root layout.tsx by placing it inside the <body> tag Like:

```javascript
// app/layout.tsx
import { Providers } from "./providers";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

Note: suppressHydrationWarning in <html> tag. AND DARK CLASSES WILL BE IN THS CODE..

step 5: useTheme
Your implementation of next-themes is not complete until you create a component allowing users to manually change the themes. I called my component ThemeSwitch.Jsx.

```javascript
// app/components/ThemeSwitch.tsx
"use client";

import { FiSun, FiMoon } from "react-icons/fi";
import { useState, useEffect } from "react";
import { useTheme } from "next-themes";
import Image from "next/image";

export default function ThemeSwitch() {
  const [mounted, setMounted] = useState(false);
  const { setTheme, resolvedTheme } = useTheme();

  useEffect(() => setMounted(true), []);

  if (!mounted)
    return (
      <Image
        src="data:image/svg+xml;base64,PHN2ZyBzdHJva2U9IiNGRkZGRkYiIGZpbGw9IiNGRkZGRkYiIHN0cm9rZS13aWR0aD0iMCIgdmlld0JveD0iMCAwIDI0IDI0IiBoZWlnaHQ9IjIwMHB4IiB3aWR0aD0iMjAwcHgiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PHJlY3Qgd2lkdGg9IjIwIiBoZWlnaHQ9IjIwIiB4PSIyIiB5PSIyIiBmaWxsPSJub25lIiBzdHJva2Utd2lkdGg9IjIiIHJ4PSIyIj48L3JlY3Q+PC9zdmc+Cg=="
        width={36}
        height={36}
        sizes="36x36"
        alt="Loading Light/Dark Toggle"
        priority={false}
        title="Loading Light/Dark Toggle"
      />
    );

  if (resolvedTheme === "dark") {
    return <FiSun onClick={() => setTheme("light")} />;
  }

  if (resolvedTheme === "light") {
    return <FiMoon onClick={() => setTheme("dark")} />;
  }
}
```
