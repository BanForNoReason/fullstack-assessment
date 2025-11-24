# Stackline Full Stack Assignment

# Jonah Chegarnov

## Overview

This is a sample eCommerce website that includes:

- Product List Page
- Search Results Page
- Product Detail Page

## Your Task & Submission

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

1. **Search bar issue when searching for a product**

   - I get an error having to due with '`next/image` Un-configured Host' whenever I am searching for a product in the search bar. Reading the docs it seems the inital root cause you need to check for has to do with "One of your pages that leverages the next/image component, passed a src value that uses a hostname in the URL that isn't defined in the images.remotePatterns in next.config.js".
   - **Fix implemented**
     - The root issue was the images path url was trying to access an image from the "images-na.ssl-images-amazon.com" hostname, however this dependecy was not added into the next.onfig.ts file. After adding another hostname entry into the remotePatterns area, this problem was resolved.
   - **Reason for approach**
     - I hadn't seen and issue or bug like this before. I took a few minutes to read the documentation that Next.js provided to help better assess the issue or potential cause of the issue. After figuring out what the problem was, it was quite simple to implement the fix, by adding another hostname entry in remotePatterns.

2. **Clear Filter Bug**
   - ## Whenever you press the clear filter button, it clear subcategories properly, however it will not clear out the initial category you have selected.

### Priority Group 2

3. **UX Design Flaw**
   - The View Details Buttons aren't the same across different products. if a product has more product tags for the filter, then it will cause the button to be lower, or higher if less tags.
4. **Duplicate Same items**
   - There are a few items that have the exact same information, and exact same Titles etc... need to check out what is causing these issues to appear first.
