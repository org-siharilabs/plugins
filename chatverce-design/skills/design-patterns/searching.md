
# Searching

## What the User Is Doing

The user knows something exists in the product — a conversation, a contact, a message — and needs to find it quickly. They are not browsing; they have intent. The search experience must respect that intent by returning relevant results with minimal friction. A search that fails or is slow erodes trust in the entire product.

## Emotional Intention

**"I can find anything fast."** Users should feel that Chatverce is their exoskeleton — it extends their memory across thousands of conversations and contacts. Search should feel instant and exhaustive. If something exists in the product, search will find it.

## How It Works

**Input → Debounce → Results → Empty state**

1. **Input:** User types in the search field. Do not require pressing Enter to search — search-as-you-type is the standard.
2. **Debounce:** 300ms debounce before triggering the search request. This prevents excessive API calls on every keystroke.
3. **Loading state:** While results are fetching, show skeleton rows in the result area. Do not clear existing results until new results arrive (prevents layout jump).
4. **Results:** Display grouped by type (Conversations, Contacts, Messages) with a channel indicator badge per result.
5. **Empty state:** If no results match, show: *"No results for '[query]'"* with a suggestion — *"Try searching by phone number or channel."* Never show a bare empty list.
6. **Clear:** A clear/reset button (×) appears in the input once text is entered. Clearing returns to the default state (no results, no skeleton).

**Query handling:**
- Minimum 2 characters to trigger search (prevents meaningless broad results)
- Highlight the matching query string in results (bold or `cv-text-link` color)
- Results are ranked: exact title matches first, then message body matches

## Components Used

- **Search input** — with clear button; receives keyboard focus on activation
- **List** — result rows, grouped by type section headers
- **Empty state** — "No results" view with contextual suggestion
- **Badge** — channel indicator (WhatsApp, Telegram) on each result row
- **Skeleton** — loading placeholder rows while results are fetching

## Platform Considerations

### Web (Next.js + Tailwind + shadcn/ui)

Activate search via **Cmd+K** (Mac) / **Ctrl+K** (Windows/Linux). The global search opens as a modal command palette — centered, full-width up to 640px, overlaying the page with a backdrop. This is the primary search entry point.

A secondary search input exists within the conversations sidebar for filtering within the current view.

Keyboard shortcuts within results: `↑` / `↓` to navigate, `Enter` to open, `Escape` to close.

### iOS (Expo + NativeWind)

Search is accessed via **pull-down** on the conversations list (iOS standard pattern — UISearchController behavior). A search bar slides down from above the list. The keyboard opens automatically.

Results appear inline below the search bar, replacing the conversation list. The channel indicator badge appears to the left of each result.

Dismiss by pressing `Cancel` or swiping down to close the keyboard.

### Android (Expo + NativeWind)

Search is accessed via a **search icon in the header** (top-right). Tapping it expands the header into a search field (Material-adjacent pattern). The back button collapses search and returns to the list.

Results display below the expanded header in the same list area.

## Do / Don't

| Do | Don't |
|----|-------|
| Debounce 300ms before sending search requests | Require the user to press Enter to initiate search |
| Show skeleton rows while results are loading | Clear the previous results before new ones arrive (causes layout jump) |
| Group results by type (Conversations, Contacts, Messages) | Show a flat unsorted list of all result types mixed together |
| Highlight the matching query string in result text | Navigate away from search on result selection without preserving query state |
| Show an empty state with a helpful suggestion | Show a blank empty list with no guidance |
| Include a channel indicator badge on every result | Search only within the current channel when cross-channel results are possible |

## Chatverce-Specific

**Cross-channel search:** Chatverce's primary differentiator in search is unified results across all connected channels. A search for "Priya" should return WhatsApp conversations with Priya, Telegram conversations with Priya, and any contacts named Priya — grouped by type, with channel indicator badges making the source immediately clear.

**Channel indicator in results:** Every search result that originates from a specific channel must display the channel badge (e.g., WhatsApp green dot, Telegram blue dot). Users must always know where a conversation lives before opening it.

**Message-level search:** Chatverce supports searching within message bodies. Results from message body matches display the matching message snippet with the query term highlighted, beneath the conversation name.

**No results suggestion:** Tailor the empty state suggestion to the query context. If the query looks like a phone number (starts with +, contains only digits), suggest: *"Try searching for the contact name instead."* If the query is very short (2 characters), suggest: *"Add more characters for better results."*
