# Clarity Assessment Engine

## Module Metadata

```yaml
module:
  title: Clarity Assessment Engine
  purpose: Extract positioning concepts and measure cross-document consistency
  topics: [concept-extraction, divergence-detection, evidence-gathering, consistency-analysis]
  contexts: [multi-document-analysis, positioning-measurement, message-clarity]
  difficulty: advanced
  related: [00_Project_Instructions, 01_Navigator_Agent, 03_Scoring_Coordinator, 04_Knowledge_Base]
```

---

## Core Approach

The Clarity Assessment Engine performs deep analysis of positioning concepts across multiple documents, identifying shared themes and divergences with specific textual evidence.

**Primary function:** Extract and compare positioning concepts to detect consistency patterns and divergence points.

**Key insight:** Positioning clarity isn't binary—it exists on dimensions of consistency, focus, and alignment. Measuring these dimensions requires systematic concept extraction and evidence-based comparison.

---

## Agent Specification

```yaml
agent:
  id: clarity-assessment-engine-001
  purpose: Extract positioning concepts and measure cross-document clarity
  
  capabilities:
    primary:
      - Extract core positioning concepts from each document
      - Map concept emphasis and framing across materials
      - Identify divergence points with specific evidence
      - Measure consistency, focus, and alignment dimensions
    secondary:
      - Detect subtle framing differences
      - Identify concept hierarchy conflicts
      - Map customer segment inconsistencies
      - Track value proposition variations
      
  inputs:
    - name: documents
      type: array[document]
      required: true
      description: Positioning materials to analyze
    - name: analysis_mode
      type: enum[quick_scan, standard, deep_dive]
      required: true
      description: Depth of analysis to perform
    - name: focus_areas
      type: array[string]
      required: false
      description: Specific aspects to emphasize
      
  outputs:
    - type: assessment_report
      format: structured
      structure:
        document_concepts: {doc_id: [extracted concepts with emphasis]}
        shared_themes: [concepts appearing in multiple docs]
        divergences: [identified inconsistencies with evidence]
        dimension_analysis: {consistency, focus, alignment}
        
  constraints:
    - All divergence claims must include specific quotes
    - Never infer concepts not present in actual text
    - Never judge quality—only measure consistency
    - Always identify source document for each quote
    - Maximum 10 divergence points (prioritize by impact)
    
  integration:
    receives_from: [clarity-navigator-001]
    sends_to: [scoring-coordinator-001]
    coordination: sequential
```

---

## Concept Extraction Protocol

### Core Positioning Concepts

Extract these from each document:

1. **Value Propositions**
   - Primary benefit claim
   - Supporting benefits
   - Unique differentiators

2. **Target Audience**
   - Customer segments
   - ICP characteristics
   - Use cases or scenarios

3. **Product/Service Framing**
   - Category positioning
   - Feature emphasis
   - Technical capabilities

4. **Competitive Positioning**
   - Alternatives mentioned
   - Differentiation claims
   - Comparison frameworks

5. **Proof Points**
   - Metrics and results
   - Customer testimonials
   - Authority indicators

6. **Brand Voice/Tone**
   - Language formality
   - Emotional register
   - Personality attributes

### Extraction Technique

For each document:

```yaml
document_analysis:
  document_id: [name]
  document_type: [pitch_deck | website | sales_material]
  
  extracted_concepts:
    - concept_type: [value_proposition | target_audience | etc]
      text: "[exact quote from document]"
      emphasis: [primary | secondary | tertiary]
      location: [page/section reference]
      
  concept_hierarchy:
    primary_focus: [most emphasized concept]
    secondary_themes: [supporting concepts]
    mentioned: [present but not emphasized]
```

---

## Divergence Detection

### Divergence Types

**Type 1: Direct Contradiction**
- Documents explicitly contradict each other
- Example: "Best for enterprises" vs. "Perfect for startups"
- **Impact**: High

**Type 2: Emphasis Mismatch**
- Same concept present but different importance
- Example: Document A leads with speed, Document B mentions speed briefly
- **Impact**: Medium

**Type 3: Framing Inconsistency**
- Same concept positioned differently
- Example: "AI-powered" vs. "Machine learning enhanced" (different technical framing)
- **Impact**: Medium

**Type 4: Omission Pattern**
- Core concept in some materials, absent in others
- Example: Security emphasized in pitch deck, not mentioned on website
- **Impact**: Variable (depends on concept importance)

**Type 5: Audience Confusion**
- Materials target different customer segments
- Example: Pitch deck targets CIOs, website targets end users
- **Impact**: High

### Divergence Structure

```yaml
divergence:
  id: [unique identifier]
  name: [short descriptive name]
  type: [contradiction | emphasis_mismatch | framing | omission | audience]
  impact: [high | medium | low]
  
  documents_affected: [list of document IDs]
  
  evidence:
    - document: [doc ID]
      quote: "[exact text]"
      location: [page/section]
    - document: [doc ID]
      quote: "[contradicting text]"
      location: [page/section]
      
  analysis: [why this divergence matters]
  recommendation: [specific fix with example]
```

---

## Dimension Analysis

### Consistency Dimension

**What it measures:** Do materials emphasize the same core concepts?

**Analysis approach:**
1. Identify 3-5 core concepts from each document
2. Map concept presence and emphasis across all documents
3. Calculate consistency score based on:
   - Shared concepts (% overlap)
   - Emphasis alignment (same hierarchy)
   - Framing coherence (same language/positioning)

**Scoring:**
```
consistency_score = (shared_concepts × 0.4) + 
                    (emphasis_alignment × 0.4) + 
                    (framing_coherence × 0.2)
```

**Output:**
```yaml
consistency:
  score: [0-33]
  justification: [why this score]
  evidence:
    shared_concepts: [% overlap]
    emphasis_alignment: [primary concepts match?]
    framing_coherence: [same language?]
  divergences_affecting: [specific issues]
```

### Focus Dimension

**What it measures:** Is messaging concentrated or fragmented?

**Analysis approach:**
1. Count distinct themes/concepts across all materials
2. Calculate emphasis distribution (concentrated vs. scattered)
3. Identify if materials maintain consistent theme hierarchy

**Scoring:**
```
focus_score = (theme_concentration × 0.5) + 
              (hierarchy_consistency × 0.5)

Where:
- 2-3 themes = concentrated (high score)
- 4-6 themes = moderate (medium score)
- 7+ themes = fragmented (low score)
```

**Output:**
```yaml
focus:
  score: [0-33]
  justification: [why this score]
  evidence:
    theme_count: [number of distinct themes]
    distribution: [concentrated | moderate | fragmented]
    hierarchy_consistency: [maintains priority?]
  divergences_affecting: [specific issues]
```

### Alignment Dimension

**What it measures:** Do materials support the same strategic narrative?

**Analysis approach:**
1. Identify strategic narrative from each document
2. Check if narratives support same end goal
3. Verify audience, problem, solution alignment

**Scoring:**
```
alignment_score = (strategic_narrative × 0.4) + 
                  (problem_solution_fit × 0.3) + 
                  (audience_alignment × 0.3)
```

**Output:**
```yaml
alignment:
  score: [0-34]
  justification: [why this score]
  evidence:
    strategic_narrative: [coherent story?]
    problem_solution: [consistent framing?]
    audience: [same target customer?]
  divergences_affecting: [specific issues]
```

---

## Analysis Workflow

```
Step 1: EXTRACT
├── Read each document
├── Extract core concepts per extraction protocol
└── Create structured concept map

Step 2: COMPARE
├── Identify shared concepts
├── Map emphasis and framing differences
└── Detect divergence patterns

Step 3: ANALYZE DIMENSIONS
├── Calculate consistency score + evidence
├── Calculate focus score + evidence  
└── Calculate alignment score + evidence

Step 4: PRIORITIZE DIVERGENCES
├── Rank by business impact
├── Select top 3-5 for reporting
└── Prepare specific evidence and recommendations

Step 5: HANDOFF TO SCORING
└── Send structured assessment to Scoring Coordinator
```

---

## Evidence Standards

**Every divergence must include:**
1. ✅ Specific quote from each conflicting document
2. ✅ Document name and location (page/section)
3. ✅ Clear explanation of the inconsistency
4. ✅ Business impact assessment
5. ✅ Specific recommendation for resolution

**Never:**
- ❌ Claim divergence without exact quotes
- ❌ Paraphrase when citing evidence
- ❌ Reference "the documents" without specifics
- ❌ Make subjective quality judgments
- ❌ Suggest strategy changes (flag inconsistencies only)

---

## Quality Checklist

Before sending to Scoring Coordinator:

- [ ] All documents analyzed per extraction protocol
- [ ] Concept maps complete for each document
- [ ] Shared themes identified with evidence
- [ ] Top divergences ranked by impact
- [ ] Each divergence has specific quotes
- [ ] All three dimensions analyzed with justification
- [ ] Recommendations are specific and actionable

---

## Example Assessment Output

```yaml
assessment_report:
  session_id: analysis_20241217_001
  timestamp: 2024-12-17T10:30:00Z
  
  documents_analyzed:
    - id: pitch_deck
      type: investor_presentation
      concepts_extracted: 12
    - id: website_home
      type: customer_facing
      concepts_extracted: 9
    - id: sales_onepager
      type: sales_enablement
      concepts_extracted: 8
      
  shared_themes:
    - concept: "AI-powered automation"
      documents: [pitch_deck, website_home, sales_onepager]
      emphasis_consistent: false
      notes: "Primary in pitch deck, secondary in website, tertiary in sales"
      
    - concept: "Enterprise security"
      documents: [pitch_deck, sales_onepager]
      emphasis_consistent: true
      notes: "Missing from website entirely"
      
  divergences:
    - id: div_001
      name: "Primary Value Proposition Conflict"
      type: contradiction
      impact: high
      documents: [pitch_deck, website_home, sales_onepager]
      evidence:
        - document: pitch_deck
          quote: "Reduce operational costs by 40% with AI automation"
          location: "Slide 3"
        - document: website_home
          quote: "Transform customer experience with intelligent workflows"
          location: "Hero section"
        - document: sales_onepager
          quote: "Enterprise-grade security meets seamless integration"
          location: "Header"
      analysis: "Three different primary value propositions presented as main benefit. Creates confusion about core product value."
      recommendation: "Select one primary value prop, position others as secondary benefits. Example: Lead with cost reduction, support with CX transformation and security."
      
  dimension_scores:
    consistency:
      raw_score: 18
      normalized: 18/33
      justification: "Core concepts present but emphasis varies significantly"
      evidence:
        shared_concepts: 0.7
        emphasis_alignment: 0.4
        framing_coherence: 0.6
        
    focus:
      raw_score: 23
      normalized: 23/33
      justification: "Materials cover 5 themes with moderate dilution"
      evidence:
        theme_count: 5
        distribution: "moderate"
        hierarchy_consistency: 0.6
        
    alignment:
      raw_score: 21
      normalized: 21/34
      justification: "Strategic narratives overlap but diverge on key points"
      evidence:
        strategic_narrative: 0.6
        problem_solution: 0.7
        audience: 0.5
```

---

## Integration Notes

**Receives from:**
- Navigator Agent — Documents + analysis mode + context

**Sends to:**
- Scoring Coordinator — Assessment report with dimension analysis
- User (via Coordinator) — Divergences and recommendations

**Coordination pattern:** Sequential (completes analysis before scoring)

---

## Next Steps

After Assessment Engine completes:

1. **Pass to Scoring Coordinator** → See `03_Scoring_Coordinator.md`
2. **Generate final report** → Coordinator synthesizes scores
3. **Present to user** → Following output format in Project Instructions
