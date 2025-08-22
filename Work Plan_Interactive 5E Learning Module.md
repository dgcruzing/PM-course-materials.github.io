# Work Plan: Interactive 5E Learning Module

This document outlines the standard operating procedure for creating an interactive HTML learning module based on source content, structured with the 5E Instructional Model, and styled to a client's specifications.

## Phase 1: Discovery & Scoping

**Objective:** To fully understand the project requirements, source materials, and desired outcomes before beginning development.

1. **Initial Request Analysis:**
   * **Identify Core Goal:** Deconstruct the user's prompt to establish the primary objective. For this project, it was to convert a WHS case study into an interactive HTML page.
   * **Note Key Constraints & Requirements:**
     * **Instructional Model:** 5E (Engage, Explore, Explain, Elaborate, Evaluate).
     * **Target Audience:** 25-30 year olds.
     * **Persona:** Project Management Lecturer.
     * **Visual Goal:** "Make it pop," referencing a specific example.
     * **Dual Outputs:** Generate both an interactive HTML file and a Moodle Book Markdown version.
2. **Source Material Inventory:**
   * **Content Source:** Identify the primary document containing the core information to be taught (e.g., `whs-principles_labour-hire-case-study_jul2024.pdf`).
   * **Styling Reference:** Identify the document that will serve as the visual inspiration or style guide (e.g., `Client or Customer Requirements in Project Management.html`).
   * **User-Provided Assets:** Log any additional files or examples provided by the user for clarification (e.g., `image_6eaf44.png`).

## Phase 2: Content Strategy & Design

**Objective:** To structure the raw content into a compelling narrative that fits the instructional model and design a visually engaging layout.

1. **Content Mapping (5E Model):**
   * Read through the **Content Source** (the PDF) and break it down into logical sections.
   * Map each section to a phase of the 5E model:
     * **Engage:** Create an introduction that hooks the learner. Use the user-provided text to set the scene (the project kick-off, the key players).
     * **Explore:** Present the initial problem-solving steps from the case study (initial meetings, identifying gaps).
     * **Explain:** Detail the core concepts being taught (the Hierarchy of Controls).
     * **Elaborate:** Show the application of these concepts in a dynamic situation (managing the unforeseen weather event).
     * **Evaluate:** Conclude with the project wrap-up and lessons learned (the post-mortem).
2. **Persona & Tone Crafting:**
   * For each of the 5E sections, write a brief, introductory paragraph in the persona of a **Project Management Lecturer**.
   * Use direct, engaging language appropriate for the target audience.
3. **Visual Design & "Pop" Factor:**
   * Analyze the **Styling Reference** and user-provided images to create a design language.
   * **Layout:** Use a two-column layout with a sticky table of contents for easy navigation.
   * **Component Styling:** Design visually distinct components for different types of information:
     * Use colored callout boxes with icons for introductory text in each chapter.
     * Use simple, clean cards for defining key elements like "Case Scenario" and "Key Players."
     * For core concepts like the Hierarchy of Controls, create a grid of multi-colored containers with distinct icons and borders, directly referencing the user's visual feedback.
   * **Typography & Color:** Use a modern, readable font (like Inter) and a defined color palette (primary, secondary, accent colors) based on the reference material.
4. **Interactive Element Design:**
   * Design an activity that reinforces the key learning objectives. A drag-and-drop matching game is effective for definitions and principles.
   * Define the concepts and their corresponding descriptions for the activity.

## Phase 3: Development & Implementation

**Objective:** To build the fully functional and styled interactive HTML page.

1. **HTML Structure (Scaffolding):**
   * Build the main HTML structure including the header, the two-column layout for the TOC and main content, and the final activity section.
   * Use semantic HTML5 tags (`<header>`, `<main>`, `<aside>`, `<nav>`).
2. **Styling (Tailwind CSS):**
   * Link the Tailwind CSS CDN.
   * Implement the visual design from Phase 2, applying utility classes for layout, colors, fonts, shadows, and rounded corners.
   * Write any necessary custom CSS for animations (e.g., `fadeIn`, `shake`) or complex styles not covered by Tailwind utilities.
3. **JavaScript Interactivity:**
   * **Content Injection:** Write a script to dynamically generate the chapter/sub-chapter content and the table of contents from a JavaScript data object. This makes the content easy to manage and update.
   * **TOC Navigation:** Implement smooth scrolling and an "active" state indicator for the table of contents.
   * **Drag-and-Drop Activity:**
     * Write the logic to initialize the activity, shuffling the items.
     * Add event listeners (`dragstart`, `dragover`, `drop`, `dragend`).
     * Implement the validation logic to check for correct/incorrect matches.
     * Provide clear visual feedback (e.g., green for correct, red and a shake animation for incorrect).
     * Add a completion message and a reset button.

## Phase 4: Finalization & Output Generation

**Objective:** To deliver the final, polished artifacts as requested by the user.

1. **Code Review & Refinement:**
   * Review all HTML, CSS, and JavaScript for clarity, comments, and efficiency.
   * Ensure the page is fully responsive and looks good on mobile, tablet, and desktop screens.
   * Test all interactive elements thoroughly.
2. **Moodle Book Markdown Generation:**
   * Extract all the written content from the JavaScript data object.
   * Format the content using standard Markdown syntax for headings (`#`, `##`, `#####`), bold text (`**text**`), bullet points (`*`), and horizontal rules (`---`).
   * Replace YouTube links with the `[Related Videos]` placeholder as specified.
3. **Final Delivery:**
   * Present the complete, self-contained HTML code in a code immersive artifact.
   * Present the generated Markdown content in a separate text/markdown immersive artifact.
   * Provide a concluding message and invite further collaboration.

## Phase 5: Post-Launch Updates & Maintenance

**Objective:** To efficiently modify the existing interactive module based on user feedback or new requirements.

1. **Adding "Points to Ponder" to the Evaluate Section:**
   * **Locate the Target:** In the JavaScript data object (`chapters`), find the object with `id: 'c5'` (or the relevant evaluation section).
   * **Modify the Content:** Within the `content` backticks for this object, add a new `div` after each scenario's `<p>` tag.
   * **Structure:** Use a container `div` with a light background (e.g., `bg-gray-50`). Inside, add a `<p>` for the "Points to Ponder:" title and a `<ul>` with `<li>` elements for each point.

2. **Conditionally Removing Video Links:**
   * **Locate the Target:** Find the `createContentCard` JavaScript function.
   * **Implement Conditional Logic:** Wrap the video link `<a>` tag generation in a variable. Before generating the final `innerHTML`, check the `id` of the content card being created.
   * **Logic:** If the `id` matches the section to be excluded (e.g., `if (id === 'c5')`), set the video link variable to an empty string.

3. **Adding a Video Iframe:**
   * **Locate the Target:** In the main HTML body, find the closing tag of the main content container and the opening tag of the next major section (e.g., the Drag and Drop Activity).
   * **Insert the Iframe Section:** Add a new `<div>` container between these two sections.
   * **Structure for Responsiveness:** To ensure a 16:9 aspect ratio, use a parent `div` with `position: relative` and `padding-top: 56.25%`. The `iframe` itself should have `position: absolute`, `top: 0`, `left: 0`, `width: 100%`, and `height: 100%`.
   * **Placeholder:** Use a placeholder URL in the `src` attribute for easy replacement.

