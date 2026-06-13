## ⚠️ Project No Longer Maintained

This project is an activity of the past and is no longer being actively developed or maintained. 

# Mobile-Optimized Unabridged Book Reader

A premium, lightweight, standalone web application designed specifically for reading books in a structured, mobile-optimized format. Designed with high-fidelity aesthetics and layout constraints optimized for modern displays like the **OnePlus 8T** (20:9 ratio, high-refresh rates, AMOLED display).

## 🚀 Features

*   **Progressive Disclosure Hierarchy**: View hierarchical summaries of the book, chapters, topics, and stories, and expand them into full, unabridged text seamlessly.
*   **AMOLED True Black & Eye-Care Themes**: Supports true black AMOLED mode (`#000000`) for low battery consumption and zero glow, Warm Sepia mode for natural light, and Soft Dim mode.
*   **Offline-Capable & Standalone**: Pure single-file HTML/JS/CSS that functions anywhere, loading files locally.
*   **Progress Auto-Save & JSON Export**: Auto-saves completed sections, last read positions, and scroll state in `localStorage`. Allows exporting the updated book JSON file containing embedded reading progress for cross-device syncing and backup.
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
  "progress": {
    "completedSections": {
      "sec-1-1": true
    },
    "lastReadSectionId": "sec-1-1",
    "lastReadChapterId": "ch-1",
    "scrollPositions": {
      "sec-1-1": 0.35
    }
  },
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

## 🤖 AI LLM Generator Prompt
Copy and paste this prompt into your LLM agent (along with the book PDF) to generate the required JSON structure recursively:

```markdown
You are an expert AI Data Processing Agent specializing in high-fidelity book parsing, summarization, and JSON conversion. 

Your task is to parse a PDF book and structure it into a specific hierarchical JSON format suited for a mobile reader.

### 📋 JSON SCHEMA REFERENCE:
The target format is:
{
  "bookId": "kebab-case-unique-id",
  "title": "Book Title",
  "author": "Author Name",
  "summary": {
    "coreThesis": "One-page comprehensive book summary (exactly 400-500 words).",
    "keyTakeaways": [
      "Takeaway 1",
      "Takeaway 2"
    ]
  },
  "chapters": [
    {
      "chapterId": "ch-1",
      "chapterNumber": 1,
      "title": "Chapter Title",
      "summary": {
        "coreIdea": "Chapter main idea summary",
        "keyInsights": ["Insight 1", "Insight 2"]
      },
      "sections": [
        {
          "sectionId": "sec-1-1",
          "title": "Section/Topic/Story Title",
          "type": "story" | "topic",
          "summary": "Rich summary (~10% of the unabridged text length)",
          "unabridgedText": "Full unabridged text content...",
          "sections": [] // Optional recursive nested subsections
        }
      ]
    }
  ]
}

---

### 🛠️ WORKFLOW RULES & GUIDELINES:

1. **Interactive Scope Selection (Before Parsing)**
   - Before writing any JSON, ask me:
     * "Which chapters or page ranges do you want to process in this turn?"
     * Wait for my response before proceeding.

2. **Incremental Appending & Resuming**
   - We will build this book JSON incrementally. 
   - If this is a **new session**, initialize the root metadata (`bookId`, `title`, `author`, `summary`).
   - If this is a **resumed session**:
     * I will provide the existing JSON and point you to the last processed section/chapter.
     * Identify where we left off, ask for confirmation, and append the new chapters/sections seamlessly into the schema array without losing existing nodes.

3. **Rich Summarization Constraints**
   - **Section Summaries (Story/Topic mode)**: Each summary must be a rich, descriptive overview that is approximately **10%** of the word count of its corresponding `unabridgedText`. Avoid shallow one-sentence descriptions.
   - **Book Core Thesis Summary**: This root-level summary must be exactly **one page** (~400-500 words), covering the complete scope of the book comprehensively.

4. **100% Unabridged Text Preservation**
   - Never truncate, edit, or paraphrase the text going into the `unabridgedText` fields. Keep it exact, including paragraphs, dialogues, and punctuation.

---

### 🚀 LET'S START:

To begin, please ask me:
1. The title/author of the book (or upload the text file/PDF).
2. Which chapters or page range we should process for this first run.
3. Whether we are starting fresh or resuming a previous JSON build.
```
