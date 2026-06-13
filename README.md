# Mobile-Optimized Unabridged Book Reader

A premium, lightweight, standalone web application designed specifically for reading books in a structured, mobile-optimized format. Designed with high-fidelity aesthetics and layout constraints optimized for modern displays like the **OnePlus 8T** (20:9 ratio, high-refresh rates, AMOLED display).

## 🚀 Features

*   **Progressive Disclosure Hierarchy**: View hierarchical summaries of the book, chapters, topics, and stories, and expand them into full, unabridged text seamlessly.
*   **AMOLED True Black & Eye-Care Themes**: Supports true black AMOLED mode (`#000000`) for low battery consumption and zero glow, Warm Sepia mode for natural light, and Soft Dim mode.
*   **Offline-Capable & Standalone**: Pure single-file HTML/JS/CSS that functions anywhere, loading files locally.
*   **Progress Auto-Save**: Keeps track of completed sections, last read locations, and precise scroll positions in `localStorage`.
*   **One-Handed Navigation**: All critical reader actions (prev, next, font adjustment, theme toggles) are placed in a bottom-floating glassmorphic bar within thumbs' reach.
*   **Interactive Drag-and-Drop Loader**: Simply drag and drop your parsed book JSON to load any book instantly.

## 📂 Project Structure

```text
├── index.html          # Core single-page web app container
├── sample_book.json    # Demonstration book structured in our schema
└── README.md           # This project guide and technical summary
```

## 📖 Getting Started

1.  Open the [index.html](file:///storage/emulated/0/Documents/Antigravity/Book%20Reader/index.html) file in any mobile or desktop web browser.
2.  If opened via a web server (e.g. `npx serve` or Live Server), it will automatically load `sample_book.json`.
3.  If opened directly via the file protocol (`file://`), click **Browse File** or drag and drop [sample_book.json](file:///storage/emulated/0/Documents/Antigravity/Book%20Reader/sample_book.json) into the uploader area to load the preview book.
4.  Switch themes, scale font size, toggle chapters, and expand the stories to read the unabridged text.

## 📊 Book Schema Format
The app reads a generic JSON structure that separates chapters, summaries, and stories to keep the reader program independent of any specific book text:

```json
{
  "bookId": "unique-identifier",
  "title": "Title of the Book",
  "author": "Author Name",
  "summary": {
    "coreThesis": "The core message...",
    "keyTakeaways": ["Takeaway 1", "Takeaway 2"]
  },
  "chapters": [
    {
      "chapterId": "ch-1",
      "chapterNumber": 1,
      "title": "Chapter One Title",
      "summary": {
        "coreIdea": "Key chapter summary idea...",
        "keyInsights": ["Insight 1", "Insight 2"]
      },
      "sections": [
        {
          "sectionId": "sec-1-1",
          "title": "Section Title",
          "type": "story",
          "summary": "Short section synopsis...",
          "unabridgedText": "Unabridged text contents..."
        }
      ]
    }
  ]
}
```

## 🛠️ Data Processing Workflow
When parsing large books:
1.  We extract page boundary information of the PDF chapters.
2.  We process the text chapter-by-chapter using LLM subagents to generate summaries and identify stories/topics while preserving 100% of the unabridged text.
3.  We compile the JSON chunks into a single book dataset and load it into this application container.
