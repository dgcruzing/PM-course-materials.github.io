

-----

description: Creating interactive 5E instructional modules with HTML, Tailwind CSS, and vanilla JavaScript.
globs:

  - "\*\*/\*.html"
    alwaysApply: true

-----

# Interactive HTML Learning Module Guide

## 1\. Module Architecture & Philosophy

All interactive modules must be built as a **single, self-contained `.html` file**. This ensures maximum portability and ease of use in various learning environments like Moodle or a standard web server.

  - **Tech Stack**: The required stack is **HTML**, **Tailwind CSS** (via CDN), and **vanilla JavaScript**. No external frameworks like React, Vue, or jQuery are permitted.
  - **Data-Driven Content**: The module's text content, structure, and activity data **must** be defined in JavaScript arrays/objects at the top of the main script block. The HTML body should be a minimal shell, with content dynamically injected by the script. This is the most critical rule.
  - **Instructional Model**: Content **must** be structured around the **5E Instructional Model** (Engage, Explore, Explain, Elaborate, Evaluate).
  - **Responsive & Accessible**: Modules must be fully responsive, using mobile-first principles with Tailwind's breakpoint utilities.

## 2\. HTML File Structure

The HTML document must follow this specific structure to ensure consistency and proper script execution.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Module Title</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        /* Required global styles and keyframe animations */
        body { font-family: 'Inter', sans-serif; /* ... */ }
        .toc-link.active { /* ... */ }
        @keyframes fadeIn { /* ... */ }
        @keyframes shake { /* ... */ }
    </style>
    <script>
        // Required Tailwind theme extensions
        tailwind.config = { /* ... */ }
    </script>
</head>
<body class="antialiased text-slate-700">

    <div class="container mx-auto p-4 md:p-8">
        <header> </header>

        <div class="flex flex-col lg:flex-row gap-8">
            <aside class="lg:w-1/3 xl:w-1/4 lg:sticky top-8 self-start">
                <nav id="table-of-contents-container">
                    </nav>
            </aside>

            <main class="lg:w-2/3 xl:w-3/4">
                <div id="content-container" class="space-y-6">
                    </div>
            </main>
        </div>
        
        <div id="activity-section">
             </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // All module logic, data, and rendering functions go here.
        });
    </script>
</body>
</html>
```

## 3\. JavaScript Engine Conventions

The core of the module is a single vanilla JavaScript script block at the end of the `<body>`.

### 3.1. Data-Driven Content Pattern

All chapter and activity content **must** be defined in two distinct data structures: `chapters` and `activityData`.

```javascript
// ✅ Correct: Content defined as a JavaScript array of objects
const chapters = [
    { 
        id: 'c1', // Unique ID for anchoring
        title: '1. Engage: The Project Manager\'s Dilemma', 
        content: `<p>HTML content for this section...</p>`
    },
    {
        id: 'c3',
        title: '3. Explain: Core Concepts',
        content: `<p>Introductory content...</p>`,
        subChapters: [ // Nested array for sub-sections
            {
                id: 'c3-1',
                title: '3.1. Sub-Chapter Title',
                content: `<p>Detailed content...</p>`
            }
        ]
    }
];

const activityData = [
    { id: 'item-1', concept: 'Concept A', description: 'This is the description for Concept A.' },
    { id: 'item-2', concept: 'Concept B', description: 'This is the description for Concept B.' }
];
```

### 3.2. Dynamic Rendering Pattern

Use dedicated functions to generate DOM elements from the data objects. This keeps the logic clean and separated from the data.

```javascript
// ✅ Correct: Use functions to render content and TOC
function renderContent() {
    // Loop through the 'chapters' array
    // Call createContentCard() and createTocLink() for each item
}

function createContentCard(id, title, content) {
    const card = document.createElement('div');
    card.id = id;
    card.className = 'content-card bg-white rounded-xl shadow-md p-6 md:p-8';
    // Dynamically generate a YouTube search link from the title
    const youtubeLink = `<a href="https://www.youtube.com/results?search_query=${encodeURIComponent(title)}">...</a>`;
    card.innerHTML = `<h2>${title}</h2><div>${content}</div>${youtubeLink}`;
    return card;
}

// Always wrap the main execution in this event listener
document.addEventListener('DOMContentLoaded', function () {
    renderContent();
    initializeActivity();
    // ... other initializations
});
```

## 4\. Styling & Component Patterns (Tailwind CSS)

Styling must adhere to a consistent visual language defined by Tailwind utility classes.

### 4.1. Color System & Lecturer Persona

Each of the 5E sections must begin with a themed "callout box" to deliver the "Project Management Lecturer" persona.

  - **Pattern**: `bg-color-50`, `border-l-4 border-color-500`, `text-color-800`.
  - **Standard Colors**:
      - Engage: **Indigo**
      - Explore: **Sky**
      - Explain: **Emerald**
      - Elaborate: **Purple**
      - Evaluate: **Slate**

<!-- end list -->

```html
<div class="bg-indigo-50 border-l-4 border-indigo-500 text-indigo-800 p-4 rounded-r-lg mb-6 shadow-sm">
    <p class="font-semibold">Alright team, let's kick off with a scenario.</p>
    <p class="mt-1">This is the engaging introduction for the lesson...</p>
</div>
```

### 4.2. Drag & Drop Activity Styling

The activity requires specific styling for user feedback.

  - **Drop Zones (Initial State)**: `bg-slate-100`, `border-dashed`, `border-slate-300`.
  - **Drop Zones (Hover State)**: `bg-indigo-100`, `border-primary`.
  - **Correct Match**: The drop zone turns **emerald** (`bg-emerald-50`, `border-secondary`) and holds the matched item. The original draggable item becomes faded (`opacity-50`).
  - **Incorrect Match**: The drop zone **must** use the `shake-animation` and have a temporary **red** border.

## 5\. Interactivity Patterns

Functionality for the TOC and the activity must be implemented consistently.

### 5.1. Table of Contents (TOC)

  - The TOC must be in a sticky `aside` element.
  - Links must use `e.preventDefault()` and `element.scrollIntoView({ behavior: 'smooth' })` for navigation.
  - The clicked link must receive an `.active` class for visual highlighting.

### 5.2. Drag & Drop Activity Logic

The activity script must perform these actions in order:

1.  **Initialize**: On page load and on reset, dynamically create and shuffle both the "concepts" and "descriptions" from the `activityData` array.
2.  **Handle Drag & Drop**: Use `dragstart`, `dragover`, `drop`, and `dragend` event listeners.
3.  **Validate**: On drop, compare the `dataset.id` of the dragged item and the drop zone.
4.  **Provide Feedback**: Apply the correct styling for a match or the incorrect styling for a mismatch.
5.  **Track State**: Maintain a counter for `correctMatches`.
6.  **Complete**: When `correctMatches` equals the total number of items, display a congratulatory message.
7.  **Reset**: A "Reset" button must call the initialization function to restart the activity.

## 6\. Common Anti-Patterns to Avoid

  - ❌ **Hardcoding Content**: Do not write chapter or activity text directly into the main HTML body. Always use the JS data-driven pattern.
  - ❌ **Using External JS Libraries**: Do not import jQuery, Alpine.js, or any other JS library. Use vanilla JavaScript only.
  - ❌ **Inconsistent Styling**: Do not invent new color schemes or component styles. Adhere strictly to the defined patterns (callout boxes, cards, activity feedback).
  - ❌ **Missing Dynamic YouTube Links**: Every content card (except the final 'Evaluate' section) must have a dynamically generated "Find Related Videos" link.