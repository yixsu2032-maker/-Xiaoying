# App Navigator

> Open official websites in one click and discover popular apps worldwide.

An app directory featuring widely used services in mainland China and around the world. Browse by category, search instantly, read app descriptions, and visit official websites. The current catalog includes **1,217 apps and services**, **12 categories**, **20 languages**, and **8 themes**. The default interface uses Chinese and the light theme.

## Features

- **Instant search:** Find apps by name, keyword, or supported Chinese and English brand aliases, including NetEase, Tencent, ByteDance, Alibaba, Google, and Meta.
- **Category filters:** Browse apps by category and see the number of entries in each category.
- **Two action modes:** Switch between “Visit Website” and “Copy Link.” App card buttons update to match the selected mode.
- **Copy feedback:** A successful copy displays “Copied” for 5 seconds.
- **App details:** Click an app's icon or name to view a larger icon, its name, publisher or operator, and a description of its purpose.
- **Multilingual interface:** Changing the language updates the interface and app descriptions. Some descriptions use category-based templates; brand and operator names may remain in their original language.
- **Theme selection:** Choose from Light, Dark, Ocean, Forest, Sunset, Purple, Rose, and Slate.
- **Animated mode switching:** The action selector uses an approximately 0.25-second sliding transition. Related animations are disabled when reduced motion is preferred.
- **Responsive layout:** The page adapts to desktop and mobile screen widths.

## App Categories

These counts reflect the current source code and may change as the catalog is updated.

| Category | Apps |
| --- | ---: |
| Social | 79 |
| Messaging | 72 |
| Video | 84 |
| Music | 75 |
| AI | 118 |
| Games | 139 |
| Shopping | 86 |
| Finance | 82 |
| Productivity | 225 |
| News | 86 |
| Travel | 87 |
| Health | 84 |
| **Total** | **1,217** |

## Local Use

1. Open `应用导航.html` in Chrome, Edge, or another modern browser.
2. Enter an app or brand name in the search box, or select a category.
3. Click an app's icon or name to read its description. Use its action button to visit the website or copy the link.
4. Adjust the language and theme using the menus at the top of the page.

The interface is bundled into the HTML file. You do not need Node.js or a development server to view it. External logos, official websites, and other remote resources still require an internet connection.

Language, theme, and action-mode preferences are not persisted. Refreshing the page restores Chinese, the light theme, and “Visit Website” mode.

## Files

| File | Purpose |
| --- | --- |
| `应用导航.html` | Complete webpage that opens directly in a browser. Copy and rename it to `index.html` for publishing. |
| `AppNavigator.tsx` | React / TypeScript source for further development. Rebuild the webpage after making changes. |
| `README.md` | Project documentation. |
| Icon audit lists and update records | Notes on selected icon sources, changes, and verification status. These files are not required to run the website. |

Editing the `.tsx` file does not automatically update the generated HTML. The project uses React 18, TypeScript/TSX, Tailwind CSS, and esbuild. The current local build script is `build.cjs` in the working directory.

## Publishing with GitHub Pages

Copy the latest `应用导航.html`, rename the copy to **`index.html`**, and upload it to the top level of your configured GitHub Pages publishing directory. You can include this README in the repository to introduce the project.

Configure the publishing source under **Settings → Pages** in your repository. Once deployment finishes, select **Visit site** to open the website. To publish updates, upload the latest `index.html` again; changing a local file does not automatically update the published site.

See the [official GitHub Pages guide](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site) for setup details.

## Icons and Content

Icons come from several sources, including official websites, app stores, official project accounts, brand icon libraries, and partner pages. Dola's icon is embedded in the webpage; most other icons remain remote images.

Apps without a dedicated icon override may still use a website favicon service. A globe, gray arrow, or initials may be a placeholder rather than the app's actual logo. Broken image links, access restrictions, and changes at the source can affect display. Changing the theme does not resolve an image-source problem.

App descriptions provide an introduction to each product, and some use templates. Operators, features, regional availability, and official URLs may change. Refer to the app's official information for current details.

## Frequently Asked Questions

**Why am I still seeing an older version?**

Make sure you opened the latest file. Your local HTML file and the GitHub Pages website are separate copies. Update the published file, wait for deployment to finish, and refresh your browser.

**Does “Visit Website” download the app directly?**

No. It opens the listed official website. Downloads, registration, and other actions are handled by that website.

**Why did copying a link fail?**

Your browser may restrict clipboard access. Allow clipboard access for the page and try again, or visit the website and copy its URL from the address bar.

**Can I use the directory entirely offline?**

The interface and bundled app data can be viewed offline. Remote icons and external websites require an internet connection.

## Maintenance and Verification

The current version has passed the webpage build, app-name and URL uniqueness checks, category-count checks, and selected functional checks covering description length, multilingual rendering, copy-feedback timing, and the scope of targeted icon changes.

These checks do not mean every external link and logo has been individually verified in a browser. When reporting an issue, include the app name, the observed problem, and the correct official website or icon source if available.

App names, logos, and trademarks belong to their respective owners. This project provides navigation and information and does not imply an official partnership with any listed app. Third-party images remain subject to their source terms; this document does not grant permission to use those assets.

---

You've reached the end～
