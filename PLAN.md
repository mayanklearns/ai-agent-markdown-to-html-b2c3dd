# Project Plan: Markdown to HTML Converter - Round 2 Update

## Update Overview
**Mission**: Add tab functionality to switch between rendered HTML view and original Markdown source view while keeping content synchronized.

## 1. Architectural Vision - Changes Required

### Modified File Structure
```
markdown-to-html-b2c3dd/
├── index.html          # MODIFY: Add tab buttons and markdown-source element
├── script.js           # MODIFY: Add tab switching logic and populate markdown-source
├── styles.css          # MODIFY: Add styling for tabs and source view
├── input.md            # UNCHANGED: Source markdown file
├── README.md           # UPDATE: Document new tab feature
├── preview.png         # REGENERATE: New screenshot with tabs
├── LICENSE             # UNCHANGED
└── PLAN.md             # REPLACE: This updated plan
```

### New Components to Add
1. **#markdown-tabs**: Container with tab switching buttons
2. **#markdown-source**: Pre-formatted text container for raw markdown
3. **Tab Switching Logic**: JavaScript to toggle visibility between views
4. **Tab Styling**: Active/inactive states for buttons

## 2. Component Strategy - HTML Changes

### Updated HTML Structure (`index.html`)
```html
<main>
  <!-- NEW: Tab Navigation -->
  <div id="markdown-tabs" class="mb-3">
    <button id="tab-preview" class="tab-button active">Preview</button>
    <button id="tab-source" class="tab-button">Markdown Source</button>
  </div>
  
  <!-- EXISTING: Rendered HTML Output (initially visible) -->
  <div id="markdown-output" class="markdown-content tab-content active">
    <div class="text-center">
      <div class="spinner-border text-primary" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
      <p class="mt-3">Loading markdown content...</p>
    </div>
  </div>
  
  <!-- NEW: Markdown Source View (initially hidden) -->
  <div id="markdown-source" class="markdown-source tab-content">
    <!-- Will be populated with raw markdown text -->
  </div>
</main>
```

**Key Design Decisions:**
- Use semantic button elements for accessibility
- Add `active` class to track current tab state
- Both content containers have `tab-content` class for unified styling
- Initially show Preview tab as default view

## 3. Styling Strategy - CSS Changes

### New CSS Rules (`styles.css`)
```css
/* Tab Navigation Styles */
#markdown-tabs {
  display: flex;
  gap: 0.5rem;
  border-bottom: 2px solid #e9ecef;
  padding-bottom: 0;
}

.tab-button {
  background: transparent;
  border: none;
  border-bottom: 3px solid transparent;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  font-weight: 500;
  color: #6c757d;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tab-button:hover {
  color: #495057;
  background-color: #f8f9fa;
}

.tab-button.active {
  color: #0d6efd;
  border-bottom-color: #0d6efd;
}

/* Tab Content Visibility */
.tab-content {
  display: none;
}

.tab-content.active {
  display: block;
}

/* Markdown Source Styles */
#markdown-source {
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  padding: 1.5rem;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.9rem;
  line-height: 1.6;
  white-space: pre-wrap;
  word-wrap: break-word;
  overflow-x: auto;
  min-height: 200px;
  color: #212529;
}
```

**Styling Approach:**
- Use flexbox for tab button layout
- Active tab indicated by bottom border (following common UI pattern)
- Smooth transitions for better UX
- Source view uses monospace font for code-like appearance
- Preserve whitespace and line breaks in source view with `white-space: pre-wrap`

## 4. Logic & Interactivity - JavaScript Changes

### Updated JavaScript Flow (`script.js`)
```javascript
document.addEventListener('DOMContentLoaded', async () => {
  // EXISTING: Configure marked.js (keep as-is)
  
  const outputElement = document.getElementById('markdown-output');
  const sourceElement = document.getElementById('markdown-source');
  
  try {
    // EXISTING: Fetch markdown file
    const response = await fetch('input.md');
    if (!response.ok) throw new Error(...);
    const markdownText = await response.text();
    
    // EXISTING: Convert and render HTML
    const htmlContent = marked.parse(markdownText);
    const fullHtmlContent = '<h1>Project Overview</h1>' + htmlContent;
    outputElement.innerHTML = fullHtmlContent;
    
    // NEW: Populate markdown source view
    sourceElement.textContent = markdownText;
    
    // EXISTING: Apply syntax highlighting
    document.querySelectorAll('pre code').forEach(...);
    
  } catch (error) {
    // EXISTING: Error handling
  }
  
  // NEW: Tab Switching Logic
  setupTabSwitching();
});

function setupTabSwitching() {
  const tabPreview = document.getElementById('tab-preview');
  const tabSource = document.getElementById('tab-source');
  const outputElement = document.getElementById('markdown-output');
  const sourceElement = document.getElementById('markdown-source');
  
  tabPreview.addEventListener('click', () => {
    // Switch to preview tab
    tabPreview.classList.add('active');
    tabSource.classList.remove('active');
    outputElement.classList.add('active');
    sourceElement.classList.remove('active');
  });
  
  tabSource.addEventListener('click', () => {
    // Switch to source tab
    tabSource.classList.add('active');
    tabPreview.classList.remove('active');
    sourceElement.classList.add('active');
    outputElement.classList.remove('active');
  });
}
```

**Key Logic Points:**
1. Fetch markdown text once and populate both views
2. Use `textContent` (not `innerHTML`) for source to preserve formatting
3. Tab switching toggles `active` class on both buttons and content containers
4. Content stays synchronized because both views use the same fetched data
5. Separate function for tab logic to maintain clean code organization

### Edge Cases Handled
- ✅ Content loaded once and shared between views (no re-fetching)
- ✅ Tab state managed through CSS classes
- ✅ Error handling applies to both views
- ✅ Source view preserves exact markdown formatting including whitespace
- ✅ Clicking active tab doesn't break state

## 5. Evaluation Criteria Compliance - Round 2

### ✅ NEW Criterion 1: At least two buttons inside #markdown-tabs
- **Implementation**: 
  - Create `<div id="markdown-tabs">` container
  - Add two `<button>` elements inside:
    - `<button id="tab-preview">Preview</button>`
    - `<button id="tab-source">Markdown Source</button>`
- **Verification**: HTML structure contains exactly this
- **Location**: `index.html` line ~30

### ✅ NEW Criterion 2: #markdown-source contains non-empty Markdown text
- **Implementation**:
  - Create `<div id="markdown-source">` element in HTML
  - In JavaScript, after fetching `input.md`, set: `sourceElement.textContent = markdownText`
  - This populates it with the raw markdown content from input.md (756 bytes of text)
- **Verification**: After page load, inspect element content
- **Location**: 
  - HTML: `index.html` line ~40
  - JS: `script.js` line ~55

### ✅ EXISTING: Maintain all Round 1 functionality
- ✅ marked.js library loaded and used
- ✅ highlight.js library loaded and used
- ✅ #markdown-output contains rendered HTML with heading tags
- All existing functionality preserved and working

## 6. Documentation Updates

### README.md Changes
- Add new feature to Features section: "📑 **Tab Switching**: Toggle between rendered HTML and raw Markdown source"
- Update "How It Works" section to mention tab functionality
- Update screenshot reference (preview.png will show tabs)
- Ensure Deployment URLs are correct:
  - Repository: `https://github.com/mayanklearns/ai-agent-markdown-to-html-b2c3dd`
  - Live Demo: `https://mayanklearns.github.io/ai-agent-markdown-to-html-b2c3dd/`

## 7. Implementation Order

1. ✅ Review existing codebase (COMPLETE)
2. ✅ Create updated PLAN.md (this file)
3. Update `index.html` with tab structure
4. Update `styles.css` with tab styling
5. Update `script.js` with tab logic
6. Analyze code quality for all modified files
7. Run and preview application
8. Verify both tabs work correctly
9. Update README.md with new feature
10. Final quality check and preview
11. Final review against all criteria

## 8. Risk Mitigation - Round 2 Specific

- **Risk**: Tab switching breaks existing HTML rendering
  - **Mitigation**: Keep existing rendering logic intact; only add new elements and toggle visibility
  
- **Risk**: #markdown-source empty on load
  - **Mitigation**: Populate immediately after fetch, same as #markdown-output
  
- **Risk**: Content not synchronized between tabs
  - **Mitigation**: Both views populated from same fetched markdown text
  
- **Risk**: Active tab state confusion
  - **Mitigation**: Clear class management; only one active tab at a time

## 9. Success Metrics - Round 2

- ✅ All 2 NEW evaluation criteria met
- ✅ All existing functionality preserved (no regressions)
- ✅ Tab switching works smoothly
- ✅ No console errors in browser
- ✅ Clean, valid HTML5 markup
- ✅ Professional tab UI following best practices
- ✅ Code quality analysis passes with no errors
- ✅ README.md updated with new features
- ✅ Preview screenshot shows new tab interface

---

**Plan Status**: Ready for Round 2 execution
**Next Phase**: ACT - Implement tab functionality while preserving existing features
