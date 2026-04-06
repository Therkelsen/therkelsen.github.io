# Examiner Feedback: "Get Out of the Way, Stupid! Adaptive Yielding for Robot Navigation"

**Authors:** Thomas Therkelsen & Peter Frydensberg  
**Programme:** M.Eng. in Robot Systems, SDU  
**Grading Scale:** Danish 7-tier (12, 10, 7, 4, 02, 00, -3)

---

## Section-by-Section Grading and Feedback

### Abstract — Grade: 00 (Inadequate)

The abstract is entirely Lorem Ipsum placeholder text. This is a serious omission. The abstract is the single most-read part of any academic work and serves as the reader's first (and sometimes only) point of contact. Submitting a placeholder abstract in a final report signals either a last-minute rush or a fundamental lack of attention to the deliverable. This alone would raise concerns with any examiner.

**Recommendation:** Write a 150–250 word abstract summarizing the problem (deadlocks in heterogeneous AMR fleets), the approach (configuration-based coordination with five planning strategies), the key results (DWB as the most balanced, Joint Space A* as highest success rate), and the main conclusion (all planners outperform uncoordinated navigation; layered strategy recommended).

---

### 1. Introduction (Background, Problem Description, Contributions) — Grade: 10

**Strengths:**
- The motivation is clearly grounded in an industrial problem (MiR customer sites experiencing deadlocks), making the work immediately relevant.
- The four research questions (A–D) are well-formulated, specific, and testable. They span effectiveness comparison, trade-off analysis, heterogeneity effects, and architectural feasibility.
- The contribution list is concrete and maps cleanly onto the research questions.
- Good use of terminology and appropriate scope-setting.

**Weaknesses:**
- The title is informal and may not be appropriate for an academic submission. While memorable, "Get Out of the Way, Stupid!" could be seen as unprofessional. A subtitle like "Adaptive Yielding for Robot Navigation" is appropriate, but the main title undermines academic tone.
- The transition from background to problem description could be tighter; there is slight repetition between Sections 1.1 and 1.2 regarding decentralized planning leading to deadlocks.

**Recommendation:** Consider whether the title aligns with SDU's expectations for MSc reports. The content itself is strong.

---

### 2. Related Work — Grade: 10

**Strengths:**
- Comprehensive coverage of the relevant landscape: centralized MAPF, decentralized/swarm methods, reactive collision avoidance, resource allocation, and local repair.
- Excellent use of Section 2.6 ("Positioning of this Project") to clearly distinguish the contribution from prior work. This is often missing in student reports and is a hallmark of mature academic writing.
- The discussion of why CBS and M* were explored but ultimately rejected (dense roadmaps, large footprints) is honest, well-reasoned, and demonstrates genuine engagement with the literature rather than superficial citation.
- Good explanation of the gap: existing MAPF assumes direct trajectory control, which is incompatible with production navigation stacks.

**Weaknesses:**
- Citation formatting errors: "kavraki1996" and "hsu2003" appear as raw text rather than proper references in Section 5.1 (though this section is technically under Methods, the PRM discussion is related-work adjacent). This suggests incomplete bibliography management.
- The related work could benefit from a summary table comparing approaches along key dimensions (centralized vs. decentralized, completeness, scalability, stack compatibility).

**Recommendation:** Fix citation formatting. A comparison table would strengthen an already strong section.

---

### 3. System Overview — Grade: 12

**Strengths:**
- This is one of the strongest sections of the report. The description of the existing navigation stack (GlobalMove, SBPL lattice planner, MiRPlannerROS, costmaps, fleet coordination) is thorough and demonstrates deep understanding of the system they are working within.
- The "Main System Issue" subsection (temporal coordination gap) is excellently articulated. It identifies four specific failure scenarios with clear causal explanations.
- Section 3.2 on Configuration-Based Coordination is the architectural heart of the thesis and is well-presented. The three-phase operation (collision prediction, proactive stop, configuration planning) is clearly described.
- The discussion of why trajectory-level control was discarded is a highlight. It demonstrates engineering maturity: they understood that disabling safety layers, the MPC controller, and collision avoidance would create a fundamentally different and unsafe system.
- The ROS service architecture (Registration, Monitoring, Goal distribution) is cleanly documented.

**Weaknesses:**
- Minor: The section is somewhat long and could benefit from a high-level architecture diagram showing the relationship between the existing stack and the injected coordination layer.

---

### 4. Environment — Grade: 7

**Strengths:**
- Clear description of the two test maps with dimensions and spatial properties.
- Good justification for choosing MiR250 and MiR600 as representative platforms.
- Useful table of robot kinematic and geometric properties.

**Weaknesses:**
- Only two maps are used. While the authors justify this in Section 6 (and later in Section 8.2), the environment section itself feels thin.
- The maps are quite simple (a corridor with one bay, and a T-intersection). While they capture fundamental deadlock topologies, the lack of more complex environments (L-shaped corridors, cross-intersections, multi-bay layouts) limits generalizability claims.
- The figures (Fig. 3a, 3b) are referenced but their quality and informativeness cannot be fully evaluated from the text extraction. The description suggests they are adequate but basic.

**Recommendation:** Acknowledge the limitation of two maps more explicitly in this section rather than deferring it entirely to the discussion.

---

### 5. Methods — Grade: 10

**Strengths:**
- This is the most substantial section and demonstrates significant technical work. Five distinct planning strategies are designed, implemented, and described in detail.
- Each method follows a consistent structure: concept, algorithm steps, strengths, limitations. This makes comparison easy and shows good organizational thinking.
- The methods span a thoughtful range: from simple heuristic approaches (DWB) through graph-based (Bay Pass), search-based (Joint Space A*), prioritized (Priority Schedule), to game-theoretic (IBR). This is a well-designed experimental variable.
- The Joint Space A* description (Section 5.5) is particularly strong, with clear formalization of state space, collision constraints (vertex, swap, edge), heuristic design, and post-processing steps.
- The complexity analysis in Section 5.9 with the scaling plot is valuable and well-presented.
- Table 1 (qualitative comparison) is excellent and provides a clear summary.
- The discussion of PRM vs. cell-based roadmap (Section 5.1) shows good methodological reasoning.

**Weaknesses:**
- Section 5.1 contains broken citations ("kavraki1996", "hsu2003") that appear as raw text.
- The Default baseline (Section 5.2) is described at length for what is essentially "do nothing." While completeness is appreciated, this section could be more concise.
- Some algorithm descriptions are very detailed (almost code-level), which while thorough, makes the section long. A more abstract algorithmic description with pseudocode would be more readable in places.
- The inadmissibility of Joint Space A*'s heuristic (due to the corridor conflict penalty) is acknowledged but not deeply analyzed. How often does this lead to suboptimal solutions? What is the quality gap?

**Recommendation:** Fix citation formatting. Consider adding pseudocode for key algorithms. The level of detail is generally a strength but occasionally crosses into implementation specifics that belong in supplementary material.

---

### 6. Experimental Design — Grade: 7

**Strengths:**
- Clear experimental protocol: 24 runs per map per planner, 288 total runs.
- Good justification for focusing on two maps (preliminary testing showed additional maps didn't introduce new deadlock structures).
- Performance metrics are well-defined with clear mathematical formulations.
- The statistical analysis formulation (PERMANOVA model) is properly specified with null and alternative hypotheses.
- Honest acknowledgment of survivorship bias (excluding failed runs from runtime/path length analysis).

**Weaknesses:**
- The sample size is relatively small for statistical analysis. With only 288 total runs and some planners failing a significant fraction, the effective sample sizes for some comparisons are quite small (e.g., Default has only 16 successful runs).
- Only two robot types and two maps limit the generalizability of conclusions. The "combinations with replacement" approach yields only 3 type combinations (250-250, 250-600, 600-600).
- No repeated trials of the same scenario. Each configuration is run once, so there is no measure of run-to-run variability (even though the simulator is deterministic, this should be stated explicitly).
- The computing environment section is adequate but brief. It would be helpful to know if the simulator is deterministic (it seems to be, given the RCLA framework).

**Recommendation:** State explicitly whether the simulation is deterministic. If so, justify why single runs per configuration are sufficient. If not, multiple trials would be needed.

---

### 7. Experimental Results — Grade: 10

**Strengths:**
- Rigorous statistical methodology: normality testing (Shapiro-Wilk, Henze-Zirkler), non-parametric analysis (PERMANOVA, Kruskal-Wallis), and post-hoc pairwise comparisons (Dunn's test with Bonferroni correction).
- The progression from normality assessment → choice of non-parametric methods → multivariate analysis → univariate follow-up is textbook-correct and well-motivated.
- The results are presented clearly with well-structured tables.
- The identification of three statistical clusters (Default/IBR/Priority Schedule; Bay Pass/DWB; Joint Space) from the pairwise comparisons is insightful.
- The trade-off visualization (Fig. 8: success rate vs. runtime overhead) effectively communicates the practical implications.
- Table 8 (success rate by robot type) reveals the important finding that MiR600 succeeds in only 49% of scenarios.

**Weaknesses:**
- The presentation is somewhat mechanical — it reads as a sequence of statistical tests without always connecting back to the engineering implications. More interpretive text between tables would improve readability.
- The planner computation time analysis (Table 7) shows Joint Space is 3-4 orders of magnitude slower, but this is somewhat buried. This is a critical practical finding that deserves more prominence.
- The separation of planner time from PERMANOVA is justified but means the most practically important metric is analyzed separately and less prominently.

**Recommendation:** Add more interpretive narrative between statistical results. Lead with the engineering story, supported by the statistics.

---

### 8. Discussion — Grade: 7

**Strengths:**
- The future work section (8.2) is thoughtful and identifies meaningful extensions: more robot types, multi-robot deadlocks, physical deployment, adaptive planner selection, and fleet management integration.
- The adaptive planner selection idea (Section 8.2.4) is particularly compelling and demonstrates forward-thinking.

**Weaknesses:**
- The discussion section is surprisingly thin for a 50-page report. Section 8.1 discusses only DWB's computational bottleneck (clearance probe). What about the other four planners?
- There is no discussion of threats to validity (internal, external, construct). The survivorship bias is mentioned in Section 6 but not revisited here.
- The discussion does not adequately address why the coordinated planners still fail in 15-50% of scenarios. What are the failure modes? Are they inherent to the configuration-based approach or specific to the implementations?
- No discussion of the results in relation to the specific research questions (A-D). The conclusion does this partially, but the discussion should systematically address each research question.
- The constraint of working within an unmodified navigation stack is mentioned but its impact on results is not deeply analyzed. How much performance is left on the table compared to what trajectory-level control could achieve?

**Recommendation:** Significantly expand the discussion. Address each research question. Analyze failure modes. Discuss threats to validity. Compare configuration-based vs. trajectory-based coordination implications more deeply.

---

### 9. Conclusion — Grade: 10

**Strengths:**
- Concise and effective summary of findings.
- Directly addresses the research questions with specific numbers.
- The practical recommendation (layered strategy: DWB for most conflicts, escalation for failures) is a valuable engineering takeaway.
- The finding about robot heterogeneity as a "first-order factor" is clearly stated.
- The validation of the configuration-based architecture is explicitly noted.

**Weaknesses:**
- Could be slightly more structured by explicitly mapping conclusions to the four research questions (A-D).
- Does not mention limitations or caveats (placeholder abstract, limited maps, two robot types).

---

### 10. References — Grade: 7

**Strengths:**
- References are relevant and appropriately chosen, spanning foundational work (Dijkstra, Lamport, Hart/Nilsson/Raphael) and recent publications (2024-2025).
- Good mix of conference papers, journal articles, and a PhD dissertation.

**Weaknesses:**
- Only 21 references for a 50-page MSc report is somewhat light. More references in the methods sections (particularly for the specific algorithmic techniques) would strengthen the work.
- The broken in-text citations ("kavraki1996", "hsu2003") suggest bibliography management issues.
- Some references lack complete information (e.g., [2] and [3] lack volume/page numbers).

---

### 11. Appendix — Grade: 10

**Strengths:**
- The parameter tables (Tables 14-19) are thorough and provide excellent reproducibility.
- The time complexity analyses for all five planners are rigorous and clearly presented.
- The notation table (Table 20) is a nice touch for readability.

**Weaknesses:**
- The complexity analysis could include empirical validation (e.g., plotting actual runtime vs. predicted scaling).

---

## Overall Assessment

### Overall Grade: 7 (Good)

The grade would be **10** if not for the placeholder abstract, the thin discussion section, and the limited experimental scope. The technical work is strong and the system architecture is well-designed, but these presentation and scope issues prevent the report from reaching the highest tier.

---

### Academic Quality Assessment

**Strengths of academic quality:**
1. The report follows a clear academic structure (Introduction → Related Work → System → Methods → Experiments → Results → Discussion → Conclusion).
2. The statistical analysis is rigorous and well-chosen (PERMANOVA, Kruskal-Wallis, Dunn's tests with proper corrections).
3. The related work section demonstrates genuine engagement with the literature and honest positioning.
4. Research questions are formulated as testable hypotheses.
5. The methodology is reproducible: parameters are documented, the computing environment is specified, and algorithms are described in detail.

**Weaknesses of academic quality:**
1. **The Lorem Ipsum abstract is a critical failing.** An academic report without a real abstract is fundamentally incomplete. This is the single most damaging issue in the entire document.
2. **The informal title** ("Get Out of the Way, Stupid!") undermines the academic register. While attention-grabbing, it is inappropriate for a master's thesis.
3. **Broken citations** in Section 5.1 suggest incomplete proofreading.
4. **The discussion section is underdeveloped** relative to the depth of the technical work. A strong academic report requires substantive interpretation of results, not just statistical output.
5. **Limited experimental scope:** Two maps and two robot types is narrow for the claims being made. While justified, it limits external validity.

**Overall academic assessment:** The report demonstrates strong technical competence and solid methodological foundations. However, it falls short of fully mature academic writing due to the incomplete abstract, underdeveloped discussion, and occasional formatting issues. The technical depth is at a master's level; the academic presentation is slightly below.

---

### Were the Problem Statements Successfully and Correctly Answered?

**Research Question A:** *How do hybrid coordination strategies compare to uncoordinated navigation?*
- **Answered: Yes.** All five strategies significantly outperform the Default baseline (33% → 56-85% success rate). This is clearly demonstrated in Table 4 and Figure 8, with statistical confirmation via PERMANOVA. ✓

**Research Question B:** *What are the trade-offs between coordination effectiveness, computational cost, and path efficiency?*
- **Answered: Yes.** The trade-off space is well-characterized: DWB offers the best balance (113% success improvement, 22% runtime overhead), Joint Space has the highest success but 234% runtime overhead, and IBR/Priority Schedule occupy the middle ground. The Dunn's post-hoc tests identify specific statistical clusters. ✓

**Research Question C:** *To what extent does robot heterogeneity affect success rate and efficiency?*
- **Answered: Partially.** The report shows MiR600 succeeds in only 49% vs. 78% for MiR250, and PERMANOVA confirms robot pair as a significant factor (R² = 0.103). However, with only two robot types, the analysis cannot characterize the *extent* of heterogeneity effects across the full spectrum of robot sizes. The answer is correct but limited in scope. ⚠

**Research Question D:** *Can configuration-based coordination reliably resolve corridor and intersection deadlocks?*
- **Answered: Yes, with caveats.** The configuration-based approach works (all planners improve over baseline), but "reliably" is debatable — the best planner (Joint Space) still fails 12-17% of the time. The report honestly acknowledges this. The answer is nuanced and appropriate. ✓

**Overall:** The problem statements are correctly answered with appropriate evidence. The statistical methodology is sound. The main limitation is that some answers are constrained by the narrow experimental scope (2 maps, 2 robot types).

---

### Significance of the Contribution

**What is genuinely novel:**
1. The configuration-based coordination framework that operates *on top of* an unmodified production navigation stack is a practically significant contribution. Most MAPF research assumes direct trajectory control, which is unrealistic for deployed AMR fleets.
2. The systematic comparison of five coordination strategies under identical conditions on a production-grade simulation is valuable. This is not a comparison of algorithms in isolation but within a realistic industrial context.
3. The finding that DWB (the simplest coordinated method) provides the best trade-off is an important practical insight.

**What limits significance:**
1. The experimental scope is narrow: 2 maps, 2 robot types, 2 robots per scenario. This limits the generalizability of conclusions.
2. The planners are not compared against any external baselines from the literature (no comparison with, e.g., a windowed CBS variant or an ORCA-based approach adapted to the same framework).
3. The work is highly specific to the MiR ecosystem. While the principles generalize, the implementation is tied to Eagle Eye and the RCLA framework.
4. The contribution is positioned as "not a new MAPF algorithm" (Section 2.6), which is honest but also limits the novelty claim.

**Assessment:** The contribution is significant for the industrial application domain (MiR/Teradyne AMR fleet management) and makes a meaningful engineering contribution. It is less significant as a pure academic contribution to the MAPF literature, as no new algorithmic ideas are introduced — the novelty is in the integration approach and the empirical comparison. For an MSc project, particularly one conducted in an industrial context, this is an appropriate and valuable contribution.

---

### Was It Made Clear That the Injection Constraint Did Not Block Other Parts?

This is a critical point for evaluating the work, and the report addresses it in several places, but with varying degrees of clarity:

**Where it IS addressed well:**
1. **Section 3.2 (Centralized Configuration-Based Coordination)** explicitly states: *"Rather than controlling the trajectories that robots follow, the server determines a sequence of intermediate goal configurations... Each robot retains full control over how it navigates to each intermediate goal, using its own planner and collision avoidance systems unaltered."* This is clear and well-stated.

2. **Section 3.2 ("Why Trajectory Control Was Discarded")** provides a thorough explanation of why trajectory-level control was rejected: it would require disabling collision avoidance, bypassing the MPC controller, disabling the global planner, and demanding tight timing synchronization. The conclusion — *"every autonomous layer of the stack... must be dismantled"* — makes a strong case.

3. **Section 2.6** states: *"the coordination system developed here complements, rather than replaces, this stack: it sends only intermediate goal poses."*

4. **The Conclusion** states: *"The configuration-based architecture... supported all five planners without disabling collision avoidance, bypassing the MPC controller, or modifying any of the navigation stack."*

**Where it could be STRONGER:**

1. **There is no dedicated subsection** that explicitly frames the injection as an architectural constraint and systematically analyzes its implications on planner design and performance. The information is scattered across Sections 2.6, 3.2, and 9.

2. **The discussion section (8)** does NOT revisit this constraint. It should analyze: What performance ceiling does the configuration-based approach impose? How much better could the planners perform with trajectory-level control? Is the gap significant?

3. **The term "injection"** is used only once in passing (*"a server-client injection that layers on top of the existing navigation stack"*). The report would benefit from a clearer, more prominent framing of this as a deliberate design constraint — not a limitation but a feature — with explicit discussion of what it enables (safety preservation, stack compatibility) and what it costs (less precise control, no temporal guarantees).

4. **The results section** does not separate the contribution of the planning algorithms from the overhead of the configuration-based execution model. When Joint Space A* fails, is it because the planner couldn't find a solution, or because the configuration-based execution couldn't faithfully execute the plan? This distinction is important but never explicitly made.

**Assessment:** The report successfully communicates *that* the injection was non-destructive and *why* trajectory control was rejected. However, it does not sufficiently analyze *the consequences* of this architectural choice on the results. An examiner looking for this would find the key statements but might wish for a more structured and prominent treatment. The constraint is presented more as a design rationale than as a central thesis contribution, even though it arguably IS the most practically significant aspect of the work.

---

## Summary Table

| Section | Grade | Key Feedback |
|---------|-------|-------------|
| Abstract | 00 | Lorem Ipsum placeholder — unacceptable |
| Introduction | 10 | Strong motivation, clear research questions, informal title |
| Related Work | 10 | Comprehensive, well-positioned, minor citation issues |
| System Overview | 12 | Excellent architecture description, strong engineering reasoning |
| Environment | 7 | Adequate but limited scope (2 maps, 2 robot types) |
| Methods | 10 | Strong technical depth, five well-described planners |
| Experimental Design | 7 | Sound methodology but narrow scope, small sample sizes |
| Results | 10 | Rigorous statistics, clear presentation, could be more interpretive |
| Discussion | 7 | Too thin; lacks failure mode analysis, threats to validity, RQ mapping |
| Conclusion | 10 | Concise, effective, actionable recommendations |
| References | 7 | Adequate but light (21 refs); broken citations |
| Appendix | 10 | Thorough parameter documentation and complexity analysis |
| **Overall** | **7** | Strong technical work undermined by incomplete abstract, thin discussion, and limited scope |

**Note:** The overall grade of 7 reflects a weighted assessment. If the abstract were completed and the discussion section expanded to match the quality of the technical work, the report would comfortably merit a 10. The technical content alone (System Overview + Methods + Results) is at the 10-12 level. The presentation and completeness issues bring the overall assessment down.
