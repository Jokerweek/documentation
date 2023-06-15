---
layout: default
title: Pages
parent: Source
grand_parent: Structure
---
# /pages/ directory

<!-- explain folder
- what it has to do with Nextjs
- why tsx
- example page structure-->

This folder contains all the pages of the application. Pages are the smallest unit of routing in [Next.js](https://nextjs.org/). Each file in this folder represents a page in the application. The structure of each page is as follows:

```tsx
// pages/[PAGE_NAME].tsx
import { SubPage } from "~/components";

export default function Sponsors() {
  return (
    <SubPage title="Sponsors" image="/images/[TOP_IMAGE_OF_PAGE]">
        [PAGE_CONTENT]
    </SubPage>
  );
}
```
{: .highlight }
With the exection of the `index.tsx` and `_app.tsx` files. Which are respectively the home page and the main application file.

## _App.tsx
### Overview

This is the main application file in a Next.js project, utilizing TypeScript and incorporating the Vercel Analytics, Recoil state management, and custom local fonts.

### Structure

```tsx
import { Analytics } from "@vercel/analytics/react";
import type { AppProps } from "next/app";
import localFont from "next/font/local";
import Head from "next/head";
import { RecoilRoot } from "recoil";
import "~/styles/globals.css";
...
```

The imports are bringing in necessary libraries and types for the application. Notably, `@vercel/analytics/react` for Vercel's analytics suite, `next/app` for base application properties, `next/font/local` for loading local fonts, `next/head` for adding elements to the head of the page, and `recoil` for state management. Also, the global styles are imported from `~/styles/globals.css`.

```tsx
const titleFont = localFont({ ... });
const subTitleFont = localFont({ ... });
const bodyFont = localFont({ ... });
```

These lines define custom local fonts. You can change them by altering the paths and variables accordingly.

```tsx
export default function App({ Component, pageProps }: AppProps) { ... }
```

This is the main application component, which renders all other components. It receives `Component` and `pageProps` as props, which represent the current page and its properties, respectively.

```tsx
<Head>
  <link rel="icon" href="/images/logo/Logo-Insta-fb.jpg" />
</Head>
```

Inside the `Head` tag, you can set the favicon (browser-tab logo) for your website. Change the `href` to point to your desired favicon image.

```tsx
<main className={...}>
  <Component {...pageProps} />
</main>
```
The `main` tag has assigned class names related to the fonts defined before and includes other font and typography styles. Inside it, the current page (`Component`) is rendered with its properties (`pageProps`).

```tsx
<Analytics />
```

Finally, the `Analytics` component from Vercel is rendered, which automatically starts tracking user interactions and pageviews.

## index.tsx

This file represents the landing page of the application. It is the first page that is loaded when the website is opened. It contains the sponsors as well as the countdown timer to the event. Which once the event starts, will be replaced by by a button to the home page.

### Structure

```tsx
const Timer = dynamic(() => import("~/components/Timer"), {
  ssr: false,
});
```

Here, the `Timer` component is loaded dynamically, which means it will not be server-side rendered (`ssr: false`). This is useful for components that have dependencies on `window` in the browser, in this case, the `Timer` component uses the `window` object to know the current time.

```tsx
export default function Home() { ... }
```

```tsx
<Image
  width={300}
  height={300}
  src="/images/logo/Spel-officieel-Transparant.png"
  alt="logo"
  className="mt-10 w-[300px] max-w-[min(60vw,_60vh)]"
/>
```

This `Image` tag is from the `next/image` library which is an extension of the standard HTML `img` element, providing better performance and user experience. It displays the logo image at a certain width and height. The source (`src`) is pointing to the image file, and the `alt` attribute provides alternative text describing the image.

It ensures that the image send to each user is optimized for their device and connection speed.

```tsx
<Timer deadline="2023-03-27T09:30:00+01:00">
  <Link
    className="..."
    href="home" >
    Ga door
  </Link>
</Timer>
```

The custom `Timer` component is counting down to a specific deadline. Inside it, a `Link` component is used, providing navigation to the `home` page. The `Link` component is from the `next/link` library. It ensures that the page is preloaded before the user clicks on it, providing a better user experience.

<img src="/images/icons/link.svg">
[Timer component](http://example.com/)

## 
