# [Frontend Feature/Component Name]

*Write a brief paragraph describing the high-level purpose and context from a user perspective. What user problem does this solve? What is the main user objective or workflow? Keep this concise and focused on user value rather than technical implementation.*

## Requirements

*List the specific, measurable acceptance criteria that define when this frontend feature is complete. These should be testable from a user perspective and include both functional and non-functional requirements. Think of these as your definition of done.*

- Requirement 1 (e.g., "User can submit form with validation feedback")
- Requirement 2 (e.g., "Component loads within 200ms on 3G connection")
- Requirement 3 (e.g., "Interface is fully accessible via keyboard navigation")

## Rules

*Specify any rules files that should be included when working on this frontend feature. This includes any rules files that might be relevant for this UI slice to be implemented. The rules files are usually found under the `rules/` directory of this project.*

- rules/react-rules.md
- rules/design-rules.md
- rules/state-management-rules.md
- rules/code-quality-rules.md

## Component Architecture

*If applicable, describe the component structure and data flow using pseudo-code in TypeScript/JavaScript. Focus on component hierarchy, props interface, state management, and key interactions. This section helps establish the mental model for the feature.*

```typescript
// Component structure and interfaces in markdown block
interface ComponentProps {
  // Define props interface
}

interface ComponentState {
  // Define state shape
}

// Component hierarchy and relationships
```

## Extra Considerations

*List important factors that need special attention during implementation. This often grows as you discover edge cases or constraints during development. Think about accessibility, performance, browser compatibility, responsive design, and user experience.*

- Accessibility consideration (e.g., ARIA labels, screen reader support)
- Performance consideration (e.g., code splitting, lazy loading)
- Responsive design requirement (e.g., mobile-first approach)
- Browser compatibility concern (e.g., IE11 support, Safari quirks)
- User experience consideration (e.g., loading states, error handling)

## Testing Considerations

*Describe how you want this frontend feature to be tested. What types of tests are needed? What user scenarios should be covered? What are the quality gates for UI components?*

*Examples: unit test coverage for components, integration testing for user flows, visual regression testing, accessibility testing, cross-browser compatibility, performance benchmarks*

## Implementation Notes

*Document your preferences for how this should be built from a frontend perspective. This might include component patterns, styling approaches, state management choices, performance optimizations, or specific frontend technologies.*

*Examples: preferred component patterns (hooks, render props), CSS methodology (CSS modules, styled-components), state management approach (context, Redux), performance requirements (bundle size limits, Core Web Vitals), accessibility standards (WCAG 2.1 AA)*

## Specification by Example

*Provide concrete examples of what the frontend feature should do from a user perspective. This could be user interaction flows, component usage examples, visual mockups, responsive behavior descriptions, or accessibility scenarios. Make the abstract requirements tangible.*

*Examples: User journey scenarios, component API usage, visual design mockups, responsive breakpoint behavior, keyboard navigation flows, error state examples*

## Verification

*Create a checklist to verify that the frontend feature is complete and working correctly from both technical and user experience perspectives. These should be actionable items that can be checked off systematically.*

- [ ] Component renders correctly across target browsers
- [ ] All interactive elements are keyboard accessible
- [ ] Component meets performance benchmarks (loading time, bundle size)
- [ ] Visual design matches specifications at all breakpoints
- [ ] Error states display appropriate user feedback
- [ ] Loading states provide clear user indication
- [ ] Component integrates correctly with existing state management
- [ ] All user flows complete successfully
- [ ] Cross-browser compatibility verified
- [ ] Accessibility requirements met (WCAG compliance)