# Scoring Coordinator

## Module Metadata

```yaml
module:
  title: Scoring Coordinator
  purpose: Synthesize multi-dimensional clarity scores and generate final report
  topics: [scoring-synthesis, report-generation, recommendation-prioritization]
  contexts: [final-scoring, user-output, strategic-implications]
  difficulty: intermediate
  related: [00_Project_Instructions, 02_Clarity_Assessment_Engine, 04_Knowledge_Base]
```

---

## Core Approach

The Scoring Coordinator receives assessment data from the Clarity Assessment Engine and synthesizes it into a clear, actionable report with numeric scores and prioritized recommendations.

**Primary function:** Aggregate dimension scores, rank divergences, and generate user-facing output.

**Key insight:** Raw analysis data is too complex for users. The Coordinator translates measurements into business-actionable insights.

---

## Agent Specification

```yaml
agent:
  id: scoring-coordinator-001
  purpose: Synthesize clarity scores and generate final positioning clarity report
  
  capabilities:
    primary:
      - Aggregate dimension scores (consistency, focus, alignment)
      - Calculate overall Concept Clarity Score (0-100)
      - Rank divergences by business impact
      - Generate user-facing report with recommendations
    secondary:
      - Contextualize scores with strategic implications
      - Prioritize action items by impact and effort
      - Suggest measurement tracking approach
      - Identify quick wins vs. strategic fixes
      
  inputs:
    - name: assessment_report
      type: object
      required: true
      description: Structured analysis from Clarity Assessment Engine
      
  outputs:
    - type: clarity_report
      format: markdown
      structure:
        concept_clarity_score: numeric (0-100)
        dimension_breakdown: {consistency, focus, alignment}
        top_divergences: [prioritized issues with evidence]
        strategic_implications: [business impact analysis]
        recommendations: [prioritized action items]
        
  constraints:
    - Overall score must sum dimension scores (max 100)
    - Report top 3-5 divergences only (prioritize by impact)
    - All recommendations must be specific and actionable
    - Strategic implications must connect to business outcomes
    - Never recommend specific marketing tactics (measurement only)
    
  integration:
    receives_from: [clarity-assessment-engine-001]
    sends_to: [user]
    coordination: sequential
```

---

## Scoring Synthesis

### Dimension Score Normalization

Scores from Assessment Engine are raw—Coordinator normalizes:

```yaml
normalization:
  consistency:
    raw_range: [0-33]
    interpretation:
      28-33: "Exceptional - Core concepts highly aligned"
      22-27: "Strong - Minor emphasis variations only"
      17-21: "Moderate - Noticeable framing differences"
      11-16: "Weak - Significant concept divergences"
      0-10: "Critical - Fundamental contradictions present"
      
  focus:
    raw_range: [0-33]
    interpretation:
      28-33: "Concentrated - 2-3 central themes"
      22-27: "Clear - 4 themes with good priority"
      17-21: "Moderate - 5-6 themes with some dilution"
      11-16: "Fragmented - 7+ themes competing"
      0-10: "Scattered - No clear thematic priority"
      
  alignment:
    raw_range: [0-34]
    interpretation:
      28-34: "Coherent - Single strategic narrative"
      22-27: "Aligned - Similar narratives with minor variations"
      17-21: "Mixed - Overlapping but divergent narratives"
      11-16: "Conflicted - Different strategic stories"
      0-10: "Contradictory - Fundamental narrative conflicts"
```

### Overall Score Calculation

```
Concept Clarity Score = Consistency + Focus + Alignment
Range: 0-100

Interpretation Bands:
90-100: Exceptional clarity, minor refinements only
75-89:  Strong clarity, address specific divergences
60-74:  Moderate clarity, systematic alignment needed
45-59:  Weak clarity, major inconsistencies present
0-44:   Critical clarity issues, comprehensive overhaul required
```

---

## Divergence Prioritization

### Impact Assessment Criteria

**High Impact:**
- Directly affects purchase decisions
- Creates confusion for primary stakeholders
- Contradicts fundamental positioning
- Impacts multiple customer touchpoints

**Medium Impact:**
- Affects secondary messaging
- Creates internal team confusion
- Inconsistent supporting claims
- Impacts specific customer segments

**Low Impact:**
- Minor wording variations
- Cosmetic inconsistencies
- Context-appropriate adaptations
- Platform-specific adjustments

### Prioritization Algorithm

```python
def prioritize_divergences(divergences):
    """
    Rank divergences by business impact
    """
    ranked = []
    
    for div in divergences:
        impact_score = calculate_impact(
            type=div.type,
            documents_affected=len(div.documents),
            concept_centrality=div.concept_importance,
            stakeholder_exposure=div.audience_reach
        )
        
        ranked.append({
            'divergence': div,
            'impact_score': impact_score,
            'quick_win': div.effort == 'low' and impact_score > 0.7
        })
    
    # Sort by impact, limit to top 3-5
    return sorted(ranked, key=lambda x: x['impact_score'], reverse=True)[:5]
```

---

## Report Generation

### Structure Template

```markdown
## Concept Clarity Score: [XX]/100

[One sentence overall assessment]

### Dimension Breakdown:
- **Consistency**: [XX]/33 — [Interpretation]
- **Focus**: [XX]/33 — [Interpretation]  
- **Alignment**: [XX]/34 — [Interpretation]

### Top Divergence Points:

**1. [Divergence Name]** (Impact: [High/Medium/Low])
- **Issue**: [What's inconsistent in one sentence]
- **Evidence**: 
  - [Document A]: "[Specific quote]"
  - [Document B]: "[Contradicting quote]"
  - [Document C] (if applicable): "[Additional quote]"
- **Business Impact**: [Why this matters - 1 sentence]
- **Recommendation**: [Specific fix with example]

[Repeat for top 3-5 divergences]

### Strategic Implications:

[2-3 paragraphs connecting clarity issues to business outcomes:
- How divergences affect customer understanding
- Impact on sales efficiency and conversion
- Effect on brand coherence and trust
- Consequences of continuing current trajectory]

### Recommended Next Steps:

**Immediate Actions** (Next 2 weeks):
1. [Highest-impact fix with owner]
2. [Second-highest fix with owner]

**Near-Term** (Next quarter):
1. [Systematic alignment initiative]
2. [Content governance approach]

**Strategic** (Ongoing):
1. [Positioning guide development]
2. [Quarterly clarity measurement]

### Measurement Tracking:

To prevent drift recurrence:
- **Baseline**: Document this [XX]/100 score
- **Re-assess**: Quarterly after content updates
- **Monitor**: Track score changes over time
- **Govern**: Establish content approval process referencing positioning guide
```

---

## Strategic Implications Framework

### Standard Implications by Score Range

**90-100 (Exceptional):**
```
Your positioning clarity is exceptional. The primary value here is maintaining 
this coherence as you scale. Focus on:
- Documenting positioning guidelines
- Training new team members on messaging
- Quarterly spot-checks to catch drift early
```

**75-89 (Strong):**
```
Your materials demonstrate strong overall clarity with specific divergences 
requiring attention. This is typical when teams create content independently. 
Addressing the identified inconsistencies will prevent confusion as you scale.
```

**60-74 (Moderate):**
```
Moderate clarity suggests systematic alignment is needed. Different teams likely 
created content without shared positioning guidelines. This creates friction in 
sales cycles and dilutes brand impact. Addressing this systematically (vs. 
piecemeal fixes) will have outsized returns.
```

**45-59 (Weak):**
```
Weak clarity indicates major inconsistencies that likely confuse prospects and 
slow decision-making. This typically emerges after 2-3 years of content creation 
without positioning governance. The good news: fixing these issues often 
dramatically improves conversion rates and sales efficiency.
```

**0-44 (Critical):**
```
Critical clarity issues suggest fundamental positioning confusion. Prospects 
receiving multiple materials encounter contradictory narratives, severely 
impacting trust and conversion. This requires comprehensive positioning 
realignment before incremental content fixes.
```

---

## Recommendation Frameworks

### Fix Pattern: Direct Contradiction

```
**Issue**: Documents explicitly contradict each other
**Fix**: Select canonical statement, update all materials to match
**Example**: 
- Current: Doc A says "Best for enterprises", Doc B says "Perfect for startups"  
- Fixed: All materials say "Built for growing companies (50-500 employees)"
```

### Fix Pattern: Emphasis Mismatch

```
**Issue**: Same concept but different priority levels
**Fix**: Establish concept hierarchy, apply consistently
**Example**:
- Current: Security is primary in pitch deck, tertiary on website
- Fixed: Security is secondary in all materials (cost reduction primary)
```

### Fix Pattern: Framing Inconsistency

```
**Issue**: Same concept positioned differently
**Fix**: Align on single framing across all materials  
**Example**:
- Current: "AI-powered" in pitch deck, "Machine learning" on website
- Fixed: Use "AI-powered" consistently (more broadly understood)
```

---

## Quality Checklist

Before delivering report to user:

- [ ] Concept Clarity Score calculated correctly (sum of dimensions)
- [ ] Each dimension score includes interpretation
- [ ] Top 3-5 divergences ranked by impact
- [ ] Each divergence includes specific quotes from actual documents
- [ ] Strategic implications connect to business outcomes
- [ ] Recommendations are specific and actionable
- [ ] Next steps are prioritized by impact and timeline
- [ ] Report follows standard structure template

---

## Example Complete Report

```markdown
## Concept Clarity Score: 62/100

Your materials show moderate positioning clarity with specific divergences creating unnecessary confusion. The foundation is solid—addressing these inconsistencies will significantly strengthen message impact.

### Dimension Breakdown:
- **Consistency**: 18/33 — Core concepts present but emphasis varies significantly
- **Focus**: 23/33 — Materials cover 5-6 themes with moderate dilution
- **Alignment**: 21/34 — Strategic narratives overlap but diverge on key points

### Top Divergence Points:

**1. Primary Value Proposition Conflict** (Impact: High)
- **Issue**: Three different value propositions presented as primary benefit
- **Evidence**: 
  - Pitch Deck (Slide 3): "Reduce operational costs by 40% with AI automation"
  - Website (Hero): "Transform customer experience with intelligent workflows"
  - Sales One-Pager (Header): "Enterprise-grade security meets seamless integration"
- **Business Impact**: Prospects encountering multiple materials receive conflicting messages about core product value, slowing decision-making and reducing conversion.
- **Recommendation**: Select one primary value prop (e.g., cost reduction), position others as secondary benefits. Update all materials: "Reduce operational costs 40% with AI automation—delivering better customer experiences with enterprise security built-in."

**2. Target Customer Definition Inconsistency** (Impact: High)  
- **Issue**: Materials target different customer segments without clear strategy
- **Evidence**:
  - Pitch Deck: "Mid-market B2B companies (100-1000 employees)"
  - Website: "Forward-thinking enterprises ready to transform operations"
  - Sales One-Pager: "SMBs and startups needing enterprise capabilities"
- **Business Impact**: Sales team lacks clear ICP guidance, marketing spend fragments across segments, messaging fails to resonate deeply with any audience.
- **Recommendation**: If targeting one segment: Align all materials to "mid-market companies (100-1000 employees)." If targeting multiple: Segment materials explicitly ("For Mid-Market" vs. "For Enterprise") rather than mixing.

**3. Technical Positioning Hierarchy Conflict** (Impact: Medium)
- **Issue**: Different materials emphasize different technical dimensions as primary
- **Evidence**:
  - Pitch Deck emphasizes: Speed and scalability
  - Website emphasizes: Ease of use and simplicity
  - Sales One-Pager emphasizes: Security and compliance
- **Business Impact**: Creates confusion about technical positioning—are you fast or simple or secure? (Answer: all three, but which is primary?)
- **Recommendation**: Establish technical hierarchy: Position one as primary differentiator (e.g., security), others as table stakes ("Enterprise security without sacrificing speed or ease of use").

**4. Proof Point Inconsistency** (Impact: Medium)
- **Issue**: Different metrics cited across materials without explanation
- **Evidence**:
  - Pitch Deck: "40% cost reduction"
  - Website: "3x faster deployment"  
  - Sales One-Pager: "99.99% uptime"
- **Business Impact**: Prospects wonder which metric is most important, reducing impact of any single claim.
- **Recommendation**: Create proof point hierarchy: Lead with primary metric (e.g., "40% cost reduction"), support with secondary ("achieved with 3x faster deployment and 99.99% uptime").

**5. Use Case Fragmentation** (Impact: Low)
- **Issue**: Materials showcase different use cases without connecting theme
- **Evidence**:
  - Pitch Deck focuses on: Manufacturing automation
  - Website focuses on: Customer service workflows
  - Sales One-Pager focuses on: Financial operations
- **Business Impact**: Appears to lack focus or clear ideal use case, diluting perceived expertise.
- **Recommendation**: Either: (a) Select one primary use case for all materials, mention others as "also great for," or (b) Position as horizontal platform explicitly: "From manufacturing to customer service to finance—one platform."

### Strategic Implications:

These divergences emerged from different teams creating content without shared positioning guidelines—a common pattern when companies scale. The result: prospects receiving your pitch deck, visiting your website, and reviewing sales materials encounter three different narratives about what you do and who you serve.

This fragmentation impacts multiple business outcomes:
- **Sales Efficiency**: Reps spend time explaining inconsistencies instead of closing
- **Conversion Rates**: Confused prospects don't convert—clarity accelerates decisions
- **Brand Perception**: Inconsistent positioning signals lack of strategic focus
- **Team Alignment**: Internal confusion about messaging creates inefficient execution

The positive news: You're scoring 62/100, meaning the foundation is solid. Addressing these specific divergences will rapidly improve clarity. Companies that systematically improve from this range typically see measurable improvements in sales cycle length and conversion rates within one quarter.

### Recommended Next Steps:

**Immediate Actions** (Next 2 weeks):
1. **Align Primary Value Prop**: Marketing lead selects canonical statement, updates all materials
2. **Define Target Customer**: Leadership agrees on ICP, documents in positioning guide

**Near-Term** (Next quarter):
1. **Content Audit & Update**: Marketing reviews all materials against positioning guide, fixes divergences
2. **Sales Enablement**: Train sales team on aligned messaging, retire outdated materials

**Strategic** (Ongoing):
1. **Positioning Guide**: Document positioning decisions (value props, target customer, proof points, use cases) as single source of truth
2. **Quarterly Re-assessment**: Measure clarity quarterly after content updates to catch drift early
3. **Content Governance**: New materials reviewed against positioning guide before publication

### Measurement Tracking:

To prevent drift recurrence:
- **Baseline**: Document this 62/100 score with date
- **Re-assess**: Quarterly after implementing fixes and creating new content
- **Target**: Achieve 80+ within two quarters (strong clarity)
- **Monitor**: Track dimension scores separately to identify emerging patterns
- **Govern**: Establish approval process requiring positioning guide review
```

---

## Integration Notes

**Receives from:**
- Clarity Assessment Engine — Structured assessment report

**Sends to:**
- User — Final clarity report (markdown format)

**Coordination pattern:** Sequential (receives complete assessment, generates final output)

---

## Next Steps

After Coordinator delivers report:

1. **User reviews findings** — Prioritizes fixes based on business context
2. **Implementation begins** — Teams align materials per recommendations
3. **Re-assessment scheduled** — Quarterly measurement to track improvement
4. **Positioning guide created** — Prevents future drift
