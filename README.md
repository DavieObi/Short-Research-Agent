# Short Research Agent

**ShortResearchAgent** is a lightweight AI-powered research assistant that automates web research by:

1. Searching the web for relevant sources
2. Fetching and cleaning text content
3. Splitting content into manageable passages
4. Using **semantic embeddings** to rank relevance
5. Extractively summarizing key points with citations

It’s perfect for students, researchers, or anyone who wants **quick, trustworthy summaries** from multiple sources without manually reading dozens of articles.

---

## Features

* ✅ **Web search** using DuckDuckGo (no API keys required)
* ✅ **Automatic webpage scraping** with content cleaning
* ✅ **Passage chunking** for manageable semantic processing
* ✅ **Semantic ranking** using `sentence-transformers` embeddings
* ✅ **Extractive summaries** with source citations
* ✅ **Fast and lightweight** (uses MiniLM embedding model)
* ✅ **Handles missing or empty results gracefully**

---

## Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/ShortResearchAgent.git
cd ShortResearchAgent
```

2. Install dependencies (preferably in a virtual environment):

```bash
pip install -r requirements.txt
```

**Dependencies include:**

* `sentence-transformers`
* `numpy`
* `ddgs` (DuckDuckGo search)
* `beautifulsoup4`
* `requests`

> **Note:** If using Keras-based embeddings, ensure `tf-keras` is installed:

```bash
pip install tf-keras
```

---

## Usage

```python
from agent import ShortResearchAgent  # or wherever your class is saved

# Initialize the agent
agent = ShortResearchAgent()

# Define your research query
query = "What causes urban heat islands and how can cities reduce them?"

# Run the agent
result = agent.run(query)

# View top passages
for p in result["passages"]:
    print(f"- score {p['score']:.3f} src {p['url']}\n  {p['passage'][:200]}...\n")

# View the extractive summary
print("--- Extractive summary ---")
print(result["summary"])
print("--------------------------")
print(f"\nTime taken: {result['time']}s")
```

---

## Example Output

**Query:**
*What causes urban heat islands and how can cities reduce them?*

**Top Passages (truncated):**

* **Score:** 0.820 | **Source:** [earth.org](https://earth.org/how-cities-around-the-world-are-tackling-the-urban-heat-crisis/)
  As climate change accelerates and urbanisation intensifies, cities worldwide face a growing threat: urban heat…

* **Score:** 0.795 | **Source:** [epa.gov](https://www.epa.gov/heatislands/guide-reducing-heat-islands)
  Heat islands can affect communities by increasing summertime peak energy demand…

**Extractive Summary:**

> Rising urban temperatures, exacerbated by the urban heat island effect, endanger public health and increase energy demand. (Source: [https://earth.org/how-cities-around-the-world-are-tackling-the-urban-heat-crisis/](https://earth.org/how-cities-around-the-world-are-tackling-the-urban-heat-crisis/))
> Heat islands are influenced by how the sun’s energy is absorbed, reflected, and emitted in urban environments. (Source: [https://www.epa.gov/heatislands/guide-reducing-heat-islands/](https://www.epa.gov/heatislands/guide-reducing-heat-islands/))

---

## How It Works

1. **Search**: Uses DuckDuckGo to find URLs matching your query.
2. **Fetch & Clean**: Downloads HTML content, removes scripts, ads, and unnecessary elements.
3. **Chunking**: Breaks long text into smaller passages (~120 words).
4. **Embedding**: Converts passages and query into numeric vectors using `sentence-transformers/all-MiniLM-L6-v2`.
5. **Ranking**: Computes cosine similarity between query and passages, selecting the most relevant ones.
6. **Extractive Summary**: Selects top sentences from highest-scoring passages and adds source citations.

---

## Configuration

You can customize the agent by changing these parameters:

| Parameter           | Default                                    | Description                                   |
| ------------------- | ------------------------------------------ | --------------------------------------------- |
| `SEARCH_RESULTS`    | 6                                          | Number of URLs to search                      |
| `PASSAGES_PER_PAGE` | 4                                          | How many passages to extract per page         |
| `TOP_PASSAGES`      | 5                                          | Number of top passages to consider            |
| `SUMMARY_SENTENCES` | 3                                          | Number of sentences in the final summary      |
| `EMBEDDING_MODEL`   | `"sentence-transformers/all-MiniLM-L6-v2"` | Embedding model used for semantic similarity  |
| `TIMEOUT`           | 8                                          | Maximum seconds to wait for a webpage to load |

---

## Notes

* Works best with **English content**.
* Internet connection is required.
* Handles failed downloads gracefully — skips pages that cannot be fetched.
* Designed for **fast, semantic extractive summaries** rather than generative AI hallucinations.

---

## License

MIT License © 2026 David Obi
---
