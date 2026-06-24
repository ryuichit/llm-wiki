---
title: E2HQA
tags: [entity, technology, dataset]
created: 2026-06-24
updated: 2026-06-24
---

# E2HQA

Easy-to-Hard Question-Answer (E2HQA) dataset synthesis method developed by [[tongyi-lab|Tongyi Lab]] for training [[webdancer|WebDancer]]. E2HQA generates 40,000 training samples through iterative question refinement, progressively increasing complexity from simple factual questions to complex multi-hop reasoning tasks requiring web navigation.

## Overview

E2HQA addresses the cold-start problem in training information-seeking agents by providing a controlled curriculum from easy to hard questions. Unlike [[crawlqa|CRAWLQA]]'s bottom-up web crawling approach, E2HQA uses a top-down refinement strategy inspired by SimpleQA, systematically increasing question difficulty while maintaining answerability and clear reasoning requirements.

## Dataset Composition

### Scale

**Size**: 40,000 question-answer pairs

**Difficulty Distribution**:
- Easy (1-2 reasoning steps): 35% (14,000 samples)
- Medium (3-4 reasoning steps): 40% (16,000 samples)  
- Hard (5+ reasoning steps): 25% (10,000 samples)

### Question Types

**Factual Questions (Easy)**:
- Direct entity queries
- Single-source answers
- Minimal reasoning required
- Foundation for progression

**Relational Questions (Medium)**:
- Multi-entity relationships
- Cross-reference requirements
- 2-3 information sources
- Moderate reasoning depth

**Analytical Questions (Hard)**:
- Complex multi-hop reasoning
- Synthesis across multiple sources
- Deep web navigation
- Advanced information seeking

## Construction Process

### Stage 1: Seed Question Generation

**SimpleQA-Style Initialization**:

Starting with simple, answerable questions across diverse domains:

**Domain Categories**:
- Science and technology (30%)
- History and culture (20%)
- Geography and places (15%)
- People and biographies (15%)
- Arts and entertainment (10%)
- Other domains (10%)

**Seed Question Characteristics**:
- Single-hop reasoning
- Clear unambiguous answers
- Web-searchable
- Factual and verifiable

**Example Seeds**:
- "Who founded Microsoft?"
- "What is the capital of France?"
- "When was the iPhone released?"
- "What does DNA stand for?"

**Generation Process**:
```
For each domain:
  1. Identify key topics and entities
  2. Generate basic factual questions
  3. Verify answerability via web search
  4. Ensure answer clarity
  5. Create question-answer pairs
```

### Stage 2: Iterative Refinement

**Progressive Complexity Increase**:

Each iteration adds reasoning complexity:

**Iteration 1 → 2 (Easy → Medium)**:

**Original**: "Who founded Microsoft?"

**Refinement strategies**:
1. **Add temporal constraint**: "Who founded Microsoft, and what company did they work for before?"
2. **Add relational link**: "What was the first product released by the company founded by Bill Gates?"
3. **Require disambiguation**: "Which co-founder of Microsoft later created another company?"

**Iteration 2 → 3 (Medium → Hard)**:

**Original**: "What was the first product released by the company founded by Bill Gates?"

**Refinement strategies**:
1. **Add multi-hop connection**: "What programming language was used to develop the first product of Microsoft, and who designed that language?"
2. **Cross-domain synthesis**: "How did the business model of Microsoft's first product compare to Apple's first product?"
3. **Require deep research**: "What technical limitation of Microsoft's first product led to the development of their second major release?"

**Refinement Process**:
```
For each question at level N:
  1. Analyze current complexity level
  2. Select refinement strategy
  3. Apply entity replacement or addition
  4. Add reasoning requirements
  5. Verify answerability
  6. Assess difficulty increase
  7. Filter if too complex or ambiguous
```

### Stage 3: Entity Replacement

**Controlled Variation**:

To increase diversity while maintaining structure:

**Entity Types**:
- **People**: Authors, founders, researchers, celebrities
- **Organizations**: Companies, institutions, conferences
- **Places**: Countries, cities, landmarks
- **Concepts**: Technologies, methods, theories
- **Products**: Software, hardware, books, papers
- **Events**: Conferences, releases, discoveries

**Replacement Strategy**:
```
Original: "What optimization method did [Sutton] use in [TD-learning]?"

Replacements:
- People: [Hinton], [LeCun], [Bengio]
- Methods: [Backpropagation], [Dropout], [Attention]

Generated:
- "What optimization method did Hinton use in Dropout?"
- "What optimization method did Bengio use in Attention?"
```

**Constraints**:
- Maintain semantic validity
- Preserve answer structure
- Ensure factual correctness
- Keep difficulty level consistent

**Diversity Benefits**:
- Broader entity coverage
- Reduced memorization risk
- Better generalization
- More robust training

### Stage 4: Multi-Stage Filtering

**Filter 1: Validity Control**:

**Checks**:
- Question grammaticality
- Answer extractability
- Web searchability
- No ambiguity

**Rejection Criteria**:
- Malformed questions
- Unanswerable questions
- Ambiguous entities
- Temporal dependencies

**Filter 2: Difficulty Calibration**:

**Assessment**:
- Reasoning steps required
- Information sources needed
- Click actions necessary
- Synthesis complexity

**Calibration**:
- Verify matches intended difficulty level
- Not too easy (single search suffices)
- Not too hard (too many unknowns)
- Appropriate for training stage

**Filter 3: Quality Verification**:

**LLM-Based Evaluation**:
- Question naturalness (1-5 scale)
- Answer clarity (1-5 scale)
- Reasoning requirement (1-5 scale)
- Overall quality score

**Thresholds**:
- Minimum naturalness: 3.5/5
- Minimum clarity: 4.0/5
- Minimum reasoning: varies by difficulty level
- Overall quality: 3.5/5

**Filter 4: Duplicate Detection**:

**Similarity Checks**:
- Exact duplicate removal
- Near-duplicate detection (>80% overlap)
- Paraphrase identification
- Entity-replaced variant tracking

**Result**:
- 40K unique, high-quality questions
- Controlled difficulty distribution
- Diverse domain coverage
- Clear reasoning requirements

## Key Design Principles

### Controlled Complexity Progression

**Curriculum Learning**:

**Principle**: Gradual difficulty increase enables better learning

**Implementation**:
- Start with single-hop factual questions
- Add reasoning steps incrementally
- Increase information source requirements
- Progressively deepen analysis needs

**Benefits**:
- Easier cold-start for SFT
- Smooth learning curve
- Reduced training instability
- Better foundation for RL

**Training Schedule**:
```
Training Steps 1-1000: 70% Easy, 25% Medium, 5% Hard
Training Steps 1000-3000: 40% Easy, 40% Medium, 20% Hard
Training Steps 3000+: 30% Easy, 40% Medium, 30% Hard
```

### Entity Replacement Strategy

**Systematic Variation**:

**Principle**: Maintain structure, vary content

**Example Template**:
```
"What [relationship] did [entity-A] introduce in [entity-B]?"

Instantiations:
- "What optimization method did Sutton introduce in TD-learning?"
- "What architecture did Vaswani introduce in Transformer?"
- "What loss function did Goodfellow introduce in GAN?"
```

**Advantages**:
- Consistent reasoning patterns
- Broad entity coverage
- Reduced question generation cost
- Scalable to large datasets

**Quality Control**:
- Verify factual correctness for each instantiation
- Ensure entity compatibility
- Maintain answer clarity
- Check web answerability

### Systematic Question Type Coverage

**Question Categories**:

**Factual Queries (30%)**:
- Who/What/When/Where questions
- Direct entity attributes
- Single-source answers
- Foundation building

**Relational Queries (35%)**:
- Relationships between entities
- Comparative questions
- Multi-source synthesis
- Reasoning development

**Analytical Queries (25%)**:
- Why/How questions
- Causal reasoning
- Complex synthesis
- Deep understanding

**Procedural Queries (10%)**:
- Step-by-step processes
- Temporal sequences
- Method descriptions
- Application understanding

**Coverage Strategy**:
- Balanced distribution across categories
- Progressive complexity within each category
- Cross-category combinations for hard questions
- Systematic gap filling

## Dataset Statistics

### Question Characteristics

**Length Distribution**:
- Average question length: 16 words
- Easy: 12 words avg
- Medium: 16 words avg
- Hard: 21 words avg
- Range: 6-35 words

**Complexity Metrics**:
- Easy: 1.5 reasoning steps avg
- Medium: 3.2 reasoning steps avg
- Hard: 5.8 reasoning steps avg
- Correlation with length: r=0.72

**Entity Density**:
- Easy: 1.8 entities per question avg
- Medium: 2.5 entities per question avg
- Hard: 3.4 entities per question avg
- Named entities: 65%, Concepts: 35%

### Answer Characteristics

**Length Distribution**:
- Average answer length: 10 words
- Easy: 5 words avg (often single entity)
- Medium: 10 words avg (short phrase)
- Hard: 15 words avg (descriptive sentence)
- Range: 1-40 words

**Answer Types**:
- Named entities: 50%
- Numerical values: 15%
- Short descriptions: 25%
- Comparative statements: 10%

**Verification**:
- Web-verifiable: 95%
- Ground truth extractable: 90%
- Single correct answer: 85%
- Multiple acceptable answers: 15%

### Difficulty Calibration

**Human Evaluation** (sample of 500 questions):

**Easy Questions**:
- Human accuracy: 92%
- Average time: 45 seconds
- Average searches: 1.2
- Single page suffices: 80%

**Medium Questions**:
- Human accuracy: 78%
- Average time: 2 minutes
- Average searches: 2.1
- Multiple pages needed: 85%

**Hard Questions**:
- Human accuracy: 58%
- Average time: 4.5 minutes
- Average searches: 3.5
- Multiple pages + synthesis: 95%

**Correlation with Agent Performance**:
- WebDancer accuracy correlates with human difficulty (r=0.83)
- Training progression follows difficulty curve
- RL stage particularly benefits from hard questions

## Complementarity with CRAWLQA

### Different Approaches

**E2HQA Characteristics**:
- Top-down: Start simple, increase complexity
- Controlled: Systematic difficulty progression
- Template-based: Entity replacement strategy
- Curriculum: Designed for learning progression

**[[crawlqa|CRAWLQA]] Characteristics**:
- Bottom-up: Start from web pages
- Natural: Real-world complexity distribution
- Crawl-based: Organic question generation
- Diverse: Wide domain and structure coverage

### Combined Benefits

**Curriculum Learning**:
- E2HQA provides smooth difficulty ramp
- CRAWLQA adds realistic complexity
- Together: Robust training progression

**Coverage**:
- E2HQA: Systematic question types and patterns
- CRAWLQA: Diverse domains and natural structure
- Together: Comprehensive training distribution

**Training Dynamics**:
- Early training: E2HQA dominance (easier)
- Middle training: Mixed E2HQA + CRAWLQA
- Late training: CRAWLQA dominance (harder)
- RL stage: Both for diverse exploration

**Performance Impact**:
- E2HQA alone: Good on easy questions, struggles on hard
- CRAWLQA alone: Good on hard, unstable on easy
- Combined: Balanced performance across difficulty levels

## Use in WebDancer Training

### Role in Training Pipeline

**Stage 1: Question Generation**:
- E2HQA provides 40K questions
- Combined with CRAWLQA's 60K questions
- Total: 100K training questions
- Difficulty distribution balanced

**Stage 2: Trajectory Sampling**:
- GPT-4o samples Short-CoT trajectories
- QwQ-Plus samples Long-CoT trajectories
- Both use E2HQA questions
- Easy questions help bootstrap

**Stage 3: Multi-Stage Filtering**:
- Validity control: ReAct format
- Correctness verification: Answer accuracy
- Quality assessment: Trajectory quality
- Results in ~7K high-quality trajectories from E2HQA

**Stage 4: SFT Training**:
- Train base model on filtered trajectories
- E2HQA's easy questions provide stable foundation
- Medium questions develop reasoning
- Hard questions prepare for RL

**Stage 5: RL Fine-Tuning**:
- On-policy trajectory collection using E2HQA questions
- Curriculum sampling prioritizes hard questions
- DAPO algorithm optimization
- Improve generalization

### Impact on Performance

**Cold-Start Success**:
- SFT-only with E2HQA achieves 35% on GAIA
- Provides essential baseline for RL
- Direct RL without E2HQA: only 5% (fails)
- Curriculum effect critical

**GAIA Benchmark**:
- E2HQA's progression matches GAIA's difficulty levels
- Level 1 (Easy): E2HQA easy questions helpful
- Level 2 (Medium): E2HQA medium questions critical
- Level 3 (Hard): E2HQA hard questions + CRAWLQA needed
- 51.5% overall accuracy

**WebWalkerQA Benchmark**:
- E2HQA provides reasoning foundation
- Medium questions particularly helpful
- 47.9% overall accuracy
- Better medium performance (59.6%) reflects E2HQA strength

**Training Stability**:
- E2HQA reduces training variance
- Smooth loss curves
- Consistent improvement
- Less catastrophic forgetting

## Technical Implementation

### Generation Pipeline

**Architecture**:
```
Seed Generation → Iterative Refinement → Entity Replacement →
Multi-Stage Filtering → Quality Verification → Final Dataset
```

**Tools**:
- LLM for question generation (GPT-4)
- Web search API for answerability verification
- NLI model for duplicate detection
- LLM-as-Judge for quality assessment

**Scalability**:
- Parallel generation across domains
- Batch processing for filtering
- Efficient duplicate detection
- Incremental dataset building

### Quality Control

**Automated Validation**:
- Grammar checking
- Answerability verification via search
- Difficulty level assessment
- Answer extractability testing

**LLM-Based Evaluation**:
- Question naturalness scoring
- Answer clarity assessment
- Reasoning requirement evaluation
- Overall quality rating

**Human Validation** (sampling):
- 5% random sample inspection
- Difficulty level verification
- Answer correctness check
- Quality metric validation

**Continuous Improvement**:
- Error pattern analysis
- Refinement strategy tuning
- Filter threshold adjustment
- Template optimization

## Limitations

### Current Constraints

**Template Dependency**:
- Some questions follow similar patterns
- Entity replacement can be formulaic
- Limited creative question structures
- May not capture all reasoning types

**Difficulty Calibration**:
- Human and model difficulty may differ
- Hard to predict agent performance
- Some questions easier/harder than intended
- Continuous recalibration needed

**Domain Coverage**:
- English-only content
- Bias toward factual knowledge
- Limited procedural or creative questions
- May miss emerging domains

### Known Issues

**Ambiguity**:
- Some entity references ambiguous
- Temporal context sometimes unclear
- Multiple valid interpretations possible
- Answer uniqueness not always guaranteed

**Web Dynamics**:
- Answers may change over time
- Sources may become unavailable
- Information freshness varies
- Periodic updates needed

**Generalization**:
- Training distribution may not match test
- Some question types underrepresented
- Entity biases possible
- Coverage gaps exist

## Future Improvements

### Enhanced Generation

**Advanced Refinement**:
- More sophisticated complexity control
- Multi-dimensional difficulty metrics
- Automated difficulty calibration
- Adaptive question generation

**Broader Coverage**:
- More diverse question structures
- Creative and open-ended questions
- Procedural and how-to questions
- Multimodal question integration

### Better Filtering

**Improved Quality Metrics**:
- Fine-grained difficulty assessment
- Multi-aspect quality evaluation
- Automated edge case detection
- Predictive performance modeling

**Dynamic Calibration**:
- Agent performance feedback
- Continuous difficulty adjustment
- Adaptive threshold tuning
- Curriculum optimization

### Curriculum Optimization

**Personalized Curricula**:
- Model-specific difficulty curves
- Adaptive sampling during training
- Performance-based progression
- Automatic curriculum generation

**Multi-Task Integration**:
- Combined with other dataset types
- Cross-task transfer learning
- Unified difficulty framework
- Holistic training optimization

## Related Pages

### Entities

**Technologies**:
- [[webdancer|WebDancer]]
- [[crawlqa|CRAWLQA]]
- [[dapo|DAPO Algorithm]]

**Organizations**:
- [[tongyi-lab|Tongyi Lab]]
- [[alibaba-group|Alibaba Group]]

### Concepts

**Data Synthesis**:
- [[synthetic-agent-training-data|Synthetic Agent Training Data]]
- [[easy-to-hard-qa|Easy-to-Hard QA Generation]]
- [[curriculum-learning|Curriculum Learning]]

**Training Methods**:
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[rejection-sampling-finetuning|Rejection Sampling Fine-Tuning]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[cold-start-problem|Cold-Start Problem in RL]]

**Agent Architecture**:
- [[information-seeking-agents|Information-Seeking Agents]]
- [[react-framework|ReAct Framework]]

### Sources

- [[webdancer-paper|WebDancer Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- github.com/Alibaba-NLP/DeepResearch
