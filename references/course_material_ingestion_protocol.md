# Course Material Ingestion Protocol

Use this protocol when the learner supplies PPTX, DOCX, PDF, screenshots, or a
folder of course materials and wants explanation, discussion, optional
practice, note refinement, mistake diagnosis, or an exam-oriented learning
path grounded in those files.

## Workflow

1. Run `python scripts/ingest_course_materials.py --check` when dependency
   availability is unknown.
2. Run `python scripts/ingest_course_materials.py <file-or-folder>`.
3. Read `_tutor_materials/_tutor_material_bundle.json`.
4. Visually inspect source pages or extracted candidates when formulas,
   diagrams, curves, tables, or screenshots carry meaning that text extraction
   may lose. Do not infer an unreadable formula from nearby text.
5. Build a compact course-point map:
   - covered topic and course module;
   - ordered points and dependencies inside the requested scope;
   - source locations supporting it;
   - required prerequisites;
   - likely learner gap or misconception;
   - smallest useful next teaching step.
6. Use the material's course structure as the backbone. Start from the first
   relevant point, teach one point at a time, and allow discussion before
   moving to the next point. Add background only when it repairs a prerequisite
   or deepens the current point without replacing the course framework.
7. Distinguish conversational understanding checks from formal practice. A
   brief question, learner paraphrase, or discussion prompt may support the
   current point; exercises, quizzes, scoring, and test sets are optional and
   require the learner to choose them.
8. Near the end of the requested scope, offer practice once as a choice. If the
   learner declines or does not choose it, continue to summary or note
   refinement without generating questions.
9. When the learner provides a Markdown note, load
   `markdown_note_refinement_protocol.md` and refine it from the course evidence
   plus the current conversation. Use selected PPT images or a compact Mermaid
   flowchart when they materially clarify a knowledge point.

## Evidence Rules

- Label tutor-added background or derivation as supplemental explanation.
- Keep page, slide, heading, and filename provenance when making source-specific
  claims.
- State uncertainty when extraction is incomplete, a dependency is missing,
  or visual content is unreadable.
- Treat decorative images, logos, and repeated backgrounds as low priority.
- Prefer diagrams, system blocks, plots, tables, worked examples, and formula
  derivations for visual review.
- Retain each useful PPT image's source filename and slide number so note
  refinement can embed it with provenance and a local portable asset path.
- Never claim that extracted text alone proves the learner understands it.

## Output Selection

- For “teach/explain”: reconstruct the source logic, split it into ordered
  points, then teach and discuss one point at a time.
- For “review/exam”: map tested concepts and traps. Offer the Practice &
  Mastery Loop as an optional next step; do not enter it automatically.
- For “make a full chapter pack”: a long source-grounded synthesis and question
  set is allowed only when explicitly requested. Otherwise omit the questions.
- For “完善笔记/refine notes”: use the uploaded Markdown note, current
  conversation, and verified source locations to create a corrected and
  improved Markdown version while preserving the learner's useful wording.
- For “continue later”: include relevant source locations and current blocker
  in a visible Learning State Card.

## Supported Inputs and Dependencies

- PPTX: text, tables, notes, and embedded pictures via `python-pptx`.
- DOCX: paragraphs, styles, tables, and embedded media via `python-docx`.
- PDF: page text and tables via `pdfplumber`; embedded images are additionally
  extracted when PyMuPDF is available.
- PNG/JPG/JPEG/WebP: registered for direct visual inspection.

Legacy `.ppt` and `.doc` files are not parsed. Ask the user to export them as
`.pptx` or `.docx`. The script reports missing optional dependencies instead of
installing packages automatically.
