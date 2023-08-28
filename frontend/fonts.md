# Efficient Font Loading

Our project's font requirements can be satisfied using Google Fonts. However, there are ways to optimize the font loading process for enhanced performance.

## Ensuring Proper Font Loading

Always ensure that the selected font is fetched and loaded correctly. It's possible that the font is installed on your system, leading you to see it, but the user might not experience the same.

## Pages Directory

Update your `pages/_app.tsx` with this bit of code:

```js
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] })
 
export default function MyApp({ Component, pageProps }) {
  return (
    <main className={inter.className}>
      <Component {...pageProps} />
    </main>
  )
}
```

In scenarios where the font doesn't support variable weight, include the font weight as follows:

```js
import { Roboto } from 'next/font/google'
 
const roboto = Roboto({
  weight: '400',
  subsets: ['latin'],
})
 
export default function MyApp({ Component, pageProps }) {
  return (
    <main className={roboto.className}>
      <Component {...pageProps} />
    </main>
  )
}
```

[More reading for pages dir](https://nextjs.org/docs/pages/building-your-application/optimizing/fonts)

## App directory

Similar to the `pages` directory, make adjustments to your root layout file within the `app` directory:

```js
import { Inter } from 'next/font/google'
 
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

[More reading for app dir](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)
