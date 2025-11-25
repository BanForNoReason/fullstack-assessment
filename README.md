# Jonah Chegarnov - Stackline Full Stack Assignment

## Overview

This is a sample eCommerce website that includes:

- Product List Page
- Search Results Page
- Product Detail Page

## Tasks Give and Submission Details

1. **Identify and fix bugs** - Review the application thoroughly and fix any issues you find
2. **Document your work** - Create a comprehensive README that includes:
   - What bugs/issues you identified
   - How you fixed each issue
   - Why you chose your approach
   - Any improvements or enhancements you made
3. **Submission Steps**

- Fork this repository
- Make your fixes and improvements
- **Replace this README** with your own that clearly documents all changes and your reasoning
- Provide your Stackline contact with a link to a git repository where you have committed your changes

## Identified bugs

### Priority approach

- I will prioritize critical or high-impact issues first, especially those that directly break core functionality.
- My reasoning is that if this were a live, user-facing product, bugs that prevent features from working are more urgent than UX/UI issues. While design problems are still important, they are more tolerable in the short term while blocking or functionality-breaking bugs are being resolved.

### Priority Group 1

1. **Search bar issue when searching for an "apple" product (Bug)** (completed)

   - I get an error related to `next/image` saying the host is not configured whenever I search for a product in the search bar. From the docs, the root cause is that a page using the `next/image` component is passing a `src` URL whose hostname isn’t defined in `images.remotePatterns` in `next.config.ts`.
   - **Fix implementation**
     - The image URL was trying to load from the `images-na.ssl-images-amazon.com` hostname, but this host wasn’t listed in `next.config.ts`. I added a second `remotePatterns` entry for that hostname, and the error went away.
   - **Reason for Approach**
     - I hadn’t run into this specific bug before, so I took a few minutes to read the Next.js docs to understand what the error was actually saying. Once I understood that it was just a missing allowed host, the fix was straightforward: explicitly whitelist the additional image domain in `remotePatterns`.

2. **Clear Filters behavior + Select warning (Bug)** (completed)
   - When you press the Clear Filters button, it clears the subcategory correctly, but it does not clear out the main category you originally selected. At the same time, the dev console shows a warning about the select “changing from uncontrolled to controlled.”
   - **Fix implementation**
     - I noticed that every time the filter was used and reset, the console warning about “changing from uncontrolled to controlled” appeared, which pointed to a state management issue. On the first render the Select value was `undefined` (uncontrolled), and later it became a string (controlled), causing the warning and also leaving the main category stuck so you couldn’t re-select the same option. I changed the default state for both category and subcategory to use an empty string (`""`) instead of `undefined`, and I made Clear Filters reset both values back to `""`. In the effects, `""` is treated as a falsy “no filter” state, so no category is applied when filters are cleared.
   - **Reason for Approach**
     - It isn’t a good habit to leave something in an undefined state, because bugs like this can slip through and only show up in specific flows. By keeping the Select components fully controlled for their entire lifecycle (using `""` as the explicit “no selection” value), I get a consistent “no filter” state that can be reliably reset, and the warning goes away.

### Priority Group 2

3. **UX Layout Flaw (completed)**
   - The “View Details” buttons weren’t aligned across products. If a product had more tags, its card got taller and the button sat lower than cards with fewer tags.
   - **Fix implementation**
     - I updated the product grid to stretch all items to the same height and made each `Card` fill its grid cell (`items-stretch`, `h-full` on the link and card).
     - For the `CardFooter`, I added the `mt-auto` class so, since the card is already `flex flex-col`, `margin-top: auto` pushes the footer down to the bottom of the card, away from the content above.
   - **Reason for Approach**
     - This keeps the cards visually consistent and guarantees that every “View Details” button sits at the bottom of its card, regardless of how many tags or how much content each product has. It’s a small change, but it makes the grid look cleaner and easier to scan for users.
4. **Duplicate Same items (Potential UX Flaw)** (not completed)

   - There are a few items that have the exact same information, and exact same Titles etc... need to check out what is causing these issues to appear first.

5. **Change the StackShop Title to be a Homepage link**
   - For the user it is inconvient to have to clear out the search to go to the "main" page, so thinking of making the main header in the nav to be a responsive '/...' home link (not sure yet).
