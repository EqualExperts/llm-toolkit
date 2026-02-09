# LikeC4 Architecture Modeling Rules

Guidelines for creating effective, maintainable, and self-documenting LikeC4 architecture models that serve as reliable sources of truth for system documentation.

## Context

*Applies to:* Architecture documentation projects using LikeC4, C4 model implementations, system architecture diagrams
*Level:* Strategic/Tactical - governs how architecture is documented and communicated
*Audience:* Architects, Technical Leads, Documentation Teams

## Core Principles

1. *Self-Explanatory Documentation:* Architecture models should be comprehensible without external explanation, backed by objective sources
2. *Metadata-Driven Traceability:* Extensive use of metadata to link models to source documents, interviews, and research
3. *Business-Aligned Vocabulary:* Use terminology that matches business domain language for clarity and adoption
4. *Hierarchical Clarity:* Organize models using clear hierarchies and relationships that reflect actual system structure

## Rules

### Must Have (Critical)

- *RULE-001:* All elements must include metadata linking to source documentation (wikis, Slack channels, SharePoint, repositories)
- *RULE-002:* Use consistent naming conventions across the entire project (choose one: kebab-case, snake_case, or camelCase)
- *RULE-003:* Include business-meaningful descriptions for all elements and relationships
- *RULE-004:* Multi-projects must have `likec4.config.json` with a `name` property for identification

### Should Have (Important)

- *RULE-101:* Use single quotes consistently for all quoted values in LikeC4 DSL
- *RULE-102:* Apply tags using UPPER_CASE convention for filtering and organization
- *RULE-103:* Include technology information that is specific and helpful (`kotlin`, `Kafka`, `JSON + HTTP` vs generic terms)
- *RULE-104:* Use descriptive relationship labels that explain the nature of interactions
- *RULE-105:* Organize files following established naming patterns: `model.[name.]c4`, `view.[name.]c4`, `_spec.c4`

### Could Have (Preferred)

- *RULE-201:* Use limited color palette and consistent icon styles for visual clarity
- *RULE-202:* Apply size modifiers (xs, sm, lg) to create visual hierarchy
- *RULE-203:* Include navigation links between related views using `navigateTo`
- *RULE-204:* Use predicate groups for consistent filtering across views

## Patterns & Anti-Patterns

### ✅ Do This

```c4
// Good: Comprehensive element with metadata and clear relationships
domain marketing {
  #DOMAIN_MARKETING
  title 'Marketing Domain'
  description 'Marketing domain covers direct to customer interactions'

  api customer-preference {
    #DEPRECATED #LINE_OF_BUSINESS
    title 'Customer Marketing Preference'
    description 'All marketing preferences for an online customer'
    technology 'python'
    metadata {
      source1 'https://example.com/wiki'
      slack '#team-marketing | https://slack.com/ABC/DEF'
      email 'team@example.com'
    }

    customer-preference .uses salesforce.cloud {
      description 'Uses Salesforce to store marketing preferences'
    }
  }
}
```

### ❌ Don't Do This

```c4
// Bad: Missing metadata, vague descriptions, inconsistent naming
domain mkt {
  api custPref {
    title 'API'
    description 'does stuff'
    technology 'python'

    custPref .uses salesforce
  }
}
```

## Decision Framework

*When rules conflict:*
1. Prioritize clarity and business understanding over technical elegance
2. Choose the approach that provides better traceability to sources
3. Favor consistency with existing project patterns

*When facing edge cases:*
- Document the rationale for deviation in metadata
- Ensure the approach serves the primary audience (business stakeholders)
- Maintain consistency within the specific context or view

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Legacy system constraints that prevent standard modeling approaches
- Tool limitations that require alternative representations
- Specific business requirements for confidential or sensitive information

*Process for exceptions:*
1. Document the exception and rationale in element metadata
2. Ensure alternative documentation exists for missing information
3. Review exceptions during regular architecture reviews

## Quality Gates

- *Automated checks:* Validate that all elements have required metadata fields and follow naming conventions
- *Code review focus:* Verify source documentation links are accessible and descriptions align with business terminology
- *Testing requirements:* Ensure views render correctly and navigation links function as expected

## Related Rules

- rules/domain-driven-design.md - Alignment with domain boundaries and ubiquitous language
- rules/clean-code.md - Naming conventions and documentation standards
- rules/hexagonal-architecture.md - Architectural layer definitions and dependencies

## References

- [LikeC4 Documentation](https://likec4.dev) - Official LikeC4 documentation and examples
- [C4 Model](https://c4model.com/) - Foundation concepts for hierarchical architecture modeling
- [LikeC4 DSL Reference](https://likec4.dev/dsl/) - Complete DSL syntax and features

---

## TL;DR

*Key Principles:*
- Make architecture models self-explanatory with comprehensive metadata
- Use business-aligned terminology and maintain consistency
- Organize hierarchically with clear relationships and dependencies

*Critical Rules:*
- Must link all elements to source documentation via metadata
- Must use consistent naming conventions throughout project
- Must include meaningful descriptions for elements and relationships

*Quick Decision Guide:*
When in doubt: Choose the approach that makes the architecture most understandable to business stakeholders while maintaining traceability to sources
