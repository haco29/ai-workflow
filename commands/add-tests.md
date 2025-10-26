# Add Tests Command

This command helps you create minimal, effective tests for recent changes using Vitest and React Testing Library with best practices focused on real user interactions and accessibility.

## Usage

Use this command when you want to:

- **Add Tests for New Components** - Create focused tests for recently added components
- **Test User Interactions** - Verify components work as users would interact with them
- **Ensure Accessibility** - Use recommended queries that also validate a11y
- **Minimal Test Coverage** - Create the minimum number of tests for good coverage
- **Real User Scenarios** - Test actual user workflows, not implementation details

## Testing Philosophy

### **🎯 User-Centric Testing**

- Test what users see and do, not internal implementation
- Use accessibility-focused queries (getByRole, getByLabelText)
- Focus on user interactions and expected outcomes
- Avoid testing implementation details or internal state

### **🚫 Minimal Mocking**

- Mock only external dependencies (APIs, third-party libraries)
- Don't mock internal components or utilities
- Use real DOM interactions when possible
- Prefer integration tests over isolated unit tests

### **✅ Quality Over Quantity**

- Write fewer, more meaningful tests
- Focus on critical user paths and edge cases
- Each test should have a clear, single purpose
- Descriptive test names that explain the user scenario

## What This Command Does

1. **Analyzes Recent Changes** - Identifies new components and modified functionality
2. **Determines Test Strategy** - Decides what needs testing based on component type
3. **Generates Test Files** - Creates test files with proper structure and naming
4. **Uses Best Practices** - Implements recommended testing patterns
5. **Focuses on Accessibility** - Uses queries that validate a11y compliance
6. **Minimal Coverage** - Creates only essential tests for good coverage

## Test Types Created

### **🧩 Component Tests**

- **Rendering**: Component renders without crashing
- **User Interactions**: Click, type, keyboard navigation
- **Accessibility**: Screen reader compatibility, ARIA attributes
- **State Changes**: User actions produce expected results

### **🔄 Hook Tests**

- **Return Values**: Hook returns expected data structure
- **State Updates**: State changes work correctly
- **Side Effects**: useEffect and cleanup functions work

### **🎯 Integration Tests**

- **User Workflows**: Complete user scenarios
- **Component Interaction**: Multiple components working together
- **Form Submission**: End-to-end form workflows

## Testing Patterns & Examples

### **✅ Good Test Example**

```typescript
describe('TrackableDetailModal', () => {
  it('allows user to override AI status assessment', async () => {
    const mockOnSave = vi.fn()
    const trackableItem = {
      id: '1',
      content: 'Test objective',
      suggestedTrackingStatus: 'partially_achieved',
      user_tracking_status: null
    }

    render(
      <TrackableDetailModal
        isOpen={true}
        item={trackableItem}
        onSave={mockOnSave}
        onClose={vi.fn()}
      />
    )

    // User sees the modal
    expect(screen.getByRole('dialog')).toBeInTheDocument()

    // User can see AI assessment
    expect(screen.getByText('partially achieved')).toBeInTheDocument()

    // User changes status
    const statusSelect = screen.getByLabelText(/your assessment/i)
    await user.selectOptions(statusSelect, 'not_achieved')

    // User saves changes
    const saveButton = screen.getByRole('button', { name: /save changes/i })
    await user.click(saveButton)

    // System saves user's override
    expect(mockOnSave).toHaveBeenCalledWith(
      expect.objectContaining({
        user_tracking_status: 'not_achieved'
      })
    )
  })
})
```

### **❌ Avoid These Patterns**

```typescript
// Don't test implementation details
expect(component.state.isLoading).toBe(false)

// Don't test internal methods
expect(component.handleClick).toHaveBeenCalled()

// Don't use generic queries when specific ones exist
screen.getByTestId('submit-button') // Use getByRole('button') instead

// Don't over-mock internal components
vi.mock('./TrackableCard') // Test the real component
```

## Recommended Queries Priority

### **🥇 First Choice - Accessible to Everyone**

```typescript
// Best for buttons, links, form elements
screen.getByRole('button', { name: /save/i })
screen.getByRole('textbox', { name: /search/i })

// Best for form labels
screen.getByLabelText(/email address/i)

// Best for text content users see
screen.getByText(/welcome back/i)
```

### **🥈 Second Choice - Semantic HTML**

```typescript
// For headings, landmarks
screen.getByDisplayValue('current input value')
screen.getByPlaceholderText(/enter your name/i)
```

### **🥉 Last Resort - When Nothing Else Works**

```typescript
// Only when accessibility queries don't work
screen.getByTestId('complex-component')
```

## Test File Structure

### **Component Test Template**

```typescript
import { describe, it, expect, vi } from 'vitest'
import { render, screen } from '@testing-library/react'
import { user } from '@testing-library/user-event'
import { ComponentName } from './ComponentName'

describe('ComponentName', () => {
  it('describes what user can do', async () => {
    // Arrange - Set up component with props
    render(<ComponentName prop="value" />)

    // Act - User interaction
    await user.click(screen.getByRole('button'))

    // Assert - Expected outcome
    expect(screen.getByText('result')).toBeInTheDocument()
  })
})
```

### **Hook Test Template**

```typescript
import { describe, it, expect } from 'vitest'
import { renderHook, act } from '@testing-library/react'
import { useCustomHook } from './useCustomHook'

describe('useCustomHook', () => {
  it('returns expected initial state', () => {
    const { result } = renderHook(() => useCustomHook())

    expect(result.current.value).toBe(expectedValue)
  })

  it('updates state when action is called', () => {
    const { result } = renderHook(() => useCustomHook())

    act(() => {
      result.current.updateValue('new value')
    })

    expect(result.current.value).toBe('new value')
  })
})
```

## Specific Checks Performed

### **Component Analysis**

- **New Components**: Identifies recently added React components
- **Modified Components**: Detects significant changes to existing components
- **User Interactions**: Maps out clickable elements, form inputs, navigation
- **Accessibility Features**: Identifies ARIA attributes, semantic HTML

### **Test Coverage Strategy**

- **Critical Paths**: User workflows that must work correctly
- **Error States**: How component handles errors and edge cases
- **Accessibility**: Screen reader compatibility and keyboard navigation
- **Integration Points**: How components work together

### **Mock Strategy**

- **External APIs**: Mock HTTP requests and external services
- **Third-party Libraries**: Mock complex external dependencies
- **Real Components**: Use actual internal components when possible
- **User Events**: Use real DOM events, not synthetic ones

## Best Practices Enforced

### **📝 Test Naming**

```typescript
// ✅ Good: Describes user behavior
it('shows error message when email is invalid')
it('allows user to filter results by status')
it('saves form data when user clicks submit')

// ❌ Bad: Describes implementation
it('calls handleSubmit function')
it('sets state.loading to true')
it('renders without errors')
```

### **🎯 Test Focus**

- **One Assertion Per Concept**: Each test verifies one user scenario
- **Clear Setup**: Minimal, focused test setup
- **Real Interactions**: Use user-event library for realistic interactions
- **Meaningful Assertions**: Assert on user-visible outcomes

### **♿ Accessibility Testing**

```typescript
// ✅ Good: Tests accessibility while testing functionality
const button = screen.getByRole('button', { name: /delete item/i })
expect(button).toHaveAttribute('aria-describedby')

// ✅ Good: Tests keyboard navigation
await user.keyboard('{Tab}')
expect(screen.getByRole('button')).toHaveFocus()

// ✅ Good: Tests screen reader content
expect(screen.getByRole('alert')).toHaveTextContent('Error occurred')
```

## Related Commands

- `precommit` - Runs tests as part of quality checks
- `reflect-changes` - Identifies what components need testing

## Example Usage Scenarios

### **After Component Creation**

```
🎯 Use Case: Created new TrackableDetailModal component
✅ Tests Added: User can view details, change status, save changes
📋 Coverage: Critical user workflows with accessibility validation
```

### **After Bug Fix**

```
🎯 Use Case: Fixed Select component crash issue
✅ Tests Added: Component renders, user can select options, no errors
📋 Coverage: Regression prevention with real user interactions
```

### **Before Feature Release**

```
🎯 Use Case: Completed trackable insights feature
✅ Tests Added: End-to-end user workflows, edge cases, error states
📋 Coverage: Complete user journey with minimal but effective tests
```

---

_This command creates focused, user-centric tests that validate real functionality while ensuring accessibility compliance, using the minimum number of tests needed for confident coverage._
