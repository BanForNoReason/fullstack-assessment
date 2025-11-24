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

1. **Search bar issue when searching for an 'apple' product (Bug)** (completed)

   - I get an error having to due with '`next/image` Un-configured Host' whenever I am searching for a product in the search bar. Reading the docs it seems the inital root cause you need to check for has to do with "One of your pages that leverages the next/image component, passed a src value that uses a hostname in the URL that isn't defined in the images.remotePatterns in next.config.js".
   - **Fix implementation**
     - The root issue was the images path url was trying to access an image from the "images-na.ssl-images-amazon.com" hostname, however this dependecy was not added into the next.onfig.ts file. After adding another hostname entry into the remotePatterns area, this problem was resolved.
   - **Reason for Approach**
     - I hadn't seen and issue or bug like this before. I took a few minutes to read the documentation that Next.js provided to help better assess the issue or potential cause of the issue. After figuring out what the problem was, it was quite simple to implement the fix, by adding another hostname entry in remotePatterns.

2. **Clear Filter (Bug)** (completed)
   - Whenever you press the clear filter button, it clear subcategories properly, however it will not clear out the initial category you have selected. There is also a dev console error having to do with 'select changing from uncontrolled to controlled'.
   - **Fix implementation**
     - I noticed that in the dev console whenever the filter was being used and reset there was a warning thrown syaing "select changing from uncontrolled to controlled". This gave me the idea that there was some sort of state management issue. Looking into the code the first render had the value set as -> undefined, which is treated as uncontrolled. After I set it to a string, it is always degined and it is always defined (no unctronolled or controlled flip warning). Now this pivents into the next error section where the no category was also given the option to sometimes be undefined, the "Clear Filters" didn't reset the main Select properly, so you couldn't re-select the same category either. Changing the default string value to be a blank string "", ensure that the select feature -> category/subcategory are not set to a blank "" string, treated as a flase value so no filter is applied.
     - **Reason for Approach**
     - It isn't a good habit to leave something in an undefined state, as it can allow bugs like this to slip through. I made the select component remain controlled for its entire lifecycle (by using "" as the explicit “no selection” value), which results in a consistent “no filter” state that can be reliably reset.

### Priority Group 2

3. **UX Design Flaw (UX Flaw)** (not-complete)
   - The View Details Buttons aren't the same across different products. if a product has more product tags for the filter, then it will cause the button to be lower, or higher if less tags.
4. **Duplicate Same items (Potential UX Flaw)** (not completed)

   - There are a few items that have the exact same information, and exact same Titles etc... need to check out what is causing these issues to appear first.

5. **Change the StackShop Title to be a Homepage link**
   - For the user it is inconvient to have to clear out the search to go to the "main" page, so thinking of making the main header in the nav to be a responsive '/...' home link (not sure yet).
