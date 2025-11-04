# Code Review (cr) - Compare Branch to Main

Review changes in the current branch against main, checking for:

- Potential bugs and errors
- Best practices violations (from Cursor rules)
- Code quality issues
- Architecture consistency
- TypeScript type safety
- React patterns and hooks usage
- Accessibility compliance (WCAG 2.1 AA)

## Instructions for AI

When this command is run:

1. **Get the list of changed files and their diffs:**

   ```bash
   # First, get the list of all changed files
   git diff --name-status main...HEAD

   # Then, for each file, get its specific diff to avoid truncation
   git diff main...HEAD -- <file_path>
   ```

   **Important:** Process each file's diff individually to prevent output truncation on large changes.
   For each file in the changed files list, read its full diff separately.

2. **Analyze the changes against our Cursor rules:**
   - Read all rules from `.cursor/rules/` directory
   - Check changes against:
     - `react-patterns.mdc` - Component architecture, hooks, state management
     - `api-development.mdc` - HttpClient patterns, API integration
     - `code-quality.mdc` - Constants usage, error handling patterns
     - `styling-chakra.mdc` - Chakra UI usage and theme patterns
     - `testing-accessibility.mdc` - Testing patterns and WCAG 2.1 AA compliance
     - `web-interface-guidelines.mdc` - UI/UX best practices
     - `quality-check.mdc` - Pre-commit quality standards
     - Any other `.mdc` files in the rules directory

3. **Provide structured feedback in this format:**

   ````markdown
   ## Code Review Summary

   **Branch:** [current-branch-name]
   **Comparing:** [current-branch] → main
   **Files Changed:** [count]

   ---

   ## 🔴 Critical Issues (Must Fix)

   ### [File Path]

   **Issue:** [Description]
   **Rule Violated:** [Which Cursor rule, if applicable]
   **Suggestion:** [How to fix]
   **Example:**

   ```tsx
   // Current (problematic)
   ...

   // Suggested
   ...
   ```

   ---

   ## 🟡 Warnings (Should Fix)

   ### [File Path]

   **Issue:** [Description]
   **Rule Reference:** [Which Cursor rule]
   **Suggestion:** [How to improve]

   ---

   ## 💡 Suggestions (Nice to Have)

   ### [File Path]

   **Suggestion:** [What could be better]
   **Why:** [Explanation]

   ---

   ## ✅ Positive Observations

   - [What was done well]
   - [Good practices followed]

   ---

   ## 📊 Summary

   - **Critical Issues:** X
   - **Warnings:** Y
   - **Suggestions:** Z
   - **Overall Assessment:** [Good/Needs Work/Ready to Merge]
   ````

4. **Focus on:**
   - **React & TypeScript violations:**
     - Missing type definitions or using `any`
     - Incorrect hook dependencies or rules of hooks violations
     - Not using existing UI components from `src/components/ui/`
     - Direct state mutations (should be immutable)
     - Missing error boundaries for error handling
     - Props interfaces not properly defined
     - Component not memoized when appropriate (React.memo, useMemo, useCallback)
     - Async operations not properly handled in useEffect

   - **Architecture & API issues:**
     - Not using `HttpClient` for API calls (should never use axios directly)
     - Missing error handling in API calls
     - Not using React Query (useQuery/useMutation) for server state
     - Mixing server state with local state (useState for API data)
     - Missing loading and error states in components
     - Not invalidating queries after mutations

   - **Code Quality issues:**
     - Magic numbers instead of constants (especially HTTP status codes)
     - Using magic numbers instead of `HTTP_STATUS` or `AUTHENTICATION_ERROR_STATUS_CODE`
     - Hardcoded values that should be in constants files
     - Missing error messages or generic error handling
     - Console.log statements left in code (should use proper logging)
     - Duplicate code that could be extracted to utilities

   - **UI & Styling violations:**
     - Not using Chakra UI components consistently
     - Hardcoded colors instead of theme tokens (`colors.blue.500` vs `#0000FF`)
     - Inline styles instead of Chakra props or styled components
     - Not using responsive design patterns (array syntax for responsive values)
     - Missing loading states or skeleton screens
     - Poor mobile/tablet responsiveness

   - **Testing & Accessibility issues:**
     - Missing tests for new components or features
     - Using `fireEvent` instead of `@testing-library/user-event`
     - Testing implementation details instead of user behavior
     - Not testing error scenarios
     - Missing accessibility attributes (aria-labels, roles)
     - Missing keyboard navigation support
     - Not testing with screen readers in mind
     - Missing data-testid attributes for E2E testing
     - Form inputs without proper labels
     - Interactive elements without focus states

   - **Security & Best Practices:**
     - Hardcoded secrets or API keys
     - Missing input validation
     - XSS vulnerabilities (dangerouslySetInnerHTML without sanitization)
     - Missing CORS or authentication checks
     - Exposing sensitive data in error messages or logs

5. **DO NOT flag as issues:**
   - Proper formatting (assume ESLint/Prettier will catch these)
   - Minor style preferences
   - Things that are already correct according to our rules
   - Pre-existing issues in unchanged code

6. **Be specific:**
   - Quote the exact code that's problematic
   - Reference specific line numbers when possible
   - Explain WHY something is an issue (user impact, maintainability, performance)
   - Provide concrete examples of how to fix with actual code
   - Reference the specific Cursor rule that's being violated

7. **Be constructive:**
   - Focus on helping, not criticizing
   - Acknowledge good practices when you see them
   - Explain the reasoning behind suggestions
   - Prioritize issues (Critical > Warning > Suggestion)
   - Link to relevant documentation or examples when helpful

## Example Output

````markdown
## Code Review Summary

**Branch:** feature/user-profile-edit
**Comparing:** feature/user-profile-edit → main
**Files Changed:** 4

---

## 🔴 Critical Issues (Must Fix)

### src/components/UserProfile/UserProfile.tsx (Line 45-60)

**Issue:** Using axios directly instead of HttpClient
**Rule Violated:** `api-development.mdc` - "Always use HttpClient for API requests"
**Suggestion:** Replace axios with HttpClient static methods

**Example:**

```tsx
// Current (problematic)
import axios from 'axios'

const updateProfile = async (data: UserData) => {
  try {
    const response = await axios.post('/api/users/profile', data)
    return response.data
  } catch (error) {
    console.error(error)
  }
}

// Suggested
import { HttpClient } from '@/network/HttpClient'
import { useMutation } from '@tanstack/react-query'

const useUpdateProfile = () => {
  return useMutation({
    mutationFn: (data: UserData) => HttpClient.updateUserProfile(data),
    onError: (error) => {
      toast({
        title: 'Failed to update profile',
        description: error.message,
        status: 'error',
      })
    },
  })
}
```

---

### src/components/UserProfile/UserForm.tsx (Line 23)

**Issue:** Missing accessibility labels on form inputs
**Rule Violated:** `testing-accessibility.mdc` - WCAG 2.1 AA compliance
**Suggestion:** Add proper labels or aria-labels to all form controls

**Example:**

```tsx
// Current (problematic)
<Input placeholder="Enter your name" {...register('name')} />

// Suggested
<FormControl>
  <FormLabel htmlFor="name">Full Name</FormLabel>
  <Input
    id="name"
    placeholder="Enter your name"
    aria-label="Full Name"
    {...register('name')}
  />
</FormControl>
```

---

## 🟡 Warnings (Should Fix)

### src/components/UserProfile/UserProfile.tsx (Line 12)

**Issue:** Using magic number for HTTP status code
**Rule Reference:** `code-quality.mdc` - "Always use constants for HTTP status codes"
**Suggestion:** Import and use `HTTP_STATUS.UNAUTHORIZED` or `AUTHENTICATION_ERROR_STATUS_CODE`

**Example:**

```tsx
// Current
if (error.response?.status === 401) {
  redirect('/login')
}

// Suggested
import { AUTHENTICATION_ERROR_STATUS_CODE } from '@/constants/httpStatusCodes'

if (error.response?.status === AUTHENTICATION_ERROR_STATUS_CODE) {
  redirect('/login')
}
```

---

### src/components/UserProfile/UserForm.tsx (Line 34-50)

**Issue:** Not using existing Input component from ui/ folder
**Rule Reference:** `react-patterns.mdc` - "Always check ui/ folder before creating components"
**Suggestion:** Use `@/components/ui/Input` which includes validation states and clear button

---

### tests/components/UserProfile/UserForm.test.tsx (Line 45)

**Issue:** Using fireEvent instead of userEvent for interactions
**Rule Reference:** `testing-accessibility.mdc` - "Always use @testing-library/user-event"
**Suggestion:** Replace fireEvent with userEvent for more realistic user interactions

**Example:**

```tsx
// Current
import { fireEvent } from '@testing-library/react'
fireEvent.click(button)

// Suggested
import userEvent from '@testing-library/user-event'
const user = userEvent.setup()
await user.click(button)
```

---

## 💡 Suggestions (Nice to Have)

### src/components/UserProfile/UserProfile.tsx

**Suggestion:** Consider adding optimistic updates for better UX
**Why:** User sees immediate feedback while the mutation is processing, improving perceived performance

**Example:**

```tsx
const mutation = useMutation({
  mutationFn: HttpClient.updateUserProfile,
  onMutate: async (newData) => {
    // Cancel outgoing refetches
    await queryClient.cancelQueries({ queryKey: ['user'] })

    // Snapshot previous value
    const previous = queryClient.getQueryData(['user'])

    // Optimistically update
    queryClient.setQueryData(['user'], newData)

    return { previous }
  },
  onError: (err, newData, context) => {
    // Rollback on error
    queryClient.setQueryData(['user'], context?.previous)
  },
})
```

---

### src/components/UserProfile/UserProfile.tsx (Line 78)

**Suggestion:** Add skeleton loader while data is loading
**Why:** Better perceived performance and clear loading state for users

---

## ✅ Positive Observations

- ✅ Excellent use of React Hook Form for form validation
- ✅ Comprehensive test coverage for new UserProfile component
- ✅ Proper TypeScript interfaces defined in types.ts
- ✅ Using Chakra UI theme tokens consistently for colors and spacing
- ✅ Good component composition and separation of concerns
- ✅ Error boundaries properly implemented for error handling
- ✅ All interactive elements have proper focus states and keyboard navigation

---

## 📊 Summary

- **Critical Issues:** 2
- **Warnings:** 3
- **Suggestions:** 2
- **Overall Assessment:** Needs Work (address critical issues before merging)

**Recommendation:** Fix the critical HttpClient and accessibility issues, then address the warnings. The suggestions can be implemented in a follow-up PR. Overall, solid work with good TypeScript usage and testing!
````

## Notes

- This command should be run from the project root
- Requires git to be available
- Only reviews changes in the current branch vs main
- Does not modify any files, only provides feedback
- If no changes found, report "No changes to review"
- Focus on React/TypeScript specific patterns and best practices
- Prioritize accessibility and user experience issues
- Consider both developer experience and end-user impact
