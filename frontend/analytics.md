# Guide to Setting Up Google Analytics in Next.js Projects

The purpose of this guide is to help you seamlessly log page views and track specific events whenever required.

## Setting Up Google Analytics

### Creating Utility Functions

1. Begin by creating a new file named `utils/ga.js` with the following code:

```js
// Log the pageview with its URL
export const pageview = (url) => {
  window.gtag('config', process.env.NEXT_PUBLIC_GOOGLE_ANALYTICS, {
    page_path: url,
  });
};

// Log specific events
export const event = ({ action, params }) => {
  window.gtag('event', action, params);
};
```

2. Next, establish a new component called `GoogleAnalytics.tsx`:

```js
import Script from "next/script";

const GoogleAnalytics = ({ ga_id }: { ga_id: string }) => (
  <>
    <Script
      async
      src={`https://www.googletagmanager.com/gtag/js? 
      id=${ga_id}`}
    />
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
    />
  </>
);
export default GoogleAnalytics;
```

### Adding an Environment Variable

In your .env file, introduce a new environment variable named `NEXT_PUBLIC_GOOGLE_ANALYTICS` with the provided GTAG ID during setup.

### In the `pages` directory (up to Next 12)

1. Open the `pages/_app.tsx` file and implement the following changes:

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

### In the `apps` directory (Next 13+)

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

## Logging Custom Events

In addition to tracking page views, Google Analytics enables you to log custom events. For instance, let's say you want to track the number of times users click the download button. Follow these steps:

```js
import { event } from '@/utils/ga';

function handleDownload() {
  // ... download logic

  event({
      action: 'download-dataset',
      params: {
        path: router.pathname,
      },
    });
}
```
