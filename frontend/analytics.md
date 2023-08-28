# Analytics

This guide is to help setup google analytics in Nextjs projects. Aim is to log page views and certain events, when required.

## Set up Google Analytics

### Setup utility functions

1. Create a new file `utils/ga.js` with the following code:

```js
// log the pageview with their URL
export const pageview = (url) => {
  window.gtag('config', process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS, {
    page_path: url,
  });
};

// log specific events happening.
export const event = ({ action, params }) => {
  window.gtag('event', action, params);
};
```

2. Create a new component `GoogleAnalytics.tsx`

```js
import Script from "next/script";

const GoogleAnalytics = ({ ga_id }: { ga_id: string }) => (
  <>
    <Script
      async
      src={`https://www.googletagmanager.com/gtag/js? 
      id=${ga_id}`}
    ></Script>
    <Script
      id="google-analytics"
      dangerouslySetInnerHTML={{
        __html: `
          window.dataLayer = window.dataLayer || [];
          function gtag(){dataLayer.push(arguments);}
          gtag('js', new Date());

          gtag('config', '${ga_id}');
        `,
      }}
    ></Script>
  </>
);
export default GoogleAnalytics;
```

### Add env variable

Add `NEXT_PUBLIC_GOOGLE_ANALYTICS` as variable in the `.env` file with the GTAG ID provided while setting up.

### In pages directory (upto Next 12)

1. Import and use `pageview` function in the `pages/_app.tsx` file

```js
import GoogleAnalytics from "@/components/GoogleAnalytics";

const MyApp: React.FC<Props> = ({ Component, pageProps }) => {
    // ...
    return (
        <>
            { process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS ? (
                <GoogleAnalytics ga_id={process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS} />
            ) : null }
            <Component {...pageProps} />
        </>
    );
};

export default MyApp;

```

### In apps directory (Next 13+)

1. Add the component in the root `layout.tsx` file

```js
import GoogleAnalytics from "@/components/GoogleAnalytics";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
      <html lang="en">
        <body>
          { process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS ? (
            <GoogleAnalytics ga_id={process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS} />
          ) : null }
          // ... your other code content here.
        </body>
      </html>
  );
}
```
