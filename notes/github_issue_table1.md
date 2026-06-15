# GitHub Issue: Adoption Status Table (Table 1) の取り扱い

## Issue Title

Determine publication venue for the full Adoption Status table (formerly Table 1)

## Issue Body

以下をそのままGitHub Issueの本文として使用できます：

---

### Background

The comprehensive "Adoption Status of Pairing-Friendly Curves" table (Table 1 in the v12 XML) has been removed from the I-D body in the Markdown migration. This table served as evidence that selected curves meet the "widely used" criterion in our selection policy (Section 4).

### Reason for removal

- CFRG members provided feedback that the table is too large for inclusion in the I-D body
- The table contains complex multi-column layout with security level classifications that is difficult to maintain in kramdown-rfc Markdown format

### Current status

- The table data is preserved in `notes/table_adoption.xml` (raw XML, 410 lines) and `notes/table_adoption.md` (readable Markdown, 49 lines)
- The prose descriptions in Section 4.1 (§impl) still describe the adoption status in detail
- The 100-bit security level table (smaller) remains in the document as Appendix C

### Options to consider

1. **Companion document**: Publish the table as a separate resource (e.g., on the lepidum.co.jp blog, which already hosts a version)
2. **Appendix**: Move to an appendix (similar to the 100-bit table) — but size may still be a concern
3. **External reference**: Reference an external URL where the table is maintained
4. **GitHub-only**: Keep in the repository's `notes/` directory as reference material

### Action needed

- Decide on the publication venue
- If the table is to be included in any form, update the cross-references in the I-D text accordingly
- Consider whether the table needs updating before publication (e.g., adding new libraries/applications)

### Labels suggestion

`editorial`, `table`
