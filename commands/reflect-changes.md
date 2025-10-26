# Reflect Changes Command

This command helps you reflect on and summarize recent changes made to the codebase, providing insights into what was implemented, modified, or fixed.

## Usage

Use this command when you want to:

- **Summarize Recent Work** - Get an overview of what changes have been made
- **Document Progress** - Create a summary of completed tasks and implementations
- **Review Modifications** - Understand the scope and impact of recent changes
- **Prepare Status Updates** - Generate content for team updates or documentation

## What This Command Does

1. **Analyzes Recent Changes** - Reviews git commits, file modifications, and code changes
2. **Categorizes Updates** - Groups changes by type (features, fixes, refactoring, etc.)
3. **Identifies Key Components** - Highlights which files and components were modified
4. **Validates React Patterns** - Checks adherence to @react-development.mdc guidelines
5. **Reviews UI Component Usage** - Ensures proper use of the ui folder for reusable components
6. **Identifies Extraction Opportunities** - Suggests components that could be moved to ui folder
7. **Summarizes Impact** - Explains the business value and technical improvements
8. **Provides Context** - Links changes to requirements, issues, or user stories

## Example Output

The command will provide a structured reflection including:

- **📋 Summary of Changes** - High-level overview of what was accomplished
- **🔧 Technical Modifications** - Specific code changes and implementations
- **✅ Features Added** - New functionality and capabilities
- **🐛 Issues Fixed** - Bugs resolved and problems addressed
- **♿ Accessibility Improvements** - UX/accessibility enhancements
- **🧪 Testing Updates** - Test coverage and quality improvements
- **📚 Documentation Changes** - README updates, comments, and docs

### React Development Pattern Compliance

- **🎯 Component Structure** - Adherence to file organization and naming conventions
- **📝 TypeScript Usage** - Proper interface definitions and type safety
- **🎨 Styling Patterns** - Chakra UI usage and theme compliance
- **♿ Accessibility Standards** - WCAG 2.1 AA compliance and data-testid usage
- **🔄 State Management** - React Query for server state, useState for local state
- **🧩 Component Architecture** - Separation of concerns and single responsibility
- **⌨️ Keyboard Navigation** - Focus management and keyboard accessibility
- **📊 Constants Usage** - Proper use of HTTP status codes and configuration constants

### UI Component Analysis

- **✅ Proper UI Folder Usage** - Components correctly placed in src/components/ui/
- **🔍 Extraction Opportunities** - Components that could be made reusable
- **📦 Shared Component Benefits** - Reusability and maintainability improvements
- **🏗️ Component Hierarchy** - Proper separation between feature and UI components
- **📋 Missing Exports** - UI components not exported from index.ts
- **🎨 Design System Consistency** - Alignment with established UI patterns

## Best Practices

- Use this command after completing a feature or significant work session
- Run it before code reviews to prepare comprehensive summaries
- Include it in your workflow when preparing release notes
- Use the output to update project documentation and status reports
- Review React development pattern compliance before committing
- Identify and extract reusable components to the ui folder
- Ensure proper accessibility standards are followed
- Validate TypeScript usage and component architecture

## Specific Checks Performed

### React Development Pattern Validation

The command will specifically check for:

- **Component Structure**: PascalCase naming, proper file organization
- **TypeScript Interfaces**: Proper prop definitions and type safety
- **Accessibility**: WCAG 2.1 AA compliance, data-testid attributes, ARIA labels
- **Styling**: Chakra UI usage, theme consistency, responsive design
- **State Management**: React Query vs useState usage patterns
- **Constants**: HTTP status codes from constants file vs magic numbers
- **Generic Components**: Proper TypeScript generic usage without build tool conflicts
- **Error Handling**: Proper error boundaries and user-friendly error messages
- **Performance**: Memoization patterns and optimization opportunities

### UI Component Extraction Analysis

The command will identify:

- **Reusable Components**: Components that appear in multiple places but aren't in ui folder
- **Generic UI Elements**: Buttons, inputs, cards, modals that could be shared
- **Design System Gaps**: Missing common UI patterns in the ui folder
- **Component Hierarchy Issues**: Feature-specific components mixed with reusable ones
- **Export Completeness**: UI components not properly exported from index.ts
- **Naming Consistency**: UI components following proper naming conventions
- **Props Interface**: Reusable components with proper, flexible prop interfaces
- **Documentation**: UI components with proper TypeScript documentation

## Related Commands

- `precommit` - Run quality checks before committing changes
- `get-started` - Initial project setup and development guidelines

## Example Usage Scenarios

### After Feature Implementation

```
🎯 Use Case: Just completed trackable insights feature
✅ Check: Component structure, UI extraction, accessibility compliance
📋 Output: Comprehensive analysis of React patterns and reusability opportunities
```

### Before Code Review

```
🎯 Use Case: Preparing for team code review
✅ Check: TypeScript usage, state management patterns, component architecture
📋 Output: Detailed compliance report with improvement suggestions
```

### UI Component Audit

```
🎯 Use Case: Regular design system maintenance
✅ Check: Identify components that should be in ui folder
📋 Output: Extraction opportunities and design system gaps
```

---

_This command helps maintain project visibility, ensures React development pattern compliance, and promotes proper component reusability through the ui folder structure._
