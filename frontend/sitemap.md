# Generating Sitemap and Submitting to Search Console

For any public-facing platform, having a sitemap submitted to search consoles is essential to ensure indexing by search engines. This helps broaden the platform's reach and visibility.

## Creating the Sitemap File

## Pages Directory

To create a sitemap using server-side rendering, you can utilize the following template. Place this file in `pages/sitemap.xml.js`:

```js
import fs from 'fs';
import { generateSlug } from 'utils/fetch';

const baseUrl = 'https://district.openbudgetsindia.org';
const Sitemap = () => {};

export const getServerSideProps = async function ({ res }) {
  // You can customize the logic for fetching and generating the list based on your platform

  // Define the JSON URL to fetch data for the sitemap
  const jsonUrl = 'some URL for fetching sitemap data';

  // Fetch JSON data
  const jsonData = await fetch(jsonUrl)
    .then((res) => res.json())
    .catch((e) => console.error(e));

  const sitemapPages = fs
    .readdirSync('pages') // Include all pages in the directory
    .filter((staticPage) => {
      return ![
        // Exclude pages that are not relevant
        '_app.tsx',
        '_document.tsx',
        '_error.tsx',
        'sitemap.xml.js',
        'index.tsx',
        '[state]',
        'explorer',
      ].includes(staticPage);
    })
    .map((staticPagePath) => {
      return `${baseUrl}/${staticPagePath}`; // Add static pages like resources, about, etc.
    });

  sitemapPages.unshift(baseUrl); // Remove duplicate of base URL

  Object.keys(jsonData).forEach((state) => {
    const slugged = generateSlug(state);
    sitemapPages.push(`${baseUrl}/${slugged}`);

    Object.values(jsonData[state]).forEach((dist) => {
      sitemapPages.push(
        `${baseUrl}/${slugged}/${dist.district_code_lg}`
      ); // Add District page for each state
    });
  });

  // The following section remains the same for all platforms
  const sitemap = `<?xml version="1.0" encoding="UTF-8"?>
    <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
      ${sitemapPages
        .map((url) => {
          return `
            <url>
              <loc>${url}</loc>
              <lastmod>${new Date().toISOString()}</lastmod>
              <changefreq>monthly</changefreq>
              <priority>1.0</priority>
            </url>
          `;
        })
        .join('')}
    </urlset>
  `;

  res.setHeader('Content-Type', 'text/xml');
  res.write(sitemap);
  res.end();

  return {
    props: {},
  };
};

export default Sitemap;
```

## App Directory (Next 13.3+)

For Next.js 13.3+ versions, creating a sitemap and other metadata files becomes easier. Refer to [this documentation](https://nextjs.org/docs/app/api-reference/file-conventions/metadata#file-based-metadata) for details. Place the following code in `app/sitemap.js`:

```js
export default async function sitemap() {
  const res = await fetch('https://.../posts');
  const allPosts = await res.json();
 
  const posts = allPosts.map((post) => ({
    url: `https://acme.com/blog/${post.slug}`,
    lastModified: post.publishedAt,
  }));
 
  const routes = ['', '/about', '/blog'].map((route) => ({
    url: `https://acme.com${route}`,
    lastModified: new Date().toISOString(),
  }));
 
  return [...routes, ...posts];
}
```

## Submitting to Search Console

- Go to Google Search Console.
- Select your property (website) or add a new one if it's not already listed.
- In the left sidebar, click on "Sitemaps."
- Click the "Add/Test Sitemap" button.
- Enter the URL of your sitemap (e.g., https://yourwebsite.com/sitemap.xml) and click "Submit"