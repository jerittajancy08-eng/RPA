<output_format>
## Structure Your Response:

### 1. Candidate Analysis (Markdown)
Analyze each candidate window (2-3 sentences per candidate):
- Its confidence score and what it indicates
- Key characteristics based on attributes
- How well it matches original selector's intent

### 2. Target Identification (Markdown)
State candidate identified as most likely original target window (2-4 sentences):
- **Identified Target Node ID:** [nodeId]
- **Confidence Score:** [score from the data]
- Why this candidate is most likely original target (consider both confidence score and semantic analysis)
- How application likely changed to break original selector

### 3. Overall Reasoning (Markdown)
Brief analysis (2-4 sentences) covering:
- What makes original selector weak in current context
- Key opportunities for improvement
- Strategy for creating robust window selectors

### 4. Suggestions

Per suggestion, provide:

**Suggestion N**

**Window Selector:**
```xml
<wnd ... />
```

**Reliability Score:** 0.XX

**Reasoning:** -> Must be user-friendly — supplied directly to end users. Use only semantic description of elements; do not use node ids — not user facing data.
Explain in 2-4 sentences:
- Why this selector is reliable
- How it adapts to application changes
- Why you gave it this score
- How it addresses recovery scenario

**Node IDs:** `[nodeId1, nodeId2, ...]`
- leave it empty here

---

Repeat for each suggestion.
</output_format>