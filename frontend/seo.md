# SEO: Enhancing Visibility and Organic Traffic

Search Engine Optimization (SEO) plays a vital role in ensuring that publicly accessible dashboards gain visibility among users, leading to increased organic traffic. One of the key elements of effective SEO is optimizing meta tags. This guide focuses on implementing meta tags for improved SEO.

We can implement a good SEO by adding sitemap, robots file (if required), and meta tags. This guide will talk about meta tags.

## For Next.js Versions Earlier than 13.0

Begin by creating a new component named `Seo.tsx`:

```js
import Head from 'next/head';

const Seo: React.FC<{ seo?: any }> = ({ seo }) => {
  const title = seo && seo.title ? seo.title : 'Default Title';
  const description = seo && seo.description ? seo.description : 'Default Description';

  const url = `BaseUrl`;
  return (
      <Head>
        {title && (
          <>
            <title>{title}</title>
            <meta property="og:title" content={title} />
            <meta name="twitter:title" content={title} />
          </>
        )}
        {description && (
          <>
            <meta name="description" content={description} />
            <meta property="og:description" content={description} />
            <meta name="twitter:description" content={description} />
          </>
        )}
        {url && (
          <>
            <meta property="og:url" content={url} />
            <meta property="twitter:url" content={url} />
          </>
        )}

        {/* type */}
        <meta property="og:type" content="website" />
        <meta property="twitter:card" content="summary_large_image" />

        {/* Image */}
        <meta
          property="og:image"
          content={`${url}/assets/images/constituency_home.png`}
        />
        <meta
          property="twitter:image"
          content={`${url}/assets/images/constituency_home.png`}
        />

        <meta name="application-name" content="Constituency Dashboard" />
      </Head>
  );
};

export default Seo;
```

Now, you can use this component on every page in your project to obtain dynamic SEO for each specific page:

```js
  const seo = {
    title: pageProps?.meta?.title || 'Welcome - District Dashboard',
    description:
      pageProps?.meta?.description ||
      'A unique, one-of-its-kind dashboard that opens up district-wise fiscal information for several centrally sponsored and central sector schemes.',
  };

  // ....

  return (
    <>
        <Seo seo={seo} />
        // ....
    </>
  )
```

## For Next.js Versions 13+

With the introduction of layouts, there's a new way to implement metadata. Add the following code block to your root `layout` file:

```js
import { siteConfig } from '@/config/site';

export async function generateMetadata({ params: { locale } }: Props) {
  return {
    metadataBase: new URL(siteConfig.url),
    title: {
      default: siteConfig.name,
      template: `%s | ${siteConfig.name}`,
    },
    description: siteConfig.description,
    keywords: [
      'Next.js',
      'React',
      'Server Components',
      'Radix UI',
      'OPub',
      'Open Publishing',
    ],
    authors: [
      {
        name: 'CivicDataLab',
        url: 'https://civicdatalab.in/',
      },
    ],
    creator: 'CivicDataLab',
    themeColor: [
      { media: '(prefers-color-scheme: light)', color: 'white' },
      { media: '(prefers-color-scheme: dark)', color: 'black' },
    ],
    openGraph: {
      type: 'website',
      locale: 'en_US',
      url: siteConfig.url,
      title: siteConfig.name,
      description: siteConfig.description,
      siteName: siteConfig.name,
      images: [`${siteConfig.url}/og.png`],
    },
    twitter: {
      card: 'summary_large_image',
      title: siteConfig.name,
      description: siteConfig.description,
      images: [`${siteConfig.url}/og.png`],
      creator: 'CivicDataLab',
    },
    icons: {
      icon: '/favicon.ico',
      shortcut: '/favicon-16x16.png',
      apple: `${siteConfig.url}/apple-touch-icon.png`,
    },
    manifest: `${siteConfig.url}/site.webmanifest`,
  };
}
```