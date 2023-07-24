---
layout: default
title: Pages
parent: Source
grand_parent: Structure
---
# /pages/ directory

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
With the exection of the `home.tsx`, `index.tsx` and `_app.tsx` files. Which are respectively the home page, the landing page and the main application file.

---

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

### Dependencies

- [Timer component](components#timertsx) for the countdown timer
- [SponsorsLogos component](components#sponsorslogostsx) for the sponsors logos

## home.tsx

The file has two primary functions, `Mobile()` and `Desktop()`, for rendering the home page on mobile and desktop devices, respectively. Each function returns a collage of animated pictures on a backgound image. The collage is made up of clickable images (`Pin` components) that link to various pages on the website.

{: .important }
> To make this whole collage responsive, all dimentions are expessed in function of the viewport width `vw`. This will result in a collage that scales with the viewport width and adapt to different screen sizes, while keeping the initial proportions.

### Structure

The `Pin` component is utilized to place various clickable images at specific locations on the screen.

```tsx
<Pin
  infinite
  box={["112.5vw", "47vw", "11vw", "10.6vw"]}
  skew="0, 24deg"
  href="weekmenu"
  order={2}
>
  <Image
    width={300}
    height={150}
    className="w-[32.4vw] -translate-x-[11vw] -translate-y-[1.9vw]"
    src="/images/bg/mobielKlikvelden/Weekmenu.png"
    alt="logo"
  />
</Pin>
```	
The following attributes are used to customize the `Pin` component:

- `infinite`: Determines whether the animation should loop infinitely. This attribute is optional and defaults to `false`.

- `box`: Determines the size and position of the `Pin` component, more precisely the clickable area. The first two values determine the **position** of the `Pin` component, while the last two values determine the **size** of the `Pin` component.
```ts
box={["top", "left", "height", "width"]}
```

- `skew`: Determines the skew of the `Pin` component's clickable are. The first value determines the skew along the horizontal axis, while the second value determines the skew along the vertical axis.

- `href`: Determines the link the user is directed to when they click on the `Pin` component.

- `order`: Determines the order in which the `Pin` component is rendered, to create a cascading revealing effect. This attribute is optional and defaults to `0`.

In the `Image` child:
- `width` and `height` are used to optimize the image for web. A maximum expected size required. (units = pixels)

- `className` is used to position the image on top of the clickable area. It uses the following format:
```ts
className="w-[width image] translate-x-[horizontal offset]
    ↪ translate-y-[vertical offset]"
```
There is also a `handleAnim()` function in the `Desktop` component that is designed to handle animation on the desktop version of the site. It triggers the reveal of the component when the user hovers over it.

Lastly, there's the `Home()` function, which serves as the default export of this file. This function uses the custom `useBreakpoint()` hook to determine whether to render the mobile or desktop version of the home page. The `useBreakpoint()` hook returns `true` or `false` based on the viewport width.

### Customization

To customize the website:

- **Change Images**: Modify the `src` attributes in the `Image` components to point to the new images you want to use.

- **Change Links**: Modify the `href` attributes in the `Pin` components to change where the user is directed when they click on a particular image.

- **Alter Position and Size of Elements**: Tweak the values in the `box` attribute of the `Pin` component AND of the the `Image` components.

- **Adjust Animation order**: Change the `order` attribute of the `Pin` components to change the order in which they are revealed.

{: .highlight }
> uncomment the following comment in the Pin component to visualise the clickable areas: ```// border: "1px solid red",```

### Dependencies

- [Pin component](components#pintsx) for the clickable images
- [useBreakpoint hook](hooks#usebreakpointhook) for switching between mobile and desktop versions

## expo.tsx

The `Expo` component shows a Google Maps iframe and provides a link to the map. It's encapsulated within a `SubPage` component that manages the layout and styles common to all the subpages. It also provides the title, footer and back button.

### Structure

```tsx
const googleMapsUrl = "...";
const googleMapsEmbedUrl = "...";
```

These are two constant strings representing URLs. `googleMapsUrl` is a link to the actual Google Maps page, while `googleMapsEmbedUrl` is a link used for embedding the map in an `iframe`. Both are provided by Google Maps when creating a [custom map](https://mymaps.google.com).

```tsx
<SubPage title="Expo" image="/images/bg/doorklik/expo.png"> ... </SubPage>
```

The `Expo` component is wrapped in a `SubPage` component. `SubPage` accepts `title` and `image` props that set the title and top image of the subpage, respectively.

```tsx
<a href={googleMapsUrl} className="underline underline-offset-4">GOOGLE MAPS</a>
```

This `a` tag is an HTML anchor used to create a hyperlink to `googleMapsUrl`.

```tsx
<iframe src={googleMapsEmbedUrl} width="100%" height="480" title="Google Maps" />
```

This `iframe` tag is used to embed the Google Maps page in the `Expo` page. It uses `googleMapsEmbedUrl` as the source.

## huisregels.tsx

Speaks for itself. It's just a list of rules.
```tsx
<ul>
  <li>...</li>
  <li>...</li>
  ...
</ul>
```

## randactiviteiten.tsx
This page consists primarily of text which make intensive use of the native HTML tags `<h1>` to `<h4>`, `<br>` and `<a>`. Which stands for titles, back to line and links respectively. The customization is handled with tailwind classes.

```tsx	
<Scores BA1={0} BA2={0} BA3={0} MA1={0} MA2={0} />
```
It also illustrates the current score of the different years in the "Jarenstrijd". The flags animation is made with the `<Scores>` component.

### Dependencies

- [Scores component](components#scorestsx) for the flags animation

## sponsors.tsx

This page only contains the sponsors logos. The logos are displayed using the `SponsorsLogos` component.

### Dependencies

- [SponsorsLogos component](components#sponsorslogostsx) for the sponsors logos

lol