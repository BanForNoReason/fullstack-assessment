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

1. **Search bar error when looking up an "apple" product (Bug)** (completed)

   - When I tried searching for certain products (like anything “apple” related), I was getting a `next/image` error saying the host wasn’t configured. After checking the docs, I realized the page using `next/image` was pulling an image from a URL whose hostname wasn’t included in `images.remotePatterns` in `next.config.ts`.
   - **Fix implementation**

     - The image URL was coming from `images-na.ssl-images-amazon.com`, but that hostname wasn’t in `next.config.ts` at all. I added a second `remotePatterns` entry for that domain, saved it, and the error disappeared.

   - **Reason for approach**

     - I hadn’t hit this exact error before, so I paused and read through the Next.js docs instead of just randomly changing things. Once it clicked that the problem was simply an unlisted host, the fix was basically just “tell Next.js this domain is allowed.” Adding the extra domain into `remotePatterns` keeps it explicit and easy to update later if more image hosts show up.

2. **Clear Filters behavior + Select warning (Bug)** (completed)

   - When I clicked Clear Filters, it reset the subcategory, but the main category stayed selected, which felt off. On top of that, the console was yelling about the select “changing from uncontrolled to controlled,” which pointed to a state issue.
   - **Fix implementation**

     - I noticed the Select value started out as `undefined` on the first render (uncontrolled) and then later became a string (controlled). That’s what triggered the warning and also made it impossible to re-select the same category, because the state wasn’t behaving consistently. I changed the default state for both `category` and `subCategory` to `""` instead of `undefined`, and I wired Clear Filters to set both back to `""`. In the logic, `""` is treated as “no filter,” so when filters are cleared, nothing is applied.

   - **Reason for approach**

     - Leaving things as `undefined` is usually asking for subtle bugs like this, especially with controlled components. By keeping the Select fully controlled from the beginning and using `""` as the explicit “no selection” state, I get predictable behavior: Clear Filters actually clears everything, the user can re-select options, and the React warning disappears.

---

### Priority Group 2

3. **Product card layout / button alignment (UX issue)** (completed)

   - The “View Details” buttons weren’t lining up across all product cards. If a product had more tags, its card got taller and the button ended up lower than the others, which made the grid look a bit messy.
   - **Fix implementation**

     - I updated the product grid so that all items stretch to the same height and each `Card` fills its grid cell (`items-stretch` plus `h-full` on the link and card).
     - Since the card is already `flex flex-col`, I added `mt-auto` on the `CardFooter`. That pushes the footer to the bottom of the card, no matter how much content is above it.

   - **Reason for approach**

     - This keeps the cards visually consistent and makes the grid easier to scan. No matter how many tags or lines of text a product has, the “View Details” button should always sit at the bottom of the card, which just feels cleaner and more intentional from a UX perspective.

4. **Largest Contentful Paint image warning (Performance)** (completed)

   - Next.js was warning that one of the product images (from `m.media-amazon.com`) was being flagged as the Largest Contentful Paint (LCP) element and suggested using the `priority` prop if that image is above the fold. In this layout, the first product card’s image is basically the main above-the-fold visual on the page.
   - **Fix implementation**

     - Inside the `products.map(...)`, I used the `index` from the `.map` and passed `priority={index === 0}` to the `Image` component. That way only the first product image—the one you actually see immediately—is marked as high priority, and the rest of the images load normally.

   - **Reason for approach**

     - I didn’t want to mark every product image as `priority`, because that would preload a bunch of images and could backfire on performance. Targeting just the first, above-the-fold image follows Next.js’ recommendation, improves LCP where it actually matters, and keeps the rest of the grid lightweight.

---

### Potential Features / Suggestions

**Make the “StackShop” title link back to the homepage (Suggestion)**

- Right now, the title doesn’t take you anywhere, which might be intentional, so I didn’t change it. That said, most users naturally expect the main logo/title (“StackShop”) to act as a quick way back to the homepage. Making it clickable and routing it back home would line up with common UX patterns and make navigation feel more intuitive when you’re jumping between different pages.
